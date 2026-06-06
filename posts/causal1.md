---
title: Causal ML 1 - Three Rungs and a Simulator
published_date: 2026-06-04 12:00:00 +0000
description: Why a probability distribution is the wrong object for the questions we want to ask about a model.
tags:
  - causality
  - machine-learning
  - fairness
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/kausales-ml-1/
---

A neural network trained to convergence learns, in the best case, the conditional distribution $P(Y \mid X)$. The claim this series rests on is that for a large class of the questions we actually care about — whether a model discriminates, what it would have predicted under a different input, what happens if we *change* something rather than merely observe it — this object is not just insufficient. It is the wrong kind of thing, and no amount of additional data fixes that.

The cleanest way to see this is to watch an association reverse itself under intervention. Suppose a treatment $X \in \{0, 1\}$ and a recovery indicator $Y \in \{0, 1\}$, and suppose the data say

$$P(Y=1 \mid X=1) = 0.67, \qquad P(Y=1 \mid X=0) = 0.33.$$

Treated patients recover more often. Yet the correct answer to "should we give this treatment?" can be *no* — the treatment can lower everyone's chance of recovery — and the rest of this post is, in essence, an explanation of how both statements hold at once, and why the resolution is not statistical but causal.

## The three rungs

Pearl organises causal questions into a hierarchy of three levels, each strictly stronger than the last in the sense that knowing everything about one rung does not determine the next.

**Rung 1, association.** The object is $P(Y \mid X = x)$: the distribution of $Y$ among the subpopulation where $X = x$. This is filtering — we look at the rows of the dataset with $X = x$ and read off $Y$. Supervised learning lives here. Nothing a standard model computes ever leaves this rung.

**Rung 2, intervention.** The object is $P(Y \mid \operatorname{do}(X = x))$: the distribution of $Y$ in a world where we *set* $X$ to $x$, overriding whatever would normally have produced it. The notation is deliberately not $P(Y \mid X = x)$, because — as we are about to compute — the two are different numbers.

**Rung 3, counterfactual.** The object is $P(Y_{X=x} \mid X = x', Y = y')$: given that we observed this individual with $X = x'$ and outcome $y'$, what would their outcome have been had $X$ been $x$ instead. This conditions on the factual and asks about the contrary-to-fact in the same breath. We need it for fairness — the statement "this decision would have been the same had the applicant been of another sex" is exactly of this form — and it is the subject of the next post. Here I will build only the first two rungs, because they are enough to make the central point and because the third is meaningless without them.

## What a distribution leaves out

To define the second rung we need a model that says more than a distribution does. A *structural causal model* (SCM) is a tuple $(U, V, F, P(U))$:

- $V = \{V_1, \dots, V_n\}$, the **endogenous** variables — the things we model;
- $U = \{U_1, \dots, U_n\}$, the **exogenous** variables — background noise, one term feeding each endogenous variable;
- $F = \{f_1, \dots, f_n\}$, **structural assignments**: the value of each endogenous variable is set by

  $$V_i := f_i(\mathrm{PA}_i,\, U_i),$$

  with the parent set $\mathrm{PA}_i$ drawn from $V$;
- $P(U)$, a distribution over the noise, with the $U_i$ jointly independent.

The assignments are read as instructions, not equations — the $:=$ is asymmetric, like a line of code. Each assignment defines a directed edge pointing from a parent set into its child variable; taken together they form a DAG. The $U_i$ being mutually independent then gives the Markov factorisation

$$P(v) = \prod_{i=1}^{n} P\bigl(v_i \mid \mathrm{pa}_i\bigr).$$

Here is the point. Two different SCMs — two different sets of assignments, two different graphs — can induce the *same* joint distribution $P(v)$. The distribution is a shadow that several distinct mechanisms cast. Rung 1 sees only the shadow. The moment we intervene, the mechanisms come apart.

An intervention $\operatorname{do}(X = x)$ is surgery on $F$: delete the assignment $f_X$, replace it with the constant $x$, and leave every other assignment alone. The noise $U$ and its distribution are untouched. The graph loses every arrow *into* $X$ and keeps every arrow *out* of it. The resulting distribution is the original factorisation with one factor struck out and one variable pinned — the **truncated factorisation**:

$$P\bigl(v \mid \operatorname{do}(X = x)\bigr) = \prod_{i \,:\, V_i \neq X} P\bigl(v_i \mid \mathrm{pa}_i\bigr)\,\Big|_{X = x}.$$

Conditioning keeps the arrow into $X$ and reweights the population by it; intervening deletes that arrow. That difference is not notation. It is the whole subject.

## Confounding in one line

Take the graph $Z \to X$, $Z \to Y$, $X \to Y$, with $Z$ a common cause of treatment and recovery — say, illness severity, which makes a patient both more likely to be treated and less likely to recover. Conditioning gives

$$P(Y \mid X = x) = \sum_z P(Y \mid X = x, Z = z)\, P(Z = z \mid X = x);$$

the truncated factorisation gives

$$P\bigl(Y \mid \operatorname{do}(X = x)\bigr) = \sum_z P(Y \mid X = x, Z = z)\, P(Z = z).$$

The summands are identical. The weights are not: conditioning weights each stratum by $P(z \mid x)$, intervention by the marginal $P(z)$. When $Z$ is associated with $X$ — when sicker patients are the ones who get treated — those weights differ, and the two rungs disagree. The discrepancy *is* confounding; there is nothing more to it than the swap of $P(z \mid x)$ for $P(z)$.

## The simulator

Forty lines of PyTorch make the disagreement concrete. We treat the structural assignments literally: each variable is a function of its parents plus fresh randomness, sampled in topological order. Intervention is implemented as exactly what the definition says — we stop calling `sample_X` and substitute a constant, leaving `sample_Z` and `sample_Y` alone.

```python
import torch
torch.manual_seed(0)
N = 1_000_000

def bernoulli(p):
    return (torch.rand(N) < p).float()

# SCM:  Z -> X,  Z -> Y,  X -> Y.  Z is the confounder.
def sample_Z():
    return bernoulli(0.5)

def sample_X(Z):                       # X tracks Z: treatment follows severity
    return bernoulli(torch.where(Z == 1, 0.9, 0.1))

def sample_Y(X, Z):                    # logit = -1 - X + 3 Z : X hurts, Z helps more
    return bernoulli(torch.sigmoid(-1.0 - 1.0 * X + 3.0 * Z))

# Rung 1 - observe.  Condition by filtering rows.
Z = sample_Z(); X = sample_X(Z); Y = sample_Y(X, Z)
obs1, obs0 = Y[X == 1].mean(), Y[X == 0].mean()

# Rung 2 - intervene.  Delete f_X, pin X by hand; Z keeps its own assignment.
Z = sample_Z(); do1 = sample_Y(torch.ones(N),  Z).mean()
Z = sample_Z(); do0 = sample_Y(torch.zeros(N), Z).mean()

print(f"observe:    P(Y|X=1)={obs1:.3f}  P(Y|X=0)={obs0:.3f}  diff={obs1-obs0:+.3f}")
print(f"intervene:  P(Y|do1)={do1:.3f}  P(Y|do0)={do0:.3f}  diff={do1-do0:+.3f}")
```

The output:

```
observe:    P(Y|X=1)=0.669  P(Y|X=0)=0.330  diff=+0.339
intervene:  P(Y|do1)=0.424  P(Y|do0)=0.575  diff=-0.151
```

Observing $X = 1$ is associated with a recovery rate higher by $0.34$. Setting $X = 1$ lowers it by $0.15$. The treatment helps in the data and harms in reality, and the sign flips because the treated are mostly the severe cases ($Z = 1$): conditioning reads off their high baseline recovery and credits it to the treatment, while intervention holds the mix of severities fixed and exposes the treatment's own negative effect. The two columns of numbers come from the same simulator, differing only in whether `sample_X` was allowed to run.

A supervised model fit to this data would learn the first row exactly and the second row not at all — not because it was trained badly, but because the second row is not a function of $P(X, Y, Z)$. It is a function of the assignments, and those were never in the data to begin with.

## What this costs us later

The adjustment formula above looks like an escape: measure $Z$, average over it, recover the interventional answer from observational data. It works here because the graph was handed to us and $Z$ was a single observed bit. Both gifts vanish in the settings this series is aimed at. When $X$ is an image or a sentence there is no `sample_X` to write down by hand; we will have to *learn* it (post 3). When the confounder is unobserved, the formula is not merely hard to evaluate — the interventional quantity need not be identifiable from the data at all (post 6). And the variable we most want to intervene on in the fairness setting — a protected attribute — turns out to resist the surgery we just performed so casually, for reasons that are as much conceptual as technical.

Next: fairness as a statement on the third rung, and the first case where conditioning and intervening give not just different numbers but different verdicts about whether a model is allowed to do what it does.
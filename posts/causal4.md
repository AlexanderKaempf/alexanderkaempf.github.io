---
title: Causality 4 - Confounding, the Back-door and the Front-door
published_date: 2026-05-29 12:00:00 +0000
tags:
  - causality
  - probability-theory
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/kausalitaet-4/
---

The previous post derived a single causal effect by hand, conditioning on the one variable that closed the spurious path. The criteria below say in general which variables do that job, and the second of them does something the truncated factorization seemed to forbid: it identifies an effect through a confounder that is never measured.

## Confounding, defined backwards

The intuitive picture of confounding — a lurking common cause inflating an association — is hard to make exact, because every attempt to define it as "a variable correlated with both $X$ and $Y$" admits counterexamples in both directions. The graphical definition sidesteps the search for a single confounding variable and asks instead about *paths*.

A path between $X$ and $Y$ is a *back-door path* if it begins with an arrow into $X$. Such a path does not represent any effect of $X$; it represents a route by which $X$ and $Y$ vary together for reasons upstream of $X$. The effect of $X$ on $Y$ travels the directed paths out of $X$; everything spurious travels the back doors. Identification, then, is the problem of holding the back doors shut while leaving the front ones open.

**Definition (back-door criterion).** *A set $Z$ satisfies the back-door criterion relative to an ordered pair $(X, Y)$ if*

1. *no node in $Z$ is a descendant of $X$, and*
2. *$Z$ blocks every back-door path from $X$ to $Y$.*

The first condition keeps $Z$ from sitting on a directed path out of $X$, where conditioning on it would amputate part of the effect we are trying to measure or, worse, open a collider downstream. The second is the actual work. Together they isolate the directed influence.

**Theorem (back-door adjustment).** *If $Z$ satisfies the back-door criterion relative to $(X, Y)$, then*

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P(y \mid x, z)\thinspace  P(z).
$$

*Proof.* Condition on $Z$ inside the interventional distribution:

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P\bigl(y \mid \operatorname{do}(x),\thinspace  z\bigr)\thinspace  P\bigl(z \mid \operatorname{do}(x)\bigr).
$$

For the second factor, apply Rule 3. Since no node of $Z$ descends from $X$, deleting the arrows into $X$ leaves $X$ with only outgoing edges, and a non-descendant reached only by outgoing edges is d-separated from $X$; hence $(Z \perp\mkern-10mu\perp X)$ in $G_{\overline{X}}$ and $P(z \mid \operatorname{do}(x)) = P(z)$. For the first factor, apply Rule 2. Deleting the arrows out of $X$ leaves only back-door paths between $X$ and $Y$, all of which $Z$ blocks by hypothesis, so $(Y \perp\mkern-10mu\perp X \mid Z)$ in $G_{\underline{X}}$ and $P(y \mid \operatorname{do}(x), z) = P(y \mid x, z)$. Substitution gives the formula. $\square$

The derivation is the one from the previous post, now seen to depend on nothing about that particular three-node graph — only on the two conditions. Any $Z$ meeting them licenses the same adjustment.

## The reversal

Adjustment is not cosmetic. A well-chosen $Z$ can invert the apparent direction of an effect, and the failure to condition on it is not a loss of precision but a wrong sign. Consider a treatment $X$, a recovery indicator $Y$, and a binary covariate $Z$ — say patient group — that causes both which treatment is given and how likely recovery is. The graph is the back-door triangle: $Z \to X$, $Z \to Y$, $X \to Y$, so $Z$ satisfies the criterion as a singleton.

Suppose the data, pooled and then split by $Z$, read as follows.

| | recovered | total | rate |
|---|---|---|---|
| $Z=0$, $X=0$ | 234 | 270 | 0.867 |
| $Z=0$, $X=1$ | 81 | 87 | 0.931 |
| $Z=1$, $X=0$ | 55 | 80 | 0.688 |
| $Z=1$, $X=1$ | 192 | 263 | 0.730 |

Within each group, treatment $X=1$ recovers more often: $0.931$ against $0.867$ in the first, $0.730$ against $0.688$ in the second. Pool the groups, though, and the naive rates reverse,

$$
P(Y=1 \mid X=0) = 0.826, \qquad P(Y=1 \mid X=1) = 0.780,
$$

because $X=1$ was administered disproportionately to the group with the worse baseline. The pooled comparison answers the observational question — how do treated patients fare against untreated — and that question is contaminated by who received which treatment. Adjusting closes the back door:

$$
P\bigl(Y=1 \mid \operatorname{do}(X=1)\bigr) = \sum_z P(Y=1 \mid X=1, z)\thinspace  P(z) = 0.833,
$$

against $0.779$ for $\operatorname{do}(X=0)$. The treatment helps, by about five points, and the unadjusted figure had the sign backwards. This is Simpson's reversal, and the causal reading dissolves the paradox: the two numbers answer two different questions, and only the adjusted one answers what to do.

## When the confounder is invisible

Back-door adjustment needs to measure $Z$. Its limitation is stark: if the confounder is unobserved, there is no set to sum over, and the effect looks lost. The truncated factorization confirms the pessimism, since its product would contain a factor in the unmeasured variable. Yet an effect can still be identifiable, through a structure that routes the entire influence of $X$ through an observed intermediate.

**Definition (front-door criterion).** *A set $Z$ satisfies the front-door criterion relative to $(X, Y)$ if*

1. *$Z$ intercepts every directed path from $X$ to $Y$;*
2. *no unblocked back-door path runs from $X$ to $Z$;*
3. *every back-door path from $Z$ to $Y$ is blocked by $X$.*

The picture to hold is $X \to Z \to Y$ with an unobserved $U$ that confounds $X$ and $Y$ directly, $X \leftarrow U \to Y$. The mediator $Z$ carries all of $X$'s effect (condition 1); nothing confounds the $X$-to-$Z$ link, because $U$ does not point at $Z$ (condition 2); and the $Z$-to-$Y$ link is confounded only through $X$, which is observed and can be conditioned on (condition 3). The confounder $U$ is never measured, and need not be.

**Theorem (front-door adjustment).** *If $Z$ satisfies the front-door criterion relative to $(X, Y)$ and $P(x, z) > 0$, then*

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P(z \mid x) \sum_{x'} P(y \mid x', z)\thinspace  P(x').
$$

*Proof sketch.* Decompose the effect through the mediator,

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P\bigl(z \mid \operatorname{do}(x)\bigr)\thinspace  P\bigl(y \mid \operatorname{do}(x),\thinspace  z\bigr).
$$

The first factor is a clean back-door problem with the empty adjustment set: condition 2 says no back-door path links $X$ to $Z$, so $P(z \mid \operatorname{do}(x)) = P(z \mid x)$ by Rule 2. For the second factor, intervening on $X$ and observing $Z$ already determines $Y$ through $Z$ alone, so $P(y \mid \operatorname{do}(x), z) = P(y \mid \operatorname{do}(z))$; condition 1 lets the action move from $X$ to its mediator. The remaining $\operatorname{do}(z)$ is itself identifiable by back-door adjustment with $X$ as the adjustment set, since condition 3 says $X$ blocks the back-door paths from $Z$ to $Y$:

$$
P\bigl(y \mid \operatorname{do}(z)\bigr) = \sum_{x'} P(y \mid x', z)\thinspace  P(x').
$$

Chaining the two stages gives the stated double sum. Each step is one application of the rules; the completeness theorem guarantees no step was a lucky guess. $\square$

The formula's two stages have a clean reading. The inner sum estimates the effect of the mediator on the outcome, averaging over $X$ to break the $Z$-to-$Y$ confounding. The outer sum propagates that through the effect of $X$ on the mediator. The product of two identifiable links spans a gap that neither endpoint could cross directly, and the unobserved $U$ never appears.

## The estimate is exact, not approximate

To see that the front-door formula returns the true interventional effect rather than an approximation of it, instantiate the graph with explicit mechanisms and compare. Take all variables binary, with an unobserved fair coin $U$ that raises both the chance of $X=1$ and the chance of $Y=1$, a mediator $Z$ driven by $X$ alone, and $Y$ driven by $Z$ and $U$. Computing the true effect by surgery on the full model — which an analyst without access to $U$ could never do — gives

$$
P\bigl(Y=1 \mid \operatorname{do}(X=0)\bigr) = 0.30, \qquad P\bigl(Y=1 \mid \operatorname{do}(X=1)\bigr) = 0.70.
$$

Feeding only the observational distribution over $(X, Z, Y)$ into the front-door formula reproduces both numbers exactly, $0.30$ and $0.70$. The naive conditional, by contrast, reports $0.21$ and $0.79$ — pulled apart by the confounder it cannot see, overstating the effect at both ends. The front-door estimate used no more information than the naive one; it used the same information through the correct structure.

What the two criteria share is the discipline of the graph. Neither asks for a confounder to be named or a model to be guessed; each reads a sufficient adjustment off the diagram and certifies it with the rules of the previous post. What separates them is the reach: the back door requires the confounder on the table, the front door requires only an honest mediator. Both stop where the graph stops licensing a derivation, and that boundary — the line completeness draws — is where the question turns from adjustment to discovery. Finding the graph itself, when it is not handed to us, is the last problem the series takes up.
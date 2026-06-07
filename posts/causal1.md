---
title: Causality 1 - The Ladder of Causation
published_date: 2026-06-07 12:00:00 +0000
description: Why the founders of statistics built a science that could not say "because", and the hierarchy of questions that repairs the omission.
tags:
  - causality
  - probability-theory
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/kausalitaet-1/
---

This is the first post in a series on causal inference, built as the foundation for a later series on causal methods in machine learning. The only prerequisite is measure-theoretic probability; the graph theory is developed where it is needed. The motivation and the historical thread come from Pearl and Mackenzie's *The Book of Why*; the definitions and theorems from Pearl's *Causality: Models, Reasoning, and Inference*. The popular book argues by anecdote, which is its charm and its limit. The aim here is to keep the argument and prove what can be proved.

## A science built to avoid a word

Galton, studying heredity in the 1880s, found that the sons of tall fathers were tall but closer to average, and called it regression toward the mean. The effect is a property of any imperfectly correlated pair and says nothing about heredity pulling anyone toward mediocrity, though he first thought it did. His student Karl Pearson drew the harder conclusion: that the entire content of a relationship is its correlation, and that "cause" is a metaphysical relic the grown-up sciences should drop. Statistics was built on this refusal. The causal revolution is the claim that the refusal was an error — that there are well-posed questions no functional of the observational distribution can answer. The rest of this post makes the claim exact and proves its smallest instance.

## What association is

Fix a probability space $(\Omega, \mathcal{F}, P)$. Observable quantities are random variables on it; a finite collection $V = (V_1, \dots, V_n)$ valued in a product of standard Borel spaces has a joint law
$$P_V := V_* P = P \circ V^{-1},$$
the pushforward of $P$ along $V$. This law is the whole observational content of the system: everything classical statistics can estimate from a sample is a functional of $P_V$.

Conditioning defines the first rung. On standard Borel spaces the disintegration theorem grants a regular conditional distribution — a map $x \mapsto P(Y \in \cdot \mid X = x)$ that is a probability measure for $P_X$-almost every $x$ and measurable in $x$ — so that $P(Y \mid X = x)$ is a genuine measure and not a formal ratio, including when $X$ has no atoms.

**Definition 1 (Conditional independence).** Random variables $X$ and $Y$ are *conditionally independent given* $Z$, written $X \perp\!\!\!\perp Y \mid Z$, if for all bounded measurable $f, g$,
$$\mathbb{E}\bigl[f(X)\,g(Y) \mid Z\bigr] = \mathbb{E}\bigl[f(X) \mid Z\bigr]\,\mathbb{E}\bigl[g(Y) \mid Z\bigr] \qquad P\text{-a.s.}$$

**Definition 2 (Associational query).** An *associational* or *rung-one* query is any quantity expressible as a functional $\Psi(P_V)$ of the joint law of the observed variables.

Regression functions, conditional independences, mutual information are all functionals of $P_V$, hence rung-one. The definition is generous on purpose. It grants Pearson everything he could want — unlimited data, perfect estimation, the joint law known exactly — and then asks whether that suffices.

## The ladder

Pearl sorts causal questions into three levels, each tied to a different verb.

**Rung one — association — *seeing*.** Given that I observe $X = x$, what do I expect of $Y$? The object is $P(Y \mid X = x)$, a functional of $P_V$. This is the rung of curve-fitting and of every model trained to predict one coordinate of a distribution from the others.

**Rung two — intervention — *doing*.** If I *set* $X$ to $x$ by an action that reaches in and fixes its value, what happens to $Y$? The object is written $P(Y \mid \operatorname{do}(X = x))$, kept notationally distinct from conditioning because the two are different objects. Reading a low barometer and forcing the needle down by hand share a rung-one description and have opposite rung-two consequences for the weather.

**Rung three — counterfactual — *imagining*.** Given that I observed $X = x'$ and $Y = y'$, what would $Y$ have been had $X$ been $x$ instead? The object conditions on what happened and asks about a world contrary to it at once. This is the rung of regret and of "the headache would have gone anyway", and it needs more structure than either rung below. It is the subject of the fifth post.

The claim that organises the subject is that the rungs are distinct: knowing everything on one leaves the rungs above it open. For the gap between rung one and rung two this is a theorem, elementary enough to prove now, once we have the little structure that rung two needs.

## The structure rung two requires

Conditioning needs only the joint law. Intervention needs a model of the mechanism, because to say what setting a variable does we must say which equations the setting overrides and which it leaves alone. That information is carried by the structural causal model. The full graphical theory is the next post; here is the minimum the theorem needs.

**Definition 3 (Structural causal model, preliminary form).** A *structural causal model* on endogenous variables $V = (V_1, \dots, V_n)$ consists of mutually independent exogenous variables $U = (U_1, \dots, U_n)$ with law $P_U$, and for each $i$ a measurable *structural function* $f_i$ with a parent set $\mathrm{PA}_i \subseteq V \setminus \{V_i\}$, such that
$$V_i = f_i(\mathrm{PA}_i,\, U_i).$$
We assume acyclicity, so that $V$ is determined as a measurable function of $U$, and $P_V$ is the law of that function under $P_U$.

The assignment is a mechanism, not an equation: the left side is computed from the right, and the asymmetry is the content. Two functions that are algebraically equivalent but disagree on which variable is the output describe different mechanisms, and behave differently under intervention.

**Definition 4 (Atomic intervention).** Given a model $M$ and a value $x$, the *intervened model* $M_{\operatorname{do}(X = x)}$ deletes the equation for $X$ and replaces it with $X := x$, leaving every other equation and the law $P_U$ untouched. The *interventional distribution* $P^{M}(\,\cdot \mid \operatorname{do}(X = x))$ is the law of $V$ under $M_{\operatorname{do}(X = x)}$.

Intervention is surgery on the mechanism. It does not condition on $X = x$; it overwrites the process that produced $X$, cutting it from its former causes while leaving its effects to propagate.

## The rungs do not collapse

**Proposition 1.** There exist two structural causal models over the same pair $(X, Y)$ with identical observational distributions but different interventional distributions. No functional of the observational law can therefore equal $P(Y \mid \operatorname{do}(X = x))$ in general: rung two is not determined by rung one.

**Proof.** Fix the joint law on $\{0,1\}^2$ given by
$$P(X{=}0,Y{=}0) = 0.4,\quad P(X{=}0,Y{=}1) = 0.1,\quad P(X{=}1,Y{=}0) = 0.2,\quad P(X{=}1,Y{=}1) = 0.3,$$
so that $X \not\perp\!\!\!\perp Y$. Write $P(Y \mid X)$, $P(X \mid Y)$ for the conditionals and $P(X)$, $P(Y)$ for the marginals. Build two models, each reproducing this joint by construction.

*Model $A$ (X causes Y).* Let $X := U_X$ with $U_X \sim P(X)$, and $Y := f(X, U_Y)$ realising $P(Y \mid X)$. The induced joint is $P(X)\,P(Y \mid X) = P_{X,Y}$. Deleting the first equation and setting $X := x$ leaves the second intact, so
$$P^A(Y \mid \operatorname{do}(X = x)) = P(Y \mid X = x).$$

*Model $B$ (Y causes X).* Let $Y := U_Y$ with $U_Y \sim P(Y)$, and $X := g(Y, U_X)$ realising $P(X \mid Y)$. The induced joint is $P(Y)\,P(X \mid Y) = P_{X,Y}$, identical to $A$'s. Here $Y$ has no parents, so deleting the equation for $X$ leaves the law of $Y$ alone:
$$P^B(Y \mid \operatorname{do}(X = x)) = P(Y).$$

The two joints agree, being two factorisations of one law. The interventional laws differ whenever $P(Y \mid X = x) \neq P(Y)$, which holds because $X \not\perp\!\!\!\perp Y$. With the numbers above, $P(Y \mid \operatorname{do}(X{=}1))$ is $(0.4, 0.6)$ under $A$ and $(0.6, 0.4)$ under $B$. The interventional law is not a function of the shared observational law, so no functional of the latter can express it. $\square$

The construction is worth fixing in code, and the series will compute throughout.

```python
import numpy as np

# A joint law P(X,Y) on {0,1}^2 with X not independent of Y.
P  = np.array([[0.4, 0.1],     # [P(X=0,Y=0), P(X=0,Y=1)]
               [0.2, 0.3]])    # [P(X=1,Y=0), P(X=1,Y=1)]
PX = P.sum(axis=1)
PY = P.sum(axis=0)
P_Y_g_X = P / PX[:, None]      # P(Y|X)
P_X_g_Y = P / PY[None, :]      # P(X|Y)

# Model A : X := U_X , Y := f(X,U_Y)   (X causes Y)
jointA = PX[:, None] * P_Y_g_X         # = P
doA    = P_Y_g_X                       # P(Y | do(X=x)) = P(Y|X=x)

# Model B : Y := U_Y , X := g(Y,U_X)   (Y causes X)
jointB = PY[None, :] * P_X_g_Y         # = P
doB    = np.repeat(PY[None, :], 2, 0)  # P(Y | do(X=x)) = P(Y)

print("same observational law :", np.allclose(jointA, jointB))
print("P(Y | do X=1) under A  :", doA[1])
print("P(Y | do X=1) under B  :", doB[1])
```

```
same observational law : True
P(Y | do X=1) under A  : [0.4 0.6]
P(Y | do X=1) under B  : [0.6 0.4]
```

The models are observationally indistinguishable. No sample from either could tell them apart, since they agree on the one thing a sample sees. They disagree on what an intervention does, by a flip of the distribution. This is the sense in which statistics, confined to rung one, cannot say "because": the thing it would need is absent from its object of study.

## How thin is the agreement?

Proposition 1 gives one pair of models that agree below and split above. One might hope this is pathological and that for typical mechanisms the observational law fixes the interventional one. It does not. Bareinboim, Correa, Ibeling and Icard (2022) formalised the hierarchy and proved a *Causal Hierarchy Theorem*: under a natural measure over structural models, the set on which a lower rung determines the next has measure zero. The layers collapse only on a thin exceptional set. I will not reproduce the statement, which needs the apparatus of their paper; the content is that Proposition 1 is the rule, and the work of climbing the ladder is supplying the missing structure.

One principle says where to look.

**Reichenbach's Common Cause Principle.** If $X$ and $Y$ are statistically dependent, then $X$ causes $Y$, or $Y$ causes $X$, or some $Z$ is a common cause of both with $X \perp\!\!\!\perp Y \mid Z$. No correlation without causation.

This is a postulate about the world, not a theorem, and one can argue with it. But it is the hinge: it says the rung-one quantity, dependence, always has a rung-two explanation of one of three shapes. The screening-off clause — dependence vanishing once the common cause is fixed — is the seed of d-separation, which makes the link between graphical structure and conditional independence exact. That is the next post: structural causal models in full, the graphs that encode them, and the theorem that turns "the flow of association" from a metaphor into something one can prove.
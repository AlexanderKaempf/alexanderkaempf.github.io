---
title: Causality 3 - The do-operator and the Completeness of do-calculus
published_date: 2026-05-13 12:00:00 +0000
tags:
  - causality
  - probability-theory
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/kausalitaet-3/
---

To observe $X = x$ is to restrict attention to the part of the world where that already held. To set $X = x$ is to reach in and make it hold, overriding whatever would have produced $X$ otherwise. The previous posts showed why these differ; this one defines the second operation and then proves the surprising fact that three rules exhaust it.

## Surgery

The mechanism for $X$ in a structural model is the function $f_X(\mathrm{Pa}_X, U_X)$ that normally sets its value. An intervention discards that function and substitutes a constant.

**Definition (intervention).** *Given an SCM $M$ and a value $x$, the submodel $M_x$ is $M$ with $f_X$ replaced by the constant assignment $X := x$, all other functions unchanged. The interventional distribution is*

$$
P\bigl(v \mid \operatorname{do}(X = x)\bigr) := P_{M_x}(v),
$$

*the distribution induced by the submodel.*

Graphically the substitution deletes every arrow into $X$ — the variable no longer listens to its parents — and leaves the rest of the diagram intact. Write $G_{\overline{X}}$ for this mutilated graph. The deletion is the formal residue of the word *force*: a forced variable has no incoming mechanism left to respond to its old causes.

The submodel is itself a Markovian SCM, so Proposition 2 of the previous post applies to it, and its factorization is the original product with one factor struck out.

**Proposition 1 (truncated factorization).** *For $v$ consistent with $X = x$,*

$$
P\bigl(v \mid \operatorname{do}(X = x)\bigr) = \prod_{i \thinspace :\thinspace  V_i \neq X} P\bigl(v_i \mid \mathrm{pa}_i\bigr),
$$

*and the probability is zero otherwise.*

*Proof.* The submodel $M_x$ has the same graph as $M$ with the parents of $X$ removed, and the same conditional factors for every variable other than $X$, since their mechanisms are untouched. Its factor for $X$ is the point mass at $x$. Writing out the causal Markov factorization for $M_x$ and discarding the degenerate factor gives the product over $i$ with $V_i \neq X$. $\square$

The formula is already a complete answer to the interventional question whenever every variable in the graph is observed: knowing the conditional factors $P(v_i \mid \mathrm{pa}_i)$, all of which are observational quantities, one simply omits the factor for $X$ and reads off the effect. The difficulty that motivates everything below is the case of *latent* variables, where some factors involve nodes we never measure. Then the truncated product is a formula in unobservable quantities, and the question becomes whether the effect can be re-expressed using only what the data supply.

## Identifiability

**Definition (identifiability).** *Let $G$ be a causal graph over observed variables $V$ and latent variables. The effect $P(y \mid \operatorname{do}(x))$ is* identifiable *from $G$ if it is uniquely determined by the observational distribution over $V$ — that is, any two models sharing the graph and the observed distribution agree on the effect.*

Identifiability is a property of the graph, not of any particular dataset. It asks whether the structure leaves enough constraints to pin the interventional quantity down from observation alone, or whether two models can match on everything observable and still disagree on the consequence of acting. The first post built exactly such a disagreeing pair for the bare two-node case; a richer graph may forbid the disagreement, and the question is how to tell.

The tool that decides it operates by rewriting expressions that mix observation and action until, if possible, every $\operatorname{do}$ is eliminated. The rewriting is governed by three rules.

## The three rules

Two further surgeries name the graphs the rules quantify over. Let $G_{\overline{X}}$ delete the arrows *into* $X$, as above, and let $G_{\underline{X}}$ delete the arrows *out of* $X$. These compose: $G_{\overline{X}\thinspace \underline{Z}}$ deletes arrows into $X$ and out of $Z$. Throughout, $X, Y, Z, W$ are disjoint node sets, and a graph subscript on $\perp\mkern-10mu\perp$ means d-separation in that mutilated graph.

**Rule 1 (insertion and deletion of observations).**

$$
P\bigl(y \mid \operatorname{do}(x),\thinspace  z,\thinspace  w\bigr) = P\bigl(y \mid \operatorname{do}(x),\thinspace  w\bigr) \quad\text{if } (Y \perp\mkern-10mu\perp Z \mid X, W) \text{ in } G_{\overline{X}}.
$$

An observation that the post-intervention graph renders irrelevant to $Y$ may be dropped. This is nothing more than d-separation soundness applied inside the submodel.

**Rule 2 (exchange of action and observation).**

$$
P\bigl(y \mid \operatorname{do}(x),\thinspace  \operatorname{do}(z),\thinspace  w\bigr) = P\bigl(y \mid \operatorname{do}(x),\thinspace  z,\thinspace  w\bigr) \quad\text{if } (Y \perp\mkern-10mu\perp Z \mid X, W) \text{ in } G_{\overline{X}\thinspace \underline{Z}}.
$$

Setting $Z$ and seeing $Z$ have the same effect on $Y$ when, after cutting $Z$'s outgoing arrows, no back-door path ties $Z$ to $Y$. The condition checks that $Z$'s only route to $Y$ was the directed one that an intervention and an observation share.

**Rule 3 (insertion and deletion of actions).**

$$
P\bigl(y \mid \operatorname{do}(x),\thinspace  \operatorname{do}(z),\thinspace  w\bigr) = P\bigl(y \mid \operatorname{do}(x),\thinspace  w\bigr) \quad\text{if } (Y \perp\mkern-10mu\perp Z \mid X, W) \text{ in } G_{\overline{X}\thinspace \overline{Z(W)}},
$$

*where $Z(W)$ is the set of $Z$-nodes that are not ancestors of any $W$-node in $G_{\overline{X}}$.*

An action with no remaining path to $Y$ changes nothing and may be removed. The ancestor caveat handles the case where conditioning on $W$ has opened a collider that an intervention on $Z$ would otherwise travel.

Each rule is a corollary of d-separation soundness applied to a specific submodel; none is deep on its own. Their force is collective, and it took two decades to state precisely how much they collectively reach.

## What the rules reach

**Theorem (soundness and completeness of do-calculus).** *The three rules are sound: every equality they license holds in every model with the given graph. They are also complete: if $P(y \mid \operatorname{do}(x))$ is identifiable from $G$, then it can be reduced to a $\operatorname{do}$-free expression by a finite sequence of the three rules together with the standard probability axioms. If it is not identifiable, no such sequence exists.*

Soundness is the easy half, established rule by rule above. Completeness is the result of Shpitser and Pearl (2006) and, independently, Huang and Valtorta (2006). It says the rules are not merely a useful toolkit that might miss some identifiable effects; they are a decision procedure. The accompanying ID algorithm takes a graph and a target effect and returns either an estimand built from observational quantities or a certificate of non-identifiability — a subgraph called a *hedge* whose presence proves that two models can match observationally while disagreeing on the effect, exactly the construction from the first post lifted to arbitrary structure.

The shape of the claim deserves emphasis, because completeness theorems are rarer than soundness theorems and easier to overstate. It does not say causal effects are always identifiable; most are not. It says the *boundary* between identifiable and not is drawn entirely by these three rules. Where they succeed the effect is recoverable from data; where they provably cannot, no method built on the same assumptions can do better, because there is genuinely nothing in the observational distribution that distinguishes the competing answers. The rules are the whole map, including its edge.

## The rules at work

Take the smallest graph where the answer is not immediate and watch the machinery run. Suppose $X \to Y$, with an observed variable $Z$ that is a cause of both: $Z \to X$ and $Z \to Y$. Every variable here is observed, so Proposition 1 already settles it, but deriving the same result from the rules shows how a derivation proceeds when truncation alone would leave latent factors.

Begin by conditioning on $Z$:

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P\bigl(y \mid \operatorname{do}(x),\thinspace  z\bigr)\thinspace  P\bigl(z \mid \operatorname{do}(x)\bigr).
$$

The second factor falls to Rule 3. In $G_{\overline{X}}$ the arrow $Z \to X$ is gone, so $Z$ — a non-descendant of $X$ — is d-separated from $X$, giving $(Z \perp\mkern-10mu\perp X)$ in $G_{\overline{X}}$ and hence

$$
P\bigl(z \mid \operatorname{do}(x)\bigr) = P(z).
$$

The first factor falls to Rule 2. In $G_{\underline{X}}$ the arrow $X \to Y$ is gone, so the only remaining route between $X$ and $Y$ runs $X \leftarrow Z \to Y$, which $Z$ blocks. Thus $(Y \perp\mkern-10mu\perp X \mid Z)$ in $G_{\underline{X}}$, and the action becomes an observation:

$$
P\bigl(y \mid \operatorname{do}(x),\thinspace  z\bigr) = P(y \mid x, z).
$$

Substituting both,

$$
P\bigl(y \mid \operatorname{do}(x)\bigr) = \sum_z P(y \mid x, z)\thinspace  P(z).
$$

Every term on the right is observational. The interventional quantity on the left, which no amount of staring at $P(y \mid x)$ would yield, has been written entirely in quantities a dataset reports. The pattern — condition on a well-chosen set, clear the residual actions with Rules 2 and 3 — recurs across the identifiable cases, and the set one conditions on is what the next post turns into a criterion. There it acquires a name, and a companion that survives even when the confounder is never measured.
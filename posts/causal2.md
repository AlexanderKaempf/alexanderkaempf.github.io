---
title: Causality 2 - Structural Causal Models and d-separation
published_date: 2026-03-10 12:22:14 +0000
tags:
  - causality
  - probability-theory
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/kausalitaet-2/
---

The last post argued that a single probability measure underdetermines the answers we want, and left the interventional rung as a promissory note. Paying it requires an object richer than a distribution: something that says not only how the variables co-vary, but which mechanism produced which. That object is the structural causal model.

## The model

A *structural causal model* is a quadruple $M = (U, V, F, P)$ where

- $U = \{U_1, \dots, U_m\}$ are the *exogenous* variables, fixed by influences outside the model;
- $V = \{V_1, \dots, V_n\}$ are the *endogenous* variables, those the model claims to explain;
- $F = \{f_1, \dots, f_n\}$ assigns to each endogenous variable a function $V_i = f_i(\mathrm{Pa}_i, U_i)$, with $\mathrm{Pa}_i \subseteq V \setminus \{V_i\}$ its endogenous *parents* and $U_i \subseteq U$ its exogenous inputs;
- $P$ is a probability measure on the range of $U$.

The functions carry the asymmetry that a joint distribution cannot. Reading $V_i = f_i(\mathrm{Pa}_i, U_i)$ as an *assignment* rather than an equation is the whole point: the left side is computed from the right, and never the reverse. A linear regression of $Y$ on $X$ produces a coefficient that can be solved for $X$ just as well as for $Y$; the structural equation $Y := \beta X + U_Y$ cannot be turned around, because turning it around would assert a different mechanism.

Each model induces a directed graph $G(M)$: draw an arrow into $V_i$ from every variable in $\mathrm{Pa}_i$. We assume throughout that $G(M)$ is acyclic, which is the formal content of the word *recursive*. The exogenous variables are usually left off the drawing unless two endogenous variables share one, in which case the shared cause is what a dashed bidirected edge stands in for.

Acyclicity buys the first result, the one that lets us speak of "the distribution of a model" at all.

**Proposition 1.** *In a recursive SCM, every endogenous variable is a function of the exogenous variables alone. Consequently $M$ together with $P$ induces a unique probability measure $P_M$ on $V$.*

*Proof.* Acyclicity gives a topological order $V_{(1)}, \dots, V_{(n)}$ in which every parent precedes its child. Argue by induction along it. The first variable has no endogenous parents, so $V_{(1)} = f_{(1)}(U_{(1)})$ is already a function of $U$. Suppose $V_{(1)}, \dots, V_{(k-1)}$ are each functions of $U$. The parents of $V_{(k)}$ sit among them, so substituting their expressions into $f_{(k)}$ writes $V_{(k)}$ as a function of $U$. The map $g : \mathrm{dom}(U) \to \mathrm{dom}(V)$ assembled from these functions is measurable, and $P_M := g_* P$, the pushforward, is the claimed measure. Uniqueness is immediate, since $g$ is determined by $F$. $\square$

The construction matters more than the statement. It says a causal model is a deterministic machine wrapped in a single source of randomness — the exogenous draw $U \sim P$ — and the observed distribution is what you get by looking only at the machine's endogenous outputs. Every counterfactual question we reach later is a question about running the same machine on the same $U$ with one function overwritten.

## Independence without numbers

When the exogenous variables are jointly independent the model is called *Markovian*, and the induced distribution inherits a rigid factorization.

**Proposition 2 (Causal Markov factorization).** *For a Markovian model with graph $G$,*

$$
P_M(v_1, \dots, v_n) = \prod_{i=1}^{n} P\bigl(v_i \mid \mathrm{pa}_i\bigr),
$$

*where $\mathrm{pa}_i$ denotes the values the parents take in the argument $v$.*

*Proof sketch.* Each $U_i$ feeds exactly one mechanism and the $U_i$ are independent, so conditioning a variable on its parents screens off everything non-descendant: the residual noise $U_i$ is the only thing left, and it is independent of the rest. Expanding $P_M$ in the topological order by the chain rule and dropping every non-parent from each conditioning set on these grounds yields the product. The dropped terms are exactly the ones the graph licenses dropping. $\square$

The factorization is the bridge between mechanism and probability, but it still presents independence as something to be computed. The contribution of graph theory is that independence can instead be *seen*. To see it we need to say when a path transmits association and when it does not.

Fix a graph $G$ and consider an undirected path — a sequence of distinct nodes joined by edges, arrows pointing either way along it. Relative to a conditioning set $Z$, a node on the path is one of three kinds:

- a *chain* $a \to m \to b$ or a *fork* $a \leftarrow m \to b$, where $m$ is a non-collider;
- a *collider* $a \to m \leftarrow b$, where two arrowheads meet.

The path is *blocked* by $Z$ when at least one of the following holds: some non-collider on it lies in $Z$, or some collider on it lies outside $Z$ and has no descendant in $Z$. Otherwise the path is *open*.

Two sets $X$ and $Y$ are *d-separated* by $Z$, written $(X \perp\mkern-10mu\perp Y \mid Z)_G$, when $Z$ blocks every path between a node of $X$ and a node of $Y$. The symbol $\perp\mkern-10mu\perp$ will also denote ordinary conditional independence in a distribution; which one is meant is fixed by whether a graph subscript is present.

The collider clause is the rule people find counterintuitive, and it is worth stating its content in plain terms before proving anything: conditioning on a common effect of two causes makes those causes dependent, even when they began independent. The graph encodes this by leaving a collider path *open* precisely when its middle node is observed.

## Soundness

**Theorem (d-separation soundness).** *Let $P$ be Markov relative to $G$. If $(X \perp\mkern-10mu\perp Y \mid Z)_G$ then $X$ and $Y$ are conditionally independent given $Z$ under $P$.*

The general statement is Verma and Pearl's; the proof reduces to the three elementary configurations on a single intermediate node, from which the path version follows by composing blocks. Take the three configurations in turn, each on variables $A, B$ with one middle node $M$ and independent exogenous noise.

*Chain $A \to M \to B$.* Here $B = f_B(M, U_B)$ and $M = f_M(A, U_M)$, with $U_B$ independent of $(A, U_M)$. Conditioning on $M$ fixes the only argument of $f_B$ that depends on $A$; what remains in $B$ is the fresh noise $U_B$, independent of $A$. Hence $A \perp\mkern-10mu\perp B \mid M$. Conditioning on $M$ blocks the chain, and the algebra agrees.

*Fork $A \leftarrow M \to B$.* Now $A = f_A(M, U_A)$ and $B = f_B(M, U_B)$ with $U_A, U_B, M$ mutually independent in the relevant sense. Given $M$, the two outputs are functions of disjoint independent noises, so they are conditionally independent. The common cause, once held fixed, has nothing left to share.

*Collider $A \to M \leftarrow B$.* With $A$ and $B$ independent and $M = f_M(A, B, U_M)$, the pair is marginally independent — no path is open when $M$ is unobserved. Condition on $M$, however, and $A$ and $B$ are tied by the constraint $f_M(A, B, U_M) = m$: learning one narrows the other. This is the open collider.

Composing these along a path, an open path requires every non-collider off $Z$ and every collider on $Z$ (or with a descendant in $Z$), which is exactly the negation of blocking. A blocked path therefore contributes no dependence, and d-separation of all paths gives the global independence. $\square$

The converse fails in general — a distribution can carry an independence the graph does not show, through a numerical coincidence among the structural functions. Models in which *every* independence is graphical are called *faithful*, and faithfulness is an assumption about the world, not a theorem about graphs. The non-collapse example from the first post was an instance of two graphs agreeing on observational independences while disagreeing on interventions; faithfulness is the dual concern, where one graph's distribution hides an independence it structurally should not have. Soundness needs neither assumption. That is why it is the workhorse: it lets us certify an independence from the picture alone, with no access to the numbers.

## Explaining away, in numbers

A minimal collider makes the abstract clause concrete. Let $A$ and $B$ be independent fair coins, and let $C = A \lor B$ be their logical or — an alarm that sounds if either of two independent faults occurs. Marginally,

$$
P(A=1) = P(B=1) = \tfrac{1}{2}, \qquad P(A=1, B=1) = \tfrac{1}{4} = P(A=1)\,P(B=1),
$$

so $A$ and $B$ are independent, as the unconditioned collider predicts. Condition on the alarm, $C = 1$. Three of the four equally likely fault combinations survive, and a short computation gives

$$
P(A=1 \mid C=1) = \tfrac{2}{3}, \qquad P(A=1 \mid B=1,\, C=1) = \tfrac{1}{2}.
$$

Learning that the second fault occurred drops the probability of the first from two-thirds back to one-half. The alarm made the faults dependent; observing one fault then explains the alarm and discounts the other. Nothing in the mechanism changed — $A$ and $B$ are generated by independent coins regardless — yet conditioning on their shared effect manufactured a correlation. A model that read association straight off the conditional distribution would report the two faults as linked and have no vocabulary for saying the link is an artifact of what was conditioned on.

That artifact is the engine behind a long list of statistical traps, selection bias and Berkson's paradox among them, and every one of them is the same open collider seen from a different applied angle. The graph diagnoses it without a single number: a collider conditioned upon is a path left open.

A distribution Markov to a graph thus comes with a catalogue of guaranteed independences, all readable by eye. What it does not yet come with is a way to compute the effect of *forcing* a variable to a value rather than observing it. That operation cuts arrows instead of reading them, and it is the subject of the next post.

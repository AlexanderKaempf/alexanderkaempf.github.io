---
title: Entanglement and the Schmidt Decomposition
published_date: 2026-06-16 09:00:00 +0000
description: A canonical form for bipartite pure states that makes entanglement visible at a glance, and the awkward question of what to do about mixed states.
tags:
  - quantum-information
  - entanglement
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/verschraenkung/
---

The [last post](/posts/composite-systems/) ended on a puzzle dressed as a computation: a pure state $\Psi$ of two qubits whose reduced state on each part was maximally mixed. The whole was sharp; the parts were maximally uncertain; the missing information lived in the correlations between them. We called such states entangled in passing and moved on. This post stops to do it properly.

The central result is the **Schmidt decomposition**, a structure theorem that puts every bipartite pure state into a canonical, nearly diagonal form. From that form, entanglement becomes a matter of counting: how many terms does the decomposition need? One term means a product state, more than one means entanglement, and the reduced density matrices of the previous post fall out as a free corollary. As before $H_A, H_B$ are finite-dimensional with orthonormal bases $\{e_i\}, \{f_j\}$; the finite-dimensionality genuinely matters here, since the proof runs through the singular value decomposition of a finite matrix.

## Product and entangled states

**Definition 1.** A unit vector $\psi \in H_A \otimes H_B$ is a **product state** if there exist $\phi_A \in H_A$ and $\phi_B \in H_B$ with $\psi = \phi_A \otimes \phi_B$. Otherwise $\psi$ is **entangled**.

The definition is clean but operationally useless: it asks us to search over all factorizations and report failure. We want instead an invariant we can compute. The Schmidt decomposition provides exactly that.

## The Schmidt decomposition

**Theorem (Schmidt).** Let $\psi \in H_A \otimes H_B$ be a unit vector. Then there exist orthonormal sets $\{u_1, \dots, u_r\} \subseteq H_A$ and $\{v_1, \dots, v_r\} \subseteq H_B$, together with strictly positive reals $\lambda_1 \geq \dots \geq \lambda_r > 0$ satisfying $\sum_{k=1}^r \lambda_k^2 = 1$, such that
$$\psi = \sum_{k=1}^{r} \lambda_k\, u_k \otimes v_k.$$
The number $r$ and the coefficients $\{\lambda_k\}$ are uniquely determined by $\psi$.

**Proof.** Expand $\psi$ in the product basis as $\psi = \sum_{i,j} c_{ij}\, e_i \otimes f_j$ and collect the coefficients into a matrix $C = (c_{ij})$. Normalization of $\psi$ says $\sum_{i,j}|c_{ij}|^2 = 1$, i.e. $\|C\|_{\mathrm{HS}} = 1$. The **singular value decomposition** writes $C = U \Sigma V^\ast$ with $U, V$ unitary and $\Sigma$ diagonal with non-negative entries $\lambda_1 \geq \dots \geq \lambda_r > 0$ (and zeros beyond), so that
$$c_{ij} = \sum_k U_{ik}\,\lambda_k\,\overline{V_{jk}}.$$
Define $u_k := \sum_i U_{ik}\,e_i$ and $v_k := \sum_j \overline{V_{jk}}\,f_j$. Because the columns of a unitary matrix are orthonormal,
$$\langle u_k, u_l\rangle = \sum_i \overline{U_{ik}}\,U_{il} = \delta_{kl}, \qquad \langle v_k, v_l\rangle = \sum_j V_{jk}\,\overline{V_{jl}} = \delta_{kl},$$
so $\{u_k\}$ and $\{v_k\}$ are orthonormal. Substituting,
$$\psi = \sum_{i,j} c_{ij}\, e_i\otimes f_j = \sum_k \lambda_k \Bigl(\sum_i U_{ik} e_i\Bigr)\otimes\Bigl(\sum_j \overline{V_{jk}} f_j\Bigr) = \sum_k \lambda_k\, u_k \otimes v_k.$$
The Frobenius norm is unchanged by the unitaries, so $\sum_k \lambda_k^2 = \|C\|_{\mathrm{HS}}^2 = 1$. Uniqueness of $r$ and $\{\lambda_k\}$ follows from the uniqueness of the singular values of $C$, which we will re-derive in the next proposition as eigenvalues of an operator attached to $\psi$. $\square$

**Definition 2.** The number $r$ of nonzero coefficients in the Schmidt decomposition of $\psi$ is the **Schmidt rank**, and the $\{\lambda_k\}$ are the **Schmidt coefficients**.

The Schmidt rank is the invariant we were after. It collapses the search over all factorizations into a single integer.

**Proposition 1.** A unit vector $\psi \in H_A \otimes H_B$ is a product state if and only if its Schmidt rank is $1$.

**Proof.** If $r = 1$ then $\psi = \lambda_1\, u_1 \otimes v_1$ with $\lambda_1 = 1$, a product state. Conversely, if $\psi = \phi_A \otimes \phi_B$, then writing $\phi_A, \phi_B$ as unit vectors (absorbing norms and a phase into one factor) gives a decomposition with a single term, and uniqueness forces $r = 1$. $\square$

So entanglement is simply $r > 1$. It remains to connect this to the reduced states of the previous post, and here the decomposition pays off immediately.

**Proposition 2.** Let $\psi = \sum_k \lambda_k\, u_k\otimes v_k$ be the Schmidt decomposition of a unit vector, and let $\rho = \psi\langle\psi,\cdot\rangle$. Then the reduced density matrices are
$$\rho_A = \operatorname{tr}_B(\rho) = \sum_k \lambda_k^2\, u_k\langle u_k, \cdot\rangle, \qquad \rho_B = \operatorname{tr}_A(\rho) = \sum_k \lambda_k^2\, v_k\langle v_k, \cdot\rangle.$$
In particular $\rho_A$ and $\rho_B$ have the same nonzero eigenvalues, namely $\{\lambda_k^2\}$, and $\psi$ is a product state if and only if $\rho_A$ (equivalently $\rho_B$) is pure.

**Proof.** Using the defining property of the partial trace on $\rho = \sum_{k,l}\lambda_k\lambda_l\,(u_k\otimes v_k)\langle u_l\otimes v_l, \cdot\rangle$ and the orthonormality $\langle v_l, v_k\rangle = \delta_{kl}$, the cross terms vanish and
$$\rho_A = \sum_{k,l}\lambda_k\lambda_l\,\langle v_l, v_k\rangle\, u_k\langle u_l, \cdot\rangle = \sum_k \lambda_k^2\, u_k\langle u_k, \cdot\rangle.$$
The computation for $\rho_B$ is identical with the roles exchanged. Both reduced states are therefore diagonal in their Schmidt bases with eigenvalues $\{\lambda_k^2\}$. By **Proposition 4** of the first post, $\rho_A$ is pure iff a single eigenvalue equals $1$, i.e. iff $r = 1$, which by **Proposition 1** is exactly the product case. $\square$

This closes the loop with the previous post. The maximally mixed reduced state $\rho_A = \tfrac12 I$ we found for the Bell state $\Psi = \tfrac1{\sqrt2}(e_0\otimes e_0 + e_1\otimes e_1)$ is now legible: its Schmidt coefficients are $\lambda_1 = \lambda_2 = \tfrac1{\sqrt2}$, the reduced eigenvalues are $\tfrac12, \tfrac12$, and the state is as far from a product as a two-qubit state can be. The "lost" information was the off-diagonal correlation between $A$ and $B$, and the Schmidt form displays it as the presence of a second term. States whose reduced state is maximally mixed are called **maximally entangled**, and the Bell states are the canonical examples.

## Mixed states and the trouble they cause

Everything above concerned *pure* states, where entanglement reduces to the single integer $r$. For mixed states the question is harder, and honesty requires admitting that it is not fully solved.

**Definition 3.** A density matrix $\rho$ on $H_A \otimes H_B$ is **separable** if it is a convex combination of product states,
$$\rho = \sum_k p_k\; \rho_A^{(k)} \otimes \rho_B^{(k)}, \qquad p_k \geq 0,\ \sum_k p_k = 1,$$
with each $\rho_A^{(k)}, \rho_B^{(k)}$ a density matrix on the respective factor. Otherwise $\rho$ is **entangled**.

The definition is the natural one — a separable state is one that can be prepared by local operations and shared classical randomness, with no quantum correlation manufactured between the parts. The difficulty is that, unlike the pure case, there is no decomposition to read $r$ off from, and deciding membership in the separable set is computationally hard in general. What we have instead are *necessary* conditions, the cleanest of which is the following.

**Theorem (PPT criterion; Peres 1996, Horodecki³ 1996).** Let $\rho^{T_B}$ denote the partial transpose of $\rho$ on the $B$ factor, i.e. the operator with matrix elements
$$\langle e_i\otimes f_j,\; \rho^{T_B}\, e_k\otimes f_l\rangle = \langle e_i\otimes f_l,\; \rho\, e_k\otimes f_j\rangle.$$
If $\rho$ is separable, then $\rho^{T_B} \geq 0$. Moreover, when $\dim H_A \cdot \dim H_B \leq 6$ — that is, in dimensions $2\times 2$ and $2\times 3$ — the converse also holds: $\rho^{T_B}\geq 0$ implies $\rho$ separable.

The asymmetry in that statement is the honest part. Transposition sends a density matrix to a density matrix, but it is positive without being *completely* positive, and the partial transpose can therefore drag an entangled state below zero. That a separable state survives the operation is immediate from **Definition 3**, since each $(\rho_A^{(k)}\otimes\rho_B^{(k)})^{T_B} = \rho_A^{(k)}\otimes(\rho_B^{(k)})^{T}$ is again a product of density matrices and hence non-negative. The converse holding only in the smallest dimensions is not a gap in the argument but a fact about the world: in $3\times 3$ and beyond there exist entangled states with non-negative partial transpose, the so-called bound entangled states, invisible to this test. We will not need them, but it is worth knowing the clean criterion has a ceiling.

## Looking ahead

We now have states — pure and mixed, product and entangled — and a fairly complete picture of the bipartite pure case. What we do not yet have is *dynamics*: a description of how states change, how they are measured, and what happens to a system left exposed to an environment it cannot control. Notably, the partial trace of the last post and the not-completely-positive transpose of this one have both quietly raised the question of which maps on density matrices are physically allowed.

The [next post](/posts/quantum-channels/) answers it. We characterize the physically realizable operations on density matrices as the **completely positive, trace-preserving maps**, prove the **Kraus representation** that writes every such map in a concrete operator form, and meet the **Stinespring dilation**, which reveals every channel as a unitary on a larger system followed by — fittingly — a partial trace. The thread of this post, that ignoring a subsystem is where the interesting structure hides, turns out to run straight through the theory of open quantum systems.

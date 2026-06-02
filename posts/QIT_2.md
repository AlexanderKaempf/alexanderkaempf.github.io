---
title: QIT 2 - Composite Systems and the Partial Trace
published_date: 2025-10-09 14:22:03 +0000
description: Mixed states appear when "joining" two states
tags:
  - quantum-information
  - density-matrices
  - entanglement
data:
  lang: en
  nav: posts
  alt_url: /de/posts/zusammengesetzte-systeme/
---

In the [previous post](/posts/density-matrices/) we replaced the unit vector by the density matrix and found, in the interior of the state space, the mixed states. At that point, the existence of mixed states was only a mathematical property of our new model. In this post, we will see how mixed states arise by "joining" two quantum systems together.

The setting is two quantum systems, $A$ and $B$, regarded as one. The mathematical object that joins them is the tensor product; the operation that lets us forget one of them is the partial trace; and the consequence — that a pure state of the whole can have a mixed state on each part — is the first genuinely quantum phenomenon we meet. Throughout, $H_A$ and $H_B$ are finite-dimensional Hilbert spaces, with orthonormal bases $\{e_i\}$ and $\{f_j\}$ respectively. The finite-dimensional restriction is a convenience, not a necessity, and I will flag where it is doing work.

## The tensor product

**Definition 1.** The **composite system** of $A$ and $B$ has Hilbert space $H_A \otimes H_B$, the tensor product, with the inner product determined on product vectors by
$$\langle \phi_A \otimes \phi_B,\; \chi_A \otimes \chi_B\rangle = \langle \phi_A, \chi_A\rangle\,\langle \phi_B, \chi_B\rangle$$
and extended sesquilinearly. The family $\{e_i \otimes f_j\}$ is then an orthonormal basis of $H_A \otimes H_B$.

Two remarks fix the picture. First, an operator $A \otimes B \in B(H_A \otimes H_B)$ acts on product vectors by $(A\otimes B)(\phi_A\otimes\phi_B) = A\phi_A \otimes B\phi_B$, and these *product operators* span $B(H_A\otimes H_B)$ — but they do not exhaust it, which is the whole story. Second, a general vector of $H_A \otimes H_B$ is a sum $\sum_{ij} c_{ij}\, e_i \otimes f_j$ and need not factor as a single product $\phi_A \otimes \phi_B$. The vectors that do not factor are exactly the ones we will eventually call entangled; for now we only need that they exist.

## The partial trace

We want an operation that takes a state of the composite and returns the state of $A$ alone — the state that reproduces every measurement made on $A$ while ignoring $B$ entirely. That requirement turns out to pin the operation down uniquely.

**Definition 2.** Let $\{f_j\}$ be an orthonormal basis of $H_B$. The **partial trace over $B$** is the linear map $\operatorname{tr}_B : B(H_A \otimes H_B) \to B(H_A)$ determined by
$$\langle \phi,\, (\operatorname{tr}_B M)\,\chi\rangle = \sum_j \langle \phi \otimes f_j,\; M\,(\chi\otimes f_j)\rangle, \qquad \phi, \chi \in H_A.$$
For a density matrix $\rho$ on $H_A \otimes H_B$ we write $\rho_A := \operatorname{tr}_B(\rho)$ and call it the **reduced density matrix** of $A$. The map $\operatorname{tr}_A$ and the reduced state $\rho_B$ are defined symmetrically.

The definition refers to a basis $\{f_j\}$, which is unsatisfying; we would like the reduced state to be a property of the physics, not of our choice of coordinates on a system we have agreed to ignore. The following proposition removes the basis and, in doing so, says precisely why the partial trace is the *right* operation.

**Proposition 1.** $\operatorname{tr}_B M$ is the unique operator in $B(H_A)$ satisfying
$$\operatorname{tr}\bigl((\operatorname{tr}_B M)\,A\bigr) = \operatorname{tr}\bigl(M\,(A\otimes I_B)\bigr) \qquad \text{for all } A \in B(H_A).$$
In particular, $\operatorname{tr}_B M$ does not depend on the chosen basis $\{f_j\}$.

**Proof.** Compute the right-hand side in the product basis $\{e_i \otimes f_j\}$:
$$\operatorname{tr}\bigl(M(A\otimes I_B)\bigr) = \sum_{i,j} \langle e_i\otimes f_j,\; M(Ae_i \otimes f_j)\rangle.$$
Now compute the left-hand side with the operator $\operatorname{tr}_B$ from **Definition 2**, in the basis $\{e_i\}$:

$$\operatorname{tr}\bigl((\operatorname{tr}_B M)A\bigr) = \sum_i \langle e_i,\, (\operatorname{tr}_B M)\,Ae_i\rangle = \sum_i \sum_j \langle e_i \otimes f_j,\; M(Ae_i\otimes f_j)\rangle.$$

The two sums agree, confirming that the operator from **Definition 2** satisfies the stated identity. For uniqueness, recall that the pairing $(N, A) \mapsto \operatorname{tr}(NA)$ on $B(H_A)$ is non-degenerate: if $\operatorname{tr}(NA) = 0$ for all $A$, then taking $A = N^\ast$ gives $\operatorname{tr}(N^\ast N) = \|N\|_{\mathrm{HS}}^2 = 0$, hence $N = 0$. Therefore any operator satisfying the identity is uniquely determined by it, and since the right-hand side makes no reference to $\{f_j\}$, neither does the partial trace. $\square$

So the partial trace is not one operation among several; it is *the* operation consistent with the demand that local measurements on $A$ be computed from a single operator on $H_A$. That demand is exactly condition (the defining identity), and it has one solution.

**Proposition 2.** If $\rho$ is a density matrix on $H_A \otimes H_B$, then $\rho_A = \operatorname{tr}_B(\rho)$ is a density matrix on $H_A$.

**Proof.** We check the three defining properties from the previous post.

*Self-adjoint.* For $\phi, \chi \in H_A$, using self-adjointness of $\rho$,
$$\langle \phi, \rho_A \chi\rangle = \sum_j \langle \phi\otimes f_j,\, \rho(\chi\otimes f_j)\rangle = \sum_j \langle \rho(\phi\otimes f_j),\, \chi\otimes f_j\rangle = \overline{\langle \chi, \rho_A \phi\rangle}.$$

*Non-negative.* For $\phi \in H_A$, again because $\rho \geq 0$,
$$\langle \phi, \rho_A \phi\rangle = \sum_j \langle \phi\otimes f_j,\, \rho(\phi\otimes f_j)\rangle \geq 0.$$

*Trace one.* Apply **Proposition 1** with $A = I_A$:
$$\operatorname{tr}(\rho_A) = \operatorname{tr}\bigl(\rho_A\, I_A\bigr) = \operatorname{tr}\bigl(\rho\,(I_A\otimes I_B)\bigr) = \operatorname{tr}(\rho) = 1. \quad\square$$

## The return of mixedness

We can now make good on the promise. The reduced state of a pure composite state need not be pure — and not because we have averaged over any classical uncertainty, but because the global state simply does not factor.

**Proposition 3.** Let $H_A = H_B = \mathbb{C}^2$ with orthonormal basis $\{e_0, e_1\}$, and set
$$\Psi = \tfrac{1}{\sqrt{2}}\,(e_0\otimes e_0 + e_1\otimes e_1) \in H_A \otimes H_B.$$
Then $\Psi\langle\Psi, \cdot\rangle$ is a pure state on the composite, yet its reduced state is
$$\rho_A = \tfrac{1}{2}\,I,$$
which is mixed — indeed maximally so, sitting at the center of the Bloch ball.

**Proof.** $\Psi$ is a unit vector, so by **Proposition 3** of the previous post $\Psi\langle\Psi, \cdot\rangle$ is a pure density matrix on $H_A \otimes H_B$. For the reduced state, compute its matrix elements directly from **Definition 2**:
$$\langle e_m, \rho_A\, e_n\rangle = \sum_{j} \langle e_m \otimes e_j,\, \Psi\rangle\,\langle \Psi,\, e_n\otimes e_j\rangle.$$
Since $\langle e_m\otimes e_j, \Psi\rangle = \tfrac{1}{\sqrt2}\,\delta_{mj}$, this becomes
$$\langle e_m, \rho_A\, e_n\rangle = \sum_j \tfrac{1}{2}\,\delta_{mj}\,\delta_{nj} = \tfrac{1}{2}\,\delta_{mn},$$
so $\rho_A = \tfrac{1}{2} I$. Finally $\operatorname{tr}(\rho_A^2) = \tfrac{1}{2} < 1$, so by **Proposition 4** of the previous post $\rho_A$ is mixed. $\square$

This is worth dwelling on, because it is the hinge on which the rest of the series turns. The state $\Psi\langle\Psi,\cdot\rangle$ is *pure*: it is a definite, maximal-information state of the composite, an extremal point of the joint state space, with no classical ambiguity about which vector we hold. And yet, asked what state system $A$ is in, the formalism returns the most ignorant state there is. There is no contradiction, only a fact about quantum mechanics that has no classical shadow: a whole can be in a sharp state while neither of its parts is. The information that the maximally mixed $\rho_A$ appears to have lost was never stored in $A$, nor in $B$, but in the *correlations* between them — correlations that the partial trace, by construction, throws away.

A purely classical system can also have a mixed reduced description, but only by genuine ignorance: a coin under a cup is heads or tails, and our $\tfrac12, \tfrac12$ records what we do not know. The quantum case is different in kind. Here the global state is completely known and still the part is maximally uncertain. The next post gives this gap between the whole and its parts a name, a measure, and a structure theorem.

## Looking ahead

The vectors of $H_A \otimes H_B$ that do not factor as a single product were lurking in **Definition 1**, and the state $\Psi$ above is the cleanest example of one. In the [next post](/posts/entanglement/) we call such states *entangled*, and we prove the **Schmidt decomposition**: every pure state of a bipartite system has a canonical, almost diagonal form, from which its reduced states — and the exact sense in which it is or is not entangled — can be read off at a glance. The maximally mixed $\rho_A$ we just computed will turn out to be the signature of maximal entanglement, and that is no coincidence.

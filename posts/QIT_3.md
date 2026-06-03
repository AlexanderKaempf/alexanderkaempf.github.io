---
title: QIT 3 - Entanglement and the Schmidt Decomposition
published_date: 2025-11-16 18:01:55 +0000
description: A canonical form for bipartite pure states that makes entanglement immediately visible.
tags:
  - quantum-information
  - entanglement
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/verschraenkung/
---

A pure state of a composite system is either a product of local states or it is not; the latter is entanglement. The Schmidt decomposition supplies canonical coordinates that make the distinction quantitative and exhibit the reduced states at once.

Throughout $\mathcal{H}\_A$, $\mathcal{H}\_B$ are finite-dimensional, and a state $|\psi\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_B$ is a unit vector unless stated otherwise.

## Product and entangled states

**Definition 1.** A pure state $|\psi\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_B$ is a *product state* if $|\psi\rangle = |a\rangle \otimes |b\rangle$ for some $|a\rangle, |b\rangle$, and *entangled* otherwise. A mixed state $\rho$ is *separable* if it is a convex combination of product states $\sum\_i q\_i  \sigma\_i \otimes \tau\_i$, and entangled otherwise.

Mixed-state separability is a strictly harder condition to test and is taken up later in the series. This post concerns the pure-state structure, which the next theorem resolves completely.

## The Schmidt decomposition

**Theorem 2.** For every unit vector $|\psi\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_B$ there are orthonormal sets $\{u\_k\} \subset \mathcal{H}\_A$ and $\{v\_k\} \subset \mathcal{H}\_B$ and reals $\lambda\_1 \geq \dots \geq \lambda\_r > 0$ with

$$|\psi\rangle = \sum\_{k=1}^{r} \lambda\_k  u\_k \otimes v\_k, \qquad \sum\_k \lambda\_k^2 = 1.$$

The number $r$ (the *Schmidt rank*) and the coefficients $\lambda\_k$ (the *Schmidt coefficients*) are uniquely determined by $|\psi\rangle$.

*Proof.* Expand $|\psi\rangle$ in product bases as $|\psi\rangle = \sum\_{i,j} c\_{ij}  e\_i \otimes f\_j$ and collect the coefficients into a matrix $C = (c\_{ij})$. Normalization reads $\sum\_{i,j} \lvert c\_{ij}\rvert^2 = 1$, i.e. $\lVert C\rVert\_{\mathrm{HS}} = 1$. The singular value decomposition writes $C = U \Sigma V^\ast$ with $U, V$ unitary and $\Sigma$ diagonal with entries $\lambda\_1 \geq \dots \geq \lambda\_r > 0$ (and zeros beyond), so that

$$c\_{ij} = \sum\_k U\_{ik}  \lambda\_k  \overline{V\_{jk}}.$$

Define $u\_k = \sum\_i U\_{ik}  e\_i$ and $v\_k = \sum\_j \overline{V\_{jk}}  f\_j$. Since the columns of a unitary matrix are orthonormal,

$$\langle u\_k, u\_l\rangle = \sum\_i \overline{U\_{ik}}  U\_{il} = \delta\_{kl}, \qquad \langle v\_k, v\_l\rangle = \sum\_j V\_{jk}  \overline{V\_{jl}} = \delta\_{kl},$$

so $\{u\_k\}$ and $\{v\_k\}$ are orthonormal. Substituting,

$$|\psi\rangle = \sum\_{i,j} c\_{ij}  e\_i \otimes f\_j = \sum\_k \lambda\_k \Bigl(\sum\_i U\_{ik}  e\_i\Bigr) \otimes \Bigl(\sum\_j \overline{V\_{jk}}  f\_j\Bigr) = \sum\_k \lambda\_k  u\_k \otimes v\_k.$$

The Hilbert–Schmidt norm is invariant under the unitaries, so $\sum\_k \lambda\_k^2 = \lVert C\rVert\_{\mathrm{HS}}^2 = 1$. Uniqueness of $r$ and the $\lambda\_k$ follows from uniqueness of the singular values of $C$, re-derived below as eigenvalues of the reduced state. $\square$

## Reduced states and the Schmidt rank

**Proposition 3.** If $|\psi\rangle = \sum\_k \lambda\_k  u\_k \otimes v\_k$ is a Schmidt decomposition, then

$$\rho\_A = \operatorname{tr}\_B |\psi\rangle\langle\psi| = \sum\_k \lambda\_k^2  |u\_k\rangle\langle u\_k|, \qquad \rho\_B = \operatorname{tr}\_A |\psi\rangle\langle\psi| = \sum\_k \lambda\_k^2  |v\_k\rangle\langle v\_k|.$$

In particular $\rho\_A$ and $\rho\_B$ share the nonzero spectrum $\{\lambda\_k^2\}$, and $\operatorname{rank}\rho\_A = \operatorname{rank}\rho\_B = r$.

*Proof.* Orthonormality of $\{v\_k\}$ gives $\operatorname{tr}|v\_k\rangle\langle v\_l| = \delta\_{kl}$, so

$$\operatorname{tr}\_B |\psi\rangle\langle\psi| = \sum\_{k,l} \lambda\_k \lambda\_l  |u\_k\rangle\langle u\_l|  \operatorname{tr}|v\_k\rangle\langle v\_l| = \sum\_k \lambda\_k^2  |u\_k\rangle\langle u\_k|.$$

The computation for $\rho\_B$ is identical. Both are diagonal with the same nonzero entries $\lambda\_k^2$, $k = 1, \dots, r$. $\square$

This exhibits the Schmidt coefficients as the square roots of the common eigenvalues of the two reduced states, which proves their uniqueness and closes the gap left in Theorem 2.

**Corollary 4.** For a pure bipartite state the following are equivalent: $|\psi\rangle$ is a product state; its Schmidt rank is $1$; $\rho\_A$ is pure; $\rho\_B$ is pure.

*Proof.* A product state $|a\rangle \otimes |b\rangle$ is a single Schmidt term, and conversely $r = 1$ gives $|\psi\rangle = \lambda\_1  u\_1 \otimes v\_1$ with $\lambda\_1 = 1$, a product. By Proposition 3, $r = 1$ holds exactly when $\rho\_A = |u\_1\rangle\langle u\_1|$ is rank one, i.e. pure, and likewise for $\rho\_B$. $\square$

Thus a pure state is entangled precisely when its reduced states are mixed: entanglement of a pure bipartite state is the local loss of purity.

## Entanglement entropy

**Definition 5.** The *entanglement entropy* of $|\psi\rangle$ is the von Neumann entropy of either reduced state,

$$E(\psi) = S(\rho\_A) = -\sum\_k \lambda\_k^2 \log \lambda\_k^2,$$

which equals $S(\rho\_B)$ by Proposition 3 and is therefore well-defined.

By Corollary 4, $E(\psi) = 0$ holds exactly for product states. With $d = \min(d\_A, d\_B)$, the entropy is maximal, equal to $\log d$, exactly when all $\lambda\_k^2 = 1/d$; such states are called maximally entangled, and their reduced states are the maximally mixed operator $I/d$.

## Uniqueness of purification

The Schmidt decomposition settles the uniqueness clause of the purification theorem from the previous post.

**Proposition 6.** If $|\psi\rangle, |\psi'\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_R$ both purify $\rho \in \mathcal{S}(\mathcal{H}\_A)$, there is a unitary $W$ on $\mathcal{H}\_R$ with $(I \otimes W)|\psi\rangle = |\psi'\rangle$.

*Proof.* By Proposition 3 the $A$-reductions of $|\psi\rangle$ and $|\psi'\rangle$ both equal $\rho$, so both Schmidt decompositions have coefficients $\lambda\_k = \sqrt{p\_k}$, where the $p\_k$ are the eigenvalues of $\rho$. Choosing the same eigenbasis $\{u\_k\}$ of $\rho$ for the $A$-factor of both (possible after a unitary on each eigenspace of $\rho$ that fixes $\rho$ and acts within the relevant Schmidt vectors), write $|\psi\rangle = \sum\_k \sqrt{p\_k}  u\_k \otimes v\_k$ and $|\psi'\rangle = \sum\_k \sqrt{p\_k}  u\_k \otimes v\_k'$ with $\{v\_k\}, \{v\_k'\}$ orthonormal in $\mathcal{H}\_R$. Define $W$ on $\mathcal{H}\_R$ by $W v\_k = v\_k'$, extended to a unitary on the orthogonal complement. Then $(I \otimes W)|\psi\rangle = |\psi'\rangle$. $\square$
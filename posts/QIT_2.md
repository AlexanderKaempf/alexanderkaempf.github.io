---
title: QIT 2 - Composite Systems, Partial Trace
published_date: 2025-10-09 14:22:03 +0000
tags:
  - quantum-information
  - density-matrices
  - entanglement
data:
  lang: en
  nav: posts
  alt_url: /de/posts/zusammengesetzte-systeme/
---

The state space of a composite system is the tensor product of its component spaces. This post fixes the tensor-product formalism, defines the partial trace and the reduced state it produces, and proves that every mixed state is the restriction of a pure state on a larger system.

Throughout $\mathcal{H}\_A$, $\mathcal{H}\_B$ are finite-dimensional with $d\_A = \dim\mathcal{H}\_A$, $d\_B = \dim\mathcal{H}\_B$, and $\mathcal{S}(\mathcal{H})$ denotes the density operators on $\mathcal{H}$: positive semidefinite, unit trace.

## The tensor product

**Definition 1.** The state space of the system composed of $A$ and $B$ is $\mathcal{H}\_A \otimes \mathcal{H}\_B$, the tensor product, with inner product determined by $\langle a \otimes b,  a' \otimes b'\rangle = \langle a, a'\rangle \langle b, b'\rangle$ on product vectors. It has dimension $d\_A d\_B$, and the product vectors $a \otimes b$ span it.

**Proposition 2.** The product operators $A \otimes B$, defined by $(A \otimes B)(a \otimes b) = Aa \otimes Bb$, span $\mathcal{B}(\mathcal{H}\_A \otimes \mathcal{H}\_B)$.

*Proof.* The map $(A, B) \mapsto A \otimes B$ is bilinear, and for bases $\{A\_i\}$ of $\mathcal{B}(\mathcal{H}\_A)$ and $\{B\_j\}$ of $\mathcal{B}(\mathcal{H}\_B)$ the operators $A\_i \otimes B\_j$ are linearly independent with count $d\_A^2 d\_B^2 = \dim\mathcal{B}(\mathcal{H}\_A \otimes \mathcal{H}\_B)$. $\square$

## The partial trace

**Definition 3.** The *partial trace over $B$* is the unique linear map $\operatorname{tr}\_B : \mathcal{B}(\mathcal{H}\_A \otimes \mathcal{H}\_B) \to \mathcal{B}(\mathcal{H}\_A)$ satisfying $\operatorname{tr}\_B(A \otimes B) = (\operatorname{tr} B) A$.

**Proposition 4.** $\operatorname{tr}\_B$ exists and is unique. For any orthonormal basis $\{|k\rangle\}$ of $\mathcal{H}\_B$,

$$\operatorname{tr}\_B X = \sum\_k (I \otimes \langle k|)  X  (I \otimes |k\rangle),$$

independently of the basis, and it is characterized by the adjoint relation $\operatorname{tr}\bigl((M \otimes I) X\bigr) = \operatorname{tr}\bigl(M  \operatorname{tr}\_B X\bigr)$ for all $M \in \mathcal{B}(\mathcal{H}\_A)$ and $X \in \mathcal{B}(\mathcal{H}\_A \otimes \mathcal{H}\_B)$.

*Proof.* The displayed formula is linear and gives $\operatorname{tr}\_B(A \otimes B) = A \sum\_k \langle k| B |k\rangle = (\operatorname{tr} B) A$, so a map with the required property exists. Writing $X = \sum\_i A\_i \otimes B\_i$,

$$\operatorname{tr}\bigl((M \otimes I) X\bigr) = \sum\_i \operatorname{tr}(M A\_i) \operatorname{tr}(B\_i) = \operatorname{tr}\Bigl(M \sum\_i (\operatorname{tr} B\_i)  A\_i\Bigr) = \operatorname{tr}\bigl(M  \operatorname{tr}\_B X\bigr),$$

which is the adjoint relation. If $S$ also satisfies it then $\operatorname{tr}\bigl(M(\operatorname{tr}\_B X - S X)\bigr) = 0$ for every $M$; nondegeneracy of the pairing $(M, N) \mapsto \operatorname{tr}(M N)$ on $\mathcal{B}(\mathcal{H}\_A)$ forces $\operatorname{tr}\_B X = S X$. The characterization references no basis, so the formula is basis-independent. $\square$

## Reduced states

**Proposition 5.** For $\rho \in \mathcal{S}(\mathcal{H}\_A \otimes \mathcal{H}\_B)$, the reduced operator $\rho\_A = \operatorname{tr}\_B \rho$ lies in $\mathcal{S}(\mathcal{H}\_A)$ and is the unique state reproducing all local expectations: $\operatorname{tr}\bigl(\rho (M \otimes I)\bigr) = \operatorname{tr}(\rho\_A M)$ for all $M$.

*Proof.* Taking $M = I$ in the adjoint relation gives $\operatorname{tr} \rho\_A = \operatorname{tr} \rho = 1$. For $\rho \geq 0$ and any $|a\rangle \in \mathcal{H}\_A$,

$$\langle a| \rho\_A |a\rangle = \sum\_k \bigl(\langle a| \otimes \langle k|\bigr)  \rho  \bigl(|a\rangle \otimes |k\rangle\bigr) \geq 0,$$

so $\rho\_A \geq 0$ and $\rho\_A \in \mathcal{S}(\mathcal{H}\_A)$. The local-expectation identity is the adjoint relation with $X = \rho$, and uniqueness follows from nondegeneracy of the trace pairing as in Proposition 4. $\square$

The partial trace is the operation of discarding system $B$: it is positive and trace-preserving by Proposition 5, hence a candidate channel in the sense of the dynamics post, where its complete positivity is established. The reduced state $\rho\_A$ encodes every measurement statistic accessible to an observer holding only $A$; nothing measured on $A$ can detect operations applied to $B$ alone.

## Purification

**Theorem 6.** For every $\rho \in \mathcal{S}(\mathcal{H}\_A)$ there is a Hilbert space $\mathcal{H}\_R$ with $\dim\mathcal{H}\_R = \operatorname{rank}\rho$ and a unit vector $|\psi\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_R$ with $\operatorname{tr}\_R |\psi\rangle\langle\psi| = \rho$. Any two purifications of $\rho$ in the same reference space are related by a unitary on $\mathcal{H}\_R$.

*Proof.* Let $\rho = \sum\_{k=1}^{r} p\_k |a\_k\rangle\langle a\_k|$ be a spectral decomposition with $p\_k > 0$ and $r = \operatorname{rank}\rho$. Take $\mathcal{H}\_R = \mathbb{C}^r$ with orthonormal basis $\{|r\_k\rangle\}$ and set $|\psi\rangle = \sum\_k \sqrt{p\_k}  |a\_k\rangle \otimes |r\_k\rangle$. Then $\||\psi\rangle\|^2 = \sum\_k p\_k = 1$, and

$$\operatorname{tr}\_R |\psi\rangle\langle\psi| = \sum\_{k,l} \sqrt{p\_k p\_l}  |a\_k\rangle\langle a\_l|  \operatorname{tr}|r\_k\rangle\langle r\_l| = \sum\_k p\_k |a\_k\rangle\langle a\_k| = \rho.$$

Uniqueness up to a unitary on $\mathcal{H}\_R$ is the content of the Schmidt decomposition, proved in the next post. $\square$
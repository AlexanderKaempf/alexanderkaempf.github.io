---
title: QIT 4 - Quantum Channels
published_date: 2026-01-18 14:32:19 +0200
layout: default.liquid
is_draft: false
data:
  lang: en
  nav: posts
  alt_url: /de/posts/Kanaele/

tags:
  - quantum-information
  - operator-algebras
---

Closed-system evolution is unitary: $\rho \mapsto U\rho U^\ast$. A system coupled to an environment it cannot access evolves by a strictly larger class of maps, obtained by applying a joint unitary and discarding the environment. This post fixes the class precisely — the completely positive trace-preserving (CPTP) maps — and proves three equivalent descriptions: the operator-sum (Kraus) form, dilation to an isometry (Stinespring), and a linear bijection with bipartite operators (Choi).

Throughout, $\mathcal{H}\_A$, $\mathcal{H}\_B$ are finite-dimensional, $d\_A = \dim\mathcal{H}\_A$, $d\_B = \dim\mathcal{H}\_B$, and $\mathcal{B}(\mathcal{H}\_A,\mathcal{H}\_B)$ the linear maps between them. A linear $\Phi : \mathcal{B}(\mathcal{H}\_A) \to \mathcal{B}(\mathcal{H}\_B)$ on operators is called a superoperator. Density operators are positive semidefinite with unit trace.

## Positivity is not enough

**Definition 1.** A superoperator $\Phi : \mathcal{B}(\mathcal{H}\_A) \to \mathcal{B}(\mathcal{H}\_B)$ is *positive* if $X \geq 0$ implies $\Phi(X) \geq 0$; *completely positive* (CP) if $\Phi \otimes \mathrm{id}\_n$ is positive on $\mathcal{B}(\mathcal{H}\_A \otimes \mathbb{C}^n)$ for every $n \geq 1$; *trace-preserving* (TP) if $\operatorname{tr}\Phi(X) = \operatorname{tr} X$ for all $X$. A *quantum channel* is a CPTP map.

The physical content of complete positivity is that $\Phi$ remains a valid state transformation when applied to one half of a larger system. Positivity alone fails this, and the standard witness is the transpose.

**Proposition 2.** The transpose $T : \mathcal{B}(\mathbb{C}^2) \to \mathcal{B}(\mathbb{C}^2)$, $T(X) = X^\top$, is positive but not completely positive.

*Proof.* Transposition preserves the spectrum, hence positivity, so $T$ is positive. Let $|\Omega\rangle = |00\rangle + |11\rangle$ and $P = |\Omega\rangle\langle\Omega| \geq 0$. Writing $P = \sum\_{i,j} |i\rangle\langle j| \otimes |i\rangle\langle j|$ and transposing the first tensor factor,

$$(T \otimes \mathrm{id})(P) = \sum\_{i,j} |j\rangle\langle i| \otimes |i\rangle\langle j| = \sum\_{i,j} |ji\rangle\langle ij|,$$

which is the swap operator on $\mathbb{C}^2 \otimes \mathbb{C}^2$. Swap has eigenvalue $-1$ on the antisymmetric subspace, so $(T \otimes \mathrm{id})(P) \not\geq 0$ and $T \otimes \mathrm{id}\_2$ is not positive. $\square$

## The Kraus representation

Fix an orthonormal basis $\{|i\rangle\}$ of $\mathcal{H}\_A$ and set $|\Omega\rangle = \sum\_i |i\rangle \otimes |i\rangle \in \mathcal{H}\_A \otimes \mathcal{H}\_A$ (unnormalized). For $K \in \mathcal{B}(\mathcal{H}\_A,\mathcal{H}\_B)$ define the vectorization $v(K) = (K \otimes I)|\Omega\rangle = \sum\_i K|i\rangle \otimes |i\rangle \in \mathcal{H}\_B \otimes \mathcal{H}\_A$. The map $K \mapsto v(K)$ is linear and injective ($v(K) = 0$ forces $K|i\rangle = 0$ for all $i$), and both spaces have dimension $d\_A d\_B$, so it is a bijection.

The **Choi operator** of $\Phi$ is $J(\Phi) = (\Phi \otimes \mathrm{id})(|\Omega\rangle\langle\Omega|) \in \mathcal{B}(\mathcal{H}\_B \otimes \mathcal{H}\_A)$. Expanding, $J(\Phi) = \sum\_{i,j} \Phi(|i\rangle\langle j|) \otimes |i\rangle\langle j|$.

**Lemma 3.** $\Phi$ is recovered from its Choi operator by $\Phi(X) = \operatorname{tr}\_A\bigl[(I\_B \otimes X^\top) J(\Phi)\bigr]$. Consequently $\Phi \mapsto J(\Phi)$ is a linear bijection from superoperators onto $\mathcal{B}(\mathcal{H}\_B \otimes \mathcal{H}\_A)$.

*Proof.* With $X = \sum\_{i,j} X\_{ij} |i\rangle\langle j|$,

$$\operatorname{tr}\_A\bigl[(I\_B \otimes X^\top) J(\Phi)\bigr] = \sum\_{i,j} \Phi(|i\rangle\langle j|) \langle j| X^\top |i\rangle = \sum\_{i,j} X\_{ij} \Phi(|i\rangle\langle j|) = \Phi(X).$$

So $J$ is injective. Both domain and codomain have dimension $d\_A^2 d\_B^2$, hence $J$ is bijective. $\square$

**Theorem 4 (Kraus / Choi).** $\Phi : \mathcal{B}(\mathcal{H}\_A) \to \mathcal{B}(\mathcal{H}\_B)$ is completely positive if and only if there exist $K\_k \in \mathcal{B}(\mathcal{H}\_A,\mathcal{H}\_B)$ with

$$\Phi(X) = \sum\_k K\_k X K\_k^\ast.$$

It is trace-preserving if and only if $\sum\_k K\_k^\ast K\_k = I$. The number of Kraus operators may be taken $\leq d\_A d\_B$.

*Proof.* For any $K, L$ one has $(K \otimes I)|\Omega\rangle\langle\Omega|(L \otimes I)^\ast = v(K) v(L)^\ast$.

($\Leftarrow$) If $\Phi(X) = \sum\_k K\_k X K\_k^\ast$ then for $Y \geq 0$, $(\Phi \otimes \mathrm{id})(Y) = \sum\_k (K\_k \otimes I) Y (K\_k \otimes I)^\ast \geq 0$, as each summand has the form $A Y A^\ast$. Hence $\Phi$ is CP.

($\Rightarrow$) If $\Phi$ is CP then $J(\Phi) = (\Phi \otimes \mathrm{id})(|\Omega\rangle\langle\Omega|) \geq 0$. Take a spectral decomposition $J(\Phi) = \sum\_k |\psi\_k\rangle\langle\psi\_k|$ with $|\psi\_k\rangle \in \mathcal{H}\_B \otimes \mathcal{H}\_A$, and let $K\_k$ be the unique operator with $v(K\_k) = |\psi\_k\rangle$. The map $\Psi(X) = \sum\_k K\_k X K\_k^\ast$ has Choi operator $\sum\_k v(K\_k) v(K\_k)^\ast = \sum\_k |\psi\_k\rangle\langle\psi\_k| = J(\Phi)$, so $\Psi = \Phi$ by Lemma 3. The decomposition has $\leq \operatorname{rank} J(\Phi) \leq d\_A d\_B$ terms.

For the trace condition, $\operatorname{tr}\Phi(X) = \operatorname{tr}\bigl(\sum\_k K\_k^\ast K\_k  X\bigr)$ by cyclicity, which equals $\operatorname{tr} X$ for every $X$ if and only if $\sum\_k K\_k^\ast K\_k = I$. $\square$

The Kraus operators are not unique: $\{K\_k\}$ and $\{K\_k'\}$ define the same channel exactly when $K\_k' = \sum\_l u\_{kl} K\_l$ for an isometric matrix $(u\_{kl})$ (pad the shorter list with zeros). This is the unitary freedom in the spectral decomposition of $J(\Phi)$.

## Stinespring dilation

**Theorem 5 (Stinespring).** $\Phi$ is CPTP if and only if there exist a Hilbert space $\mathcal{H}\_E$ and an isometry $V : \mathcal{H}\_A \to \mathcal{H}\_B \otimes \mathcal{H}\_E$ with

$$\Phi(X) = \operatorname{tr}\_E\bigl(V X V^\ast\bigr).$$

One may take $\dim\mathcal{H}\_E \leq d\_A d\_B$.

*Proof.* ($\Rightarrow$) Take Kraus operators $K\_1,\dots,K\_r$ with $\sum\_k K\_k^\ast K\_k = I$, let $\mathcal{H}\_E = \mathbb{C}^r$ with basis $\{|k\rangle\}$, and define $V|\phi\rangle = \sum\_k K\_k|\phi\rangle \otimes |k\rangle$. Then $V^\ast V = \sum\_k K\_k^\ast K\_k = I$, so $V$ is an isometry, and

$$\operatorname{tr}\_E(V X V^\ast) = \sum\_k K\_k X K\_k^\ast = \Phi(X).$$

($\Leftarrow$) Given such a $V$, fix a basis $\{|k\rangle\}$ of $\mathcal{H}\_E$ and put $K\_k = (I\_B \otimes \langle k|) V$. Then $\operatorname{tr}\_E(V X V^\ast) = \sum\_k K\_k X K\_k^\ast$, so $\Phi$ is CP by Theorem 4, and $\sum\_k K\_k^\ast K\_k = V^\ast V = I$, so $\Phi$ is TP. $\square$

When $d\_B = d\_A$ the isometry extends to a unitary $U$ on $\mathcal{H}\_A \otimes \mathcal{H}\_E$ acting on an environment prepared in a fixed pure state $|e\rangle$, recovering the "couple and discard" picture: $\Phi(\rho) = \operatorname{tr}\_E\bigl(U(\rho \otimes |e\rangle\langle e|)U^\ast\bigr)$. Stinespring says every channel is of this form, and the bound on $\dim\mathcal{H}\_E$ shows the environment need be no larger than $d\_A d\_B$.

## Choi–Jamiołkowski as a state correspondence

Normalize the Choi operator to $\tau(\Phi) = d\_A^{-1} J(\Phi)$.

**Proposition 6.** $\Phi$ is CPTP if and only if $\tau(\Phi) \geq 0$ and $\operatorname{tr}\_B \tau(\Phi) = d\_A^{-1} I\_A$. Thus channels $\mathcal{H}\_A \to \mathcal{H}\_B$ correspond bijectively to density operators on $\mathcal{H}\_B \otimes \mathcal{H}\_A$ whose marginal on $\mathcal{H}\_A$ is maximally mixed.

*Proof.* Complete positivity is equivalent to $J(\Phi) \geq 0$ by the proof of Theorem 4. For the marginal, $\operatorname{tr}\_B J(\Phi) = \sum\_{i,j} \operatorname{tr}\bigl(\Phi(|i\rangle\langle j|)\bigr) |i\rangle\langle j|$, and trace-preservation gives $\operatorname{tr}\Phi(|i\rangle\langle j|) = \delta\_{ij}$, so $\operatorname{tr}\_B J(\Phi) = I\_A$, i.e. $\operatorname{tr}\_B \tau(\Phi) = d\_A^{-1} I\_A$. Conversely this marginal condition forces $\operatorname{tr}\Phi(|i\rangle\langle j|) = \delta\_{ij}$, which is trace-preservation. $\square$

This turns questions about channels into questions about states. The next post treats the observational counterpart of this formalism: measurements, where the CP structure reappears as the building block of quantum instruments.

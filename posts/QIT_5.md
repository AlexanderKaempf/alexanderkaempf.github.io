---
title: QIT 5 - Measurements, POVMs, and Instruments
published_date: 2026-03-01 07:01:05 +0200
layout: default.liquid
is_draft: false
data:
  lang: en
  nav: posts
  alt_url: /de/posts/Messungen/

tags:
  - quantum-information
  - operator-algebras
---

The projection postulate describes one kind of measurement. The object that records outcome probabilities in full generality is a positive operator-valued measure (POVM); the object that records probabilities *together with* the resulting state is a quantum instrument. Both connect to the channel formalism of the previous post through dilation theorems and the completely positive structure.

Throughout $\mathcal{H}$ is finite-dimensional and outcome sets $\Omega$ are finite.

## POVMs

**Definition 1.** A *POVM* on $\mathcal{H}$ with outcome set $\Omega$ is a family $\{E\_m\}\_{m \in \Omega}$ of operators with $E\_m \geq 0$ and $\sum\_{m} E\_m = I$. On state $\rho$ the outcome $m$ occurs with probability $p(m) = \operatorname{tr}(\rho E\_m)$.

Positivity gives $p(m) \geq 0$ and the resolution of identity gives $\sum\_m p(m) = \operatorname{tr}\rho = 1$, so the Born rule yields a genuine distribution for every state. A *projection-valued measure* (PVM) is the special case where the $E\_m = P\_m$ are orthogonal projectors with $P\_m P\_{m'} = \delta\_{mm'} P\_m$; this is the von Neumann measurement of an observable with spectral projectors $P\_m$.

Not every POVM is a PVM, and the gap is genuine rather than a normalization artifact.

**Example 2.** On a qubit, let $|\psi\_k\rangle$, $k = 0,1,2$, be the trine states at angles $2\pi k/3$ on a great circle, and set $E\_k = \tfrac{2}{3}|\psi\_k\rangle\langle\psi\_k|$. Then $\sum\_k E\_k = \tfrac{2}{3} \cdot \tfrac{3}{2} I = I$, so $\{E\_k\}$ is a POVM with three outcomes. A PVM on $\mathbb{C}^2$ has at most two nonzero elements, so this is not a PVM. It is the optimal measurement for distinguishing the three trine states symmetrically.

## Naimark dilation

A POVM on $\mathcal{H}$ is the compression of a sharp measurement on a larger space, mirroring Stinespring for channels.

**Theorem 3 (Naimark).** For every POVM $\{E\_m\}$ on $\mathcal{H}$ there is a Hilbert space $\mathcal{K}$, an isometry $V : \mathcal{H} \to \mathcal{K}$, and a PVM $\{P\_m\}$ on $\mathcal{K}$ with $E\_m = V^\ast P\_m V$.

*Proof.* Let $\mathcal{K} = \mathcal{H} \otimes \mathbb{C}^{|\Omega|}$ with basis $\{|m\rangle\}$ of the second factor, and define $V|\phi\rangle = \sum\_m \bigl(\sqrt{E\_m} |\phi\rangle\bigr) \otimes |m\rangle$. Then $V^\ast V = \sum\_m E\_m = I$, so $V$ is an isometry. Take the PVM $P\_m = I \otimes |m\rangle\langle m|$. The adjoint acts by $V^\ast(\xi \otimes |m\rangle) = \sqrt{E\_m} \xi$, since $\langle\phi| V^\ast(\xi \otimes |m\rangle) = \langle V\phi | \xi \otimes |m\rangle = \langle \sqrt{E\_m} \phi | \xi \rangle$. Hence

$$V^\ast P\_m V |\phi\rangle = V^\ast\bigl(\sqrt{E\_m} |\phi\rangle \otimes |m\rangle\bigr) = \sqrt{E\_m} \sqrt{E\_m} |\phi\rangle = E\_m |\phi\rangle. \qquad \square$$

Stinespring and Naimark are the same statement for two coarse-grainings of measurement: a channel is an isometry followed by discarding the environment, a POVM is a projective measurement on the dilated space followed by restriction to $\mathcal{H}$. The next definition makes the common refinement explicit.

## Instruments

A POVM gives outcome probabilities but says nothing about the post-measurement state. The full object is the following.

**Definition 4.** A *quantum instrument* with outcome set $\Omega$ is a family $\{\Phi\_m\}\_{m \in \Omega}$ of completely positive maps on $\mathcal{B}(\mathcal{H})$ such that $\sum\_m \Phi\_m$ is trace-preserving. On input $\rho$, outcome $m$ occurs with probability $p(m) = \operatorname{tr}\Phi\_m(\rho)$, and the conditional post-measurement state is $\rho\_m = \Phi\_m(\rho)/p(m)$.

The instrument carries two coarse-grainings, each discarding one kind of information.

**Proposition 5.** Let $\{\Phi\_m\}$ be an instrument.

*Discarding the state.* The operators $E\_m = \Phi\_m^\ast(I)$ form a POVM, and $\operatorname{tr}\Phi\_m(\rho) = \operatorname{tr}(\rho E\_m)$ for all $\rho$.

*Discarding the outcome.* The map $\rho \mapsto \sum\_m \Phi\_m(\rho)$ is a channel — the non-selective evolution induced by the measurement.

*Proof.* Each $\Phi\_m$ is CP, so its adjoint $\Phi\_m^\ast$ is positive and $E\_m = \Phi\_m^\ast(I) \geq 0$. Trace-preservation of $\sum\_m \Phi\_m$ reads $\operatorname{tr}\bigl(\sum\_m \Phi\_m(\rho)\bigr) = \operatorname{tr}\rho$ for all $\rho$, equivalently $\sum\_m \Phi\_m^\ast(I) = I$, so $\{E\_m\}$ resolves the identity. The defining adjoint relation $\operatorname{tr}\Phi\_m(\rho) = \operatorname{tr}(\rho \Phi\_m^\ast(I)) = \operatorname{tr}(\rho E\_m)$ recovers the Born rule. The second claim is immediate: $\sum\_m \Phi\_m$ is CP as a sum of CP maps and TP by assumption. $\square$

So the instrument is the finest of the three objects, with the channel and the POVM as its two marginals: forget the outcome to get a channel, forget the state to get a POVM.

## Kraus form and the state-update freedom

Applying the Kraus theorem to each component, an instrument has the form $\Phi\_m(\rho) = \sum\_j M\_{m,j} \rho M\_{m,j}^\ast$, and trace-preservation of the sum reads $\sum\_{m,j} M\_{m,j}^\ast M\_{m,j} = I$. The induced POVM is $E\_m = \sum\_j M\_{m,j}^\ast M\_{m,j}$.

The POVM does not determine the instrument. Given any POVM $\{E\_m\}$, the family $\Phi\_m(\rho) = U\_m \sqrt{E\_m} \rho \sqrt{E\_m} U\_m^\ast$ is an instrument with that POVM for every choice of unitaries $U\_m$; the $U\_m$ describe state disturbance invisible to the outcome statistics. The choice $U\_m = I$ is the **Lüders instrument** $\Phi\_m(\rho) = \sqrt{E\_m} \rho \sqrt{E\_m}$.

**Proposition 6.** For the Lüders instrument of a PVM, $\Phi\_m(\rho) = P\_m \rho P\_m$, and the measurement is repeatable: $\Phi\_m \circ \Phi\_m = \Phi\_m$.

*Proof.* For a PVM $\sqrt{E\_m} = \sqrt{P\_m} = P\_m$, giving $\Phi\_m(\rho) = P\_m \rho P\_m$. Then $\Phi\_m(\Phi\_m(\rho)) = P\_m P\_m \rho P\_m P\_m = P\_m \rho P\_m = \Phi\_m(\rho)$ using $P\_m^2 = P\_m$. $\square$

Repeatability is special to projective measurements; a general POVM measured twice need not reproduce its first outcome. This is the precise sense in which projective measurements are the "sharp" case, and it closes the loop with the observable picture from the first post of the series. The next block turns from describing states and their evolution to using them: quantum computation.

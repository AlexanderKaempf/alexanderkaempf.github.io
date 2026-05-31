---
title: A Note on Spectral Graph Theory
published_date: 2026-04-08 09:00:00 +0000
description: How the eigenvalues of a graph's Laplacian quietly encode its shape, its bottlenecks, and how it connects.
tags:
  - graph-theory
  - linear-algebra
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/spektralgraphen/
---

Take a graph — vertices joined by edges — and build its Laplacian matrix
$L = D - A$, where $D$ is the diagonal degree matrix and $A$ the adjacency
matrix. The eigenvalues of $L$,

\[
0 = \lambda_1 \le \lambda_2 \le \cdots \le \lambda_n ,
\]

turn out to know a remarkable amount about the graph.

## The second eigenvalue tells a story

The multiplicity of $\lambda_1 = 0$ counts the connected components. But the
real protagonist is $\lambda_2$, the **algebraic connectivity**. When it is
small, the graph has a bottleneck — it can be cut into two large pieces by
removing few edges. When it is large, the graph is robustly woven together.

This is the discrete cousin of an idea from geometry: the lowest vibration
frequency of a drum reflects its shape. Here the "drum" is a network, and its
frequencies are eigenvalues.

## Why it pairs with number theory

In [the previous note](/posts/prime-gaps/) I wrote about prime gaps. The link is
spiritual rather than literal: in both cases a single spectrum — of primes, of a
matrix — compresses an enormous amount of structure into a list of numbers. Much
of my work is about learning to read those lists.

---
title: Eine Notiz zur Spektralgraphentheorie
published_date: 2026-04-08 09:00:00 +0000
permalink: /de/posts/spektralgraphen/
description: Wie die Eigenwerte des Laplace-Operators eines Graphen still seine Gestalt, seine Engstellen und seinen Zusammenhalt verschlüsseln.
tags:
  - graphentheorie
  - lineare-algebra
  - darstellung
data:
  lang: de
  nav: posts
  alt_url: /posts/spectral-graphs/
---

Man nehme einen Graphen — Knoten, durch Kanten verbunden — und bilde seine
Laplace-Matrix $L = D - A$, wobei $D$ die diagonale Gradmatrix und $A$ die
Adjazenzmatrix ist. Die Eigenwerte von $L$,

\[
0 = \lambda_1 \le \lambda_2 \le \cdots \le \lambda_n ,
\]

wissen erstaunlich viel über den Graphen.

## Der zweite Eigenwert erzählt eine Geschichte

Die Vielfachheit von $\lambda_1 = 0$ zählt die Zusammenhangskomponenten. Doch
die eigentliche Hauptfigur ist $\lambda_2$, die **algebraische Konnektivität**.
Ist sie klein, hat der Graph eine Engstelle — er lässt sich durch das Entfernen
weniger Kanten in zwei große Teile zerschneiden. Ist sie groß, ist der Graph
robust verwoben.

Das ist der diskrete Vetter einer Idee aus der Geometrie: Die tiefste
Schwingungsfrequenz einer Trommel spiegelt ihre Form wider. Hier ist die
„Trommel" ein Netzwerk, und ihre Frequenzen sind Eigenwerte.

## Warum es zur Zahlentheorie passt

In [der vorigen Notiz](/de/posts/primzahlluecken/) schrieb ich über
Primzahllücken. Die Verbindung ist eher geistiger als wörtlicher Natur: In
beiden Fällen verdichtet ein einziges Spektrum — von Primzahlen, von einer
Matrix — eine enorme Menge an Struktur in eine Liste von Zahlen. Ein großer Teil
meiner Arbeit besteht darin, solche Listen lesen zu lernen.

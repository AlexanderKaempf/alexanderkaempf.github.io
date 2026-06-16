---
title: QIT 7 - Quantum Algorithms
published_date: 2026-06-12 09:00:00 +0000
tags:
  - quantum-information
  - quantum-algorithms
  - exposition
data:
  lang: en
  nav: posts
  alt_url: /de/posts/quantenalgorithmen/
---

The circuit model of the previous post says what a quantum computer is without giving one reason to build it. A circuit is a word over a gate alphabet; the question this post answers is which words compute something the classical world cannot, or cannot do as fast. Three algorithms map the terrain, and they escalate deliberately: a contrived problem solved with certainty in one query, a broad problem solved with a quadratic edge, and a problem of real consequence solved exponentially faster than anything known classically. All three run on the same engine. Each prepares a superposition over inputs, arranges for the unwanted answers to cancel and the wanted ones to reinforce, and reads out what survives.

## The oracle, and the kickback

Two of the three algorithms are stated against a black box, an idealization that isolates the quantum mechanism from the messier question of how a function is built. A Boolean function $f : \lbrace 0,1\rbrace^n \to \lbrace 0,1\rbrace$ is supplied as a unitary.

**Definition 1.** *The oracle for $f$ is the unitary $U_f$ defined on the computational basis by $U_f |x\rangle |y\rangle = |x\rangle |y \oplus f(x)\rangle$, with $x$ the $n$-qubit query register and $y$ a single target qubit.*

The construction reversibly writes $f(x)$ into the target by addition modulo two, which is the only way a unitary is permitted to evaluate a function. The reason it is useful is a small computation that recurs throughout the subject.

**Proposition 2 (phase kickback).** *With the target prepared in $|-\rangle = \tfrac{1}{\sqrt 2}(|0\rangle - |1\rangle)$,*

$$
U_f |x\rangle |-\rangle = (-1)^{f(x)} |x\rangle |-\rangle .
$$

*Proof.* If $f(x) = 0$ the target is untouched. If $f(x) = 1$ the oracle swaps the target's components, sending $|-\rangle$ to $\tfrac{1}{\sqrt 2}(|1\rangle - |0\rangle) = -|-\rangle$. Either way the result is $(-1)^{f(x)}|x\rangle|-\rangle$. $\square$

The value of $f$ has moved out of the target register and into a phase on the query register, where interference can act on it. The target, unchanged, can be ignored from here. This relocation of information into relative phase is the recurring move; every speedup below is a way of making those phases cancel or conspire.

## Deutsch–Jozsa, certainty from one query

The first problem is artificial, chosen so the mechanism shows with nothing else in the way. A function $f$ on $n$ bits is promised to be either *constant* (the same value everywhere) or *balanced* (equal to $0$ on exactly half the inputs and $1$ on the other half). Decide which.

Classically the promise is unforgiving. Reading $2^{n-1}$ inputs that all agree leaves open whether the next disagrees, so a deterministic algorithm may need $2^{n-1} + 1$ queries in the worst case. One quantum query settles it.

**Proposition 3.** *Apply $H^{\otimes n}$ to $|0\rangle^{\otimes n}$, query the oracle with the target in $|-\rangle$, apply $H^{\otimes n}$ again, and measure. The amplitude of the outcome $|0\rangle^{\otimes n}$ is*

$$
\frac{1}{2^n} \sum_{x \in \lbrace 0,1\rbrace^n} (-1)^{f(x)} ,
$$

*which equals $\pm 1$ if $f$ is constant and $0$ if $f$ is balanced.*

*Proof.* The first $H^{\otimes n}$ produces the uniform superposition $2^{-n/2}\sum_x |x\rangle$. The oracle, by kickback, attaches the sign $(-1)^{f(x)}$ to each term. The second $H^{\otimes n}$ sends $|x\rangle$ to $2^{-n/2}\sum_z (-1)^{x \cdot z}|z\rangle$, so the amplitude of $|z\rangle = |0\rangle^{\otimes n}$ collects every term with no sign from the inner product, leaving $2^{-n}\sum_x (-1)^{f(x)}$. For constant $f$ the sum is $\pm 2^n$, giving amplitude $\pm 1$ and a certain all-zero outcome. For balanced $f$ the signs cancel exactly and the amplitude is $0$, so the all-zero outcome never occurs. Observing the register answers the question. $\square$

A direct simulation confirms the two extremes: a constant function returns the all-zero string with probability one, a balanced function with probability zero. The algorithm is a parlour trick — the promise is too strong to arise naturally — but it displays the whole method in miniature. Superpose, kick the function values into phases, and let a Hadamard fold the phases into a single observable bit.

## Grover, a quadratic edge on an honest problem

The next problem is one anyone has: find the needle. Among $N = 2^n$ items, one marked element $w$ satisfies a condition checkable by an oracle $O_w = I - 2|w\rangle\langle w|$, which flips the sign of $|w\rangle$ alone. With no structure to exploit, a classical search reads about $N/2$ items on average. Grover's algorithm needs about $\sqrt N$.

The mechanism is best seen as a rotation. Write $|s\rangle = N^{-1/2}\sum_x |x\rangle$ for the uniform superposition, and consider the plane it spans together with $|w\rangle$.

**Proposition 4.** *Let $G = (2|s\rangle\langle s| - I)\thinspace O_w$ be the Grover iterate, and define $\theta$ by $\sin\theta = N^{-1/2}$. Then $G$ acts on the plane $\mathrm{span}\lbrace |w\rangle, |s\rangle \rbrace$ as a rotation by $2\theta$ toward $|w\rangle$.*

*Proof.* The oracle $O_w$ reflects a vector across the hyperplane orthogonal to $|w\rangle$; within the plane this is a reflection about the axis perpendicular to $|w\rangle$. The diffusion operator $2|s\rangle\langle s| - I$ reflects about $|s\rangle$. A product of two reflections, about axes meeting at angle $\theta$, is a rotation by $2\theta$. The starting vector $|s\rangle$ sits at angle $\theta$ from the orthogonal axis, since $\langle w | s \rangle = N^{-1/2} = \sin\theta$. $\square$

Each iterate turns the state $2\theta$ closer to $|w\rangle$, so after $k$ iterations the amplitude on $|w\rangle$ is $\sin\bigl((2k+1)\theta\bigr)$. For large $N$ the angle $\theta \approx N^{-1/2}$ is small, and the amplitude reaches one when $(2k+1)\theta \approx \pi/2$, that is after about $\tfrac{\pi}{4}\sqrt N$ iterations. Simulating $N = 16$ makes the arithmetic concrete: the predicted optimum is $\lfloor \tfrac{\pi}{4}\sqrt{16} \rfloor = 3$ iterations, and the success probability climbs $0.06, 0.47, 0.91, 0.96$ over the first three steps before overshooting and falling back. The overshoot is worth dwelling on — running Grover too long *un*-rotates the state and the success probability drops, which is the clearest sign that this is rotation, not accumulation.

The square-root saving is real but bounded, and the bound is a theorem rather than a failure of imagination.

**Theorem 5 (optimality, Bennett–Bernstein–Brassard–Vazirani).** *Any quantum algorithm that finds a marked item among $N$ using only oracle access requires $\Omega(\sqrt N)$ queries.*

Unstructured search admits no exponential quantum speedup. The quadratic factor is the whole prize, and it is the most broadly applicable quantum advantage known — anything phrased as a search inherits it — precisely because it assumes no structure. The exponential speedups must come from problems that have structure to exploit.

## Shor, where the structure pays

Factoring has structure, and Shor's algorithm finds it. The target is to split a composite $N$; the route runs through a problem of pure number theory.

**Definition 6.** *For $a$ coprime to $N$, the order of $a$ is the least $r \ge 1$ with $a^r \equiv 1 \pmod N$.*

The reduction from factoring to order finding is classical and elementary. Choose $a$ at random; if it shares a factor with $N$, Euclid is already done. Otherwise compute the order $r$. When $r$ is even and $a^{r/2} \not\equiv -1 \pmod N$, the difference of squares $a^r - 1 = (a^{r/2}-1)(a^{r/2}+1)$ is divisible by $N$ while neither factor is, so $\gcd(a^{r/2} - 1, N)$ is a nontrivial divisor. For odd composite $N$ with at least two distinct prime factors, a random $a$ meets the two conditions with probability at least one half, so a handful of attempts suffices.

The arithmetic is vivid at $N = 15$. Taking $a = 7$ gives the order $r = 4$, since $7^2 = 49 \equiv 4$ and $7^4 \equiv 1$. Then $a^{r/2} = 7^2 \equiv 4$, and $\gcd(4 - 1, 15) = 3$ while $\gcd(4 + 1, 15) = 5$. The factors fall out of one order computation.

Everything classical except that order. Computing $r$ is exactly as hard as factoring by any known classical method, and it is the step the quantum computer performs.

**Theorem 7 (order finding).** *The order of $a$ modulo $N$ can be computed by a quantum circuit using $O(\mathrm{poly}(\log N))$ gates, with success probability bounded below by a constant.*

The circuit prepares a superposition over exponents, applies the modular-exponentiation map $|x\rangle \mapsto |x\rangle |a^x \bmod N\rangle$, and measures the second register, collapsing the first to a superposition supported on an arithmetic progression of period $r$. A quantum Fourier transform over $\mathbb Z_{2^m}$ converts that period into a measurable frequency: the measurement returns an integer close to $2^m s / r$ for some $s$, from which the continued-fraction expansion recovers $r$ when $s$ and $r$ are coprime. The Fourier transform is doing here exactly what kickback did for Deutsch–Jozsa — turning a global property of the function, its period, into something a single measurement can see.

The consequence is the reason a cryptographer reads this far. The security of RSA rests on factoring being intractable, and Shor's algorithm runs in time polynomial in $\log N$ against the best known classical methods, which are sub-exponential. A scalable quantum computer would break the assumption outright. That collision between what the algorithm promises and what current cryptography assumes is the subject the applications thread builds toward.

The three algorithms together draw the map. Deutsch–Jozsa shows the mechanism in its purest and least useful form; Grover shows a modest speedup that asks nothing of the problem and so applies to almost everything; Shor shows that when a problem carries the right hidden periodicity, the gain is dramatic. What every one of them spends is interference, and what makes interference possible at scale is entanglement, which the next post isolates and studies as a resource in its own right.

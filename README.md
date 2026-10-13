<!-- solve-problem:40f8172d-4f2c-4094-8a4a-3f4c1c226eb7 -->
# Optimal-seed generators for general branching programs

A [Tangle Solve problem](https://solve.tangle.tools/problems/40f8172d-4f2c-4094-8a4a-3f4c1c226eb7).

## What is the problem?

Construct an explicit pseudorandom generator (PRG) with seed length O(log n) that ε-fools width-w, length-n branching programs with inverse-polynomial error ε = 1/poly(n), for w = poly(n).

Formal setting: a branching program (BP) B is a layered DAG with n layers, each layer containing at most w vertices (width w); each vertex is labeled with a Boolean function of the current input bit(s) determining which of two edges to the next layer; B: {0,1}^n → {0,1} outputs whether an accepting state is reached. A PRG G: {0,1}^s → {0,1}^n fools B if |Pr[B(G(U_s)) = 1] − Pr[B(U_n)) = 1]| ≤ ε. Goal: s = O(log n) with ε = n^(−Ω(1)) (often one asks s = O(log n) + O(log(1/ε)) or s = O(log(n/ε)) — note the exact slack convention is an ambiguity in the informal statement). "Explicit" means computable in poly(n) time (or at least poly(log n) space, matching the derandomization goal). Note "read-once" here means each input bit is read once (the standard general branching-program model), not the more restrictive "oblivious, permutation branching program" (boolean matrix/read-once *matrix* programs) for which PRGs are known. The problem asks for the fully general ordered branching program.

## Why does it matter?

Such a generator would give BPL ⊆ L with a black-box, reusable derandomization: any randomized log-space algorithm could be simulated deterministically in log space by enumerating all 2^(O(log n)) = poly(n) seeds. Unlike a mere equality proof BPL = L, an explicit PRG yields randomness-efficient simulation for arbitrary users of randomness (streaming algorithms with small state, reproducible randomized protocols). It is the natural next milestone after Nisan's generator, and progress here is the standard proving ground for pseudorandomness techniques (pseudorandom restrictions, XOR lemmas, Fourier analysis of branching programs).

## What's already known?

- Nisan (1992): seed O(log² n) for poly-width, poly-length BPs, via a generator built from pairwise-independent hash functions. This underlies Saks–Zhou's BPL ⊆ SPACE(log^1.5 n).
- Impagliazzo–Nisan–Wigderson: derandomization of general BPs from any one-way function (non-black-box).
- For restricted classes, O(log n)-seed PRGs exist: read-once *oblivious* branching programs — Nisan's PRG analysis by Braverman et al., then Chattopadhyay–Hatami–Reingold–Tal (2018), Forbes–Kelley (poly-width regular BPs), and Culvehouse et al./Meka–Reingold–Tal (2025-era results for low-width and general oblivious models); Forbes–Kelley gave O(log n) seeds for regular BPs, with later extensions. These do NOT cover general width-poly(n) branching programs.
- Bogdanov–Papakonstantinou–Wan and later work: polynomial-factor seed length for *general* (non-oblivious) BPs but with seed n^(1/2+o(1)) or similar — far from logarithmic.
- (verify exact current best seed for general width-poly BPs; it remains poly(n) or O(log² n) territory.)

The general case resists all known structural tools because the transition functions at each layer are arbitrary, destroying the Fourier/matrix-analysis techniques that exploit the oblivious/regular structure.

## What would count as solving it?

An explicit family of generators G_n: {0,1}^(c log n) → {0,1}^n, computable in poly(n) time, with a proof that for every width-poly(n), length-n branching program B, |Pr[B(G(U))=1] − Pr[B(U)=1]| ≤ n^(−c′). Partial credit variants: (a) seed O(log n · log log n); (b) seed O(log n) for width n^(o(1)) general BPs; (c) a lower-bound/barrier result showing O(log n) is impossible for some explicit construction paradigm. A referee would demand a full proof for general (non-oblivious) branching programs — results for oblivious or regular programs do not solve this.

## How to start

1. Read Nisan (1992) first — the O(log² n) generator and its analysis are the baseline; understand exactly where the log n slack arises (the recursion needs log n levels of hash-based hybrid arguments).
2. Read the Chattopadhyay–Hatami–Reingold–Tal pseudorandom-restriction framework and Forbes–Kelley to see precisely which structural property of oblivious/regular BPs is used — this identifies what must be replaced for general BPs.
3. Candidate approaches: try to "oblivianize" a general BP (with small error in PRG advantage) — analyze how much a general BP can behave differently from an oblivious one on pseudorandom input; or try to generalize Forbes–Kelley's Fourier mass bounds to non-regular programs. Main obstacle: arbitrary layer functions can correlate with the input prefix, so no inductive Fourier-mass statement is currently known to hold.

## Key references

- Nisan, N. "RL is contained in SC," STOC 1992. — Constructs the O(log² n)-seed generator for space-bounded computation via hash-based recursion; the standing best general construction.
- Chattopadhyay, E., Hatami, H., Reingold, O., Tal, A. "Improved pseudorandomness for unordered branching programs through nested commutators," STOC 2018, arXiv:1801.04990. — Develops pseudorandom restriction/matrix-analysis machinery fooling read-once oblivious BPs with near-optimal seed.
- Forbes, M., Kelley, Z. "Pseudorandomness for read-once, oblivious branching programs," FOCS 2018. — Achieves O(log n)-seed PRGs for regular read-once oblivious BPs; closest known positive result to the target.
- Impagliazzo, R., Nisan, N., Wigderson, A. "Pseudorandomness for network algorithms," STOC 1994. — Derandomizes general BPs non-black-box from any one-way function, showing why black-box PRGs are strictly harder.
- Tal (2020), "Derandomizing Space-Bounded Computation" workshop materials. — (verify) Survey-level discussion of the O(log n)-seed goal and barriers; not a standard citable paper.

## Warnings

- The census record's phrasing "read-once branching programs" is ambiguous: in this literature "read-once branching program" without qualifiers means each bit read once but states/layers arbitrary (the general model, still open), whereas "read-once oblivious" (or ROBP) is the solved restricted model. Do not mistakenly target the oblivious version.
- Shalunov (2024) appears in the census record but could not be verified as a real citation — treat it as (verify) and do not rely on it.
- Merely proving BPL = L does not solve this; the deliverable is an explicit black-box generator.
- Common dead end: trying to apply Fourier/matrix-norm arguments directly; they fail on non-oblivious programs because layer functions depend adaptively on the input prefix.

---
*`ALGO-000052` · unverified*

## Acceptance guidance

An explicit family of generators G_n: {0,1}^(c log n) → {0,1}^n, computable in poly(n) time, with a proof that for every width-poly(n), length-n branching program B, |Pr[B(G(U))=1] − Pr[B(U)=1]| ≤ n^(−c′). Partial credit variants: (a) seed O(log n · log log n); (b) seed O(log n) for width n^(o(1)) general BPs; (c) a lower-bound/barrier result showing O(log n) is impossible for some explicit construction paradigm. A referee would demand a full proof for general (non-oblivious) branching programs — results for oblivious or regular programs do not solve this.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/40f8172d-4f2c-4094-8a4a-3f4c1c226eb7?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000052.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000052)

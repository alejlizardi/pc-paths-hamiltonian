# Long Properly Colored Paths in the Union of Two Hamiltonian Paths

Independent research note (May 2026) on an extremal question about
two-edge-colored multigraphs whose color classes are Hamiltonian paths. The
paper gives a reduction lemma connecting the problem to a step in the
Norin–Steiner–Thomassé–Wollan approach to the Lovász vertex-transitive path
problem, proves a tight extremal value for a specific block-reversal family,
and refutes the natural half-bound conjecture with an explicit counterexample
at n = 18.

The full writeup is in [paper.pdf](paper.pdf).

> **Update (June 2026).** The main open question of this note (whether
> ρ(σ) ≥ cn for some absolute constant c > 0) is answered in the negative by a
> follow-up paper in this repository: ρ_min(n) = O(√n). See
> [the sequel below](#follow-up-paper-june-2026-properly-colored-paths-can-have-length-on).

## Background

For a permutation σ ∈ S_n, let H_σ be the two-edge-colored multigraph on
{0, 1, ..., n−1} whose blue edges form the natural-order Hamiltonian path and
whose red edges form the Hamiltonian path induced by the value order of σ. A
properly colored (PC) path is a simple path whose consecutive edges alternate
in color. Let ρ(σ) be the maximum number of vertices on such a path. The
parameter ρ_min(n) := min over σ of ρ(σ) controls a matching-edge invariant
appearing in work of Norin, Steiner, Thomassé, and Wollan (arXiv:2505.08634,
2025) on Lovász's vertex-transitive path conjecture: any linear lower bound
ρ_min(n) ≥ cn with c > 0 would push the best bound on that problem from
Ω(n^{9/14}) to Ω(n^{2/3}).

## Key results

- **Conversion Lemma.** A PC path on k vertices in H_σ yields a simple path
  in the associated graph G(n, σ) using exactly k matching edges. The
  construction is explicit, so any uniform lower bound on ρ transfers to the
  matching-edge parameter f(n).

- **Tight family.** The block-reversal permutation σ_m ∈ S_{4m+3} (composition
  (2, 3, 1, 3, 1, ..., 3, 1, 3, 2) with each block reversed) achieves
  ρ(σ_m) = 2m+3 = (n+3)/2. An explicit PC path attaining this value is given,
  and a matching upper bound follows from a local block calculus.

- **Refutation of the half-bound conjecture.** The conjecture
  ρ(σ) ≥ ⌈(n+3)/2⌉, supported by exhaustive enumeration through n ≤ 14, is
  false. At n = 18, the permutation
  σ* = (1, 0, 4, 3, 2, 5, 8, 7, 6, 11, 10, 9, 12, 15, 14, 13, 17, 16)
  has ρ = 10 < 11. It is obtained by deleting one interior singleton of σ_4.

- **Exhaustive computational verification.** ρ_min(n) is computed for every
  n ≤ 14. The n = 14 run covers all 8.72 × 10^{10} permutations of S_14 and
  finds no violation of the half-bound at that size.

- **Stacking and a conditional asymptotic bound.** A stackable variant of the
  σ* construction gives an infinite family with slope Δρ/Δn = 5/11 at every
  recorded k (verified for k ≤ 4). If the slope continues asymptotically, the
  true constant c_* := liminf_n ρ_min(n)/n satisfies c_* ≤ 5/11. A separate
  module search up to size 28 produces slope 10/28 at k = 1.

## File structure

```
.
├── paper.pdf                            first paper (May 2026), full writeup
├── cstar_paper.pdf                      follow-up paper (June 2026), see below
├── README.md                            this file
├── LICENSE                              MIT
├── src/                                 verifiers for the first paper
│   ├── alt_verify_resumable.cpp         exhaustive S_n verifier (paper §3.4)
│   ├── verify_n18_counterexample.cpp    independent check of ρ(σ*) = 10 (§4.1)
│   ├── deletion_test.cpp                singleton-deletion experiment (§4.2)
│   ├── module_search.cpp                module search behind the stacking family (§4.3)
│   ├── block_deficit_census.cpp         block-by-block deficit statistics
│   └── landscape_analysis.cpp           adjacent-transposition local-minima census (§6)
└── paper2/
    └── artifact/                        reproducibility artifact for the follow-up paper
```

## Build and run

All sources are C++17 with OpenMP. Tested with `g++` 12+ on Linux and the
MinGW-w64 toolchain on Windows. Compile any source with:

```
g++ -O3 -march=native -std=c++17 -fopenmp src/<file>.cpp -o <file>
```

Sample invocations:

```
# Verify the half-bound conjecture exhaustively at n=10 (seconds)
./alt_verify_resumable 10

# Same at n=14 (about 5 days on 16 OpenMP threads; supports --resume)
./alt_verify_resumable 14 --resume

# Independent check of the n=18 counterexample (seconds)
./verify_n18_counterexample

# Adjacent-transposition local-minima census at n=11
./landscape_analysis 11

# Block-composition deficit statistics at n=12
./block_deficit_census 12
```

`alt_verify_resumable` writes a per-chunk log (`n<n>_chunks.log`) and can
restart from where it left off via `--resume`; running it with no arguments
prints the full flag list.

## Open questions

- Is c_* > 0? Equivalently, is there an absolute constant c > 0 such that
  ρ(σ) ≥ cn − O(1) for every σ?
  **Answered in the negative by the follow-up paper below: c_* = 0.**
- Does ρ(σ) ≥ ρ(τ_σ) hold for every σ, where τ_σ is the block quotient
  permutation? (Verified empirically through n ≤ 11.)
- Stacking conjecture: for M = (3, 1, 3, 3, 1), does the family σ^{(k)}
  satisfy ρ(σ^{(k)}) = 5k + 5 for every k ≥ 0? A positive answer gives
  c_* ≤ 5/11.

---

# Follow-up paper (June 2026): Properly colored paths can have length O(√n)

Sequel (June 2026) answering the main open question of the first paper, in
the negative: there is no constant c > 0 such that the union of two
Hamiltonian paths must contain a properly colored path on cn vertices. The
proof is by an explicit construction, with a second independent construction
in an appendix and a fully reproducible computational artifact.

The full writeup is in [cstar_paper.pdf](cstar_paper.pdf).

## Background

The first paper asked: does there exist c > 0 such that every two-edge-colored
multigraph that is the union of two Hamiltonian paths on the same vertex set
contains a properly colored simple path on at least cn vertices? A positive
answer would have transferred, through the Conversion Lemma, to a linear lower
bound on the matching-edge parameter f(n) in the Norin–Steiner–Thomassé–Wollan
approach to Lovász's 1969 problem on Hamiltonian paths in vertex-transitive
graphs.

**Scope.** The negative answer closes only that particular route (the
properly-colored relaxation, proposed in the first paper), not the underlying
matching-edge question: whether f(n) is linear remains open. See the paper's
Section 1.2.

## Key results

1. **Main theorem.** ρ_min(n) = O(√n); explicitly, ρ_min(n) ≤ 8√n + 20 for
   all n ≥ 100. Consequently ρ_min(n)/n → 0, so no such constant c exists.
   This resolves the first open question of the May 2026 note (c_* = 0).

2. **Inflation calculus and transit numbers.** An inflation (substitution)
   operation on permutations, together with a per-permutation invariant, the
   transit number M(π), which bounds how many vertices a PC path can collect
   while passing through an inflated copy of π. The core lemma is a complete
   classification of the admissible visits of the gadget family
   T_b = (0, b−1, 2, 3, ..., b−2, 1): for every even b ≥ 6 there are exactly
   five admissible visits, of sizes 1, 2, 2, 3, 3, so M(T_b) = 3 independent
   of b. The classification turns on a parity obstruction in a long run of
   doubled edges.

3. **Two-sided bounds for the witness family.** For σ_{a,b} on n = ab
   vertices (a consecutive copies of T_b), 2a + 2b − 7 ≤ ρ(σ_{a,b}) ≤ 4a + 2b,
   so ρ(σ_{a,b}) = Θ(a + b); with a ≍ b ≍ √n the family realizes the order √n
   exactly. (Exact machine computation gives ρ(σ_{a,b}) = 2a + 2b − 4 at every
   tested point up to n = 720; this formula is stated as data only and used
   nowhere in the proofs.)

4. **A second, independent construction.** Appendix A proves the same O(√n)
   bound via trap chains: compositions of reversed blocks in which large
   reversed blocks of odd size, flanked by singletons, are traps that no
   properly colored path can cross. The appendix is independent of the main
   text, and vice versa.

5. **Reproducibility.** Every computational claim in the paper maps to a
   script and log in [paper2/artifact/](paper2/artifact/) via the
   claim-to-log table in the artifact's README, including a ~100-line minimal
   independent verifier written directly from the definitions (no shared
   code) and a SHA-256 manifest covering all 28 code and log files.

## File structure

```
paper2/
└── artifact/
    ├── README.md            claim-to-log map + reproduction commands
    ├── MANIFEST.sha256      checksums of all code and logs
    ├── pc.py                reference solvers for ρ(σ)
    ├── family.py            memoized solvers; the gadget family T_b
    ├── inflate.py           the inflation σ = τ[π_1..π_a]
    ├── verify_M.py          minimal independent verifier (classification + M values)
    ├── verify_path.py       edge-by-edge check of the explicit lower-bound path
    ├── (7 more .py scripts)  per-claim checks; see the claim-to-log map
    ├── cpp/rho_tool.cpp     independent C++ implementation (exhaustive n ≤ 12)
    └── logs/                all referenced logs (exh7.txt–exh12.txt, *.log)
```

## Build and run

The Python scripts have no external dependencies and run with plain
`python script.py` (Python 3.14 used for the logs; ~5 minutes wall-clock to
regenerate all Python logs). The C++ tool compiles with:

```
g++ -O2 -fopenmp paper2/artifact/cpp/rho_tool.cpp -o rho_tool
```

The artifact's own [README](paper2/artifact/README.md) has the full
claim-to-log map (paper Section 7) and per-script reproduction commands.
Verify file integrity with:

```
cd paper2/artifact && sha256sum -c MANIFEST.sha256
```

## Open questions

- No superconstant lower bound on ρ_min(n) is proved; even ρ_min(n) → ∞
  appears to be open.
- Conjecture (8.1 in the paper): ρ_min(n) = Θ(√n). Only the upper bound is
  proved. Exhaustively for n ≤ 12, ρ(σ) ≥ b(σ) (the number of ±1-blocks)
  holds for every σ, but the constant 1 fails asymptotically on σ_{a,b}
  (e.g. ρ(σ_{20,12}) = 60 < 80 = b(σ_{20,12}) at n = 240). The sharp form:
  does ρ(σ) ≥ c·b(σ) hold for some c ≤ 1/2? Any such c settles the
  conjecture.
- Is M(π) ≥ 3 for every π with |π| ≥ 4? (Exhaustively true for 4 ≤ b ≤ 9.)
- The analogue for the union of two Hamiltonian cycles, and the Lovász-side
  question: for the graphs G(n, σ) built from σ_{a,b} or from trap chains, is
  the maximum number of matching edges on a path still Θ(n)? The
  properly-colored relaxation is now known to be lossy; quantifying the loss
  may indicate whether f(n) is linear after all.

## Notes

The verification scripts in `src/` (first paper) were written with assistance
from Claude Code. The proofs and computations in the follow-up paper were
developed with the assistance of Claude (Fable 5, Anthropic); see the paper's
Acknowledgments and Section 7. The artifact includes a minimal independent
verifier so the central classification can be checked without trusting either
the author or the assistant.

## Contact

Alejandro Lizardi, alejlizardi05@gmail.com
                                                                    
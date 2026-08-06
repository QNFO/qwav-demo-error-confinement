# Error Confinement — Bruhat-Tits Error Suppression Demo (Artifact A1)

**Status:** ✅ LIVE — deployed 2026-08-04 | **Enhanced:** 2026-08-06
**URL:** https://qnfo.github.io/qwav-demo-error-confinement/

## What This Shows

Physical errors (qubit flips, decoherence events) at the leaves of an ultrametric
(Bruhat-Tits) tree are suppressed exponentially as they propagate upward through
majority voting. Each parent node corrects errors by requiring more than half its
children to be in error before it becomes erroneous. The result: a physical error
rate of 1% can yield a logical error rate of 10⁻³² — a 10³⁰× suppression factor.

## The Math

```
LER = f^d(ε)

where  f(ε) = Σ_{j=⌊p/2⌋+1}^{p} C(p,j) · ε^j · (1-ε)^{p-j}
```

| Symbol | Meaning |
|:-------|:--------|
| ε | Physical error rate (probability a leaf qubit is bad) |
| p | Branching factor (prime; tree is p-ary) |
| d | Tree depth |
| f(ε) | One-level majority-vote error propagation |
| C(p,j) | Binomial coefficient |
| LER | Logical error rate (error probability at root after d levels of correction) |

The `f(ε)` function computes the probability that a parent node becomes bad given
each child is independently bad with probability ε. The condition "more than
⌊p/2⌋ children bad" is the majority vote. Applying f repeatedly d times (once
per tree level) gives the final logical error rate.

## How to Use

1. **Start here:** Drag the Physical Error Rate slider to see how the logical
   error rate prediction (cyan number) changes in real time. At ε = 1%, the
   predicted LER is vanishingly small.
   
2. **Try changing the prime (p):** Click p = 2, 3, or 5. Higher p means more
   children per node and stronger error suppression. The tree visualization
   updates to show the new structure.
   
3. **Run a Monte Carlo simulation:** Click ▶ Run Simulation to actually inject
   random errors at the leaves and watch them get corrected via majority voting.
   The amber number shows the simulated logical error rate; the amber dot on the
   chart shows where it lands relative to the theoretical curve.

4. **Watch the collapse animation:** Click ▶ Run Collapse to see how errors are
   suppressed level by level as we move up the tree.

## Parameters

| Parameter | Symbol | Range | Default | Description |
|:----------|:-------|:------|:--------|:------------|
| Physical Error Rate | ε | 0% – 100% | 1% | Probability each leaf has an error |
| Prime | p | 2, 3, 5 | 3 | Branching factor; majority needs ⌊p/2⌋+1 children |
| Tree Depth | d | 2 – 7 | 5 | Number of correction layers |
| Seed | — | 0 – 2³¹−1 | 42 | PRNG seed for deterministic Monte Carlo |

## Interpreting the Output

### Tree Visualization
- **Purple nodes:** Internal tree nodes (correction layers)
- **Cyan nodes:** Leaf nodes (physical qubits where errors originate)
- **Gold node:** Root (logical qubit — the final error rate)
- Tree structure reflects the chosen p (branching) and d (depth)

### Readouts
| Readout | Meaning |
|:--------|:--------|
| **Logical Error Rate (theoretical)** | Predicted LER from the recursive binomial formula |
| **Logical Error Rate (Monte Carlo)** | Simulated LER from injecting random errors and propagating via majority vote |
| **Suppression Factor** | ε / LER — how many times the error rate is reduced |
| **MC Trials** | Number of Monte Carlo trials (10,000) |

## Reproducibility

- **Seed:** 42 (fixed for deterministic output; adjustable via seed input)
- **PRNG:** mulberry32 (32-bit seeded generator)
- **Algorithm:** Binomial error propagation (theoretical) + Monte Carlo with seeded RNG (simulated)
- **Monte Carlo trials:** 10,000 per simulation
- **Deterministic:** Yes — same seed + same parameters = identical output
- **Math verification:** `verifyMath()` in browser console runs 6 checks (edge cases, monotonicity, analytical match, determinism)

## Limitations

- This is a pedagogical model; it does not simulate actual quantum hardware with
  realistic noise models (depolarizing, amplitude damping, etc.)
- The tree is symmetric and fixed; real error-correcting codes may have irregular
  connectivity and varying node degrees
- Monte Carlo uses 10,000 trials; statistical noise is ~1/√N ≈ 1%
- The model assumes independent errors; correlated errors (bursts, cosmic rays)
  would degrade performance
- Only corrects bit-flip errors; phase-flip and general errors require separate
  or concatenated codes

## Source

- **Strategy:** QNFO/QWAV strategy/3.0.md — Tier 1 Artifact A1
- **Publication:** Computational Validation (Tier 0), Symmetric Extension (Tier 1)
- **Build:** single-file HTML, canvas rendering, zero external dependencies
- **Repository:** https://github.com/QNFO/qwav-demo-error-confinement
- **Skill:** qwav-demo-kit v1.0 (design, build, test, deploy, document pipeline)

## Testing

- **Math verification:** `verifyMath()` or Ctrl+V in browser — 6/6 checks pass
- **Chrome automation:** `python scripts/test-demo.py --url <live-url>`
- **Last test run:** 2026-08-06 — 6/6 math verification passed, zero console errors
- **Features:** seeded RNG (mulberry32), guided first-run tour, formula display,
  keyboard shortcuts (Ctrl+R = run simulation, Ctrl+V = verify math)

---

*Generated with DeepChat | All page content is AI-generated and for reference only.*

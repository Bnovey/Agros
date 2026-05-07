# Modal Cost Estimate — IG Paper Analysis

## Core cost driver

Integrated gradients requires **M forward+backward passes per complex**. M=50 is the planned default. Each pass costs roughly the same as one Boltz-2 affinity inference (trunk + AffinityModule, no diffusion). No structure generation is needed — only the trunk and affinity head run.

**Estimated time per forward+backward pass on A100: ~8–15s**
(trunk-only path; N ≈ 500 tokens)

---

## Per-complex estimates

| Step | Passes | Time (A100) | Cost (A100 @ $3.04/hr) |
|---|---|---|---|
| IG (M=50) | 50 | ~7–12 min | ~$0.35–0.60 |
| Alanine scan validation (15-residue peptide) | 16 | ~2–4 min | ~$0.10–0.20 |
| **Total per complex** | **~66** | **~9–16 min** | **~$0.45–0.80** |

---

## Full paper scope

Rough estimate: 4–5 targets × 5 peptides = **20–25 complexes** for the main analysis.

| Scenario | A100 time | Modal cost |
|---|---|---|
| One clean paper run (25 complexes) | ~4–7 hrs | ~$12–22 |
| Full paper with 3–4 experimental iterations | ~15–25 hrs | ~$45–75 |
| **Comfortable upper bound** | — | **~$100** |

Well inside the $300–500 paper budget, with room for re-runs and figure generation.

---

## Gotchas

**Batch per Modal call — don't one-shot per complex.**
Cold starts are 10–30s for Boltz-2. 25 separate Modal calls = 5–12 min of wasted cold-start overhead. Run all complexes for a target in one app invocation.

**Memory is fine.**
M=50 gradient tensors at N=500 tokens (float16) ≈ 19MB per complex — no issue on A100 (40GB).

**Alanine scan: parallelize across residues.**
Each single-Ala mutant is independent. Run them in parallel within one Modal call rather than sequentially — cuts wall time from ~4 min to ~30s.

**Baseline sweeps double the IG cost.**
If comparing zero-embedding vs. masked-token baseline across all complexes, that's 2× the IG compute. Run the baseline comparison on a 3–5 complex subset first before committing to full scale.

---

## Summary

The method is computationally modest. The paper's bottleneck is analysis and writing, not compute.

$$\text{Expected total Modal spend} \approx \$50\text{–}\$100$$

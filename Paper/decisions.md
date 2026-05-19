# Paper Decision Log

---

## 2026-05-06 — Per-residue affinity analysis method: integrated gradients

**What:** Use integrated gradients (IG) to generate per-residue binding affinity heatmaps from Boltz-2's AffinityModule output.

**Why:** IG is more principled than vanilla gradient saliency (avoids local linearization artifacts), requires no model modification, and produces a directly interpretable per-residue scalar that maps to "contribution to binding affinity." Output is publishable and validatable against in silico alanine scanning and/or experimental mutagenesis data.

Alternatives considered:
- Attention maps — tell you where the model looks, not what drives the affinity score. Not directly interpretable as affinity contribution.
- Vanilla gradient saliency — fast but noisy; local linearization misses non-linear contributions.
- In silico alanine scanning — most biologically interpretable but slower (N forward passes). Use as validation, not primary method.

**Who:** Bowman

---

## 2026-05-06 — Target scope: ag + pharma comparison

**What:** Include both agricultural targets (primary) and pharma targets (positive control). Target count: 2 ag + 1–2 pharma.

**Why:** Pharma targets serve one purpose — positive control. Pick 1–2 with rich published mutagenesis data, run IG, confirm it identifies known binding residues. This answers the reviewer question "how do you know IG is working at all?" before presenting the novel ag results.

The paper's central result is a comparison: on pharma targets (in-distribution for Boltz-2), IG identifies known binding residues at correlation r=X; on ag targets (out-of-distribution), IG still identifies resistance-mapped interface residues at r=Y. Whether X ≈ Y or X >> Y, either outcome is publishable.

Dropping pharma entirely makes the paper harder to defend — no positive control means reviewers can't assess whether the method is functioning or whether any signal in the ag results is real.

Alternatives considered:
- Ag-only — novel, but no positive control. Reviewers will push back.
- Full comparative analysis across many pharma targets — stronger but out of scope for a preprint; adds weeks.

**Scope constraint:** Pharma is the validation section. Ag targets are the paper. Don't let pharma target count exceed 2.

**Open question:** Do we have a directional hypothesis about whether Boltz-2 generalizes well to ag targets? If yes, the paper argues it does. If genuinely uncertain, frame as exploratory. Framing affects the abstract and introduction.

**Who:** Bowman

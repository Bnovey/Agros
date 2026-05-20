# Pivot Playbook

YC will ask "what if this doesn't work?" Have a specific, pre-thought answer for each failure mode. "We'll figure it out" is not an answer. Showing you've thought about failure modes deeply is a signal of founder maturity, not weakness.

---

## Failure Mode 1: First peptide candidates show no Botrytis activity in vitro

**Likelihood:** Moderate — even with IG-guided selection, in vitro hit rates for de novo peptides can be low in early rounds.

**What this means:**
- The candidates we synthesized are wrong, not necessarily the approach
- IG analysis should tell us *why* they failed — which residues the model mispredicted

**Pivot response:**
1. Run residue ablation analysis: systematically vary the positions IG said were unimportant — if those actually matter, model needs recalibration
2. Rerun BoltzGen with tighter constraints on known structural features of β-glucan synthase binding site
3. Move to a second generation of candidates informed by the failure — this is the point of the closed loop
4. If 3 rounds show no activity: reconsider whether Botrytis β-glucan synthase is the right target, or whether Boltz-2 has sufficient structural data on this specific protein

**Pivot to:** Lepidopteran nAChR — already in our pipeline, better-validated structural data, strong published mutagenesis literature for IG validation

**What to say in the interview:**
> "A negative in vitro result isn't a pivot trigger — it's data. IG tells us what the model got wrong, so the second round is faster and better-targeted. We'd need three rounds of zero activity before we question the target. At that point we have the Lepidopteran nAChR target ready to go."

---

## Failure Mode 2: Boltz-2 / BoltzGen produces structurally unsound or insoluble candidates at scale

**Likelihood:** Low-moderate — NetSolP and RDKit filters screen for this, but edge cases exist.

**Pivot response:**
1. Tighten solubility filters upstream (NetSolP threshold, charge distribution constraints)
2. Introduce ProteinMPNN redesign pass for top candidates — use inverse folding to improve stability while preserving predicted binding
3. If the generative model is fundamentally producing garbage for this target class: switch to scaffold-based design — start from known antifungal peptide backbones (AMPs from DBAASP/APD3 databases) and use the model for optimization rather than de novo generation

**What to say in the interview:**
> "We already have NetSolP and RDKit in the pipeline for this reason. If the filters still let bad candidates through at scale, we switch from de novo generation to scaffold optimization — start from known antifungal peptide backbones and let BoltzGen find improvements."

---

## Failure Mode 3: Licensing model gets no traction — agchem companies won't engage without in vivo data

**Likelihood:** High — agchem companies are conservative and the bar for licensing conversations is typically in vivo field data, not just in vitro.

**Pivot response:**
1. Don't wait for licensing — go direct to specialty crop growers for pilot trials
2. Partner with a university extension program for in-field trials (UC Cooperative Extension for California wine grapes is a natural fit)
3. Apply for EPA biopesticide registration directly — a registered product is worth more to a licensee than a promising candidate
4. If agchem licensing is slow: look at smaller specialty biopesticide distributors (Certis, BioWorks, Pro Farm Group) as first commercial partners — lower bar for engagement than BASF/Bayer

**What to say in the interview:**
> "If agchem companies won't engage at the in vitro stage, we go direct to growers through university extension trials and get the in vivo data ourselves. That's actually better long-term — it builds our proprietary dataset and our licensing leverage simultaneously."

---

## Failure Mode 4: A better-funded competitor (Bindwell or a new entrant) moves faster

**Likelihood:** Moderate — Bindwell has $6M and is already in the field.

**Pivot response:**
1. Our moat is the interpretability layer — Bindwell can screen faster, but we produce hits with mechanistic understanding. This isn't replicable without rebuilding the IG analysis from scratch.
2. If a competitor registers a similar peptide active first: our IP is the discovery method and the specific sequences, not just the activity class. File broad patent claims early.
3. Double down on targets where we have structural data advantage — Boltz-2 trained on specific ag targets gives us an edge for targets with limited PDB coverage

**What to say in the interview:**
> "Bindwell optimizes for speed and breadth. We optimize for mechanism. Even if they find an antifungal candidate before us, they won't know why it works, which means they can't iterate as fast as we can when it partially works or fails in the field."

---

## Failure Mode 5: The core technical premise is wrong — IG analysis doesn't actually predict wet lab activity

**Likelihood:** Low, but existential if true.

**What "wrong" means:** IG attributions converge to a binding region, but the top-scored candidates consistently fail in vitro, and the failure isn't explainable by the IG analysis.

**How we know before it's existential:**
- Validate IG predictions against published mutagenesis data *before* synthesizing candidates
- If IG-predicted binding residues don't match known mutagenesis data from literature, the model is wrong about mechanism even if it's right about binding score
- This validation step is the early warning system — build it into round 1

**Pivot response if the premise is truly wrong:**
1. Fall back to pure Boltz-2 binding score ranking (no IG) — still a valid selection criterion, just loses the interpretability advantage
2. Explore alternative interpretability methods (attention map analysis, already in the research paper pipeline) to replace IG
3. Worst case: reposition as a high-throughput in silico screening platform with wet lab validation — closer to Bindwell's model but with better structural prediction (Boltz-2 > PLAPT)

**What to say in the interview:**
> "We validate IG against published mutagenesis data before we synthesize a single candidate. If the attributions don't match known biology on our first target, we know the interpretability layer has a problem before we've burned any synthesis budget."

---

## The Meta-Answer for "What's Your Biggest Risk?"

Pick one and go deep — don't give a list. The right answer:

> "The biggest risk is that our first in vitro round produces no hits. That would tell us either the model isn't learning real biology for our specific targets, or we need a different generation strategy. We've mitigated this by validating IG against known mutagenesis literature before synthesizing — if the attributions don't match known biology, we know we have a problem before we've burned wet lab budget. But if we get to three rounds with no hits, we have the Lepidopteran nAChR target ready and we pivot to that immediately."

---

## What We're NOT Willing to Pivot On

- Interpretability as the core differentiator — this is the moat
- Agricultural targets — we're not pivoting to pharma/human health (different regulatory world, different customer, Bowman's network is in ag/biotech not pharma)
- Closed-loop wet lab integration — removing this would make us a pure screening company, which is exactly what Bindwell is

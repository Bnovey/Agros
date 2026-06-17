# Agrōs — AI-Designed Peptide Biopesticides

*De Novo* protein design for crop protection

June 2026

---

## The problem we address

*In silico* protein design in agriculture is constrained by its reliance on scaffolds. Today's approaches start from existing protein backbones and build binding motifs onto them — it makes the computation tractable, but it locks the search to a narrow neighborhood of known structures. Although these scaffolds are inspired by nature, this greatly reduces the search space of structures that can bind the target receptor. The result is incremental: candidates that resemble what already exists, rather than the genuinely novel binders a target may demand. For agriculture — where the goal is to replace entrenched chemical pesticides with new biological actives — that constraint is the difference between rediscovering known molecules and discovering the ones nobody has found yet.

## What we do

Agrōs designs novel peptide candidates from scratch — *de novo*, not grafted from existing scaffolds. We combine state-of-the-art protein generation and structure models with a binding-affinity scoring stack to explore the full design space against any target you specify, then hand over a ranked shortlist ready for synthesis and assay. 

We don't replace the wet lab. We feed it a larger, more diverse, pre-prioritized candidate pools. 

---

## The technology — at a high level

**1. Generation.** Structure-based generative models design thousands of candidate peptides specifically for your target, with diversity optimization so the pool covers the design space rather than clustering around a single motif.

**2. Multi-model scoring.** Every candidate is scored and filtered for binding affinity, solubility, and stability. Affinity is the primary objective, validated across multiple independent models — from deep-learning predictors to physics-based docking — so we're not betting on a single model's blind spots. We rank and filter thousands down to the most promising leads.

**3. Design rationale.** Affinity prediction models at their core have limited prediction power. That is why we extract per-residue attribution maps. This gives us insight into which amino acid positions the models identify as critical for binding. This gives a residue-level explanation of *why* a candidate is predicted to work, not just a score. 

**4. Iteration.** Attribution-guided redesign refines each cycle, constraining the residues that matter and re-exploring the rest. Additionally, when wet-lab results feed back in, models are fine tuned and generation continues to improve. 

**In Other Words:** We are iteratively creating the scaffold, meaningfully constraining the search space. Effectively pushing protein generation models towards better results. 

--- 

## Where it applies

The platform is target-agnostic. Representative agricultural targets include:

- **Insecticidal** — lepidopteran nicotinic acetylcholine receptor (nAChR), ryanodine receptor (RyR)
- **Fungicidal** — *Botrytis cinerea* β-glucan synthase
- **Other programs** — nematode, bacterial, and disease targets as partners specify

If you have a validated target and wet-lab capacity, we accelerate the front of the funnel: more and better candidates, faster, with a defensible design rationale behind each one.

---

**Contact:** 
Bowman Novey (CEO) - bnovey@berkeley.edu 
Frederick Dehmel (CTO) - dehmelf@berkeley.edu

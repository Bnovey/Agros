# Competitive Landscape

## Direct Competitors — AI-Driven Pesticide/Biopesticide Discovery

| Company | Approach | Funding | Weakness vs. Agros |
|---------|----------|---------|-------------------|
| **Bindwell** (YC W25) | Scale AI models (Foldwell, PLAPT, APPT), scan all known synthesized compounds in 6 hours, in-house IP licensing. Not peptide-specific. | $6M seed (General Catalyst, A Capital, SV Angel, Paul Graham). Pre-seed from Character Capital. | Black-box approach — no mechanistic insight into *why* candidates bind. Broad pesticide focus (not peptide-specific). No closed-loop wet lab feedback reported. |
| **Pepticom** | AI-driven peptide design platform for pharma AND agriculture partners. Reinforcement learning iterations. | Undisclosed (private, Israel-based) | Primarily pharma-focused; agriculture is secondary. No biopesticide-specific wet lab integration. |
| **Invaio Sciences** | AI for soil/plant delivery + pesticide formulation. Broad ag biotech platform. | ~$75M raised | Not focused on peptide discovery specifically. Different technical approach (delivery vs. discovery). |

---

## Indirect Competitors / Incumbents

| Company | Why they're a threat | Why we still win |
|---------|---------------------|-----------------|
| **BASF Biologics** (Becker Underwood acquisition) | Scale, distribution, regulatory expertise, existing grower relationships | Acquired rather than built — incrementalist R&D culture. Not peptide-native. |
| **Bayer Biologics** (AgraQuest acquisition) | Same as above | Same; 10+ year dev cycles preclude platform-first bets |
| **Certis Biologicals / BioWorks** | Existing biofungicide products (Botector, Cease) on market | Microbial/plant-extract focus — not AI-native, no de novo design capability |
| **Marrone Bio (now Bioceres Biologicals)** | Market share in biofungicides, regulatory approvals in place | Traditional discovery, no ML-guided design loop |
| **Syngenta Biologics** | Global distribution, relationships with growers | Conventional agchem mindset; biologicals is a small internal division |

---

## Key Differentiation from Bindwell (expect this question)

| Factor | Bindwell | Agros |
|--------|----------|-------|
| **Discovery modality** | Broad molecular screening (small molecules + proteins) | Peptide-specific design (BoltzGen, Boltz-2) |
| **Interpretability** | Black-box prediction | Integrated Gradients → residue-level mechanistic insight |
| **Wet lab loop** | AI-designed, external validation | Closed-loop: assay results feed back into model |
| **Target strategy** | Broad pesticide targets | Specific ag targets (Botrytis β-glucan synthase, Lepidopteran nAChR/RyR) |
| **IP strategy** | License in-house AI-designed molecules | Novel peptide sequences + mechanistic IP |
| **Moat** | Speed + breadth of screening | Interpretability flywheel + proprietary experimental dataset |

**One-line answer to "what about Bindwell?":**
> "Bindwell screens broadly and fast. We design and understand — we know which residues drive binding, so every experiment informs the next. They optimize for throughput; we optimize for insight."

---

## Our Moat

**Short-term (0–18 months):**
- Interpretability advantage: IG analysis gives residue-level mechanistic insight competitors don't have
- Boltz-2 + BoltzGen stack applied specifically to ag targets — no one else has this pipeline tuned for biopesticide discovery
- Closed-loop: wet lab results feed back, making each model generation better

**Medium-term (2–4 years):**
- Proprietary dataset of peptide-activity pairs against ag-specific targets (Botrytis, nAChR, RyR) — no competitor can replicate without running the same experiments
- Target-specific fine-tuned models trained on our own experimental data

**Long-term:**
- IP portfolio on novel peptide sequences and their binding mechanisms
- Platform moat: each new target makes the system smarter
- Regulatory moat: first mover on EPA biopesticide registration for peptide actives

---

## The "Why Not Big Agchem?" Answer

> Large agchem runs 10–15 year development cycles and allocates R&D to incrementally improving existing chemistry. They can't justify a platform bet that requires 3 years of negative cash flow before first candidate. They acquire after de-risking. We're building what they'll either want to buy or won't be able to replicate in time.

---

## Competitive Positioning Statement

> "We're not competing with screening platforms. We're building the first closed-loop, interpretability-driven discovery engine for peptide biopesticides. Our moat isn't the model — it's the feedback loop between the model and the wet lab, and the mechanistic insight that makes every experiment count."

# Competitive Landscape

## Direct Competitors — AI-Driven Pesticide/Biopesticide Discovery

| Company                   | Approach                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Funding                                                                                                   | Weakness vs. Agros                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Bindwell** (YC W25)     | Scale AI models (Foldwell, PLAPT, APPT), scan all known synthesized compounds in 6 hours, in-house IP licensing. Not peptide-specific.                                                                                                                                                                                                                                                                                                                                                                                                              | $6M seed (General Catalyst, A Capital, SV Angel, Paul Graham). Pre-seed from Character Capital.           | Black-box approach — no mechanistic insight into *why* candidates bind. Broad pesticide focus (not peptide-specific). No closed-loop wet lab feedback reported.                                                                                                                                                                                                    |
| **Micropep Technologies** | Micropeptide-based crop protection using Krisalix (proprietary AI platform: generative AI + predictive modeling + PPI technology). Scans millions of candidates → dozens for in-planta screening. First AI-designed product (MPD-01, biofungicide for soybean rust/mildew/late blight) in field trials across EU, US, LatAm. **Business model: ingredient licensing** — partners with agchem companies to formulate/distribute, does not go to market itself. Regulatory submission for MPD-01 planned 2026; target launch US/Brazil 2028, EU 2030. | $69.3M total ($29M Series B); Bpifrance, Corteva, Ducera Growth Ventures. Founded 2016, Toulouse, France. | 10-year head start in peptide ag discovery but lab-first, not ML-native — Krisalix is AI-accelerated but built on traditional AMP screening (not structure-based generative design like BoltzGen/Boltz-2). No interpretability layer; no closed-loop model fine-tuning from assay results. Licensing model caps upside and cedes grower relationships to partners. |
| **Pepticom**              | AI-driven peptide design platform for pharma AND agriculture partners. Reinforcement learning iterations.                                                                                                                                                                                                                                                                                                                                                                                                                                           | Undisclosed (private, Israel-based)                                                                       | Primarily pharma-focused; agriculture is secondary. No biopesticide-specific wet lab integration.                                                                                                                                                                                                                                                                  |
| **Invaio Sciences**       | AI for soil/plant delivery + pesticide formulation. Broad ag biotech platform.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | ~$75M raised                                                                                              | Not focused on peptide discovery specifically. Different technical approach (delivery vs. discovery).                                                                                                                                                                                                                                                              |

---

| **Plant Health Care (PHC / PI Industries)** | Harpin-derived peptide elicitors (PREtec platform). Peptides mimic naturally occurring harpin proteins to trigger plant immune response (salicylic acid + jasmonic acid pathways) — they activate plant defenses rather than directly killing the pathogen. Products: Saori® (Brazil, 2021), Obrona® (US, 2023), PHC949 bionematicide (Brazil, 2024). Ea peptide 91398 (PHC-91398) received EPA registration June 2020 — the direct regulatory precedent for our path. Acquired by PI Industries (India) in August 2024 for ~£32.8M. FY2023 revenue: $11.2M, 60% gross margin. | Acquired by PI Industries (~£32.8M, Aug 2024). AIM-listed prior to acquisition. PI Industries: $787M trailing revenue. | **Elicitor, not inhibitor** — PHC peptides stimulate plant immunity, they do not directly inhibit the pathogen. This means efficacy depends on the plant's own immune response, which varies by crop, stress conditions, and timing. Agros peptides target the pathogen directly (β-glucan synthase, nAChR) — mechanism is independent of plant immune status. **No AI-driven de novo design** — PREtec is variants of known harpin proteins, not generatively designed sequences. No interpretability layer, no closed-loop model fine-tuning. Discovery is biology-first, not ML-native. **Acquired, not independent** — PI Industries is a conventional agchem company; PHC's innovation pace will slow under corporate ownership. |

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

## Deep Dive: Plant Health Care (PHC)

### What they built
PHC's core technology is **PREtec** — peptides derived from harpin proteins, which are naturally produced by certain plant-pathogenic bacteria. When a plant detects harpin proteins, it interprets them as an infection signal and activates its own immune system (the salicylic acid and jasmonic acid defense pathways). PHC synthesized short peptide variants of harpin that trigger this same response at low doses.

This is fundamentally an **elicitor strategy**: the peptide doesn't kill the pathogen — it makes the plant harder to infect. The peptide is a signal, not a weapon.

**Products on market:**
- **Saori®** — launched Brazil 2021, targets fungal disease and nematodes in soy
- **Obrona®** — launched US 2023, plant defense elicitor for broad disease protection
- **PHC949** — bionematicide peptide, Brazil 2024 soybean season
- **Ea peptide 91398 (PHC-91398)** — EPA registered June 11, 2020 (~23 months from application); broad agricultural + residential label; tolerance exemption granted (no residue limits required)

**Business:**
- FY2023 revenue: $11.2M, 60% gross margin
- Acquired by PI Industries (India) August 2024 for ~£32.8M
- Previously AIM-listed (London Stock Exchange)
- Pilot production: 100 kg peptide/month in Seattle lab

**Discovery process:** Biology-first, not ML-native. Peptides are rational variants of known harpin protein sequences — not generatively designed. No published AI/ML discovery pipeline. No closed-loop model fine-tuning from experimental results.

---

### How we're different — and better

| Dimension | Plant Health Care | Agros |
|-----------|------------------|-------|
| **Mechanism** | Elicitor — activates plant immune system | Direct inhibitor — kills/disables the pathogen directly |
| **Target** | The plant (immune pathway) | The pathogen (β-glucan synthase, nAChR, RyR) |
| **Efficacy dependence** | Depends on plant immune status, timing, crop variety | Independent of plant — works against the organism itself |
| **Discovery** | Rational variants of known harpin proteins | Generative design from scratch via BoltzGen + Boltz-2 |
| **Interpretability** | None — empirical screening of harpin variants | Integrated Gradients → residue-level mechanistic insight |
| **Feedback loop** | None — no closed-loop model fine-tuning | Wet lab results fine-tune Boltz-2 after every round |
| **Novelty ceiling** | Bounded by harpin protein family | Unbounded — de novo sequences not derived from any known protein |
| **Independence** | Acquired by PI Industries — innovation pace slows | Independent, ML-native, moving faster |

**The one-line answer if asked about PHC:**
> "PHC tells the plant to defend itself. We kill the pathogen directly. Their mechanism is limited by the plant's immune capacity; ours isn't. And they discovered their peptides by hand — we design ours with generative AI and know exactly which residues drive binding before we synthesize."

**Why the EPA precedent matters for us:**
PHC's Ea peptide 91398 registered in 23 months (July 2018 → June 2020) via the EPA biochemical biopesticide pathway, with a tolerance exemption — meaning no residue limit was required. This is the closest public data point we have for our own regulatory timeline. It validates that peptide biopesticides can move through EPA in ~2 years, not 5–7.

---

## Competitive Positioning Statement

> "We're not competing with screening platforms. We're building the first closed-loop, interpretability-driven discovery engine for peptide biopesticides. Our moat isn't the model — it's the feedback loop between the model and the wet lab, and the mechanistic insight that makes every experiment count."

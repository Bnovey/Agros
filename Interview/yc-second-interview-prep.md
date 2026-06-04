# YC Second Interview — Business Plan Prep
**Date:** 2026-06-04
**Focus:** Business model, deal traction, numbers

---

## What's Different From the First Interview

The second interview goes deeper on business model and numbers. They've already bought the technical story. Now they want to know if there's a real company here.

**New since first interview:**
- Two signed NDAs — Invaio Sciences (Flagship Pioneering) and Innatrix (SOSV)
- Tiered licensing deal structure defined
- Clear revenue model with specific numbers at each stage

Lead with this. It's real traction.

---

## The Opening Frame (30 seconds)

> "Since we last spoke, we've signed NDAs with two peptide biopesticide companies — Invaio Sciences, which is Flagship Pioneering-backed, and Innatrix, which is SOSV-backed. Both want to use our generative pipeline to produce peptide candidates for their in vivo testing programs. We're in commercial term discussions with both. That's the model in action."

---

## The Business Model (know this cold)

**What we do:** Generate ~100 ranked peptide candidates per target. Partners handle synthesis and in vivo testing. Rights to advance candidates into further development stages require a relicense at each tier.

**Why tiered:** Aligns our revenue to their value realization. We get paid more as our candidates prove out. It's defensible — the candidates are worth more at each stage.

### License Tiers

| Tier | Trigger | Invaio | Innatrix |
|---|---|---|---|
| 1 — Initial batch | Delivery of 100 candidates for 1 target | $150K | $75K |
| 2 — Field trials | Per candidate advancing to field trials | $200K/candidate | $100K/candidate |
| 3 — Registration | Per candidate submitted to EPA | $400K/candidate | $250K/candidate |
| 4 — Commercial | First commercial sale | 5% net sales | 4% net sales |

### Deal Value (base scenario — memorize these)

| Partner | Scenario | Total pre-royalty |
|---|---|---|
| Invaio | 5 candidates to field, 1 to registration | **$1.55M** |
| Innatrix | 4 candidates to field, 1 to registration | **$725K** |
| **Combined** | Both deals at base | **~$2.3M** |

**The number for YC:** *"Two deals. Base scenario gets us to $2.3M in milestone payments before either partner reaches commercial launch. If one candidate makes it to a $20M/year product, that's $1M/year in royalties at 5%."*

---

## The Deals (know both cold)

### Invaio Sciences
- **Backing:** Flagship Pioneering (Moderna-pedigree firm)
- **Contact:** Joseph DeBartolo, Head of ML
- **What they want:** Peptide candidates (100/batch) for in vivo testing
- **Status:** NDA signed. Next: schedule term sheet meeting
- **Why us:** They have Peptyde Bio's IP library of known peptides. They don't have a generative pipeline. We give them new candidates against any target, with residue-level design rationale.
- **Why they came to us:** Agros initiated outreach.

### Innatrix
- **Backing:** SOSV
- **Contact:** Jiarui Li, CEO
- **What they want:** TBD — to clarify in next meeting. Likely: pipeline access for their existing targets (potato late blight, SCN, citrus greening)
- **Status:** NDA signed. Next: schedule term sheet meeting
- **Why us:** They have targets and in vivo testing capacity. We give them a faster, more diverse candidate pool than biological screening.

**If YC asks which deal is more advanced:** Both are at the same stage — NDA signed, commercial terms TBD. Invaio is more resourced (Flagship), Innatrix is more directly comparable in stage and focus.

---

## Market Numbers (one breath each)

- **Biopesticide market:** $7–8B today, 12–15% CAGR → $14–18B by 2030
- **Botrytis losses:** $10B+ annually — single most economically damaging fungal pathogen
- **Resistance:** 94% of isolates resistant to thiophanate-methyl, 80% to pyraclostrobin, 67% to boscalid
- **Why now:** EU Farm to Fork — 50% synthetic pesticide reduction by 2030. Growers need alternatives now.
- **Peptide synthesis cost:** $2–4/amino acid. 20-residue candidate costs ~$100. Down 80% over a decade.

---

## Unit Economics (if asked)

| Item | Cost |
|---|---|
| GPU compute per 100 candidates (Modal A100) | ~$500–2,000 |
| Our cost to deliver a Tier 1 batch | <$2,000 |
| Tier 1 fee (Invaio) | $150,000 |
| Gross margin on Tier 1 | ~99% |

**The honest framing:** Our COGS on candidate delivery is almost zero — compute only. The value we're charging for is the IP and the model, not the inference cost. That's a good business.

---

## Revenue Timeline

| Milestone                         | When                         | Cash event               |
| --------------------------------- | ---------------------------- | ------------------------ |
| Tier 1 delivery (Invaio)          | As soon as term sheet signed | $150K                    |
| Tier 1 delivery (Innatrix)        | As soon as term sheet signed | $75K                     |
| Tier 2 — first candidates advance | 6–18 months post-delivery    | $200K/candidate (Invaio) |
| Series A                          | 12–18 months post-YC         | $5–10M                   |
| First royalties                   | 5–8 years post-delivery      | Ongoing                  |

**YC answer to "when's first revenue?":** *"As soon as we close the Tier 1 term sheets — which is the next step with both partners. That could be within weeks of finishing YC onboarding."*

---

## The Competitive Question (Invaio already has a peptide platform)

This will come up. Invaio acquired Peptyde Bio in 2023 for a peptide platform. Why do they need us?

**Answer:** Peptyde Bio gave them a library of *known* peptides — a catalog of what's already been found. We give them a generative pipeline that creates *new* candidates against *any* target they specify, with per-residue IG analysis that tells them why each candidate binds. They can't build that quickly internally. They're buying speed and a different type of output.

---

## The Hard Questions on Business Model

**"Why would they pay you per-candidate if they could just do this themselves?"**
> They can't replicate our pipeline quickly. BoltzGen + Boltz-2 on Modal with IG analysis isn't something you stand up in a quarter. And the candidates we deliver have per-residue attribution — that's not just sequence generation, that's IP-relevant mechanistic understanding they can put in a patent filing. That's the value they're paying for.

**"What if they just take the sequences and cut you out after Tier 1?"**
> The IP is ours. The candidates are licensed, not sold. Advancing beyond Tier 1 without a relicense would be a breach of contract. And practically: they want ongoing access to new batches. One Tier 1 delivery isn't the relationship — it's the start.

**"Why not just charge a flat upfront instead of tiered?"**
> Because a flat upfront would have to be low to close the deal at this stage — we have no wet lab data yet. The tiered structure lets us price to the actual value as it's realized. If their in vitro results show our candidates work, the Tier 2 payment is easy to justify. The risk is shared.

**"Two NDAs isn't revenue. Why should we be impressed?"**
> Fair. We're not claiming it's revenue. We're claiming it's two Flagship- and SOSV-backed companies who looked at our pipeline and decided it was worth signing an NDA and engaging in commercial discussions. At pre-revenue, that's what traction looks like in a capital-intensive biopesticide market. The term sheet conversation is the next meeting.

**"What's your plan if neither deal closes?"**
> Run the same playbook with the next 10 companies on our outreach list — Certis, BioConsortia, Gowan, FMC. We have a full outreach pipeline. Invaio and Innatrix are the most advanced, but they're not the only conversations we can have. And we'd use YC to accelerate those introductions.

---

## The Ask

**YC standard:** $500K for 7%

**How we spend it:**

| Period | Activity | Budget |
|---|---|---|
| Month 1–3 | Close Tier 1 term sheets, deliver first batches, in vitro Botrytis assays on top candidates | ~$30–60K |
| Month 4–6 | Second target (Lepidopteran nAChR), in vivo greenhouse trial partner | ~$40–80K |
| Month 7–12 | In vivo field data, bioRxiv preprint, Series A prep | ~$80–150K |
| Ongoing | Salaries (2 founders), compute (Modal) | ~$150–250K |

**3-month milestone:** Tier 1 deliveries to both Invaio and Innatrix, in vitro efficacy data on ≥5 Botrytis candidates, IG convergence validated.

**Series A trigger:** In vivo field efficacy data on ≥1 peptide active against Botrytis on a commercial specialty crop.

---

## What YC Wants to Hear You Say

- Specific names and deal stages, not vague "conversations"
- Exact numbers at each tier, not ranges
- A clear answer to "when's first revenue"
- That you know what you don't know (wet lab data, regulatory navigation)
- That the platform is the moat, not any individual candidate
- That the two NDAs are step 1, not the finish line

## What NOT to Say

- Don't call them "LOIs" — they're not. They're NDAs with commercial term discussions upcoming.
- Don't say "we're in talks with" — say "we've signed NDAs with and are scheduling term sheet meetings"
- Don't overstate wet lab progress — you don't have in vitro data yet. Be direct about it.
- Don't say "Flagship Pioneering-backed" and then not be able to explain what Flagship Pioneering is (Moderna, Evelo, Ring Therapeutics — the firm that bets on platform biotech)
- Don't confuse Tier 1 rights with a full license — be precise about what each tier grants

---

## Delivery Notes

- YC second interviews are 10 minutes, fast. They will interrupt. Let them.
- If they push back hard on the NDAs not being revenue: *agree with the premise, then make the real point.* "You're right — NDAs aren't revenue. Here's what they are: [make the point]."
- Numbers should come out in one breath: "$2.3M across both deals at base scenario" — not "well, it depends, but if you look at the tiers..."
- If they ask something you haven't thought through: "I don't know the exact answer to that — here's how I'd think about it."

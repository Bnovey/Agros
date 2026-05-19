# YC Interview Q&A Master Sheet

Drill these until every answer is under 30 seconds. YC partners will interrupt — that's fine. Land the point and stop.

---

## Company

**Q: What does Agros do?**
A: We use interpretable ML to design novel peptide biopesticides. We generate candidates with protein ML models, identify which residues drive activity using gradient attribution, then validate in the wet lab. Each round feeds back to improve the model. We're starting with antifungal peptides for Botrytis cinerea, which causes $10B+ in annual crop losses.

**Q: How is this different from just screening peptides?**
A: Screening is random — you hope you get lucky across millions of compounds. We know *why* each candidate works: which 3–5 amino acid residues matter. So we iterate systematically instead of randomly. It's like navigating with a map versus wandering.

**Q: How is this different from Bindwell?**
A: Bindwell screens broadly and fast — they scan all known synthesized compounds in hours and license what sticks. We design interpretable peptides and understand the mechanism. Our IG analysis tells us exactly which residues drive binding to Botrytis β-glucan synthase. That mechanistic insight is what lets us close the loop: every wet lab result teaches the model something specific.

---

## Market

**Q: How big is the market?**
A: The global biopesticide market is $7–8B today, growing 12–15% annually. The biofungicide segment specifically is $3.2B growing at 11% CAGR. And Botrytis alone causes $10B+ in annual losses — it's the single most economically damaging fungal pathogen in agriculture, yet growers are running out of fungicides that work.

**Q: Why biopesticides now?**
A: Three things aligned at once. First, regulation: the EU Farm to Fork targets a 50% cut in synthetic pesticide use by 2030 — growers need alternatives. Second, resistance: Botrytis has developed resistance to all currently registered single-site fungicides — 94% resistance to thiophanate-methyl, 80% to pyraclostrobin, 67% to boscalid in recent greenhouse surveys. Third, the ML tools now exist: Boltz-2, ESM-2, BoltzGen — the infrastructure to design peptides at scale didn't exist 5 years ago.

**Q: Who's your customer?**
A: Short-term: agchem biologicals divisions (Bayer, BASF, Syngenta) who license novel actives. Licensing gets us to revenue faster than building our own product. Medium-term: specialty crop growers directly — grapes, strawberries, ornamentals — who are paying $100–250/acre/season for fungicide programs and losing efficacy.

**Q: What's the regulatory path?**
A: Two tracks. For peptides that qualify as minimum-risk: EPA 25(b) exemption, no federal registration required, can sell state-by-state immediately. For novel mechanisms that need full registration: EPA biochemical pesticide pathway, faster and cheaper than conventional pesticide registration. We'd pursue 25(b) first to get to market, then full registration for scale.

---

## Traction

**Q: What have you built?**
A: A closed-loop discovery pipeline: BoltzGen generates peptide candidates against our targets, Boltz-2 scores binding affinity, and Integrated Gradients attribution maps tell us which residues are driving the prediction. We've run this on [X] candidates against Botrytis β-glucan synthase and Lepidopteran nAChR. [Add: any in vitro results.]

**Q: Have you talked to customers?**
A: Yes. [Name 2–3 people, their role, what they said. Use a direct quote. This is non-negotiable — have it ready.]

**Q: Do you have any revenue / LOIs?**
A: [Be honest — if none yet: "No revenue yet, but we have [specific traction signal: grower who agreed to pilot, agchem contact who asked for our data package, etc."]

**Q: Have you done in vitro validation?**
A: [Fill in your actual status. If not yet: "Not yet — we're using the YC funding to synthesize our top candidates and run Botrytis growth inhibition assays. We have [X] candidates ranked by Boltz-2 score and IG confidence."]

---

## Team

**Q: Why are you the right people to do this?**
A: We're Berkeley undergrads — one in bioengineering + data science, one in CS + math — and we've already shipped the pipeline before raising money. BoltzGen, Boltz-2 scoring, IG analysis on Modal infrastructure. We didn't wait to be credentialed. We just built it.

**Q: What does each founder bring?**
A: [Founder 1]: protein ML, IG analysis, the scientific side. [Founder 2]: systems engineering, compute infrastructure, the build-and-ship side. Together we cover the full stack from model to wet lab design.

**Q: Are you both full-time?**
A: [Be direct. If not yet: "We're both leaving [school/jobs] on [date] and will be full-time by [date]."]

**Q: How did you meet?**
A: [Your honest answer. Keep it short.]

---

## Product / Technical

**Q: What data are you training on?**
A: Boltz-2 was pre-trained on PDB structures and protein interaction data. We're using it for inference against our agricultural targets — Botrytis β-glucan synthase and Lepidopteran nAChR/RyR — and fine-tuning on ag-specific data as we generate it. BoltzGen handles peptide generation. ESM-2 provides embeddings.

**Q: What's your moat if someone copies your model?**
A: The model is only part of it. Our moat is the proprietary experimental dataset we're building — validated peptide-activity pairs against ag-specific targets. Every in vitro assay makes our model better. That data can't be replicated without running the same experiments. The longer we run, the harder it gets to catch up.

**Q: What if Bindwell or a big lab just does what you're doing?**
A: Bindwell's thesis is breadth and speed of screening — they'd have to completely change their approach to match ours. Big labs lack the closed loop between ML and wet lab that makes our system compound. And Boltz-2 + BoltzGen aren't widely used for ag targets yet — we're first.

**Q: What about AlphaFold / large foundation models eating your lunch?**
A: General protein models don't have activity data for our specific targets. They can predict structure; they can't tell you which peptide inhibits Botrytis β-glucan synthase at 8 µg/mL. That requires a domain-specific, wet-lab-validated dataset. We're building that dataset. The foundation models are inputs to our system, not competitors.

---

## Business Model

**Q: How do you make money?**
A: Licensing. We discover novel peptide actives, validate them in vitro and in vivo, and license the IP to agchem biologicals divisions. Deal structure: upfront payment ($500K–$3M for early validated actives) + development milestones + royalties on net sales (3–8%). This gets us to revenue without the full regulatory timeline of building our own product.

**Q: When will you be profitable?**
A: First licensing revenue possible within 18–24 months — once we have in vivo field data. A single licensing deal for a validated novel active can be $5–15M total value. That's the milestone we're building toward.

**Q: Why not build a product and sell directly to growers?**
A: We will — in phase 2. But licensing first is faster to revenue and doesn't require us to build distribution. While we're generating licensing income, we're also building the proprietary dataset that makes our platform more valuable. It's not either/or; it's sequenced.

---

## Competition

**Q: Who are your competitors?**
A: Most directly, Bindwell (YC W25, $6M, broad AI pesticide screening). Also Pepticom (peptide design platform, pharma + ag). And the traditional biopesticide companies — Certis, BioWorks, Marrone Bio — who aren't AI-native. Incumbents like BASF and Bayer biologicals divisions are the eventual buyers, not competitors.

**Q: Why won't Monsanto/Bayer just do this?**
A: They run 10–15 year development cycles optimized for incremental improvements to existing chemistry. A platform bet that requires 3 years of negative cash flow before first candidate doesn't fit their R&D model. They acquire after de-risking. We're building the asset they'll want to acquire — or that makes them irrelevant in biopesticides.

---

## Hard / Gotcha Questions

**Q: Why hasn't this been done before?**
A: Three things just converged: Boltz-2 and BoltzGen are new (2024–2025), peptide synthesis is 80% cheaper than a decade ago, and the regulatory environment is forcing growers off synthetic fungicides. Each of these individually wasn't enough. Together they create the window.

**Q: What could kill this company?**
A: Three real risks. One: our peptide candidates don't show in vitro activity — mitigated by IG analysis, which gives us mechanistic confidence before we synthesize. Two: regulatory pathway for novel peptides is longer than expected — mitigated by targeting 25(b) exemption path first. Three: Bindwell or a well-funded competitor moves faster — mitigated by our interpretability moat, which they'd have to rebuild from scratch.

**Q: You're undergrads — what if you just don't know what you don't know?**
A: That's fair. Here's what we know: we've shipped the pipeline, we've read the literature, and we've talked to growers. Where we're weakest is wet lab execution and regulatory navigation — which is why we're building an advisor network with lab access and ag domain expertise. We know what we don't know.

**Q: What's the hardest thing about what you're doing?**
A: Getting good wet lab data without breaking the bank. In vitro Botrytis assays at a contract lab run $500–1,500 per compound. That means we need our model to be right more often than random — which is exactly what the IG analysis is designed to do. The pressure to be right before we synthesize is real.

**Q: What will you do if your first peptides don't work in vitro?**
A: Go back to the model. The IG analysis should tell us *why* they didn't work — which residues were wrong, what the model got wrong. That's the point of interpretability. A failed assay isn't just a failure; it's data that sharpens the next round.

---

## Closing

**Q: What do you want from YC beyond money?**
A: Three things: introductions to agchem biologicals divisions who license novel actives (Bayer, BASF, Syngenta, Corteva); credibility with specialty crop growers for pilot agreements; and the regulatory and IP network to navigate EPA biopesticide registration.

**Q: What will you do with the money?**
A: First 3 months: synthesize top 20 candidates, run in vitro Botrytis assays, validate IG predictions. That costs roughly $30–60K in synthesis and assays. Remainder builds toward in vivo greenhouse trials and Series A prep.

**Q: What's your 18-month milestone?**
A: In vivo field efficacy data for at least one peptide active against Botrytis on a commercial specialty crop, bioRxiv preprint live, and one licensing conversation with an agchem biologicals division in progress.

---

## Delivery Notes

- If you don't know: say "I don't know, but here's how I'd find out" — never bluff
- If they push back hard: don't immediately capitulate — engage with the substance. "I hear that — here's why I think it's different in our case"
- If they interrupt: let them, then come back to finish your point in one sentence
- Never say: "great question", "interesting", or "that's a good point" — just answer
- Numbers to memorize: $7–8B biopesticide market, 12–15% CAGR, $10B+ Botrytis losses, 94%/80%/67% resistance rates, $2–4/AA synthesis cost, 10–30% hit rate expectation

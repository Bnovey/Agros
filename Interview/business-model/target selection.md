# Pest Target Selection

## Primary target: Botrytis cinerea (gray mold)

**Botrytis cinerea** is the first pest Agros will target.

### What it is

A necrotrophic fungal pathogen that causes gray mold disease on over 200 crop species — strawberries, grapes, tomatoes, lettuce, cannabis, and most soft fruits and vegetables. It is the second most economically damaging fungal pathogen in the world after *Magnaporthe oryzae* (rice blast), with global losses estimated at $10–100B annually depending on the season and geography.

### Why Botrytis first

**1. Resistance crisis creates an open market.**
Botrytis is notorious for acquiring resistance to every major fungicide class that has been deployed against it. It has now developed resistance to FRAC groups 1, 2, 3, 7, 9, 11, 12, and 17 in commercial fields. Growers are running out of effective rotations. A novel MOA that does not cross-resist existing chemistry is commercially valuable immediately — not after a 10-year adoption curve.

**2. The biology maps directly onto our platform.**
Botrytis secretes phospholipases (including PLA2-family enzymes) as part of its infection toolkit. These enzymes degrade host membrane phospholipids to release fatty acids that suppress plant immune responses and fuel hyphal growth. Targeting the fungal PLA2 is therefore a validated mechanism — we are not speculating about a target, we are attacking a known virulence factor with a peptide designed to bind exactly that enzyme class.

Our current pipeline already uses PLA2 (1BCI) as the target receptor, and our three BoltzGen candidates were designed against it. This means **our existing peptide work is the Botrytis program** — we do not need to start over.

**3. The delivery problem is solved.**
Botrytis is a foliar pathogen — it attacks above-ground plant tissue and spends significant time on the leaf surface and in the waxy cuticle layer before penetrating. Topical peptide delivery (spray application) is sufficient; we do not need to cross the plant cell wall or achieve systemic movement. This eliminates the biggest barrier to peptide-based crop protection.

**4. High-value crops absorb premium pricing.**
Strawberry and wine grape growers already pay $200–600/acre/season for fungicide programs. A premium biopeptide at 2–3× conventional cost is still economically rational if it provides resistance control. We do not need commodity-crop economics to build a viable business.

**5. Regulatory pathway is favorable.**
Peptide-based biopesticides qualify for FIFRA biopesticide classification in the US (BPPD) and are eligible for reduced-risk review. Botrytis registration packages are well-understood; EPA has approved multiple biopesticide actives against it (Bacillus amyloliquefaciens, Trichoderma, etc.), which provides a regulatory template.

**6. Fast experimental feedback loop.**
Botrytis is easy to culture, has a short infection cycle (48–72 hours from spore to visible lesion), and well-established in vitro and greenhouse bioassay protocols exist. We can get activity data in weeks, not years.

---

## Backup candidates

### #2 — Fusarium oxysporum (Fusarium wilt)

A soil-borne vascular pathogen that causes wilt disease in tomato, banana, cucumber, and dozens of other crops. Fusarium wilt is nearly impossible to control once established in a field because the pathogen colonizes the xylem — chemical fungicides cannot reach sufficient concentrations in vascular tissue. There is no curative treatment; growers rely on resistant varieties that are increasingly being overcome by new races.

**Why it's a strong backup:**
- Fusarium also secretes PLA2-family enzymes as virulence factors, so our target class is relevant
- Banana Fusarium wilt (TR4 strain) is a global food security crisis with essentially no chemical solution — enormous unmet need and political urgency
- Soil peptide delivery is harder than foliar but achievable via seed treatment or drip irrigation formulations

**Why it's #2 and not #1:**
Soil delivery adds formulation complexity. The feedback loop is slower (weeks per experiment vs. days for Botrytis). Better to validate the platform on a foliar pathogen first.

### #3 — Sclerotinia sclerotiorum (white mold / stem rot)

A broad-host-range necrotrophic fungus that attacks soybeans, canola, sunflowers, and many vegetables. Like Botrytis, it is necrotrophic, secretes cell-wall-degrading enzymes and oxalic acid, and has developed resistance to multiple fungicide classes. It is taxonomically close to Botrytis (both are Sclerotiniaceae), which means:

- Bioassay protocols and field trial designs developed for Botrytis transfer almost directly
- A peptide active against Botrytis PLA2 has a reasonable probability of cross-activity against Sclerotinia PLA2, potentially giving us a second label extension without a full new development program

**Why it's #3:**
Smaller total market than Botrytis, and cross-activity is a hypothesis to test rather than a guarantee. It is best developed as a line extension once the Botrytis program has generated proof of concept.

### #4 — Bemisia tabaci (silverleaf whitefly) — insecticide pivot

If the fungicide programs encounter unexpected obstacles (target not essential in planta, delivery failures, regulatory complications), the fallback pivot is an insecticide program against whitefly.

Whitefly is the vector for over 100 plant viruses, causes direct feeding damage to tomatoes, cucumbers, and peppers, and has developed resistance to neonicotinoids, pyrethroids, and organophosphates. Peptide insecticides (particularly those targeting insect gut epithelium or cuticle) have an established proof-of-concept base.

The mechanism would differ from the PLA2 program — we would need to identify whitefly-specific target proteins — but the IG pipeline infrastructure (BoltzGen design → IG attribution → residue optimization) transfers directly. This represents a platform pivot, not a technology pivot.

---

## Decision criteria for advancing a target

A target moves from candidate to active program when it satisfies all four:

1. **Validated mechanism:** PLA2 or analogous enzyme is confirmed as a virulence factor in published literature, not just predicted
2. **In vitro activity:** peptide achieves ≥50% growth inhibition in a plate bioassay at ≤10 µM
3. **Selectivity:** no mammalian cytotoxicity at 10× the active concentration (hemolysis assay, HEK293 viability)
4. **Delivery feasibility:** formulation team has a credible path to achieve ≥1 µg/cm² surface dose via spray or seed treatment

---

## EPA Data Requirements for Biochemical Biopesticide Registration

Governed by **40 CFR Part 158, Subpart U** and OPPTS Series 880 test guidelines. Requirements are substantially lower than conventional pesticides. EPA evaluates each study package case-by-case — many studies are conditionally required (CR) or waivable depending on exposure scenario and use pattern.

### 1. Product Chemistry (required for all registrations)

- Identity, composition, and manufacturing process of the active ingredient
- Certified limits (min/max concentrations of active + impurities)
- Physical/chemical properties (solubility, stability, pH, vapor pressure)
- Analytical method for detecting the active in the formulated product
- Storage stability data

### 2. Mammalian Toxicology — the "six-pack" (Tier I, all required)

These six acute studies are the baseline for every registration:

| Study | Route | Regulatory citation |
|-------|-------|-------------------|
| Acute oral toxicity | Gavage, rat | OPPTS 870.1100 |
| Acute dermal toxicity | Rat or rabbit | OPPTS 870.1200 |
| Acute inhalation toxicity | Rat, 4-hour | OPPTS 870.1300 |
| Primary eye irritation | Rabbit | OPPTS 870.2400 |
| Primary dermal irritation | Rabbit | OPPTS 870.2500 |
| Dermal sensitization | Guinea pig or LLNA | OPPTS 870.2600 |

**Tier II (conditionally required — triggered by Tier I results or food-use label):**
- 90-day oral (subchronic feeding study, rat) — required if food/feed use on label or if Tier I oral LD50 raises concern
- 90-day dermal — CR, often waived for foliar sprays with low dermal exposure
- 90-day inhalation — CR, often waived unless spray application creates significant aerosol exposure
- Developmental toxicity (rat + rabbit) — CR if food use
- Reproductive toxicity — CR if food use or subchronic raises concern
- Mutagenicity (Ames + chromosomal aberration) — CR

**Key advantage for peptides:** Naturally derived peptides that degrade rapidly are typically granted waivers on subchronic and chronic studies. PHC's Ea peptide 91398 received a **tolerance exemption** (no dietary residue limit required), which eliminated the entire residue chemistry data package. We should pursue the same.

### 3. Residue Chemistry (may be waived entirely)

- Nature of residue in plants and livestock — CR
- Crop field trials (magnitude of residue) — CR
- Tolerance or tolerance exemption petition

**Tolerance exemption** (what PHC got): if the peptide degrades rapidly and poses negligible dietary risk, EPA grants an exemption from the requirement of a tolerance — no residue trials needed. This is the target outcome for us and eliminates the most expensive data category.

### 4. Environmental Fate (reduced vs. conventional)

- Hydrolysis — CR
- Aerobic soil metabolism — CR for soil/foliar uses
- Photodegradation — CR
- Mobility (soil adsorption/leaching) — CR

Peptides that degrade rapidly in soil/water typically receive waivers on most environmental fate studies. The case is made by demonstrating rapid degradation in the product chemistry and available literature data.

### 5. Non-Target Organism (ecotoxicology)

| Organism | Study | Status |
|----------|-------|--------|
| Avian acute oral (bobwhite quail or mallard) | OPPTS 850.2100 | CR |
| Avian dietary (5-day) | OPPTS 850.2200 | CR |
| Freshwater fish acute (96-hr LC50, rainbow trout + bluegill) | OPPTS 850.1075 | CR |
| Freshwater invertebrate acute (Daphnia, 48-hr) | OPPTS 850.1010 | CR |
| Honeybee acute contact | OPPTS 850.3020 | CR if outdoor use |
| Aquatic plant growth inhibition | OPPTS 850.4400 | CR |
| Terrestrial plants (seedling emergence + vegetative vigor) | OPPTS 850.4100/4150 | CR if soil use |

For foliar-applied peptides, EPA typically requires the acute fish and invertebrate studies (low bar for natural peptides) and waives chronic/sublethal studies if acute results are clean.

### 6. Label and Application Requirements

- Proposed label (use sites, application rates, PPE, re-entry interval)
- Pest management claim substantiated by efficacy data (not formally submitted to EPA for biochemical biopesticides, but must be on file)
- Inert ingredient clearance (all formulation components must be on EPA's approved inerts list or separately cleared)

---

### Summary: realistic data package for a foliar peptide fungicide

| Category | Studies likely needed | Estimated cost |
|----------|----------------------|----------------|
| Product chemistry | Full package | $20–50K |
| Six-pack mammalian tox | All 6 acute studies | $50–150K |
| Subchronic (90-day oral) | Required for food-use label | $100–300K |
| Residue/tolerance | Tolerance exemption petition | $20–50K (if exemption granted, no field trials) |
| Environmental fate | Hydrolysis + soil metabolism (rest waived) | $30–80K |
| Ecotox | Fish, Daphnia, avian acute, honeybee | $50–150K |
| **Total** | | **~$300K–800K** |

This compares to $3–10M+ for a conventional pesticide registration. The PHC Ea peptide precedent (registered June 2020, ~23 months, tolerance exemption granted) is the closest public benchmark for our expected cost and timeline.

# EVAL REPORT v4 — Phase 1 corpus evaluation

Produced 2026-09-07 by `scripts/corpus-eval.ts`.

**This report presents evidence. It does not reach a verdict.** The six exit criteria in
`MASTER_PLAN.md` are the owner's to rule on, and section 5 below lays out what the numbers
show against each without declaring pass or fail.

## Run parameters

| | |
|---|---|
| Model | `claude-sonnet-5` (from `EXTRACTION_MODEL`; nothing in code selects a model) |
| Pricing | Batch API, 50% of list. Rates verified 2026-08-29 at platform.claude.com/docs/en/about-claude/pricing |
| Extraction batches | msgbatch_01E13rLs3ptEKUeKEePM1b6Q |
| Sweeper batches | msgbatch_01TnbBPSvs1jb8e2Eq35YndT, msgbatch_01Rcj7oUfSkFKbJsi9oU4uR9 |
| Random-sample seed | **20260829** — recorded so the samples below are reproducible and provably not hand-picked |
| Documents | 17 (13 readable, 4 unreadable, 0 failed) |
| Readable pages | 2,024 |
| **Total cost** | **$64.72** |

## 1. Summary table — every document

Duplicate rate is duplicates removed as a share of rows the model returned. Review-flag rate
is the share of final rows the pipeline itself marked as needing human attention.

| Document | Status | Pages | Rows | Dup rate | Dropped (invented) | Review flags | Trunc / lost chunks | Unresolved before → after | Cost |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of` | ok | 13 | 39 | 2.4% | 1 (0) | 20.5% | 0 / **0** | 3 → **3** | $0.00 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260` | ok | 74 | 519 | 0.0% | 10 (10) | 10.4% | 6 / **6** | 20 → **14** | $0.00 |
| `15F06726R0000194__RFP-15F06726R0000194-Tie` | ok | 75 | 394 | 0.0% | 13 (6) | 19.8% | 4 / **4** | 61 → **27** | $0.00 |
| `1616-26__RFP1620000348` | ok | 104 | 342 | 5.8% | 16 (1) | 24.3% | 2 / **2** | 58 → **34** | $0.00 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | ok | 81 | 457 | 0.0% | 20 (19) | 19.3% | 7 / **7** | 74 → **49** | $0.00 |
| `36C26026Q0939__SF-1449-36C26026Q0939-Stora` | **UNREADABLE** | 39 | — | — | — | — | — | — | $0.00 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VA` | ok | 73 | 154 | 13.7% | 10 (3) | 19.5% | 0 / **0** | 110 → **87** | $0.00 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | **UNREADABLE** | 8 | — | — | — | — | — | — | $0.00 |
| `70CDCR26R00000026__Attachment-01-Turnkey-F` | ok | 152 | 1414 | 3.2% | 21 (17) | 22.6% | 11 / **11** | 206 → **130** | $0.00 |
| `75N98026Q00962__RFQ-75N98026Q00962` | ok | 39 | 120 | 8.5% | 4 (3) | 29.2% | 0 / **0** | 77 → **33** | $0.00 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC0` | ok | 23 | 118 | 0.8% | 1 (0) | 15.3% | 0 / **0** | 20 → **12** | $0.00 |
| `PANMCC26P0000048766__Combined-Synopsis` | **UNREADABLE** | 33 | — | — | — | — | — | — | $0.00 |
| `W15P7T-26-R-A006__Solicitation-Amendment-0` | ok | 342 | 853 | 6.9% | 53 (12) | 24.5% | 4 / **4** | 324 → **173** | $0.00 |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicitati` | **UNREADABLE** | 144 | — | — | — | — | — | — | $0.00 |
| `W911SG27BA002__Solicitation-Amendment-W911` | ok | 157 | 755 | 3.3% | 31 (12) | 20.9% | 4 / **4** | 162 → **87** | $0.00 |
| `W912P726RA022__W912P726RA002-San-Rafael-So` | ok | 645 | 1732 | 3.2% | 137 (81) | 19.1% | 15 / **15** | 326 → **196** | $0.00 |
| `W912P825BA029__Solicitation-W912P825BA029-` | ok | 246 | 1221 | 6.6% | 55 (32) | 17.5% | 6 / **7** | 315 → **205** | $0.00 |

**Totals across readable documents:** 8,118 rows · 196 rows dropped as unlocatable · 3117 rows the sweeper recovered that the first pass missed · unresolved 1756 → 1050.

## 2. The unreadable documents

These are in the corpus on purpose. v1 does not OCR (DECISIONS.md D-004), so the correct
behaviour is to declare them unreadable and charge nothing — not to return a thin matrix.
They were **not skipped**: each was ingested, measured, and classified.

| Document | Pages | Low-text pages | Chars/page | What the user would be told |
|---|---:|---:|---:|---|
| `36C26026Q0939__SF-1449-36C26026Q0939-Sto` | 39 | 39 | 0 | This PDF appears to be scanned images rather than text: 39 of 39 pages carry almost no ext |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | 8 | 0 | 2387 | This PDF's text is garbled: 7 of the 8 pages with text come out as characters that are not |
| `PANMCC26P0000048766__Combined-Synopsis` | 33 | 33 | 0 | This PDF appears to be scanned images rather than text: 33 of 33 pages carry almost no ext |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicita` | 144 | 143 | 21 | This PDF appears to be scanned images rather than text: 143 of 144 pages carry almost no e |

## 3. THE AUDIT PACKET

Everything needed for the hand audit in EXTRACTION_PROMPT_SPEC §7, without asking anyone.

**How to read a source page.** From the repo root:

```bash
node scripts/extract.ts <corpus-path> --page <N>
```

That prints the exact text the pipeline saw on that page — no model call, no cost.

**The two full-read pages are deliberately not chosen here.** Criterion 1 asks whether the
pipeline misses what a careful reader catches. If this report picked the pages, the sample
would be steerable and the check worthless. **Pick any two pages per document yourself.**

### 0020153254COHEN__Attachment-1-Statement-of-Work-RFI

Path: `corpus/0020153254COHEN__Attachment-1-Statement-of-Work-RFI.pdf`

**13 pages · 39 rows · unresolved 3 · cost $0.00**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 4 | SOW 3 | shall | high | The Contractor shall adhere to the following applicable Federal standard /DHS/CBP/Office of Information and Technology (OIT) policies, standards, directives, processes, and procedures in performing work under this contract. These  |
| 2 | 4 | SOW 4.1 | must | high | The replacement radio systems must, at a minimum, provide: • Frequency range: 1.5 to 29.9999 MHz transmit-and-receive • Output power: 1 kW (+/-1dB) PEP and average • Duty cycle: 100% • Modes: SSB, ISB, CW, AM, AME Tx, WBHF • Data  |
| 3 | 5 | SOW 4.2 | shall | high | The Contractor shall test and functionally certify all equipment prior to delivery to ensure compliance and specifications. |
| 4 | 6 | SOW 5.1 | must | high | The Government Purchase Order number must be reflected on the Shipping label and clearly delineated on the packing slip(s). |
| 5 | 8 | SOW 10.1 | required | high | Absence of any comments by the COR does not relieve the Contractor of the responsibility for complying with the requirements of the Task Order. |
| 6 | 9 | SOW 10.4 | must | **review** | Any such proposed changes must be brought to the immediate attention of the CO for action. |
| 7 | 9 | SOW 10.4 | shall | high | The acceptance of any changes by the contractor without specific approval and written consent of the CO shall be at the contractor’s risk. |
| 8 | 11 | Addendum B | will | **review** | All models will be submitted using Business Process Modeling Notation (BPMN 1.1 or BPMN 2.0 when available) and the CBP Architectural Modeling Standards. |
| 9 | 12 | Addendum B | shall | high | Applicability of Internet Protocol version 6 (IPv6) to DHS-related components (networks, infrastructure, and applications) specific to individual acquisitions shall be in accordance with the DHS EA (per OMB Memorandum M-05-22, Aug |
| 10 | 13 | Addendum B | must | high | All hardware, software, and services provided under this task order must be compliant with DHS 4300A DHS Sensitive System Policy and the DHS 4300A Sensitive Directive Handbook. |

#### The 3 flagged pages with the largest shortfall

*No pages were flagged on this document.*

### 1240LT26Q0172__3-Combined-SpecsCBPEWI20260624

Path: `corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf`

**74 pages · 519 rows · unresolved 14 · cost $0.00**
Rows dropped because their quote could not be found in the document: **10**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 35 | 260010 1.2.B | required | high | It is the Contractor's responsibility to perform all coordination to determine Ameren’s requirements at the time of bidding and include all costs within the contract amount. |
| 2 | 41 | 260010 3.7.D | imperative | high | Splices in splice vaults: Indicate circuit number's point of origin, voltage and phase if power cable, and point of disconnecting means, see 260400. |
| 3 | 41 | 260010 3.9.A | imperative | high | Do not install conductors at temperatures less than 30 degrees F. |
| 4 | 44 | 260010 3.17.C.1 | imperative | high | Install all mechanical and electrical equipment before any preliminary electrical tests are made and final connections are made. |
| 5 | 49 | 260050 2.4.B | shall | high | Engraving shall be filled with black enamel for ivory plates, white enamel for brown plates and ivory plates filled with orange paint for isolated ground receptacle. |
| 6 | 50 | 260050 2.12.A | imperative | high | NSC threads on all threaded fasteners, nuts, bolts, screws, lag bolts, expansion shield, and other miscellaneous fasteners. Provide cadmium plated steel hardware for indoor dry locations. |
| 7 | 60 | 260400 G | imperative | high | Continue plot for five minutes. |
| 8 | 62 | 329200 2.2.A | imperative | high | Provide topsoil which is fertile, friable, natural loam, surface soil which is reasonably free of subsoil, high clay content, brush, weeds, litter, stumps, rocks and any other material which may be harmful to plant growth. |
| 9 | 62 | 329200 2.2.B.1 | imperative | high | B. Mulches: 1. Straw Mulch: Provide air dry, clean, mildew- and seed-free, and certified weed-free threshed straw of wheat, rye, oats, or barley. |
| 10 | 62 | 329200 3.2.A | imperative | high | Scarify existing subsoil and compacted areas prior to spreading topsoil. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 54 | 8 | 5 | 3 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 6 | 4 | 2 | 2 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 13 | 3 | 1 | 2 | yes | 1 exclusion(s): definitional — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 54
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 6
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 13
```

### 15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer

Path: `corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf`

**75 pages · 394 rows · unresolved 27 · cost $0.00**
Rows dropped because their quote could not be found in the document: **6**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 18 | D.2 | shall | high | All temporary shipping restraints and protective materials shall be identified and removed by the Contractor before final turnover unless the Government directs otherwise. |
| 2 | 19 | D.6 | shall | **review** | Government-furnished property and equipment shall retain all required Government property identification and accountability markings. |
| 3 | 23 | F.1 | declaration | high | Ordering Period Start Date End Date |
| 4 | 23 | F.2 | declaration | **review** | A delivery order issued during an effective ordering period shall remain governed by the contract until completion, even when delivery or performance extends beyond the end of that ordering period. |
| 5 | 34 | H.22 | conditional | high | Unless a delivery order expressly establishes a separately reimbursable item, all travel, lodging, per diem, shipping, permits, escorts, testing expenses, and incidental costs necessary to perform the order are included in the app |
| 6 | 39 | FBI-0023 | declaration | **review** | Registration is complete when the initial administrative user logs into the IPP web site with the User ID and password provided and accepts the IPP rules of behavior. |
| 7 | 52 | L.1 | shall | high | Any proposed exception or deviation shall be clearly identified in accordance with L.4 and L.8. |
| 8 | 56 | L.5.1 | shall | high | The submitted certification shall: 1. Identify the prime Offeror as the certified organization; and 2. Encompass quality-management activities applicable to the work the prime Offeror proposes to perform under this contract. |
| 9 | 57 | L.5.2 | imperative | high | Complete the Offeror-response columns of Attachment 3 – Master Requirements Traceability Matrix. |
| 10 | 58 | L.5.3 | imperative | high | Address: • Government design reviews; • Consolidation and resolution of Government comments; • Included review cycles; • Comment adjudication; • Configuration control; • Engineering changes; • GFE/GFI integration; • Contractor cor |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 21 | 15 | 9 | 6 | yes | 7 exclusion(s): government-actor, permission, definitional — resolved |
| 23 | 6 | 1 | 5 | yes | 4 exclusion(s): definitional, government-actor — **still unresolved** |
| 41 | 8 | 3 | 5 | yes | 5 exclusion(s): government-actor, definitional — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 21
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 23
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 41
```

### 1616-26__RFP1620000348

Path: `corpus/1616-26__RFP1620000348.pdf`

**104 pages · 342 rows · unresolved 34 · cost $0.00**
Rows dropped because their quote could not be found in the document: **1**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 38 | Section C | shall | high | Successful offeror(s) shall establish installation facilities in such a manner that receipt, inspection, and delivery of UNICOR product occurs by the customer’s due date. |
| 2 | 44 | SOW | conditional | **review** | All go-back quotes and/or additional work should be completed only after receipt of a purchase order/official authorization from an authorized UNICOR contractor. |
| 3 | 49 | SOW | shall | high | All sheets SHALL include UNICOR part numbers as well as all other requested information. |
| 4 | 49 | SOW | will | high | Offeror (s) will utilize the attached form. |
| 5 | 50 | SOW | must | **review** | Knockdown Product (KD)- product that is flat packed and must be assembled on-site. |
| 6 | 52 | Section E | declaration | **review** | SECTION E - INSPECTION AND ACCEPTANCE El. 52.246-4 Inspection of Services-Fixed-Price. (AUG 1996) |
| 7 | 65 | 52.212-4(q) | shall | high | The Contractor agrees to comply with 31 U.S.C. 1352 relating to limitations on the use of appropriated funds to influence certain Federal contracts; 40 U.S.C. chapter 37, Contract Work Hours and Safety Standards; 41 U.S.C. chapter |
| 8 | 80 | Section I (e)(2) | shall | high | (2) The Contractor shall search for the phrase “FASCSA order” in the System for Award Management (SAM) at https://www.sam.gov to locate applicable FASCSA orders. |
| 9 | 85 | Section I, DOJ-02.A.4 | conditional | high | In the event of adverse job actions resulting in the dismissal of a Contractor or Subcontractor employee before the separation checklist can be completed, the Prime Contractor must notify the Contracting Officer within 24 hours an |
| 10 | 91 | PWS D(3)(b) | conditional | high | (b) If the contract involves an IT system build or substantial development or changes to an IT system that may require privacy risk assessment and documentation, the Contractor shall provide adequate support to DOJ to ensure DOJ c |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 82 | 8 | 2 | 6 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 61 | 7 | 2 | 5 | yes | 3 exclusion(s): government-actor, definitional — **still unresolved** |
| 53 | 5 | 2 | 3 | yes | 3 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 82
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 61
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 53
```

### 19C02026Q0027__Solicitation-19C02026Q0027

Path: `corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf`

**81 pages · 457 rows · unresolved 49 · cost $0.00**
Rows dropped because their quote could not be found in the document: **19**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 41 | L.1 | shall | high | The Offeror shall review the list of excluded parties in the System for Award Management (SAM) at https://www.sam.gov for entities excluded from receiving federal awards for “covered telecommunications equipment or services.” |
| 2 | 49 | *(none)* | imperative | **review** | Electrical work required: Remove and dispose existing luminaries (5 und) and replace them with new Outdoor wall light, LED, neutral light 4000K, 10 Watts (5 und). |
| 3 | 53 | SOW 6.4 | shall | high | The new membrane shall replace the existing covering under identical conditions, replicating the exact geometry, shape, curvature, and pattern of folds of the existing installation to ensure complete architectural and structural c |
| 4 | 53 | SOW 6.3 | imperative | high | Maintain the designated work and staging area in a clean, orderly, and well-organized condition at all times, ensuring all contractor tools, equipment, and materials are properly stored exclusively within this assigned zone. |
| 5 | 54 | SOW 6.5 | imperative | high | Install transparent 10mm alveolar polycarbonate sheets over designated expansion frameworks, ensuring proper thermal expansion clearances, secure fastening, and continuous EPDM compression gaskets between the metal structure and p |
| 6 | 56 | SOW 8 | shall | high | All personnel on site shall wear approved safety helmets, high-visibility vests, safety glasses, steel-toed footwear, leather welding gloves, and hearing protection as required. |
| 7 | 76 | SOW | conditional | high | Si un punto de anclaje no ofrece la resistencia recomendada para protección contra caídas (5000 lbs), la empresa contratista deberá suministrar equipos de protección para alturas con amortiguadores que permitan disminuir la fuerza |
| 8 | 77 | SOW 12 | shall | high | La empresa contratista deberá enviar al COR los certificados (avalado por la ONAC) de la maquinaria empleada para la prestación del servicio (Incluye montacargas y camiones grúa) y los documentos de estos vehículos (SOAT, tarjeta  |
| 9 | 78 | SOW 14 | must | high | Las camionetas y vehículos pesados deben contar con pito y sensor de reversa. |
| 10 | 80 | SOW | must | **review** | Se debe mantener una copia de este Permiso de Trabajo en Caliente en los archivos de POSHO |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 29 | 11 | 2 | 9 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 68 | 9 | 3 | 6 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 43 | 5 | 0 | 5 | yes | 1 exclusion(s): government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 29
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 68
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 43
```

### 36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-

Path: `corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf`

**73 pages · 154 rows · unresolved 87 · cost $0.00**
Rows dropped because their quote could not be found in the document: **3**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 9 | SOW 8 | shall | high | Invoices shall reference: Contract Number Delivery Order Number Contract Line Item Description of flooring materials Delivery date and location |
| 2 | 10 | SOW 9.C.5 | shall | high | 5. The Contractor(s) shall immediately notify the appropriate Contracting Officer upon discovery of any inadvertent or unauthorized disclosures of information, data, documentary materials, records or equipment. |
| 3 | 11 | SOW 9.C.8 | shall | high | 8. The Contractor(s) shall not create or maintain any records containing any non-public [FACILITY] information that are not specifically tied to or authorized by the contract. |
| 4 | 32 | Section C | shall | **review** | In connection with any discount offered for early payment, time shall be computed from the date of the invoice. |
| 5 | 35 | Section C | shall | high | The Contractor shall make available at its offices, at all reasonable times, the records, materials, and other evidence for examination, audit, or reproduction, until 3 years after final payment under this contract or for any shor |
| 6 | 38 | C.3 52.209-9(c)(1) | must | high | The contractor must cite 52.209-9 and request removal within 7 calendar days of the posting to FAPIIS. |
| 7 | 53 | Section C | shall | high | (ii) Unless an exception applies according to paragraph (d)(4)(iii) or the Government grants a waiver, contractor shall not export certain sensitive technology to Iran, as determined by the President, and has an active exclusion i |
| 8 | 56 | C.13 VAAR 852.212-71 | shall | high | No gray market items shall be provided. Gray market items are OEM goods intentionally or unintentionally sold outside an authorized sales territory or sold by non-authorized dealers in an authorized sales territory. |
| 9 | 62 | E.2 | conditional | high | (c) If the offeror checked "has" in paragraph (b) of this provision, the offeror represents, by submission of this offer, that the information it has entered in the Federal Awardee Performance and Integrity Information System (FAP |
| 10 | 73 | E.10 52.212-2 | declaration | **review** | Veteran-Owned Small Businesses (“VOSB”). The following clauses, which are included in the Addendum to FAR 52.212-4 in Section C in of this Solicitation, set forth what will be considered under this evaluation factor. - VAAR 852.21 |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 6 | 33 | 0 | 33 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 5 | 18 | 2 | 16 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 55 | 12 | 3 | 9 | yes | 1 exclusion(s): permission — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 6
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 5
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 55
```

### 70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem

Path: `corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf`

**152 pages · 1414 rows · unresolved 130 · cost $0.00**
Rows dropped because their quote could not be found in the document: **17**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 20 | PWS 21.3 | shall | high | After the one-year base warranty period the contractor shall include four one-year option periods for technical services, support, testing, maintenance and repair of the system and components installed in this PWS. |
| 2 | 54 | PWS 42 | must | high | The Contractor must certify they will only utilize a GSA FedRAMP certified environment. |
| 3 | 62 | SOW 44.3 | shall | high | Contractors shall provide only Original Equipment Manufacturer (OEM) parts to the Government. |
| 4 | 69 | 3052.204-72 | shall | high | (h) Authority to Operate. The Contractor shall not collect, process, store, or transmit CUI within a Federal information system until an ATO has been granted by the Component or Headquarters CIO, or designee. |
| 5 | 81 | SOW (f) | shall | high | The CSP shall provide a federal facility using appropriate protective measures to provide for physical security. |
| 6 | 82 | SOW 5.2 | shall | high | Contractor shall be responsible for the following privacy and security safeguards: a) To the extent required to carry out the FedRAMP assessment and authorization process and FedRAMP continuous monitoring, to safeguard against thr |
| 7 | 107 | SOW | required | high | n. As required by the OSHA, 29 CFR, Part 1910.1035 (Occupational Exposure to Tuberculosis), all employees in occupations with high-risk exposure are required to have a TB Skin Test completed annually. |
| 8 | 115 | SOW | shall | high | ** Firearm training for detention officers who are required to provide Armed Transportation shall be in accordance with state licensing requirements. |
| 9 | 122 | SOW 64.5 | shall | high | The Contractor shall develop and implement a comprehensive sexual abuse/assault prevention and intervention program in accordance with Standard 2.11, Sexual Abuse and Assault Prevention and Intervention, found in the applicable IC |
| 10 | 126 | SOW 66.1 | shall | high | The Contractor shall provide enough ammunition for each armed detention officer, including uniformed contract supervisor(s) to be issued three full magazines. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 11 | 22 | 14 | 8 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 54 | 16 | 9 | 7 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 23 | 21 | 15 | 6 | yes | 1 exclusion(s): government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 11
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 54
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 23
```

### 75N98026Q00962__RFQ-75N98026Q00962

Path: `corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf`

**39 pages · 120 rows · unresolved 33 · cost $0.00**
Rows dropped because their quote could not be found in the document: **3**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | *(none)* | shall | high | Delivery shall be made 30 days after award date. |
| 2 | 3 | *(none)* | must | high | Responses to this solicitation must include clear and convincing evidence of the Offeror’s capability of fulfilling the requirement as it relates to the government requirements stated in this solicitation. |
| 3 | 3 | *(none)* | must | high | All responses must reference solicitation number 75N98026Q00960. |
| 4 | 12 | SOW - DELIVERY OR DELIVERABLE | shall | high | Documentation shall be provided by the vendor to show they completed the service on the equipment and left with the COR or POC. |
| 5 | 13 | SOW - Section 508 | must | high | The contractor must ensure that all EIT products that are less than fully compliant are offered pursuant to extensive market research which ensures that they are the most compliant products and services available. |
| 6 | 16 | Attachment 2 (i)(6) | shall | high | All amounts that become payable by the Contractor to the Government under this contract shall bear simple interest from the date due until paid unless paid within 30 days of becoming due. |
| 7 | 18 | Attachment 2 (p) | will | **review** | Except as otherwise provided by an express warranty, the Contractor will not be liable to the Government for consequential damages resulting from any defect or deficiencies in accepted items. |
| 8 | 19 | Attachment 2 (t)(1) | required | high | To remain registered in the SAM database after the initial registration, the Contractor is required to review and update on an annual basis from the date of initial registration or subsequent updates its information in the SAM dat |
| 9 | 20 | Attachment 2 (t)(3) | shall | high | The Contractor shall not change the name or address for EFT payments or manual payments, as appropriate, in the SAM record to reflect an assignee for the purpose of assignment of claims (see subpart 32.8, Assignment of Claims). |
| 10 | 34 | 52.212-5(d) | conditional | high | If this contract is completely or partially terminated, the records relating to the work terminated shall be made available for 3 years after any resulting final termination settlement. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 37 | 7 | 1 | 6 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 38 | 8 | 2 | 6 | yes | 5 exclusion(s): permission, government-actor — **still unresolved** |
| 15 | 9 | 4 | 5 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 37
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 38
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 15
```

### 80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final

Path: `corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf`

**23 pages · 118 rows · unresolved 12 · cost $0.00**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | *(none)* | imperative | **review** | NOTE: OFFEROR TO COMPLETE BLOCKS 12, 17, 23, 24, AND 30. |
| 2 | 4 | Section B | declaration | high | 3a. OPTIONAL: Price for Reservation of Medical Transport via Air Ambulance for Traveler*, *** |
| 3 | 4 | Section B | declaration | high | 3b. OPTIONAL: Price for Call-Up/Execution of Medical Transport via Air Ambulance for Traveler*, **, *** |
| 4 | 5 | SOW 1.1.2 | conditional | high | In the event that the Contractor cannot provide a medevac in specific parts of the world, then the Contractor shall provide sufficient rationale to the NASA COR why it cannot and provide an alternate solution at no additional cost |
| 5 | 7 | SOW 2.2.5.a | conditional | high | If a medical event occurs, all costs for evacuation and additional medical travel services shall be the responsibility of the Contractor and no additional cost beyond the prepaid provided group membership as specified in Section B |
| 6 | 7 | SOW 2.3.3 | shall | **review** | The Contractor shall provide, within 60 calendar days of contract start and update annually for currency by Oct 1st, a Medical Facility Support Plan for each of the Russia and Kazakhstan medical facilities identified by NASA which |
| 7 | 9 | SOW 2.3.10 | shall | high | The Contractor shall provide, within 60 calendar days after contract award, wallet sized cards to the NASA COR for every NASA traveler with information including an international call number for support for medical support and eme |
| 8 | 14 | Section H | declaration | high | Additional Regulation or Supplement Clauses Incorporated by Reference Number Title Effective Date NFS 1852.232-77 Limitations of Funds (Fixed Price Contract) Mar 1989 |
| 9 | 17 | Section K | must | high | The Offeror must submit a statement of verification that, as of the date of its offer, its representations and certifications posted electronically in www.SAM.gov for the provisions listed in FAR 52.204-19 - Incorporation by Refer |
| 10 | 19 | Section L | shall | high | Offerors shall describe how they would respond to (4) four medical scenarios provided below assuming they occur at the time of the solicitation. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 5 | 12 | 9 | 3 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 8 | 9 | 6 | 3 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 18 | 12 | 9 | 3 | yes | 4 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 5
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 8
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 18
```

### W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006

Path: `corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf`

**342 pages · 853 rows · unresolved 173 · cost $0.00**
Rows dropped because their quote could not be found in the document: **12**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 56 | 52.209-1(e) | conditional | high | If an offeror, manufacturer, source, product or service has met the qualification requirement but is not yet on a qualified products list, qualified manufacturers list, or qualified bidders list, the offeror must submit evidence o |
| 2 | 62 | Section I | conditional | high | the Contractor shall not be obligated to continue performance if to do so would exceed the ceiling price set forth in the Schedule, unless and until the Contracting Officer notifies the Contractor in writing that the ceiling price |
| 3 | 93 | 52.212-4 | shall | **review** | In the event of such termination, the Contractor shall immediately stop all work hereunder and shall immediately cause any and all of its suppliers and subcontractors to cease work. |
| 4 | 184 | Section I (g) | conditional | high | The Contractor, and its subcontractors or suppliers, may only assert restrictions on the Government's rights to use, modify, reproduce, release, perform, display, or disclose technical data or computer software the Contractor must |
| 5 | 199 | (c)(4) | must | **review** | (4) Restricted rights in computer software. The Government shall have restricted rights in other than commercial computer software the Contractor must deliver or otherwise furnished to the Government under this contract that was d |
| 6 | 280 | L.2.2 | shall | high | Each Offeror shall calculate and submit a numerical self-score as part of its proposal Attachment 0002 for each Domain they are proposing to. |
| 7 | 280 | L.2.2 | conditional | high | If the CAGE or UEI was added to the certification post-issuance, the Offeror shall provide evidence of Government acceptance of the addendum. |
| 8 | 282 | L.2.2.1 | shall | high | *Documentation shall include a screenshot of your Supplier Performance Risk System (SPRS) certificate. |
| 9 | 306 | *(none)* | required | **review** | the Offeror is required to submit the actual cost of performance |
| 10 | 324 | M.1 | must | high | Proposals must contain the best offer. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 292 | 13 | 6 | 7 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 33 | 8 | 3 | 5 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 133 | 7 | 2 | 5 | yes | 4 exclusion(s): government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 292
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 33
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 133
```

### W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001

Path: `corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf`

**157 pages · 755 rows · unresolved 87 · cost $0.00**
Rows dropped because their quote could not be found in the document: **12**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 21 | *(none)* | must | **review** | Employees must be permitted to use paid sick leave for their own illness, injury or other health-related needs, including preventive care; to assist a family member (or person who is like family to the employee) who is ill, injure |
| 2 | 37 | Section 00 45 00 | declaration | high | FAR Provisions Incorporated by Reference |
| 3 | 108 | SOW 1.23.4 | shall | high | The removal from the job site or dismissal from the premises shall not relieve the Contractor of the requirement to provide sufficient personnel to perform the services as required by this SOW. |
| 4 | 108 | SOW 1.23.5 | shall | high | Contractor personnel, whose tasks involve operation of any vehicles, shall possess a valid U.S. state driver's license, certificates and permits, applicable for the type and class of vehicle being operated. |
| 5 | 109 | SOW 1.27 | conditional | high | If key personnel is to be replaced, the Contractor shall notify the KO within 24 hours and the alternate Program Manager or QCM shall be identified. |
| 6 | 114 | SOW 1.41 | shall | high | The Contractor shall check all drawings furnished immediately upon receipt; compare all drawings and verify the figures before laying out the work; promptly notify the KO of any discrepancies; be responsible for any errors that mi |
| 7 | 114 | SOW 1.41.3 | shall | high | 1.41.3. Email tittle nomenclature shall include the following: FYXX_Facility_Work_Order_Task Order_Project_Description_Subject. |
| 8 | 118 | SOW 4.7 | shall | high | The Contractor shall be responsible for safety during all phases of project undertaking and recognizing activities, operations, or conditions which pose a hazard or risk to life, health, or safety. |
| 9 | 126 | SOW 4.31 | conditional | high | If any holes/pits/trenches required are to be left open (unfilled) overnight, one side of the excavation needs to be sloped to allow any potentially trapped wildlife to escape. |
| 10 | 129 | SOW 4.41 | shall | high | Schedules shall be provided at no additional cost to the Government. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 16 | 15 | 3 | 12 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 77 | 11 | 4 | 7 | yes | 5 exclusion(s): government-actor — **still unresolved** |
| 41 | 6 | 1 | 5 | yes | 1 exclusion(s): government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 16
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 77
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 41
```

### W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-

Path: `corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf`

**645 pages · 1732 rows · unresolved 196 · cost $0.00**
Rows dropped because their quote could not be found in the document: **81**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 20 | 00 22 00 3.2 | must | high | Both volumes must be received not later than the date and time specified in Block 13 of SF 1442, or as amended in a SF 30. |
| 2 | 157 | 01 32 01.00 10 1.4 | shall | high | Each schedule submittal shall identify the Project Scheduler who was responsible for the preparation of the schedule submittal and all required updating and production of reports. |
| 3 | 176 | 01 32 23 1.8 | shall | high | Any changes to sensor offsets or lever arms during the project shall be documented and submitted to the Government for approval prior to performing additional survey work. |
| 4 | 176 | 01 32 23 1.9 | shall | high | Survey procedures, data collection equipment, methods and densities, and equipment calibration for this work shall follow the criteria given in EM 1110-2-1003 for class of survey specified in task orders to this basic contract. |
| 5 | 182 | SOW 1.5.1 | imperative | high | Use transmittal form ENG Form 4025 provided in RMS for submitting both Government approved and information only submittals in accordance with the instructions on the reverse side of the form. |
| 6 | 182 | SOW 1.5.1 | required | high | These forms are included in the RMS CM software that the Contractor is required to use for this contract. |
| 7 | 204 | SOW 1.12.4 | imperative | high | Develop a Standard Lift Plan (SLP) in accordance with EM 385-1-1 using ENG Form 6203 Standard Pre-Lift Crane Plan/Checklist for each lift planned. |
| 8 | 208 | SOW 01 35 26 l | imperative | high | Have VHF radios at all steering stations on board all dredges, monitoring vessels, survey vessels, launches and other boats used on the project. |
| 9 | 232 | SOW 3.8 | must | **review** | All calendar days must be accounted for throughout the life of the contract. |
| 10 | 239 | SOW 1.7.3.4 | imperative | high | Roll up the labor and equipment exposure data into a monthly exposure report. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 404 | 13 | 1 | 12 | yes | 8 exclusion(s): government-actor — **still unresolved** |
| 371 | 15 | 5 | 10 | yes | 10 exclusion(s): government-actor — resolved |
| 364 | 9 | 1 | 8 | yes | 1 exclusion(s): government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 404
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 371
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 364
```

### W912P825BA029__Solicitation-W912P825BA029-OM25035

Path: `corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf`

**246 pages · 1221 rows · unresolved 205 · cost $0.00**
Rows dropped because their quote could not be found in the document: **32**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 20 | 00010 1.5.a | shall | high | EM 385-1-1, Chapter 19, "Floating Plant and Marine Activities" shall be complied with at all times. |
| 2 | 21 | *(none)* | shall | **review** | In the event an Option Item is partially exercised, the contract completion date shall be extended using the following formula: [(partial option hours exercised)/(option hours listed in bid schedule for applicable bid lot)] x cale |
| 3 | 54 | 52.232-16 | shall | high | The estimates shall include sufficient detail to permit Government verification. |
| 4 | 77 | 1.6.b | conditional | high | b. If the Contractor proposes a deviation from the Government furnished rights-of-way for his convenience, the Contractor shall notify the Contracting Officer or their representative in writing. |
| 5 | 80 | 1.12 | shall | high | The "As-Built" drawings shall be a record of the construction as completed by the Contractor. |
| 6 | 88 | 01 33 00 1.10 | shall | high | Adequate time (a minimum of 30 calendar days exclusive of mailing time) shall be allowed and shown on the register for review and approval. |
| 7 | 92 | *(none)* | declaration | high | SUBMITTAL REGISTER CONTRACT NO. |
| 8 | 100 | 1.13 | shall | high | The Contractor shall develop, implement, and maintain at the workplace a written, Comprehensive Hazard Communication Program (see Chapter 6 of EM 385-1-1) that includes identification of potential hazards as prescribed in 29 CFR P |
| 9 | 118 | 3.7 | shall | **review** | The bathroom facilities shall be kept clean and sanitary at all times. |
| 10 | 237 | 3.1.1.2.4 | shall | high | The X and Y position of the terminal end of the outfall pipe shall be monitored continuously and the position reported as part of the work event string. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 76 | 14 | 5 | 9 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 98 | 25 | 17 | 8 | yes | 1 exclusion(s): permission — **still unresolved** |
| 53 | 14 | 7 | 7 | yes | 6 exclusion(s): government-actor, definitional — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 76
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 98
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 53
```

## 4. The invention count, per document

A row is dropped when its quoted text cannot be located anywhere in the document. This is the
structural defence against a fabricated citation, and the count is what the defence caught.
A dropped row never reaches a user — but a rising count means the model is leaning on the net.

| Document | Rows kept | Dropped: not found | Dropped: bad shape | Dropped: page out of range |
|---|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-W` | 39 | **0** | 0 | 0 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI2026062` | 519 | **10** | 0 | 0 |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-` | 394 | **6** | 0 | 0 |
| `1616-26__RFP1620000348` | 342 | **1** | 0 | 0 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 457 | **19** | 0 | 0 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPI` | 154 | **3** | 0 | 0 |
| `70CDCR26R00000026__Attachment-01-Turnkey-Fac` | 1414 | **17** | 0 | 0 |
| `75N98026Q00962__RFQ-75N98026Q00962` | 120 | **3** | 0 | 0 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027` | 118 | **0** | 0 | 0 |
| `W15P7T-26-R-A006__Solicitation-Amendment-004` | 853 | **12** | 0 | 0 |
| `W911SG27BA002__Solicitation-Amendment-W911SG` | 755 | **12** | 0 | 0 |
| `W912P726RA022__W912P726RA002-San-Rafael-Soli` | 1732 | **81** | 1 | 0 |
| `W912P825BA029__Solicitation-W912P825BA029-OM` | 1221 | **32** | 3 | 0 |

**Total dropped as unlocatable across the corpus: 196.**

## 5. The six exit criteria — what the numbers show

Ranked by how much of the decision rests on each. **No verdict is offered here.**

### 1. Catches everything (the recall check) — the criterion the project turns on

The owner reads two pages per readable document by hand and counts what the pipeline missed.
**This report deliberately does not choose those pages.**

What the machine can say about its own recall:

- The independent scan found **6689** binding-verb occurrences across readable documents.
- After the sweeper, **1050** occurrences remain neither captured nor explained.
- The sweeper recovered **3117** requirements the first pass had missed. The first pass alone is measurably not enough.
- The scan sees Class-A verbs only. Imperatives, conditionals, and declarations carry no magic
  word and are invisible to it, so this measures one kind of miss, not all kinds. The hand audit
  is the only check that covers the rest.

### 2. Nothing invented

- **196** rows were dropped corpus-wide because their quote could not be found.
- Every row that reached the matrix had its quote located in the source page text.
- The check itself was proven by planting a fake row and a paraphrase, both rejected; disabling
  the check turns those tests red. See the 2026-08-29 Task 3 report.

### 3. Coverage check honest

- Unresolved across readable documents: **1756 before the sweeper, 1050 after.**
- Unresolved is reported exactly as computed. It is never suppressed, rounded, or estimated.
- **Known bias, stated plainly:** where the scan wrongly excludes a contractor obligation as a
  government one, the denominator shrinks and coverage looks *better* than it is. Where the scan
  double-counts a fill-in form line, the shortfall inflates and coverage looks *worse*. The first
  error flatters us and the sweeper cannot catch it, because it only visits pages already showing
  a shortfall. Section 3's flagged pages are where to check this by hand.

### 4. Citations right

- Page numbers were verified by an independent extractor on 16 pages across 5 documents
  (2026-08-28 report): 16 of 16, with an off-by-one probe against neighbouring pages.
- Section references are **not** machine-checkable — the pipeline is instructed never to invent
  one and to use null instead. The random samples in section 3 are where that gets checked.

### 5. Unreadable documents declared unreadable

- 4 of 17 documents were declared unreadable and returned no matrix.
- They appear in section 2 with their measured page counts and text density, not as skips.

### 6. Cost measured and recorded per document

- **$64.72** for the corpus at Batch API rates.
- Per-document figures are in the summary table and the per-document JSON.
- MASTER_PLAN sets no pass mark on cost: pricing is decided after the real number is known.

## 6. Where everything is

| What | Where |
|---|---|
| Per-document JSON, including every row | `corpus/eval/` |
| The corpus documents | `corpus/` |

## What changed from v3

v3 was the same extraction as v2 with mechanical corrections applied on top. This run
re-extracted every document with the corrected reading order, the corrected duplicate
rule, and the one documented prompt iteration. Everything below is measured.

### Read the row counts correctly: row count is no longer a measure of recall

The prompt now captures a list lead-in **together with its items as a single row**,
where before it produced a bare lead-in (or one row per item, inconsistently). Rows
therefore get fewer and longer at the same time, and a drop in row count can mean more
of the document is captured, not less.

Measured on the two-document metering pilot of 2026-09-07 (`corpus/eval-v4-pilot/`),
same documents, same Batch path, old prompt versus new:

| | v2/v3 | v4 |
|---|---:|---:|
| Rows, 0020153254COHEN / 80JSC026MEDEVAC5Q | 40 / 132 | 37 / 114 |
| Mean verbatim length, characters | 175 / 158 | 213 / 237 |
| Longest verbatim, 80JSC026MEDEVAC5Q | 583 | 1,956 |
| Rows that are a bare lead-in ending in a colon | 3 / 6 | 1 / 0 |
| Model output tokens, both documents | 118,531 | 155,977 |

**12% fewer rows carrying 29% more quoted text.** Judge recall by the reviewer-finding
table and the coverage figures, never by the row count alone.

### The output-ceiling patch, and why the raw run understated the result

60 of the 257 extraction chunks (59 truncated at the output ceiling, 1 unparseable)
produced NO rows on the first pass - a bigger hole than v2's 47, because capturing a
list lead-in together with its items makes responses longer and pushes more of them
into the ceiling. Those chunks were pre-split and resubmitted over four rounds. Before
the patch this run held 6,685 rows and 8.9% LESS quoted text than v3; after it, 8,118
rows and 7.9% MORE. The unpatched figures are not the result and are not reported as
such anywhere in this document.

Two chunks were never recovered, and both are REFUSALS, not truncations - the model
declined pages 102 and 103 of W912P825BA029. They are listed in `not_processed` with
that reason. A refusal and a truncation need different answers and never share a
counter here.

### Read the unresolved counts correctly: v3 and v4 are not on the same basis

Unresolved rose from 593 in v3 to 1,050 here. Most of that is definitional, not a
recall regression, and the honest comparison is the reviewer-finding table rather than
this number:

- **The sweeper is credited more strictly.** A page already swept is now credited only
  with what the sweeper explicitly EXPLAINED on it. Its captured rows are already in
  the recomputed comparison, so counting them again would flatter the number. This
  change alone raises the count and cannot lower it.
- **The scan sees more.** Two new Class D triggers, running headers stripped, and
  sentences rejoined across page breaks all expose binding phrases the v3 scan walked
  past - including the one both reviewers named on 0020153254COHEN pp.11-12.
- **The list rule shrinks the captured side of the comparison.** Coverage compares
  Class-A ROWS per page against scanned occurrences. Ten list items captured as one
  row count once, not ten, so the shortfall on that page rises by nine even though
  more of the page is in the matrix. This is the same trap as the row count.

### Rows per document, before and after

| Document | v3 rows | v4 rows | change | v3 unresolved | v4 unresolved |
|---|---:|---:|---:|---:|---:|
| `0020153254COHEN` | 40 | 39 | -1 | 0 | 3 |
| `1240LT26Q0172` | 621 | 519 | -102 | 3 | 14 |
| `15F06726R0000194` | 427 | 394 | -33 | 29 | 27 |
| `1616-26` | 385 | 342 | -43 | 20 | 34 |
| `19C02026Q0027` | 587 | 457 | -130 | 16 | 49 |
| `36C26126Q1034` | 255 | 154 | -101 | 24 | 87 |
| `70CDCR26R00000026` | 1674 | 1414 | -260 | 52 | 130 |
| `75N98026Q00962` | 159 | 120 | -39 | 29 | 33 |
| `80JSC026MEDEVAC5Q` | 132 | 118 | -14 | 5 | 12 |
| `W15P7T-26-R-A006` | 1008 | 853 | -155 | 122 | 173 |
| `W911SG27BA002` | 841 | 755 | -86 | 63 | 87 |
| `W912P726RA022` | 2149 | 1732 | -417 | 154 | 196 |
| `W912P825BA029` | 1351 | 1221 | -130 | 72 | 205 |
| **All readable** | **9648** | **8118** | **-1530** | **593** | **1050** |

v3 covered 14 readable documents; v4 covers 13, because 47QMCA26Q0098 is now declared
unreadable with reason `garbled-text` rather than extracted as nonsense. Its v3 rows are
therefore absent from the v4 column by design.

### Unresolved, and what the change means

Unresolved counts binding-verb occurrences the pipeline could not account for after the
sweeper. Three things moved it in this run, in different directions:

- **Up:** two new Class D triggers (`requirements include`, `verification requirements
  include`) mean the scan now sees binding phrases it used to walk past.
- **Up:** running headers are stripped and page-break sentences rejoined, so phrases
  that were previously invisible to the scan ("no" on one page, "less than four" on the
  next) are now counted.
- **Down:** wage-determination footnotes inside a detected schedule are excluded as
  `schedule` — counted and reported under `excluded.schedule`, never silently dropped.

| Document | occurrences excluded as `schedule` |
|---|---:|
| `W912P726RA022` | 160 |

### Invention: rows dropped because the quote could not be found

| Document | v3 not-found | v4 not-found | v4 mangled (second reader) |
|---|---:|---:|---:|
| `0020153254COHEN` | 1 | 0 | 1 |
| `1240LT26Q0172` | 6 | 10 | 0 |
| `15F06726R0000194` | 10 | 6 | 7 |
| `1616-26` | 6 | 1 | 15 |
| `19C02026Q0027` | 12 | 19 | 1 |
| `36C26126Q1034` | 14 | 3 | 7 |
| `70CDCR26R00000026` | 19 | 17 | 4 |
| `75N98026Q00962` | 0 | 3 | 1 |
| `80JSC026MEDEVAC5Q` | 2 | 0 | 1 |
| `W15P7T-26-R-A006` | 42 | 12 | 41 |
| `W911SG27BA002` | 27 | 12 | 19 |
| `W912P726RA022` | 23 | 81 | 55 |
| `W912P825BA029` | 24 | 32 | 20 |

Every one of these rows was withheld from the matrix. `not-found` means the pipeline's
own reader could not locate the quote; `mangled` means an independent reader could not,
which catches a quote spliced across columns that looks correct to us.

### Rows removed as same-page contained fragments

A row whose text sits wholly inside a longer row on the same page is a fragment of it,
not a second requirement. The ten longest removed fragments are quoted per document so
the rule can be checked against real text rather than trusted.

**`1616-26`** — 4 fragment(s) removed.

- p.94: "52.203-11 Certification and Disclosure Regarding Payments to Influence Certain Federal Transactions (Sep 2024) 52.204-16 Commercial and Government Entity Code Reporting. (Aug 2020) 52.204-17 Ownership or Control of Offeror. (Aug 2020)"
- p.45: "National Installation Manager – who shall meet with UNICOR management in Washington, DC unless otherwise directed elsewhere, at a minimum of quarterly."
- p.58: "No adjustment shall be made if the referenced change is less than 2%."
- p.58: "SECTION I – CONTRACT CLAUSES"

**`36C26126Q1034`** — 10 fragment(s) removed.

- p.32: "If the Contractor becomes aware of a duplicate contract financing or invoice payment or that the Government has otherwise overpaid on a contract financing or invoice payment, the Contractor shall— (i) Remit the overpayment amount to the payment office cited in the contract along with a description o…"
- p.32: "The Contractor shall indemnify the Government and its officers, employees, and agents against liability, including costs, for actual or alleged direct or contributory infringement of, or inducement to infringe, any United States or foreign patent, trademark, or copyright, arising out of the performa…"
- p.38: "The Contractor shall update the information in the Federal Awardee Performance and Integrity Information System (FAPIIS) on a semi-annual basis, throughout the life of the contract, by posting the required information in the System for Award Management via https://www.sam.gov."
- p.56: "Vendor shall be an OEM, authorized dealer, authorized distributor, or authorized reseller for the proposed equipment/system, verified by an authorization letter or other documents from the OEM."
- p.32: "All amounts that become payable by the Contractor to the Government under this contract shall bear simple interest from the date due until paid unless paid within 30 days of becoming due."
- p.55: "For indefinite delivery contracts, the Contractor shall report to both the contracting office for the indefinite delivery contract and the contracting office for any affected order."
- p.56: "No used, refurbished, or remanufactured supplies or equipment/parts shall be provided."
- p.56: "This procurement is for new Original Equipment Manufacturer (OEM) items only."
- p.56: "No counterfeit supplies or equipment/ parts shall be provided."
- p.56: "No gray market items shall be provided."

**`70CDCR26R00000026`** — 1 fragment(s) removed.

- p.40: "c. Provide the FMC with a point of contact who is assigned by the Contractor, primarily responsible for managing community MedPAR/referrals within the IHSC MPv2S and who primarily is responsible for the following: 1) Sends completed recruitment letters to the FMC for potential enrollment in the IHSC…"

**`75N98026Q00962`** — 3 fragment(s) removed.

- p.29: "(2) The Contractor asserts its right to the adjustment within 30 days after the end of the period of work stoppage; provided, that, if the Contracting Officer decides the facts justify the action, the Contracting Officer may receive and act upon the claim submitted at any time before final payment u…"
- p.29: "If a stop-work order issued under this clause is canceled or the period of the order or any extension thereof expires, the Contractor shall resume work."
- p.29: "the Contractor shall be liable to the Government for any and all rights and remedies provided by law."

**`W15P7T-26-R-A006`** — 13 fragment(s) removed.

- p.262: "If in its annual representations and certifications in SAM the Offeror has represented in paragraph (c) of the provision at 252.204-7016, Covered Defense Telecommunications Equipment or Services-Representation, that it "does" provide covered defense telecommunications equipment or services as a part…"
- p.168: "The Contractor agrees to begin promptly negotiating with the Contracting Officer the terms of a definitive contract that will include- (1) All clauses required by the Federal Acquisition Regulation (FAR) on the date of execution of the undefinitized contract action; (2) All clauses required by law o…"
- p.262: "The Offeror shall review the list of excluded parties in the System for Award Management (SAM) at https://www.sam.gov for entities that are excluded when providing any equipment, system, or service to carry out covered missions that uses covered defense telecommunications equipment or services as a …"
- p.26: "The Contractor shall ensure a payment request includes documentation appropriate to the type of payment request in accordance with the payment clause, contract financing clause, or Federal Acquisition Regulation 52.216-7, Allowable Cost and Payment, as applicable."
- p.100: "In a joint venture comprised of a small business protégé and its mentor approved by the Small Business Administration, the small business protégé shall perform at least 40 percent of the work performed by the joint venture."
- p.303: "If the Offeror answered in the affirmative in paragraphs (c)(1) and (2) of this provision, then the Offeror shall submit its conflict-of-interest mitigation plan to the Contracting Officer for approval."
- p.135: "The Contractor shall provide the Government with one complete set of rates, terms, and conditions of service which are in effect as of the date of this contract and any subsequently approved rates."
- p.24: "The Contractor shall use the information in the Routing Data Table below only to fill in applicable fields in WAWF when creating payment requests and receiving reports in the system."
- p.337: "For QPs where the Offeror is in an approved SBA Mentor-Protege JV, passthrough rate shall be calculated on subcontracted efforts to firms outside of the JV members only."
- p.100: "In an 8(a) joint venture, the 8(a) participant(s) shall perform at least 40 percent of the work performed by the joint venture."

**`W911SG27BA002`** — 7 fragment(s) removed.

- p.61: "In a joint venture comprised of a small business protege and its mentor approved by the Small Business Administration, the small business protege shall perform at least 40 percent of the work performed by the joint venture. Work performed by the small business protege in the joint venture must be mo…"
- p.61: "In an 8(a) joint venture, the 8(a) participant(s) shall perform at least 40 percent of the work performed by the joint venture. Work performed by the 8(a) participants in the joint venture must be more than administrative functions."
- p.49: "When the end item being acquired is a kit of supplies, at least 50 percent of the total cost of the components of the kit shall be manufactured, processed, or produced by small businesses in the United States or its outlying areas."
- p.138: "Failure by the Contractor to submit close-out deliverable package IAW these requirements shall result in withholding from progress payments ten (10) percent of the progress payment."
- p.40: "(1) The offeror represents as part of its offer that- (i) it is, is not a small business concern; or"
- p.31: "General Decision Number: TX20260293 05/18/2026"
- p.50: "FAR Clauses Incorporated by Reference"

**`W912P726RA022`** — 7 fragment(s) removed.

- p.50: "The Offeror has completed the annual representations and certifications electronically via the SAM website at https://www.sam.gov After reviewing the SAM database information, the Offeror verifies by submission of the offer that the representations and certifications currently posted electronically …"
- p.83: "(d) Omissions from the drawings or specifications or the misdescription of details of work that are manifestly necessary to carry out the intent of the drawings and specifications, or that are customarily performed, shall not relieve the Contractor from performing such omitted or misdescribed detail…"
- p.30: "The Offeror shall provide immediate written notice to the Contracting Officer if, at any time prior to contract award, the Offeror learns that its certification was erroneous when submitted or has become erroneous by reason of changed circumstances."
- p.204: "Provide an interim ENG3394 report within 24hrs. and with a final post investigation report 10 calendar day(s) of the accident per the EM385-1-1."
- p.16: "Failure to furnish a bid guarantee in the proper form and amount, by the time set for opening of bids, may be cause for rejection of the bid."
- p.83: "The Contractor shall perform such details as if fully and correctly set forth and described in the drawings and specifications."
- p.16: "The amount of the bid guarantee shall be twenty (20) percent of the bid price or $3,000,000.00, whichever is less."

**`W912P825BA029`** — 3 fragment(s) removed.

- p.171: "The following chart shows the minimum production rates the dredge must perform at as specified in Section 35 20 23.23, paragraph entitled "Required Production and Dredging Tolerances"."
- p.241: "This computer must meet or exceed the following performance specifications:"
- p.211: "Disposal of dredged material shall be at the Government's direction."


### Running headers and footers stripped

A line recurring at the top or bottom of at least half the pages, digits wildcarded, is
document furniture. `#` stands for any run of digits.

**`0020153254COHEN`** — 63 lines, 1 page-break joins.

- `enterprise infrastructure and operations directorate (eiod)` — 13 lines
- `enterprise wireless communications branch (ewcb)` — 13 lines
- `cothen technology refresh` — 13 lines
- `office of information and technology (oit) #` — 12 lines
- `pr# #-#` — 12 lines

**`1240LT26Q0172`** — 137 lines, 6 page-break joins.

- `mark twain national forest` — 69 lines
- `council bluff primary electric and well improvements` — 68 lines

**`15F06726R0000194`** — 75 lines, 8 page-break joins.

- `#f#r# page # of #` — 75 lines

**`1616-26`** — 95 lines, 30 page-break joins.

- `order number: page # of` — 95 lines

**`19C02026Q0027`** — 42 lines, 8 page-break joins.

- `#` — 42 lines

**`36C26126Q1034`** — 144 lines, 11 page-break joins.

- `#c#q#` — 72 lines
- `page # of #` — 72 lines

**`75N98026Q00962`** — 25 lines, 3 page-break joins.

- `page # of #` — 25 lines

**`80JSC026MEDEVAC5Q`** — 44 lines, 2 page-break joins.

- `page # of #` — 22 lines
- `#jsc#r#` — 22 lines

**`W15P7T-26-R-A006`** — 682 lines, 77 page-break joins.

- `w#p#t#ra#` — 341 lines
- `page # of #` — 341 lines

**`W911SG27BA002`** — 310 lines, 36 page-break joins.

- `w#sg#ba#` — 155 lines
- `page # of #` — 155 lines

**`W912P726RA022`** — 1224 lines, 80 page-break joins.

- `w#p#ra#` — 637 lines
- `page # of #` — 587 lines

**`W912P825BA029`** — 159 lines, 58 page-break joins.

- `miss r, new orleans harbor and various bar channels cutterhead om#` — 159 lines

### The reviewer-finding table

The owner's outside reviewers audited five of these documents and quoted every
requirement they found missing - 69 sentences in all. Each was checked against this
evaluation by `scripts/reviewer-check.mjs`, which requires the quote to appear inside a
SINGLE row, on word boundaries, on the page the reviewer cited (one page of slack for a
sentence cut by a page break). None was omitted.

| Document | quoted misses | now captured | still missed |
|---|---:|---:|---:|
| `0020153254COHEN` | 14 | 12 | 2 |
| `1240LT26Q0172` | 17 | **17** | 0 |
| `1616-26` | 8 | 6 | 2 |
| `75N98026Q00962` | 6 | 5 | 1 |
| `80JSC026MEDEVAC5Q` | 24 | 17 | 7 |
| **All five** | **69** | **57** | **12** |

The full row-by-row table, with each quote and the capturing row's page, is at
`corpus/eval-v4/REVIEWER_FINDINGS_v4.txt`.

All twelve still missed are present in the extracted page text and absent from every
row, so they are recall misses rather than text the pipeline could not read. Five of
them (items c-g of the Medical Facility Support Plan on 80JSC026MEDEVAC5Q p.8) are one
list that was missed while the near-identical list beside it on the same page was
captured in full.

### Section-label formats used, per document

Read from each document's own headings before extraction, and enforced afterwards: an
unsupported `PWS`/`SOW` prefix is stripped and an unresolvable clause number is blanked.

| Document | prefixes the document supports | specification sections | UCF sections | clause headings |
|---|---|---|---:|---:|
| `0020153254COHEN` | SOW | — | 0 | 0 |
| `1240LT26Q0172` | **none** | 011000, 012700, 012800, 013100, 013250, 013300, … | 0 | 2 |
| `15F06726R0000194` | PWS | — | 12 | 74 |
| `1616-26` | PWS, SOW | — | 13 | 15 |
| `19C02026Q0027` | SOW | — | 12 | 75 |
| `36C26126Q1034` | PWS, SOW | — | 4 | 36 |
| `70CDCR26R00000026` | PWS, SOW | — | 0 | 1 |
| `75N98026Q00962` | SOW | — | 0 | 2 |
| `80JSC026MEDEVAC5Q` | SOW | — | 12 | 17 |
| `W15P7T-26-R-A006` | PWS, SOW | — | 12 | 151 |
| `W911SG27BA002` | SOW | — | 0 | 97 |
| `W912P726RA022` | SOW | — | 0 | 121 |
| `W912P825BA029` | **none** | — | 0 | 142 |

| The six exit criteria, verbatim | `MASTER_PLAN.md`, Phase 1 |
| The audit rubric | `EXTRACTION_PROMPT_SPEC.md` §7 |
| The verdict slot | `DECISIONS.md` D-010 — still empty |


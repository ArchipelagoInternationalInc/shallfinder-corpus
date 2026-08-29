# EVAL_REPORT v2 — Phase 1 corpus evaluation

Produced 2026-08-29. Supersedes v1 of 2026-08-29.

**This report presents evidence. It does not reach a verdict.** Section 5 lays out what
the numbers show against each of the six exit criteria in `MASTER_PLAN.md` without
declaring pass or fail. That ruling is the owner's and is recorded as D-010.

## What changed from v1

v1 was produced from a run in which **47 of 263 chunks returned nothing**, because their
responses were cut off at the model's output ceiling and the batch path had no retry. The
owner ruled that this is a mechanical defect in the pipeline, not a finding about
extraction quality, and that the matrix should be patched rather than re-run.

**The fix.** A response cut off at the ceiling now causes the chunk to be split in half at
a page boundary and both halves to be run, repeating until the halves stop truncating or
reach a configured floor. The ceiling was not raised — raising it only moves the cliff.

**The patch.** 48 chunks were resubmitted, pre-split, over 3 rounds. 
2 newly flagged page(s) were re-swept. Nothing else was re-run. Cost: **$10.14**.

| | v1 | v2 | change |
|---|---:|---:|---:|
| Rows | 8,044 | **9,668** | **+1,624** |
| Chunks yielding nothing | 47 | **0** | −47 |
| Invention count (dropped, unlocatable) | 164 | 236 | +72 |
| Unresolved *as v1 reported it* | 353 | — | — |
| Unresolved *on v2's accounting* | 790 | **704** | **−86** |

### An accounting correction the owner must see

**v1's published unresolved figure of 353 was too flattering.** When crediting a swept
page, v1 counted both the rows the sweeper captured *and* the exclusions it stated. But
captured rows already reduce that page's shortfall, so counting them again double-credited
the page and understated what remained unexplained.

v2 credits a swept page only with what the sweeper actually **explained** — the exclusions
it stated. Recomputing v1's own row set under that corrected rule gives **790**, not 353.

So the honest like-for-like comparison is **790 → 704**: the patch reduced unresolved by
86 while adding 1,624 rows. The apparent jump from 353 is the accounting fix, not a regression.

## Run parameters

| | |
|---|---|
| Model | `claude-sonnet-5` (from `EXTRACTION_MODEL`; nothing in code selects a model) |
| Pricing | Batch API, 50% of list. Rates verified 2026-08-29 at platform.claude.com/docs/en/about-claude/pricing |
| Patch batches | msgbatch_01THkdv2WLXHF4XbnKUNAjxh, msgbatch_01Eu2eDpLJ6waa6eEUFL71aG, msgbatch_01DB5fLKdtsijNVz6qWk48y3, msgbatch_01JmqvM4XhWAE4gouA5odrUN |
| Random-sample seed | **20260829** — unchanged from v1, so the 10-row samples are directly comparable |
| Documents | 17 (14 readable, 3 unreadable, 0 failed) |
| Readable pages | 2,032 |
| **Total cost** | **$53.58** — $43.44 for the v1 corpus run plus $10.14 for the patch |

## 1. Summary table — every document

| Document | Status | Pages | Rows (v1 → v2) | Dup rate | Dropped (invented) | Review flags | Unresolved before → after | Cost |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-` | ok | 13 | 40 → **40** | 19.6% | 1 (1) | 17.5% | 4 → **4** | $0.13 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI202` | ok | 74 | 263 → **621** (+358) | 3.4% | 6 (6) | 12.9% | 12 → **8** | $1.34 |
| `15F06726R0000194__RFP-15F06726R0000194-T` | ok | 75 | 385 → **427** (+42) | 7.8% | 10 (10) | 19.0% | 52 → **30** | $1.94 |
| `1616-26__RFP1620000348` | ok | 104 | 353 → **385** (+32) | 4.1% | 6 (6) | 27.3% | 38 → **24** | $1.99 |
| `19C02026Q0027__Solicitation-19C02026Q002` | ok | 81 | 365 → **587** (+222) | 1.6% | 12 (12) | 21.6% | 53 → **37** | $2.28 |
| `36C26026Q0939__SF-1449-36C26026Q0939-Sto` | **UNREADABLE** | 39 | — | — | — | — | — | $0.00 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-` | ok | 73 | 255 → **255** | 8.8% | 15 (14) | 20.0% | 51 → **31** | $1.39 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | ok | 8 | 5 → **19** (+14) | 0.0% | 51 (50) | 26.3% | 4 → **2** | $0.34 |
| `70CDCR26R00000026__Attachment-01-Turnkey` | ok | 152 | 1316 → **1674** (+358) | 4.9% | 28 (19) | 20.5% | 116 → **75** | $6.62 |
| `75N98026Q00962__RFQ-75N98026Q00962` | ok | 39 | 159 → **159** | 1.6% | 0 (0) | 32.1% | 46 → **16** | $1.11 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JS` | ok | 23 | 132 → **132** | 2.3% | 2 (2) | 16.7% | 11 → **9** | $0.50 |
| `PANMCC26P0000048766__Combined-Synopsis` | **UNREADABLE** | 33 | — | — | — | — | — | $0.00 |
| `W15P7T-26-R-A006__Solicitation-Amendment` | ok | 342 | 945 → **1008** (+62) | 10.2% | 44 (42) | 26.8% | 236 → **146** | $6.27 |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicita` | **UNREADABLE** | 144 | — | — | — | — | — | $0.00 |
| `W911SG27BA002__Solicitation-Amendment-W9` | ok | 157 | 781 → **858** (+77) | 3.8% | 29 (27) | 25.1% | 101 → **57** | $4.17 |
| `W912P726RA022__W912P726RA002-San-Rafael-` | ok | 645 | 1712 → **2152** (+440) | 5.2% | 28 (23) | 24.9% | 283 → **150** | $9.10 |
| `W912P825BA029__Solicitation-W912P825BA02` | ok | 246 | 1333 → **1351** (+18) | 5.3% | 36 (24) | 16.8% | 193 → **115** | $6.26 |

**Totals across readable documents:** 9,668 rows · 236 dropped as unlocatable · unresolved 704. **No chunk yielded nothing.**

**Reported, not hidden:** 3 chunk(s) truncated and could not be split further (a single page, or at the floor). Their responses were still parsed where possible; where not, the loss is counted above.

## 2. The unreadable documents

In the corpus on purpose. v1 does not OCR (DECISIONS.md D-004), so the correct behaviour is
to declare them unreadable and charge nothing. They were not skipped.

| Document | Pages | What the user would be told |
|---|---:|---|
| `36C26026Q0939__SF-1449-36C26026Q0939-Sto` | 39 | This PDF appears to be scanned images rather than text: 39 of 39 pages carry almost no extracta |
| `PANMCC26P0000048766__Combined-Synopsis` | 33 | This PDF appears to be scanned images rather than text: 33 of 33 pages carry almost no extracta |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicita` | 144 | This PDF appears to be scanned images rather than text: 143 of 144 pages carry almost no extrac |

## 3. THE AUDIT PACKET

Everything needed for the hand audit in EXTRACTION_PROMPT_SPEC §7, without asking anyone.

**How to read a source page.** From the repo root:

```bash
node scripts/extract.ts <corpus-path> --page <N>
```

No model call, no cost.

**The two full-read pages are deliberately not chosen here.** Criterion 1 asks whether the
pipeline misses what a careful reader catches. If this report picked the pages, the sample
would be steerable and the check worthless. **Pick any two pages per document yourself.**

### 0020153254COHEN__Attachment-1-Statement-of-Work-RFI

Path: `corpus/0020153254COHEN__Attachment-1-Statement-of-Work-RFI.pdf`

**13 pages · 40 rows · unresolved 4 · cost $0.13**
Rows dropped because their quote could not be found: **1**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 5 | SOW 4.1 | must | **review** | • Compliance with MIL-STD-188-110D and MIL-STD-188-141D • Interoperability with the existing COTHEN High Frequency Cellular Network, including compatibility with Collins Aerospace software and protocols currently in use • All othe |
| 2 | 5 | SOW 4.2 | shall | high | The Contractor shall test and functionally certify all equipment prior to delivery to ensure compliance and specifications. |
| 3 | 5 | SOW 5 | shall | high | The Contractor shall deliver equipment as outlined below. |
| 4 | 7 | SOW 8 | declaration | high | The period of performance is Date of Award (DOA) for Six Months. |
| 5 | 9 | SOW 10.4 | shall | high | The acceptance of any changes by the contractor without specific approval and written consent of the CO shall be at the contractor’s risk. |
| 6 | 9 | SOW 10.5 | shall | high | Payment requests shall be submitted electronically through the U. S. Department of the Treasury's Invoice Processing Platform System (IPP). |
| 7 | 11 | Addendum B | shall | high | The Offeror shall ensure that the design conforms to the Department of Homeland Security (DHS) and Customs and Border Protection (CBP) Enterprise Architecture (EA), the DHS and CBP Technical Reference Models (TRM), and all DHS and |
| 8 | 12 | Addendum B | shall | high | Compliance with the HLS EA shall be derived from and aligned through the CBP EA. |
| 9 | 12 | Addendum B | shall | high | Applicability of Internet Protocol version 6 (IPv6) to DHS-related components (networks, infrastructure, and applications) specific to individual acquisitions shall be in accordance with the DHS EA (per OMB Memorandum M-05-22, Aug |
| 10 | 13 | *(none)* | must | high | All hardware, software, and services provided under this task order must be compliant with DHS 4300A DHS Sensitive System Policy and the DHS 4300A Sensitive Directive Handbook. |

#### The 3 flagged pages with the largest shortfall

*No pages are flagged on this document.*

### 1240LT26Q0172__3-Combined-SpecsCBPEWI20260624

Path: `corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf`

**74 pages · 621 rows (+358 recovered by the patch) · unresolved 8 · cost $1.34**
Rows dropped because their quote could not be found: **6**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 20 | 013300 1.4.A | imperative | high | Mark each copy to show specific product choices and options applicable to the project. |
| 2 | 36 | 260010 1.5.B | imperative | high | Modify wiring and location, provide additional materials and work as required for proper installation in accordance with NEC or manufacturer's instructions. |
| 3 | 41 | 3.7.C | imperative | high | Mount labels with machine screws, except where screw penetration will injure equipment, contact or epoxy cement or ty-raps may be used. |
| 4 | 42 | 3.12.C | imperative | high | Provide stainless steel material for corrosive or outdoor locations. |
| 5 | 44 | PWS 3.17.E.1 | imperative | high | Coordinate testing with COR so that tests can be witnessed if desired. |
| 6 | 48 | 260050 2.2.B | imperative | high | Provide removable screw cover on the largest access side of the box unless otherwise detailed. |
| 7 | 49 | 260050 2.4.B | shall | high | Engraving shall be filled with black enamel for ivory plates, white enamel for brown plates and ivory plates filled with orange paint for isolated ground receptacle. |
| 8 | 56 | PWS 3.3.A.4 | shall | high | Bored coilable duct shall end 4’-5’ from a new vault or manhole. |
| 9 | 56 | 260400 3.3 | imperative | high | Prior to pulling cable thru conduits clean all conduits by swabbing out conduits to remove all debris. |
| 10 | 62 | PWS 329200 3.2.E | imperative | high | Grade topsoil to eliminate rough, low or soft areas and to ensure positive drainage. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 30 | 16 | 13 | 3 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 13 | 3 | 1 | 2 | yes | 2 exclusion(s): definitional — resolved |
| 20 | 6 | 4 | 2 | yes | 1 exclusion(s): government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 30
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 13
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 20
```

### 15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer

Path: `corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf`

**75 pages · 427 rows (+42 recovered by the patch) · unresolved 30 · cost $1.94**
Rows dropped because their quote could not be found: **10**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 17 | C.7 | conditional | high | When identified in a delivery order, the Contractor shall provide, upfit, integrate, test, document, deliver, and warrant an associated prime hauler vehicle meeting the Technical Specifications. |
| 2 | 33 | H.15 | shall | high | Administrative credentials, license keys, configuration files, source configuration data, recovery media, and required registration information shall be transferred using a Government-approved secure method. |
| 3 | 34 | H.22 | shall | high | The Contractor shall not invoice such costs separately. |
| 4 | 34 | H.24 | shall | high | Notice shall include the affected requirement and order, current impact, root cause if known, mitigation, recovery plan, decisions needed, and date by which Government action is required. |
| 5 | 42 | *(none)* | shall | **review** | Delivery or performance shall be made only as authorized by orders issued in accordance with the Ordering clause. |
| 6 | 43 | 2852.201-70 | shall | **review** | Failure of the Contractor and Contracting Officer to agree that technical direction is within the scope of the contract is a dispute that shall be subject to the ‘‘Disputes’’ clause and/or other similar contract term. |
| 7 | 58 | L.5.3 | imperative | **review** | Describe the vehicle-specific quality and verification approach, including: • Development and maintenance of the vehicle-specific quality plan; • In-process inspection; |
| 8 | 62 | L.7 | imperative | high | Submit a completed Attachment 4 – Price Template in native Microsoft Excel format. |
| 9 | 62 | L.8 | shall | high | Any exception, qualification, deviation, assumption, substitution, proprietary restriction, recurring charge, or conditional term shall be disclosed in the Volume I Exceptions, Qualifications, Deviations, and Assumptions Matrix an |
| 10 | 63 | L.9 | must | high | Questions must be submitted using Attachment 5 – Questions Template to the Contracting Officer identified as the SAM.gov point of contact. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 21 | 15 | 9 | 6 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 23 | 6 | 2 | 4 | yes | 4 exclusion(s): definitional, government-actor — resolved |
| 24 | 12 | 8 | 4 | yes | 2 exclusion(s): government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 21
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 23
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 24
```

### 1616-26__RFP1620000348

Path: `corpus/1616-26__RFP1620000348.pdf`

**104 pages · 385 rows (+32 recovered by the patch) · unresolved 24 · cost $1.99**
Rows dropped because their quote could not be found: **6**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 38 | Section C | must | high | For receive/deliver items, any concealed carrier damage found when unpackaged at site must be photographed prior to installing the product in the designated space at site. |
| 2 | 42 | Pricing | shall | **review** | This shall be used as a general way to determine the length of a project. |
| 3 | 43 | *(none)* | shall | **review** | UNICOR shall not be invoiced for installation services until the "Acceptance Form" is attached to the invoice. |
| 4 | 45 | *(none)* | shall | high | Current rules and regulations applicable to the premises, where the work will be performed, shall apply to the contractor and its employees while working on the premises. |
| 5 | 47 | *(none)* | must | high | All staff employed directly under the offeror (s) must complete and pass an NCIC check which will be sent to UNICOR. |
| 6 | 51 | *(none)* | shall | high | The installation site manager shall ensure all personnel are on-site and following all agency rules |
| 7 | 53 | Section F | shall | high | Order confirmation shall be signed and dated in blocks 30a, b and c of the delivery order |
| 8 | 53 | Section F | must | high | Order confirmation containing the following information must be faxed or emailed to the contracting officer or their designee. |
| 9 | 87 | *(none)* | declaration | **review** | The Contractor should maintain signed copies of the NDA for all employees as a record of compliance. |
| 10 | 89 | C.(1)(a) | will | **review** | This training will be provided at the outset of the Subcontractor's/employee's work on the contract and every year thereafter. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 82 | 8 | 3 | 5 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 61 | 7 | 3 | 4 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 83 | 5 | 2 | 3 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 82
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 61
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 83
```

### 19C02026Q0027__Solicitation-19C02026Q0027

Path: `corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf`

**81 pages · 587 rows (+222 recovered by the patch) · unresolved 37 · cost $2.28**
Rows dropped because their quote could not be found: **12**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 14 | G.1.1 | shall | high | G.1.1 The Contractor shall provide the information required by the paragraph above within ten (10) calendar days after award. |
| 2 | 17 | G.5.2 | declaration | high | Failure to provide any of the above information may be considered grounds for rejection and/or resubmittal of the application. |
| 3 | 26 | 52.240-91(b)(3) | must | **review** | (3) Covered telecommunications equipment or services used as a substantial or essential component of any system, or as critical technology as part of any system (paragraphs (a)(1)(A) of section 889 of the John S. McCain National D |
| 4 | 27 | 52.240-91(d)(1)(i) | conditional | **review** | Unless an applicable waiver has been issued by the Government, the Contractor cannot use any equipment, systems, or services that uses covered telecommunications equipment or services as a substantial or essential component of any |
| 5 | 30 | *(none)* | shall | high | The Contractor shall report the following information within 72 hours for each covered article or each product or service produced or provided by a source, where the covered article or source is subject to a FASCSA order: |
| 6 | 33 | 652.236-70 | shall | **review** | The mishap reporting requirement shall include fires, explosions, hazardous materials contamination, and other similar incidents that may threaten people, property, and equipment. |
| 7 | 54 | SOW 6.5 | imperative | high | Fabricate, erect, and anchor structural steel extensions to cover previously exposed zones, the main entry, the rear service area, and the specific front expansion zone measuring 4.00 m by 2.00 m (aligned with the shoe store inter |
| 8 | 63 | Final Acceptance & Closeout | shall | high | The Contractor shall request a final inspection only when all work, including the 'Punch List' repairs, is fully completed. |
| 9 | 66 | Safety Requirements §3a | must | high | Contractor personnel must use personal protective equipment (PPE) required and in accordance with the contracted work. |
| 10 | 76 | 8 | must | high | Todo el personal que realice trabajo eléctrico deberá estar lo suficientemente entrenado y se debe ser una persona competente para poder ejecutar el trabajo. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 29 | 11 | 2 | 9 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 30 | 9 | 4 | 5 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 43 | 5 | 0 | 5 | yes | 1 exclusion(s): government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 29
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 30
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 43
```

### 36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-

Path: `corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf`

**73 pages · 255 rows · unresolved 31 · cost $1.39**
Rows dropped because their quote could not be found: **14**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 6 | SOW 2 | must | high | Must meet ASTM F137 and ASTM F1515 – Flexibility Excellent/ Light Stability ∆E ≤ 8 |
| 2 | 7 | SOW 2 | declaration | **review** | -Compatible with concrete moisture testing per ASTM F1869 and ASTM F2170. |
| 3 | 10 | 9.C.3 | shall | high | In accordance with 36 CFR 1222.32, Contractor shall maintain all records created for Government use or created in the course of performing the contract and/or delivered to, or under the legal control of the Government and must be  |
| 4 | 36 | Addendum to 52.212-4 | declaration | high | The following clauses are incorporated into 52.212-4 as an addendum to this contract: |
| 5 | 42 | C.9(e)(2) | will | high | it will not pay more than 50 percent of the amount paid by the Government for contract performance, excluding the cost of materials, to subcontractors that are not similarly situated entities. |
| 6 | 44 | C.10(c) | shall | high | The Contractor shall rerepresent its size status in accordance with the size standard in effect at the time of this rerepresentation that corresponds to the North American Industry Classification System (NAICS) code(s) assigned to |
| 7 | 53 | *(none)* | conditional | high | Unless an exception applies according to paragraph (d)(4)(iii) or the Government grants a waiver, contractor shall not export certain sensitive technology to Iran, as determined by the President, and has an active exclusion in SAM |
| 8 | 64 | E.4(c)(1)(ii) | declaration | high | (ii) It [ ] is, [ ] is not a small business joint venture that complies with the requirements of 13 CFR 121.103(h) and 13 CFR 125.8(a) and (b). |
| 9 | 64 | E.4(c)(3) | declaration | high | (3) Women-owned small business (WOSB) joint venture eligible under the WOSB Program. The offeror represents as part of its offer that it [ ] is, [ ] is not a joint venture that complies with the requirements of 13 CFR 127.506(a) t |
| 10 | 66 | E.5(d)(2) | conditional | high | (2) If the Offeror indicates "is" in paragraph (d)(1) of this provision, then the Offeror represents that—I am claiming on the IRS Form W-14 □ a full exemption, or □ partial or no exemption [Offeror shall select one] from the exci |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 55 | 12 | 2 | 10 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 33 | 5 | 1 | 4 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 34 | 8 | 4 | 4 | yes | 5 exclusion(s): government-actor, definitional — resolved |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 55
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 33
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 34
```

### 47QMCA26Q0098__RFQ47QMCA26Q0098-SF18

Path: `corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf`

**8 pages · 19 rows (+14 recovered by the patch) · unresolved 2 · cost $0.34**
Rows dropped because their quote could not be found: **50**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | *(none)* | imperative | high | Please complete SF18: Block 8 a,b,c,d,e & f Block 13 a,b,c,d,e,& f Block 14 Block 15 Block 16 a, b, and c **Follow instructions for submission** |
| 2 | 2 | B.1 | will | **review** | Partial minimum quoted quantities will NOT be evaluated. |
| 3 | 2 | B.1 | declaration | high | DELIVERY DATES ARE A SIGNIFICANT EVALUATION FACTOR (i.e., technical factor). |
| 4 | 3 | B.2 | must | high | PRICING MUST BE FOB DESTINATION. |
| 5 | 6 | 52.212-2 | imperative | high | Limit 20 Pages. |
| 6 | 7 | PRICE | shall | high | Offerors shall not assume that the Government will award sixty (60) vehicles for purposes of pricing, and the Government will not be obligated to purchase quantities exceeding the minimum quantity stated herein. |
| 7 | 8 | E.1.2 | required | high | Vendors are required to complete the GSA Source of Supply Letter. |
| 8 | 8 | E.1 | declaration | high | If any of the items are not provided and filled out in their entirety, then the quote will be disqualified and not evaulated |
| 9 | 8 | E.1 | imperative | **review** | Complete and submit͘ |
| 10 | 8 | E.1 | conditional | high | If the vendor is not the manufacturer of the products being offered, the vendor may only offer products it is authorized to distribute, either by the manufacturer itself, or as otherwise authorized pursuant to wholesaler agreement |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 2 | 3 | 1 | 2 | yes | 3 exclusion(s): government-actor — resolved |
| 8 | 3 | 1 | 2 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf --page 2
node scripts/extract.ts ../shallfinder-corpus/corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf --page 8
```

### 70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem

Path: `corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf`

**152 pages · 1674 rows (+358 recovered by the patch) · unresolved 75 · cost $6.62**
Rows dropped because their quote could not be found: **19**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 11 | PWS 12 | shall | high | The emergency plan shall include provisions for two or more disturbance control teams consisting of at least 12 people on each team. |
| 2 | 14 | *(none)* | shall | high | The transporting officers shall comply with all local, State and Federal motor vehicle regulations (including DOT, Interstate Commerce Commission, and Environmental Protection Agency), including, but not limited to: |
| 3 | 16 | 15 | declaration | **review** | it is expected that virtual attorney visitation will be made available for at least eight (8) hours each day on weekdays and four (4) hours per day on weekends and holidays. |
| 4 | 27 | 30. Evacuation Plan | shall | high | The contractor shall furnish emergency evacuation diagrams showing exit routes as well as 24-hour emergency evacuation procedures. |
| 5 | 46 | PWS i. Radiology Services | required | high | Additionally, the Contractor is financially responsible for all associated radiology staffing service costs, and diagnostic interpretation costs. |
| 6 | 54 | PWS 42 | shall | high | All EHR solutions and services shall meet DHS Enterprise Architecture policies, standards, and procedures. |
| 7 | 57 | PWS 43.4.C | imperative | high | Maintain office, warehouse, and clinical areas in clean, dust-free condition. |
| 8 | 81 | *(none)* | will | **review** | All facilities will be located within the United States. |
| 9 | 125 | PWS 9 | shall | high | The Contractor shall share all intelligence information with the COR and ICE officials. |
| 10 | 141 | Deliverables Chart | declaration | high | Holiday Menus Annually |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 11 | 22 | 17 | 5 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 21 | 14 | 9 | 5 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 6 | 9 | 5 | 4 | yes | 3 exclusion(s): definitional, government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 11
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 21
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 6
```

### 75N98026Q00962__RFQ-75N98026Q00962

Path: `corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf`

**39 pages · 159 rows · unresolved 16 · cost $1.11**
Rows dropped because their quote could not be found: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 3 | *(none)* | must | **review** | Responses must be submitted electronically to |
| 2 | 5 | SOW - Scope of Work | shall | high | Due to the critical nature of these units the contractor must ensure the following: 1. The contractor shall make commercially reasonable efforts to obtain any required part within the targeted 48 hours of the service technician as |
| 3 | 12 | Delivery or Deliverable | imperative | high | The service technician is to present a service ticket that briefly details the work performed to the POC for signature and a copy of the ticket is to be left with the POC. |
| 4 | 12 | Travel | declaration | **review** | All travel time should be included within the provided service agreement quote. |
| 5 | 13 | Section 508—Electronic and Information Technology Standards | conditional | **review** | If significant difficulty or expense is involved, a commercial non-availability is declared. |
| 6 | 18 | *(none)* | conditional | **review** | The Government may terminate this contract, or any part hereof, for cause in the event of any default by the Contractor, or if the Contractor fails to comply with any contract terms and conditions, or fails to provide the Governme |
| 7 | 25 | *(none)* | shall | **review** | any hours expended and material costs incurred by the Contractor in excess of the ceiling price before the increase shall be allowable to the same extent as if the hours expended and material costs had been incurred after the incr |
| 8 | 27 | *(none)* | shall | **review** | Amounts shall be due at the earliest of the following dates: |
| 9 | 28 | (10) | shall | **review** | In connection with any discount offered for early payment, time shall be computed from the date of the invoice. |
| 10 | 35 | 52.212-5(e)(1)(viii) | conditional | high | If the subcontract (except subcontracts to small business concerns) exceeds the applicable threshold specified in FAR 19.702(a) on the date of subcontract award, the subcontractor must include 52.219-8 in lower tier subcontracts t |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 38 | 8 | 2 | 6 | yes | 5 exclusion(s): government-actor — **still unresolved** |
| 2 | 6 | 2 | 4 | yes | 4 exclusion(s): government-actor, definitional — resolved |
| 16 | 6 | 2 | 4 | yes | 6 exclusion(s): government-actor, definitional — resolved |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 38
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 2
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 16
```

### 80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final

Path: `corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf`

**23 pages · 132 rows · unresolved 9 · cost $0.50**
Rows dropped because their quote could not be found: **2**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | Block 28 | required | high | CONTRACTOR IS REQUIRED TO SIGN THIS DOCUMENT AND RETURN COPIES TO ISSUING OFFICE. CONTRACTOR AGREES TO FURNISH AND DELIVER ALL ITEMS SET FORTH OR OTHERWISE IDENTIFIED ABOVE AND ON ANY ADDITIONAL SHEETS SUBJECT TO THE TERMS AND CON |
| 2 | 5 | SOW 1.1.2 | shall | high | The Contractor shall provide services globally without exception. |
| 3 | 5 | SOW 1.1.2 | conditional | high | In the event that the Contractor cannot provide a medevac in specific parts of the world, then the Contractor shall provide sufficient rationale to the NASA COR why it cannot and provide an alternate solution at no additional cost |
| 4 | 5 | SOW 1.1.5 | shall | high | The Contractor shall have a documented Safety Management System (SMS) in accordance with International Civil Aviation Organization (ICAO), Federal, and industry standards. |
| 5 | 6 | SOW 2.1.1.a | imperative | high | Provide transportation of patient from site of injury to appropriate medical facilities; |
| 6 | 6 | SOW 2.1.1.n | imperative | high | Provide repatriation of mortal remains for travelers to the location specified by NASA; |
| 7 | 8 | SOW 2.3.5 | shall | high | The plan shall include at a minimum: |
| 8 | 9 | SOW 3.1.3 | shall | **review** | The crew member and the associated NASA flight surgeon shall be transported to Houston, Texas. |
| 9 | 19 | Section L | shall | high | Offeror’s responses shall include action plans; decision points; operational timelines; specific resources, personnel, equipment, transport and other assets used at each stage; travel route from origin of incident to hospital of c |
| 10 | 19 | Section L | shall | high | Offerors shall state in their response the specific SOW requirements the steps in their solution addresses. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 6 | 9 | 7 | 2 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 18 | 12 | 10 | 2 | yes | 2 exclusion(s): government-actor — resolved |
| 22 | 4 | 2 | 2 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 6
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 18
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 22
```

### W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006

Path: `corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf`

**342 pages · 1008 rows (+62 recovered by the patch) · unresolved 146 · cost $6.27**
Rows dropped because their quote could not be found: **42**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 96 | 52.216-22(b) | shall | **review** | Delivery or performance shall be made only as authorized by orders issued in accordance with the Ordering clause. |
| 2 | 107 | 52.219-... (HUBZone JV representation) | shall | **review** | [____The Contractor shall enter the name and unique entity identifier of each party to the joint venture: .] |
| 3 | 116 | 52.227-11(i)(5) | conditional | **review** | Allow the Secretary of Commerce to review the Contractor's licensing program and decisions regarding small business applicants, and negotiate changes to its licensing policies, procedures, or practices with the Secretary of Commer |
| 4 | 152 | 252.208-7999(a)(3) | imperative | high | (3) The completed address(es) to which the Contractor's mail, freight, and billing documents are to be directed. |
| 5 | 189 | *(none)* | shall | high | Except as provided in paragraph (l)(2)(ii) of this clause, the Contractor shall include this clause in any subcontract or contractual instrument under which technical data or computer software will be obtained from a subcontractor |
| 6 | 245 | 52.212-3(h) | conditional | high | (h) Certification Regarding Responsibility Matters (Executive Order 12689). (Applies only if the contract value is expected to exceed the simplified acquisition threshold.) The offeror certifies, to the best of its knowledge and b |
| 7 | 273 | L.1 | must | high | All supporting documentation submitted in response to this solicitation must be designated as Unclassified, up to and including Controlled Unclassified Information (CUI). |
| 8 | 274 | L.1 | shall | high | 6. All information the Offeror intends to have considered shall be submitted with the initial proposal. |
| 9 | 279 | L.2.1 | declaration | **review** | Signed SF33Proof of completed SAM registration. |
| 10 | 286 | L.2.2.2.1 | must | high | The letter must include the name, address, phone number, and email of the CPA and a copy of the signer's CPA Registration such as the printout from https://cpaverify.org/. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 289 | 7 | 0 | 7 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 133 | 7 | 1 | 6 | yes | 5 exclusion(s): government-actor, definitional — **still unresolved** |
| 287 | 8 | 3 | 5 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 289
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 133
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 287
```

### W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001

Path: `corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf`

**157 pages · 858 rows (+77 recovered by the patch) · unresolved 57 · cost $4.17**
Rows dropped because their quote could not be found: **27**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 12 | 00 21 13 / 2.3 | declaration | high | This solicitation is set-aside 100% for 8(a) Small Businesses. |
| 2 | 49 | 252.219-7010 | will | **review** | The will notify the Contracting____ [insert name of SBA's contractor] MICC Fort Bliss Officer in writing immediately upon entering an agreement (either oral or written) to transfer all or part of its stock or other ownership inter |
| 3 | 105 | PWS 1.18.2 | shall | **review** | The DPW PM shall be notified of all accidents within one (1) hour of the occurrence. |
| 4 | 116 | PWS 3.4 | shall | high | The Contractor shall provide and install a sign at the storage entrance identifying the Contractor and POC. |
| 5 | 121 | PWS 4.14.1 | will | high | No material other than construction materials, e.g. PVC, CPVC, or other suitable materials, will be brought into the installation. |
| 6 | 126 | PWS 4.35 | shall | high | The submittal name nomenclature shall be as follows: "Submittal # _Type of submittal_YYYY_MM_DD". |
| 7 | 133 | 4.48.12-4.48.15 | declaration | **review** | DFARS Clause 252.225-7040, Contractor Personnel Authorized to Accompany U.S. Armed Forces Deployed Outside the United States, shall be used in solicitations and contracts that authorize Contractor personnel to accompany U.S. Armed |
| 8 | 134 | 4.51.c | declaration | **review** | Duties, responsibilities, and authorities of each person in the QC organization. |
| 9 | 135 | 4.51.i | imperative | high | Include in the list of DFOWs, but not be limited to, all critical path activities. |
| 10 | 136 | PWS 4.53 | conditional | high | If the Contractor requires space in addition to or outside of the provided footprint, the Contractor shall submit a Staging and Laydown Plan with the applicable permit request documentation. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 76 | 5 | 1 | 4 | yes | 3 exclusion(s): government-actor — **still unresolved** |
| 77 | 11 | 7 | 4 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 78 | 7 | 3 | 4 | yes | 2 exclusion(s): government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 76
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 77
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 78
```

### W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-

Path: `corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf`

**645 pages · 2152 rows (+440 recovered by the patch) · unresolved 150 · cost $9.10**
Rows dropped because their quote could not be found: **23**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 3 | BIDDER QUESTIONS | must | high | INQUIRIES MUST BE RECEIVED NO LATER THAN 14 CALENDAR DAYS PRIOR TO THE DATE SET FOR BID OPENING. |
| 2 | 73 | 52.232-5(b)(1) | shall | high | The Contractor's request for progress payments shall include the following substantiation: |
| 3 | 80 | 52.248-3(h) | shall | high | The Contractor shall include an appropriate value engineering clause in any subcontract of $90,000 or more and may include one in subcontracts of lesser value. |
| 4 | 165 | 3.4 | imperative | high | Provide the submissions as described below. |
| 5 | 168 | 3.7.4 | must | high | The proposed fragnet must be approved by the Contracting Officer or delegated representative prior to incorporation into the project schedule. |
| 6 | 206 | PWS 1.14 | shall | high | All personnel in an area that is not protected by handrails shall wear either a PLB or a fall protection system that meets all the requirements of EM 385-1-1. |
| 7 | 250 | 1.2.10 | imperative | **review** | As a solid waste, perform a hazardous waste determination prior to disposal. |
| 8 | 281 | PWS 3.2.2 | conditional | **review** | In the event a reported parameter is calculated based on multiple sensors, the sensor values as used in the equation must be visible in addition to the required parameter. |
| 9 | 364 | *(none)* | shall | high | The drag head, cutterheads, and pipeline intakes shall remain in contact with the seafloor during suction dredging. |
| 10 | 636 | Special Condition 11 | shall | high | These reports shall describe the cause(s) of the problems, any steps taken to rectify the problems, and whether the problems occurred on subsequent disposal trips. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 122 | 9 | 0 | 9 | yes | 9 exclusion(s): definitional — resolved |
| 124 | 8 | 1 | 7 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 130 | 7 | 0 | 7 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 122
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 124
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 130
```

### W912P825BA029__Solicitation-W912P825BA029-OM25035

Path: `corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf`

**246 pages · 1351 rows (+18 recovered by the patch) · unresolved 115 · cost $6.26**
Rows dropped because their quote could not be found: **24**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 37 | Section 00 45 00 | declaration | **review** | (1) The following representations or certifications in the SAM database are applicable to this solicitation as indicated: |
| 2 | 48 | (c)(1)(ii) | conditional | high | A request based on unreasonable cost shall include a reasonable survey of the market and a completed price comparison table in the format in paragraph (d) of this clause. |
| 3 | 63 | 252.232-7006 | shall | high | (1) Document type. The Contractor shall submit payment requests using the following document type(s): |
| 4 | 72 | Section 00800, 3.4 | imperative | **review** | (d)(2)(i)(B) In accordance with FAR 31.105(d)(2)(i)(b), for the predetermined schedule of construction equipment use rates, use Engineer Pamphlet (EP) 1110-1-8, Construction Equipment Ownership and Operating Expense Schedule. |
| 5 | 80 | 1.12 | conditional | high | If the Contractor worked in multiple dredging regions, the Contractor shall fill out a separate utility summary sheet for each dredging region. |
| 6 | 126 | PWS 3.1.8 | shall | **review** | which shall be noted on each page of the book. |
| 7 | 137 | 1.3.1 | shall | high | The Contractor shall prepare and submit a Daily Report of Operations (MVN Form 4267) and the Original Leverman's Log signed by each shift leverman, for each dredge working. |
| 8 | 143 | 2.1.4.e | conditional | **review** | Furthermore, the Contractor may be required to disconnect all or part of his pipeline to allow the passage of other vessels, if the pipeline would otherwise be a hazard to navigation. |
| 9 | 147 | 2.2.4 | shall | high | The dredge shall be equipped with an Automatic Identification System (AIS) in accordance with the Code of Federal Regulations, 33 CFR 164, reference the note to 164.46(a). |
| 10 | 238 | 3.1.1.3.4.3 | shall | high | The total length of shore pipe shall be reported with the tag "length_land". |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | Captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 75 | 15 | 9 | 6 | yes | 1 exclusion(s): government-actor — **still unresolved** |
| 98 | 25 | 19 | 6 | yes | 1 exclusion(s): definitional — **still unresolved** |
| 143 | 21 | 15 | 6 | yes | 1 exclusion(s): government-actor — **still unresolved** |

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 75
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 98
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 143
```

## 4. The invention count, per document

A row is dropped when its quoted text cannot be located anywhere in the document. This is
the structural defence against a fabricated citation. A dropped row never reaches a user.

| Document | Rows kept | Dropped: not found (v1 → v2) | Rate |
|---|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of` | 40 | 1 → **1** | 2.5% |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260` | 621 | 3 → **6** | 1.0% |
| `15F06726R0000194__RFP-15F06726R0000194-Tie` | 427 | 10 → **10** | 2.3% |
| `1616-26__RFP1620000348` | 385 | 4 → **6** | 1.6% |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 587 | 8 → **12** | 2.0% |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VA` | 255 | 14 → **14** | 5.5% |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | 19 | 9 → **50** | 263.2% |
| `70CDCR26R00000026__Attachment-01-Turnkey-F` | 1674 | 13 → **19** | 1.1% |
| `75N98026Q00962__RFQ-75N98026Q00962` | 159 | 0 → **0** | 0.0% |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC0` | 132 | 2 → **2** | 1.5% |
| `W15P7T-26-R-A006__Solicitation-Amendment-0` | 1008 | 39 → **42** | 4.2% |
| `W911SG27BA002__Solicitation-Amendment-W911` | 858 | 22 → **27** | 3.1% |
| `W912P726RA022__W912P726RA002-San-Rafael-So` | 2152 | 21 → **23** | 1.1% |
| `W912P825BA029__Solicitation-W912P825BA029-` | 1351 | 18 → **24** | 1.8% |

**Total dropped as unlocatable: 236** (v1: 164).

## 5. The six exit criteria — what the numbers show

Ranked by how much of the decision rests on each. **No verdict is offered here.**

### 1. Catches everything (the recall check) — the criterion the project turns on

The owner reads two pages per readable document by hand. **This report does not choose them.**

- The independent scan found **6842** binding-verb occurrences across readable documents.
- After the sweeper, **704** remain neither captured nor explained.
- The patch recovered **1,624** rows that the truncation defect had silently lost. **No chunk now yields nothing.**
- The scan sees Class-A verbs only. Imperatives, conditionals and declarations carry no magic
  word and are invisible to it, so this measures one kind of miss, not all kinds. The hand
  audit is the only check that covers the rest.

### 2. Nothing invented

- **236** rows dropped corpus-wide because their quote could not be found (v1: 164).
- Every row that reached the matrix had its quote located in the source page text.
- Sweeper rows and patch rows face the same locate-check as first-pass rows.
- The check was proven by planting a fake row and a paraphrase, both rejected.

### 3. Coverage check honest

- Unresolved: **704** across readable documents.
- **v1's figure was understated.** See "An accounting correction" above. The rule is now:
  a swept page is credited only with what the sweeper explained, never with rows already
  counted in the shortfall.
- **Known bias, stated plainly:** where the scan wrongly excludes a contractor obligation as
  a government one, the denominator shrinks and coverage looks *better* than it is. Where it
  double-counts a fill-in form line, the shortfall inflates and coverage looks *worse*. The
  first flatters us and the sweeper cannot catch it, since it only visits pages already
  showing a shortfall. Section 3's flagged pages are where to check this by hand.

### 4. Citations right

- Page numbers verified by an independent extractor on 16 pages across 5 documents,
  16 of 16, with an off-by-one probe against neighbouring pages (2026-08-28 report).
- Section references are not machine-checkable — the pipeline is instructed never to invent
  one and to use null instead. The random samples in section 3 are where that gets checked.

### 5. Unreadable documents declared unreadable

- 3 of 17 documents declared unreadable, returning no matrix, billed $0.00.

### 6. Cost measured and recorded per document

- **$53.58** total at Batch API rates: $43.44 for the v1 corpus run plus $10.14 for the truncation patch.
- The per-document costs in the summary table are the v1 run only; the patch cost is
  pooled in `corpus/eval/_patch.json` rather than apportioned, because a patched chunk
  cannot be attributed to a document without re-deriving token counts it did not record.
- Per-document figures are in the summary table and the per-document JSON.
- MASTER_PLAN sets no pass mark on cost: pricing is decided after the real number is known.

## 6. Where everything is

| What | Where |
|---|---|
| Per-document JSON, including every row | `corpus/eval/` |
| Patch record (batches, rounds, deltas) | `corpus/eval/_patch.json` |
| The corpus documents | `corpus/` |
| The six exit criteria, verbatim | `MASTER_PLAN.md`, Phase 1 |
| The audit rubric | `EXTRACTION_PROMPT_SPEC.md` §7 |
| The verdict slot | `DECISIONS.md` D-010 — still empty |


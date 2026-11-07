# EVAL REPORT v5 — Phase 1 corpus evaluation

Produced 2026-11-07 by `scripts/corpus-eval.ts`.

**This report presents evidence. It does not reach a verdict.** The six exit criteria in
`MASTER_PLAN.md` are the owner's to rule on, and section 5 below lays out what the numbers
show against each without declaring pass or fail.

## Run parameters

| | |
|---|---|
| Model | `claude-sonnet-5` (from `EXTRACTION_MODEL`; nothing in code selects a model) |
| Pricing | Batch API, 50% of list. Rates verified 2026-08-29 at platform.claude.com/docs/en/about-claude/pricing |
| Extraction batches | msgbatch_01RAAgsrieLoptSt9BctADHQ |
| Sweeper batches | msgbatch_011c4MRS4kEFzYfD4qBNjeHg |
| Random-sample seed | **20260829** — recorded so the samples below are reproducible and provably not hand-picked |
| Documents | 17 (13 readable, 4 unreadable, 0 failed) |
| Readable pages | 2,024 |
| **Total cost** | **$39.85** |

## 1. Summary table — every document

Duplicate rate is duplicates removed as a share of rows the model returned. Review-flag rate
is the share of final rows the pipeline itself marked as needing human attention.

| Document | Status | Pages | Rows | Dup rate | Dropped (invented) | Review flags | Trunc / lost chunks | Unresolved before → after | Cost |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of` | ok | 13 | 43 | 4.8% | 3 (0) | 34.9% | 0 / **0** | 2 → **1** | $0.14 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260` | ok | 74 | 234 | 23.1% | 6 (2) | 55.1% | 5 / **5** | 18 → **1** | $1.26 |
| `15F06726R0000194__RFP-15F06726R0000194-Tie` | ok | 75 | 356 | 29.6% | 13 (0) | 51.1% | 3 / **3** | 42 → **4** | $1.57 |
| `1616-26__RFP1620000348` | ok | 104 | 338 | 14.8% | 48 (5) | 48.2% | 2 / **2** | 52 → **16** | $1.66 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | ok | 81 | 265 | 35.8% | 18 (8) | 74.7% | 7 / **7** | 47 → **8** | $1.91 |
| `36C26026Q0939__SF-1449-36C26026Q0939-Stora` | **UNREADABLE** | 39 | — | — | — | — | — | — | $0.00 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VA` | ok | 73 | 193 | 46.4% | 18 (4) | 51.3% | 0 / **0** | 83 → **4** | $1.48 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | **UNREADABLE** | 8 | — | — | — | — | — | — | $0.00 |
| `70CDCR26R00000026__Attachment-01-Turnkey-F` | ok | 152 | 1292 | 33.6% | 13 (3) | 67.1% | 14 / **14** | 138 → **11** | $5.16 |
| `75N98026Q00962__RFQ-75N98026Q00962` | ok | 39 | 168 | 18.6% | 9 (6) | 54.8% | 1 / **1** | 33 → **3** | $1.16 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC0` | ok | 23 | 124 | 9.0% | 8 (2) | 17.7% | 0 / **0** | 14 → **2** | $0.64 |
| `PANMCC26P0000048766__Combined-Synopsis` | **UNREADABLE** | 33 | — | — | — | — | — | — | $0.00 |
| `W15P7T-26-R-A006__Solicitation-Amendment-0` | ok | 342 | 943 | 24.7% | 86 (5) | 55.6% | 5 / **5** | 212 → **28** | $6.21 |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicitati` | **UNREADABLE** | 144 | — | — | — | — | — | — | $0.00 |
| `W911SG27BA002__Solicitation-Amendment-W911` | ok | 157 | 731 | 21.4% | 55 (4) | 49.8% | 5 / **5** | 116 → **22** | $3.83 |
| `W912P726RA022__W912P726RA002-San-Rafael-So` | ok | 645 | 1328 | 18.2% | 193 (34) | 52.6% | 16 / **16** | 217 → **70** | $8.03 |
| `W912P825BA029__Solicitation-W912P825BA029-` | ok | 246 | 1303 | 35.1% | 104 (23) | 42.1% | 6 / **7** | 231 → **35** | $6.78 |

**Totals across readable documents:** 7,318 rows · 96 rows dropped as unlocatable · 90 rows the sweeper recovered that the first pass missed · unresolved 1205 → 205.

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

**13 pages · 43 rows · unresolved 1 · cost $0.14**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 4 | SOW 4 | shall | **review** | The Contractor shall complete the following tasks as required to comply with this SOW. |
| 2 | 5 | SOW 4.1 | must | **review** | • Compliance with MIL-STD-188-110D and MIL-STD-188-141D • Interoperability with the existing COTHEN High Frequency Cellular Network, including compatibility with Collins Aerospace software and protocols currently in use • All othe |
| 3 | 5 | SOW 4.2 | shall | high | The Contractor shall test and functionally certify all equipment prior to delivery to ensure compliance and specifications. |
| 4 | 5 | SOW 5 | declaration | high | The following details deliverables that are to be provided under this Task Order. |
| 5 | 9 | SOW 10.4 | must | **review** | Any such proposed changes must be brought to the immediate attention of the CO for action. |
| 6 | 9 | SOW 10.5 | conditional | high | If the Contractor is unable to comply with the requirement to use IPP for submitting invoices for payment, the Contractor must submit a waiver request in writing to the Contracting Officer. |
| 7 | 9 | SOW 10.5 | imperative | high | Please ensure the following are included in all invoices: • Purchase Order number • Description of services provided for a specified time period. • Unit price and total amount of each item. • Discount terms • Company name, telepho |
| 8 | 12 | Addendum B | shall | **review** | Compliance with the HLS EA shall be derived from and aligned through the CBP EA. |
| 9 | 12 | Addendum B | shall | high | All EA related component acquisitions shall be IPv6 compliant, as defined in the USGv6 Profile (NIST Special Publication 500-267) and the corresponding declarations of conformance, defined in the USGv6 Test Program. |
| 10 | 12 | Addendum B | must | **review** | Federal employees and members of the public with disabilities must be afforded access to and use of information and data comparable to that of Federal employees and members of the public without disabilities. |

#### The 3 flagged pages with the largest shortfall

*No pages were flagged on this document.*

### 1240LT26Q0172__3-Combined-SpecsCBPEWI20260624

Path: `corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf`

**74 pages · 234 rows · unresolved 1 · cost $1.26**
Rows dropped because their quote could not be found in the document: **2**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 5 | *(none)* | shall | **review** | The Contractor shall clear the old road corridor along the top of Wild Boar Ridge leading to the well as necessary to allow construction vehicle access. |
| 2 | 16 | 013100 3.1.B | imperative | high | Apply protective enclosures or coverings for protection from damage or deterioration. |
| 3 | 24 | 014000 1.5.C | shall | high | The Contractor shall cooperate with others performing required tests, inspections and other quality control services, shall provide access to the work, and shall furnish incidental labor and facilities necessary to facilitate insp |
| 4 | 26 | 017700 3.1.A | imperative | high | Do not discharge volatile, harmful, or dangerous materials into drainage or septic systems. |
| 5 | 26 | 017700 3.1.A.1 | imperative | **review** | Complete the following cleaning operations before requesting inspection for certification of Substantial Completion for entire Project: |
| 6 | 30 | 221114 2.1.E | shall | high | Each torque arrestor shall include 2 each stainless-steel hose clamps to secure the torque arrestor to the drop pipe. |
| 7 | 42 | *(none)* | shall | **review** | Provide all grounding systems and connections above grade by the fusion-welding process or with bolted solderless connectors, in compliance with UL 467, and those below grade shall be made by a fusion-welding process. |
| 8 | 47 | *(none)* | shall | **review** | Panelboard drawings shall provide detail of the size, materials, and ratings of all panel board construction components including circuit breakers. |
| 9 | 50 | *(none)* | shall | **review** | All feeder terminations shall be rated at 75 degrees C. |
| 10 | 63 | 329200 N | imperative | high | Remove temporary protective devices after final acceptance. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 15 | 5 | 3 | 2 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 19 | 2 | 0 | 2 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 30 | 16 | 14 | 2 | yes | 3 exclusion(s): permission, government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 15
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 19
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 30
```

### 15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer

Path: `corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf`

**75 pages · 356 rows · unresolved 4 · cost $1.57**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 8 | B.6 | shall | **review** | The First Article price shall include, as applicable: a. Nonrecurring engineering; b. Design finalization; c. Prototype and production planning; d. Trailer fabrication; e. Structural and mechanical systems; |
| 2 | 10 | B.9 | shall | high | Each Prime Hauler Vehicle shall meet the requirements of the PWS and shall be fully compatible with the MCPV trailer with which it is intended to operate. |
| 3 | 18 | D.1 | shall | high | The Contractor shall preserve, protect, package, secure, and mark each MCPV, prime hauler, component, accessory, spare, tool, document, and other deliverable in a manner that prevents damage, deterioration, contamination, loss, ta |
| 4 | 21 | E.5 | shall | high | The Contractor shall provide the facilities, equipment, instrumentation, personnel, utilities, safety support, test procedures, records, and other resources necessary to conduct the required testing. |
| 5 | 23 | F.2 | shall | **review** | The Contractor shall not be required to make any delivery or complete performance after September 29, 2033. |
| 6 | 28 | *(none)* | shall | **review** | In addition to the requirements of the applicable payment clause, each invoice shall include: |
| 7 | 32 | *(none)* | shall | **review** | The Contractor shall notify the Contracting Officer promptly of any suspension, withdrawal, expiration without renewal, material reduction in certified scope, change in the certified legal entity, or other condition that may affec |
| 8 | 33 | *(none)* | shall | **review** | Software, firmware, applications, embedded systems, remote-monitoring functions, and network-connected components shall be delivered in a secure and supportable configuration. |
| 9 | 47 | K.3 | will | high | Unless expressly disclosed, the Offeror represents that its proposal accepts and will comply with the solicitation requirements within the proposed firm-fixed prices. |
| 10 | 63 | L.9 | must | high | Questions are due no later than 2 SEPT 2026 at 3:00 p.m. Eastern Time (ET). |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 17 | 15 | 12 | 3 | yes | 3 exclusion(s): government-actor, definitional — resolved |
| 18 | 14 | 11 | 3 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 21 | 15 | 12 | 3 | yes | 3 exclusion(s): government-actor, permission — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 17
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 18
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 21
```

### 1616-26__RFP1620000348

Path: `corpus/1616-26__RFP1620000348.pdf`

**104 pages · 338 rows · unresolved 16 · cost $1.66**
Rows dropped because their quote could not be found in the document: **5**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 33 | *(none)* | shall | **review** | Interested offerors shall read the solicitation in its entirety prior to submission of any questions, All questions shall be submitted in writing to Brent Ponder at Brent.Ponder@usdoj.gov and Barbara McMaster at Barbara.McMaster@u |
| 2 | 40 | SOW | will | high | Offeror (s) will obtain a signed final punch list from the customer showing what items remain to be completed prior to finishing initial install. |
| 3 | 41 | SOW | shall | high | All pricing shall be based on the US Department of Labor Wage Determination. |
| 4 | 44 | SOW | conditional | high | If the shipping company attempts a delivery within the defined time period and is unable to do so for any reason, the installer shall be responsible for all costs associated with redelivery fees. |
| 5 | 44 | SOW | conditional | **review** | Installer is not expected to accept delivery outside the normal delivery time frame described above but may do so if agreed upon by both parties. |
| 6 | 46 | SOW | declaration | high | Sub-contractors will not be considered sufficient to fulfil these positional requirements. |
| 7 | 46 | SOW | shall | high | This information shall be provided as part of the written proposal, utilizing the attached Excel spreadsheet. |
| 8 | 50 | SOW | shall | **review** | Package Office- These ancillary items shall have the same receive/inspect/deliver/notification requirements that UNICOR product has. |
| 9 | 85 | Section I | must | **review** | Any handling of Government information on Contractor networks or IT must be approved by the Senior Component Official for Privacy of the component entering into this contract. |
| 10 | 103 | M5 | shall | high | M5. Each proposal package shall consist of the following completed and signed documents: o,,SSF 1449 o,,Amendments o,,SAM Registration o,,Unique Entity Identification Number o,,TIN Number o,,(3) Business Management Questionnaires  |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 61 | 7 | 3 | 4 | yes | 3 exclusion(s): government-actor, definitional — resolved |
| 64 | 8 | 4 | 4 | yes | 3 exclusion(s): government-actor — resolved |
| 82 | 8 | 4 | 4 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 61
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 64
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 82
```

### 19C02026Q0027__Solicitation-19C02026Q0027

Path: `corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf`

**81 pages · 265 rows · unresolved 8 · cost $1.91**
Rows dropped because their quote could not be found in the document: **8**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 9 | CONTRACTOR'S SUBMISSION OF CONSTRUCTION SCHEDULES | declaration | high | (a) The time for submission of the schedules referenced in FAR 52.236-15, "Schedules for Construction Contracts", paragraph (a), is hereby modified to reflect the due date for submission as ten (10) calendar days after receipt of  |
| 2 | 10 | WORKING HOURS | shall | high | All work shall be performed during business hours, from Monday through Friday, from 8:00 to 17:00. |
| 3 | 26 | 52.240-91(b) | conditional | high | (b) Prohibitions on providing or using specific products or services in performance of contract. Unless a waiver or exception applies, the Contractor is prohibited from providing any products or services to the Government or using |
| 4 | 27 | 52.240-91(d)(1) | conditional | high | (i) Unless an applicable waiver has been issued by the Government, the Contractor cannot use any equipment, systems, or services that uses covered telecommunications equipment or services as a substantial or essential component of |
| 5 | 27 | 52.240-91(d)(4)(i) | conditional | high | Unless an exception applies according to paragraph (d)(4)(iii) or the Government grants a waiver, the contractor shall not engage in certain activities or transactions relating to Iran (section 6(b)(1)(A) of Iran Sanctions Act (50 |
| 6 | 29 | *(none)* | must | **review** | If a report must be submitted to a contracting office, the Contractor shall submit the report as follows: |
| 7 | 33 | *(none)* | shall | **review** | The contractor shall maintain an accurate record on all mishaps incident to work performed under this contract resulting in death, traumatic injury, occupational disease, or damage to or theft of property, materials, supplies, or  |
| 8 | 56 | *(none)* | shall | **review** | (SDS) for all chemical products (sealants, Pintuco paints, solvents, adhesives) shall be maintained on site and made available to the COR upon request. |
| 9 | 69 | *(none)* | shall | **review** | The contractor shall send to the COR all the certificates (issued by a certified ONAC entity) of all the slings, chains, shackles, or other elements used for lifting loads. |
| 10 | 69 | *(none)* | shall | **review** | The contractor shall ensure all welder operators and attendants exposed to the hazard use proper welding personal protection equipment. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 66 | 9 | 0 | 9 | yes | 1 exclusion(s): government-actor — resolved |
| 65 | 7 | 1 | 6 | yes | 1 exclusion(s): government-actor — resolved |
| 9 | 10 | 6 | 4 | yes | 1 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 66
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 65
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 9
```

### 36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-

Path: `corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf`

**73 pages · 193 rows · unresolved 4 · cost $1.48**
Rows dropped because their quote could not be found in the document: **4**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 6 | SOW 2 | must | **review** | - Must meet ASTM E648 Critical Radiant Flux |
| 2 | 9 | SOW 8 | shall | **review** | Description of flooring materials Delivery date and location Invoices shall be submitted through the VA Invoice Processing Platform (IPP) unless otherwise directed. |
| 3 | 11 | SOW 9.C.8 | shall | high | 8. The Contractor(s) shall not create or maintain any records containing any non-public [FACILITY] information that are not specifically tied to or authorized by the contract. |
| 4 | 31 | 52.212-4(f) | shall | **review** | When an excusable delay occurs, the Contractor shall— (1) Notify the Contracting Officer in writing as soon as possible; |
| 5 | 32 | 52.212-4(g) | shall | high | The Contractor shall submit invoices to the address designated in the contract to receive invoices. |
| 6 | 33 | 52.212-4(i)(5)(i) | shall | **review** | Amounts shall be due at the earliest of the following dates: |
| 7 | 34 | 52.212-4(l) | shall | **review** | The Contractor shall not be paid for any work performed or costs incurred which reasonably could have been avoided. |
| 8 | 34 | 52.212-4(r) | shall | **review** | Any inconsistencies in this solicitation or contract shall be resolved by giving precedence in the following order: (1) The schedule of supplies/services. |
| 9 | 38 | C.3 52.209-9(a) | shall | high | The Contractor shall update the information in the Federal Awardee Performance and Integrity Information System (FAPIIS) on a semi-annual basis, throughout the life of the contract, by posting the required information in the Syste |
| 10 | 55 | *(none)* | conditional | **review** | (2) If a report must be submitted to a contracting office, the Contractor shall submit the report as follows: (i) If a Department of Defense contracting office, the Contractor shall report to the website at https://dibnet.dod.mil. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 6 | 33 | 15 | 18 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 55 | 12 | 2 | 10 | yes | 1 exclusion(s): permission — resolved |
| 5 | 18 | 9 | 9 | yes | 1 exclusion(s): permission — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 6
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 55
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 5
```

### 70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem

Path: `corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf`

**152 pages · 1292 rows · unresolved 11 · cost $5.16**
Rows dropped because their quote could not be found in the document: **3**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 4 | PWS 3 | shall | high | In addition to comprehensive detention services, the Contractor shall provide comprehensive operations and maintenance (O&M) services including ongoing preventive and corrective maintenance, facility operations, post-closing servi |
| 2 | 11 | *(none)* | shall | **review** | The plan shall receive the concurrence of the COR prior to implementation and shall not be modified without the further written concurrence of the CO. |
| 3 | 17 | PWS 17 | shall | high | Contractor employees shall write reports of incidents as outlined in the post orders. |
| 4 | 26 | *(none)* | shall | **review** | Written documentation of any problem areas shall be submitted to the COR by the end of the shift. |
| 5 | 26 | *(none)* | shall | **review** | The Contractor shall develop and implement a comprehensive suicide prevention and intervention program in accordance with ICE policy and standards. |
| 6 | 44 | *(none)* | shall | **review** | If death is due to violence, accident surrounded by unusual or questionable circumstances or is sudden and the deceased has not been under immediate medical supervision, the Contractor shall notify the coroner of the local jurisdi |
| 7 | 83 | *(none)* | shall | **review** | In accordance with the Federal Acquisitions Regulations (FAR) clause 52.239-1, contractor shall be responsible for the following privacy and security safeguards: |
| 8 | 119 | PWS 60.35 | shall | high | Changes in duty hours shall also be posted on this form in enough time to ensure 24-hour advance notice. |
| 9 | 135 | PWS 68 | will | **review** | The individual will interface with OPR PSD through the COR on all security matters, to include physical, personnel, and protection of all Government information and data accessed by the Contractor. |
| 10 | 143 | *(none)* | shall | high | The Service Provider shall provide all deliverables to the COR utilizing Microsoft Excel, PowerPoint or Word format as deemed appropriate. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 137 | 7 | 2 | 5 | yes | 1 exclusion(s): government-actor — resolved |
| 9 | 16 | 12 | 4 | yes | 2 exclusion(s): government-actor — **still unresolved** |
| 11 | 23 | 19 | 4 | yes | 3 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 137
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 9
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 11
```

### 75N98026Q00962__RFQ-75N98026Q00962

Path: `corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf`

**39 pages · 168 rows · unresolved 3 · cost $1.16**
Rows dropped because their quote could not be found in the document: **6**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 5 | SOW | declaration | high | Requirements include: • instrument software updates • preventative maintenance inspections, incl. one (1) planned PM / year. • technical telephone consulting available during normal business hours • unlimited on-site service visit |
| 2 | 6 | SOW | declaration | high | The Contractor shall provide dCODE Dextramer® with the following specifications: |
| 3 | 11 | *(none)* | declaration | high | dCODE Dextramer® (10x) - Gold, HLA-A*0201 / 1 |
| 4 | 14 | 52.212-4(a) | shall | high | The Contractor shall only tender for acceptance those items that conform to the requirements of this contract. |
| 5 | 14 | 52.212-4(b) | shall | high | However, when a third party makes payment (e.g., use of the Governmentwide commercial purchase card), the Contractor may not assign its rights to receive payment under this contract. |
| 6 | 18 | Attachment 2(l) | required | **review** | The Contractor shall not be required to comply with the cost accounting standards or contract cost principles for this purpose. |
| 7 | 20 | 52.212-4(t)(3) | shall | **review** | Assignees shall be record that indicates payments, including those made by EFT, to an ultimate recipient other than that Contractor will be considered to be incorrect information within the meaning of contract. |
| 8 | 23 | *(none)* | shall | **review** | Fractional parts of an hour shall be payable on a prorated basis. |
| 9 | 25 | 52.212-4 Alt I (i)(1)(ii)(C) | shall | **review** | If at any time during the performance of this contract, the Contractor has reason to believe that the total price to the Government for performing this contract will be substantially greater or less than the then stated ceiling pr |
| 10 | 37 | *(none)* | must | **review** | If identified in the award as a separate line item, it must be billed separately. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 15 | 9 | 4 | 5 | yes | 1 exclusion(s): government-actor — resolved |
| 5 | 8 | 5 | 3 | yes | 2 exclusion(s): government-actor, permission — **still unresolved** |
| 21 | 5 | 2 | 3 | yes | 1 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 15
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 5
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 21
```

### 80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final

Path: `corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf`

**23 pages · 124 rows · unresolved 2 · cost $0.64**
Rows dropped because their quote could not be found in the document: **2**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | *(none)* | imperative | high | NOTE: OFFEROR TO COMPLETE BLOCKS 12, 17, 23, 24, AND 30. |
| 2 | 4 | *(none)* | declaration | high | Additional Regulation or Supplement Clauses Incorporated by Reference Number Title Effective Date NFS 1852.216-78 Firm-Fixed-Price Dec 2025 |
| 3 | 5 | SOW 1.1.2 | shall | high | 1.1.2. The Contractor shall provide services globally without exception. |
| 4 | 5 | SOW 1.1.2 | conditional | high | In the event that the Contractor cannot provide a medevac in specific parts of the world, then the Contractor shall provide sufficient rationale to the NASA COR why it cannot and provide an alternate solution at no additional cost |
| 5 | 5 | SOW 1.1.5 | shall | high | 1.1.5. The Contractor shall have a documented Safety Management System (SMS) in accordance with International Civil Aviation Organization (ICAO), Federal, and industry standards. |
| 6 | 7 | SOW 2.3.3 | shall | **review** | 2.3.3. The Contractor shall provide, within 60 calendar days of contract start and update annually for currency by Oct 1st, a Medical Facility Support Plan for each of the Russia and Kazakhstan medical facilities identified by NAS |
| 7 | 14 | H.1 | shall | high | In addition, the Contractor shall submit in advance any proposed public use of the NASA name or initials for NASA review and approval. |
| 8 | 15 | 52.247-34 | shall | **review** | The total duration of this contract, including the exercise of any options under this clause, shall not exceed 5 years. |
| 9 | 18 | Section L | shall | high | The Offeror shall prepare their proposal in accordance with the instructions in this solicitation. |
| 10 | 19 | Section L | shall | high | The Offeror shall provide a compliance matrix (cross-walk table) explicitly mapping each element of the Statement of Work (SOW) to the specific section and page number where it is addressed in the Technical Capability Proposal. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 9 | 8 | 5 | 3 | yes | 6 exclusion(s): government-actor — **still unresolved** |
| 23 | 5 | 2 | 3 | yes | 2 exclusion(s): permission, government-actor — resolved |
| 5 | 12 | 10 | 2 | yes | 2 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 9
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 23
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 5
```

### W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006

Path: `corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf`

**342 pages · 943 rows · unresolved 28 · cost $6.21**
Rows dropped because their quote could not be found in the document: **5**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 11 | PWS 4.2.2 | shall | high | U.S. based Contractor employees and associated sub-contractor employees shall receive Government provided area of responsibility (AOR) specific AT awareness training as directed by AR 525-13. |
| 2 | 97 | 52.216-32 | must | **review** | The Ombudsman must review complaints from the Contractor concerning all task-order and delivery-order actions for this contract and ensure the Contractor is afforded a fair opportunity for consideration in the award of orders, con |
| 3 | 161 | 252.215-7991(b) | conditional | **review** | (b) Canadian Commercial Corporation shall obtain and provide the following for modifications that exceed the $150,000 [or higher dollar value specified by the U.S. Contracting Officer in the solicitation]. (i) Profit rate or fee ( |
| 4 | 186 | *(none)* | shall | high | (4) Limited rights markings. Data delivered or otherwise furnished to the Government with limited rights shall be marked as follows: LIMITED RIGHTS Contract Number TBD Contractor Name TBD Contractor Address TBD The Government's ri |
| 5 | 191 | *(none)* | required | **review** | Whether, how much, and what type of analysis or testing is required to establish workability depends on the nature of the item, component, or process, and the state of the art. |
| 6 | 238 | *(none)* | shall | **review** | [ The offeror shall enter the name and unique entity identifier of each party to the joint venture: |
| 7 | 276 | *(none)* | must | **review** | Offerors must submit the Government their passphrase in order for the Government to open and download the files submitted via DoD SAFE. |
| 8 | 281 | Section L | shall | high | The Offeror's Attachment 0002 submission shall be a consolidation of the Offeror's scorecard; the affiliate, subsidiary, or different division's scorecard (as applicable); and all corresponding required supporting documentation fo |
| 9 | 300 | 252.204-7025(b)(1) | required | high | This CMMC level, or higher (see 32 CFR part 170), is required prior to award for each contractor information system that will process, store, or transmit Federal contract information (FCI) or controlled unclassified information (C |
| 10 | 320 | 252.215-7994(b)(1)(ii) | shall | **review** | or (4) If the Offeror does not sell a comparable commercial product to the general public or nongovernmental entities for purposes other than government purposes, the Offeror shall-- (i) Notify the Contracting Officer in writing t |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 292 | 13 | 5 | 8 | yes | 1 exclusion(s): government-actor — resolved |
| 58 | 8 | 3 | 5 | yes | 3 exclusion(s): government-actor, permission — resolved |
| 10 | 17 | 13 | 4 | yes | 3 exclusion(s): permission, government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 292
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 58
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 10
```

### W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001

Path: `corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf`

**157 pages · 731 rows · unresolved 22 · cost $3.83**
Rows dropped because their quote could not be found in the document: **4**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 63 | 52.219-28 | shall | high | The Contractor shall notify the contracting officer in writing within the timeframes specified in paragraph (b) of this clause, that the data have been validated or updated, and provide the date of the validation or update. |
| 2 | 76 | 52.232-32(d)(2) | conditional | high | (2) If at any time the amount of payments under this contract exceeds any limitation in this contract, the Contractor shall repay to the Government the excess. |
| 3 | 98 | SOW 1.6 | declaration | **review** | The following attachments are incorporated into this Contract. |
| 4 | 105 | SOW 1.18.1 | must | high | The SSHO must maintain the APP and a log of signatures by each Subcontractor foreman on-site, attesting that they have read and understand the plan. |
| 5 | 124 | SOW 4.26 | must | **review** | Responsible authorities must implement, administer, and maintain ongoing back-flow prevention and crossconnection control programs to prevent back-flow; |
| 6 | 128 | SOW 4.38 | must | high | The SSHO must provide and maintain the APP and a log of signatures by each Subcontractor foreman, attesting that they have read and understand the APP, and make the APP and log available on-site to the KO. |
| 7 | 129 | SOW 4.42 | shall | **review** | Time shall not be extended to correct insufficient progress prior to the modification. |
| 8 | 134 | *(none)* | shall | **review** | The Contractor or Subcontractor shall provide required equipment, materials, shields, extinguishers, sand, and other devices at each location where work of the type requiring a permit is conducted. |
| 9 | 137 | *(none)* | shall | **review** | To receive progress payments, the Contractor shall provide the following but not limited to (See specifications for additional submittals): |
| 10 | 139 | SOW 4.72.10 | conditional | high | 4.72.10. If TO drawings or specifications allow options, only the option selected for construction shall be shown on the record drawings. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 16 | 15 | 3 | 12 | yes | 2 exclusion(s): government-actor — resolved |
| 104 | 21 | 15 | 6 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 17 | 5 | 0 | 5 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 16
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 104
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 17
```

### W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-

Path: `corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf`

**645 pages · 1328 rows · unresolved 70 · cost $8.03**
Rows dropped because their quote could not be found in the document: **34**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 36 | 52.209-13(e) | declaration | **review** | The certification in paragraph (b)(1) of this provision is a material representation of fact upon which reliance was placed when making award. |
| 2 | 76 | 52.232-5(i) | shall | **review** | In accordance with 31 U.S.C. 3903(c)(1), the amount payable under paragraph (d)(2) of this clause shall be- |
| 3 | 80 | 52.248-3(h) | shall | high | In computing any adjustment in this contract's price under paragraph (f) of this clause, the Contractor's allowable development and implementation costs shall include any subcontractor's allowable development and implementation co |
| 4 | 211 | 01 35 26 3.4.1.a | declaration | high | Material handling equipment fitted with personnel work platform attachments are prohibited from traveling or positioning while personnel are working on the platform. |
| 5 | 238 | 01 45 00.15 10 1.7.2.1 | must | high | Each pay activity must be assigned to a Contract Line Item Number (CLIN). |
| 6 | 243 | 01 50 00 2.1.2 | shall | high | The project signs shall be constructed in accordance with USACE Engineer Pamphlets, EP 310-1-6a, Section 16. |
| 7 | 263 | 01 57 19.01 43 3.7.4.1 | imperative | high | Submit verbal and written notifications as required by the federal( 40 CFR 300.125 and 40 CFR 355), state, local regulations and instructions. |
| 8 | 342 | EM 385-1-1 01.D.05 | shall | **review** | As a minimum , these records shall include exposure work hours and a record of occupational injuries and illnesses that include the data elements listed below. |
| 9 | 342 | EM 385-1-1 01.D.05 | conditional | high | If data on log provided below is revised after it is submitted to USACE, Contractor shall provide a revised report to the GDA. |
| 10 | 504 | *(none)* | must | **review** | For Central Valley listed anadromous salmonids (i.e, Sacramento winter-run Chinook, CV spring-run Chinook and CCV steelhead) all individuals must travel through the estuary to access the ocean as smolts and return as adults to the |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 364 | 9 | 1 | 8 | yes | 3 exclusion(s): government-actor — resolved |
| 371 | 15 | 7 | 8 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 372 | 12 | 5 | 7 | yes | 10 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 364
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 371
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 372
```

### W912P825BA029__Solicitation-W912P825BA029-OM25035

Path: `corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf`

**246 pages · 1303 rows · unresolved 35 · cost $6.78**
Rows dropped because their quote could not be found in the document: **23**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 60 | *(none)* | shall | **review** | (2) Provide a copy of the remittance and supporting documentation to the Contracting Officer. |
| 2 | 98 | 01 35 26 1.6 | shall | high | The lettering shall be black, the safety circle and cross green, and the background white. |
| 3 | 110 | 01 45 00.15 10 1.5.2.1 | must | high | Each pay activity must be assigned to a Contract Line Item Number (CLIN). |
| 4 | 110 | 01 45 00.15 10 1.5.2.2 | shall | high | After the update, the Contractor shall generate a payment request and prompt payment certification using RMS. |
| 5 | 112 | 01 45 00.15 10 1.5.5 | shall | high | The Contractor shall enter and update the contract project schedule in RMS by either manually entering all schedule data or by importing the Standard Data Exchange Format (SDEF) file. |
| 6 | 115 | 01 57 20.00 10 1.4.1 | declaration | **review** | a. Environmental pollution control plan; b. Applicable Federal, State, and Local pollution control regulations; c. Air Pollution - dust, smoke, noise, etc.; d. Water Pollution - disposal of water, oil, etc.; e. Land Pollution - de |
| 7 | 125 | 35 01 50.73 3.1.4.c | shall | high | The applicable gage shall be read to the nearest 0.1-foot prior to and after each day's survey. |
| 8 | 133 | 35 20 23.23 1.2.2.j | conditional | high | When conditions are such that the Contractor is placed on 0% pay time hours, the Contractor shall remain at 0% pay time hours until material begins flowing out of the discharge pipeline. |
| 9 | 175 | 35 20 23.23 08 1.3 | shall | high | All costs for the work covered under this section shall be included in the contract unit price per hour in the "NOH Dredging" and "NOH Option Dredging" items on the bidding schedule. |
| 10 | 202 | 35 20 23.23 16, 3.2.1 | shall | high | These indicators of breeding activity shall be brought to the attention of the Government upon discovery to determine if nest prevention measures or avoidance measures are required. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 80 | 10 | 2 | 8 | yes | 1 exclusion(s): government-actor — resolved |
| 98 | 25 | 17 | 8 | yes | 2 exclusion(s): permission — **still unresolved** |
| 43 | 9 | 3 | 6 | yes | 1 exclusion(s): permission — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 80
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 98
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 43
```

## 4. The invention count, per document

A row is dropped when its quoted text cannot be located anywhere in the document. This is the
structural defence against a fabricated citation, and the count is what the defence caught.
A dropped row never reaches a user — but a rising count means the model is leaning on the net.

| Document | Rows kept | Dropped: not found | Dropped: bad shape | Dropped: page out of range |
|---|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-W` | 43 | **0** | 0 | 0 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI2026062` | 234 | **2** | 0 | 0 |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-` | 356 | **0** | 0 | 0 |
| `1616-26__RFP1620000348` | 338 | **5** | 0 | 0 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 265 | **8** | 1 | 0 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPI` | 193 | **4** | 0 | 0 |
| `70CDCR26R00000026__Attachment-01-Turnkey-Fac` | 1292 | **3** | 1 | 0 |
| `75N98026Q00962__RFQ-75N98026Q00962` | 168 | **6** | 0 | 0 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027` | 124 | **2** | 0 | 0 |
| `W15P7T-26-R-A006__Solicitation-Amendment-004` | 943 | **5** | 0 | 0 |
| `W911SG27BA002__Solicitation-Amendment-W911SG` | 731 | **4** | 0 | 0 |
| `W912P726RA022__W912P726RA002-San-Rafael-Soli` | 1328 | **34** | 0 | 0 |
| `W912P825BA029__Solicitation-W912P825BA029-OM` | 1303 | **23** | 0 | 0 |

**Total dropped as unlocatable across the corpus: 96.**

## 5. The six exit criteria — what the numbers show

Ranked by how much of the decision rests on each. **No verdict is offered here.**

### 1. Catches everything (the recall check) — the criterion the project turns on

The owner reads two pages per readable document by hand and counts what the pipeline missed.
**This report deliberately does not choose those pages.**

What the machine can say about its own recall:

- The independent scan found **6769** binding-verb occurrences across readable documents.
- After the sweeper, **205** occurrences remain neither captured nor explained.
- The sweeper recovered **90** requirements the first pass had missed. The first pass alone is measurably not enough.
- The scan sees Class-A verbs only. Imperatives, conditionals, and declarations carry no magic
  word and are invisible to it, so this measures one kind of miss, not all kinds. The hand audit
  is the only check that covers the rest.

### 2. Nothing invented

- **96** rows were dropped corpus-wide because their quote could not be found.
- Every row that reached the matrix had its quote located in the source page text.
- The check itself was proven by planting a fake row and a paraphrase, both rejected; disabling
  the check turns those tests red. See the 2026-08-29 Task 3 report.

### 3. Coverage check honest

- Unresolved across readable documents: **1205 before the sweeper, 205 after.**
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

- **$39.85** for the corpus at Batch API rates.
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
| `0020153254COHEN` | 40 | 43 | +3 | 0 | 1 |
| `1240LT26Q0172` | 621 | 234 | -387 | 3 | 1 |
| `15F06726R0000194` | 427 | 356 | -71 | 29 | 4 |
| `1616-26` | 385 | 338 | -47 | 20 | 16 |
| `19C02026Q0027` | 587 | 265 | -322 | 16 | 8 |
| `36C26126Q1034` | 255 | 193 | -62 | 24 | 4 |
| `70CDCR26R00000026` | 1674 | 1292 | -382 | 52 | 11 |
| `75N98026Q00962` | 159 | 168 | +9 | 29 | 3 |
| `80JSC026MEDEVAC5Q` | 132 | 124 | -8 | 5 | 2 |
| `W15P7T-26-R-A006` | 1008 | 943 | -65 | 122 | 28 |
| `W911SG27BA002` | 841 | 731 | -110 | 63 | 22 |
| `W912P726RA022` | 2149 | 1328 | -821 | 154 | 70 |
| `W912P825BA029` | 1351 | 1303 | -48 | 72 | 35 |
| **All readable** | **9648** | **7318** | **-2330** | **593** | **205** |

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
| `W912P726RA022` | 155 |

### Invention: rows dropped because the quote could not be found

| Document | v3 not-found | v4 not-found | v4 mangled (second reader) |
|---|---:|---:|---:|
| `0020153254COHEN` | 1 | 0 | 3 |
| `1240LT26Q0172` | 6 | 2 | 4 |
| `15F06726R0000194` | 10 | 0 | 13 |
| `1616-26` | 6 | 5 | 43 |
| `19C02026Q0027` | 12 | 8 | 9 |
| `36C26126Q1034` | 14 | 4 | 14 |
| `70CDCR26R00000026` | 19 | 3 | 9 |
| `75N98026Q00962` | 0 | 6 | 3 |
| `80JSC026MEDEVAC5Q` | 2 | 2 | 6 |
| `W15P7T-26-R-A006` | 42 | 5 | 81 |
| `W911SG27BA002` | 27 | 4 | 51 |
| `W912P726RA022` | 23 | 34 | 159 |
| `W912P825BA029` | 24 | 23 | 81 |

Every one of these rows was withheld from the matrix. `not-found` means the pipeline's
own reader could not locate the quote; `mangled` means an independent reader could not,
which catches a quote spliced across columns that looks correct to us.

### Rows removed as same-page contained fragments

A row whose text sits wholly inside a longer row on the same page is a fragment of it,
not a second requirement. The ten longest removed fragments are quoted per document so
the rule can be checked against real text rather than trusted.

**`15F06726R0000194`** — 3 fragment(s) removed.

- p.42: "The Contractor shall furnish to the Government, when and if ordered, the supplies or services specified in the Schedule up to and including the quantity designated in the Schedule as the "maximum.""
- p.56: "The prime Offeror shall hold a current ISO 9001 certification at the time of proposal submission"
- p.53: "An optional price explanation, when necessary under L.7, shall not exceed two (2) pages."

**`1616-26`** — 21 fragment(s) removed.

- p.63: "The interest charge shall be computed for the actual number of calendar days involved beginning on the due date and ending on- (A) The date on which the designated office receives payment from the Contractor; (B) The date of issuance of a Government check to the Contractor from which an amount other…"
- p.58: "For acquisitions of commercial products or commercial services). the prohibition in paragraph (a) applies only to the extent that any agreement restricting sales by subcontractors results in the Federal Government being treated differently from any other prospective purchaser for the sale of the com…"
- p.82: "If the report provided does not contain any of the information required by paragraph (h)(1) of this clause, and the contractor later discovers new information that is required by paragraph (h)(1) of this clause, then the contractor shall submit a subsequent report within 72 hours of discovering the …"
- p.67: "Records relating to appeals under the disputes clause or to litigation or the settlement of claims arising under or relating to this contract shall be made available until such appeals, litigation, or claims are finally resolved."
- p.58: "Total upward adjustment for the contract period, including any option periods, shall not exceed 10% of the original contract base price, based on the original or option period contract price; downward adjustments are not limited."
- p.67: "Records relating to appeals under the disputes clause or to litigation or the settlement of claims arising under or relating to this contract shall be made available until such appeals, litigation, or claims are finally resolved."
- p.63: "Amounts shall be due at the earliest of the following dates: (A) The date fixed under this contract. (B) The date of the first written demand for payment, including any demand for payment resulting from a termination for cause."
- p.82: "The contractor shall also report the information in paragraph (h)(1) if the contractor wishes to ask for a waiver of the requirements of a new FASCSA order being applied through modification."
- p.67: "If this contract is completely or partially terminated, the records relating to the work terminated shall be made available for 3 years after any resulting final termination settlement."
- p.67: "If this contract is completely or partially terminated, the records relating to the work terminated shall be made available for 3 years after any resulting final termination settlement."

**`19C02026Q0027`** — 16 fragment(s) removed.

- p.66: "The contractor must place provisional barricades and warning signs to maintain a safe and accessible path-of-travel for all pedestrians, including those with disabilities, in order to avoid accidents to third parties."
- p.66: "Erect and maintain for the duration of the Contract proper barricades including fencing material, traffic cones, caution tape and temporary curb ramps complying with all access codes and regulations."
- p.66: "a. All tools, equipment and/or machinery that will be used in the execution of the contracted work must be inspected by a competent person before starting the project."
- p.66: "d. Sharp cutting tools must be carried in Tools Carrying Cases; under no circumstances they are to be carried in uniform pockets."
- p.66: "b. Inspected at regular intervals and maintain in good condition all tools in accordance with the manufacture’s specifications."
- p.66: "b. The contractor is required to inspect and maintain spare PPE in case of damage, deterioration, or loss of the equipment."
- p.66: "a. Contractor personnel must use personal protective equipment (PPE) required and in accordance with the contracted work."
- p.66: "c. The PPE used must meet the technical specifications required by Colombian regulation and international standards."
- p.66: "e. In those tasks where PPE certified is required, the contractor must provide the current certification."
- p.66: "f. The contractor must provide records of PPE received for all the employees involved in the task."

**`36C26126Q1034`** — 56 fragment(s) removed.

- p.54: "If the Contractor identifies or is notified by any source, (including a subcontractor at any tier), that any product or service provided or used (or to be provided or used) during contract performance does not comply with any prohibition in this clause, then the Contractor shall report the following…"
- p.55: "If a report must be submitted to a contracting office, the Contractor shall submit the report as follows: (i) If a Department of Defense contracting office, the Contractor shall report to the website at https://dibnet.dod.mil. (ii) For all other contracting offices, the Contractor shall report to th…"
- p.54: "For Federal Supply Schedules, Governmentwide acquisition contracts, multi-agency contracts or any other procurement instrument intended for use by multiple agencies, upon notification from the Contracting Officer, during the performance of the contract, the Contractor shall promptly make any necessa…"
- p.55: "The Contractor shall submit a report to the contracting office identified in paragraph (h)(2) of this clause if the Contractor identifies, including through any notification by a subcontractor at any tier, that a covered article or product or service produced or provided by a source was provided to …"
- p.44: "Except as provided in paragraph (g) of this clause, the Contractor shall make the representation(s) required by paragraph (b) of this clause by validating or updating all its representations in the Representations and Certifications section of the System for Award Management (SAM) and its other data…"
- p.71: "If a disclosure is required to be submitted to a contracting office, the offeror shall submit the disclosure as follows: (i) If a Department of Defense contracting office, the offeror shall submit the disclosure to the website at https://dibnet.dod.mil. (ii) For all other contracting offices, the Of…"
- p.55: "If the Contractor identifies a new FASCSA order(s) that could impact their supply chain, then the Contractor shall conduct a reasonable inquiry to identify whether a covered article or product or service produced or provided by a source subject to the FASCSA order(s) was provided to the Government o…"
- p.55: "During contract performance, the Contractor shall review SAM at least once every three months, or as advised by the Contracting Officer, to check for covered articles subject to FASCSA order(s), or for products or services produced by a source subject to FASCSA order(s) not currently identified unde…"
- p.55: "If the report provided does not contain any of the information required by paragraph (h)(1) of this clause, and the contractor later discovers new information that is required by paragraph (h)(1) of this clause, then the contractor shall submit a subsequent report within 72 hours of discovering the …"
- p.71: "If the disclosure provided does not contain any of the information required by paragraph (1), and the Offeror later discovers new information that is required by paragraph (1), then the Offeror shall submit a subsequent disclosure within 72 hours of discovering the new information."

**`70CDCR26R00000026`** — 23 fragment(s) removed.

- p.27: "Facilities at this level shall provide medical management for individuals with complex, chronic medical conditions that require frequent clinical interactions to maintain control or stability of the condition, including assistance with activities of daily living (e.g., feeding, toileting, bathing, a…"
- p.99: "All records acquired or generated by the Contractor related to this contract and in possession of the Contractor, including those described in the two paragraphs above, shall be subject to inspection, copying, and audit by the government or its designees at all reasonable times"
- p.36: "v. Must complete 20 hours of Continuing Education Units (CEUs) annually approved by the American Psychological Association (APA) or National Association of Social Workers (NASW); Additional CEU hours may be obtained by another sponsoring equivalent; and"
- p.36: "vi. Shall maintain membership with licensing board associated with clinical license and submit required completion of CEUs to the licensing board annually as required; Membership with the state or territory clinical licensing board is required."
- p.39: "All MPv2s end-users are required to read and utilize (as applicable) the IHSC Provider Information packet (located inside of MPv2s) to assist providers with claim issues and send the IHSC Provider Information packet to the community provider."
- p.81: "Procurements for products, systems, services, hardware, or software involving controlled federal facility or information system shall be PIV-enabled by accepting HSPD-12 PIV credentials as a method of identity verification and authentication."
- p.88: "The vendor shall support a secure, dual factor method of remote authentication and authorization to identified Vendor Administrators that will allow vendor-designated personnel the ability to perform management duties on the system."
- p.87: "If the Government exercises this right, the contractor shall allow Government employees (or designated third parties) to conduct Security Assessment activities to include control reviews in accordance with FedRAMP requirements"
- p.13: "The transport of detainees to and from various destinations by the contractor shall be performed by the most economical and direct means, using traditional highways while being cognizant of local traffic conditions."
- p.137: "Data elements and groupings of data elements shall be clearly identifiable by labels embedded in the data itself, or by a separate schema or file layout which allows such elements and groupings to be identified."

**`75N98026Q00962`** — 9 fragment(s) removed.

- p.15: "If EFT banking information is not required to be on the invoice, in order for the invoice to be a proper invoice, the Contractor shall have submitted correct EFT banking information in accordance with the applicable solicitation provision, contract clause (e.g., 52.232-33, Payment by Electronic Fund…"
- p.23: "Unless the Schedule prescribes otherwise, the hourly rates in the Schedule shall not be varied by virtue of the Contractor having performed work on an overtime basis."
- p.15: "The Contractor shall include EFT banking information on the invoice only if required elsewhere in this contract."
- p.18: "the Contractor shall be liable to the Government for any and all rights and remedies provided by law."
- p.3: "All Offerors must have an active registration in the System for Award Management (SAM) www.sam.gov.”"
- p.15: "shall promptly give written notice to the Contracting Officer of the cessation of such occurrence"
- p.18: "l not be paid for any work performed or costs incurred which reasonably could have been avoided."
- p.15: "shall remedy such occurrence with all reasonable dispatch"
- p.6: "Maintain its working conditions."

**`80JSC026MEDEVAC5Q`** — 3 fragment(s) removed.

- p.23: "and must include solicitation number, FOB destination to the Johnson Space Center, discount/payment terms, warranty duration, taxpayer identification number (TIN), identification of any special commercial terms, and be signed by an authorized company representative with a proposal validity date."
- p.23: "Offers for the items(s) described above are due no later than September 15, 2026, at 4:00PM CT to kendrea.d.vallien@nasa.gov"
- p.23: "and shall not be marked as confidential, proprietary, competition sensitive, business information, etc."

**`W15P7T-26-R-A006`** — 52 fragment(s) removed.

- p.303: "The Offeror's conflict-of-interest mitigation plan shall be auditable by a contract oversight entity and shall include- (i) An identification, unless otherwise prohibited by law or regulation, of any covered contracts of the Offeror or its subsidiaries or affiliates with a covered foreign entity. If…"
- p.262: "If in its annual representations and certifications in SAM the Offeror has represented in paragraph (c) of the provision at 252.204-7016, Covered Defense Telecommunications Equipment or Services-Representation, that it "does" provide covered defense telecommunications equipment or services as a part…"
- p.310: "For subcontracts above the threshold for submission of certified cost or pricing data in FAR 15.403-4 to which the authority of the Section 890 pilot has not been flowed down, the Offeror shall require the subcontractor to provide certified cost or pricing data in accordance with the clause at 52.21…"
- p.167: "For subcontracts above the threshold for submission of certified cost or pricing data in FAR 15.403-4 to which the authority of the Section 890 pilot has been flowed down, in lieu of the requirements in paragraphs (a) and (b) of the clause at 52.215-13, Subcontractor Certified Cost or Pricing Data—M…"
- p.309: "(1) For subcontracts above the threshold for submission of certified cost or pricing data in FAR 15.403-4 to which the authority of the Section 890 pilot has been flowed down, in lieu of the requirements in paragraphs (a) and (b) of the clause at 52.215-12, Subcontractor Certified Cost or Pricing Da…"
- p.176: "Any contractor, subcontractor, or covered Government support contractor receiving access to the software for the purposes of subparagraphs (i), (ii), or (iii) of this paragraph shall be subject to the use and nondisclosure agreement at 227.7103-7 or be performing a Government contract that contains …"
- p.262: "The Offeror shall review the list of excluded parties in the System for Award Management (SAM) at https://www.sam.gov for entities that are excluded when providing any equipment, system, or service to carry out covered missions that uses covered defense telecommunications equipment or services as a …"
- p.121: "The Contractor shall not, without prior written permission of the Contracting Officer, incorporate in data delivered under this contract any data that are not first produced in the performance of this contract unless the Contractor (i) identifies such data and (ii) grants to the Government, or acqui…"
- p.168: "For subcontracts above the threshold for submission of certified cost or pricing data in FAR 15.403-4 to which the authority of the Section 890 pilot has not been flowed down, the Contractor shall require the subcontractor to provide certified cost or pricing data in accordance with the clause at 52…"
- p.36: "Any MAPS Prime Contractor interested in participating in task order request for proposals that possess contract line item numbers with the criteria identified in DFARS 242.7502(a) shall have an accounting system that the Defense Contract Audit Agency (DCAA), the Defense Contract Management Agency (D…"

**`W911SG27BA002`** — 39 fragment(s) removed.

- p.60: "(4) Construction by special trade contractors, it will not pay more than 75 percent of the amount paid by the Government for contract performance, excluding the cost of materials, to subcontractors that are not similarly situated entities. Any work that a similarly situated entity further subcontrac…"
- p.60: "(3) General construction, it will not pay more than 85 percent of the amount paid by the Government for contract performance, excluding the cost of materials, to subcontractors that are not similarly situated entities. Any work that a similarly situated entity further subcontracts will count towards…"
- p.17: "A bidder's failure to complete the online representations and certifications prior to the close of the Solicitation (to include listing of the applicable NAICS code and applicable size standards) will result in their bid being deemed non- responsive and eliminated from further consideration."
- p.83: "These data, furnished under the Value Engineering-Construction clause of contract ____, shall not be disclosed outside the Government or duplicated, used, or disclosed, in whole or in part, for any purpose other than to evaluate a value engineering change proposal submitted under the clause."
- p.49: "The ____ [insert name of SBA's contractor] will notify the MICC Fort Bliss Contracting Officer in writing immediately upon entering an agreement (either oral or written) to transfer all or part of its stock or other ownership interest to any other party."
- p.101: "The Contractor shall provide OSHA training to its personnel to recognize fire and safety hazards and encourage personnel in the performance of their duties to report fire and safety hazards and unsafe conditions to their supervisor."
- p.49: "When the end item being acquired is a kit of supplies, at least 50 percent of the total cost of the components of the kit shall be manufactured, processed, or produced by small businesses in the United States or its outlying areas."
- p.101: "If the work is found to be defective or nonconforming in any material respect due to the fault of the Contractor or its Subcontractors, the Contractor shall defray the expenses of the examination and of satisfactory reconstruction."
- p.17: "Bidder must be registered in the database IAW RFO FAR Part 52.204-7 in order to be awarded a contract and have the applicable NAICS code listed in RFO FAR Part 52.219-1(c), Small Business Program Representations, Representations."
- p.120: "Additional areas, view sheds, and buildings are subject to inclusion per 36 CFR Part 61 and AR 200-4, therefore, prior to commencement of any work a determination of eligibility must be made by the Historic Architect or CRM."

**`W912P726RA022`** — 38 fragment(s) removed.

- p.50: "The Offeror has completed the annual representations and certifications electronically via the SAM website at https://www.sam.gov After reviewing the SAM database information, the Offeror verifies by submission of the offer that the representations and certifications currently posted electronically …"
- p.371: "As such, USACE is required to implement the following provisions as it applies to the different areas of Project: a. Consistent with the Programmatic Essential Fish Habitat Consultation Agreement (Agreement), USACE shall conduct pre-dredge surveys of eelgrass areal coverage and density within the dr…"
- p.83: "Omissions from the drawings or specifications or the misdescription of details of work that are manifestly necessary to carry out the intent of the drawings and specifications, or that are customarily performed, shall not relieve the Contractor from performing such omitted or misdescribed details of…"
- p.153: "he shall furnish at his own expense and without cost to the Government, such guards as are necessary to give adequate warning to the public of any dangerous conditions to be encountered, and he shall furnish, erect, or maintain such lights, signs and other devices as are necessary to prevent acciden…"
- p.371: "This type of evaluation, also known as an “Integrated Alternatives Analysis,” or IAA, shall incorporate all USACE dredging projects (annual and non-annual) over as many years/dredging cycles as possible, up to a maximum of five years, and shall evaluate the practicability of the following beneficial…"
- p.375: "except that for petroleum products and hazardous substances discharges, notification shall be to the California Office of Emergency Services as soon as there is knowledge of the discharge and notification is possible without impeding cleanup or emergency measures consistent with Water Code sections …"
- p.21: "If an offeror does not have a record of relevant past performance or information on past performance is not available or so sparse that no meaningful past performance rating can be reasonably assigned, the offeror shall be determined to have unknown (or neutral) past performance (IAW FAR 15.305(a)(2…"
- p.86: "The Contractor shall be required to (a) commence work under this contract within 1 calendar days after the date the Contractor receives the notice to proceed, (b) prosecute the work diligently, and (c) complete the entire work ready for use not later than 75 calendar days.*"
- p.375: "The discharge of dredged sediments to the waters of the State shall cease immediately whenever violations of this Order are detected by USACE or by Water Board, and the discharge shall not resume until compliance can be assured to the Executive Officer's satisfaction."
- p.153: "Utilities or improvements, whether buried or not, which cannot be determined to exist through visual inspection by the Contractor, if inadvertently damaged by the Contractor's operations, shall be promptly repaired or replaced by the Contractor"

**`W912P825BA029`** — 73 fragment(s) removed.

- p.43: "If the Contractor does not have representations and certifications in SAM, or does not have a representation in SAM for the NAICS code applicable to this contract, the Contractor is required to complete the following rerepresentation and submit it to the contracting office, along with the contract n…"
- p.42: "If the Contractor represented its status as any of the small business concerns identified at 19.000(a)(3) prior to award of this contract, the Contractor shall rerepresent its size and socioeconomic status according to paragraph (f) of this clause or, if applicable, paragraph (h) of this clause, for…"
- p.144: "The Contractor shall be required to furnish, on the request of the Contracting Officer or any inspector: (1) The use of such boats, boatmen, laborers, and material forming a part of the ordinary and usual equipment and crew of the floating plant, as may be reasonably necessary in inspecting and supe…"
- p.50: "If the Contractor, after making a certified request for progress payments, discovers that a portion or all of such request constitutes a payment for performance by the Contractor that fails to conform to the specifications, terms, and conditions of this contract (hereinafter referred to as the "unea…"
- p.50: "The Contractor's request for progress payments shall include the following substantiation: (i) An itemization of the amounts requested, related to the various elements of work required by the contract covered by the payment requested. (ii) A listing of the amount included for work performed by each …"
- p.151: "The letter shall reference the following: (a) The anticipated date the dredge will work over the utility. (b) The anticipated elevation and width of dredge cut over the utility. (c) Point(s) of contact and telephone number(s). (d) Emergency procedures. (e) Any other information from the Contractor t…"
- p.80: "For every utility, the following verified information shall be provided: C/L station (for New Orleans Harbor, the 100' or 160' offset B/L station, whichever is applicable), C/L XY-coordinate (for New Orleans Harbor, the 100' or 160' offset B/L XY-coordinate, whichever is applicable), right descendin…"
- p.171: "If a dredge's submerged pump becomes inoperable and the Contractor elects to continue dredging, then the Contractor will be allowed to operate without the submerged pump provided the specifications have a bid lot for that size dredge without the submerged pump and provided the dredge meets the minim…"
- p.49: "The bonds shall be in the form of firm commitment, supported by corporate sureties whose names appear on the list contained in Treasury Department Circular 570, individual sureties, or by other acceptable security such as postal money order, certified check, cashier's check, irrevocable letter of cr…"
- p.80: ""As-Built" drawings shall include the following: a. A cover page that includes the project title, contract number, and Contractor; b. A dredging summary sheet that includes the dredge name(s), the dredging region(s) dredged, the dates dredged per each dredging region, credited yardage per each dredg…"


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
| `1616-26` | SOW | — | 13 | 15 |
| `19C02026Q0027` | SOW | — | 12 | 75 |
| `36C26126Q1034` | SOW | — | 4 | 36 |
| `70CDCR26R00000026` | PWS | — | 0 | 1 |
| `75N98026Q00962` | SOW | — | 0 | 2 |
| `80JSC026MEDEVAC5Q` | SOW | — | 12 | 17 |
| `W15P7T-26-R-A006` | PWS, SOW | — | 12 | 274 |
| `W911SG27BA002` | SOW | — | 0 | 128 |
| `W912P726RA022` | **none** | — | 0 | 162 |
| `W912P825BA029` | **none** | — | 0 | 187 |

| The six exit criteria, verbatim | `MASTER_PLAN.md`, Phase 1 |
| The audit rubric | `EXTRACTION_PROMPT_SPEC.md` §7 |
| The verdict slot | `DECISIONS.md` D-010 — still empty |


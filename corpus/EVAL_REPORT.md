# EVAL_REPORT — Phase 1 corpus evaluation

Produced 2026-08-29 by `scripts/corpus-eval.ts`.

**This report presents evidence. It does not reach a verdict.** The six exit criteria in
`MASTER_PLAN.md` are the owner's to rule on, and section 5 below lays out what the numbers
show against each without declaring pass or fail.

## Run parameters

| | |
|---|---|
| Model | `claude-sonnet-5` (from `EXTRACTION_MODEL`; nothing in code selects a model) |
| Pricing | Batch API, 50% of list. Rates verified 2026-08-29 at platform.claude.com/docs/en/about-claude/pricing |
| Extraction batches | msgbatch_01GmybQHLxUGtzQ7QJygYsbN |
| Sweeper batches | msgbatch_01TLQ4Njoc2McjmSxTdN91pj, msgbatch_01F5t47hdLdbweX9c9pttAtc |
| Random-sample seed | **20260829** — recorded so the samples below are reproducible and provably not hand-picked |
| Documents | 17 (14 readable, 3 unreadable, 0 failed) |
| Readable pages | 2,032 |
| **Total cost** | **$43.44** |

## 1. Summary table — every document

Duplicate rate is duplicates removed as a share of rows the model returned. Review-flag rate
is the share of final rows the pipeline itself marked as needing human attention.

| Document | Status | Pages | Rows | Dup rate | Dropped (invented) | Review flags | Unresolved before → after | Cost |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of` | ok | 13 | 40 | 19.6% | 1 (1) | 17.5% | 4 → **4** | $0.13 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260` | ok | 74 | 263 | 3.4% | 3 (3) | 11.8% | 110 → **10** | $1.34 |
| `15F06726R0000194__RFP-15F06726R0000194-Tie` | ok | 75 | 385 | 7.8% | 10 (10) | 19.0% | 89 → **13** | $1.94 |
| `1616-26__RFP1620000348` | ok | 104 | 353 | 4.1% | 4 (4) | 25.8% | 144 → **17** | $1.99 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | ok | 81 | 365 | 1.6% | 8 (8) | 13.7% | 171 → **17** | $2.28 |
| `36C26026Q0939__SF-1449-36C26026Q0939-Stora` | **UNREADABLE** | 39 | — | — | — | — | — | $0.00 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VA` | ok | 73 | 255 | 8.8% | 15 (14) | 20.0% | 62 → **9** | $1.39 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | ok | 8 | 5 | 0.0% | 9 (9) | 40.0% | 12 → **5** | $0.34 |
| `70CDCR26R00000026__Attachment-01-Turnkey-F` | ok | 152 | 1316 | 4.9% | 22 (13) | 21.1% | 671 → **53** | $6.62 |
| `75N98026Q00962__RFQ-75N98026Q00962` | ok | 39 | 159 | 1.6% | 0 (0) | 32.1% | 76 → **7** | $1.11 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC0` | ok | 23 | 132 | 2.3% | 2 (2) | 16.7% | 14 → **6** | $0.50 |
| `PANMCC26P0000048766__Combined-Synopsis` | **UNREADABLE** | 33 | — | — | — | — | — | $0.00 |
| `W15P7T-26-R-A006__Solicitation-Amendment-0` | ok | 342 | 945 | 10.2% | 41 (39) | 27.2% | 397 → **85** | $6.27 |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicitati` | **UNREADABLE** | 144 | — | — | — | — | — | $0.00 |
| `W911SG27BA002__Solicitation-Amendment-W911` | ok | 157 | 781 | 3.8% | 24 (22) | 23.6% | 362 → **20** | $4.17 |
| `W912P726RA022__W912P726RA002-San-Rafael-So` | ok | 645 | 1712 | 5.2% | 26 (21) | 27.9% | 571 → **65** | $9.10 |
| `W912P825BA029__Solicitation-W912P825BA029-` | ok | 246 | 1333 | 5.3% | 30 (18) | 16.7% | 398 → **42** | $6.26 |

**Totals across readable documents:** 8,044 rows · 164 rows dropped as unlocatable · 2882 rows the sweeper recovered that the first pass missed · unresolved 3081 → 353.

## 2. The unreadable documents

These are in the corpus on purpose. v1 does not OCR (DECISIONS.md D-004), so the correct
behaviour is to declare them unreadable and charge nothing — not to return a thin matrix.
They were **not skipped**: each was ingested, measured, and classified.

| Document | Pages | Low-text pages | Chars/page | What the user would be told |
|---|---:|---:|---:|---|
| `36C26026Q0939__SF-1449-36C26026Q0939-Sto` | 39 | 39 | 0 | This PDF appears to be scanned images rather than text: 39 of 39 pages carry almost no ext |
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

**13 pages · 40 rows · unresolved 4 · cost $0.13**
Rows dropped because their quote could not be found in the document: **1**.

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

*No pages were flagged on this document.*

### 1240LT26Q0172__3-Combined-SpecsCBPEWI20260624

Path: `corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf`

**74 pages · 263 rows · unresolved 10 · cost $1.34**
Rows dropped because their quote could not be found in the document: **3**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 7 | 011000 §1.3.F | shall | high | The Site shall be left in a safe and secure manner with the public protected from construction hazards. |
| 2 | 16 | 013100 §1.6.A | imperative | high | Post copies of this list in the temporary field office (if present). |
| 3 | 26 | 017700 1.4.A.4 | imperative | high | Submit a signed copy of the Substantial Completion inspection list of items to be completed or corrected (punch list), endorsed and dated by Government CO/COR. |
| 4 | 31 | 221114 2.1 | shall | **review** | There shall be no other openings in the well cap. |
| 5 | 31 | 221114 2.1 | shall | high | The well cap shall be lockable, Baker-Monitor well cap part number 6WTCL or approved equal. |
| 6 | 35 | 260010 1.2.C | shall | high | The Contractor shall provide and install the electrical system as shown on the drawings and indicated in the specifications. |
| 7 | 43 | 260010 | shall | high | Where not otherwise indicated, grounding conductor size shall conform to the most stringent of the governing codes. |
| 8 | 44 | 3.17.E.2 | shall | high | Minimum resistance shall be 200 megohms. |
| 9 | 54 | 260400 | will | **review** | Excessive bending of the cable will not be permitted. |
| 10 | 63 | 329200 I | shall | high | Native seed mix shall be mixed thoroughly by vendor/supplier at their recommended ratios. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 30 | 16 | 0 | 16 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 56 | 14 | 0 | 14 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 54 | 8 | 0 | 8 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 30
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 56
node scripts/extract.ts ../shallfinder-corpus/corpus/1240LT26Q0172__3-Combined-SpecsCBPEWI20260624.pdf --page 54
```

### 15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer

Path: `corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf`

**75 pages · 385 rows · unresolved 13 · cost $1.94**
Rows dropped because their quote could not be found in the document: **10**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 12 | B.12 | shall | high | The Prime Hauler unit price shall be all-inclusive. |
| 2 | 18 | D.4 | shall | high | Passwords, administrative credentials, cryptographic information, or other sensitive access information shall not be placed in an unsecured shipping container or displayed on an exterior packing list. |
| 3 | 20 | E.4 | shall | high | The Contractor shall not begin irreversible vehicle fabrication before the Contracting Officer provides the written authorization required following Critical Design Review, except for long-lead materials or activities expressly au |
| 4 | 22 | E.13 | conditional | high | Payment, possession, operational use, or Government participation in testing does not constitute acceptance unless the authorized acceptance record has been executed. |
| 5 | 25 | F.10 | shall | high | Unless an individual delivery order expressly states otherwise, all deliveries shall be F.o.b. destination. |
| 6 | 27 | G.9 | declaration | high | Unordered work, unexercised options, unaccepted deliverables, and costs incurred in anticipation of an order are not invoiceable. |
| 7 | 28 | G.15 | shall | high | After award, the Contractor shall promptly notify the Contracting Officer and payment office of an authorized change. |
| 8 | 29 | G.16 | shall | high | The Contractor shall promptly notify the Contracting Officer of changes to its legal name, ownership, address, points of contact, SAM registration, UEI, CAGE code, banking information, production facility, key personnel, ISO 9001  |
| 9 | 45 | FBI-0019 | shall | high | Final delivery of goods shall be completed from the awardee’s domestic offices and supply centers to the FBI. |
| 10 | 49 | 52.215-1(b) | shall | high | Offerors shall acknowledge receipt of any amendment to this solicitation by the date and time specified in the amendment(s). |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 33 | 19 | 12 | 7 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 21 | 15 | 9 | 6 | yes | 2 exclusion(s): government-actor — resolved |
| 24 | 12 | 6 | 6 | yes | 2 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 33
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 21
node scripts/extract.ts ../shallfinder-corpus/corpus/15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer.pdf --page 24
```

### 1616-26__RFP1620000348

Path: `corpus/1616-26__RFP1620000348.pdf`

**104 pages · 353 rows · unresolved 17 · cost $1.99**
Rows dropped because their quote could not be found in the document: **4**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 38 | Section C | shall | high | These facilities shall exist sufficiently to receive all product off site serving as a staging area. |
| 2 | 40 | *(none)* | shall | high | Offeror(s) shall report all truck shortages, damage report, and site conditions that may result in an untimely completion of an installation. |
| 3 | 40 | *(none)* | shall | high | The inspection, final shortage sheet, and final punch sheet shall be completed prior to the completion of the installation. |
| 4 | 43 | *(none)* | shall | high | the installer shall walk through with the customer to inspect the product and installation. |
| 5 | 43 | *(none)* | shall | high | A written quote of storage charges shall be provided to the Project Manager for storage fees at least 2 weeks prior to storage charges beginning. |
| 6 | 50 | *(none)* | shall | **review** | UNICOR shall not be invoiced for installation services until the job is completed 100%. |
| 7 | 61 | (h) | shall | high | The Contractor shall indemnify the Government and its officers, employees, and agents against liability, including costs, for actual or alleged direct or contributory infringement of, or inducement to infringe, any United States o |
| 8 | 90 | C.(2)(c) | shall | high | The Contractor shall not retain, use, sell, disseminate, or dispose of any government data/records or deliverables without the express written permission of the Contracting Officer or Contracting Officer's Representative. |
| 9 | 92 | D.(3)(b) | shall | **review** | work with personnel from the program office, OPCL, the Office of the Chief information Officer (OCIO), and the Office of Records Management and Policy to ensure that the privacy assessments and documentation are kept on schedule,  |
| 10 | 97 | 52.212-1(b) | declaration | high | The Offeror agrees to hold the prices in its offer firm for 60 calendar days from the date specified for receipt of offers, unless another time period is specified in an addendum to the solicitation. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 43 | 15 | 0 | 15 | yes | 2 exclusion(s): government-actor — resolved |
| 41 | 13 | 0 | 13 | yes | 1 exclusion(s): government-actor — resolved |
| 40 | 10 | 0 | 10 | yes | 2 exclusion(s): government-actor, toc-echo — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 43
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 41
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 40
```

### 19C02026Q0027__Solicitation-19C02026Q0027

Path: `corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf`

**81 pages · 365 rows · unresolved 17 · cost $2.28**
Rows dropped because their quote could not be found in the document: **8**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 14 | G.1.1 | shall | high | G.1.1 The Contractor shall provide the information required by the paragraph above within ten (10) calendar days after award. |
| 2 | 14 | G.2.2 | shall | high | The Contractor shall obtain any other types of insurance required by local law or that are ordinarily or customarily obtained in the location of the work. |
| 3 | 26 | 52.240-91(b)(1) | must | high | (1) A covered application on any information technology owned or managed by the Government, or on any information technology used or provided by the Contractor under this contract, including equipment provided by the Contractor’s  |
| 4 | 47 | SOW 2.2 | imperative | high | Laser-Cut Lattice Assembly: Fabricate and assemble decorative and structural lattices via high-precision laser cutting according to the approved architectural layouts. |
| 5 | 47 | SOW 2.2 | imperative | high | GMAW Welding Operations: Execute all structural steel assembly and welding exclusively through the Gas Metal Arc Welding (GMAW) process performed by certified welding personnel under AWS D1.1 structural welding code requirements. |
| 6 | 52 | SOW 6.1 | imperative | high | Submit all structural designs, calculation memories, and product submittals for formal review and approval by the COR prior to commencing any off-site fabrication. |
| 7 | 54 | SOW 6.6 | imperative | high | Chip away loose aggregate from the seven (7) exposed aggregate columns ("Gravilla Lavada"), patch voids using high-strength structural repair mortar matching the original mix design and texture, pressure wash, and apply an approve |
| 8 | 59 | SOW 13 | shall | high | The Contractor shall provide 24 hours advance notice for the following mandatory inspection milestones: |
| 9 | 66 | Safety Requirements §3e | conditional | high | In those tasks where PPE certified is required, the contractor must provide the current certification. |
| 10 | 80 | *(none)* | imperative | high | Barra pisos y déjelos limpios. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 69 | 16 | 0 | 16 | yes | 1 exclusion(s): government-actor — resolved |
| 67 | 13 | 0 | 13 | yes | 1 exclusion(s): government-actor — resolved |
| 29 | 11 | 1 | 10 | yes | 1 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 69
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 67
node scripts/extract.ts ../shallfinder-corpus/corpus/19C02026Q0027__Solicitation-19C02026Q0027.pdf --page 29
```

### 36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-

Path: `corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf`

**73 pages · 255 rows · unresolved 9 · cost $1.39**
Rows dropped because their quote could not be found in the document: **14**.

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

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 55 | 12 | 4 | 8 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 34 | 8 | 1 | 7 | yes | 5 exclusion(s): government-actor, definitional — resolved |
| 71 | 6 | 1 | 5 | yes | 1 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 55
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 34
node scripts/extract.ts ../shallfinder-corpus/corpus/36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-.pdf --page 71
```

### 47QMCA26Q0098__RFQ47QMCA26Q0098-SF18

Path: `corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf`

**8 pages · 5 rows · unresolved 5 · cost $0.34**
Rows dropped because their quote could not be found in the document: **9**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 2 | B.1 | will | **review** | Partial minimum quoted quantities will NOT be evaluated. |
| 2 | 7 | *(none)* | shall | high | For purposes of price evaluation, offerors shall provide a unit price for each vehicle. |
| 3 | 7 | *(none)* | shall | **review** | The evaluated unit price shall be applied to the quantity determined by the Government at the time of award. |
| 4 | 7 | *(none)* | shall | high | Offerors shall not assume that the Government will award sixty (60) vehicles for purposes of pricing, and the Government will not be obligated to purchase quantities exceeding the minimum quantity stated herein. |
| 5 | 8 | E.1.2 | required | high | Vendors are required to complete the GSA Source of Supply Letter. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 2 | 3 | 0 | 3 | yes | 3 exclusion(s): government-actor — resolved |
| 7 | 3 | 0 | 3 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 8 | 3 | 0 | 3 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf --page 2
node scripts/extract.ts ../shallfinder-corpus/corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf --page 7
node scripts/extract.ts ../shallfinder-corpus/corpus/47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf --page 8
```

### 70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem

Path: `corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf`

**152 pages · 1316 rows · unresolved 53 · cost $6.62**
Rows dropped because their quote could not be found in the document: **13**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 9 | PWS 9.1 | will | high | All furniture and case goods will be furnished by the Contractor, throughout the length of the contract, and be in good working order with no damage. |
| 2 | 9 | PWS 9.2 | shall | high | OPLA space shall be contiguous. |
| 3 | 13 | *(none)* | will | **review** | Estimated anticipated routes and/or mileage will be provided to the COR for this requirement. |
| 4 | 19 | PWS 21.2 | shall | high | All exterior cameras shall have an infrared (IR) cut-filter and/or IR night time illuminator either built in or added as an accessory. |
| 5 | 59 | PWS - Personnel Requirements | required | **review** | Maintain sufficient qualified staff, including licensed trades as required. |
| 6 | 76 | *(none)* | must | high | Before receiving access to information resources under this contract, the individual must complete a security briefing; additional training for specific categories of CUI, if identified in the contract; and any nondisclosure agree |
| 7 | 99 | *(none)* | conditional | high | upon request by the CO, the Contractor shall deliver such records to a location specified by the CO for inspection, copying, and audit |
| 8 | 108 | PWS 10.b | shall | **review** | b. Employees shall not discuss or disclose information from alien files or immigration cases, except, when necessary, in the performance of duties under this contract. |
| 9 | 121 | *(none)* | must | high | Notice of any price increases must be provided to the COR. |
| 10 | 126 | PWS 66.1 | shall | high | The Contractor shall provide well maintained and serviceable or new firearms and maintain enough licensed firearms and ammunition to equip each armed detention officer and armed supervisor(s) with a licensed weapon while on duty. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 81 | 20 | 0 | 20 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 121 | 19 | 0 | 19 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 32 | 18 | 0 | 18 | yes | 1 exclusion(s): duplicate-of-captured — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 81
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 121
node scripts/extract.ts ../shallfinder-corpus/corpus/70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem.pdf --page 32
```

### 75N98026Q00962__RFQ-75N98026Q00962

Path: `corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf`

**39 pages · 159 rows · unresolved 7 · cost $1.11**
Rows dropped because their quote could not be found in the document: **0**.

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

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 15 | 9 | 0 | 9 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 16 | 6 | 0 | 6 | yes | 6 exclusion(s): government-actor, definitional — resolved |
| 38 | 8 | 2 | 6 | yes | 5 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 15
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 16
node scripts/extract.ts ../shallfinder-corpus/corpus/75N98026Q00962__RFQ-75N98026Q00962.pdf --page 38
```

### 80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final

Path: `corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf`

**23 pages · 132 rows · unresolved 6 · cost $0.50**
Rows dropped because their quote could not be found in the document: **2**.

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

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 6 | 9 | 6 | 3 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 18 | 12 | 10 | 2 | yes | 2 exclusion(s): government-actor — resolved |
| 22 | 4 | 2 | 2 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 6
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 18
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 22
```

### W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006

Path: `corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf`

**342 pages · 945 rows · unresolved 85 · cost $6.27**
Rows dropped because their quote could not be found in the document: **39**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 9 | PWS 4.0 | shall | high | The contractor shall provide the services and documentation required by individual task orders pursuant to the general requirements specified herein. |
| 2 | 34 | H.13.6 | shall | high | After any document for a contract or order have been released, even if only in draft form, contractors shall not communicate with anyone other than the Contracting Officer or Contract Specialist; this includes any requiring activi |
| 3 | 99 | *(none)* | shall | high | the 50 percent limitation shall apply only to the service portion of the contract; |
| 4 | 133 | *(none)* | conditional | high | If at any time during performing this contract, the Contractor has reason to believe that the total price to the Government for performing this contract will be substantially greater or less than the then stated ceiling price, the |
| 5 | 133 | *(none)* | conditional | **review** | the Contractor shall not be obligated to continue performance if to do so would exceed the ceiling price set forth in the Schedule, unless and until the Contracting Officer notifies the Contractor in writing that the ceiling price |
| 6 | 150 | (e)(2) | shall | high | (2) Enter into SPRS the results of a current self-assessment for each CMMC UID, not covered by a C3PAO assessment or DIBCAC assessment, applicable to each of the contractor information systems that process, store, or transmit FCI  |
| 7 | 150 | (e)(3) | shall | **review** | (3) Complete in SPRS on an annual basis and maintain as current an affirmation of continuous compliance by the affirming official (see 32 CFR 170.4) for each self-assessment, C3PAO |
| 8 | 199 | *(none)* | shall | **review** | Technical data, including computer software documentation, or computer software that will be delivered, furnished, or otherwise provided to the Government under this contract, in which the Government has previously obtained rights |
| 9 | 275 | *(none)* | declaration | high | Any non-CUI files uploaded will not be evaluated. |
| 10 | 277 | L.1.1.3 | declaration | high | Failure to submit a copy of the Joint Venture Agreement with the proposal, meeting these requirements will result in the proposal being rejected. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 275 | 17 | 0 | 17 | yes | 2 exclusion(s): government-actor — resolved |
| 273 | 13 | 0 | 13 | yes | 1 exclusion(s): toc-echo — resolved |
| 136 | 10 | 0 | 10 | yes | 1 exclusion(s): government-actor — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 275
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 273
node scripts/extract.ts ../shallfinder-corpus/corpus/W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006.pdf --page 136
```

### W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001

Path: `corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf`

**157 pages · 781 rows · unresolved 20 · cost $4.17**
Rows dropped because their quote could not be found in the document: **22**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 16 | 12.c | shall | **review** | The Bidders shall provide page 3 and 4 of theTAB C: CLIN Price and Total Contract Amount. solicitation pricing each CLIN individually and include the OVERALL contract price. |
| 2 | 48 | 252.219-7010(a) | declaration | **review** | (a) Offers are solicited only from small business concerns expressly certified by the Small Business Administration (SBA) for participation in SBA's 8(a) Program and which meet the following criteria at the time of submission of o |
| 3 | 97 | SOW 1.3.3 | shall | high | The Contractor shall provide all equipment and tools necessary to complete the scope of work as described in this document. |
| 4 | 100 | SOW 1.8 | shall | high | The Contractor shall be responsible for performing or having performed all inspections and tests necessary to substantiate that the raw materials, components, intermediate assemblies, and end products furnished & installed under t |
| 5 | 106 | PWS 1.19.5 | shall | high | The Contractor shall not be entitled to any equitable adjustment of the contract price or extension of the performance schedule on any stop work order issued. |
| 6 | 109 | 1.27 | shall | high | The Contractor shall designate these individuals in writing and provide cell phone numbers to the KO prior to performance. |
| 7 | 120 | PWS 4.11 | shall | high | The Contractor shall test for asbestos and provide the test results to the COR & KO before proceeding for additional guidance. |
| 8 | 123 | PWS 4.20 | shall | high | Contractor shall comply with Texas Commission on Environmental Quality, Chapter 290 - Public Drinking Water, 290.101 - 290.119, 290.121, 290.122, Effective March 30, 2017, or most current approved version. |
| 9 | 139 | PWS 4.72.3 | imperative | high | Show measurements for all change of direction points and all surface or underground components such as valves, manholes, drop inlets, cleanouts, and meters. |
| 10 | 140 | PWS 4.75 | shall | high | The Contractor shall provide to COR and KO, a manufacturer's warranty certificate. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 104 | 20 | 0 | 20 | yes | 1 exclusion(s): government-actor — resolved |
| 109 | 20 | 0 | 20 | yes | 2 exclusion(s): government-actor — resolved |
| 105 | 19 | 0 | 19 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 104
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 109
node scripts/extract.ts ../shallfinder-corpus/corpus/W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001.pdf --page 105
```

### W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-

Path: `corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf`

**645 pages · 1712 rows · unresolved 65 · cost $9.10**
Rows dropped because their quote could not be found in the document: **21**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 22 | 4.3.1 | shall | high | The narrative shall be task-oriented, clearly indicating the number of calendar days following pre-construction submittals and mobilization— assumed to occur approximately 30 calendar days after Notice to Proceed (NTP)—by which dr |
| 2 | 82 | 252.236-7001 | shall | high | (2) Compare all drawings and verify the figures before laying out the work; |
| 3 | 117 | *(none)* | shall | **review** | This paragraph shall not be construed to apply to work below ground level in open cut. |
| 4 | 161 | 01 32 01.00 10 3.3.7.3 | declaration | high | Activities cannot have more than one Work Area Code. |
| 5 | 185 | 1.9 | declaration | high | No delay damages or time extensions will be allowed for time lost in late submittals. |
| 6 | 205 | PWS 1.13 | shall | high | All extinguishers shall be current inspection tagged, approved safety pin and tamper resistant seal. |
| 7 | 218 | 01 35 26 3.8.6 | shall | high | The Contractor shall furnish and install an obstruction marking and lighting in accordance with the requirements of FAA Publication Advisory Circular 70/7460-lK. |
| 8 | 238 | 1.7.1.3 | declaration | **review** | Letters are numbered starting from 0001. |
| 9 | 301 | 3.6.1 | shall | high | No debris or material other than natural mud, sand or silt shall be deposited in the Government-furnished open disposal area. |
| 10 | 303 | 3.6.2.7 | conditional | high | In such cases the towing vessel's position, and the tow cable length and compass heading to the disposal vessel at the time of discharge, must be recorded and reported. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 207 | 16 | 0 | 16 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 213 | 15 | 0 | 15 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 215 | 11 | 0 | 11 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 207
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 213
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-.pdf --page 215
```

### W912P825BA029__Solicitation-W912P825BA029-OM25035

Path: `corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf`

**246 pages · 1333 rows · unresolved 42 · cost $6.26**
Rows dropped because their quote could not be found in the document: **18**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 77 | 1.5 | shall | high | shall at all times follow the directions and instructions of the Contracting Officer or his/her authorized representative in regard to the payment of such taxes, fees, or charges. |
| 2 | 81 | 1.12 | shall | high | The Contractor shall submit the draft as-builts for review by email in PDF format to the respective area office personnel. |
| 3 | 98 | 1.7 | shall | high | Lettering for the project name shall be Helvetica Bold, and all other lettering shall be Helvetica Regular. |
| 4 | 124 | PWS 3.1.4.a | shall | high | In areas of consolidated bottom material, the digitized and recorded depth soundings shall indicate the true channel bottom. |
| 5 | 141 | 2.1.2.e | shall | high | During dredging, these instruments shall record data on control chart paper consisting of pipeline pressure and pump vacuum (lbs/cu in.), and pump RPM at rated drive of the prime mover. |
| 6 | 142 | 2.1.3.4 | shall | high | The towboat(s) shall have horsepower to provide a minimum towing speed of three miles per hour upriver against a current of five miles per hour for moving a single tow consisting of the dredge and necessary attendant plant to comm |
| 7 | 144 | 2.1.7 | shall | high | In addition, the unit shall have electronic charting with inland and offshore coverage using the most current imagery available. |
| 8 | 144 | 2.1.8.a | shall | high | The survey crew shall be prepared to perform survey work within a one hour notice from Government Inspectors. |
| 9 | 223 | 3.1.4 | shall | high | Cross-sections shall be taken normal to the beach, and spacing between the cross sections shall not exceed 100 feet. |
| 10 | 243 | PWS 3.2.6 | conditional | high | If the serial transmission option is used, sensor data shall be sent to the DQM computer via an RS-232 serial interface with a baud rate of 9600 or 19200 bps. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 98 | 25 | 0 | 25 | yes | 1 exclusion(s): definitional — **still unresolved** |
| 122 | 18 | 0 | 18 | yes | 0 exclusion(s): no exclusions stated — resolved |
| 118 | 15 | 0 | 15 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 98
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 122
node scripts/extract.ts ../shallfinder-corpus/corpus/W912P825BA029__Solicitation-W912P825BA029-OM25035.pdf --page 118
```

## 4. The invention count, per document

A row is dropped when its quoted text cannot be located anywhere in the document. This is the
structural defence against a fabricated citation, and the count is what the defence caught.
A dropped row never reaches a user — but a rising count means the model is leaning on the net.

| Document | Rows kept | Dropped: not found | Dropped: bad shape | Dropped: page out of range |
|---|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-W` | 40 | **1** | 0 | 0 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI2026062` | 263 | **3** | 0 | 0 |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-` | 385 | **10** | 0 | 0 |
| `1616-26__RFP1620000348` | 353 | **4** | 0 | 0 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 365 | **8** | 0 | 0 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPI` | 255 | **14** | 1 | 0 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | 5 | **9** | 0 | 0 |
| `70CDCR26R00000026__Attachment-01-Turnkey-Fac` | 1316 | **13** | 9 | 0 |
| `75N98026Q00962__RFQ-75N98026Q00962` | 159 | **0** | 0 | 0 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027` | 132 | **2** | 0 | 0 |
| `W15P7T-26-R-A006__Solicitation-Amendment-004` | 945 | **39** | 2 | 0 |
| `W911SG27BA002__Solicitation-Amendment-W911SG` | 781 | **22** | 2 | 0 |
| `W912P726RA022__W912P726RA002-San-Rafael-Soli` | 1712 | **21** | 5 | 0 |
| `W912P825BA029__Solicitation-W912P825BA029-OM` | 1333 | **18** | 12 | 0 |

**Total dropped as unlocatable across the corpus: 164.**

## 5. The six exit criteria — what the numbers show

Ranked by how much of the decision rests on each. **No verdict is offered here.**

### 1. Catches everything (the recall check) — the criterion the project turns on

The owner reads two pages per readable document by hand and counts what the pipeline missed.
**This report deliberately does not choose those pages.**

What the machine can say about its own recall:

- The independent scan found **6842** binding-verb occurrences across readable documents.
- After the sweeper, **353** occurrences remain neither captured nor explained.
- The sweeper recovered **2882** requirements the first pass had missed. The first pass alone is measurably not enough.
- The scan sees Class-A verbs only. Imperatives, conditionals, and declarations carry no magic
  word and are invisible to it, so this measures one kind of miss, not all kinds. The hand audit
  is the only check that covers the rest.

### 2. Nothing invented

- **164** rows were dropped corpus-wide because their quote could not be found.
- Every row that reached the matrix had its quote located in the source page text.
- The check itself was proven by planting a fake row and a paraphrase, both rejected; disabling
  the check turns those tests red. See the 2026-08-29 Task 3 report.

### 3. Coverage check honest

- Unresolved across readable documents: **3081 before the sweeper, 353 after.**
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

- 3 of 17 documents were declared unreadable and returned no matrix.
- They appear in section 2 with their measured page counts and text density, not as skips.

### 6. Cost measured and recorded per document

- **$43.44** for the corpus at Batch API rates.
- Per-document figures are in the summary table and the per-document JSON.
- MASTER_PLAN sets no pass mark on cost: pricing is decided after the real number is known.

## 6. Where everything is

| What | Where |
|---|---|
| Per-document JSON, including every row | `corpus/eval/` |
| The corpus documents | `corpus/` |
| The six exit criteria, verbatim | `MASTER_PLAN.md`, Phase 1 |
| The audit rubric | `EXTRACTION_PROMPT_SPEC.md` §7 |
| The verdict slot | `DECISIONS.md` D-010 — still empty |


## 7. Two findings the owner must weigh before ruling

These are computed from the per-document JSON in `corpus/eval/` and are unfavourable.

### 7.1 Chunks that produced nothing — a hole in recall

**46 of 263 chunk responses hit the output ceiling and were cut mid-JSON, and 47 chunks produced no rows at all.** A lost chunk means the requirements on its pages are absent from the matrix, and unlike the interactive path the batch run has no retry.

This is the same failure found and fixed on 2026-08-28, reappearing at the larger ceiling on the densest chunks. It is a known, named defect, not a mystery — but it means **roughly 18% of chunks contributed nothing to this evaluation**, and the recall criterion is being judged on a matrix with those gaps in it.

| Document | Chunks | Truncated | Yielded nothing |
|---|---:|---:|---:|
| `70CDCR26R00000026__Attachment-01-Turnkey-Fac` | 26 | 13 | **12** |
| `W912P726RA022__W912P726RA002-San-Rafael-Soli` | 82 | 9 | **9** |
| `19C02026Q0027__Solicitation-19C02026Q0027.pd` | 11 | 5 | **5** |
| `W911SG27BA002__Solicitation-Amendment-W911SG` | 20 | 5 | **5** |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI2026062` | 7 | 4 | **4** |
| `W15P7T-26-R-A006__Solicitation-Amendment-004` | 40 | 4 | **4** |
| `1616-26__RFP1620000348.pdf` | 10 | 3 | **3** |
| `W912P825BA029__Solicitation-W912P825BA029-OM` | 34 | 1 | **3** |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-` | 10 | 1 | **1** |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf` | 1 | 1 | **1** |

### 7.2 One document invented more rows than it kept

`47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf` — **9 rows dropped as unlocatable against 5 kept.** Every one was caught by the locate-check and none reached the matrix, so nothing false is published. But on this document the model produced more unfindable quotes than findable ones, which is worth the owner's eye during the audit.

Invention rate across all readable documents, dropped-not-found as a share of rows kept:

| Document | Dropped (not found) | Rows kept | Rate |
|---|---:|---:|---:|
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf` | 9 | 5 | 180.0% |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPI` | 14 | 255 | 5.5% |
| `W15P7T-26-R-A006__Solicitation-Amendment-004` | 39 | 945 | 4.1% |
| `W911SG27BA002__Solicitation-Amendment-W911SG` | 22 | 781 | 2.8% |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-` | 10 | 385 | 2.6% |
| `0020153254COHEN__Attachment-1-Statement-of-W` | 1 | 40 | 2.5% |
| `19C02026Q0027__Solicitation-19C02026Q0027.pd` | 8 | 365 | 2.2% |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027` | 2 | 132 | 1.5% |
| `W912P825BA029__Solicitation-W912P825BA029-OM` | 18 | 1333 | 1.4% |
| `W912P726RA022__W912P726RA002-San-Rafael-Soli` | 21 | 1712 | 1.2% |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI2026062` | 3 | 263 | 1.1% |
| `1616-26__RFP1620000348.pdf` | 4 | 353 | 1.1% |
| `70CDCR26R00000026__Attachment-01-Turnkey-Fac` | 13 | 1316 | 1.0% |
| `75N98026Q00962__RFQ-75N98026Q00962.pdf` | 0 | 159 | 0.0% |


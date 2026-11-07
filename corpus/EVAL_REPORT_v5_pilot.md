# EVAL REPORT v5 pilot — Phase 1 corpus evaluation

Produced 2026-11-07 by `scripts/corpus-eval.ts`.

**This report presents evidence. It does not reach a verdict.** The six exit criteria in
`MASTER_PLAN.md` are the owner's to rule on, and section 5 below lays out what the numbers
show against each without declaring pass or fail.

## Run parameters

| | |
|---|---|
| Model | `claude-sonnet-5` (from `EXTRACTION_MODEL`; nothing in code selects a model) |
| Pricing | Batch API, 50% of list. Rates verified 2026-08-29 at platform.claude.com/docs/en/about-claude/pricing |
| Extraction batches | msgbatch_017fJHyyRdcKthXGwp4qWmbE |
| Sweeper batches | msgbatch_01A5UYHpUZRNcUV4hG3XdVff |
| Random-sample seed | **20260829** — recorded so the samples below are reproducible and provably not hand-picked |
| Documents | 3 (3 readable, 0 unreadable, 0 failed) |
| Readable pages | 140 |
| **Total cost** | **$2.45** |

## 1. Summary table — every document

Duplicate rate is duplicates removed as a share of rows the model returned. Review-flag rate
is the share of final rows the pipeline itself marked as needing human attention.

| Document | Status | Pages | Rows | Dup rate | Dropped (invented) | Review flags | Trunc / lost chunks | Unresolved before → after | Cost |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of` | ok | 13 | 44 | 2.4% | 3 (0) | 29.5% | 0 / **0** | 2 → **1** | $0.15 |
| `1616-26__RFP1620000348` | ok | 104 | 346 | 12.8% | 47 (1) | 42.8% | 1 / **1** | 54 → **16** | $1.64 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC0` | ok | 23 | 115 | 26.1% | 8 (0) | 21.7% | 0 / **0** | 14 → **2** | $0.66 |

**Totals across readable documents:** 505 rows · 1 rows dropped as unlocatable · 1 rows the sweeper recovered that the first pass missed · unresolved 70 → 19.

## 2. The unreadable documents

These are in the corpus on purpose. v1 does not OCR (DECISIONS.md D-004), so the correct
behaviour is to declare them unreadable and charge nothing — not to return a thin matrix.
They were **not skipped**: each was ingested, measured, and classified.

| Document | Pages | Low-text pages | Chars/page | What the user would be told |
|---|---:|---:|---:|---|

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

**13 pages · 44 rows · unresolved 1 · cost $0.15**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 4 | SOW 4 | shall | **review** | The Contractor shall complete the following tasks as required to comply with this SOW. |
| 2 | 5 | SOW 4.1 | must | **review** | • Compliance with MIL-STD-188-110D and MIL-STD-188-141D • Interoperability with the existing COTHEN High Frequency Cellular Network, including compatibility with Collins Aerospace software and protocols currently in use • All othe |
| 3 | 5 | SOW 4.2 | shall | high | The Contractor shall test and functionally certify all equipment prior to delivery to ensure compliance and specifications. |
| 4 | 5 | SOW 5 | shall | high | The Contractor shall deliver equipment as outlined below. |
| 5 | 6 | SOW 5.1 | must | high | The Government Purchase Order number must be reflected on the Shipping label and clearly delineated on the packing slip(s). |
| 6 | 9 | SOW 10.4 | must | **review** | Any such proposed changes must be brought to the immediate attention of the CO for action. |
| 7 | 9 | SOW 10.5 | conditional | high | If the Contractor is unable to comply with the requirement to use IPP for submitting invoices for payment, the Contractor must submit a waiver request in writing to the Contracting Officer. |
| 8 | 12 | Addendum B | shall | high | Description information for all data assets, information exchanges and data standards, whether adopted or developed, shall be submitted to the Enterprise Data Management Office (EDMO) for review, approval, and insertion into the D |
| 9 | 12 | Addendum B | will | **review** | Development of data assets, information exchanges, and data standards will comply with the DHS Data Management Policy MD 103-01. |
| 10 | 12 | Addendum B | shall | high | All EA related component acquisitions shall be IPv6 compliant, as defined in the USGv6 Profile (NIST Special Publication 500-267) and the corresponding declarations of conformance, defined in the USGv6 Test Program. |

#### The 3 flagged pages with the largest shortfall

*No pages were flagged on this document.*

### 1616-26__RFP1620000348

Path: `corpus/1616-26__RFP1620000348.pdf`

**104 pages · 346 rows · unresolved 16 · cost $1.64**
Rows dropped because their quote could not be found in the document: **1**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 41 | SOW | shall | high | The contractor shall cover the floors with protective materials, i.e., plywood, or similar durable covering. |
| 2 | 41 | SOW | shall | high | The contractor shall at all times keep the job site, including staging and storage areas used by the contractor, free from accumulation of waste material or rubbish. |
| 3 | 41 | SOW | will | **review** | Before and after pictures of the staging area and job site will be included in the daily reports. |
| 4 | 49 | SOW | required | high | It is the vendor’s responsibility to verify if their proposal was received by the UNICOR Contracting Officer. |
| 5 | 51 | SOW | declaration | **review** | (The map provided by UNICOR as part of this solicitation is the only map authorized for use in the solicitation/ contract). |
| 6 | 51 | SOW | required | high | The site manager is also responsible for coordinating all deliveries and ensuring the deadlines are met. |
| 7 | 51 | SOW | must | **review** | regional site installation manager positions must be full time employees of offeror (s) and may be required by UNICOR to be on-site for high profile, high value, or problem projects. |
| 8 | 54 | 52.211-11(b) | required | **review** | (b) If the Government terminates this contract in whole or in part under the Default-Fixed-Price Supply and Service clause, the Contractor is liable for liquidated damages accruing until the Government reasonably obtains delivery  |
| 9 | 85 | DOJ-02 A.(3) | required | high | The Contractor (and any Subcontractor) is required to abide by Government and Agency guidance for protecting sensitive and proprietary information. |
| 10 | 87 | DOJ-02 B.(3) | declaration | **review** | The Contractor should maintain signed copies of the NDA for all employees as a record of compliance. |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 82 | 8 | 1 | 7 | yes | 0 exclusion(s): no exclusions stated — **still unresolved** |
| 64 | 8 | 4 | 4 | yes | 3 exclusion(s): government-actor — resolved |
| 42 | 11 | 9 | 2 | yes | 3 exclusion(s): permission, government-actor — **still unresolved** |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 82
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 64
node scripts/extract.ts ../shallfinder-corpus/corpus/1616-26__RFP1620000348.pdf --page 42
```

### 80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final

Path: `corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf`

**23 pages · 115 rows · unresolved 2 · cost $0.66**
Rows dropped because their quote could not be found in the document: **0**.

#### 10 rows drawn at random (seed 20260829)

Check: is the quote word-for-word in the document, is the page right, is the section right
or honestly blank, and does the plain-English line say what the quote says?

| # | Page | Section | Verb | Flag | Quote (verbatim, as extracted) |
|---:|---:|---|---|---|---|
| 1 | 1 | *(none)* | imperative | high | NOTE: OFFEROR TO COMPLETE BLOCKS 12, 17, 23, 24, AND 30. |
| 2 | 5 | SOW 1.1.2 | shall | high | The Contractor shall provide services globally without exception. |
| 3 | 5 | SOW 1.1.5 | shall | high | The Contractor shall have a documented Safety Management System (SMS) in accordance with International Civil Aviation Organization (ICAO), Federal, and industry standards. |
| 4 | 7 | SOW 2.2.1.d | shall | high | d. The Contractor shall provide sufficient rationale to the NASA Contracting Officer Representative (COR) why it cannot transport all accompanying travelers and provide alternate transportation for the remaining accompanying trave |
| 5 | 7 | SOW 2.2.3 | will | **review** | This independent third party will be mutually agreed upon within 30 calendar days of contract award. |
| 6 | 8 | SOW 2.3.4 | shall | high | Annually by December 15th, the Contractor shall provide a written summary report on the status of GOP advance service agreements for the NASA identified hospitals. |
| 7 | 9 | SOW 3.1.3 | will | **review** | NASA will provide call-up/execute notification to the Contractor no later than 36 hours prior to the Soyuz landing event. |
| 8 | 15 | Section I | declaration | **review** | FAR Clauses Incorporated by Reference Number Title Effective Date 52.202-1 Definitions Jun 2020 52.203-17 Contractor Employee Whistleblower Rights Nov 2023 52.204-13 System for Award Management - Maintenance Nov 2025 52.204-19 Inc |
| 9 | 18 | Section L | must | high | The Offerors’ proposal must contain the absolute best offer as the Government has the right to award without discussions. |
| 10 | 22 | Section M | shall | high | As this procurement will result in a firm-fixed-price purchase order, Offerors shall provide firm-fixed pricing that is all- inclusive (including profit and fees) for the required services on the within Section B – Supplies or Ser |

#### The 3 flagged pages with the largest shortfall

Check: is the tool's account of what it could not capture truthful on these pages?

| Page | Scan found | First pass captured | Shortfall | Swept? | What the sweeper said |
|---:|---:|---:|---:|---|---|
| 5 | 12 | 9 | 3 | yes | 3 exclusion(s): government-actor — resolved |
| 23 | 5 | 2 | 3 | yes | 1 exclusion(s): government-actor — resolved |
| 6 | 9 | 7 | 2 | yes | 0 exclusion(s): no exclusions stated — resolved |

Read each with:

```bash
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 5
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 23
node scripts/extract.ts ../shallfinder-corpus/corpus/80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final.pdf --page 6
```

## 4. The invention count, per document

A row is dropped when its quoted text cannot be located anywhere in the document. This is the
structural defence against a fabricated citation, and the count is what the defence caught.
A dropped row never reaches a user — but a rising count means the model is leaning on the net.

| Document | Rows kept | Dropped: not found | Dropped: bad shape | Dropped: page out of range |
|---|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-W` | 44 | **0** | 0 | 0 |
| `1616-26__RFP1620000348` | 346 | **1** | 0 | 0 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027` | 115 | **0** | 0 | 0 |

**Total dropped as unlocatable across the corpus: 1.**

## 5. The six exit criteria — what the numbers show

Ranked by how much of the decision rests on each. **No verdict is offered here.**

### 1. Catches everything (the recall check) — the criterion the project turns on

The owner reads two pages per readable document by hand and counts what the pipeline missed.
**This report deliberately does not choose those pages.**

What the machine can say about its own recall:

- The independent scan found **405** binding-verb occurrences across readable documents.
- After the sweeper, **19** occurrences remain neither captured nor explained.
- The sweeper recovered **1** requirements the first pass had missed. The first pass alone is measurably not enough.
- The scan sees Class-A verbs only. Imperatives, conditionals, and declarations carry no magic
  word and are invisible to it, so this measures one kind of miss, not all kinds. The hand audit
  is the only check that covers the rest.

### 2. Nothing invented

- **1** rows were dropped corpus-wide because their quote could not be found.
- Every row that reached the matrix had its quote located in the source page text.
- The check itself was proven by planting a fake row and a paraphrase, both rejected; disabling
  the check turns those tests red. See the 2026-08-29 Task 3 report.

### 3. Coverage check honest

- Unresolved across readable documents: **70 before the sweeper, 19 after.**
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

- 0 of 3 documents were declared unreadable and returned no matrix.
- They appear in section 2 with their measured page counts and text density, not as skips.

### 6. Cost measured and recorded per document

- **$2.45** for the corpus at Batch API rates.
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
| `0020153254COHEN` | 40 | 44 | +4 | 0 | 1 |
| `1616-26` | 385 | 346 | -39 | 20 | 16 |
| `80JSC026MEDEVAC5Q` | 132 | 115 | -17 | 5 | 2 |
| **All readable** | **9648** | **505** | **-9143** | **593** | **19** |

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

### Invention: rows dropped because the quote could not be found

| Document | v3 not-found | v4 not-found | v4 mangled (second reader) |
|---|---:|---:|---:|
| `0020153254COHEN` | 1 | 0 | 3 |
| `1616-26` | 6 | 1 | 46 |
| `80JSC026MEDEVAC5Q` | 2 | 0 | 8 |

Every one of these rows was withheld from the matrix. `not-found` means the pipeline's
own reader could not locate the quote; `mangled` means an independent reader could not,
which catches a quote spliced across columns that looks correct to us.

### Rows removed as same-page contained fragments

A row whose text sits wholly inside a longer row on the same page is a fragment of it,
not a second requirement. The ten longest removed fragments are quoted per document so
the rule can be checked against real text rather than trusted.

**`1616-26`** — 13 fragment(s) removed.

- p.63: "Unless the contract specifically provides otherwise, risk of loss or damage to the supplies provided under this contract shall remain with the Contractor until, and shall pass to the Government upon— (1) Delivery of the supplies to a carrier, if transportation is f.o.b. origin; or (2) Delivery of th…"
- p.64: "the Contractor shall be paid a percentage of the contract price reflecting the percentage of the work performed prior to the notice of termination, plus reasonable charges the Contractor can demonstrate to the satisfaction of the Government using its standard record keeping system, have resulted fro…"
- p.58: "Total upward adjustment for the contract period, including any option periods, shall not exceed 10% of the original contract base price, based on the original or option period contract price; downward adjustments are not limited."
- p.67: "Records relating to appeals under the disputes clause or to litigation or the settlement of claims arising under or relating to this contract shall be made available until such appeals, litigation, or claims are finally resolved."
- p.67: "If this contract is completely or partially terminated, the records relating to the work terminated shall be made available for 3 years after any resulting final termination settlement."
- p.45: "National Installation Manager – who shall meet with UNICOR management in Washington, DC unless otherwise directed elsewhere, at a minimum of quarterly."
- p.45: "National Installation Manager – who shall meet with UNICOR management in Washington, DC unless otherwise directed elsewhere, at a minimum of quarterly."
- p.91: "The Contractor shall provide adequate support to complete the applicable risk assessment and PIA document in a timely manner,"
- p.85: "Government information shall remain within the confines of authorized Government facilities at all times."
- p.33: "Interested offerors shall read the solicitation in its entirety prior to submission of any questions,"

**`80JSC026MEDEVAC5Q`** — 6 fragment(s) removed.

- p.23: "must include solicitation number, FOB destination to the Johnson Space Center, discount/payment terms, warranty duration, taxpayer identification number (TIN), identification of any special commercial terms, and be signed by an authorized company representative with a proposal validity date."
- p.18: "Describe the offeror’s approach, teaming arrangements, resources, processes and procedures to provide worldwide medical evacuation and medical travel services capabilities per the requirements of the Statement of Work (SOW)."
- p.6: "The Contractor and the traveler will handle all subsequent GOP and pay as agent reimbursements directly between themselves."
- p.23: "and shall not be marked as confidential, proprietary, competition sensitive, business information, etc."
- p.18: "Offerors must provide a proposal that responds to all items below."
- p.18: "Please only submit an electronic copy of your proposal."


### Running headers and footers stripped

A line recurring at the top or bottom of at least half the pages, digits wildcarded, is
document furniture. `#` stands for any run of digits.

**`0020153254COHEN`** — 63 lines, 1 page-break joins.

- `enterprise infrastructure and operations directorate (eiod)` — 13 lines
- `enterprise wireless communications branch (ewcb)` — 13 lines
- `cothen technology refresh` — 13 lines
- `office of information and technology (oit) #` — 12 lines
- `pr# #-#` — 12 lines

**`1616-26`** — 95 lines, 30 page-break joins.

- `order number: page # of` — 95 lines

**`80JSC026MEDEVAC5Q`** — 44 lines, 2 page-break joins.

- `page # of #` — 22 lines
- `#jsc#r#` — 22 lines

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
| `1616-26` | SOW | — | 13 | 15 |
| `80JSC026MEDEVAC5Q` | SOW | — | 12 | 17 |

| The six exit criteria, verbatim | `MASTER_PLAN.md`, Phase 1 |
| The audit rubric | `EXTRACTION_PROMPT_SPEC.md` §7 |
| The verdict slot | `DECISIONS.md` D-010 — still empty |


# CRITIC_REPORT — an independent attempt to break the Phase 1 evidence

Produced 2026-08-29 by a critic session that did not build the pipeline and did not fix
anything. Read-only against the pipeline. **No model calls; no money spent.** Every check
below is deterministic and re-runnable from `scripts/critic/`.

**What "independent" means here.** Document text comes from `pdftotext` (Poppler) and, where
that could not see the text, from `pypdf` — two libraries, neither of which is the `pdfjs`
the pipeline uses. The binding-verb scanner and the subject classifier were re-written from
`EXTRACTION_PROMPT_SPEC`, not imported from the pipeline.

**This report finds no reason to distrust the headline evidence, and four specific defects
worth the owner's attention.** It reaches no verdict on Phase 1.

---

## The short version

| Check | Result |
|---|---|
| 1. Independent locate, all 9,668 rows | **97.45% confirmed on the cited page. Zero inventions.** 6 page-citation errors, 11 order-mangled quotes |
| 2. Independent coverage recount | **6,849 vs the pipeline's 6,842 — 0.1% apart.** 3 pages disagree |
| 3. Government-actor exclusion audit | **3 false exclusions out of 305 (1.0%).** Far better than the builder's own caveat feared |
| 4. The unresolved list | 704 is reported; **only 333 specific occurrences can be enumerated.** 119 are genuine contractor requirements |
| 5. Duplicates | **Zero near-duplicates survived the merge.** But the *published rate* uses a stale denominator |
| 6. Section references | **4.4% cannot be located** near the cited page (378 of 8,564) |
| 7. Unsplittable chunks | The patch reports 3; **it did not record which pages**, so they cannot be audited |
| 8. Reconciliation | Rows, invention count and unresolved all reconcile. One structural inconsistency found |

---

## 1. Independent locate — every row, a different extractor

Matching rule, stated plainly. Text was normalised only for things that are artefacts of
PDF layout, never anything that changes a word: whitespace runs collapsed to one space; the
several dash and quote characters PDFs use interchangeably folded to one form; soft hyphens
and non-breaking spaces removed; lowercased. Punctuation was **not** stripped, tokens were
**not** reordered, and no stop-words were dropped. Three tiers were tried in order — exact
substring, then the same with hyphen-line-breaks joined, then letters-and-digits only.

| | Rows | Share |
|---|---:|---:|
| Found on the **cited page** | 9,421 | 97.45% |
| Found only on a **different page** | 6 | 0.06% |
| Not found by `pdftotext` | 241 | 2.49% |

**On the 241.** All 241 were then checked with `pypdf`, a different library again. **230 were
found** — 5 in AcroForm field values, 225 in pypdf's page text. `pdftotext` simply cannot see
form-field text, which is why they looked missing. That is a limitation of my instrument, not
a defect in the pipeline.

**Zero rows were found to be inventions.** Every row in the matrix corresponds to text that
exists in its document.

**11 rows remain that no independent tool can match** — [order-mangled-quotes.md](critic/order-mangled-quotes.md).
These are not inventions: every word is in the document. But they are spliced across table
columns, for example:

> "The SF1442 shall beTAB B: Signed and completed SF 1442 along with any amendments submitted full"

That is two columns of a table run together. The pipeline's locate-check passed it because
it was checking against its own identically-spliced text. **A proposal manager checking that
quote against the solicitation would not find that sentence.** The row is traceable in
principle and unusable in practice.

**6 page-citation errors** — [page-citation-errors.md](critic/page-citation-errors.md).
Offsets of −12, −8, −5, −4, −2 and +8 pages.

**9 rows needed fuzzy (letters-and-digits-only) matching** to be found at all — 0.09%.

## 2. Independent coverage recount

Rebuilt the per-page binding-verb count from `pdftotext` output with a scanner written from
the spec, applying the same stoplist.

| | Occurrences |
|---|---:|
| My independent count | 6,849 |
| The pipeline's count | 6,842 |
| Difference | 7 (0.1%) |

**Three pages disagree by more than the tolerance** — [coverage-disagreements.md](critic/coverage-disagreements.md).
On all three my count is higher. **I believe my count on those three pages**, but the
difference is immaterial: seven occurrences in 6,842 cannot move any conclusion. The
coverage denominator is sound.

## 3. The exclusion audit — the soft spot, and it is firmer than advertised

The builder's own caveat says false government-actor exclusions flatter coverage and that
the sweeper cannot catch them. I dumped all 305 exclusions my scan produced (the pipeline
reports 308) and classified each by its **immediate** grammatical subject, using a tighter
window than the pipeline's and stepping back over conjunctions.

**3 of 305 are wrong — 1.0%** — [false-exclusions.md](critic/false-exclusions.md).

Their effect on the denominator is 3 occurrences in 6,842: **0.04%.** The per-occurrence
attribution fix works. This was the most likely place for the evidence to be quietly wrong,
and it is not.

## 4. The unresolved list — for the owner's Part B audit

[unresolved-occurrences.md](critic/unresolved-occurrences.md) — every occurrence, grouped by
document, pages sorted by shortfall, largest first.

**A discrepancy the owner should know about.** The pipeline reports **704** unresolved.
Working from the pages it lists as uncaptured, I can enumerate **333** specific occurrences
that are genuinely unaccounted for. The gap is because **704 is an arithmetic residual**
— shortfall minus a resolution credit — **not a list of things**. The number cannot be
walked item by item, which is a problem for a figure the product intends to show users as
"pages to re-read".

Of the 333 that can be named:

| Pattern | Count |
|---|---:|
| Other | 178 |
| **Genuine contractor requirement** | **119** |
| Form fragment | 30 |
| Government-actor missed by the stoplist | 6 |
| Definitional missed by the stoplist | 0 |

**119 real requirements sit unresolved and named.** That is the honest recall gap, and it is
the most useful list in this report for Part B.

## 5. Duplicates

Recomputed independently with the §5 rule — trigram similarity ≥ 0.9 and pages within 1.

**Zero near-duplicates survived the merge**, across all 9,668 rows. The merge rule works.

**But the published duplicate rate is computed against the wrong denominator.** In 11 of 14
documents `finalRows` **exceeds** `rawRowCount` — 9,668 final against 6,546 raw — because
sweeper rows and patch rows enter the matrix without ever entering the raw count. The rates
in EVAL_REPORT §1 (`duplicatesRemoved / rawRowCount`) therefore describe the first pass
only, not the published matrix. For `47QMCA26Q0098` the raw count is 0 and the rate prints
as "0.0%", which is meaningless.

## 6. Section references

**The strict test is the wrong test, and I report it only to dismiss it.** Requiring the whole
composed label ("PWS 4.1") to appear verbatim fails on 58.1% of rows — but
`EXTRACTION_PROMPT_SPEC` §2 *asks* for a composed label, so that test measures nothing.

The fair test is whether the identifier inside the label appears within three pages either
side of the citation:

| | Rows | Share |
|---|---:|---:|
| Rows carrying a section reference | 8,564 | |
| **Identifier not found near the cited page** | **378** | **4.4%** |
| References with no extractable identifier (e.g. "Section L") | 382 | |

[section-reference-misses.md](critic/section-reference-misses.md). Note this tests
*presence*, not *correctness* — a reference can be present nearby and still be the wrong
section. Confirming correctness needs a human, and is what the owner's 10-row samples are for.

## 7. The three unsplittable chunks

**They cannot be audited, because the patch did not record which pages they were.**
`_patch.json` states `unsplittable: 3` and nothing more. Those chunks were halves created
during splitting, so they do not reappear in a fresh chunking pass and cannot be recovered
from the record.

What I *can* say: re-chunking the corpus today produces exactly **one** chunk that cannot be
split — `80JSC026MEDEVAC5Q` page 23, 131 words. On that page the scan finds 5 class-A
occurrences and the matrix carries 5 class-A rows and 6 rows total. **Shortfall zero: that
page is not incomplete.**

For the other two, the honest answer is that the evidence does not exist. **Needs the patch
to record page numbers** — a one-line change, not something a critic should make.

## 8. Reconciling the report

| Claim in EVAL_REPORT §1 | Report | JSON | |
|---|---:|---:|---|
| Total rows | 9,668 | 9,668 | reconciles |
| Dropped as unlocatable | 236 | 236 | reconciles |
| Unresolved | 704 | 704 | reconciles |
| Rows added by the patch | 1,624 | 1,624 | reconciles |

Per-document row counts, invention counts and unresolved counts all reconcile against the
JSON. `finalRows` equals `rows.length` in every document.

**One structural inconsistency**, described in §5: `finalRows > rawRowCount` in 11 of 14
documents. Nothing is misreported, but two fields that look like they belong to the same
population do not.

## 9. A defect found by reading the code, not the data

`coverageConfig.governmentActorPatterns` is declared in `lib/extraction/config.ts`, carries a
documentation comment explaining its purpose, and is **read nowhere**. The classifier uses
word lists hardcoded inside `lastSubject()` in `coverage.ts`.

Setting `COVERAGE_GOVERNMENT_PATTERNS` has no effect whatsoever. Anyone tuning the
government-actor stoplist would change the config, observe no change in behaviour, and have
no indication why. `CLAUDE.md` forbids hard-coding values that belong in configuration.

## 10. What this critic could not check

- **Whether captured rows are the *right* rows.** Recall against a careful human reader needs
  a human reader. That is criterion 1 and it is the owner's two-page hand read.
- **Whether a section reference is the correct section**, as opposed to a section that exists
  nearby.
- **Whether `normalized` faithfully restates its `verbatim`.** Comparing meaning **needs a
  model run** and none was made.
- **The two unsplittable chunks** whose page numbers were never recorded.

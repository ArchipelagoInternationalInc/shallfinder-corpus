# 2026-09-07 — Session A: the six mechanical fixes from the outside review

**No model was invoked and nothing was spent.** Every number below was measured by running
deterministic code over the corpus documents; the only model artefact touched is the prompt
text, and it was changed once, as recorded in section 4.

**This is the one documented iteration that Task 5 allows.** It changed the extraction prompt
in exactly two places (Class D triggers; wage determinations as a schedule). No further
prompt iteration without an owner ruling.

The protected files were not touched: `corpus/eval/`, `corpus/eval-v3/`, `EVAL_REPORT.md`,
`EVAL_REPORT_v3.md`, `DECISIONS.md`, `MASTER_PLAN.md`. All outputs went to the `shallfinder`
code repository and to this report.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level (both repositories) | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Last commit author (both repositories) | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remotes | `github.com/ArchipelagoInternationalInc/shallfinder`, `…/shallfinder-corpus` |

Both working trees were clean before the session began.

## 2. What was found, defect by defect

### Defect 1 — reading order, and a line-item code on 92 wrong pages

**Cause of the leak, confirmed.** The Task 2 form-field recovery was the suspect and it was the
cause, but not in the way expected. `1616-26` is an SAP-generated RFQ whose body text lives in
form widgets. pdfjs fills a widget's *stored value* from the PDF's XFA data packet when the
widget has no value of its own, and on this document those packet values do not line up with
the widgets: on page 75, 9 of 40 widgets received the page-2 line-item code
`STAIR-CARRY-1-FLIGHTS-E` as their value, while their *drawn* appearance is FAR clause text.
Two independent readers agreed with the rendered page: `pypdf` walking the real value chain
found 0 code-carrying widgets on page 75 (1 on page 2), and `pdftotext` found the code on
page 2 only. `hasAppearance`, the first hypothesis, does not discriminate: every widget on
these pages has an appearance stream; the phantom ones simply draw nothing.

**Fix.** Form text is now taken from what pdfjs reads out of the appearance stream (what a
reader sees). A widget with an XFA-style name (`data[0].Page2[0]….TDLINE[0]`) and no drawn
text contributes nothing. A plain AcroForm field with no appearance stream — the hand-filled
SF 1449 / SF 33 / SF 18 case Task 2 fixed — still contributes its stored value, and the
Task 2 fixture still passes.

**Proof — the code, before and after:**

| | Pages carrying `STAIR-CARRY-1-FLIGHTS-E` | After page 2 |
|---|---:|---:|
| Before | 93 | 92 |
| After | 1 (page 2) | **0** |

Page 75 before: `"Page 75 ofORDER NUMBER:\nRequest for Quotation\n1620000348\nSTAIR-CARRY-1-FLIGHTS-E\nRoaming means cellular communications…"`
Page 75 after: `"Roaming means cellular communications services (e.g., voice, video,\ndata) received from a visited network…"`
Pages 66 and 101 likewise: the code and the running header are gone, the clause text is
unchanged. Document total: 186,451 characters before, 161,738 after (the phantoms and the
per-page header were the difference).

**Reading order, confirmed by geometry.** On `W911SG27BA002` page 16, at the same baseline,
the content stream emits the item at x=440 (`". The SF1442 shall be"`) *before* the item at
x=55 (`"TAB B: Signed and completed SF 1442 along with any amendments"`). Page 14 and page
41 are the same shape. Text is now assembled from glyph positions: lines grouped by baseline,
ordered top to bottom, pieces left to right.

| Page | Before | After |
|---|---|---|
| 14 | `5. : ELECTRONIC SUBMISSION OF BIDS (TABS A-E), TO INCLUDE E-SUBMISSION OF BIDS` | `5. SUBMISSION OF BIDS: ELECTRONIC SUBMISSION OF BIDS (TABS A-E), TO INCLUDE E-` |
| 16 | `b. . The SF1442 shall beTAB B: Signed and completed SF 1442 along with any amendments` | `b. TAB B: Signed and completed SF 1442 along with any amendments. The SF1442 shall be` |

**Corpus-wide referee, so this is not a two-page fix.** For every page, the share of
`pdftotext`'s word trigrams that appear in the pipeline's page text (reading order only, with
header stripping switched off for the comparison because `pdftotext` keeps headers):

| | Before | After |
|---|---:|---:|
| All 1,920 judged pages, mean | 0.937 | **0.984** |
| Pages better by more than 0.05 | | 364 |
| Pages worse by more than 0.05 | | 6 |

The six "worse" pages were read: four are tables of contents where a dot leader is now joined
to its page number (`....4` instead of `.... 4`), one is a form page, one is a page whose
footer block moved from the top of the text to the bottom with no words lost (0 words of 159
missing). `1616-26` moved from 0.798 to 0.960; `W15P7T-26-R-A006` from 0.944 to 1.000.
Rotated pages fall back to stream order.

### Defect 2 — the duplicate rule was deleting real requirements

**Confirmed, and the cause is narrower than the rule.** On `W15P7T-26-R-A006`, pages 280–284
were covered by chunk 32 only and pages 287–290 by chunk 33 only: **there was no overlap
region there.** The merges that emptied page 281 and page 289 were removing genuine repeats
the document makes page after page, not overlap artefacts.

**Fix.** Two rules, different in kind. Cross-page merging is allowed only when *both* pages
were covered by two chunks (the pipeline now passes that set to the merge). On the same page,
a row wholly contained in a longer row (after whitespace and case normalization, and at least
20 characters long) is dropped as a fragment; near-identical same-page rows still merge.
Callers that cannot know the overlap region get no cross-page merging at all — the direction
that keeps rows.

**Proof on the real sentences, through the real function, no re-extraction.**
`"Offerors shall adhere to all submission requirements per RFP Section L.1."` is present in
the page text of pages 281, 282 and 284; the recorded v3 rows quote it on 282 and 284 and not
on 281 — the page-281 copy was merged away. Fed to the new rule as a page-281 row plus the
recorded page-282 row: **2 rows kept** (the old rule kept 1); with both pages marked as an
overlap region: 1 row kept. Both directions work.

`75N98026Q00962`: the recorded 159 v3 rows contain **7** same-page contained fragments by the
rule as written (the review estimated "at least fourteen"; the seven are what whole-containment
finds — e.g. page 21's `"The Contractor shall not tender for acceptance materials and services
required to be replaced\nor corrected…"` inside the full FAR 52.246-4 sentence). Across the
whole recorded corpus the rule would remove **304 of 9,648** rows as contained fragments.
The recorded row counts on pages 281/282/284/288/289 (10/6/3/11/0) cannot change without
re-running the model, which this session was told not to do.

### Defects 3 and 4 — one root cause: page break plus running header

`80JSC026MEDEVAC5Q` page 18 ended `"…with medical support for NASA"` and page 19 began
`"PAGE 19 OF 23\n80JSC027R0003\ntravelers with minor medical emergencies…"`.
`0020153254COHEN` page 11 ended `"…a trade study with no"` and page 12 began with a five-line
running header, then `"less than four alternatives…"`. The regex `/no\s+less\s+than\s+four/`
matched neither page before, so the scanner counted 0 and the extractor saw a phrase that
did not exist on any page.

**Fix.** At ingest, a line that recurs at the top or bottom of at least half the pages (digits
wildcarded, 120-character cap so body text is never furniture) is stripped. Then, when a page
ends mid-sentence and the next page opens with a lowercase continuation that is not a list
marker, the opening fragment up to its first sentence end (600-character cap) is moved onto
the page where the sentence started. Attribution follows the start of the sentence.

**Proof.**

| | Before | After |
|---|---|---|
| NASA p.18 ends | `…with medical support for NASA` | `…with medical support for NASA travelers with minor medical emergencies and medical scenarios that require commercial air business class transportation return to the US.` |
| NASA p.19 begins | `PAGE 19 OF 23\n80JSC027R0003\ntravelers…` | `The Offeror shall explain its capabilities…` |
| COHEN p.11 ends | `…a trade study with no` | `…a trade study with no less than four alternatives, one of which reflecting the status quo and another reflecting multi- agency collaboration).` |
| regex on COHEN p.11 | no match | **match** |
| scanner "no less than" on COHEN p.11 | 0 | **1** |

Corpus-wide: 3,007 running-header lines stripped and 382 page-break joins across the 14
readable documents (`W912P726RA022` alone: 1,224 lines, 80 joins; `70CDCR26R00000026`: 0 lines
— it has no running header — and 62 joins).

**Class D triggers.** `"requirements include"` and `"verification requirements include"` are
now binding-verb patterns in the scanner configuration and named in the prompt's Class D
definition. A test proves the scanner counts a `"Verification requirements include: …"`
sentence, and that the prompt text carries both phrases.

### Defect 5 — unresolved bookkeeping

**Two verbs, one sentence.** The coverage object now carries `unresolved_items`, one entry per
(page, sentence) with `count` and the distinct `verbs`. Measured with the recorded v3 rows
against the new page text: **560 unresolved occurrences become 534 listed sentences; 26
double listings avoided** (the review counted 35 against the old page text — the header
stripping and joins changed which sentences are unresolved, so the two numbers are not
like-for-like; both are measured). The occurrence count is unchanged as a number; only the
listing is grouped.

**The wage determination is a schedule.** A block is detected from its `General Decision
Number` line to `END OF GENERAL DECISION`; the patterns that mark a footnote inside it are
configuration (`COVERAGE_SCHEDULE_PATTERNS`, eleven phrases such as `shall receive`, `per hour
above`, `shall not be construed`). Inside `W912P726RA022`'s block, pages 87–147: **191
occurrences, 160 excluded as `schedule`, 8 still counted** — and the 8 are right to keep, among
them `"If this contract is covered by the EO, the contractor must provide employees with 1 hour
of paid sick leave…"`, which is a real obligation that happens to sit inside the schedule.
`W911SG27BA002` carries three back-to-back determinations (pages 19–25, 25–31, 31–36); all
three are found; none of their sentences match the footnote patterns, and none is excluded.

The pipeline now adds **one admin row per determination**, quoting the General Decision
Number line verbatim from its first page (so it passes the same locate-check as every other
row); the prompt tells the model the same thing so it does not explode footnotes into rows.
Schedule exclusions are counted and reported in `coverage.excluded.schedule`, never silent.

### Defect 6 — garbled text

**Signal, measured across the corpus:** the share of a page's non-space characters that are
not ordinary document characters (printable ASCII plus the usual typographic marks).

| Document | Pages over 0.20 | Worst page |
|---|---:|---:|
| `47QMCA26Q0098` | 7 of 8 | 0.93 |
| `W912P825BA029` | 10 of 225 | 0.96 (one genuinely garbled page and a run of near-empty pages carrying control characters) |
| every other document | 0 | ≤ 0.02 |

Thresholds: page garbled over 0.20 (pages under 50 characters not judged); document
unreadable when more than half of judged pages are garbled. Any page threshold in 0.10–0.50
and any document threshold in 0.05–0.85 classify this corpus identically; the two chosen sit
inside those bands. **`47QMCA26Q0098` is now `unreadable`, reason `garbled-text`**;
`W912P825BA029` stays readable (10 of 246 pages flagged); the three scanned documents are
still `image-only`, not garbled. A new fixture, `tests/fixtures/garbled-text.pdf`, is a real
PDF with a broken font encoding (six garbled pages behind one clean cover page), and the
pipeline declares it `garbled-text`.

## 3. Every guard proven able to fail

`scripts/prove-guards.mjs` plants each defect back into the source, runs the suite, and
requires the named test to go red before restoring the file. **27 of 27 proven**, 12 of them
new this session:

D1 reading order · D1 form values not drawn · D2 no cross-page merge outside overlap ·
D2 same-page fragment containment · D3 running header strip · D3 page-break join ·
D4 Class D triggers · D5 schedule exclusion · D5 unresolved grouped by sentence ·
D5 one admin row per wage determination · D6 garbled text · and the existing merge guard,
re-anchored to the overlap-region rule.

Test suite: **71 tests, 71 passing** (50 before; 21 new in `tests/review-fixes.test.ts`).
`npm run lint`: 0 errors (13 pre-existing warnings in older scripts, none in files touched
this session). `npm run typecheck`: clean.

## 4. The prompt change — the one documented iteration

Two edits to `lib/extraction/prompt.ts`, nothing else:

1. Class D now reads: *"…and lists introduced by 'requirements include' or 'verification
   requirements include' — capture each listed item as its own row."*
2. A new exclusion under "do not extract": a Department of Labor wage determination is a
   rate schedule; capture it once as a single admin row quoting its General Decision Number
   line and produce no rows for its footnotes.

Recorded here as Task 5's single allowed iteration. The recorded evaluations (v2, v3) were
produced by the previous prompt and are unchanged.

## 5. Limits stated plainly

- An XFA-bound form whose fields have real values but **no appearance streams at all**
  would now lose those values. None of the 17 corpus documents is one; if one arrives it
  shows as low text, which the existing checks catch — not as nonsense.
- The reading-order sort assumes single-column layout. True two-column pages would
  interleave; none of the named pages is one, and the referee found no such page.
- `EXTRACTION_PROMPT_SPEC.md` section 5 still describes the old duplicate rule ("pages
  within 1"). The code follows the corrected rule and says why in `merge.ts`. Amending the
  spec is flagged for the owner, not done.
- None of this changes any recorded number. Every recorded-corpus figure above (304
  fragments, 26 double listings, 160 schedule exclusions) is what the new code would do to
  the old rows; the actual effect on the matrix is only known after a re-run.

## 6. Cost

**$0.00.** No API call was made. Cumulative spend across all sessions remains $95.95.

## 7. Files

`shallfinder`: `lib/extraction/ingest.ts` (rewritten), `merge.ts`, `coverage.ts`, `extract.ts`,
`config.ts`, `prompt.ts`, `types.ts`; `tests/review-fixes.test.ts` (new), `tests/locate.test.ts`
(one test given an overlap set), `tests/fixtures/garbled-text.pdf` (new) and its generator;
`scripts/prove-guards.mjs` (+12 guards); `scripts/review-proofs.mjs` (new — reproduces every
number in section 2 from the corpus, read-only); `SESSION_HANDOFF.md`.
`shallfinder-corpus`: this report only.

## 8. Next

Session A stops here, as instructed. **Session B is ready to paste.**

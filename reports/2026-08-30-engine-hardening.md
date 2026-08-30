# 2026-08-30 — Engine hardening

Builder session, engine only. **No model was invoked and no tokens were billed.** Every
guard below is proven with a planted failure, not a live run. Phase 1 remains ungated; no
UI, database, auth, billing or public copy was touched, and `corpus/eval*`,
`EVAL_REPORT*.md`, `DECISIONS.md` and `MASTER_PLAN.md` are unchanged.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

One distinct author across both repositories.

## 2. Zero spend

**No model call of any kind.** Every model-facing test injects a `ModelCaller` that returns
a planted response. The suite runs with `EXTRACTION_MODEL="test-model-that-must-never-be-called"`
— deliberately not a real model id, so if a test ever did reach the API it would fail loudly
instead of quietly costing money. No batch was submitted and no API endpoint was called.

## 3. Refusals are now a first-class outcome

A chunk ending with `stop_reason: refusal` records **which pages it covered** and stops.
Splitting cannot help a refusal and neither can retrying; treating them as truncations is
exactly how nine refusals came to be reported as truncations in the 2026-08-29 evaluation.
Unparseable-after-retry gets its own reason. Both appear in `ExtractionRun.notProcessed`
and in the coverage object as **`not_processed`**.

`not_processed` is kept **deliberately separate from `unresolved`**, and the coverage
arithmetic is untouched. An unresolved occurrence is one the pipeline looked at and could
not account for. A not-processed page was never looked at. Conflating them is how a thin
result gets presented as a complete one.

The CLI prints them under a heading that says so:

```
PAGES NOT PROCESSED: 6
  These pages have NO extraction. This result is not complete and must
  not be presented as though it were.
    refusal: pages 1-6 - the model declined to process pages 1-6
```

**Proven** by planting a caller that refuses everything and asserting the pages appear with
the reason, in both the run and the coverage object.

## 4. Bad input, honestly declined — each case before and after

Seven fixtures under `tests/fixtures/`, generated deterministically by
`scripts/make-fixtures.py`.

| Planted case | Before | After |
|---|---|---|
| Password-protected PDF | **passed already** | `failed / encrypted`, message names the password |
| Word file renamed `.pdf` | **passed already** | `failed / not-a-pdf` |
| Zero-page PDF | **passed already** | `failed / empty` or `corrupt` |
| Over the page cap | **FAILED** — no cap existed | `failed / too-many-pages`, message names the cap |
| Over the size cap | **FAILED** — no cap existed | `failed / too-large`, checked before parsing |
| Text entirely in form fields | **passed already** | `ok`, text recovered |
| No extractable text at all | **FAILED** — no test existed | `unreadable / image-only` |
| Caps are configuration | **FAILED** — literals absent | `MAX_PAGE_COUNT`, `MAX_UPLOAD_MB` |

Four guards already existed and are now covered by tests; four are new. Size is checked
**before** parsing — refusing early costs nothing, and parsing a file already rejected is
work done for a result we throw away.

## 5. The second-source check is in the pipeline path

Every row must satisfy a reader that is not the pipeline's (`pdftotext`, falling back to
`pypdf`), or it is dropped as **`mangled`** — a counted drop reason alongside
`verbatim-not-found`, `page-out-of-range` and `invalid-shape`. It is skipped entirely when
no independent reader is available, because a check that cannot run must never drop rows.

Tested with one of the 20 known mangled quotes:

> `"The SF1442 shall beTAB B: Signed and completed SF 1442 along with any amendments submitted full"`

**A fixture bug worth recording.** My first version of that test put the mangled quote only
in the independent source, so the row was dropped by the pipeline's *own* locate-check and
the test passed for the wrong reason. The fixture now puts the spliced sentence into the
pipeline's own page text — which is what makes this defect invisible to the first check, and
is exactly what happened on the real document.

## 6. Guards, and the test that proves each can fail

`scripts/prove-guards.mjs` plants a defect in each guard, runs the suite, confirms the
**named** test goes red, and restores the tree — even on error. **16 of 16 proven.**

| Guard | Planted defect | Test that goes red |
|---|---|---|
| Verbatim-locate (invention) | accept unlocatable rows | *REJECTS A PLANTED FAKE ROW* |
| Second-source locate (mangling) | always return `confirmed` | *drops a known mangled quote* |
| Truncation split | never split | *halves a multi-page chunk at a page boundary* |
| Split floor (termination) | remove the floor | *refuses to split below the configured floor* |
| Page anchor format | emit `[p.N]` | *marker format EXTRACTION_PROMPT_SPEC* |
| Not-a-PDF magic bytes | skip the check | *a Word file renamed .pdf* |
| Encrypted PDF | skip the check | *password-protected PDF* |
| Page cap | skip the check | *over the page cap is declined* |
| Size cap | skip the check | *over the size cap is declined* |
| Unreadable (scanned) detection | skip the check | *returns unreadable, not a thin matrix* |
| Refusal recorded, not a truncation | skip the branch | *records the affected pages with reason refusal* |
| Empty-chunk (unparseable) detection | skip the record | *records unparseable-after-retry* |
| Row shape validation | drop the category check | *rejects a row with a bad category* |
| Merge/dedupe rule | never match a duplicate | *collapses near-identical rows* |
| Config empty-list guard | skip the throw | *THROWS on an empty list* |
| Design tokens (small radii) | `lg: 24px` | *uses small radii* |

Two were `NOT PROVEN` on the first pass, and **both were faults in my harness, not the
guards.** One had no test for the scanned case at all; the other still carried a placeholder
string because my edit targeted text that spans two lines and silently matched nothing. The
guard itself failed correctly when sabotaged by hand. A proof harness needs its own
scepticism.

After every run the tree is checked for residue: no `if (false` in `lib/` or `tokens/`, and
the token file intact.

## 7. All five commands

```
npm run lint       ->  EXIT 0   (14 warnings, 0 errors)
npm run typecheck  ->  EXIT 0
npm run test       ->  EXIT 0   Test Files 4 passed (4) · Tests 50 passed (50)
npm run build      ->  EXIT 0   Next.js 16.3.3, 4/4 static pages
npm run dev        ->  Ready in 164ms; fetched / -> HTTP 200, 36,104 bytes
```

Tests went 30 → 50.

## 8. What surprised me

- **Two of my three failures this session were in the test harness, not the code.** The
  mangled-quote fixture passed for the wrong reason, and the guard-proof harness reported
  two false negatives. Instruments need auditing as much as the thing they measure — the
  same lesson the critic pass produced, arriving from the opposite direction.
- **The model-id guard blocked the tests, correctly.** `modelConfig.model` refuses to
  resolve when `EXTRACTION_MODEL` is unset, which is right, and which meant the offline
  suite could not run until I gave it an obviously-fake id. Setting a real one would have
  been the easy path and the wrong one.
- **Making the model call injectable was what made refusals testable at all.** The defect
  that produced nine mislabelled refusals could not have been caught by any test written
  against the old shape, because there was no way to make the model refuse on demand.
- **Four of the six bad-input cases already worked.** The gap was that nothing proved it.

## 9. What I did not do

- Did not change the model, prompt, chunk sizes, thresholds, or the coverage arithmetic.
- Did not re-run extraction, or touch `corpus/eval*`, `EVAL_REPORT*.md`, `DECISIONS.md`,
  `MASTER_PLAN.md`, UI, auth, billing or public copy.

**Nothing here needed a model run.** Phase 1's verdict remains the owner's; D-010 is empty.

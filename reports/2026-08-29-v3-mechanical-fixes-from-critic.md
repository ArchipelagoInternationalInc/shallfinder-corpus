# 2026-08-29 — v3: mechanical fixes from the critic pass

Builder session. Deterministic throughout. **No model was invoked and no tokens were
billed.** No extraction was re-run; model, prompt, chunk sizes and thresholds are
unchanged. `corpus/eval/` and `corpus/EVAL_REPORT.md` were not touched — the owner is
mid-audit on them.

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

## 2. Spend

**Zero.** No model call of any kind. Two zero-token API reads were made — retrieving stored
**batch results** to recover the unsplittable-chunk information, which the task explicitly
permitted ("from the batch results or logs"). Retrieving a stored result invokes no model
and bills no tokens. Everything else ran locally.

## 3. Mangled rows — 20, and the matching rule

**The rule.** Whitespace runs collapse to one space; the several dash and quote characters
PDFs use interchangeably fold to one form; soft hyphens and non-breaking spaces are
removed; lowercased. Two tiers: exact substring, then the same with hyphen-line-breaks
joined. The quote is searched across the **whole document**, not just the cited page — page
accuracy is a different check's job, and since this check *drops* rows it must fire only
when no independent reader can see the text at all. Sources: `pdftotext` (Poppler), falling
back to `pypdf` for the form-field text `pdftotext` cannot see.

**There is deliberately no alphanumeric-only tier, and that is the whole point.** Stripping
punctuation and spaces makes `"...shall beTAB B: Signed..."` compare equal to the same words
correctly spaced — it erases the exact splice that defines mangling.

I know this because **my first version included that tier and dropped zero rows.** Measured:
all 11 rows the critic identified pass an alphanumeric comparison and fail the two tiers
kept. The check would have been unable to detect the only thing it exists for.

**20 rows dropped**, reconciling exactly with the critic: its 11 order-mangled rows, plus
the 9 it could only match with the splice erased — the same defect, e.g.
`"The offeror shall enter the name and____________ unique entity identifier"`.

| Document | Dropped |
|---|---:|
| `W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001` | 17 |
| `W912P726RA022__W912P726RA002-San-Rafael-Solicitation` | 3 |
| **Total** | **20** |

Rows: 9,668 → 9,648.

## 4. The six page-citation errors

**Four of the six were not errors.** The text is a wage-determination clause that appears
**identically on 15–17 pages** of the same document. The critic reported the first page
carrying it, which is not evidence the cited page is wrong. Those rows keep their cited page
and are flagged `confidence: review`, because which occurrence a row refers to genuinely
cannot be determined from the text.

| Cited page | Critic said | Outcome |
|---:|---:|---|
| 115 | 113 | Ambiguous — identical text on 17 pages. Kept, flagged `review` |
| 117 | 113 | Ambiguous — identical text on 17 pages. Kept, flagged `review` |
| 119 | 114 | Ambiguous — identical text on 15 pages. Kept, flagged `review` |
| 125 | 113 | Ambiguous — identical text on 17 pages. Kept, flagged `review` |
| 287 | 295 | **Corrected to 295** — unique match |
| 342 | 334 | **Not resolvable.** Matches no page under exact comparison; found only with the splice erased. Left unchanged rather than moved on weak evidence |

One correction, four reclassified as ambiguous, one honestly unresolved.

## 5. Unresolved — 704 became 593, and why

**These are not the same measurement, and the drop is not an improvement.**

v2's **704** was an arithmetic residual: page shortfall minus a resolution credit, summed.
It could not be walked item by item — the critic pass demonstrated exactly that.

v3's **593** is a count of **named occurrences**. Each carries its page, the sentence it
sits in, and why it is unresolved. Every entry can be looked up and argued with. They span
338 pages across the readable documents, and live in each document's
`coverage.unresolved_items`.

The critic enumerated 333 using only the pages v2 listed as uncaptured; v3 enumerates every
page carrying a shortfall, which is a superset. **No number was adjusted to look better** —
593 is simply what the enumeration produces.

## 6. The dead config, and the proof

`coverageConfig.governmentActorPatterns` was declared, carried a documentation comment, and
was **read nowhere**; the classifier used hardcoded lists. It is now
`governmentActors` / `offerorActors`, which `lastSubject()` actually reads.

| Test | Result |
|---|---|
| **Default unchanged** | 32 government-actor exclusions, 688 countable — **exactly** the recorded corpus values |
| **Change the value** (`COVERAGE_GOVERNMENT_ACTORS="the government"`) | 16 exclusions, 704 countable — behaviour changes |
| **Plant a bad value** (`" \| "`) | Refuses to run, exit 1: *"an empty actor list would silently disable the coverage classifier rather than change it"* |

**A regression I introduced and caught.** My first parser trimmed each entry, turning
`"the co "` into `"the co"` — which matches inside *"the **co**ntractor"*. Exclusions moved
32 → 57 and countable 688 → 663. Caught only by comparing against the recorded run before
trusting the refactor. Entries are no longer trimmed, and the reason is in the code.

## 7. The unsplittable chunks — and what they actually were

**The pages cannot be reliably recovered, and I will not guess them.** The patch assigned
`custom_id`s by position in a list built from batch-results stream order. I reconstructed
that twice under two defensible orderings and got **two completely different answers** —
4 chunks in three documents, versus 2 chunks in a different one. Neither is trustworthy.

**But something order-independent came out of it, and it matters more.** Counting
`stop_reason` across every batch:

| Batch | Stop reasons |
|---|---|
| Extraction (v1 run) | end_turn 215, max_tokens 46, **refusal 2** |
| Sweeper (v1 run) | end_turn 565 |
| Patch round 1 | end_turn 90, max_tokens 4, **refusal 2** |
| Patch round 2 | end_turn 8, max_tokens 1, **refusal 3** |
| Patch round 3 | end_turn 4, **refusal 2** |
| Patch re-sweep | end_turn 2 |
| **Total** | end_turn 884, max_tokens 51, **refusal 9** |

These reconcile exactly with the logged request counts (263, 96, 12, 6).

**Nine requests were refused by the model, and the evidence calls them truncations.** The
code treated any unparseable response as a truncation candidate, so refusals were counted
with truncations and the "3 chunks truncated and could not be split further" line named the
wrong cause. A refusal and a truncation need different answers.

Fixed so it cannot recur: the batch driver records `stopReasons` per batch, and the patch
now logs what every `custom_id` was — document, page span, word count — plus, for any chunk
that ends without usable rows, **which pages and why** (`refusal`, `truncated-unsplittable`,
or `unparseable`). An id whose meaning lives only in memory is not a log.

## 8. The owner's audit sheet

**1 of the 140 sampled rows changed.** Same seed (20260829), so the samples are the ones
already on the sheet.

| Document | Sample # | Change | Was page |
|---|---:|---|---:|
| `W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001` | 2 | **DROPPED (mangled)** | 49 |

Everything else on the audit sheet stands exactly as it was.

## 9. What surprised me

- **Both of my first attempts were wrong in the flattering direction.** The mangled check
  dropped zero rows; the config refactor silently widened a stoplist. Each looked like
  success. Both were caught only by checking against a previously recorded number rather
  than against my expectation.
- **Four of the six "page-citation errors" were not errors.** The critic's method reported
  the first page carrying a repeated clause, which reads like a wrong citation and is not
  one. A finding that survived a critic pass still needed checking.
- **The truncation story was partly wrong.** Nine responses were refusals, not truncations.
  The most-repeated defect narrative of the last several sessions was, in part, mislabelled
  — and nobody could see it because refusal and truncation shared a counter.
- **Fixing dead config found a live bug.** Wiring `governmentActorPatterns` was meant to be
  tidy-up; it surfaced that a trailing space in `"the co "` is load-bearing.

## 10. What I did not do

- Did not re-run extraction, change the model, prompt, chunk sizes or thresholds.
- Did not touch `corpus/eval/`, `corpus/EVAL_REPORT.md`, `DECISIONS.md`, `MASTER_PLAN.md`,
  UI, auth, billing or public copy.
- Did not resolve the page-342 citation, or recover the unsplittable pages. Both are
  reported as unresolved rather than guessed.

**Nothing here needs a model run.** The one thing that would is confirming whether a
`normalized` line faithfully restates its `verbatim`, which was out of scope.

The Phase 1 verdict remains the owner's. D-010 is still empty.

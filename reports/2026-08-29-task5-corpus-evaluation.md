# 2026-08-29 — Task 5: the Phase 1 corpus evaluation

The corpus ran. `corpus/EVAL_REPORT.md` and per-document JSON are filed. **No verdict is
offered — that is the owner's, and D-010 is still empty.**

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

`EXTRACTION_MODEL` read from Doppler at run time: **`claude-sonnet-5`**. Confirmed by
running the real config loader under `doppler run` — `process.env` and `modelConfig.model`
agree, and the loader throws if the variable is absent. Nothing in code selects a model.

## 2. Cost — projection against actual

Restated before submitting, from the measured per-page figure:

| | |
|---|---:|
| Measured basis (2 documents, 261 pages, standard rates) | $0.03851 / readable page |
| Corpus readable pages | 2,032 |
| At standard rates | $78.25 |
| **Projected at Batch API rates (50%)** | **$39.13** |
| Owner threshold | $50.00 |
| **Actual** | **$43.44** |

**11% over projection, still under the threshold.** The overshoot is explained and was not
a pricing surprise: the sweeper visited **565 flagged pages** corpus-wide (27.8% of pages)
against the 23.8% rate in the two-document sample. More pages needed sweeping than the
sample implied. Rates verified 2026-08-29 at `platform.claude.com/docs/en/about-claude/pricing`.

## 3. Batch jobs

| Phase | Batch ID | Requests | Result |
|---|---|---:|---|
| Extraction | `msgbatch_01GmybQHLxUGtzQ7QJygYsbN` | 263 | 263 succeeded, 0 errored |
| Sweeper 1 | `msgbatch_01TLQ4Njoc2McjmSxTdN91pj` | 500 | succeeded |
| Sweeper 2 | `msgbatch_01F5t47hdLdbweX9c9pttAtc` | 65 | succeeded |

**828 requests, 565 sweeper results, 0 errored, 0 canceled, 0 expired.** Sweeper batches
took 27.4 minutes wall clock.

One submission was rejected before it ran: `custom_id` must match `^[a-zA-Z0-9_-]{1,64}$`
and mine carried filenames. The API refused the whole batch at submission, so **nothing was
billed**. Ids now carry indices with a side map.

## 4. Summary

| | |
|---|---:|
| Documents | 17 — **14 readable, 3 unreadable, 0 failed** |
| Readable pages | 2,032 |
| **Rows** | **8,044** |
| Rows the **sweeper** recovered that the first pass missed | **2,882 (35.8% of the final matrix)** |
| Rows dropped as unlocatable (**the invention count**) | **164** |
| Rows dropped for bad shape | 31 |
| Binding-verb occurrences (deterministic scan) | 6,842 |
| **Unresolved: before → after sweeper** | **3,081 → 353** (88.5% reduction) |
| Total cost | $43.44 |

The three scanned documents appear in the results as honestly unreadable with their
measured page counts and text density — **not skipped**, and billed $0.00.

Per-document rows: 5 to 1,712. Duplicate rate 0.0%–19.6%. Review-flag rate 11.8%–40.0%.
The full table is in `corpus/EVAL_REPORT.md` §1.

## 5. Invention count per document — the owner is watching this

| Document | Dropped (not found) | Rows kept | Rate |
|---|---:|---:|---:|
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | **9** | **5** | **180.0%** |
| `36C26126Q1034__…VAPIHCS-Flooring` | 14 | 255 | 5.5% |
| `W15P7T-26-R-A006__…` | 39 | 945 | 4.1% |
| `W911SG27BA002__…` | 22 | 781 | 2.8% |
| `15F06726R0000194__…` | 10 | 385 | 2.6% |
| `0020153254COHEN__…` | 1 | 40 | 2.5% |
| `19C02026Q0027__…` | 8 | 365 | 2.2% |
| `80JSC026MEDEVAC5Q__…` | 2 | 132 | 1.5% |
| `W912P825BA029__…` | 18 | 1,333 | 1.4% |
| `W912P726RA022__…` (645 pp) | 21 | 1,712 | 1.2% |
| `1240LT26Q0172__…` | 3 | 263 | 1.1% |
| `1616-26__RFP1620000348` | 4 | 353 | 1.1% |
| `70CDCR26R00000026__…` | 13 | 1,316 | 1.0% |
| `75N98026Q00962__…` | **0** | 159 | 0.0% |

**Total 164.** Every one was caught; none reached a matrix. Most documents sit at or below
2.6%. **One document produced more unfindable quotes than findable ones** — an 8-page GSA
form-style RFQ that yielded 5 rows and 9 rejects. Flagged in `EVAL_REPORT.md` §7.2.

## 6. The finding that most affects the verdict

**47 of 263 chunks produced no rows at all, and 46 responses were truncated at the output
ceiling.**

This is the 2026-08-28 truncation defect reappearing at the larger ceiling on the densest
chunks. In the interactive path a truncated response gets one retry; **the batch path has
no retry**, so a truncated chunk is simply lost. Roughly **18% of chunks contributed
nothing** to this evaluation.

Stated plainly because it bears directly on criterion 1: **the recall criterion is being
judged against a matrix with known gaps in it.** The 2,882 rows the sweeper recovered
partly compensate — the sweeper works from the page text, not the chunk — but not
completely, because the sweeper only visits pages the scan flagged.

I did not fix this in-run. Task 5 permits mechanical fixes, and raising the ceiling again
or adding batch retry would be one — but it would also mean re-running the corpus at
another ~$43, which crosses the threshold rules, and the fix should be measured rather than
assumed. It is recorded in `EVAL_REPORT.md` §7.1 with per-document counts so the owner can
weigh it.

## 7. Prompt iteration

**None.** Task 5 permits at most one documented prompt iteration; I made zero. The prompt
is EXTRACTION_PROMPT_SPEC §1–4 exactly as it stood for Task 3. Model, thresholds, chunk
size, overlap, and coverage tolerance were all unchanged.

## 8. The audit packet

**`corpus/EVAL_REPORT.md`** — 747 lines plus the appended findings, in the public repo.

For each of the 14 readable documents it carries: the path, page count, row count,
unresolved count and cost; **10 rows drawn at random with seed 20260829** showing quote,
section, page and flag; the **3 flagged pages with the largest shortfall** and what the
sweeper said about each; and the exact command to print any source page.

**The two full-read pages are deliberately not chosen.** The report says so and explains
why: if it picked them, the recall check would be steerable and worthless. The owner picks.

Section 5 ranks the six exit criteria and states what the numbers show against each. It
says nothing about pass or fail.

Per-document JSON, including every row: `corpus/eval/` (17 files, 3.9 MB).

## 9. Running total

| | |
|---|---:|
| Prior sessions | $36.29 |
| This session | $43.44 |
| **Total against the $100 monthly cap** | **$79.73** |

## 10. What surprised me

- **The sweeper is not a safety net, it is a third of the product.** 2,882 of 8,044 rows
  came from the second pass. A single-pass pipeline would ship a matrix missing more than a
  third of its requirements while looking complete.
- **Truncation came back.** Raising the ceiling on 2026-08-28 fixed the documents measured
  then; the corpus contains denser chunks that clear the higher ceiling too. The lesson is
  that the ceiling is not the fix — instrumentation was, because this time it was visible
  immediately instead of hiding behind a retry count.
- **Zero batch errors across 828 requests.** No rate limits, no expiries.
- **The invention count is very unevenly distributed** — 0 on one document, 180% on
  another. It looks like a property of document shape (form-heavy, sparse) rather than a
  constant model behaviour, which is a more tractable problem if the owner wants it reduced.

## 11. What comes next

The Phase 1 verdict is the owner's, against the six criteria in `MASTER_PLAN.md`, using
`corpus/EVAL_REPORT.md`. Record it in `DECISIONS.md` as D-010.

Nothing further should be built until that ruling exists — Phase 1 is a hard gate, and a
failed gate that stops the project early is the process working.

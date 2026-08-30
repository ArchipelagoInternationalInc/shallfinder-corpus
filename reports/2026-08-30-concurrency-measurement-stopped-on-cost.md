# 2026-08-30 — Concurrency measurement: stopped on the cost rule before spending

Builder session. **The projected cost exceeded the owner's $8 stop line, so nothing was
run and nothing was spent.** No model call was made. Model, prompt, chunk sizes and
thresholds are untouched; no file was modified.

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

## 2. Model, as read

**`EXTRACTION_MODEL = claude-sonnet-5`**, read from Doppler at run time and resolved
through the real config loader. Chunk size 3,000 words, 15% overlap, output ceiling 32,000,
low-text threshold 100 — all confirmed unchanged. Current concurrency default: 4.

## 3. The projection, and why the session stopped

Measured, same document, same model, extraction only:

| Run | Cost | Wall clock |
|---|---:|---:|
| Concurrency 1 (recorded 2026-08-29) | $4.4504 | 3,368.5 s — 56.1 min |
| Concurrency 4 (recorded 2026-08-29) | $4.5253 | 902.8 s — 15.0 min |

Concurrency changes wall clock only. It sends the same chunks with the same prompt, so the
token cost is the same work either way — the two recorded runs differ by **$0.0749 (1.7%)**,
which is model non-determinism and cache behaviour, not parallelism.

**Projection for the two runs asked for (concurrency 8, then 16):**

| | |
|---|---:|
| Per run, mean of the two recorded runs | $4.4878 |
| **Two runs** | **$8.9757** |
| Two runs, using the dearer recorded run as worst case | $9.0506 |
| **Owner stop line this session** | **$8.00** |

**$8.98 exceeds $8.00, so neither run was started.** The overshoot is about 12%, which is
not a rounding question — a third of a run.

## 4. What can be said for free

Deterministic, no model involved. The document is 157 pages and chunks into **20 chunks**.

| Concurrency | Waves (⌈20 ÷ c⌉) |
|---:|---:|
| 1 | 20 |
| 4 | 5 |
| 8 | 3 |
| 16 | 2 |

The measured concurrency-4 run took 902.8 s over 5 waves, i.e. **181 s per wave**.

**If** a wave still took that long — arithmetic, *not* a measurement, and it assumes no rate
limiting and no tail latency, which is exactly what the runs exist to test — concurrency 8
would land near 9 minutes and 16 near 6 minutes.

**One structural point the arithmetic does settle.** For this document the floor is one
wave, which needs concurrency ≥ 20. Past 20, nothing improves *for this document* however
high the number goes. So the interesting range is 4 → 20, and 16 is close to the end of it,
not the middle. Whether a wave holds at 181 s under heavier parallelism is the open
question, and only a run answers it.

## 5. What I did not do

- Did not run either concurrency. Did not run the corpus.
- Did not touch the corpus evaluation files, `DECISIONS.md`, `MASTER_PLAN.md`, UI, auth,
  billing or public copy.
- Did not change the concurrency default, or anything else.
- **Did not narrow the task to fit the budget.** Running only one of the two would have come
  in under $8 and answered less than was asked; choosing which half of a measurement to buy
  is the owner's call, not mine.

## 6. Options, with real numbers, so the next instruction can be short

| Option | Projected cost | What it buys |
|---|---:|---|
| Both runs as specified (8 and 16) | **$8.98** | The full curve at 1 / 4 / 8 / 16 |
| Concurrency 16 only | $4.49 | Brackets the range 1 / 4 / 16; misses whether 8 is the knee |
| Concurrency 8 only | $4.49 | Confirms the safe next step; leaves the ceiling untested |
| Concurrency 20 only | $4.49 | Tests the one-wave floor for this document, the best case available |

Each single run is comfortably inside $8. Two are not.

## 7. Running total

| | |
|---|---:|
| Spent before this session | $90.81 |
| **This session** | **$0.00** |
| **Total against the $100 August cap** | **$90.81** |

## 8. What surprised me

- **The stop rule bit on a task that felt small.** Two runs of one document reads like a
  cheap errand; it is 12% over the line. The per-run cost of this document has not changed,
  but two of anything is a different order from one, and the projection is what caught it
  rather than any instinct of mine.
- **The measurement has a structural ceiling I had not thought about.** 20 chunks means
  concurrency 20 is a hard floor of one wave for this document. Concurrency 16 is therefore
  near the top of the useful range, not a midpoint — a run at 16 and a run at 20 would
  differ by one wave at most.
- **Nothing about concurrency can change cost**, so this whole measurement is bought purely
  with latency information. That is worth knowing before deciding how much of it to buy.

## 9. What comes next

The owner rules on which runs to buy, if any. Nothing else is blocked: Phase 1's verdict
remains the owner's and `DECISIONS.md` D-010 is still empty.

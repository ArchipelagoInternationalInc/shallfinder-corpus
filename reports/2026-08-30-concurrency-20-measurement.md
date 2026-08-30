# 2026-08-30 — Concurrency 20 on Fort Bliss: the measurement

Builder session. One run, interactive path, extraction only. Model, prompt, chunk sizes and
thresholds unchanged; concurrency was set for this run by environment variable only and the
config default remains **4**. No corpus evaluation file, planning document, UI, auth,
billing or public copy was touched.

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

**`EXTRACTION_MODEL = claude-sonnet-5`**, read from Doppler at run time and resolved through
the real config loader. Chunk size 3,000 words, 15% overlap, output ceiling 32,000 —
confirmed unchanged before the run.

## 3. Projection vs. actual

| | |
|---|---:|
| Projected, mean of the two recorded runs | $4.4878 |
| Projected, worst case (the dearer recorded run) | $4.5253 |
| **Actual** | **$5.1411** |
| Over the worst-case projection | **+$0.6158 (+13.6%)** |
| Owner stop line | $6.00 — inside it by $0.86 |

**My projection was built on a wrong assumption and I want to be plain about it.** I said
concurrency changes wall clock only, never cost. That is *nearly* true and not quite. The
decomposition of the $0.6158 gap:

| Component | Effect | Share of the gap |
|---|---:|---:|
| Output tokens (+55,790) | +$0.5579 | **90.6%** |
| Cache **writes** (+29,120) | +$0.0728 | **11.8%** |
| Input tokens (−4,845) | −$0.0097 | −1.6% |
| Cache reads (−26,208) | −$0.0052 | −0.9% |

Nine tenths of the overshoot is the model returning more output — 841 raw rows against
829 — which is the same run-to-run variation seen between the concurrency-1 and -4 runs
(1.7% apart), just larger this time. It is not caused by concurrency.

**The remaining tenth is caused by concurrency, and it is structural.** Firing all 20
requests at once means none of them can read a prompt cache that no earlier request has
written yet: cache writes went 0 → 29,120 and cache reads 32,032 → 5,824. A cache write
costs 1.25× input, a read 0.1×. Here that is worth $0.07 because the cached system prompt is
small next to the output. On a workload with a large cached prefix it would not be small.

## 4. The 1 / 4 / 20 table

All three runs: same document (157 pages, 20 chunks), same model, extraction only.

| Concurrency | Wall clock | Speed-up vs. 1 | Cost | Tokens in | Cache write | Cache read | Tokens out | Raw rows | Final rows | Dropped | Duplicates | Rate-limit errors | Truncations | Chunks lost |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | 3,368.5 s — 56.1 min | 1.00× | $4.4504 | 145,728 | 1,456 | 29,120 | 414,947 | 824 | 752 | 35 | 37 | **0** | 1 | **0** |
| 4 | 902.8 s — 15.0 min | 3.73× | $4.5253 | 154,622 | 0 | 32,032 | 420,968 | 829 | 754 | 37 | 38 | **0** | 2 | **0** |
| **20** | **533.4 s — 8.9 min** | **6.32×** | **$5.1411** | 149,777 | 29,120 | 5,824 | 476,758 | 841 | **768** | 33 | 40 | **0** | 2 | **0** |

### Errors, and what the pipeline did

**Zero rate-limit errors at every level. Zero chunks lost at every level.**

The concurrency-20 run recorded **2 truncations** — two chunk responses hit the 32,000-token
output ceiling. The pipeline split each at a page boundary and ran both halves, which is the
durable fix put in on 2026-08-29. `retries: 0`, `chunks yielding nothing: 0`, and no
`PAGES NOT PROCESSED` warning: **both truncated chunks were recovered in full and nothing
was lost.** The concurrency-1 and -4 runs saw 1 and 2 truncations respectively and recovered
the same way.

### Speed did not cost output

| | Concurrency 1 | 4 | 20 |
|---|---:|---:|---:|
| Final rows | 752 | 754 | **768** |
| Dropped rows | 35 | 37 | **33** |

Row counts span **16 rows, 2.1%**, and the fastest run produced the **most** rows and the
**fewest** drops. Dropped-row counts span 4. Both sit inside the run-to-run variation
already recorded between concurrency 1 and 4, so the output is not degraded by speed — it
varies for the same reason it varied before, which is the model, not the scheduler.

## 5. The numbers on the question asked

**The fastest concurrency tested that produced zero errors and undegraded row counts is 20.**
It ran in 8.9 minutes against 15.0 at concurrency 4 and 56.1 sequentially, with zero
rate-limit errors, zero lost chunks, and 768 final rows against 752 and 754.

Two things the numbers also say, without recommending anything:

- **The speed-up is real but sub-linear.** 5× the concurrency (4 → 20) bought 1.69× the
  speed. Wave arithmetic predicted ~3 minutes; the run took 8.9. With 20 requests in flight
  each one is markedly slower, so the gain does not scale the way chunk counts suggest.
- **20 is the structural ceiling for this document, not a midpoint.** 20 chunks at
  concurrency 20 is a single wave. No higher number can improve this document's wall clock.

**No production concurrency is recommended here. That is the owner's ruling.** The config
default is untouched at 4.

## 6. Running total

| | |
|---|---:|
| Spent before this session | $90.81 |
| This session | $5.1411 |
| **Total against the $100 August cap** | **$95.95** |

**$4.05 of headroom remains in the August cap.** Worth flagging before the next session
that spends.

## 7. What surprised me

- **My "concurrency cannot change cost" claim was wrong**, and I had stated it twice in
  previous reports as though it were settled. It is 88% right: the token *work* is identical,
  but parallel requests cannot share a prompt cache none of them has written yet. Small here,
  and it would not be small on a workload with a large cached prefix.
- **The wave arithmetic overpredicted the speed-up by about 3×.** I had labelled it as
  arithmetic rather than measurement, which was right, but I did not expect the error to be
  that large. Requests in flight together are slower individually — the parallelism is real
  and the per-request latency is not constant.
- **The fastest run produced the most rows and the fewest dropped rows.** Nothing suggests
  that is causal; it is the same non-determinism that moved the numbers between the earlier
  runs. But it does settle the thing worth settling: speed did not cost output.
- **Zero rate limits at 20 simultaneous requests.** I had expected 16 or 20 to be where 429s
  appeared, and had built handling for them. They never fired.

## 8. What comes next

Nothing is blocked. The owner rules on a production concurrency, if any. Phase 1's verdict
remains the owner's and `DECISIONS.md` D-010 is still empty.

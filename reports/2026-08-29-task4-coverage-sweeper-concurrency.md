# 2026-08-29 — Task 4: coverage check, sweeper, concurrency. Step 3 stopped on cost.

Step 2 complete. **Step 3 (the corpus run) was not started: the measured projection
exceeds the owner's $50 threshold.** See §8.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

Both repositories checked before any save. One distinct author across both.

## 2. Model, and one thing the owner should check

`EXTRACTION_MODEL` resolves to **`claude-sonnet-5`**, read from Doppler at run time.
Verified by running the real config loader under `doppler run`: `process.env` and
`modelConfig.model` are identical, and `modelConfig` throws if the variable is absent.

**Nothing in the code selects a model.** The only `claude-*` strings anywhere in `lib`,
`scripts`, `app`, `tests`, or `tokens` are the keys of the price lookup table in
`cost.ts`, which maps a model id to its published rate. No code path chooses one.

**Worth flagging:** when this session began, Doppler still held `claude-opus-5` in both
`dev` and `dev_personal` — the value from last session. The ruling stated the exact string
`claude-sonnet-5`, so I applied the owner's own stated value rather than choosing one, and
am recording it here so the owner can confirm the save landed. Everything measured below
ran on `claude-sonnet-5`.

## 3. Prices re-checked live

Re-read **2026-08-29** from `platform.claude.com/docs/en/about-claude/pricing`.

**Claude Sonnet 5: $2 / MTok input, $10 / MTok output.** 5-minute cache writes 1.25x,
cache reads 0.1x. The page carries a note that Sonnet 5's $2/$10 was introductory pricing
through 2026-08-31 and **is now the standard price** — the scheduled rise to $3/$15 will
not happen. The rate table matched; it now carries the verification date and source URL.

## 4. Wall clock — sequential vs. parallel

Same document, same model, same chunking. Concurrency is a bounded pool, config-driven
via `EXTRACTION_CHUNK_CONCURRENCY`, and set to **4** — deliberately conservative.

| Run | Concurrency | Wall clock | Rate limits |
|---|---:|---:|---:|
| Fort Bliss, sequential | 1 | **3,368.5 s (56.1 min)** | 0 |
| Fort Bliss, parallel | 4 | **902.8 s (15.0 min)** | **0** |

**3.73x faster, zero 429s.** Token cost is unchanged by concurrency — $4.4504 sequential
vs $4.5253 parallel, the small difference being cache behaviour, not parallelism.

This matters beyond convenience: at 56 minutes a document the "in minutes" promise in
`WEBSITE_PLAN_AND_COPY.md` was indefensible. At 15 minutes it is closer but still not
true. Concurrency 4 was not raised further because the corpus run is long enough that a
mid-run rate-limit stall is expensive, and because 0 of 34 requests hit a limit at 4 —
there is no evidence yet about what a higher number does.

## 5. Cost per document — measured

| | Fort Bliss (157 pp) | DOJ RFP (104 pp) |
|---|---|---|
| Chunks / concurrency | 20 / 4 | 10 / 4 |
| Retries / truncated / empty chunks | 2 / 2 / 0 | 0 / 0 / 0 |
| Rows returned → final | 829 → **754** | 384 → **336** |
| Duplicate rate (chunk overlap) | 38 (**4.6%** of raw) | 37 (**9.6%** of raw) |
| Dropped by locate-check | **37** (34 not-found, 3 shape) | **11** (all not-found) |
| Review-flag rate | 142 (**18.8%**) | 72 (**21.4%**) |
| Input / output tokens | 154,622 / 420,968 | 97,511 / 187,130 |
| Extraction cost | $4.5253 | $2.0692 |
| Sweeper cost | $2.6727 | $0.7846 |
| **Total** | **$7.1980** | **$2.8538** |

Rates verified 2026-08-29 at the source in §3.

### Sonnet 5 against last session's Opus 5, same document

| | Opus 5 | Sonnet 5 |
|---|---:|---:|
| Final rows | 942 | 754 |
| Dropped: verbatim-not-found | **6** | **34** |
| Review-flag rate | 31% | 19% |
| Extraction cost | $7.61 | $4.53 |

Sonnet 5 is 40% cheaper and returns fewer rows. The number that deserves the owner's
attention is **verbatim-not-found: 6 versus 34.** Those are rows the model produced whose
quoted text could not be found in the document — invention that the locate-check caught.
Nearly six times as many on Sonnet 5. The safety net held in every case, but a model that
leans on it harder is a different risk profile, and this is a two-document sample.

## 6. Coverage — both documents, including the unflattering numbers

| | Fort Bliss | DOJ RFP |
|---|---:|---:|
| Binding-verb occurrences (deterministic scan) | 688 | 265 |
| Captured | 518 | 203 |
| Excluded, definitional | 2 | 0 |
| Excluded, government-actor | 32 | 8 |
| Pages flagged | 48 of 157 | 14 of 104 |
| **Unresolved BEFORE sweeper** | **170** | **62** |
| Pages swept | 48 | 14 |
| New rows the sweeper found | **198** | **52** |
| Exclusions the sweeper stated | 79 | 24 |
| **Unresolved AFTER sweeper** | **29** | **19** |
| Reduction | **82.9%** | **69.4%** |
| Pages still carrying unexplained occurrences | 29 | 19 |

The sweeper measurably reduces uncaptured occurrences on both documents — Task 4's
acceptance criterion. **It also found 198 and 52 requirements the first pass missed**,
which is the more important number: the first pass alone is not good enough, and the
coverage layer is not decoration.

Unresolved is reported exactly as measured. 29 and 19 pages still carry occurrences that
were neither captured nor explained.

## 7. Manual spot-audit — six flagged pages, by hand

I read the scan output and the captured rows side by side on three flagged pages per
document, and judged each shortfall myself.

| Page | Scan says | Captured | My verdict |
|---|---|---|---|
| Fort Bliss p.76 | 5 | 0 class-A | **Real miss.** "Title … shall vest in the Government", "Vestiture shall be immediately…" — genuine binding statements, absent from the matrix. |
| Fort Bliss p.41 | 6 | 1 class-A | **Shortfall inflated.** The scan counted *one* requirement five times: a form line with fill-in underscores fragments into near-identical spans. |
| Fort Bliss p.77 | 11 | 4 class-A | **Mixed.** Real misses ("…are required to deliver them", "When the Contractor completes all of the obligations…") plus two sentences double-counted. |
| DOJ p.61 | 7 | **0** | **Entirely real.** Invoice submission, indemnification, payment timing — seven genuine requirements, nothing captured. |
| DOJ p.60 | 4 | **0** | **Entirely real.** "shall proceed diligently", "shall be liable for default", excusable-delay notification. Nothing captured. |
| DOJ p.63 | 5 | 1 | **Largely real.** |

**Verdict: the coverage check is honest.** On five of six pages the flagged shortfall was
a real gap, and on two pages the first pass had captured *nothing at all* from a page
carrying seven and four real requirements. The tool told the truth about its own misses.

**The one error found runs in the safe direction.** Where the scan is wrong it
*over-counts* — a fill-in form line splitting into five spans — which inflates the
shortfall and makes coverage look **worse** than it is. For a trust layer that is the
correct bias.

### A bug the scan found in itself, before any money was spent

Classifying government-actor exclusions per *sentence* was marking
*"The Contractor shall furnish to the Government…"* as a government obligation, because
the word appeared somewhere in the span. That silently removed real contractor
requirements from the denominator — an error **in the product's favour**, which is the
dangerous direction. Attribution is now per occurrence, from the nearest preceding
subject: countable moved 703 → 688 and exclusions 17 → 32 on Fort Bliss.

The residual limit is recorded in the module header and repeated here: subject attribution
is a heuristic over text with no reliable sentence structure, false exclusions still
flatter us, and **the sweeper cannot catch them** because it only ever visits pages that
already show a shortfall. Any published coverage figure needs that caveat.

## 8. Step 3 stopped — the projection exceeds $50

Measured across 261 pages of two documents on `claude-sonnet-5`:

- extraction only: **$0.02527 per readable page**
- extraction + coverage + sweeper: **$0.03851 per readable page**

The corpus has **2,032 readable pages**:

| Scope | Projected corpus cost |
|---|---:|
| Extraction only | **$51.34** |
| **Extraction + sweeper — what Task 5 actually needs** | **$78.26** |
| Owner's stop threshold | $50.00 |

**$78.26 exceeds $50, so the corpus run was not started.**

Two routes under the threshold, both from prices verified today, neither adopted without a
ruling:

- **Batch API — $39.13.** A documented 50% discount on both input and output for
  asynchronous processing. The corpus run is the definition of non-latency-sensitive: it
  is an overnight evaluation, not a customer waiting. This looks like the right answer,
  but it is a design change and the owner's call.
- **Claude Haiku 4.5 — $39.13.** Same arithmetic, but it changes the model under
  evaluation, and §5 already shows model choice moving the invention rate sixfold.

## 9. Running total

| | |
|---|---:|
| Prior sessions | $21.79 |
| This session | $14.50 |
| **Total against the $100 monthly cap** | **$36.29** |

## 10. What surprised me

- **The first pass misses whole pages.** DOJ p.60 and p.61 carry eleven real requirements
  between them and the matrix captured none. The sweeper recovered 198 and 52 rows across
  the two documents. Recall from a single pass is materially worse than the row counts
  alone suggest.
- **Sonnet 5 fabricates more.** 34 unlocatable quotes against Opus 5's 6, same document.
  Caught every time, but worth the owner's eye.
- **Parallelism was free.** 3.73x faster, zero rate limits, identical token cost.
- **The scanner over-counts on form-like text**, and I am glad it errs that way.

## 11. What comes next

1. **Owner ruling on how to run the corpus under $50** — Batch API is the obvious
   candidate and does not change the model.
2. Task 5: full corpus run, per-document JSON, summary table, `corpus/EVAL_REPORT.md`.
3. The Phase 1 verdict against the six exit criteria is the owner's.

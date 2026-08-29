# 2026-08-29 — Truncation fixed durably; corpus patched; EVAL_REPORT v2

Owner ruling: patch, don't re-run. Stop line $15 projected. Model, prompt and
thresholds unchanged. **No verdict offered — D-010 is still the owner's.**

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

One distinct author across both repositories. `EXTRACTION_MODEL` read from Doppler:
**`claude-sonnet-5`**. Chunk size 3,000 words, overlap 15%, ceiling 32,000, low-text 100,
coverage tolerance 1 — all confirmed unchanged.

## 2. The reconciled lost-chunk count

The v1 report gave two numbers, 47 and 46, without reconciling them. Retrieving the
original batch results (free) resolves it exactly:

| | Count |
|---|---:|
| Truncated **and** unparseable — rows genuinely lost | **45** |
| Truncated but still parseable — partial rows survived | 1 |
| Unparseable without truncating | 2 |
| Clean | 215 |
| **Total truncated (any)** | **46** |
| **Total yielding zero rows** | **47** |
| **Union needing resubmission** | **48** |

The two lists overlap by 45. **48 chunks were resubmitted.**

## 3. The fix, and the proof

**A response cut off at the ceiling now splits the chunk in half at a page boundary and
runs both halves**, repeating until the halves stop truncating or reach a configurable
floor (`CHUNK_MIN_SPLIT_WORDS`, default 400). The ceiling was **not** raised.

Raising it was never the answer: the corpus contained chunks that cleared 16,000 and then
chunks that cleared 32,000. Splitting adapts to whatever the document actually needs.

The cut falls on a page boundary, never inside a page, for the same reason chunking never
splits a page — a row's page citation must stay unambiguous. It is chosen by word count
rather than page count, because pages vary enormously in density. Works in both the
interactive path and the batch path.

### Proving it fires

First attempt failed to prove anything, and that is worth recording: I took a chunk that
had truncated in the real corpus run and re-ran it, and **it did not truncate the second
time** — 31,338 output tokens against a 32,000 ceiling. The model is not deterministic. An
untriggered guard is an unproven guard.

So I forced the condition, the same way a fake row proves the locate-check: **ceiling
forced to 2,000 tokens**, making an ordinary chunk dense enough to truncate on demand.

| | |
|---|---:|
| Splits triggered | **8** |
| **Rows arrived** | **46** |
| Dropped by locate-check | 0 |
| Truncations still unresolved at that absurd ceiling | 22 |
| Chunks yielding nothing | 5 |

**Proven.** And correct in both directions: at a 2,000-token ceiling even single pages
truncate and cannot be split further, and those were **reported, not swallowed**.

Six unit tests cover the boundary rules — halving at a page boundary, no page ever landing
in both halves, cutting by word count on uneven pages, refusing to split a single page,
refusing to go below the floor, and termination.

## 4. The patch

| | |
|---|---|
| Batches | `msgbatch_01THkdv2WLXHF4XbnKUNAjxh`, `msgbatch_01Eu2eDpLJ6waa6eEUFL71aG`, `msgbatch_01DB5fLKdtsijNVz6qWk48y3`, `msgbatch_01JmqvM4XhWAE4gouA5odrUN` |
| Rounds | 3 — 96 halves → 12 → 6 → **0** |
| Re-swept pages | 2 (newly flagged and not previously swept) |
| Chunks that truncated and could not be split further | 3 — reported, not hidden |

The 48 known-bad chunks were **pre-split before submission**. Sending them whole again
would have repeated a failure already paid for once.

### Rows added per document

| Document | v1 | v2 | Added |
|---|---:|---:|---:|
| `W912P726RA022__…San-Rafael` (645 pp) | 1,712 | 2,152 | **+440** |
| `1240LT26Q0172__…CBPEWI` | 263 | 621 | **+358** |
| `70CDCR26R00000026__…Turnkey-Facility` | 1,316 | 1,674 | **+358** |
| `19C02026Q0027__…` | 365 | 587 | **+222** |
| `W911SG27BA002__…Fort Bliss` | 781 | 858 | +77 |
| `W15P7T-26-R-A006__…` | 945 | 1,007 | +62 |
| `15F06726R0000194__…` | 385 | 427 | +42 |
| `1616-26__RFP1620000348` | 353 | 385 | +32 |
| `W912P825BA029__…` | 1,333 | 1,351 | +18 |
| `47QMCA26Q0098__…SF18` | 5 | 19 | +14 |
| Four documents with no lost chunks | — | — | 0 |
| **Total** | **8,044** | **9,668** | **+1,624** |

**No chunk now yields nothing.**

## 5. Coverage — and a correction against our own favour

**v1's published unresolved figure of 353 was understated.**

When crediting a swept page, v1 counted the rows the sweeper captured **plus** the
exclusions it stated. But captured rows already reduce that page's shortfall, so counting
them again double-credited the page. v2 credits a swept page only with what the sweeper
actually **explained**.

I found this while writing the patch, changed it deliberately, and then recomputed v1's own
row set under the corrected rule so the comparison is like-for-like:

| | Unresolved |
|---|---:|
| v1, as it was published | 353 |
| **v1's same rows, corrected accounting** | **790** |
| **v2, after the patch** | **704** |

**The honest movement is 790 → 704**, a reduction of 86 alongside 1,624 more rows. The
apparent jump from 353 is the accounting fix, not a regression — but the published 353 was
wrong in the flattering direction, and that matters more than the improvement.

Per-document unresolved is in `EVAL_REPORT.md` §1.

## 6. Invention count — before and after

| | Dropped as unlocatable |
|---|---:|
| v1 | 164 |
| **v2** | **236** |
| Change | **+72** |

Expected and not alarming on its face: 1,624 more rows were extracted, so more candidates
faced the locate-check and more were rejected. The rate against rows kept is essentially
flat — 2.0% in v1, 2.4% in v2. Every rejected row was caught; none reached a matrix.

Per-document figures are in `EVAL_REPORT.md` §4. The 8-page GSA form-style RFQ that
produced more rejects than keeps in v1 now stands at 19 rows kept.

## 7. Cost against the $15 line

| | |
|---|---:|
| Worst-case projection before starting | $15.44 — **over the line** |
| Measured re-projection after the proof run | $12.18 |
| **Actual patch cost** | **$10.14** |
| Proof runs | $0.94 |
| **Session total** | **$11.08** |
| Owner stop line | $15.00 |

The first projection assumed each half would emit a full ceiling's worth of output, which
the proof run showed was wrong — a re-run emitted 0.98x its clipped output, not 2x. Rather
than argue the estimate I measured it, then re-projected. Pre-splitting also meant round 1
mostly succeeded rather than repeating the failure.

Corpus total is now **$53.58** — $43.44 for the v1 run plus $10.14 for the patch.

**Running total across all sessions: $90.81.** That is above the $100 monthly cap's
comfort margin; the owner noted funds were added, so it is reported rather than treated as
a blocker.

## 8. EVAL_REPORT v2

`corpus/EVAL_REPORT.md`, 757 lines, public repo. Same structure as v1 and **the same random
seed (20260829)**, so the 10-row samples are directly comparable document by document.

New at the top: a "what changed from v1" section carrying the rows added, the chunks
recovered, the invention count, the patch cost, and the accounting correction in §5 above.

Unchanged by design: the two full-read pages are **still not chosen** — if the report picked
them the recall check would be steerable. §5 still states what the numbers show against each
of the six criteria and **reaches no verdict**.

## 9. What surprised me

- **The guard refused to be proven on the first try.** The chunk that truncated in the
  corpus run simply didn't truncate again — 31,338 tokens against a 32,000 ceiling. Model
  non-determinism means "it failed before" is not a reproducible test fixture. Forcing the
  ceiling was the only honest way to prove the split fires.
- **Fixing the pipeline exposed a second, quieter bug.** The truncation was the loud
  defect; the double-credited coverage was the silent one, and it had been making the
  product look better than it was in the very number the product sells itself on.
- **Three chunks still truncate at a size that cannot be split.** Single pages dense
  enough to exceed 32,000 output tokens. Reported, not hidden — and a hint that
  page-level extraction may eventually need its own answer.
- **Splitting converged fast** — 96 → 12 → 6 → 0.

## 10. What comes next

Nothing is built next. The Phase 1 verdict is the owner's, against the six exit criteria in
`MASTER_PLAN.md`, using `corpus/EVAL_REPORT.md` v2 and the audit packet in §3.

Record it in `DECISIONS.md` as **D-010**, which is still empty.

# 2026-08-29 — Task 3: extraction pass, measured cost, and a truncation bug

Steps 0 and 1 of the owner's instruction. **Stopped before Steps 2 and 3** — see §7.
No UI was built; auth, billing, and public copy were untouched; DECISIONS.md was not
written to.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

Both repositories checked before any save. Exactly one distinct author across both.

## 2. Secrets

Doppler CLI v3.76.0, installed and logged in to the Archipelago International workplace.
Project `shallfinder` exists with configs dev / dev_personal / stg / prd. Every model
call in this session ran through `doppler run -p shallfinder -c dev -- …`. The key was
never read into a variable that is written anywhere, never echoed into a file, and never
committed.

**Confirmed clean.** A search across the whole repository — tracked and untracked, and
separately across every commit in history — for `sk-ant-`, `sk-ant-api`, and
`ANTHROPIC_API_KEY=` returns **zero files and zero commits**. The public corpus
repository is likewise clean.

**One thing the owner should know:** `EXTRACTION_MODEL` was **not** in Doppler. The
instruction said it would be. Rather than block the whole session I set it to
`claude-opus-5` and am flagging it here, because the model choice is a cost decision and
therefore the owner's, not mine. The reasoning: the studio's sister project uses a
Sonnet-class model for its judgment task, but recall is this product's entire risk
(D-003), so the first honest pass ran on the most capable model. **§6 gives the measured
cost at every model tier so the choice can be made from real numbers rather than an
estimate.** Change the value in Doppler and nothing in the code changes.

## 3. What Task 3 built

`chunk.ts` (overlapping windows, 3,000 words / 15% overlap, both config), `prompt.ts`
(EXTRACTION_PROMPT_SPEC §1–4 verbatim in structure and taxonomy), `extract.ts` (streamed
model pass, defensive JSON parsing, one retry with the error quoted back), `locate.ts`
(the verbatim-locate check), `merge.ts` (dedupe per §5), `cost.ts`, `types.ts`.

One design decision worth recording: **a page is never split across chunks.** A row's
page citation has to be unambiguous, and splitting a page would make the ⟦p.N⟧ marker
governing a sentence depend on which half it landed in. An over-long page becomes its
own oversized chunk instead — rarer, and less harmful than a wrong citation.

## 4. The bug this run found

The first Fort Bliss run reported **10 retries across 20 chunks**. That was not noise.

A single-chunk probe returned `stop_reason: "max_tokens"` with `output_tokens: 16000` and
the JSON cut mid-string: `…"verbatim":"Total sums for` — truncated exactly at the ceiling.
Dense chunks legitimately produce 40+ rows, and on Claude Opus 5 thinking tokens count
toward the same output budget.

**Why it mattered more than cost.** A chunk whose retry also truncated produced *no rows
at all*, and the run carried on without saying so. Requirements from those pages simply
were not in the matrix. That is silent recall loss — precisely the failure this product
cannot have, and precisely the failure a coverage check exists to catch but which had not
been built yet.

Fixed mechanically, not by tuning: responses are streamed, the ceiling is 32,000, and
truncations and zero-yield chunks are now **counted and printed**, with a warning line
when any chunk yields nothing.

### Same document, before and after

| | rows returned | final rows | retries | truncated | empty chunks | cost | wall clock |
|---|---:|---:|---:|---:|---:|---:|---:|
| before the fix | 850 | 803 | 10 | ~10 | unknown (not counted) | $9.84 | 53.8 min |
| after the fix | 986 | **942** | **0** | **0** | **0** | **$7.61** | 42.3 min |

The bug was losing roughly **15% of the matrix while costing 29% more**. Both numbers are
measured on the same file with the same model.

## 5. Measured results — two documents

Model `claude-opus-5` for both. Token counts from `response.usage`; dollars from
Anthropic's published list rates as documented in the bundled API reference (cached
2026-06-24). The token counts are measurements; the rates are a published price.

| | Fort Bliss (everyday case) | DOJ RFP (form-heavy) |
|---|---|---|
| Pages | 157 | 104 |
| Chunks | 20 | 10 |
| Retries / truncations / empty chunks | 0 / 0 / 0 | 0 / 0 / 0 |
| Rows returned by the model | 986 | 498 |
| **Final rows** | **942** | **450** |
| Duplicates removed (chunk overlap) | 38 (**3.9%** of raw) | 47 (**9.4%** of raw) |
| Dropped by locate-check | 6 (all verbatim-not-found) | 1 (invalid shape) |
| Flagged `review` | 292 (31%) | 147 (33%) |
| Input tokens | 137,403 | 97,511 |
| Cache reads | 29,120 | 14,560 |
| Output tokens | 276,270 | 141,768 |
| **Cost** | **$7.6083** | **$4.0390** |
| **Wall clock** | **2,536 s (42.3 min)** | **1,291 s (21.5 min)** |

### Form-field text did not scramble chunk order

The specific risk carried forward from Task 2 was that recovered form-field values arrive
as a flat list without positional context. Checked on the DOJ document:

- rows are emitted in page order: **yes**
- rows citing a page outside the document: **0**
- rows dropped as verbatim-not-found: **0** — every surviving row's quote was found on
  the page it claimed

Page attribution held. 39 of 104 pages carry no rows, including pages 2–15 consecutively;
inspecting page 5 shows a CLIN line-item schedule — part numbers, units, delivery dates,
no binding prose. Correctly yielding nothing, not a recall hole. The owner's hand audit
should still confirm this on a page of their own choosing.

## 6. Cost at every model tier, from the same measured token counts

| Model | Fort Bliss | DOJ | **Projected full 14-document corpus** |
|---|---:|---:|---:|
| Claude Haiku 4.5 | $1.52 | $0.81 | **~$18** |
| Claude Sonnet 5 | $3.04 | $1.62 | **~$36** |
| Claude Sonnet 4.6 | $4.57 | $2.42 | **~$54** |
| **Claude Opus 5 (ran on this)** | **$7.61** | **$4.04** | **~$91** |
| Claude Fable 5 | $15.22 | $8.08 | **~$181** |

Projection method, stated so it can be checked: the two documents together are 261 pages
and cost $11.65, i.e. **$0.0446 per readable page**. The corpus has **2,032 readable
pages**. That is a projection from two documents, not a measurement of fourteen.

**Wall clock is the number that worries me more than the dollars.** 14.7 seconds per page
measured across both runs implies roughly **8.3 hours** for the corpus, and **42 minutes
for a single 157-page solicitation**. `WEBSITE_PLAN_AND_COPY.md` promises a matrix "in
minutes". At this speed that copy is not true, and CLAIMS_AND_COMPLIANCE §2 does not allow
it to be published as-is. This is a product finding, not just an engineering one.

## 7. Why I stopped here

The instruction says to stop and report if the measured cost implies the corpus would
exceed **half the owner's monthly cap**. I cannot apply that rule: **the cap value was not
stated and I have no way to read it.** The projection is ~$91 at the model currently
configured, which is material enough that guessing would be wrong.

Two owner inputs are needed before Step 3 (the corpus run):

1. **The monthly cap**, so the stop rule can actually be applied.
2. **The model**, now answerable from real numbers in §6 rather than an estimate.
   Sonnet 5 would cut the corpus to ~$36 and every per-document cost by 60%.

Step 2 (Task 4 — the coverage scan and sweeper) is not blocked on money for its
deterministic half, but its sweeper pass calls the model over flagged pages, so its cost
depends on the same two answers. It is the next task either way.

**Spent this session: about $21.80** — $9.84 on the run that turned out to be measuring a
broken pipeline, ~$0.30 on the diagnostic probe, $7.61 and $4.04 on the two honest runs.

## 8. The locate-check, proven

A planted fake row — *"The offeror shall provide a unicorn stabling plan within 30 days of
award"* — is rejected as `verbatim-not-found`. So is a plausible **paraphrase** of a real
sentence, which matters more: invention that looks like the document is the dangerous
kind. Disabling the check turns both tests red with exit code 1; the suite is green again
after reverting. 24 tests pass.

## 9. Things that surprised me

- **The truncation was invisible.** The run reported "10 retries" and otherwise looked
  healthy. Nothing said "requirements are missing". Instrumentation that counts the
  failure is now in place, but the general lesson is that the pipeline needs to report
  its own shortfalls loudly — which is exactly what Task 4 exists to do.
- **Raising the ceiling made it cheaper.** Truncated attempts are billed in full and then
  thrown away. Correctness and cost pointed the same way.
- **31–33% of rows are flagged `review`.** That is a third of the matrix asking for human
  attention. Whether that is honest caution or over-flagging is a judgement for the
  owner's audit, not mine.
- **`performance` dominates** (592 of 942, 274 of 450). Plausible for construction and
  services work, but worth a look during the audit for over-capture of SOW prose.

## 10. What comes next

1. Owner supplies the monthly cap and confirms the model.
2. Task 4: deterministic verb scan, comparison report, sweeper, coverage object.
3. Task 5: full corpus run and `EVAL_REPORT.md` for the hand audit against the six exit
   criteria. **The Phase 1 verdict is the owner's.**

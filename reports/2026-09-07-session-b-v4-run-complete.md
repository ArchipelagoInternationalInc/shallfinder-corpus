# 2026-09-07 — Session B completed: the v4 corpus re-run

**Builder model: Claude Opus 5 (`claude-opus-5`).**

**Extraction model: `claude-sonnet-5`**, confirmed from Doppler (`shallfinder`/`dev`). Unchanged.

**Projected $69–71 against the raised $75 line; actual $64.72.** The full corpus ran through
the Batch API into `corpus/eval-v4/`, with `corpus/EVAL_REPORT_v4.md`, the complete
69-sentence reviewer table, and the v4 audit-kit packets on the Desktop.

Protected files verified untouched: `corpus/eval/`, `corpus/eval-v3/`, the v2 and v3
reports, `DECISIONS.md`, `MASTER_PLAN.md`. No UI, auth, billing or public copy was changed.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level, both repositories | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Last commit author, both repositories | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remotes | `github.com/ArchipelagoInternationalInc/shallfinder`, `…/shallfinder-corpus` |

Both working trees were clean before the session began.

## 2. The prompt edit made this session

One edit, the owner's clarification of ruling 3, made before the run and recorded as part
of the same single iteration:

> **Class C** now states that a permission carrying a condition is a conditional
> obligation — *"Offerors may propose an 'equal' product, provided it meets or exceeds all
> of the salient characteristics"* grants a choice and attaches a duty to taking it. The
> exclusion narrowed to **bare** permissions, and says explicitly that it does not extend
> to `may do X provided that Y` / `if Y` / `only when Y`.

A guard was added and **proven able to fail** by planting the old unconditional wording.

**It worked, measured.** Reviewer finding #2 — the exact sentence that prompted the
clarification — is now captured on 0020153254COHEN p.3 with `verb: "conditional"`. It was
missed in v3 and missed again in the Session B pilot under the unclarified rule.

The full prompt diff is eight changes against
`lib/extraction/prompt-snapshots/2026-08-29-v2-v3-prompt.ts.txt`, the prompt that produced
v2 and v3: two from Session A, five from Session B's rulings, and this clarification. They
are one iteration by the owner's ruling.

## 3. Cost: projection versus actual

**Projection, restated before submitting** (from the measured per-page figure, corrected by
the metered pilot):

| Method | Projected |
|---|---:|
| Complete v2 run, $53.58 over 2,032 pages = $0.02637/page, × 2,024 v4 pages | $53.37 |
| A — each document's v2 output × the pilot's measured ×1.316, × the v2 patch multiplier | **$69.48** |
| B — complete v2 cost × page ratio × the pilot's measured ×1.325 | **$70.73** |

Under $75, so the run was submitted.

**Actual:**

| Item | Requests | Result | Cost |
|---|---:|---|---:|
| Extraction batch | 257 | 257 succeeded, 0 errored, 173.8 min | $28.27 |
| Sweeper batches | 657 | 657 succeeded, 0 errored, 21.4 min | $23.12 |
| Output-ceiling patch, 4 rounds | 141 | 60 chunks recovered | $13.33 |
| **Total** | | | **$64.72** |

**8% under the low projection.** The projection was high for a reason worth recording: the
pilot's ×1.32 ratio came from two documents that had *no* truncation waste in v2, whereas
v2's corpus-wide output burned 47 chunks at the full ceiling before its patch recovered
them. Scaling a clean pair across a corpus that was not clean over-predicted. v4's
extraction output is 5.2M tokens against v2's 8.15M — lower in absolute terms.

**Running total across all sessions: $161.51.** September's spend is $65.56 (the $0.84
metering pilot plus this $64.72) against the $100/month cap; the earlier $95.95 was August.

## 4. The corpus result

| | |
|---|---:|
| Documents | 17 — **13 readable, 4 unreadable, 0 failed** |
| Readable pages | 2,024 |
| Rows | **8,118** |
| Rows dropped as unlocatable (invention) | 196 |
| Rows dropped as mangled (second reader) | 172 |
| Unresolved occurrences | 1,050 |
| Cost | $64.72 |

The four unreadable documents are declared with their reasons and charged nothing: three
scanned documents as `image-only`, and the GSA document `47QMCA26Q0098` as **`garbled-text`**
— 7 of its 8 text pages come out as characters that are not the letters on the page.

## 5. Two numbers that must not be read the old way

**Row count is no longer a measure of recall.** The prompt now captures a list lead-in
together with its items as one row. Measured, v3 → v4 over the same 13 documents:

| | v3 | v4 |
|---|---:|---:|
| Rows | 9,629 | 8,118 (−15.7%) |
| **Quoted text captured** | 1,567,475 chars | **1,691,733 chars (+7.9%)** |
| Rows that are a bare lead-in ending in a colon | 366 | **46 (−87%)** |

**Fewer rows carrying more of the document.** Judge recall by the reviewer table below.

**Unresolved is not on the same basis as v3** (593 → 1,050). Three causes, none of them a
recall regression: the sweeper is now credited only with what it explicitly explained (the
stricter rule, which can only raise the count); the scan sees more (new Class D triggers,
running headers stripped, sentences rejoined across page breaks); and the coverage check
compares Class-A *rows* per page, so ten items captured as one row count once, raising that
page's shortfall by nine while more of the page is in the matrix. Both traps are stated in
`EVAL_REPORT_v4.md` at the top of the comparison.

## 6. The reviewer-finding table — all 69, none omitted

The reviewers quoted 69 missed sentences across five documents. Each was checked against v4
by `scripts/reviewer-check.mjs`, which requires the quote to appear inside a **single** row,
on word boundaries, on the page the reviewer cited (one page of slack for a sentence cut by
a page break). Every one is scored.

| Document | quoted misses | now captured | still missed |
|---|---:|---:|---:|
| `0020153254COHEN` | 14 | 12 | 2 |
| `1240LT26Q0172` | 17 | **17** | 0 |
| `1616-26` | 8 | 6 | 2 |
| `75N98026Q00962` | 6 | 5 | 1 |
| `80JSC026MEDEVAC5Q` | 24 | 17 | 7 |
| **All five** | **69** | **57** | **12** |

Row by row: `corpus/eval-v4/REVIEWER_FINDINGS_v4.txt`.

**All twelve still missed are present in the extracted page text and absent from every
row** — recall misses, not text the pipeline could not read. Five of them (items c–g of the
Medical Facility Support Plan, 80JSC026MEDEVAC5Q p.8) are one list missed while the
near-identical list beside it on the same page was captured in full. Two are the
cross-page NASA sentence and the COHEN trade-study parenthetical, both of which the
coverage check does flag as unresolved rather than hide.

**The defect the reviewers named most often is gone.** `1240LT26Q0172`, the CSI construction
specification, went from **110 rows carrying an invented `PWS` prefix to 0**, and all 17 of
its quoted misses are captured. `W912P825BA029`, also with no such section, went 216 → 0.

## 7. The output-ceiling patch, and why it was necessary

60 of 257 chunks (59 truncated at the ceiling, 1 unparseable) produced **no rows** on the
first pass — a bigger hole than v2's 47, because longer list rows push more responses into
the 32,000-token ceiling. Unpatched, the run held 6,685 rows and **8.9% less** quoted text
than v3; patched, 8,118 rows and **7.9% more**. Publishing the unpatched figures would have
presented a hole in the pipeline as a finding about the documents.

The chunks were pre-split and resubmitted over four rounds. **Two were never recovered, and
both are refusals** — the model declined pages 102 and 103 of `W912P825BA029`. They are
recorded as refusals, not truncations; the two need different answers and do not share a
counter.

## 8. Verification

- **86 tests, 86 passing.**
- **`scripts/prove-guards.mjs`: 38 guards, every one proven able to fail.**
- `npm run lint`: 0 errors. `npm run typecheck`: clean.
- Audit-kit v4 packets: 13 document folders, generated once from `eval-v4` with the same
  sample seed (20260829), and not altered afterwards. The v3 packets are left in place
  beside them. `47QMCA26Q0098` has no v4 folder because it is now declared unreadable —
  there is nothing to audit.

## 9. What surprised me

1. **The run came in 8% under the low projection**, for the reason in section 3. The
   metering pilot was still the right call — without it I would have projected $53 from a
   superseded prompt — but a two-document pilot cannot represent a corpus's failure modes,
   only its prompt.
2. **The list rule's real cost is truncation, not tokens.** I expected longer output; I did
   not expect 23% of chunks to hit the ceiling. That is the one change here with a
   structural cost, and it is why the patch was needed.
3. **A single timed-out poll destroyed a batch already paid for.** The patch crashed on a
   transient API timeout while waiting on a live batch. Batch requests are charged when
   submitted, so re-running would have paid twice. I fixed the polling to tolerate
   transient failures, added a resume path that collects an existing batch instead of
   creating one, and made it **abort** if the rebuilt request ids do not match what comes
   back rather than risk attributing rows to the wrong document. It verified all 120
   matched before proceeding. This would have cost far more had it happened three hours
   into the main run.
4. **The coverage number moved for definitional reasons.** I nearly reported 593 → 1,050 as
   a straight comparison. It is not one, and section 5 says so.
5. **My section-scheme detector is looser than it should be.** It matches "statement of
   work" anywhere in a line, so a body-text mention can enable a prefix. On all 13
   documents the outcome is correct — 1616-26 really does have a `Section C - Statement of
   Work` heading on p.36, and 70CDCR is literally a Performance Work Statement — but the
   mechanism is weaker than the result suggests. Reported rather than tightened, because
   tightening it would need another run to evaluate.
6. **Two of my own guards had gone stale.** Editing the header-stripping code and the
   permission wording moved the text those guards plant into, so they reported ANCHOR
   MISSING — proving nothing while looking green. Both anchors were repaired and re-proven.
   A guard that silently stops guarding is worse than no guard.
7. **I broke my own test run by scheduling.** Running the guard harness concurrently with
   the test suite made vitest observe a planted defect and report a failure that did not
   exist. Diagnosed rather than "fixed"; the harness restores every file it touches.

## 10. Next

The evidence is filed. **The verdict is the owner's**, against the six exit criteria in
`MASTER_PLAN.md`, using `corpus/EVAL_REPORT_v4.md` and the v4 audit kit. D-010 in
`DECISIONS.md` is still the empty verdict slot, and this session did not touch it.

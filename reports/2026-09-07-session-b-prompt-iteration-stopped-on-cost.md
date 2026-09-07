# 2026-09-07 — Session B: prompt iteration built and metered; the full re-run STOPPED on cost

**Builder model: Claude Opus 5 (`claude-opus-5`).** From this session onward every report
states which Claude model ran the Builder session.

**Extraction model: `claude-sonnet-5`**, confirmed from Doppler (`shallfinder`/`dev`). It
did not change.

**The full re-run was not submitted.** The measured projection is **$69–71**, above the
owner's **$60** stop line. Everything the run depends on is built, tested, and metered; the
run itself awaits the owner's ruling. **Spent this session: $0.84**, on the metering pilot
described in section 4.

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

## 2. The prompt iteration — what changed

The prompt that produced v2 and v3 is kept verbatim in the code repository at
`lib/extraction/prompt-snapshots/2026-08-29-v2-v3-prompt.ts.txt`, so the difference is
visible in the repository rather than only described here. Per the owner's ruling,
Session A's two edits and Session B's five are recorded as **one** iteration — the single
iteration Task 5 allows.

| # | From | Change |
|---|---|---|
| 1 | Session A | `requirements include` / `verification requirements include` named as Class D triggers |
| 2 | Session A | A wage determination is a rate schedule: captured once as an admin row, its footnotes not exploded into rows |
| 3 | Session B, ruling 1 | **Lists.** A lead-in and its enumerated items are ONE row. Never a bare lead-in ending in a colon; never items stripped of their lead-in; never one row per item. A very long list may stop after the items included, with the total stated in `normalized` — but the items must be present |
| 4 | Session B, ruling 2 | **Section labels.** Never invent a prefix; `PWS`/`SOW` only where such a heading exists; on a specification the section number as printed; an unresolvable clause number is blank, never `52.219-...`; each row labelled with the clause in force at its own position on the page |
| 5 | Session B, ruling 3 | **Subject of the verb.** Government, agency, acquisition official and third-party subjects excluded by asking who the sentence makes responsible — not by whether the word "Government" appears. Permissions (`may be required to`), clause title lines, and blank-form boilerplate with empty fill-ins are not rows |
| 6 | Session B, ruling 4 | **Tables.** A product-specification or line-item table produces one summarizing row naming the table, its page span, and its item count |
| 7 | Session B, ruling 5 | **Definitions.** A duty stated inside a definition is captured; definitional boilerplate is not |

### Two mechanisms, not one, behind ruling 2

A prompt instruction cannot guarantee a label. Two reviewers independently found roughly
110 rows on the construction specification `1240LT26Q0172` carrying an invented `PWS`
prefix — a CSI-format document with sections numbered 013300, 260010, 329200 and no
Performance Work Statement anywhere in it. So there is now:

- `lib/extraction/sections.ts` — reads each document once and tells the model, in the
  prompt, which prefixes exist **in that document** and what its headings look like. It
  discards contents-page entries: without that, `329200 SEEDING AND MULCHING` from the
  table of contents was in force over the electrical sections. Measured on the real
  document, the heading in force is now 013300 at p.20, 221114 at p.31, 260010 at pp.40–44,
  260050 at p.48 and 260400 at p.56 — matching the reviewers' own description page by page.
- `sanitizeSectionRef` — removes an unsupported prefix or a placeholder **after** the model
  answers, on every row from both passes. `PWS 3.17.E.1` becomes `3.17.E.1`; `PWS 329200
  3.2.E` becomes `329200 3.2.E` (the digits were right in every case the reviewers checked);
  `52.219-...` becomes blank, not `52.219`, because a FAR subpart is not a clause.

**The sweeper now receives the same section context the first pass receives** (the owner's
ruling). It sees a single page with no document around it, so it was the pass with the
least context and the same instruction to produce a label.

## 3. `EXTRACTION_PROMPT_SPEC.md` section 5, amended

Amended as authorized, and only there. The section now states the rule the code implements:
same-page duplicates by ≥90% similarity **or** containment of a fragment; cross-page
duplicates only where both pages lie in a chunk-overlap region. It records why, with the
measured evidence from W15P7T-26-R-A006. No other planting document was touched.

## 4. Cost: projection versus the $60 line

**The measured per-page figure.** The complete v2 evaluation (the $43.44 run plus the
$10.14 truncation patch) cost **$53.58 over 2,032 readable pages = $0.02637 per page**. The
v4 corpus is 13 readable documents, **2,024 pages, 257 chunks**, and its input text is
**1.9% smaller** than v2's (running headers stripped, phantom form values gone).

> Projected from the measured per-page figure alone: **2,024 × $0.02637 = $53.37.**

That figure was produced by a prompt that no longer exists, and the changes above all push
output length up. Rather than submit $53 of work on a superseded number, I metered a pilot:
the two smallest documents — `0020153254COHEN` and `80JSC026MEDEVAC5Q` — through the same
Batch API path, extraction and sweeper, with the new prompt.

| | v2 recorded | v4 pilot | ratio |
|---|---:|---:|---:|
| Pages | 36 | 36 | — |
| Output tokens | 118,531 | 155,977 | **×1.316** |
| Input tokens | 36,967 | 43,375 | ×1.173 |
| Cost | $0.6311 | **$0.8364** | **×1.325** |
| Rows | 172 | 151 | ×0.878 |

Neither document truncated in either run, so this ratio is a clean prompt effect and not
patch work in disguise. Output is 94% of the cost of this pipeline, so a 32% output increase
is very nearly a 32% cost increase.

**Projections, weighted properly:**

| Method | Result |
|---|---:|
| A — scale each document's recorded v2 output by ×1.316, then apply the measured v2 patch multiplier (1.233) | **$69.48** |
| B — complete v2 corpus cost × page ratio × ×1.325 | **$70.71** |
| C — pilot dollars-per-page × 2,024 pages | $47.02 |

**C is rejected and should not be quoted.** Those two documents cost $0.0175 per page in v2
against a corpus average of $0.0214 — 0.82× — so scaling them unweighted across a corpus of
denser documents understates it. A and B agree at **$69–71**.

**$69–71 exceeds $60, so I stopped and did not submit the run.** Cumulative spend was
$95.95 through 2026-08-30 against a $100/month cap; that was August. September spend is the
**$0.84** pilot. Running total across all sessions: **$96.79**.

## 5. What the pilot actually showed

Real v4 output on two of the five documents the outside reviewers audited.

| Signal | v2 | v4 pilot |
|---|---:|---:|
| Bare lead-ins ending in a colon — COHEN / NASA | 3 / 6 | **1 / 0** |
| Mean verbatim length, characters — COHEN / NASA | 175 / 158 | **213 / 237** |
| Longest verbatim, NASA | 583 | **1,956** |
| Rows — COHEN / NASA | 40 / 132 | 37 / **114** |
| Rows with a blank section label — COHEN / NASA | 3 / 0 | 0 / 7 |

Fewer rows carrying more content, which is what the list rule was for. **Row count is no
longer a proxy for recall**, and any comparison that treats it as one will read the
improvement backwards.

## 6. The reviewer-finding table — partial, and why

The reviewers quoted **69 missed sentences** across five documents. All 69 are recorded in
the code repository at `reviewer-findings/findings.json`, and
`scripts/reviewer-check.mjs` scores any evaluation against them. None is omitted.

Only the two piloted documents can be scored; the other three were not run.

| Document | Quoted misses | Now captured (Y) | Still missed (N) | Scored? |
|---|---:|---:|---:|---|
| 0020153254COHEN | 14 | **11** | 3 | yes (pilot) |
| 80JSC026MEDEVAC5Q | 24 | **13** | 11 | yes (pilot) |
| 1240LT26Q0172 | 17 | — | — | **not run** |
| 1616-26 | 8 | — | — | **not run** |
| 75N98026Q00962 | 6 | — | — | **not run** |
| **Total** | **69** | **24 of 38 scored** | 14 | 31 unscored |

The full row-by-row table, with each quote and the capturing row's page, is at
`corpus/eval-v4-pilot/REVIEWER_FINDINGS_pilot.txt`.

Worth the owner's attention:

- **The defect both reviewers named most often is fixed.** COHEN's ten-item
  applicable-documents list on p.4 — quoted as ten separate misses (#5–#14) — is now a
  single row containing all ten. NASA's p.8 plan-contents lists (#57–#69) are likewise
  captured, thirteen of them.
- **One "miss" the tool is now right to leave out.** Finding #2, "Offerors **may** propose
  an 'equal' product…", is a permission. The owner's ruling 3 says permissions are not
  rows; the reviewer called it Class C. These conflict, and the ruling wins in the code.
  **This one needs the owner's word**, because it is a policy disagreement, not a bug.
- **Finding #4 is half-fixed.** Session A's page-break repair put "no less than four
  alternatives" into the page text — it is there now, on p.11 — but no v4 row quotes it.
  Fixing the text was necessary and was not sufficient.
- **A matching artifact worth knowing about.** Eight of the 69 quotes first scored as
  "not locatable in the document", including #4. The cause was typographic: the page reads
  `multi- agency` and `(CF- 242)` where the reviewer wrote `multi-agency` and `(CF-242)`,
  and the page uses double quotes where a reviewer transcribed singles. The checker now
  absorbs hyphenation and quote glyphs, as the locate check already does for whitespace.
  Without that fix the tool would have been marked wrong eight times over nothing.

## 7. Verification

- **`npm run test`: 85 tests, 85 passing** (84 before this section's regression test).
- **`scripts/prove-guards.mjs`: 37 of 37 guards proven able to fail**, 10 of them new this
  session — one per owner ruling plus the section-label mechanisms.
- **`npm run lint`: 0 errors** (13 pre-existing warnings in older scripts).
- **`npm run typecheck`: clean.**

## 8. What surprised me, including a mistake of mine

1. **The list rule is expensive.** 32% more output for 12% fewer rows. That single change is
   the whole reason this run is over the line. It is the intended trade — a lead-in with its
   nine items costs more tokens than a lead-in alone — but the size of it was not obvious
   before it was measured.
2. **I overwrote the owner's protected `corpus/EVAL_REPORT.md`.** The pilot run's report
   writer defaulted its output file name, so a run configured for `eval-v4-pilot` wrote the
   v2 report. I restored it from git **byte-for-byte** (`git diff` is empty against the
   committed version, verified), and the file name is now a **required** field with no
   default, derived from the output folder, with a regression test. It was recoverable only
   because the file was committed; a protected file that lives only on disk would have been
   lost. Reported here rather than quietly fixed.
3. **Blank section labels went up on NASA, 0 to 7.** That looks like a regression and is the
   opposite: they are places the model previously produced a confident label it could not
   justify, and now declines to.
4. **The reviewers disagree with the owner's ruling in at least one place** (finding #2,
   permissions). The rulings were written from the reviewers' findings, so this was not
   expected; it needs a decision, not a code change.

## 9. Not delivered, and why

All four items below are gated on the run that the $60 line stopped:

- the full corpus re-run into `corpus/eval-v4/`;
- `corpus/EVAL_REPORT_v4.md` with its "what changed from v3" section;
- the reviewer-finding table for the three unscored documents (31 of 69 findings);
- the v4 audit-kit packets.

Everything they need is built and proven. The re-run is one command.

## 10. The owner's decision

The projection is $69–71 against a $60 line. Three ways forward, all the owner's call, and I
am not choosing among them:

- **Raise the line to about $75** and run as built.
- **Hold $60 and change the list rule** — for instance capping a very long list's verbatim
  harder. That is a further prompt change, and the one documented iteration is spent, so it
  needs an explicit ruling.
- **Hold $60 and run part of the corpus.** Recorded for completeness, and I recommend
  against it: it narrows the task to fit a budget, which the owner has previously ratified
  refusing, and it would leave the reviewer table permanently partial.

Session B stops here. The verdict is the owner's.

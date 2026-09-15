# 2026-09-14 — ShallFinder converted to the Unattended Loop

**Builder model: Claude Opus 5 (`claude-opus-5`).** **Money spent: $0** — no model call, no API
call, no paid service.

This session built no product feature. It built the loop around the product (Studio Kit v2 file
06): an inventory of what the repository can really run, a Definition of Done for the current
phase, a checking script proven able to fail, the Stop hook and the reviewer, and one dry run.
**The first unattended campaign has not started, and must not start until the owner approves the
Definition of Done.**

Public-repository hygiene: this report names no solicitation, quotes no document text, and carries
no machine path, hostname, secret or personal contact. Document-level detail stays in the private
repository.

---

## 1. Identity

All five identity checks — repository config, global config, last commit author, GitHub account,
remotes — returned the studio account, in both repositories.

## 2. Inventory — what runs today, measured

| Check | Result |
|---|---|
| Unit tests | **PASS** — 86 of 86 |
| Lint | **PASS** — 0 errors (13 warnings in older scripts) |
| Typecheck | **PASS** |
| Build | **PASS** |
| Planted-failure guards | **PASS** — 38 of 38 proven able to fail |
| CI | none configured |
| End-to-end, browser, screenshot, policy tests | none exist; the only UI is a placeholder page |

Of the other scripts: two diagnostics and the earlier before/after proof script run and exit 0. The
eight scripts of the 2026-08-29 critic pass exit 1 today, because they depend on a temporary scratch
folder that no longer exists — they were one-off audits, not repeatable checks. Scripts that call the
model, write the protected evaluation outputs, or rewrite committed fixtures were deliberately not run.

## 3. Checks — machine-runnable and [NEEDS OWNER]

**Machine-runnable, enforced from now on — 10 standing checks:** lint, typecheck, tests, the
38-guard proof, build, protected evaluation outputs unchanged, Definition-of-Done integrity, proof
that the campaign checkers catch the known defects, handoff hygiene, and the loop's anchor records.

**Machine-runnable, enforced once the owner approves the Definition of Done — 9 campaign checks:** one
for each of the five integrity defects an outside review found and the saved v4 outputs confirm —
refused pages missing from coverage; a coverage object that contradicts itself; reviewer credit given
to the wrong obligation; three execution paths that apply different checks; per-document costs
printed as $0.00 — plus deterministic regeneration of corrected outputs at $0, a change report whose
every number must match the data, the independent-reader check over every final v4 row, and a guard
for each fix. Until approval they run and are reported as PENDING, but do not block.

**Loop checks — 2, machine-runnable:** the verifier's rubric, pinned by content hash to the exact work
it reviewed; the evidence file accounting for every check.

**[NEEDS OWNER] — not machine-runnable, 5:**

| # | Item | Why a script cannot decide it |
|---|---|---|
| O1 | The hand audit against the Phase 1 exit criteria | whether a requirement was missed is a human reading of the document |
| O2 | The D-010 verdict | the pass/fail ruling on Phase 1 is the owner's alone |
| O3 | Remove, or only count, rows the independent reader cannot confirm | the two choices give different row counts, and the plan does not say which |
| O4 | Confirmation that nothing was spent | only the billing console proves $0 |
| O5 | Whether to note that the published v4 reviewer figure is overstated | the corpus repository is public, and the v4 report itself may not be edited |

**Two measurements taken while building the checkers ($0):**
- The independent-reader check over all **8,118** final v4 rows: **7,982 confirmed, 136 fail, 0
  unreadable.**
- The published v4 figure "57 of 69 reviewer-quoted misses captured" is **overstated by at least
  one**: two reviewer findings with identical wording, in two different lists, were both credited to
  the one list that is in the matrix.

## 4. The checking script, seen failing — three proofs

Each proof: plant one defect, run the full checking script, compare its failure list with a clean
baseline run, restore, and confirm the restore. Strictly one run at a time.

| # | Planted defect | Check tripped | Exit | Newly failing | Restored | Verdict |
|---|---|---|---:|---|---|---|
| 1 | A lint error — `prefer-const`, which this project treats as an error. Unused variables are only warnings here, so a weaker plant would have passed | lint | 2 | exactly lint | yes | **PROVEN** |
| 2 | A broken test | tests | 2 | exactly tests | yes | **PROVEN** |
| 3 | A removed file — the protected-output baseline | protected outputs | 2 | exactly protected outputs | yes, byte-identical | **PROVEN** |

The guard-proof script was also run on its own (38 of 38) and inside every run above. The four data
checkers each **fail** on the frozen, defective v4 outputs — a standing proof that they can fail,
which stays true after the fixes because those outputs never change. The verifier independently
re-ran proof 3 in its first round, proof 1 in its second, and proof 2 in its third — all three confirmed by a reviewer that did not write them.

## 5. The dry run

The tiny, already-planned item: the session handoff stated two different "next exact task"
sections, one still pointing at a superseded report, and listed a task as blocked that was
completed on 2026-08-29.

| Step | What happened | Checking script |
|---|---|---|
| Iteration 1 | Loop started on the unfixed handoff | **exit 2** — blocked on the handoff check, the anchors, the rubric, the evidence |
| Fix | Stale section and stale roadblock removed — 11 lines deleted, nothing added | — |
| Iteration 2 | Re-ran | **exit 2** — handoff check now **passes**; anchors, rubric, evidence still block |
| Verifier, round 1 | Blind review of steps 0–6 | **VERDICT: FAIL** — the anchors check, because step 7 had not been done yet |
| Step 7 | Anchors written; two further review findings acted on (below) | — |
| Iteration 3 | Re-ran, pre-review | **exit 0** — every enforced check green |
| Verifier, round 2 | Blind review of steps 0–7 | **VERDICT: PASS** — with one gap it advised fixing before approval (below) |
| Fix | The gap closed; the change invalidated round 2's pinned rubric, by design | — |
| Iteration 4 | Re-ran, pre-review | **exit 0** |
| Verifier, round 3 | Blind review of the fixed version | **VERDICT: PASS** — with one gap left for the owner (below) |
| Final run | Full checking script, rubric included | **exit 0** — every enforced check green, rubric current (89 s) |

**Round 2's gap.** The Definition of Done said the test suite "includes tests for V1–V5", but the test
command passes just as well without them, and nothing checked. The guard-presence check now also
requires that the test each fix's guard names really exists; the guard proof already shows that test
goes red when the fix is undone. The new half was seen failing in a throwaway copy before it was
trusted. Because the work changed, round 2's rubric — pinned to the exact content it reviewed — no
longer matched it, so a third round reviewed the fixed version. That is the pin doing its job.

**Round 3's gap, left for the owner.** The check for the three execution paths confirms that its test exists, names every path and stage, and passes — but not, by itself, that the test really asserts identical results on the four planted inputs the Definition of Done describes; that part is judged by the reviewer reading the test. It cannot become a script check until the test exists, so rather than a fourth round it is decision 7 below. Round 3 also disclosed a lapse of its own: told not to read the earlier review narrative before forming its verdict, it did — and then re-derived every figure it relied on from its own runs.

Round 1 also found two real faults, both fixed before round 2:
- **The Definition of Done claimed a check no script performed.** It said the final commit must begin
  `DONE:`, but the evidence check never reads commit messages — and cannot, because the checking
  script runs before that commit exists. It is now a separate check, confirmed afterwards from the
  git log.
- **The template's deny-list blocks the studio's own identity check** (details in §6). Exact
  read-only identity commands were added to this project's allow-list.

**The door, honestly.** The Stop hook was installed but did **not** fire in the installing session;
Claude Code evidently reads hook settings when a session starts. The permission lists, by contrast, took effect at once: the deny-list refused a cleanup command of mine that used `rm -rf`, and the work was redone without it. The exact installed hook command,
run by hand, exited 2 and handed back the failure list without reaching the report check — the
intended order. Seeing it block a live session needs a fresh session (owner decision 6).

**A slip of mine, recorded.** One commit was meant to carry regenerated evidence; the generator
crashed on a percent sign in its own text, the command checked only that the older file was complete,
and the commit went ahead with the stale file. Nothing it committed was wrong; the evidence was
regenerated and committed immediately after, with the slip written into it.

## 6. What the conversion found about the kit itself

These affect every studio project, not just this one.

1. **Checks for unapproved work would trap the converting session.** A Definition of Done's own
   checks fail until its campaign runs, and the campaign cannot start before approval. Enforced
   immediately, the Stop hook would force unapproved work. Here they run and are reported, but are
   enforced only once an approval file exists; editing or deleting an approval fails a standing check.
2. **The verifier template cannot pass as written.** It runs the checking script and stops if it is
   not green — but the checking script requires the passing rubric the verifier has not yet written.
   Fixed here with a pre-review mode.
3. **Timestamps cannot show a stale review.** The guard harness rewrites every file it restores, so
   the whole tree looks newer than any review after every run. The rubric is pinned to a content hash.
4. **Listing two Stop hooks does not order them.** Claude Code runs matching hooks in parallel, so the
   report check could not be "after" the checking script by listing order. It is one chained command.
5. **The conversion order guarantees a failed first review.** The prompt calls the verifier (step 6)
   before the anchors are written (step 7), while the anchors are themselves a check.
6. **The template deny-list has a gap and an overreach.** `git config user.*` does not match a
   `git config --global user.name` *write*; and `git config user.*` and `gh auth *` block the read-only
   commands the studio's mandatory identity check uses. The lists were kept exactly as the template
   requires; read-only forms were added to the allow-list instead.
7. **The template files were not in the repository root** as the conversion prompt says; they were in
   the owner's kit folder and were copied in.
8. **An agent file created mid-session is not available to that session**, so the verifier ran as a
   general-purpose subagent given the verifier file's exact instructions. It could technically have
   written other files; `git status` afterwards showed it wrote only its two.
9. **A blind reviewer can still read the narrative.** The evidence file records earlier review rounds, and the
   round-3 reviewer read that narrative despite being told not to. Point future reviewers at the checks, and keep
   verdict history out of the file they audit.

## 7. Owner decisions needed before the first unattended campaign can begin

**For this project:**
1. **Approve `DEFINITION_OF_DONE.md`** — or return it with changes. Nothing starts until you do.
2. **O3:** in the corrected outputs, should the 136 rows the independent reader cannot confirm be
   *removed*, or only *counted*? This changes the regenerated numbers, so it is needed before the
   campaign reaches that check.
3. **O5:** should the corrected report say plainly that the published "57 of 69" was overstated?
4. **O4:** who confirms from the billing console that the campaign spent $0, and when?
5. **The approval mechanism:** you say "approved" in the planning chat; the Builder then records it
   with the approval script. It is a convention, not a lock — confirm that is acceptable.
6. **See the door work once:** start one fresh session and confirm the Stop hook blocks it, since it
   did not load in the session that installed it.
7. **How strictly V4 is enforced before you approve:** accept that the three-path parity check is partly
   judged by the reviewer until its test exists, or require a stricter script check to be written first
   (round 3's finding).

**For the studio kit — not blocking this campaign:**
8. Amend the template deny-list (the identity-command gap and overreach, §6.6), or keep the
   per-project allow-list fix.
9. Adopt the pre-review fix in the verifier template (§6.2) and the chained Stop hook (§6.4).
10. Reorder the conversion prompt so the anchors come before the first review (§6.5), or accept that
   round 1 fails.

**Unchanged, after the campaign:** O1, the hand audit, and O2, the D-010 ruling, remain the gate at
the end of the campaign. Phase 2 does not begin without them.

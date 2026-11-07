# 2026-11-07: Phase 1 repair round, audit rounds 2 and 3 — not passed yet

The owner's Phase 1 audit (round 1, 2026-11-06, on the corrected results `eval-v4-corrected/`)
met 3 of the 6 pass marks. On 2026-11-07 the owner recorded Phase 1 as **not passed yet** and
approved a repair round followed by a repeated audit (`shallfinder` DECISIONS.md, D-010 round 1).

## What was repaired

All repairs are code that runs without a model. The planned paid extraction rerun did not run:
the cloud session that did this work has no access to the AI key, which is kept on the studio's
own computer. **Model spend for this whole round: $0.00.**

The corrected results were rebuilt from `eval-v4-corrected/` into **`eval-v5-repaired/`**
(`scripts/regenerate-v5-repaired.mjs` in `shallfinder`). Nothing in `eval/`, `eval-v3/`,
`eval-v4/` or `eval-v4-corrected/` changed.

| | v4c | v5r |
|---|---:|---:|
| Rows | 8,118 | 8,798 |
| Rows added: rest of a list cut off by a page break | — | 47 |
| Rows added: binding sentence the coverage check found in no row | — | 633 |
| Section labels corrected against the document's own headings | — | 2,848 |
| Sentences on the coverage check's "not captured" list | 1,402 | 92 |

- **Coverage check.** About half of the 1,402 listed sentences were already held by a row: a
  heading line, list bullets, half of an abbreviation ("the U." of "U. S."), a page footer, or a
  second sentence the PDF ran together stuck to them, so no row could match. Sentence splitting
  and matching were corrected. A row must still hold every word of a sentence; only heading or
  numbering "glue" in front of it, or repeated page furniture, may be missing.
- **Section labels.** Labels are checked against the headings printed in the document: a
  construction-specification section ("SOW 1.5.1" → "01 33 00 1.5.1"), an attachment
  ("SOW 12" in Attachment 3 → "Attachment 3 §12"), the document's one work-statement name
  ("SOW 44.3" in a Performance Work Statement → "PWS 44.3"), a Uniform Contract Format section,
  and a clause number in front of a bare paragraph label ("(e)" → "252.227-7993 (e)"). A label
  that names nothing ("SOW (f)") is left blank.
- **Added rows.** Each quotes the page word for word, passes both independent text readers
  (the same checks as every model row), is marked confidence `review` and carries an `origin`
  field (`list-continuation` or `coverage-backstop`). Government duties are not added.

## Audit rounds 2 and 3

Each round drew new random sample rows and new full-read pages (round 2 seeds 20261107 / 20261108,
round 3 seeds 20261109 / 20261110) and used new independent AI readers, so no repair is judged on
the rows and pages that found its problem. Round 2 found more to fix; the fixes were made and
round 3 was run on a rebuilt v5r. The findings of every round are kept in `shallfinder`
(`content/audit-checks-round1`, `-round2`, and `content/audit-checks` for round 3).

| Pass mark | Round 1 (v4c) | Round 3 (v5r) | Needed |
|---|---|---|---|
| Catches everything | 7 of 13 | 8 of 13 | 12 of 13 |
| Citations right | 10 of 13 | 11 of 13 | 13 of 13 |
| Coverage check honest | 0 of 13 | 6 of 13 | 13 of 13 |
| Nothing invented | met | met (8,798 of 8,798 quotes found; planted fakes rejected) | — |
| Unreadable documents declared | met | met (4) | — |
| Cost recorded | met | met | — |

**Result: not passed yet.** All 130 sampled quotes in round 3 are word for word and on the right
page. What remains:

- Requirements in no row on full-read pages, in 5 documents: the end of a sentence or a list cut
  off at a page break (2), a sentence printed several times on one page (1), "will be required
  to", which the coverage scan did not look for (1), and two softer instructions (1).
- Wrong section labels written by the model itself in 2 documents (a paragraph labelled (h)
  where the text is (i)(3); a clause number taken from a mention rather than the heading).
- In 7 documents, one flagged page still lists a sentence a row already holds (a false alarm),
  and on 2 pages called clean a requirement is in no row.

Further mechanical fixes for several of these were written after round 3. They are **not yet
audited** and are not part of `eval-v5-repaired/`.

## Next

One paid extraction run with a revised reader prompt (section labels with their clause; softer
instructions such as forms to send), together with the post-round-3 fixes, then audit round 4 on
new random rows and pages. The last full run cost $64.72; the monthly cap is $100. The run needs
the AI key, so it runs on the studio's computer, and it is metered on a small pilot first.

Running total of model spend across all sessions is unchanged by this round: **$161.51**.

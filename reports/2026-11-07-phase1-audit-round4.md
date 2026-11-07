# 2026-11-07: Phase 1 audit round 4, on the paid rerun — not passed yet

After audit round 3 the owner approved one paid extraction run with a revised reader prompt,
capped at $70 (`shallfinder` DECISIONS.md D-010 round 3). It ran on the studio's computer and
cost **$58.03** (`corpus/eval-v5/`; run record in `shallfinder` `artifacts/RERUN_V5_2026-11-07.md`).
Round 4 of the audit then checked those results at no AI cost.

| | v5r (round 3) | v5 (round 4) |
|---|---:|---:|
| Rows | 8,798 | 8,660 |
| Rows added: rest of a list cut off by a page break | 47 | 14 |
| Rows added: binding sentence the coverage check found in no row | 633 | 1,031 |
| Sentences on the coverage check's "not captured" list | 92 | 192 |
| Quotes found word for word in their document | all | all |

## How round 4 was run

New random sample rows and full-read pages (seeds 20261111 / 20261112) and new independent AI
readers, as in every round, so no change is judged on the rows and pages that found its
problem. The planted-row test ran on three documents not used before (1616-26,
75N98026Q00962, W911SG27BA002): all nine planted fake rows were rejected. The findings are kept
in `shallfinder` `content/audit-checks`; rounds 1 to 3 are kept unchanged beside them.

| Pass mark | Round 1 | Round 3 | Round 4 | Needed |
|---|---|---|---|---|
| Catches everything | 7 of 13 | 8 of 13 | 10 of 13 | 12 of 13 |
| Citations right | 10 of 13 | 11 of 13 | 6 of 13 | 13 of 13 |
| Coverage check honest | 0 of 13 | 6 of 13 | 10 of 13 | 13 of 13 |
| Nothing invented | met | met | met | — |
| Unreadable documents declared | met | met | met (4) | — |
| Cost recorded | met | met | met | — |

**Result: not passed yet.** 129 of the 130 sampled quotes are word for word and all 130 are on
the right page. The one that is not word for word has a gap where the PDF's own text layer is
missing words, so no check that reads the same text can see it.

## What remains

- **Section labels** (the main gap). 22 of the 130 sampled rows have a wrong section label, in
  7 documents. 10 of them are blank where the page prints a clear label, mostly a clause
  paragraph such as 52.240-91(d)(3); the rest name the wrong paragraph or the wrong numbered
  section.
- **Missed requirements** on full-read pages in 3 documents: a sentence split by a page footer,
  a notice duty worded "The Government must be notified", and bulleted product specifications
  and softer instructions.
- **Coverage check:** the two hard misses fell on pages the check called clean; two flagged
  pages list a sentence that rows already hold.

## Next

Fixes at no AI cost: find whether the blank labels come from the AI reader or from the label
checks, and fill them from the clause and paragraph printed above the row; keep page footers
from splitting sentences; treat "The Government must be notified" as a contractor duty. Then
audit round 5 on new random rows and pages. No further paid run without the owner's yes.

Running total of model spend across all sessions: **$219.54** ($161.51 before this run).

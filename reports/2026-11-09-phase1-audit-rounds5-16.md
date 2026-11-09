# Phase 1 audit rounds 5 to 16 on the paid rerun: not passed yet

*2026-11-09. No model calls after the $58.03 paid rerun (eval-v5, round 4).*

Each round rebuilt the paid rerun at no AI cost with the fixes made after the round before
(`corpus/eval-v5b` for round 5 through `corpus/eval-v5m` for round 16), then drew new random
sample rows and full-read pages and used fresh independent readers. The owner recorded rounds 4
to 15 as not passed yet (DECISIONS.md D-010, 2026-11-09).

| Round | Build | Catches everything (12 of 13 needed) | Citations right (13 needed) | Coverage honest (13 needed) | Docs passing all three |
|---|---|---:|---:|---:|---:|
| 5 | eval-v5b | 8 | 10 | 10 | 7 |
| 6 | eval-v5c | 9 | 10 | 10 | 7 |
| 7 | eval-v5d | 11 | 9 | 8 | 5 |
| 8 | eval-v5e | 12 | 10 | 10 | 8 |
| 9 | eval-v5f | 11 | 11 | 12 | 9 |
| 10 | eval-v5g | 9 | 11 | 13 | 7 |
| 11 | eval-v5h | 11 | 10 | 13 | 8 |
| 12 | eval-v5i | 11 | 10 | 12 | 7 |
| 13 | eval-v5j | 12 | 13 | 12 | 12 |
| 14 | eval-v5k | 11 | 12 | 12 | 10 |
| 15 | eval-v5l | 11 | 11 | 13 | 9 |
| 16 | eval-v5m | 8 | 12 | 11 | 7 |

Figures are computed from each round's saved reader findings in the shallfinder repository
(`content/audit-checks-roundN/findings/`).

Nothing invented, unreadable documents declared and cost recorded were met in every round.

**From round 16:** pages where printed lines are missing from the PDF's text are listed in each
result's `coverage.text_layer_gaps` and declared "read this page by hand" (D-017). In this corpus:
75N98026Q00962 9 pages, W912P726RA022 41, 19C02026Q0027 2, W912P825BA029 2.

**What is left:** softer requirements written without a binding word, which the AI reader missed
and the no-cost check cannot see, and readers' judgement calls on borderline sentences. Which
documents miss changes with each new sample. No further rounds or paid runs without the owner's go.

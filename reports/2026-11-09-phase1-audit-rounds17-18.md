# Phase 1 audit rounds 17 and 18 on the paid rerun: round 18 meets all six marks

*2026-11-09. No model calls after the $58.03 paid rerun (eval-v5, round 4).*

Both rounds rebuilt the paid rerun at no AI cost with the fixes made after the round before, then
drew new random sample rows and full-read pages and used fresh independent readers, as in rounds 5
to 16 (`reports/2026-11-09-phase1-audit-rounds5-16.md`).

| Round | Build | Catches everything (12 of 13 needed) | Citations right (13 needed) | Coverage honest (13 needed) | Docs passing all three |
|---|---|---:|---:|---:|---:|
| 17 | eval-v5n | 11 | 13 | 12 | 11 |
| 18 | eval-v5o | 13 | 13 | 13 | 13 |

Figures are computed from each round's saved reader findings in the shallfinder repository
(`content/audit-checks-round17/findings/` and `content/audit-checks/findings/`). Nothing
invented, unreadable documents declared and cost recorded were met in both rounds.

**Fixes before round 17:** whose duty a sentence states (a Government cost limit or a Government
adjective no longer sets a contractor duty aside), and "may not" read as a prohibition.

**Fixes before round 18:** "will be responsible for" binds; a plain command on a specification
page ("Provide pull boxes ...", "Where ..., provide ...") that no row holds gets a row marked for
review.

**Round 18 close calls, all judged in the tool's favour by the readers:** W912P726RA022 met the
section mark at 9 of 10 with two borderline labels; W15P7T-26-R-A006 has two labels without the
provision number; 75N98026Q00962 has one quote on a page declared "read by hand" (D-017).

The verdict (D-010) is the owner's alone. The builder recommends recording Phase 1 as passed.

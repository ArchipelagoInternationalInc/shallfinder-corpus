# 2026-11-05: the 4 held form rows are decided

On 2026-11-02 the v4c correction held 4 rows for the owner (see
`2026-11-02-v4-corrected-evidence.md`). All 4 come from page 1 of a fill-in government form
(SF 1442) in 19C02026Q0027 and W911SG27BA002, where the form's printed words and its filled-in
answers are mixed together in the PDF text.

The owner read each one on the private preview, beside a picture of the form line, and chose
**Correct** for all 4. v4c is rebuilt from the frozen v4 results and the updated ledger
(`shallfinder` repository, `artifacts/row-adjudication/ledger.json`, DECISIONS.md D-012):

- Adjudication: 54 kept, 82 corrected, 0 removed, 0 unresolved (was 78 corrected, 4 unresolved).
- Rows: 8,118 (was 8,114). Each of the 4 rows' summary now states what the printed form shows
  (start and finish days, bonds required within 15 days after award, the original and 1 copy,
  the required offer guarantee). Their confidence is `review`, so a reader checks the form.
- Quotes stay source text. The 19C02026Q0027 quote is the layout reader's text of the form line.
  The three W911SG27BA002 quotes are unchanged: the product's own reader has them word for word
  and does not see the form's filled-in numbers or tick marks.
- Every corrected row was re-located against its cited page before anything was written.
  Cost of this change: $0.00. `eval-v4/` and the earlier reports are unchanged.

# EVAL REPORT v4c — deterministic saved-data correction

Builder model: **Claude (Anthropic), run through Claude Code in a cloud session**

This report was regenerated, with no network or model access, from the frozen v4 JSON, its saved recovery records, and the completed 136-entry adjudication ledger. No extraction API or model was called.
The original `eval-v4/` directory and `EVAL_REPORT_v4.md` remain the protected historical record.

**Total cost:** **$64.72** (recorded historical extraction costs; this correction added $0.00).
Independent reader: **8,118 rows checked; 136 failed** the second-source match in frozen v4 and entered adjudication.
Adjudication: 54 kept as source-valid, 82 corrected from source evidence, 0 removed as unsupported, 0 explicitly unresolved.

The published v4 reviewer count overstated capture by crediting findings 57 (SOW 2.3.3.h) and 65 (SOW 2.3.5.h) to the same SOW 2.3.5 row. v4c attributes each finding to its own parent obligation.
Reviewer findings captured: **56 of 69**

| Document | Rows v4 | Rows v4c | Unresolved v4 | Unresolved v4c | Item occurrences v4c | Not-processed pages v4 | Not-processed pages v4c | Cost (recorded) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-Work-RFI` | 39 | 39 | 3 | 12 | 12 | 0 | 0 | $0.13 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260624` | 519 | 519 | 14 | 6 | 6 | 0 | 0 | $1.71 |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-II-MCPV-Trailer` | 394 | 394 | 27 | 83 | 83 | 0 | 0 | $2.58 |
| `1616-26__RFP1620000348` | 342 | 342 | 34 | 46 | 46 | 0 | 0 | $1.94 |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 457 | 457 | 49 | 68 | 68 | 0 | 0 | $2.56 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHCS-Flooring-Materials-` | 154 | 154 | 87 | 72 | 72 | 0 | 0 | $1.56 |
| `70CDCR26R00000026__Attachment-01-Turnkey-Facility-PWS-Non-IHSC-Requirem` | 1414 | 1414 | 130 | 252 | 252 | 0 | 0 | $7.63 |
| `75N98026Q00962__RFQ-75N98026Q00962` | 120 | 120 | 33 | 33 | 33 | 0 | 0 | $1.55 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0003-MedEvac-Final` | 118 | 118 | 12 | 12 | 12 | 0 | 0 | $0.58 |
| `W15P7T-26-R-A006__Solicitation-Amendment-004-W15P7T26RA006` | 853 | 853 | 173 | 242 | 242 | 0 | 0 | $8.02 |
| `W911SG27BA002__Solicitation-Amendment-W911SG27BA002-0001` | 755 | 755 | 87 | 67 | 67 | 0 | 0 | $4.62 |
| `W912P726RA022__W912P726RA002-San-Rafael-Solicitation-Specs-FINAL-8-` | 1732 | 1732 | 196 | 311 | 311 | 0 | 0 | $10.13 |
| `W912P825BA029__Solicitation-W912P825BA029-OM25035` | 1221 | 1221 | 205 | 309 | 309 | 0 | 2 | $8.38 |

## Adjudication record

The full original row, disposition, evidence, rationale, and any corrected row are retained in `eval-v4-corrected/_adjudication.json`.
79 of the 82 corrected quotes are confirmed against the page text only by a check that may leave out page furniture; none of them passes the product's own stricter check. 77 run across a page header, footer, page number or watermark printed in the middle of the sentence, and 2 need no skip at all (the stricter check rejects them only for spacing before punctuation or a split across pages it does not join). Each one's audit entry lists the exact page text left out (`furniture_skipped`), so a reader can check that nothing but page furniture was dropped.

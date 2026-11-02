# 2026-11-02 — Corrected v4 evidence published (repair stretch finished)

**Builder model: Claude (Anthropic), run through Claude Code in a cloud session.** **Money spent:
$0** — no model call, no extraction call, no paid service. Every regeneration ran with no API key
present.

Public-repository hygiene: this report names no solicitation, quotes no document text, and carries
no machine path, hostname, secret or personal contact. Document-level detail stays in the private
repository and in the generated files themselves.

---

## 1. What was done

The owner approved a $0 repair stretch on 2026-09-18 and, on 2026-11-02, ruled that a cloud Builder
may finish it and publish the corrected results here. This session finished it:

- **All 136 rows** that failed the independent second-source check in the frozen v4 run were
  adjudicated against the source PDFs: **54 kept** as source-valid, **78 corrected** from source
  evidence, **0 removed**, **4 left unresolved** for the owner.
- `corpus/eval-v4-corrected/` and `corpus/EVAL_REPORT_v4c.md` were regenerated deterministically from
  the frozen v4 JSON and the adjudication ledger (byte-identical across two regenerations).
- The five evidence defects an outside review found in v4 (refused pages missing from coverage,
  self-contradicting coverage, reviewer credit to the wrong obligation, three execution paths
  applying different checks, per-document costs shown as $0.00) are fixed. Four of them have a check
  that catches the defect in v4 and passes on v4c; the fifth (the execution paths) is covered by a
  parity test that runs all three paths and passes.
- The original `eval/`, `eval-v3/`, `eval-v4/` and their reports are unchanged.

## 2. Checks, measured

| Check | Result |
|---|---|
| Full check run (pre-review) | **PASS** — 20 of 20 |
| Unit tests | **PASS** — 123 of 123 |
| Planted-failure guards | **PASS** — 52 of 52 proven able to fail |
| Independent reader over every frozen v4 row | 8,118 rows: 7,982 confirmed, 136 failed (the 136 adjudicated) |
| Recorded extraction cost (historical) | $64.72 for the 17-document v4 run; this correction added $0.00 |

## 3. How the corrections were confirmed

77 of the 78 corrections only match the page text once a running page header, footer, page number
or watermark printed in the middle of the sentence is left out; 1 needs no skip. They were confirmed
with an opt-in check that may leave out such page furniture. That check is **off in the product**:
the product's own verbatim check is exactly as strict as before. Every span it left out is recorded
per row in `eval-v4-corrected/_adjudication.json` (`furniture_skipped`), so anyone can check that
only page furniture was dropped.

## 4. Review

A fresh reviewer checked the work six times. Rounds 1 and 2 failed the page-furniture rule as too
loose for general use, which is why it is off in the product. Rounds 3 to 6 found the code and the
corrected data sound and failed only on wording in the Builder's own records, each corrected. Round 6
found the published files sound, with no local paths, keys or private details. The owner approved
publication after round 6.

## 5. Decided, blocked, next

- **Decided:** publish the corrected v4 evidence (owner, 2026-11-02).
- **Held for the owner:** the 4 unresolved rows. All come from page 1 of fill-in government forms,
  where the printed form and the filled-in answers are mixed in the PDF text. They are left out of
  the corrected matrix, neither kept nor removed, and will be shown to the owner on screen before any
  launch.
- **Held for the owner:** D-010, the Phase 1 verdict (the owner's hand audit of this evidence against
  the six pass marks). Not decided here.
- **Next:** the owner's D-010 audit; product work (upload, matrix view, export) starts only after it.

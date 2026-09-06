# 2026-09-06 — Audit kit built for the owner and outside reviewers

Administrative session. **No model was invoked and nothing was spent.** Nothing in either
repository was modified: every artefact was written to a new folder on the Desktop. The one
exception is this report, which the task asked for explicitly and which the session-report
mechanism requires.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

One distinct author across both repositories. Both working trees were clean before I began
and are clean now apart from this report.

## 2. Where the kit is

```
~/Desktop/ShallFinder Audit Kit/
```

**30 files, 79.8 MB.**

## 3. Verification by count

| Item | Found | Expected |
|---|---:|---:|
| Document subfolders | 14 | 14 |
| PDFs | 14 | 14 |
| Audit packets | 14 | 14 |
| Reviewer prompt | 1 | 1 |
| Instruction sheet (`READ_ME_FIRST.md`) | 1 | 1 |
| Contents list (`KIT_CONTENTS.md`) | 1 | 1 |
| `results` folder, empty | 1 (0 items) | 1 (0 items) |

Every subfolder holds exactly two files. The three scanned documents correctly have **no**
folder: `36C26026Q0939`, `PANMCC26P0000048766`, `W31P4Q26RA002`.

Subfolders are named by the solicitation number — the part of the filename before the double
underscore.

## 4. Verification of packet contents

I opened two packets and checked Part A against the v2 report's ten-row samples.

**`1616-26`** — all ten quotes match the v2 report exactly. No page changes, no mismatches.

**`W911SG27BA002`** — nine of ten match. Row 2 is marked as no longer present, and the v2
quote it replaced was:

> "The will notify the Contracting____ [insert name of SBA's contractor] MICC Fort Bliss Officer…"

That is one of the eleven order-mangled quotes the 2026-08-29 critic pass identified — two
table columns spliced together. The packet says the row was removed because no independent
reader could match its quote, without commentary.

Across all fourteen packets: **139 of 140 sample rows carried through**, one removed and
declared. No page corrections landed inside any sample.

Part B renders as expected in both cases — the three pages with the most unresolved
occurrences and their sentences, and for `0020153254COHEN`, which has zero unresolved
occurrences, the plain "Part B is not applicable" line.

## 5. Per-document summary

| Document | Rows | Sample rows shown | Unresolved | Part B |
|---|---:|---:|---:|---|
| 0020153254COHEN | 40 | 10 | 0 | not applicable |
| 1240LT26Q0172 | 621 | 10 | 3 | yes |
| 15F06726R0000194 | 427 | 10 | 29 | yes |
| 1616-26 | 385 | 10 | 20 | yes |
| 19C02026Q0027 | 587 | 10 | 16 | yes |
| 36C26126Q1034 | 255 | 10 | 24 | yes |
| 47QMCA26Q0098 | 19 | 10 | 4 | yes |
| 70CDCR26R00000026 | 1,674 | 10 | 52 | yes |
| 75N98026Q00962 | 159 | 10 | 29 | yes |
| 80JSC026MEDEVAC5Q | 132 | 10 | 5 | yes |
| W15P7T-26-R-A006 | 1,008 | 10 | 122 | yes |
| W911SG27BA002 | 841 | **9** (1 removed) | 63 | yes |
| W912P726RA022 | 2,149 | 10 | 154 | yes |
| W912P825BA029 | 1,351 | 10 | 72 | yes |

## 6. What the instruction sheet tells the owner

Which two files to upload per document and that both are needed; that the prompt is pasted
as the first message before the files; that one or two documents per chat is a sensible
pace; that the three scanned documents have no packets because they were declared
unreadable; that `47QMCA26Q0098` has a garbled text layer and is expected to fail, and is in
the kit deliberately so an outside reviewer sees the worst case; and that finished reports go
into `results` named with the document number and the reviewer's name.

## 7. What surprised me

- **Only one upload is actually large.** The kit is 79.8 MB, which sounded like trouble, but
  it is concentrated in one file: `W912P726RA022` at 31.5 MB, giving a 32.4 MB pair. The next
  largest pair is 15.3 MB and the remaining twelve are all under 8 MB. The size worry is one
  document's problem, not the kit's.
- **The packets themselves are small.** I expected the 2,149-row Part C table for
  `W912P726RA022` to be an upload problem in its own right. It is 0.36 MB — the largest
  packet in the kit, and negligible next to any of the PDFs.
- **The kit surfaces a known defect on its own.** The single removed sample row is one of the
  spliced-column quotes the critic pass found. A reviewer looking at `W911SG27BA002` will see
  a numbered gap and the reason for it, with no need to be told the backstory.
- **My verification parser over-matched and I nearly believed it.** It reported "13 sample
  rows" in the v2 report because the flagged-pages table beneath each sample has the same
  numeric column shape. The ten real rows all matched correctly, so the conclusion held — but
  the count was wrong for a moment, and a looser check that had happened to match the wrong
  rows would have looked identical from the outside.

## 8. What comes next

Nothing is blocked and nothing was built. The kit is ready for the owner to run reviews at
whatever pace suits. Phase 1's verdict remains the owner's, and `DECISIONS.md` D-010 is still
empty.

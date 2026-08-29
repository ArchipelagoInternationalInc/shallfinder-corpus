# 2026-08-28 — Restart: identity repair, packet tidy, report guard, corpus

Session type: restart after the project sat idle since 2026-08-01. No product code was
written and Phase 1 was not begun.

---

## 1. Identity — a real mismatch was found and repaired

The newest of the two existing commits was authored the owner's personal name and personal email address (redacted here; this folder is public).
The git settings were already correct; the commits predated them, and fixing a setting does not
re-stamp commits already made. The older commit carried `Archipelago International, Inc.` — the
right company, spelled differently from the canonical identity, which is the variant that trips
automated checks later.

Both commits were re-signed and force-pushed with the owner's prior approval.

### The five checks, final values

| # | Check | Value |
|---|---|---|
| 1 | Repo-level `user.name` / `user.email` | not set; inherits global |
| 2 | Global `user.name` / `user.email` | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Last commit author | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh auth status` | logged in as `ArchipelagoInternationalInc`, active |
| 5 | `git remote -v` | `github.com/ArchipelagoInternationalInc/shallfinder` |

GitHub's own commit records were queried directly after the push. Searching every author,
committer, and linked-account field across the repository returns no match for either of the
non-studio identities that were there before. The same five checks were run again against the new `shallfinder-corpus` repository
before its first save; all five passed.

**Residual, reported rather than hidden:** the superseded commit object is still fetchable on
GitHub by its exact old SHA (`b54268f`). It is unreferenced, appears in no listing, and the
repository is private. It ages out on its own. The owner ruled to leave it.

### Commit IDs

| Repo | Commit | What |
|---|---|---|
| shallfinder | `adf4da5` | Initial commit — re-signed (was `9d67011`) |
| shallfinder | `dba8a66` | Build Packet — re-signed (was `b54268f`, the mis-stamped one) |
| shallfinder | `02ca14a` | Packet moved to root; studio kit added; housekeeping |
| shallfinder | `e596aff` | Session-report guard (Stop hook) |
| shallfinder | `eaac01e` | ShallSheet→ShallFinder rename; D-008 settled; owner rulings |
| shallfinder-corpus | `60e1aa0` | Reports notebook and corpus rules |
| shallfinder-corpus | `bb512ff` | The 17-document evaluation corpus |

Every commit in both repositories is authored by the studio identity. Verified by listing all
distinct authors: exactly one.

---

## 2. File moves and repairs

All sixteen planning documents moved from `rfp-matrix-build-packet/` to the repo root, where
the documents themselves expect to live. The subfolder is gone.

The five studio kit files (01–05) were fetched from the `project-starter` repository under the
studio account and saved to `studio-kit/`. They did not need to be pasted in by hand.

Three defects found while reading, repaired in the same commit:

- `00Scott-------00_START_HERE.md` — the filename was mangled; other documents linked to it as
  `00_START_HERE.md`. Renamed.
- That file's contents list named 13 of the 15 packet documents, silently omitting
  `DESIGN_DIRECTION.md`. Added.
- `SESSION_HANDOFF.md` still read "Nothing is built. The repo does not exist yet" — true on
  2026-08-01, false now. Rewritten.

Also corrected: stale `rfp-matrix-build-packet/` paths in `SHALLFINDER_SUMMARY.md`; added a
`.gitignore` for `.DS_Store` and env files.

---

## 3. The rename

31 occurrences of "ShallSheet" replaced with "ShallFinder" across the planning documents.

Three mentions were deliberately kept, because they record what the old placeholder *was*
rather than using it as the name: `DECISIONS.md` D-008, the header note in `PROJECT_BRIEF.md`,
and the history line in `SHALLFINDER_SUMMARY.md`. A first blanket pass overwrote these three
into nonsense ("ShallFinder was the placeholder", "the ShallFinder→ShallFinder rename"); the
error was caught on verification and repaired before commit.

**D-008 settled** per owner ruling: the product is ShallFinder at shallfinder.com. The
`MASTER_PLAN.md` open-decisions row was marked settled to match. The two documents had
disagreed — `SHALLFINDER_SUMMARY.md` already recorded the domain purchase while D-008 still
called the name open.

---

## 4. Blocked: the Phase 1 exit criteria

The owner's ruling instructed that six decided pass-mark bullets be written into
`MASTER_PLAN.md` word for word. **The bullets did not survive the paste** — the instruction
arrived carrying the literal placeholder text `[paste the six bullets from decision 2 above]`,
and no such six bullets exist anywhere in the session.

No criteria were invented to fill the gap. The same ruling stated the pass marks are the
owner's and are already decided, so substituting a Builder's numbers would have been the exact
failure the gate exists to prevent.

`MASTER_PLAN.md` Phase 1 now carries a marked placeholder that cannot be mistaken for criteria:
it states plainly that it is not the criteria, contains no numbers, and instructs any session
reaching Phase 1 judgement to stop and ask for the real text. `DECISIONS.md` lists the missing
wording as pending. D-010 remains the empty verdict slot.

**This is the top blocker. Phase 1 cannot be judged until those six bullets are pasted in.**

---

## 5. The reports mechanism, and proof that it fails

Studio kit 01 requires that a session cannot end quietly without its report. That is now a
mechanism rather than a reminder.

`.claude/hooks/require-session-report.sh` runs on the session-stop event and blocks the session
unless a report dated today exists in `shallfinder-corpus/reports/`, **is committed, and is
pushed to GitHub**. The push condition is the point: a report written but never pushed is not
filed, and that failure would otherwise pass silently — the report exists, the session ends,
and nobody outside the machine ever reads it.

### Proven to fail before being trusted

Studio kit 02: "A safety check only counts after we've planted a deliberate mistake and watched
the check catch it." Three deliberate mistakes were planted:

| Planted state | Result |
|---|---|
| No report present at all | **Blocked**, exit 2 — "No report dated 2026-08-28 was found" |
| Report written on disk, never committed | **Blocked**, exit 2 — "exists but has never been committed" |
| Report committed but not pushed | **Blocked**, exit 2 — "committed but NOT pushed to GitHub" |

The decoy was then removed and the guard confirmed to return to its blocking state. The pass
case is demonstrated by this report: the guard clears only once this file is committed *and*
pushed.

An override exists at `.claude/REPORT_WAIVED` — a file the owner must create deliberately. It
announces itself when used. It is a visible decision, not a silent bypass.

### A related defect caught while filing this report

`reports/` was created and described in the README, but git does not track empty directories,
so the folder was never actually in the repository — a fresh clone would not have had it. A
`.gitkeep` file now holds the folder open. The guard would have caught the missing report
regardless, but the notebook would have looked correct while being structurally incomplete.

---

## 6. The corpus — 17 documents

Downloaded directly from SAM.gov's public opportunity API. No click-by-click walkthrough was
needed.

**Method:** 2,000 active opportunities gathered across four notice types; attachments filtered
on SAM.gov's own `accessLevel`, `exportControlled`, and `explicitAccess` flags; 72 candidate
documents downloaded and measured; 17 selected for spread.

**Page counts and file sizes were measured from the downloaded files** with `pdfinfo` and the
filesystem — not read off the SAM.gov listing.

| # | Agency | Solicitation | Title | Type | Pages | Size | Why chosen |
|---|---|---|---|---|---:|---:|---|
| 1 | DoD | `W912P726RA022` | San Rafael Creek and ATF FY26 Maintenance Dr | Solicitation | 645 | 33.1 MB | Largest text solicitation in the set; tests whether the pipeline holds up over a very long docum... |
| 2 | DoD | `W15P7T-26-R-A006` | Marketplace for Acquisition of Professional  | Solicitation | 342 | 6.7 MB | Large multi-volume Army RFP; the 300-page band where hand-built matrices start taking days. |
| 3 | DoD | `W912P825BA029` | Mississippi River, New Orleans Harbor and Va | Solicitation | 246 | 15.7 MB | Large construction solicitation; heavy Section L/M structure with many binding statements. |
| 4 | DoD | `W911SG27BA002` | New Fort Bliss Paving IDIQ Contract | Solicitation | 157 | 6.9 MB | Mid-size DoD solicitation in ordinary Uniform Contract Format; the everyday case. |
| 5 | DHS | `70CDCR26R00000026` | Turn-key Detention Facilities | Combined Synopsis/Solicita | 152 | 1.3 MB | DHS performance work statement; requirements sit in a PWS rather than a Section C. |
| 6 | DoD | `W31P4Q26RA002` | SUPPLY CHAIN OPTIMIZATION SUPPORT (SCOS) Sol | Combined Synopsis/Solicita | 144 | 21.4 MB | SCANNED. 144 pages of image-only pages. Phase 1 must declare this unreadable rather than return ... |
| 7 | DOJ | `1616-26` | RFP FOR INSTALLATIONS SERVICES | Solicitation | 104 | 5.9 MB | DOJ RFP; civilian agency, conventional structure. |
| 8 | State | `19C02026Q0027` | COMPOUND CAFETERIA CANOPY MODERNIZATION | Solicitation | 81 | 2.4 MB | State Department construction solicitation; overseas work adds unusual clause sets. |
| 9 | DOJ | `15F06726R0000194` | Design-Build of a Tier II Mobile Command Pos | Solicitation | 75 | 0.5 MB | Additional civilian agency, for agency-format variety. |
| 10 | USDA | `1240LT26Q0172` | Council Bluff Primary Electric And Well Impr | Combined Synopsis/Solicita | 74 | 3.1 MB | Additional civilian agency, for agency-format variety. |
| 11 | VA | `36C26126Q1034` | VAPIHCS Flooring Materials Indefinite Delive | Solicitation | 73 | 0.9 MB | VA combined synopsis/solicitation; VA is a high-volume issuer and its own house format. |
| 12 | VA | `36C26026Q0939` | Underground Storage Tank Repair/Replace | Solicitation | 39 | 26.1 MB | SCANNED. A VA solicitation delivered as images at 26MB. Second unreadable case, from a different... |
| 13 | HHS | `75N98026Q00962` | dCODE Dextramer® | Combined Synopsis/Solicita | 39 | 3.0 MB | HHS RFQ; civilian services buy. |
| 14 | not stated | `PANMCC26P0000048766` | Staff Augmentation | Combined Synopsis/Solicita | 33 | 12.4 MB | SCANNED, borderline. Sits near the low-text threshold, so it tests where the scanned/readable li... |
| 15 | NASA | `80JSC026MEDEVAC5Q` | Emergency Medical Support and Evacuation (Me | Solicitation | 23 | 0.7 MB | NASA RFP. Another agency house style, and a compact well-structured document. |
| 16 | DHS | `0020153254COHEN` | source sought COTHEN Tech Refresh - RT-2200A | Sources Sought | 13 | 0.3 MB | Sources sought with a real statement of work. A different notice type carrying far fewer binding... |
| 17 | GSA | `47QMCA26Q0098` | ATF - Honda Pilot | Combined Synopsis/Solicita | 8 | 1.4 MB | GSA. Small document at the bottom of the size range; checks the pipeline does something sensible... |
**Totals: 17 documents · 8–645 pages · 2,248 pages · 142 MB · 10 agencies · 3 scanned.**

| Requirement | What the set has |
|---|---|
| At least 12 documents | 17 |
| Varied agencies (DoD, VA, GSA, civilian) | DoD, VA, GSA, DHS, DOJ, State, HHS, USDA, NASA, + 1 unnamed |
| Sizes 30 to 400+ pages | 8 to 645 pages |
| Varied types | Solicitation, Combined Synopsis/Solicitation, Sources Sought |
| At least 2 messy or scanned | 3 scanned, one of them 144 pages |

### Screening, and what the restriction check actually found

Nine documents tripped a text search for CUI, export-control, and classification terms. All
nine were inspected individually and **all nine were false positives**: each opens with a
standard solicitation form and carries no banner marking. The hits were contract clauses
telling the *contractor* what to protect — ordinary solicitation content, and useful test
material, since those clauses are themselves binding requirements. Every selected file was then
re-checked for a banner marking on its first two pages; none had one. Nothing was excluded on
restriction grounds because nothing needed to be.

Deliberately excluded: engineering drawings, plans, maps, photographs, wage determinations,
SF 30 amendment covers, Q&A logs, and a NASA presolicitation conference **slide deck** that the
filename filter mistook for a solicitation and that inspection caught.

One document (`PANMCC26P0000048766`, "Staff Augmentation") has an empty organization record in
SAM.gov. Its agency is recorded as *not stated* rather than guessed.

**The documents have not been opened or processed. Phase 1 has not begun.**

---

## 7. Decisions recorded this session

- Identity mismatch repaired; both historical commits re-signed; old orphan commit left alone by
  owner ruling.
- Corpus lives in the public `shallfinder-corpus` repository, not the private repo. `TASKS.md`
  Tasks 2 and 5 updated so the two documents no longer contradict each other.
- D-008 settled: ShallFinder / shallfinder.com.
- Phase 1 exit criteria remain unwritten and are the blocking item.

## 8. What comes next

1. **Owner pastes the six Phase 1 exit-criteria bullets** into `MASTER_PLAN.md`. Blocking.
2. Builder Task 1: repo foundation (Next.js scaffold, TypeScript strict, Tailwind + shadcn/ui,
   `/tokens`, lint/typecheck/test/build, `.env.example`). Nothing product-specific.
3. Tasks 2–5 build and validate the extraction pipeline against this corpus.

## 9. Readiness verdict

The groundwork for Phase 1 is complete: identity is clean, the documents are tidy and correctly
named, the reports mechanism is installed and proven to fail, and a 17-document corpus spanning
8 to 645 pages across ten agencies is downloaded and screened. Phase 1 building work — Task 1
onward — can begin immediately. Phase 1 cannot be *judged* until the owner's six exit-criteria
bullets are recorded, because an unwritten pass mark is not a pass mark.

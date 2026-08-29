# 2026-08-28 — Phase 1 criteria recorded; Task 1 scaffold; Task 2 ingest

Second session of the day. Steps A–C of the owner's instruction, in one session.
No AI service was called, no UI was built, nothing touching auth, billing, or public
copy was changed. Stopped before Task 3, which is the first task that spends money.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level `user.name` / `user.email` | not set; inherits global |
| 2 | Global `user.name` / `user.email` | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective signing identity, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

Both repositories were checked before anything was saved. Listing all distinct commit
authors in each returns exactly one identity, the studio's.

---

## 2. Step A — Phase 1 exit criteria recorded

**Criteria commit: `a6e5af1`** in `shallfinder`.

The owner's six pass marks are now in `MASTER_PLAN.md` verbatim, replacing the
placeholder that stood since earlier today. `DECISIONS.md` no longer lists them as
pending; D-010 remains the empty verdict slot.

Added as D-009a: a note that two of the six criteria test the *instruments* rather than
the extraction. "Nothing invented" requires planting a fake row and watching the
locate-check reject it. "Coverage check honest" requires confirming a page the tool
calls clean really is clean. A pipeline can satisfy the other four while its own
self-checks are broken, so those two must be exercised deliberately.

`CORPUS_INDEX.md` gained a **Notice ID** column alongside the solicitation number, and
now states that its characters-per-page figure is the output of `pdftotext -q <file> -`
(Poppler 26.08.0). That mattered more than expected — see §7.

The two identifiers disagree on one document: the SAM.gov notice records
`W912P726RA022`, the file inside is named `W912P726RA002`. One carries a typo. The
Notice ID is unambiguous and is the key to use.

---

## 3. Step B — Task 1, repo foundation

Next.js 16.3.3 App Router, React 19.2.8, TypeScript strict, Tailwind 4, shadcn/ui.
Folder layout per `ARCHITECTURE.md`, with a `.gitkeep` in each empty directory so the
structure survives a fresh clone — the lesson from this morning, when `reports/` turned
out not to exist in the repository at all.

`/tokens/tokens.ts` carries `DESIGN_DIRECTION.md`'s starting values as the single file
both surfaces import: near-black ink on off-white, one deep-green accent, warm amber for
review-flagged rows, small radii, tight spacing, tabular figures. No patriotic palette —
`DESIGN_DIRECTION` rules it out as gov-lookalike, which `CLAIMS_AND_COMPLIANCE` §1
prohibits. The placeholder page renders them; desktop and 390px both verified, zero
console errors.

`.env.example` lists 30 variables with no values, marking which are named literally in
the packet and which are derived from services the packet names.

One new dependency: **vitest**, the test runner Task 1 requires.

### All five commands, verbatim

```
> shallfinder@0.1.0 lint
> eslint
>>> EXIT: 0

> shallfinder@0.1.0 typecheck
> tsc --noEmit
>>> EXIT: 0

> shallfinder@0.1.0 test
> vitest run
 RUN  v4.1.11 <repo path redacted - public folder>
 Test Files  2 passed (2)
      Tests  9 passed (9)
>>> EXIT: 0

> shallfinder@0.1.0 build
> next build
▲ Next.js 16.3.3 (Turbopack)
✓ Compiled successfully in 1078ms
  Running TypeScript ...
  Finished TypeScript in 760ms ...
✓ Generating static pages using 5 workers (4/4) in 206ms
Route (app)
┌ ○ /
└ ○ /_not-found
○  (Static)  prerendered as static content
>>> EXIT: 0

> shallfinder@0.1.0 dev
> next dev
▲ Next.js 16.3.3 (Turbopack)
- Local:         http://localhost:3000
✓ Ready in 162ms
 GET / 200 in 646ms
```

`dev` was not merely started — the page was fetched over HTTP (200, 36,104 bytes) and
checked for the rendered token values.

---

## 4. Step C — Task 2, ingest module

`lib/extraction/ingest.ts` turns a PDF into per-page text carrying the `⟦p.N⟧` anchors
`EXTRACTION_PROMPT_SPEC` §4 requires, and returns a typed result — `ok`, `unreadable`,
or `failed` with a reason. Bad input never throws past the caller: encrypted files,
Word documents renamed to `.pdf`, empty files, and missing files all come back typed.

`lib/extraction/config.ts` holds both thresholds, read from the environment with
measured defaults. `scripts/extract.ts` runs one document; `scripts/corpus-eval.ts` is
the Task 5 stub that sweeps `CORPUS_DIR`.

### Every corpus document — measured

Times are wall-clock inside ingest on this machine. Characters per page are from this
pipeline's own extractor and are **not** the same figure as `CORPUS_INDEX.md`'s.

| Document | Pages | Chars/pg | Low-text pages | Classification | Seconds |
|---|---:|---:|---:|---|---:|
| `0020153254COHEN__Attachment-1-Statement-of-...` | 13 | 1,915 | 0/13 | OK | 0.21 |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI202606...` | 74 | 1,462 | 2/74 | OK | 0.16 |
| `15F06726R0000194__RFP-15F06726R0000194-Tier...` | 75 | 2,529 | 0/75 | OK | 0.13 |
| `1616-26__RFP1620000348.pdf` | 104 | 1,793 | 0/104 | OK | 0.95 |
| `19C02026Q0027__Solicitation-19C02026Q0027.pdf` | 81 | 2,219 | 0/81 | OK | 0.18 |
| `36C26026Q0939__SF-1449-36C26026Q0939-Storag...` | 39 | 0 | 39/39 | **UNREADABLE** | 0.03 |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAP...` | 73 | 2,187 | 1/73 | OK | 0.18 |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18.pdf` | 8 | 2,398 | 0/8 | OK | 0.05 |
| `70CDCR26R00000026__Attachment-01-Turnkey-Fa...` | 152 | 2,902 | 1/152 | OK | 0.41 |
| `75N98026Q00962__RFQ-75N98026Q00962.pdf` | 39 | 2,451 | 0/39 | OK | 0.17 |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC02...` | 23 | 2,455 | 0/23 | OK | 0.07 |
| `PANMCC26P0000048766__Combined-Synopsis.pdf` | 33 | 0 | 33/33 | **UNREADABLE** | 0.77 |
| `W15P7T-26-R-A006__Solicitation-Amendment-00...` | 342 | 1,950 | 1/342 | OK | 0.31 |
| `W31P4Q26RA002__W31P4Q-26-R-A002-Solicitatio...` | 144 | 21 | 143/144 | **UNREADABLE** | 1.01 |
| `W911SG27BA002__Solicitation-Amendment-W911S...` | 157 | 2,242 | 1/157 | OK | 0.17 |
| `W912P726RA022__W912P726RA002-San-Rafael-Sol...` | 645 | 2,181 | 56/645 | OK | 1.38 |
| `W912P825BA029__Solicitation-W912P825BA029-O...` | 246 | 2,271 | 22/246 | OK | 0.44 |
**14 readable · 3 unreadable · 0 failed.** The three unreadable documents are exactly
the three scanned ones. The 645-page document ingests fully in 1.38 seconds. Total
corpus: 2,248 pages in about 6.4 seconds.

### Page anchors — 16 pages spot-checked across 5 documents

Method: for each page, extract that page independently with `pdftotext -f N -l N`, take
its distinctive tokens (8+ character alphanumerics), and check they land on *our* page N
— then check the same tokens against pages N−1 and N+1 as an off-by-one probe.

| Document | Pages checked | Result |
|---|---|---|
| `W912P726RA022` (645 pp) | 1, 100, 400, 645 | 4/4 pass — 9/9, 40/40, 40/40, 2/2 tokens |
| `W15P7T-26-R-A006` (342 pp) | 1, 171, 342 | 3/3 pass — 40/40, 39/39, 33/33 tokens |
| `70CDCR26R00000026` (152 pp) | 1, 76, 152 | 3/3 pass — 6/6, 40/40, 40/40 tokens |
| `1616-26` (104 pp) | 1, 12, 104 | 3/3 pass — 39/40, 25/25, 40/40 tokens |
| `19C02026Q0027` (81 pp) | 1, 40, 81 | 3/3 pass — 40/40, 40/40, 6/6 tokens |

**16 of 16 pages passed.** Neighbouring-page hit counts were far below on-page counts
throughout (for example p.400: 40 tokens on our page 400, 22 on 399, 10 on 401 — the
overlap being ordinary repeated boilerplate, not misalignment). No off-by-one anywhere.

### The low-text threshold, and why

**`INGEST_LOW_TEXT_CHARS_PER_PAGE = 100`** and **`INGEST_UNREADABLE_PAGE_FRACTION = 0.8`.**

Both were chosen from the measured distribution, using this pipeline's own extractor:

| Population | Pages | Median chars | 5th pct | Share below 100 chars |
|---|---:|---:|---:|---:|
| Pages in the 14 readable documents | 2,032 | 2,334 | 129 | 4.1% |
| Pages in the 3 scanned documents | 216 | 0 | 0 | 99.5% |

The two populations barely overlap, so any per-page threshold between 20 and 500
classifies this corpus identically. 100 sits inside that band. What the value actually
controls is how many genuinely sparse pages in a *readable* document — section dividers,
signature pages, blank continuation sheets — get counted as low-text; at 100 that is
4.1%, far below the document-level fraction, so it changes no document's verdict.

For the document-level fraction: the three scanned documents scored 1.00, 1.00 and
0.993; the worst readable document scored 0.087 (56 low-text pages of 645). The real gap
is 0.087 to 0.993. **0.8 is deliberately placed near the top of that gap, because the two
errors are not symmetric.** Wrongly declaring a readable document unreadable refuses a
paying customer's real solicitation — and refuses it for free, per
`CLAIMS_AND_COMPLIANCE` §8. Wrongly accepting a scanned one is caught moments later by
the coverage check. Set high, the failure mode is the recoverable one.

Not tuned beyond this. Task 2 says no tuning marathon. If a future document lands
between these populations, that is a finding to report, not a number to quietly adjust.

---

## 5. Guards proven able to fail

Studio kit 02: a check only counts once a planted mistake has been caught.

| Planted defect | Result |
|---|---|
| `radius.lg` set to 24px, violating "Small. Nothing bubbly." | suite red, **exit 1** |
| `%PDF` magic-byte guard disabled | "rejects a file that is not a PDF" red, **exit 1** |
| Page anchor format changed from `⟦p.N⟧` to `[p.N]` | anchor-format test red, **exit 1** |

All three reverted; suite back to 9 passing, exit 0.

---

## 6. The finding that mattered

**The pipeline was declaring a real, readable, 104-page solicitation unreadable.**

The first corpus run returned **four** unreadable documents, not the three expected. The
extra one was a 104-page DOJ RFP that `pdftotext` had measured at 1,615 characters per
page and classified as ordinary text. Our ingest saw 48.

The instrument was at fault, not the document. `pdfjs`'s `getTextContent()` returns only
text drawn in the page **content stream**. This document's text lives in **AcroForm field
values**. On page 12: 23 characters in the stream, 950 recoverable from 86 populated form
fields.

This is not an exotic case. SF 1449, SF 33, and SF 18 — the standard federal
solicitation forms — are fillable PDFs, and on those documents the solicitation number,
the response deadline, the delivery dates, and the line items all sit in form fields.

Ingest now reads widget annotations alongside stream text, skipping values already
present in the stream so that a *flattened* form is not counted twice. The document went
from 48 to 1,793 characters per page and from `unreadable` to `ok`. A second document
improved too (1,824 → 2,451 chars/page, 6 low-text pages → 0).

Had this shipped, ShallFinder would have refused a large class of genuine federal
solicitations — politely, for free, and while appearing to behave correctly. It would
have looked like honest failure. It would have been a bug.

The catch came from the corpus disagreeing with an independent tool. That is the whole
argument for keeping a second extractor around as a cross-check, and for
`CORPUS_INDEX.md` now naming which tool produced its numbers.

---

## 7. Other things that surprised me

- **`next dev` edits `CLAUDE.md`.** Next 16 appends an agent-instructions block to
  agent instruction files on every run and re-adds it if deleted. `CLAUDE.md` is one
  of the three owner-governed anchor files; a build tool must not write to it. Set
  `agentRules: false` in `next.config.ts` and verified the file's checksum is now
  unchanged across a dev run.
- **`typecheck` silently depended on build order.** The generated layout used
  `LayoutProps<"/">`, a type Next writes into `.next/types` during a build, so
  `tsc --noEmit` failed on a clean checkout. Replaced with an explicit React type.
- **`.gitignore`'s `.env*` was swallowing `.env.example`.** The file would have been
  written, looked correct locally, and never reached the repository. Added a negation and
  confirmed with `git add --dry-run`.
- **One page inside a fully scanned document carries 3,050 characters.** A
  machine-generated sheet bound into an image-only file. Exactly why the unreadable test
  is a fraction of pages rather than "every page is blank".

---

## 8. Scope discipline

No AI service was called. No API key exists in this repo or its environment. No UI beyond
the token placeholder page. Auth, billing, and public copy untouched. Two dependencies
added, both required by the tasks as written: `vitest` (test runner) and `pdfjs-dist`
(PDF text extraction).

## 9. What comes next

**Task 3 is the gate.** It is the first task that calls a model and therefore the first
that spends money. It needs an owner-approved API key with a hard monthly spending cap
set at the provider, per `GO_TO_MARKET` step 2. Not started, per instruction.

Before Task 3 runs, one thing from §6 is worth carrying forward: the ingest fix means
form-field text now appears in a page's text without positional context. Task 3's
chunking should be checked against a form-heavy document early, since field values
arrive as a flat list rather than in reading order.

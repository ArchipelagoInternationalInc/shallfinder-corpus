# 2026-08-29 — Critic pass: an independent attempt to break the Phase 1 evidence

Critic session. Did not build the pipeline, did not fix anything, changed no code, config,
prompt or data belonging to it. **No model calls. No money spent.** Every check is
deterministic and re-runnable from `scripts/critic/`.

---

## 1. Identity — the five checks

| # | Check | Value |
|---|---|---|
| 1 | Repo-level | not set; inherits global |
| 2 | Global | `ArchipelagoInternationalInc` / `archipelagointernational@proton.me` |
| 3 | Effective, and last commit | `ArchipelagoInternationalInc <archipelagointernational@proton.me>` |
| 4 | `gh` account | `ArchipelagoInternationalInc`, active |
| 5 | Remote | `github.com/ArchipelagoInternationalInc/shallfinder` |

One distinct author across both repositories. Both working trees were clean before I
started, so nothing I found is an artefact of a half-finished state.

## 2. How independence was achieved

The pipeline reads PDFs with `pdfjs`. I read them with **`pdftotext`** (Poppler) and, where
that could not see the text, with **`pypdf`** — two different libraries. The binding-verb
scanner and the subject classifier were **re-written from `EXTRACTION_PROMPT_SPEC`**, not
imported. Where I did call pipeline code (`ingestPdf`, `scanDocument`, `chunkPages`) it was
to obtain *its* answer for comparison against mine, never as my source of truth.

## 3. Results, all eight checks

Full detail and every list: **`corpus/CRITIC_REPORT.md`**, with long lists under
`corpus/critic/`.

| Check | Result |
|---|---|
| 1. Locate, all 9,668 rows | 97.45% on the cited page. **Zero inventions.** 6 page errors, 11 order-mangled |
| 2. Coverage recount | 6,849 mine vs 6,842 theirs — **0.1% apart**, 3 pages disagree |
| 3. Exclusion audit | **3 false exclusions of 305 (1.0%)**, denominator effect 0.04% |
| 4. Unresolved | 704 reported, **333 enumerable**, of which **119 are real requirements** |
| 5. Duplicates | **Zero survived the merge**; published rate uses a stale denominator |
| 6. Section refs | **4.4%** unlocatable on the fair test (58.1% on an unfair one — see below) |
| 7. Unsplittable chunks | **Not auditable** — the patch never recorded which pages |
| 8. Reconciliation | Rows, inventions, unresolved all reconcile. One structural inconsistency |

## 4. Twice I had to distrust my own instrument first

Worth recording, because in both cases the alarming number was mine, not the pipeline's.

**241 rows my extractor could not find.** That reads like 241 inventions the pipeline's own
check missed — which would have been fatal to criterion 2. It was not. `pdftotext` cannot
see AcroForm field text at all; `pypdf` found 230 of them. The pipeline was right and my
tool was blind.

**58.1% of section references "wrong".** Also mine. The spec *asks* for a composed label
like "PWS 4.1"; requiring that exact string to appear in the document tests nothing. On the
fair test — does the identifier appear near the cited page — the figure is 4.4%.

Had I published either number without checking, I would have handed the owner two false
alarms on the two criteria that matter most.

## 5. The three findings I think matter most

### First — 11 quotes that are traceable in principle and useless in practice

Every word is in the document; the sentence is not. The pipeline's extractor spliced text
across table columns, and its own locate-check passed the result because it was checking
against that same spliced text:

> "The SF1442 shall beTAB B: Signed and completed SF 1442 along with any amendments submitted full"

A proposal manager checking that against the solicitation finds nothing. This is the only
place where the locate-check — the product's central integrity guarantee — is satisfiable by
something a customer would call wrong. It is 11 rows in 9,668, and it is a defect of kind
rather than of scale: **a self-check that validates against its own extraction can only ever
catch invention, never mangling.**

### Second — "704 unresolved" is a residual, not a list

It is shortfall minus a resolution credit. I can enumerate 333 specific occurrences; the
other 371 do not correspond to anything nameable. The product intends to show this number to
users as "pages to re-read". A number you cannot walk item by item is a weak thing to put in
front of someone who is trusting it — and the 119 genuine contractor requirements I *could*
name are far more useful than the 704.

### Third — the soft spot is firmer than its own warning label

The builder flagged false government-actor exclusions as the place where coverage could be
quietly flattering itself, and said the sweeper could not catch them. I went looking hard.
**Three, out of 305.** A 0.04% effect on the denominator. The per-occurrence attribution fix
did its job, and the caveat in `EVAL_REPORT.md` is now more pessimistic than the evidence
warrants.

## 6. What surprised me

- **The evidence mostly held.** I expected an independent extractor to disagree far more than
  0.1% on the coverage denominator, and I expected to find at least a handful of genuine
  inventions among 9,668 rows. There were none.
- **The two most alarming numbers I produced were both my own errors.** Being a critic makes
  you want to find something; that pressure is exactly what makes instrument-checking a
  discipline rather than a courtesy.
- **A config value that does nothing.** `governmentActorPatterns` is declared, documented, and
  read nowhere — the classifier uses hardcoded lists. Setting the environment variable has no
  effect and gives no indication why. Found by reading code, not data, which is a reminder
  that a data-only audit would have missed it.
- **The patch that fixed truncation did not record what it could not fix.** Three chunks
  truncated at a size that could not be split; their page numbers were never written down, so
  the one part of the evidence that is *known* incomplete is the part that cannot be examined.

## 7. What I could not check

- Whether the captured rows are the **right** rows. That is criterion 1 and needs the owner's
  two-page hand read.
- Whether each `normalized` line faithfully restates its `verbatim`. Comparing meaning
  **needs a model run**; none was made.
- Whether a section reference is the **correct** section rather than one that merely exists
  nearby.
- The two unsplittable chunks whose pages were never recorded.

## 8. Deliverables

- `corpus/CRITIC_REPORT.md` — the full report
- `corpus/critic/` — six markdown lists and the underlying JSON
- `scripts/critic/` — six scripts, re-runnable, deterministic, free

No verdict is offered. Phase 1 is the owner's ruling and `DECISIONS.md` D-010 is still empty.

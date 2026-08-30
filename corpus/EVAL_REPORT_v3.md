# EVAL_REPORT v3 — Phase 1 corpus evaluation

Produced 2026-08-29. **Supersedes v2 for the numbers listed below only.** v2 and the
`corpus/eval/` JSON are untouched — the owner is mid-audit on them.

Everything here is a **mechanical correction** arising from the critic pass in
`corpus/CRITIC_REPORT.md`. **No extraction was re-run. No model was called. Nothing was
spent.** Model, prompt, chunk sizes and thresholds are unchanged.

**This report presents evidence and reaches no verdict.** Phase 1 is the owner's ruling,
recorded as D-010, which is still empty.

## What changed from v2

| | v2 | v3 | change |
|---|---:|---:|---:|
| Rows | 9,668 | **9,648** | **−20** |
| Rows dropped as `mangled` | 0 | **20** | new check |
| Page citations corrected | — | **1** | |
| Page citations flagged as ambiguous | — | **4** | |
| Unresolved | 704 | **593** | itemized, see below |
| Owner-sample rows affected | — | **1 of 140** | |

### 1. A second-source locate check, and the 20 rows it dropped

The pipeline's own locate-check validates a quote against the pipeline's own extracted
text. That catches invention. It cannot catch **mangling** — when the extractor splices
text across table columns, the quote and the text it is checked against are wrong in the
same way, so the check passes.

A row must now survive a **second** check against readers that are not the pipeline's:
`pdftotext` (Poppler), falling back to `pypdf` for the form-field text `pdftotext` cannot
see. A row no independent reader can match is dropped with reason `mangled`.

**The matching rule, stated plainly.** Whitespace runs collapse to one space; the several
dash and quote characters PDFs use interchangeably fold to one form; soft hyphens and
non-breaking spaces are removed; lowercased. Two tiers are tried: exact substring, then
the same with hyphen-line-breaks joined. The quote is searched across the **whole
document**, not just the cited page — page accuracy is the other check's job, and this
check drops rows, so it must fire only when no independent reader can see the text at all.

**There is deliberately no alphanumeric-only tier, and that is the point.** Stripping
punctuation and spaces makes `"...shall beTAB B: Signed..."` compare equal to the same
words correctly spaced — it erases the very splice that defines mangling. Measured: all 11
rows the critic identified as mangled pass an alphanumeric comparison and fail these two.
A first attempt at this check included that tier and dropped **zero** rows.

**20 rows dropped**, which reconciles exactly with the critic's findings:
its 11 order-mangled rows, plus the 9 it could only match with the splice erased.

| Document | Rows dropped as mangled |
|---|---:|
| `W911SG27BA002__Solicitation-Amendment-W911SG27` | 17 |
| `W912P726RA022__W912P726RA002-San-Rafael-Solici` | 3 |

Full list: [`eval-v3/_v3.json`](eval-v3/_v3.json), key `mangled`.

### 2. The six page-citation errors

Each was relocated against the independent text. **Four were not errors.** The text is a
wage-determination clause that appears **identically on 15–17 pages** of the same
document; the critic reported the first page carrying it, which is not evidence the cited
page is wrong. Those rows keep their cited page and are flagged `confidence: review`,
because which occurrence the row refers to genuinely cannot be determined from the text.

| Cited page | Critic said | Outcome |
|---:|---:|---|
| 115 | — | ambiguous - identical text on 17 pages (113, 114, 116, 119, 120, 121, ...); left on cited page a |
| 117 | — | ambiguous - identical text on 17 pages (113, 114, 116, 119, 120, 121, ...); left on cited page a |
| 119 | — | ambiguous - identical text on 15 pages (114, 115, 116, 117, 120, 122, ...); left on cited page a |
| 125 | — | ambiguous - identical text on 17 pages (113, 114, 116, 119, 120, 121, ...); left on cited page a |
| 287 | — | corrected (unique match) |
| 342 | 334 | **Not resolvable by this method.** The quote matches no page under exact comparison; it was found only with the splice erased. Left unchanged rather than moved on weak evidence |

### 3. Unresolved is now an itemized list

**v2 reported 704. v3 reports 593.** The two are not the same measurement, and the
drop is not an improvement — it is a change of definition.

v2's 704 was an **arithmetic residual**: page shortfall minus a resolution credit,
summed. It could not be walked item by item, which the critic pass demonstrated.

v3's 593 is a **count of named occurrences**. Each one carries its page, the sentence
it sits in, and why it is unresolved. Every entry can be looked up and argued with.

They differ because a residual and a census count different things. The critic enumerated
333 using only the pages v2 listed as uncaptured; v3 enumerates every page carrying a
shortfall, which is a superset. **No number here was adjusted to look better** — the
itemized figure is simply what the enumeration produces.

The list spans **338 pages** across
13 documents. It is in each document's
`coverage.unresolved_items` in `eval-v3/`.

| Document | Unresolved v2 | Unresolved v3 |
|---|---:|---:|
| `0020153254COHEN__Attachment-1-Statement-of-Wor` | 4 | **0** |
| `1240LT26Q0172__3-Combined-SpecsCBPEWI20260624` | 8 | **3** |
| `15F06726R0000194__RFP-15F06726R0000194-Tier-II` | 30 | **29** |
| `1616-26__RFP1620000348` | 24 | **20** |
| `19C02026Q0027__Solicitation-19C02026Q0027` | 37 | **16** |
| `36C26126Q1034__36C26126Q1034-Brand-Name-VAPIHC` | 31 | **24** |
| `47QMCA26Q0098__RFQ47QMCA26Q0098-SF18` | 2 | **4** |
| `70CDCR26R00000026__Attachment-01-Turnkey-Facil` | 75 | **52** |
| `75N98026Q00962__RFQ-75N98026Q00962` | 16 | **29** |
| `80JSC026MEDEVAC5Q__RFP-Solicitation-80JSC027R0` | 9 | **5** |
| `W15P7T-26-R-A006__Solicitation-Amendment-004-W` | 146 | **122** |
| `W911SG27BA002__Solicitation-Amendment-W911SG27` | 57 | **63** |
| `W912P726RA022__W912P726RA002-San-Rafael-Solici` | 150 | **154** |
| `W912P825BA029__Solicitation-W912P825BA029-OM25` | 115 | **72** |
| **Total** | **704** | **593** |

### 4. Rows in the owner's audit sample that were affected

Same seed (**20260829**), so the 10-row samples are the ones already on the audit sheet.

**1 of the 140 sampled rows changed.** Everything else on the
audit sheet stands exactly as it was.

| Document | Sample # | What changed | Was page | Now page | Quote |
|---|---:|---|---:|---|---|
| `W911SG27BA002__Solicitation-Amendm` | 2 | **DROPPED (mangled)** | 49 | - | The will notify the Contracting____ [insert name of SBA's contractor] MICC Fort Bliss Officer in writing immediately upon entering an agreement (eithe |

## Everything else is unchanged from v2

The run parameters, the unreadable documents, the audit packet structure, the invention
count, and the six-criteria discussion in v2 all stand. This report changes only what is
listed above. The two full-read pages remain **unchosen** — if this report picked them the
recall check would be steerable.

| What | Where |
|---|---|
| Per-document JSON, v3 | `corpus/eval-v3/` |
| Per-document JSON, v2 (untouched) | `corpus/eval/` |
| The v2 report (untouched) | `corpus/EVAL_REPORT.md` |
| The critic pass this responds to | `corpus/CRITIC_REPORT.md` |
| The verdict slot | `DECISIONS.md` D-010 — still empty |

# CORPUS_INDEX — Phase 1 evaluation set

Downloaded from SAM.gov on 2026-08-28. Every document is a public federal solicitation
or solicitation attachment authored by a U.S. federal agency.

**Page count and file size below were measured from the downloaded files** (`pdfinfo` for
pages, the filesystem for bytes), not read off the SAM.gov listing. "Text per page" is the
extracted character count divided by pages, and is what marks a document as scanned.

**17 documents · 8–645 pages · 2,248 pages total · 142 MB · 10 agencies · 3 scanned**

| # | Agency | Solicitation | Title | Type | Pages | Size | Text/pg | Why it is in the set |
|---|---|---|---|---|---:|---:|---:|---|
| 1 | DoD | `W912P726RA022` | San Rafael Creek and ATF FY26 Maintenance Dredging | Solicitation | 645 | 33.1 MB | 2,196 | Largest text solicitation in the set; tests whether the pipeline holds up over a very long document and whether cost stays sane at this length. |
| 2 | DoD | `W15P7T-26-R-A006` | Marketplace for Acquisition of Professional Services | Solicitation | 342 | 6.7 MB | 1,957 | Large multi-volume Army RFP; the 300-page band where hand-built matrices start taking days. |
| 3 | DoD | `W912P825BA029` | Mississippi River, New Orleans Harbor and Various Ba | Solicitation | 246 | 15.7 MB | 2,288 | Large construction solicitation; heavy Section L/M structure with many binding statements. |
| 4 | DoD | `W911SG27BA002` | New Fort Bliss Paving IDIQ Contract | Solicitation | 157 | 6.9 MB | 2,250 | Mid-size DoD solicitation in ordinary Uniform Contract Format; the everyday case. |
| 5 | DHS | `70CDCR26R00000026` | Turn-key Detention Facilities | Combined Synopsis/Solicitation | 152 | 1.3 MB | 2,908 | DHS performance work statement; requirements sit in a PWS rather than a Section C. |
| 6 | DoD | `W31P4Q26RA002` | SUPPLY CHAIN OPTIMIZATION SUPPORT (SCOS) Solicitatio | Combined Synopsis/Solicitation | 144 | 21.4 MB | 23 | SCANNED. 144 pages of image-only pages. Phase 1 must declare this unreadable rather than return a thin matrix (DECISIONS.md D-004). |
| 7 | DOJ | `1616-26` | RFP FOR INSTALLATIONS SERVICES | Solicitation | 104 | 5.9 MB | 1,615 | DOJ RFP; civilian agency, conventional structure. |
| 8 | State | `19C02026Q0027` | COMPOUND CAFETERIA CANOPY MODERNIZATION | Solicitation | 81 | 2.4 MB | 2,230 | State Department construction solicitation; overseas work adds unusual clause sets. |
| 9 | DOJ | `15F06726R0000194` | Design-Build of a Tier II Mobile Command Post Vehicl | Solicitation | 75 | 0.5 MB | 2,540 | Additional civilian agency, for agency-format variety. |
| 10 | USDA | `1240LT26Q0172` | Council Bluff Primary Electric And Well Improvements | Combined Synopsis/Solicitation | 74 | 3.1 MB | 1,481 | Additional civilian agency, for agency-format variety. |
| 11 | VA | `36C26126Q1034` | VAPIHCS Flooring Materials Indefinite Delivery Indef | Solicitation | 73 | 0.9 MB | 2,197 | VA combined synopsis/solicitation; VA is a high-volume issuer and its own house format. |
| 12 | VA | `36C26026Q0939` | Underground Storage Tank Repair/Replace | Solicitation | 39 | 26.1 MB | 1 | SCANNED. A VA solicitation delivered as images at 26MB. Second unreadable case, from a different agency and a different scanner. |
| 13 | HHS | `75N98026Q00962` | dCODE Dextramer® | Combined Synopsis/Solicitation | 39 | 3.0 MB | 2,455 | HHS RFQ; civilian services buy. |
| 14 | not stated in record | `PANMCC26P0000048766` | Staff Augmentation | Combined Synopsis/Solicitation | 33 | 12.4 MB | 1 | SCANNED, borderline. Sits near the low-text threshold, so it tests where the scanned/readable line actually falls. |
| 15 | NASA | `80JSC026MEDEVAC5Q` | Emergency Medical Support and Evacuation (MedEvac) S | Solicitation | 23 | 0.7 MB | 2,471 | NASA RFP. Another agency house style, and a compact well-structured document. |
| 16 | DHS | `0020153254COHEN` | source sought COTHEN Tech Refresh - RT-2200A HF Radi | Sources Sought | 13 | 0.3 MB | 1,924 | Sources sought with a real statement of work. A different notice type carrying far fewer binding statements - a low-count case where over-extraction would show. |
| 17 | GSA | `47QMCA26Q0098` | ATF - Honda Pilot | Combined Synopsis/Solicitation | 8 | 1.4 MB | 2,404 | GSA. Small document at the bottom of the size range; checks the pipeline does something sensible on a short notice. |

## The scanned documents

Three documents are image-only or near image-only. They are in the set on purpose. v1 does not
do OCR (DECISIONS.md D-004), so the correct behaviour is to declare them unreadable as text and
charge nothing — not to return a thin matrix. A pipeline that quietly produces a short matrix
from one of these has failed, even though it did not crash.

- **W31P4Q26RA002** — 144 pages, 21.4 MB, 23 characters of extractable text per page.
- **36C26026Q0939** — 39 pages, 26.1 MB, 1 characters of extractable text per page.
- **PANMCC26P0000048766** — 33 pages, 12.4 MB, 1 characters of extractable text per page.

## What was deliberately excluded

- **Engineering drawings, plans, maps, and photographs.** They are attachments to solicitations,
  not solicitations, and contain no binding prose to extract.
- **Wage determinations, amendment cover forms (SF 30), and Q&A logs.** Supporting paperwork.
- **A NASA presolicitation conference slide deck** that was picked up by the filename filter. It
  reads like a solicitation by name and is not one; it was removed after inspection.
- **Anything not marked `public` in SAM.gov's own attachment metadata**, and anything flagged
  `exportControlled` or `explicitAccess`. Those flags were filtered before download, not after.

### The restriction check, and what it actually found

Nine documents initially tripped a text search for CUI, export-control, and classification
terms. All nine were inspected and all nine were false positives: each opens with a standard
solicitation form and carries no banner marking. The hits were contract clauses instructing the
*contractor* to protect such information — ordinary solicitation content, and in fact useful
test material, since those clauses are themselves binding requirements.

Every document in the final set was then re-checked for a banner marking on its first two pages.
None had one. No document was excluded on restriction grounds, because none needed to be.

## One agency could not be identified

`PANMCC26P0000048766` ("Staff Augmentation") has an empty organization record in SAM.gov's
search index, and the opportunity detail did not name a department either. It is listed above as
*not stated in record* rather than guessed at. It is a scanned combined synopsis/solicitation and
earns its place as a messy case regardless of which agency issued it.

## Coverage against the Phase 1 requirement

| Requirement | What the set actually has |
|---|---|
| At least 12 documents | 17 |
| Varied agencies (DoD, VA, GSA, civilian) | DHS, DOJ, DoD, GSA, HHS, NASA, State, USDA, VA, not stated in record |
| Sizes from ~30 to 400+ pages | 8 to 645 pages |
| Varied notice types | Combined Synopsis/Solicitation, Solicitation, Sources Sought |
| At least 2 messy or scanned | 3 scanned |

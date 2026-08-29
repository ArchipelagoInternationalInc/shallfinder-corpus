# CORPUS_INDEX — Phase 1 evaluation set

Downloaded from SAM.gov on 2026-08-28. Every document is a public federal solicitation or
solicitation attachment authored by a U.S. federal agency.

## How the numbers here were produced

- **Pages** — measured with `pdfinfo` (Poppler 26.08.0) on the downloaded file.
- **Size** — the file's actual size on disk, in bytes, after download.
- **Text/pg** — the character count of `pdftotext -q <file> -` (Poppler 26.08.0, default
  layout mode, whole document) divided by the page count. **This tool and these flags are
  what the characters-per-page figure means.** A different extractor, or `-layout`, gives a
  different number, so the scanned/readable classification below is only meaningful with
  reference to this tool. The pipeline's own ingest module (Task 2) uses a different
  extractor and computes its own figure; the two are not interchangeable.

Nothing here was read off the SAM.gov listing.

## Notice ID vs. solicitation number

SAM.gov gives every notice an internal **Notice ID** (a 32-character hex string, the `_id` in
its API and the value in the `/opp/<id>/view` URL) as well as the agency's own **solicitation
number**. They are different identifiers and they do not always agree: the solicitation number
is typed by the issuing office and is sometimes absent, sometimes reused across amendments,
and — as row 1 below shows — sometimes differs from the number printed inside the document
itself. The Notice ID is the stable key; use it to re-fetch a document.

**17 documents · 8–645 pages · 2,248 pages total · 142 MB · 10 agencies · 3 scanned**

| # | Agency | Notice ID | Solicitation no. | Title | Type | Pages | Size | Text/pg | Why it is in the set |
|---|---|---|---|---|---|---:|---:|---:|---|
| 1 | DoD | `d068e15a2d9146b4a0285d46dbdd7292` | `W912P726RA022` ¹ | San Rafael Creek and ATF FY26 Maintenance Dredging | Solicitation | 645 | 33.1 MB | 2,196 | Largest text solicitation in the set; tests whether the pipeline holds up over a very long document and whether cost stays sane at this length. |
| 2 | DoD | `dd067ed7e23d469dab6e5a622c30460b` | `W15P7T-26-R-A006` | Marketplace for Acquisition of Professional Servic | Solicitation | 342 | 6.7 MB | 1,957 | Large multi-volume Army RFP; the 300-page band where hand-built matrices start taking days. |
| 3 | DoD | `a0de1e1017544b4c8cf2534310b86595` | `W912P825BA029` | Mississippi River, New Orleans Harbor and Various  | Solicitation | 246 | 15.7 MB | 2,288 | Large construction solicitation; heavy Section L/M structure with many binding statements. |
| 4 | DoD | `8799e548c40f4ecb91187408ce877023` | `W911SG27BA002` | New Fort Bliss Paving IDIQ Contract | Solicitation | 157 | 6.9 MB | 2,250 | Mid-size DoD solicitation in ordinary Uniform Contract Format; the everyday case. |
| 5 | DHS | `06d1f5210673483ab27eb9b276111055` | `70CDCR26R00000026` | Turn-key Detention Facilities | Combined Synopsis/Solicitation | 152 | 1.3 MB | 2,908 | DHS performance work statement; requirements sit in a PWS rather than a Section C. |
| 6 | DoD | `65cbcfff8550450c9611a60bf1f20e82` | `W31P4Q26RA002` | SUPPLY CHAIN OPTIMIZATION SUPPORT (SCOS) Solicitat | Combined Synopsis/Solicitation | 144 | 21.4 MB | 23 | SCANNED. 144 pages of image-only pages. Phase 1 must declare this unreadable rather than return a thin matrix (DECISIONS.md D-004). |
| 7 | DOJ | `1384f1c0693e46f4a23f63cf9b4acc89` | `1616-26` | RFP FOR INSTALLATIONS SERVICES | Solicitation | 104 | 5.9 MB | 1,615 | DOJ RFP; civilian agency, conventional structure. |
| 8 | State | `ebb25512786d40158f34a46c0c209165` | `19C02026Q0027` | COMPOUND CAFETERIA CANOPY MODERNIZATION | Solicitation | 81 | 2.4 MB | 2,230 | State Department construction solicitation; overseas work adds unusual clause sets. |
| 9 | DOJ | `2096b8c82e1147738165968bf56ee10e` | `15F06726R0000194` | Design-Build of a Tier II Mobile Command Post Vehi | Solicitation | 75 | 0.5 MB | 2,540 | Additional civilian agency, for agency-format variety. |
| 10 | USDA | `af8a50fc109741a38429160b6ff92099` | `1240LT26Q0172` | Council Bluff Primary Electric And Well Improvemen | Combined Synopsis/Solicitation | 74 | 3.1 MB | 1,481 | Additional civilian agency, for agency-format variety. |
| 11 | VA | `d5b6bede19f041c8a45f44d7a95acbed` | `36C26126Q1034` | VAPIHCS Flooring Materials Indefinite Delivery Ind | Solicitation | 73 | 0.9 MB | 2,197 | VA combined synopsis/solicitation; VA is a high-volume issuer and its own house format. |
| 12 | VA | `7732648181b54834aa5608129722cd35` | `36C26026Q0939` | Underground Storage Tank Repair/Replace | Solicitation | 39 | 26.1 MB | 1 | SCANNED. A VA solicitation delivered as images at 26MB. Second unreadable case, from a different agency and a different scanner. |
| 13 | HHS | `1003f14876a54288900333a98e62e87d` | `75N98026Q00962` | dCODE Dextramer® | Combined Synopsis/Solicitation | 39 | 3.0 MB | 2,455 | HHS RFQ; civilian services buy. |
| 14 | not stated in record | `e814d359acef46bfa3e9e7ba1d141b5c` | `PANMCC26P0000048766` | Staff Augmentation | Combined Synopsis/Solicitation | 33 | 12.4 MB | 1 | SCANNED, borderline. Sits near the low-text threshold, so it tests where the scanned/readable line actually falls. |
| 15 | NASA | `4b2d5b7e1bc8429e9b6448ca664223c7` | `80JSC026MEDEVAC5Q` | Emergency Medical Support and Evacuation (MedEvac) | Solicitation | 23 | 0.7 MB | 2,471 | NASA RFP. Another agency house style, and a compact well-structured document. |
| 16 | DHS | `2231f997b7e44a7d95c2f59830928fc0` | `0020153254COHEN` | source sought COTHEN Tech Refresh - RT-2200A HF Ra | Sources Sought | 13 | 0.3 MB | 1,924 | Sources sought with a real statement of work. A different notice type carrying far fewer binding statements - a low-count case where over-extraction would show. |
| 17 | GSA | `a7027b31e6c94a2ca826e2b5329b07a4` | `47QMCA26Q0098` | ATF - Honda Pilot | Combined Synopsis/Solicitation | 8 | 1.4 MB | 2,404 | GSA. Small document at the bottom of the size range; checks the pipeline does something sensible on a short notice. |

¹ The SAM.gov notice records this solicitation as `W912P726RA022`, but the document file itself
is named `W912P726RA002`. One of the two carries a typo. The Notice ID is unambiguous and is
what should be used to identify this document.

## The scanned documents

Three documents are image-only or near image-only by the measure defined above. They are in the
set on purpose. v1 does not do OCR (DECISIONS.md D-004), so the correct behaviour is to declare
them unreadable as text and charge nothing — not to return a thin matrix. A pipeline that
quietly produces a short matrix from one of these has failed, even though it did not crash.

- **`65cbcfff8550450c9611a60bf1f20e82`** (W31P4Q26RA002) — 144 pages, 21.4 MB, 23 characters of extractable text per page.
- **`7732648181b54834aa5608129722cd35`** (36C26026Q0939) — 39 pages, 26.1 MB, 1 characters of extractable text per page.
- **`e814d359acef46bfa3e9e7ba1d141b5c`** (PANMCC26P0000048766) — 33 pages, 12.4 MB, 1 characters of extractable text per page.

## What was deliberately excluded

- **Engineering drawings, plans, maps, and photographs.** Attachments to solicitations, not
  solicitations, with no binding prose to extract.
- **Wage determinations, SF 30 amendment cover forms, and Q&A logs.** Supporting paperwork.
- **A NASA presolicitation conference slide deck** picked up by the filename filter. It reads
  like a solicitation by name and is not one; removed after inspection.
- **Anything not marked `public` in SAM.gov's own attachment metadata**, and anything flagged
  `exportControlled` or `explicitAccess`. Filtered before download, not after.

### The restriction check, and what it actually found

Nine documents initially tripped a text search for CUI, export-control, and classification
terms. All nine were inspected and all nine were false positives: each opens with a standard
solicitation form and carries no banner marking. The hits were contract clauses instructing the
*contractor* to protect such information — ordinary solicitation content, and in fact useful
test material, since those clauses are themselves binding requirements.

Every document in the final set was then re-checked for a banner marking on its first two pages.
None had one. No document was excluded on restriction grounds, because none needed to be.

## One agency could not be identified

Notice `e814d359acef46bfa3e9e7ba1d141b5c` ("Staff Augmentation", solicitation
`PANMCC26P0000048766`) has an empty organization record in SAM.gov's search index, and the
opportunity detail did not name a department either. It is listed above as *not stated in
record* rather than guessed at. It is a scanned combined synopsis/solicitation and earns its
place as a messy case regardless of which agency issued it.

## Coverage against the Phase 1 requirement

| Requirement | What the set actually has |
|---|---|
| At least 12 documents | 17 |
| Varied agencies (DoD, VA, GSA, civilian) | DHS, DOJ, DoD, GSA, HHS, NASA, State, USDA, VA, not stated in record |
| Sizes from ~30 to 400+ pages | 8 to 645 pages |
| Varied notice types | Combined Synopsis/Solicitation, Solicitation, Sources Sought |
| At least 2 messy or scanned | 3 scanned |

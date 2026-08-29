# corpus — Phase 1 evaluation set

Real federal solicitations downloaded from SAM.gov, used to test the extraction
engine before any product UI is built (see `MASTER_PLAN.md` Phase 1 in the
ShallFinder repository).

## Why these are public

Solicitations issued by U.S. federal agencies are works of the U.S. federal
government and are published openly on SAM.gov for anyone to download. Republishing
them here creates no exposure: they name government agencies and contract
opportunities, not private individuals or customers.

## What may go in this folder

Federal solicitation documents themselves — the base solicitation PDF and its
government-authored attachments (PWS, SOW, Sections L and M, amendments).

## What may NOT go in this folder

- Any attachment carrying a distribution restriction, export-control marking,
  CUI marking, or classification banner.
- Any attachment carrying a third-party copyright notice — commercial standards,
  vendor drawings, or licensed reference material incorporated into a solicitation.
- Any non-federal document (state, local, or commercial), which does not carry the
  federal government's public-domain status.
- Any customer or user upload. This corpus is downloaded from the public record only.

When in doubt about a single attachment, leave it out — the base solicitation is what
Phase 1 needs.

## What the set has to cover

At least 12 solicitations, varied deliberately rather than conveniently:

- **Agencies:** a mix across DoD, VA, GSA, and civilian agencies.
- **Sizes:** from roughly 30 pages to 400+ pages.
- **Types:** RFP, RFQ, RFI/sources sought, and combined synopsis/solicitation.
- **At least two difficult ones:** scanned image PDFs, or unusual formatting that
  breaks the ordinary structure. These exist to make the engine fail honestly rather
  than quietly.

`CORPUS_INDEX.md` lists what was chosen, with the measured page count and file size
of each document and the reason it earns its place in the set.

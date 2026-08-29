# shallfinder-corpus

The public record of the ShallFinder build: one report per Builder session in
`reports/`, and the evaluation corpus of federal solicitations in `corpus/`.

ShallFinder is a private tool from Archipelago International, Inc. It is not
affiliated with, endorsed by, or connected to the U.S. government, GSA, or SAM.gov.

## What is here

**`reports/`** — every work session files a dated report here before it ends, with
no exceptions, including short sessions. Each report says what was done, what was
decided, what is blocked, and what comes next. The reports are the project's memory
across sessions and machines, which is why they are public and why they stay.
Filenames are `YYYY-MM-DD-short-slug.md`.

**`corpus/`** — the Phase 1 evaluation set: real federal solicitations downloaded
from SAM.gov, used to test whether the extraction engine actually works before any
product is built. These are public federal paperwork. See `corpus/README.md`.

## What is not here, ever

No passwords, keys, or tokens. No server, host, or infrastructure names. No customer
documents. No uploaded user files. Nothing private.

## Rules for what goes in a report

Counts, findings, and decisions. Every number in a report is one that was actually
measured — never estimated, never recalled. Where a number was not measured, the
report says so.

## The reports rule is enforced, not remembered

A session cannot end quietly without its report. The check lives in the ShallFinder
repository at `.claude/hooks/require-session-report.sh` and runs automatically when a
session tries to finish. If today's report is missing, or written but not pushed here,
the session is stopped and told to file it.

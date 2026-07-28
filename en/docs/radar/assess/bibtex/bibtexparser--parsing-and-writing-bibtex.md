# bibtexparser

**URL:** https://github.com/sciunto-org/python-bibtexparser
**Category:** engine
**Status:** assess
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## What it is

A Python library for parsing and writing BibTeX `.bib` files. Provides
structured access to bibliography entries — authors, titles, years, keys,
fields — without performing any citation formatting.

## Why interesting for SAT

Useful as the ingestion and normalisation layer for SAT's citation pipeline.
A custom `validate-markdown.py` extension could use bibtexparser to load
`refs.bib`, confirm that every `[@key]` in a Markdown document resolves to
a real entry, and check that required fields (author, year, title, URL/DOI)
are present. This is the most practical near-term use: structured access to
the bibliography for integrity checking, not formatting.

## Concerns

- **Does not format APA7.** bibtexparser is a parser only. It must be paired
  with a formatting engine (citeproc-py or pybtex) for any output. This is
  well-documented and expected, not a deficiency.
- **v1 vs v2 API break.** bibtexparser 2.x introduced a significantly different
  API from 1.x. Code targeting one version will not work on the other. Confirm
  which version is in use and pin it.
- **BibTeX quirks.** `.bib` files from Zotero/BBT are generally clean, but
  bibtexparser's handling of edge cases (special characters, non-standard fields)
  should be validated against the actual `refs.bib` in use.

## Security assessment

**Network behaviour:**
- [ ] Checked for outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Does the tool send any content to a remote service
- [ ] Does it store content in a cloud service by default
- [ ] Are there any auto-save or backup features that copy content externally

**Assessment method:** Not yet assessed. Expected to be low-risk (pure Python,
file parsing only, no network requirements).

**Assessment status:** not assessed

## Relationship to SAT architecture

- SAT engine (validation layer — ingestion/normalisation)
- Development toolchain

## Status notes

At assess as the bibliography ingestion layer for `validate-markdown.py`. Move
to trial after: (1) confirming v1 vs v2 version choice, (2) testing against
the actual `refs.bib` export from Zotero/BBT, (3) prototyping key-resolution
check in `validate-markdown.py`. Not a standalone tool — value is as a
component within the custom validator.

## Links

- https://github.com/sciunto-org/python-bibtexparser
- https://bibtexparser.readthedocs.io
- https://pypi.org/project/bibtexparser/

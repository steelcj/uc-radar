# citeproc-py

**URL:** https://github.com/brechtm/citeproc-py
**Category:** engine
**Status:** assess
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## What it is

A Python implementation of the Citation Style Language (CSL) processor.
Applies any CSL style file — including official APA7 — to structured
bibliographic data to produce formatted citations and reference lists.
Developed by Brecht Machiels.

## Why interesting for SAT

CSL is the same standard used by Pandoc and Zotero, making citeproc-py
architecturally coherent with the existing pipeline. It is the most accurate
Python path to APA7 formatting because it uses the same official CSL files
rather than a reimplemented style. Identified as the preferred long-term
formatting engine for the SAT validation layer if deeper citation checking
is required beyond what `validate-markdown.py` currently provides.

## Concerns

- **Integration complexity.** More involved to integrate than `pybtex-apa7-style`;
  requires CSL JSON or BibTeX input and CSL file management.
- **Maintenance status.** Activity on the repository should be confirmed —
  CSL processors are non-trivial to maintain and the Python ecosystem has
  fewer contributors than the JavaScript/Rust equivalents.
- **Formatting, not validation.** Like pybtex, this is a formatting engine.
  It does not validate free text against APA7 rules. See `apa7-validation-research.md`.
- **Not yet integrated.** No prototype exists in the SAT context; marked assess,
  not trial, until integration work begins.

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
no network requirements), but confirm before integration.

**Assessment status:** not assessed

## Relationship to SAT architecture

- SAT engine (validation/formatting layer)
- Development toolchain

## Status notes

At assess as the preferred long-term Python citation formatting engine. Move to
trial after: (1) confirming repository maintenance status, (2) prototyping with
`apa.csl` against known APA7 reference samples, (3) evaluating integration
effort with `validate-markdown.py`. Would supersede `pybtex-apa7-style` if
accuracy and maintenance are confirmed. Would move to hold if the repository
is effectively unmaintained.

## Links

- https://github.com/brechtm/citeproc-py
- https://docs.citationstyles.org/en/stable/
- https://github.com/citation-style-language/styles (CSL styles repository)

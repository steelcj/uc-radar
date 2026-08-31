# Pandoc

**URL:** https://pandoc.org
**Category:** engine
**Status:** adopt
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## What it is

A universal document converter. Converts between dozens of markup formats;
in this pipeline it handles Markdown → GFM export with full citation resolution
via its built-in citeproc processor.

## Why interesting for SAT

Pandoc is the export engine for the writing-to-archive pipeline. It resolves
`[@key]` citation keys against `refs.bib` using `--citeproc`, applies APA7
formatting via `apa.csl`, and outputs clean GitHub-Flavoured Markdown ready
for SAT ingestion. No other tool in the pipeline handles this conversion step.

## Concerns

- **Version drift.** Pandoc has made breaking changes to citeproc and GFM output
  across releases. The version in use must be documented and ideally pinned.
- **`-t gfm` is lossy for citations.** All citation output becomes plain text —
  no semantic markup. If SAT requires machine-readable citation data, a parallel
  HTML or PDF export should be considered.
- **No lockfile mechanism.** Reproducibility across machines requires manual
  version coordination or a container.

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

**Assessment method:** Not yet assessed.

**Assessment status:** not assessed

## Relationship to SAT architecture

- Development toolchain
- SAT engine (export/conversion layer)

## Status notes

Adopted because it is the only realistic tool for citation-aware Markdown export
at this quality level. To remain at adopt: pin version, document in pipeline README.
Would move to hold only if a citeproc-native Markdown tool emerged with better
GFM fidelity.

## Links

- https://pandoc.org/MANUAL.html
- https://github.com/jgm/pandoc
- https://pandoc.org/installing.html

# Typora

**URL:** https://typora.io
**Category:** content
**Status:** adopt
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## What it is

A minimal, WYSIWYG-style Markdown editor. Renders Markdown inline as you type
rather than in a split preview pane. Supports Pandoc-style citation syntax
(`[@key]`) natively when configured.

## Why interesting for SAT

The authoring environment for the pipeline. Typora's inline rendering reduces
friction when writing structured Markdown that will be processed by Pandoc.
Citation keys entered as `[@key]` are preserved verbatim in the output file,
ready for citeproc resolution at export time.

## Concerns

- **Proprietary, paid software.** Typora is not open source. A licence is required
  per machine. This is a portability and longevity concern for a long-lived archive
  pipeline.
- **Single-author assumption.** Like Zotero, Typora is a personal tool. The pipeline
  does not depend on Typora's internals — any editor that outputs valid Markdown will
  work — but the workflow as documented assumes it.
- **No citation key validation at write time.** Typora does not check that `[@key]`
  keys exist in `refs.bib`. Broken keys are only caught at the validate step.
- **Vendor risk.** Typora has had extended beta periods and opaque release cycles.
  The pipeline should be documented as editor-agnostic so substitution is straightforward.

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

**Assessment method:** Not yet assessed. Priority item: confirm whether Typora
contacts any remote service on launch or document open.

**Assessment status:** not assessed

## Relationship to SAT architecture

- Content tools

## Status notes

Adopted for the authoring layer, but the pipeline is not technically dependent on
Typora specifically — any editor producing valid Markdown with `[@key]` syntax will
work. Status reflects current use, not a hard requirement. Would move to hold if
a security concern around network behaviour or content exposure is confirmed.

## Links

- https://typora.io
- https://support.typora.io

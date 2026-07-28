# Zotero-plus-Better BibTeX--reference manager

**URL:** https://www.zotero.org / https://retorque.re/zotero-better-bibtex/
**Category:** content
**Status:** adopt
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## Description

Zotero is an open-source reference manager. Better BibTeX (BBT) is a Zotero
plugin that enables stable, customisable citation keys and automatic export of
`.bib` files kept in sync with the live Zotero library.

## Why interesting for SAT

Provides the `refs.bib` file that Pandoc's citeproc requires. Better BibTeX's
auto-export feature keeps `refs.bib` current without manual intervention, and
its stable key generation ensures `[@key]` citations in Markdown remain valid
across library edits. This is the reference management layer of the pipeline.

## Concerns

- **Single-author dependency.** Zotero and BBT are personal tools. Collaborators
  not using Zotero have no clear path to maintaining `refs.bib`.
- **`refs.bib` must be version-controlled separately.** If the live export and
  the committed file diverge, citations can silently break.
- **BBT plugin maturity.** BBT is well-maintained but is a third-party plugin,
  not part of Zotero core. Plugin updates occasionally require Zotero updates.
- **Zotero 7 migration.** If still on Zotero 6, BBT compatibility should be
  confirmed before upgrading.

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

**Assessment method:** Not yet assessed. Note: Zotero offers cloud sync — assess
whether this is enabled and whether any archive-adjacent content passes through it.

**Assessment status:** not assessed

## Relationship to SAT architecture

- Content tools
- Development toolchain (bibliography management)

## Status notes

Adopted as the reference management layer. To remain at adopt: version-control
`refs.bib`, document the BBT auto-export configuration, and confirm Zotero/BBT
version compatibility. Would move to trial if a simpler `.bib` maintenance
workflow emerged that removed the Zotero dependency for non-primary authors.

## Links

- https://www.zotero.org/support/
- https://github.com/zotero/zotero
- https://retorque.re/zotero-better-bibtex/
- https://github.com/retorquere/zotero-better-bibtex

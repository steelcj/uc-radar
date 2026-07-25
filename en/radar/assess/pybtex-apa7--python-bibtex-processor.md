# pybtex + pybtex-apa7-style

**URL:** https://pybtex.org / https://pypi.org/project/pybtex-apa7-style/
**Category:** engine
**Status:** assess
**Added:** 2026-03-30
**Last reviewed:** 2026-03-30

## What it is

`pybtex` is a Python BibTeX processor — it parses `.bib` files and formats
bibliographies programmatically. `pybtex-apa7-style` is a plugin that adds
APA7 formatting support. Together they form the closest thing to a native
Python APA7 formatting engine available on PyPI.

## Why interesting for SAT

Provides a Python-native path to APA7 formatted output from `refs.bib`, without
shelling out to Pandoc. Relevant to the SAT validation layer: a custom validator
could use pybtex to reformat a reference and compare it against what appears in
the document, enabling structural APA7 checking. Simpler to integrate than
`citeproc-py` for basic use cases.

## Concerns

- **Formatting, not validation.** pybtex produces APA7 output; it does not validate
  arbitrary text against APA7 rules. The distinction is fundamental — see
  `apa7-validation-research.md`.
- **Plugin maturity.** `pybtex-apa7-style` is a third-party plugin. Maintenance
  cadence and long-term support should be confirmed before building core validation
  logic on top of it.
- **Output fidelity.** APA7 accuracy depends on the plugin's implementation. Should
  be spot-checked against authoritative APA7 examples before adoption.
- **Less powerful than `citeproc-py`.** For complex citation types or edge cases,
  the CSL-based approach will be more accurate.

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
no network requirements), but confirm before use in pipeline.

**Assessment status:** not assessed

## Relationship to SAT architecture

- SAT engine (validation/formatting layer)
- Development toolchain

## Status notes

At assess because it is the simplest Python path to APA7 formatted output, but
has not been tested in the SAT context. Move to trial after: (1) spot-checking
APA7 output fidelity, (2) confirming plugin is actively maintained, (3) prototyping
integration with `validate-markdown.py`. Would move to hold in favour of
`citeproc-py` if accuracy requirements exceed what the plugin can deliver.

## Links

- https://pybtex.org
- https://github.com/chrisyeh96/pybtex-apa7-style
- https://pypi.org/project/pybtex-apa7-style/

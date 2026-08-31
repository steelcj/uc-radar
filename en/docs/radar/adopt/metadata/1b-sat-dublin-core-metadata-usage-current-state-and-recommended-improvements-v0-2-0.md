# SAT Dublin Core Metadata Usage: Current State and Recommended Improvements

Version: 0.2.0
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7-v0.2.0.md

---

## Abstract

This document assesses current Dublin Core metadata usage across SAT's default configuration files and sidecars against the authoritative DCMI specifications, and proposes specific improvements. It is intended as a radar entry in the `assess` ring and as the basis for future ADRs and tooling decisions. Where prior SAT design decisions have already resolved an issue, those resolutions are noted. Where issues remain open, they are flagged explicitly.

**2026-07-31 note:** this entry was decomposed rather than promoted — its seven items were logged as individual backlog cards rather than carried forward as one unit. Five of the seven have since reached genuine, verified resolution (not merely drafted intent); one no longer applies to the current architecture as originally written; one remains open with expanded scope. Each item below records both the original assessment and its current status.

---

## Sources and Acknowledgements

This assessment is grounded in two authoritative sources maintained by the Dublin Core Metadata Initiative. The primary specification consulted is the <a name="apa-dcmi-terms-citation"></a>[DCMI Metadata Terms (DCMI Usage Board, 2020)](#apa-dcmi-terms-reference), which is the up-to-date specification of all metadata terms maintained by the Dublin Core Metadata Initiative. Practical guidance is drawn from <a name="apa-usageguide-citation"></a>[Using Dublin Core: The Elements (Hillmann, 2005)](#apa-usageguide-reference), a DCMI Recommended Resource providing guidance on creating descriptive records for information resources.

---

## 1. Scope

This assessment covers the following SAT files as they existed at the time of writing:

- `en/bin/content/definitions/defaults/default-canonical-metadata.yml` — content-level DC defaults
- `en/bin/sat/definitions/defaults/sat.yml` — SAT operational configuration
- `en/bin/sat/examples/sat-preseed.yml.example` — SAT preseed example
- DC sidecars produced by `sat-init.py` and related tools

**2026-07-31 note:** `default-canonical-metadata.yml` and the `sat-init.py`/`sat-preseed.yml.example` pattern it describes no longer exist. Both were retired — the former by the ADR-023 nursery-pipeline amendment (superseded by `cataloging.py`), the latter by ADR-026 (`sat init` runs the full chain directly; no tier below the instance reads a userland preseed). Several resolutions below only make sense read against that retirement — the fix, in multiple cases, turned out to be that the buggy file is simply gone.

---

## 2. Issues Identified

### 2.1 dc:language — ISO 639-1 vs ISO 639-2 inconsistency

The content-level defaults file carried `dc:language: "en"` — an ISO 639-1 two-letter code. The SAT language validation specification (v0.1.0) and ADR-003 establish ISO 639-2 three-letter codes as the standard for `dc:language` throughout SAT, with BCP 47 tags carried separately in `dc:language_bcp47`. These were inconsistent. The <a name="apa-dcmi-terms-citation-2"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) does not mandate a specific ISO 639 variant but recommends consistent use of a recognised standard.

**Resolution:** Resolved. `default-canonical-metadata.yml` — the file carrying the inconsistency — was deleted in the ADR-023 nursery-pipeline retirement. Its replacement never had the bug: `language.py` already emits ISO 639-2 forms, confirmed directly in `test_archive.py`. Closed by the deletion, not by a correction to the deleted file.

### 2.2 dc:contributor — empty string vs absent field

The content-level defaults file carried `dc:contributor: ""` — an empty string. Per the SAT calculated metadata placeholder convention (v0.1.0), a deliberately empty field is represented as an empty string, and a field still reading `<calculated>` at creation is a tooling error. However, `dc:contributor` when no contributor exists should be omitted entirely rather than set to an empty string, as an empty string implies a contributor with no name rather than the absence of a contributor. Per <a name="apa-usageguide-citation-2"></a>[Hillmann (2005)](#apa-usageguide-reference), contributor is an optional element that should not be included when not applicable.

**Resolution:** Resolved. `dc:contributor` added to ADR-023's normative cataloging policy table (v0.2.1): transcribed only, no cascade supply, omitted entirely — not an empty string — when absent, the one field in the table with no fallback. The `"Name (Organization)"` convention for AI-assisted authorship (e.g. `"Claude Sonnet 4.6 (Anthropic)"`) is recorded as a formatting note for a transcribed value, not a new detection mechanism — `cataloging.py` performs no AI-assistance detection of its own, since nothing in the ingress pipeline could supply that signal.

### 2.3 dc:rights — human-readable label vs URI

Current usage carried `dc:rights: "CC BY-SA 4.0"` — a human-readable license label. The <a name="apa-dcmi-terms-citation-3"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) recommends identifying rights with a URI where possible. The authoritative Creative Commons URI for CC BY-SA 4.0 is `https://creativecommons.org/licenses/by-sa/4.0/`.

**Resolution:** Superseded, not resolved. Checked against the actual current source (`satlib/create.py`, `create_instance_role()`): there is no baked default to convert at all. `dc:rights` is either operator-supplied at `sat init` — in whatever form the operator types it — or the `<calculated>` tripwire, unresolved. The issue as originally written doesn't apply to the current architecture. A different, smaller, undrafted question stands in its place: should SAT ship an actual default value here instead of leaving it a tripwire, and if so, in which form? Not decided.

### 2.4 dc:type — absent at archive level

The content-level defaults marked `dc:type` as `[calculated]` — always `"Text"` for `.md` files. This was correct for content. However, the archive level carried no `dc:type` at all. The <a name="apa-usageguide-citation-3"></a>[Hillmann (2005)](#apa-usageguide-reference) recommends selecting a value from the DCMI Type Vocabulary to ensure interoperability and including at least one general type term. The DCMI Type Vocabulary does not include an `Archive` type. The closest standard match for a SAT archive is `Collection`.

**Resolution:** Resolved. `dc:type: "Collection"` applied at both the collection tier (`create.py`'s `create_collection_role()`) and the archive tier (`archive.py`'s `plan_archive()`), verified by passing test assertions in both `test_create.py` and `test_archive.py`. The absence of an `Archive` type in the DCMI Type Vocabulary remains a known limitation; no SAT-specific type extension has been drafted, as the limitation hasn't become significant enough to warrant one. Recorded in `sat-controlled-vocabulary-v0.4.1.md`'s new Metadata section, including the finding that this doesn't collide with SAT's own Collection tier despite sharing the word — the role-directory path already disambiguates which is meant.

### 2.5 licence vs dc:rights in sat.yml — field naming and spelling

The SAT operational configuration file `sat.yml` carries `licence: "GPL v3"` — a non-DC field using British spelling. This is operational configuration, not resource metadata, so it does not belong in a DC sidecar. However, the spelling `licence` is inconsistent with the North American English convention (`license`) used throughout SAT's other documentation and code.

**Resolution:** Open, scope larger than originally assessed. Confirmed `sat.yml` still reads `licence: "GPL v3"`. Originally framed as a two-way inconsistency (`sat.yml` vs. the rest of the codebase's spelling convention); actually a three-way disagreement on the license itself, not just its spelling: `sat.yml` says GPL v3, `pyproject.toml` declares `GPL-3.0-only`, and `LICENSE` (plus the licence block on every ADR drafted this session) says AGPL-3.0. Worth resolving as one item — which license is actually intended — rather than the narrower spelling-and-field-name patch this entry originally proposed.

### 2.6 dc:format — MIME type confirmation

The content-level defaults mark `dc:format` as `[calculated]` — always `"text/markdown"` for `.md` files. The MIME type `text/markdown` is correct per RFC 7763. The <a name="apa-dcmi-terms-citation-4"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) recommends using a controlled vocabulary such as the list of Internet Media Types (MIME) for `dc:format` values. Current SAT usage is consistent with this recommendation.

**Resolution:** Resolved. No change required. `dc:format: "text/markdown"` is correct and spec-compliant. No new information this pass.

### 2.7 dc:publisher default — tool vs operator

The content-level defaults carried `dc:publisher: "SAT - Source Archive Tools"` — identifying the tool as the publisher rather than the operator or their organization. The <a name="apa-usageguide-citation-4"></a>[Hillmann (2005)](#apa-usageguide-reference) defines publisher as the entity responsible for making the resource available. SAT is the tool used to publish; it is not the publisher. The publisher should be the operator or their organization.

**Resolution:** Resolved. Closed by the same deletion as 2.1: `default-canonical-metadata.yml`, the file carrying the baked default, no longer exists. The instance-tier `dc.yml` (`create_instance_role()`) has no baked publisher default at all — only an operator-stated value or the `<calculated>` tripwire. The `sat-meta.yml`-based design this entry expected never shipped; ADR-026 retired that whole pattern (no tier below the instance reads a userland preseed) in favour of the tripwire approach, which resolves this issue by a different route than originally proposed.

---

## 3. Summary Table

| Issue | Severity | Status (0.1.1) | Status (0.2.0) |
|-------|----------|-----------------|-----------------|
| 2.1 dc:language ISO 639-1 vs 639-2 | High | Resolved in design, correction pending | **Resolved** |
| 2.2 dc:contributor empty string | Medium | Open | **Resolved** |
| 2.3 dc:rights label vs URI | Low | Open | **Superseded** — issue doesn't apply to current architecture |
| 2.4 dc:type absent at archive level | Medium | Partially resolved | **Resolved** |
| 2.5 licence spelling and field name | Low | Open | **Open** — scope expanded to a three-way license disagreement |
| 2.6 dc:format MIME type | None | Resolved | Resolved (unchanged) |
| 2.7 dc:publisher default | High | Open | **Resolved** |

---

## Resources

### Dublin Core Standards
- [DCMI Metadata Terms](#apa-dcmi-terms-reference)
- [Using Dublin Core: The Elements](#apa-usageguide-reference)

---

## References

<a name="apa-dcmi-terms-reference"></a>DCMI Usage Board. (2020). *DCMI metadata terms*. Dublin Core Metadata Initiative. https://www.dublincore.org/specifications/dublin-core/dcmi-terms/
[Return to citation](#apa-dcmi-terms-citation)

<a name="apa-usageguide-reference"></a>Hillmann, D. (2005). *Using Dublin Core: The elements*. Dublin Core Metadata Initiative. https://www.dublincore.org/specifications/dublin-core/usageguide/elements/
[Return to citation](#apa-usageguide-citation)

---

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Status pass against confirmed current state: 2.1, 2.2, 2.4, 2.7 resolved for real (verified against source/tests, not just drafted intent); 2.3 superseded — no baked default exists in current architecture to convert; 2.5 still open, scope expanded to a three-way license disagreement; 2.6 unchanged. Scope note added: two of the three files this entry originally assessed no longer exist. |
| 0.1.1 | Draft | "at birth" replaced by "at creation" per ADR-020 |
| 0.1.0 | Draft | Initial assessment — seven issues identified, two resolved, one partially resolved, four open |

---

**License**

This document, *SAT Dublin Core Metadata Usage: Current State and Recommended Improvements*, by Christopher Steel, with AI assistance from Claude Sonnet 4.6 (Anthropic), is licensed under the [Creative Commons Attribution-ShareAlike 4.0 License](https://creativecommons.org/licenses/by-sa/4.0/).

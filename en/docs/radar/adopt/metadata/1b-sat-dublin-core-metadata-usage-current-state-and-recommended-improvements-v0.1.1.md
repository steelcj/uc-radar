# SAT Dublin Core Metadata Usage: Current State and Recommended Improvements

Version: 0.1.1
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7-v0.2.0.md

---

## Abstract

This document assesses current Dublin Core metadata usage across SAT's default configuration files and sidecars against the authoritative DCMI specifications, and proposes specific improvements. It is intended as a radar entry in the `assess` ring and as the basis for future ADRs and tooling decisions. Where prior SAT design decisions have already resolved an issue, those resolutions are noted. Where issues remain open, they are flagged explicitly.

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

---

## 2. Issues Identified

### 2.1 dc:language — ISO 639-1 vs ISO 639-2 inconsistency

The content-level defaults file carries `dc:language: "en"` — an ISO 639-1 two-letter code. The SAT language validation specification (v0.1.0) and ADR-003 establish ISO 639-2 three-letter codes as the standard for `dc:language` throughout SAT, with BCP 47 tags carried separately in `dc:language_bcp47`. These are inconsistent. The <a name="apa-dcmi-terms-citation-2"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) does not mandate a specific ISO 639 variant but recommends consistent use of a recognised standard.

**Resolution:** SAT design has already resolved this in favour of ISO 639-2 for `dc:language` and BCP 47 for `dc:language_bcp47`. The content-level defaults file requires updating to `dc:language: "eng"` to match the settled decision. This is a tooling correction, not a new decision.

### 2.2 dc:contributor — empty string vs absent field

The content-level defaults file carries `dc:contributor: ""` — an empty string. Per the SAT calculated metadata placeholder convention (v0.1.0), a deliberately empty field is represented as an empty string, and a field still reading `<calculated>` at creation is a tooling error. However, `dc:contributor` when no contributor exists should be omitted entirely rather than set to an empty string, as an empty string implies a contributor with no name rather than the absence of a contributor. Per <a name="apa-usageguide-citation-2"></a>[Hillmann (2005)](#apa-usageguide-reference), contributor is an optional element that should not be included when not applicable.

**Resolution:** Open. The defaults file should omit `dc:contributor` entirely when no contributor is known. When AI assistance is involved, `dc:contributor` should carry the assistant's name and organization in the form `"Name (Organization)"` — for example `"Claude Sonnet 4.6 (Anthropic)"`. This is a SAT local convention for representing agents as plain strings, consistent with DCMI's recommendation to use a literal value when a URI is not available.

### 2.3 dc:rights — human-readable label vs URI

Current usage carries `dc:rights: "CC BY-SA 4.0"` — a human-readable license label. The <a name="apa-dcmi-terms-citation-3"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) recommends identifying rights with a URI where possible. The authoritative Creative Commons URI for CC BY-SA 4.0 is `https://creativecommons.org/licenses/by-sa/4.0/`.

**Resolution:** Open. SAT should adopt the URI form for `dc:rights` in a future revision of the defaults and tooling. The human-readable label may be retained as a YAML comment. This improves machine readability without loss of human readability. No blocking ADR required — this is a straightforward best practice alignment.

### 2.4 dc:type — absent at archive level

The content-level defaults mark `dc:type` as `[calculated]` — always `"Text"` for `.md` files. This is correct for content. However, the archive level carries no `dc:type` at all. The <a name="apa-usageguide-citation-3"></a>[Hillmann (2005)](#apa-usageguide-reference) recommends selecting a value from the DCMI Type Vocabulary to ensure interoperability and including at least one general type term. The DCMI Type Vocabulary does not include an `Archive` type. The closest standard match for a SAT archive is `Collection`.

**Resolution:** Partially resolved. SAT will use `dc:type: "Collection"` at both the collection and archive tiers. The absence of an `Archive` type in the DCMI Type Vocabulary is a known limitation noted in the SAT Dublin Core usage specification (v0.1.0). A future ADR may define a SAT-specific type extension if the limitation becomes significant.

### 2.5 licence vs dc:rights in sat.yml — field naming and spelling

The SAT operational configuration file `sat.yml` carries `licence: "GPL v3"` — a non-DC field using British spelling. This is operational configuration, not resource metadata, so it does not belong in a DC sidecar. However, the spelling `licence` is inconsistent with the North American English convention (`license`) used throughout SAT's other documentation and code.

**Resolution:** Open. The `sat.yml` field name should be corrected to `license` for internal consistency. The value `"GPL v3"` should be updated to the precise SPDX identifier `"GPL-3.0-only"` or replaced with the license URI `https://www.gnu.org/licenses/gpl-3.0.html` for machine readability. This is a configuration correction, not a metadata decision.

### 2.6 dc:format — MIME type confirmation

The content-level defaults mark `dc:format` as `[calculated]` — always `"text/markdown"` for `.md` files. The MIME type `text/markdown` is correct per RFC 7763. The <a name="apa-dcmi-terms-citation-4"></a>[DCMI Usage Board (2020)](#apa-dcmi-terms-reference) recommends using a controlled vocabulary such as the list of Internet Media Types (MIME) for `dc:format` values. Current SAT usage is consistent with this recommendation.

**Resolution:** Resolved. No change required. `dc:format: "text/markdown"` is correct and spec-compliant.

### 2.7 dc:publisher default — tool vs operator

The content-level defaults carry `dc:publisher: "SAT - Source Archive Tools"` — identifying the tool as the publisher rather than the operator or their organization. The <a name="apa-usageguide-citation-4"></a>[Hillmann (2005)](#apa-usageguide-reference) defines publisher as the entity responsible for making the resource available. SAT is the tool used to publish; it is not the publisher. The publisher should be the operator or their organization.

**Resolution:** Open. `dc:publisher` should be an operator-supplied value with no default, derived from the SAT metadata cascade via `sat-meta.yml` at the time of archive creation. The default `"SAT - Source Archive Tools"` should be removed from the content-level defaults file. This is addressed by the `sat-meta.yml` design being developed in parallel.

---

## 3. Summary Table

| Issue | Severity | Status |
|-------|----------|--------|
| 2.1 dc:language ISO 639-1 vs 639-2 | High — inconsistent with ADR-003 | Resolved in design, correction pending in defaults file |
| 2.2 dc:contributor empty string | Medium — misleading representation | Open |
| 2.3 dc:rights label vs URI | Low — best practice alignment | Open |
| 2.4 dc:type absent at archive level | Medium — interoperability gap | Partially resolved |
| 2.5 licence spelling and field name | Low — internal consistency | Open |
| 2.6 dc:format MIME type | None | Resolved |
| 2.7 dc:publisher default | High — incorrect attribution | Open |

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
| 0.1.1 | Draft | "at birth" replaced by "at creation" per ADR-020 |
| 0.1.0 | Draft | Initial assessment — seven issues identified, two resolved, one partially resolved, four open |

---

**License**

This document, *SAT Dublin Core Metadata Usage: Current State and Recommended Improvements*, by Christopher Steel, with AI assistance from Claude Sonnet 4.6 (Anthropic), is licensed under the [Creative Commons Attribution-ShareAlike 4.0 License](https://creativecommons.org/licenses/by-sa/4.0/).

# Dublin Core Metadata Usage in SAT

Version: 0.1.1
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7-v0.2.0.md

---

## Abstract

This document defines how SAT uses Dublin Core metadata terms in sidecars, provenance records, and metadata cascade defaults. It establishes which namespace to use, how to represent agent values (creator, contributor, publisher), and how SAT's local extensions relate to the standard. It is intended as the authoritative internal reference for anyone authoring or reviewing SAT metadata, and as the basis for tooling decisions in `sat-init`, `archive-init`, and related tools.

---

## Sources and Acknowledgements

The metadata terms used in SAT are drawn from two authoritative sources maintained by the Dublin Core Metadata Initiative. The primary specification is the <a name="apa-dcmi-terms-citation"></a>[DCMI Metadata Terms (DCMI Usage Board, 2020)](#apa-dcmi-terms-reference), which is the up-to-date specification of all metadata terms maintained by the Dublin Core Metadata Initiative, including properties, vocabulary encoding schemes, syntax encoding schemes, and classes. Practical guidance on element usage is drawn from <a name="apa-dcmi-elements-citation"></a>[Using Dublin Core — The Elements (Hillmann, 2005)](#apa-dcmi-elements-reference), a DCMI Recommended Resource that assists practitioners in creating descriptive records for information resources.

---

## 1. Namespace Decision

### 1.1 Two namespaces

Dublin Core provides two namespaces for its core properties. The legacy namespace `dc:` (`http://purl.org/dc/elements/1.1/`) contains the original fifteen properties of the Dublin Core Metadata Element Set (DCMES) Version 1.1, all defined without domains or ranges. The `dcterms:` namespace (`http://purl.org/dc/terms/`) contains semantically more precise versions of the same fifteen properties, defined as subproperties of the `dc:` variants, with domains and ranges assigned.

### 1.2 SAT namespace choice

SAT uses the `dc:` namespace prefix throughout its sidecars and metadata files. This is a deliberate pragmatic choice: SAT metadata is stored as YAML, not RDF, and the semantic precision of the `dcterms:` namespace — expressed through domains, ranges, and URI-valued properties — is not meaningful in a YAML key-value context. The `dc:` prefix is shorter, more widely recognised, and sufficient for SAT's use case. SAT does not claim RDF compliance.

Where SAT uses refinements from the `dcterms:` namespace that have no `dc:` equivalent — such as `dcterms:created` for creation dates — these are used with their full `dcterms:` prefix and noted explicitly in this document.

### 1.3 Properties used in SAT

The following Dublin Core properties are used in SAT sidecars and metadata defaults:

| SAT key | Namespace | DC label | Notes |
|---------|-----------|----------|-------|
| `dc:title` | dc: | Title | Required at archive creation |
| `dc:creator` | dc: | Creator | Primary responsibility |
| `dc:contributor` | dc: | Contributor | Secondary contributions |
| `dc:publisher` | dc: | Publisher | Organization making resource available |
| `dc:subject` | dc: | Subject | Keywords or classification |
| `dc:description` | dc: | Description | Human-readable account |
| `dc:date` | dc: | Date | ISO 8601 date |
| `dc:type` | dc: | Type | From DCMI Type Vocabulary |
| `dc:format` | dc: | Format | MIME type |
| `dc:language` | dc: | Language | ISO 639-2 |
| `dc:rights` | dc: | Rights | Rights statement or license URI |
| `dcterms:created` | dcterms: | Created | ISO 8601 creation date — dcterms: refinement |

---

## 2. Agent Properties

### 2.1 Creator

`dc:creator` identifies the entity primarily responsible for making the resource. <a name="apa-dcmi-elements-citation-2"></a>[Hillmann (2005)](#apa-dcmi-elements-reference) recommends listing creators separately in order of decreasing precedence where order is significant. SAT uses `dc:creator` as a scalar string for a single creator, or a YAML list for multiple creators:

```yaml
dc:creator: "Christopher Steel"
```

```yaml
dc:creator:
  - "Christopher Steel"
  - "Universal Cake Inc"
```

### 2.2 Contributor

`dc:contributor` identifies entities that made contributions to the resource but are not primarily responsible for it. SAT uses `dc:contributor` for AI assistants, editors, and secondary collaborators. Per the usage guide, contributors should not include those listed in `dc:creator`.

```yaml
dc:contributor: "Claude Sonnet 4.6 (Anthropic)"
```

The parenthetical organization notation `Name (Organization)` is a SAT local convention for representing an agent and their affiliated organization as a plain string, consistent with Dublin Core's recommendation to use a literal value when a URI is not available.

### 2.3 Publisher

`dc:publisher` identifies the entity responsible for making the resource available. Where the publisher is an organization, the organization name is used as a plain string:

```yaml
dc:publisher: "Universal Cake Inc"
```

---

## 3. Type

`dc:type` describes the nature or genre of the resource. <a name="apa-dcmi-elements-citation-3"></a>[Hillmann (2005)](#apa-dcmi-elements-reference) recommends selecting a value from the DCMI Type Vocabulary to ensure interoperability. SAT uses the following values from the DCMI Type Vocabulary:

| SAT tier | dc:type value |
|----------|--------------|
| Collection | `Collection` |
| Archive | `Collection` |
| Content (text) | `Text` |

Note: the DCMI Type Vocabulary does not include an `Archive` type. SAT uses `Collection` for both collection and archive tiers, as a collection is the closest standard match for an archive in Dublin Core terms. This is a known limitation and may be revisited in a future ADR.

---

## 4. Format

`dc:format` identifies the file format or physical medium of the resource. The usage guide recommends using a controlled vocabulary such as the list of Internet Media Types (MIME). SAT uses MIME types as plain strings:

```yaml
dc:format: "text/markdown"
```

---

## 5. Language

`dc:language` identifies the language of the resource. SAT uses ISO 639-2 three-letter codes for `dc:language`, and BCP 47 tags for `dc:language_bcp47`. The latter is a SAT local extension carrying the full BCP 47 tag where regional or script information is required:

```yaml
dc:language: "eng"
dc:language_bcp47: "en-CA"
```

---

## 6. Rights

`dc:rights` carries a rights statement or license reference. SAT uses plain string values. Where a Creative Commons license applies, the human-readable identifier is used:

```yaml
dc:rights: "CC BY-SA 4.0"
```

A future revision may use the license URI instead, consistent with Dublin Core's recommendation to use URIs where possible.

---

## 7. Date

`dc:date` carries a point or period of time associated with an event in the lifecycle of the resource. SAT uses ISO 8601 reduced-precision dates for `dc:date`:

```yaml
dc:date: "2026-06-14"
```

For archive creation dates SAT uses `dcterms:created`, a refinement of `dc:date` that specifically denotes the date of creation:

```yaml
dcterms:created: "2026-06-14"
```

---

## 8. Local Extensions

SAT defines the following local extensions beyond the Dublin Core standard. These use the `sat:` prefix and are not interoperable with external Dublin Core consumers without documentation:

| SAT key | Purpose |
|---------|---------|
| `dc:language_bcp47` | Full BCP 47 tag where ISO 639-2 is insufficient |
| `sat:authority` | IANA registry validation result (`external`, `partial`, `none`) |
| `sat:authority_note` | Human-readable note on non-standard language expressions |

---

## Resources

### Dublin Core Standards
- [DCMI Metadata Terms](#apa-dcmi-terms-reference)
- [Using Dublin Core — The Elements](#apa-dcmi-elements-reference)

---

## References

<a name="apa-dcmi-terms-reference"></a>DCMI Usage Board. (2020). *DCMI metadata terms*. Dublin Core Metadata Initiative. https://www.dublincore.org/specifications/dublin-core/dcmi-terms/
[Return to citation](#apa-dcmi-terms-citation)

<a name="apa-dcmi-elements-reference"></a>Hillmann, D. (2005). *Using Dublin Core: The elements*. Dublin Core Metadata Initiative. https://www.dublincore.org/specifications/dublin-core/usageguide/elements/
[Return to citation](#apa-dcmi-elements-citation)

---

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.1 | Draft | birth/born vocabulary replaced by creation/provenance per ADR-020 |
| 0.1.0 | Draft | Initial draft — namespace decision, agent properties, type, format, language, rights, date, local extensions |

---

**License**

This document, *Dublin Core Metadata Usage in SAT*, by Christopher Steel, with AI assistance from Claude Sonnet 4.6 (Anthropic), is licensed under the [Creative Commons Attribution-ShareAlike 4.0 License](https://creativecommons.org/licenses/by-sa/4.0/).

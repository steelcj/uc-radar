# dcterms: RDF Conformance — Cost of Adoption Analysis

Version: 0.1.0
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7-v0.2.0.md

---

## Abstract

ADR-028 adopts `dc:` for the SAT MVP and defers full `dcterms:` conformance, on the grounds that SAT metadata is YAML read by a regex-based checker, not RDF, and that `dcterms:`'s typed obligations — Agent-valued properties and formal encoding schemes — are not currently honored regardless of which prefix is used in front matter. This document examines what would actually be required to close that gap: to use `dcterms:` in a way that earns its semantic precision rather than borrowing its name. It separates the two obligations bundled under "RDF conformance" — Agent typing and encoding-scheme validation — and assesses each independently, since they differ enormously in cost. It is written as supporting analysis for ADR-028, not as a proposal to reverse it.

---

## Sources and Acknowledgements

The namespace distinction and its formal obligations are drawn from <a name="apa-dcmi-terms-citation"></a>[DCMI Metadata Terms (DCMI Usage Board, 2020)](#apa-dcmi-terms-reference), the authoritative specification of Dublin Core properties, encoding schemes, and classes. Practical guidance on agent representation is drawn from <a name="apa-dcmi-elements-citation"></a>[Using Dublin Core — The Elements (Hillmann, 2005)](#apa-dcmi-elements-reference). This document extends the namespace decision recorded in *Dublin Core Metadata Usage in SAT* and ADR-028, and should be read alongside both.

---

## 1. Two Separate Obligations, Not One

"Full `dcterms:` conformance" is often treated as a single yes/no adoption decision. It is not. It bundles two independent obligations with very different costs:

1. **Agent typing** — `dcterms:creator`, `dcterms:contributor`, and `dcterms:publisher` are typed as `Agent`, a resource that acts or has the power to act. Honoring this means these values are identifiable resources, not plain strings.
2. **Encoding schemes** — `dcterms:date` (and its refinements), `dcterms:type`, `dcterms:format`, and `dcterms:language` carry recommended encoding schemes: W3C-DTF for dates, the DCMI Type Vocabulary for type, IMT for format, and RFC 3066/BCP 47 for language.

Treating these as one decision is the main reason "adopt `dcterms:`" looks more expensive than it is in parts and cheaper than it is in others. They are assessed separately below.

---

## 2. Encoding Schemes — The Cheap Half

### 2.1 Current state

SAT already partially satisfies this obligation without having framed it as one:

- `dc:date` is already stored as ISO 8601 reduced-precision dates, which is compatible with W3C-DTF.
- `dc:format` is already a MIME type string (`text/markdown`), which is exactly what the IMT encoding scheme calls for.
- `sat:language_bcp47` already exists as a local extension carrying full BCP 47 tags alongside the ISO 639-2 `dc:language` value — this is, in substance, already RFC 3066/BCP 47 conformance, just not labeled as such.
- `dc:type` uses values from the DCMI Type Vocabulary (`Collection`, `Text`) already.

### 2.2 What adoption actually requires

- Documenting the existing practice as intentional encoding-scheme conformance, rather than an accidental byproduct of sensible defaults.
- Closing the two known gaps already tracked on the backlog: `dc:language` currently stored as ISO 639-1 in one defaults file instead of the settled ISO 639-2, and `dc:rights` stored as a human-readable label rather than a URI.
- No new validation infrastructure is required beyond what SAT's regex-based conformance checker already does for language tags via the IANA registry.

### 2.3 Assessment

This is close to a documentation-only change plus two already-scoped backlog items. It does not require RDF serialization, a triple store, or any new tooling category. **Low cost, largely already done.**

---

## 3. Agent Typing — The Expensive Half

### 3.1 What the obligation actually means

`dcterms:creator` and `dcterms:contributor` being typed as `Agent` means the value is expected to be a resource with its own identity — something that could, in principle, be dereferenced, linked to, or have properties of its own (an ORCID iD, a VIAF identifier, a GitHub profile URI). A plain string satisfies `dc:creator`, which carries no such constraint, but does not satisfy what `dcterms:creator` formally asks for.

### 3.2 What would actually be required to honor this

- **A resource model for agents.** Every distinct creator, contributor, or publisher needs a stable identifier — either an external URI (ORCID, VIAF, a GitHub profile) or a locally-minted one, with a place to register and resolve it. SAT currently has no such registry.
- **A migration path for existing content.** Every `dc:creator: "Christopher Steel"` string in the corpus would need to become a reference to an identified Agent resource, which means either a bulk rewrite or a dual-representation transition period.
- **Serialization capable of expressing the relationship.** YAML front matter read by a regex checker cannot natively distinguish "this value is a literal string" from "this value is a reference to a resource." Expressing that distinction meaningfully implies moving toward JSON-LD, RDF, or a structured local convention that reimplements enough of RDF's semantics to carry the distinction — at which point SAT is building bespoke tooling to simulate the thing it chose not to adopt.
- **Tooling to validate and maintain referential integrity** — broken agent references, duplicate agent identities, and resolution of local vs. external identifiers all become new failure modes that don't exist under plain strings.

### 3.3 Assessment

This is not a metadata-formatting change, it is an identity-modeling project with its own data model, migration, and tooling surface. It is disproportionate to SAT's current scope and is the correct thing to defer, exactly as ADR-028 does. **High cost, correctly deferred.**

---

## 4. Advantages of Full Adoption (Both Halves)

- **Genuine RDF interoperability.** Content becomes meaningfully consumable by RDF tooling, SPARQL endpoints, and linked-data aggregators without a translation layer.
- **Agent disambiguation.** Two different "Christopher Steel" values in different archives could be distinguished or merged correctly, which plain strings cannot support.
- **External enrichment.** Agent resources with ORCID/VIAF identifiers open the door to automatically pulling in affiliations, other works, or authority-file data.
- **Alignment with the guide's stated intent.** If the versioned-documents guide's original aim was long-term RDF conformance, closing this gap eventually fulfills that intent rather than abandoning it.

## 5. Disadvantages of Full Adoption

- **Disproportionate to current scale.** SAT is a filesystem-first, YAML/regex-validated tool. An Agent-typed identity system is infrastructure built for a much larger, more interoperability-driven deployment than SAT's current MVP scope.
- **New failure surface.** Identifier resolution, referential integrity, and dual local/external identifier handling are all new categories of bug that don't exist under the current model.
- **Migration cost paid once, benefit realized only if RDF consumption actually happens.** If nothing ever consumes SAT metadata as RDF, the entire Agent-typing investment returns no realized value — it is speculative infrastructure.
- **Delays the actual metadata project.** Building an identity-resource model before the metadata project can proceed reorders priorities around a capability nothing currently depends on.

---

## 6. Recommendation

Treat this as two decisions with two different postures, matching ADR-028's own split:

1. **Encoding schemes** — adopt formally now. The work is nearly done; the remaining gap is two backlog items and a documentation pass declaring what's already true practice as intentional policy.
2. **Agent typing** — leave deferred, with a named trigger for revisiting it: a concrete future requirement for RDF consumption, cross-archive agent disambiguation, or external authority-file enrichment. Absent such a trigger, this obligation should stay named and tracked, not built.

This keeps SAT honest about which half of `dcterms:` it can adopt cheaply and which half it is correctly choosing not to build yet, rather than treating "dcterms: vs dc:" as one undifferentiated cost.

---

## Resources

### Dublin Core Standards
- [DCMI Metadata Terms](#apa-dcmi-terms-reference)
- [Using Dublin Core — The Elements](#apa-dcmi-elements-reference)

### Related SAT documents
- ADR-028: Dublin Core Namespace — dc: for MVP, dcterms: Deferred
- Radar entry (adopted): *Dublin Core Metadata Usage in SAT*

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
| 0.1.0 | Draft | Initial draft — splits RDF conformance into encoding-scheme and Agent-typing obligations, assesses each independently, recommends adopting encoding schemes now and deferring Agent typing with a named trigger |

---

**License**

This document, *dcterms: RDF Conformance — Cost of Adoption Analysis*, by Christopher Steel, with AI assistance from Claude, is licensed under the [Creative Commons Attribution-ShareAlike 4.0 License](https://creativecommons.org/licenses/by-sa/4.0/).

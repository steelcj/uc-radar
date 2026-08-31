---
dc:title: "Publishing Vector, Content Conformance Metadata"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "metadata"
  - "dublin-core"
  - "conformance"
  - "style-guide"
  - "reading-level"
  - "accessibility"
  - "asd-ste100"
  - "wcag"
  - "publishing-vector"
dc:description: "Radar entry assessing how a content file should record the standards it conforms to — its governing style guide, its reading level, and recognized standards such as WCAG and ASD-STE100 — as portable metadata in the blank-studio publishing vector, distinguishing recognized standards from unrecognized ones, and separating the shipped interim (sat:style-guide) from the fuller model still under design."
dc:publisher: "UniversalCake"
dcterms:created: "2026-08-10"
dcterms:modified: "2026-08-10"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "spec--content-format-for-the-publishing-vector"
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, spec--content-format-for-the-publishing-vector, spec--content-ingress, vocabulary--multilingual-content-structure"
dc:identifier: "publishing-vector--content-conformance-metadata"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-08-10"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Initial entry. Documents the problem of recording standards-conformance
      on content files (governing style guide, reading level, recognized
      standards), the recognized-versus-unrecognized pattern SAT already uses
      for language standards, the shipped interim (sat:style-guide), and the
      fuller Dublin Core plus schema.org model under design. Evaluated against
      the metrics as an approach rather than a product; tool-only sections
      marked not applicable.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Opus-4.8"
      recommendation: hold
      reasoning: >
        The interim convention sat:style-guide is safe to use immediately: one
        field per content file, holding the governing guide exactly as the
        automa Style Guide line already expresses it, low risk and reversible.
        The fuller standards-conformance model is not ready to adopt. It needs
        a UC standards registry (so a reference can be resolved and flagged
        recognized or unrecognized), an identity scheme for standards (URI for
        recognized, opaque id plus label for unrecognized), a reading-level
        vocabulary, and a build-time verification path so a plain-language or
        reading-grade claim can be tagged Verified rather than Claimed.
        Recommend holding the full model in assess pending those conditions and
        adopting only the interim in the meantime. This is a design approach,
        not a product, so the tool-oriented pillars (security network and file
        behaviour, environment, interaction patterns, market position) are
        marked not applicable; the substance is in Inclusive and Agency.
      risk: low
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-08-10"
  human: []
---

# Publishing Vector, Content Conformance Metadata

## What it is

A content file in the blank-studio publishing vector needs to record which standards it was written to conform to: its governing **style guide** (the editorial rulebook), its **reading level**, and any recognized standards such as **WCAG 2.1** or **ASD-STE100**. This entry evaluates how to express that as portable, machine-legible metadata, and separates what has shipped from what is still being designed.

Two things make it more than a single field. First, register is not one concept but two facets: *which rulebook governs the writing* (a style guide) and *who the writing is readable by* (a reading level) — and reading level is itself an accessibility signal, so this metadata is a forward vector for accessibility evaluation. Second, the standards referenced fall into two classes, exactly as SAT already treats language standards: **recognized** standards that have an authoritative identity and body (WCAG, ASD-STE100), and **unrecognized** ones that are asserted locally and have no standards body (a house style guide, a "grade 7 reading level" guideline). Both are allowed; the difference must be recorded so a consumer knows the trust level, mirroring the Verified-versus-Claimed discipline of the metrics themselves.

## Why interesting

Getting this right turns editorial intent into data that SAT, this site, and any other publishing vector can read without translation, and it makes the accessibility of the content measurable rather than asserted. The shape under design:

- **Governing style guide** — the editorial rulebook. Easy to understand and needed now.
- **Reading level** — target audience level (for example a grade-7 guideline for general audiences, aligned with WCAG Success Criterion 3.1.5 Reading Level), and, ideally, a *measured* readability grade computed at build so the claim is Verified.
- **Recognized standards** — WCAG 2.1 at a level, ASD-STE100 for controlled English, each with an authoritative URI.
- **Unrecognized standards** — a local style guide or an in-house reading-level guideline, allowed but flagged as not backed by a standards body.
- **Accessibility feature inventory** — the schema.org accessibility vocabulary (`accessibilityFeature`, `accessibilitySummary`, `accessMode`), the recognized way to state what accessibility features the content actually carries.

The candidate Dublin Core and schema.org mapping, verified against the term definitions:

- `dcterms:conformsTo` — DCMI defines it as "an established standard to which the described resource conforms," range `dcterms:Standard`, value recommended to be a URI. It is a list, so a file can conform to several standards at once. This is the correct home for recognized standards (WCAG, ASD-STE100) and, by extension with a recognized flag, for local ones.
- `dcterms:educationLevel` — DCMI's audience sub-property for the level of the intended audience. The DC-native place for target reading level.
- `dcterms:isVersionOf` — considered and set aside for this purpose. It states that a resource is a version or edition *of another resource*, not that it conforms to a standard; using it for standards conformance would misuse the term. It stays reserved for work/expression and edition relations.
- `sat:style-guide` — a UC-local extension for the governing guide, honest about being editorial and local rather than a pretend-Dublin-Core term. This is the shipped interim.
- schema.org `accessibilityFeature` / `accessibilitySummary` / `accessMode` — the recognized accessibility-metadata vocabulary (W3C and DCMI aligned), reserved for the accessibility-vector phase.

A sketch of the full model on a single file:

```yaml
sat:style-guide: "style-guide--plain-language-for-general-audiences"  # interim, shipped
# --- under design, this entry ---
dcterms:conformsTo:
  - id: "https://www.w3.org/TR/WCAG21/"     # recognized
    level: "AA"
    recognized: true
  - id: "urn:uc:standard:asd-ste100"        # recognized controlled language
    recognized: true
  - id: "urn:uc:guideline:reading-grade-7"  # unrecognized local guideline
    recognized: false
dcterms:educationLevel: "urn:uc:reading-level:general"   # target audience level
sat:readabilityGrade:                                     # measured -> Verified
  method: "flesch-kincaid"
  grade: 7.2
  measured: "2026-08-10"
schema:accessibilityFeature:                              # reserved, a11y vector
  - "structuralNavigation"
  - "readingOrder"
  - "alternativeText"
```

## Concerns

- **Complexity.** A correct model needs a standards registry so a reference can be resolved and classified recognized or unrecognized, plus an identity scheme (URI for recognized, opaque id plus human label for unrecognized). That is real design work, not a field rename, which is why the full model is held in assess.
- **Verification path.** A reading-level or plain-language claim is only worth as much as its evidence tag. Without a build step that computes a readability grade, every such claim is Claimed, not Verified — weaker than the metrics ask for.
- **Per-expression, not per-work.** Register and reading level can differ across translations, so these fields live on each expression, not on the shared work. The model must not push them up to `sat:work`.
- **Vocabulary drift.** `dcterms:educationLevel` describes the *audience's* level, which is an approximation of editorial register; a UC reading-level vocabulary is needed so the values are stable and comparable.
- **Interim honesty.** `sat:style-guide` alone cannot express recognized-standard conformance; sites that need to assert WCAG or ASD-STE100 today must use optional `dcterms:conformsTo` until the full model lands.

## Evaluation

Evaluated against Universal Cake Evaluation Metrics v0.3.1. Each rating uses the scale Strong / Moderate / Weak / Unknown, tagged Verified / Inferred / Claimed. This is an approach, not a product, so tool-only areas are marked not applicable and the stakeholder lens is noted where it changes the answer.

### Inclusive

**Accessibility, alternative methods of interacting with content**
Lens: user. The accessibility floor gate applies to the *content*, not to this metadata convention, so it is not a gate here.
- Rating: Strong
- Evidence: Inferred
- Notes: The approach makes reading level and accessibility conformance first-class, machine-readable metadata (conformsTo WCAG, educationLevel, a measured readability grade, and the schema.org feature inventory), turning accessibility from an unstated hope into a queryable property. The interim alone does less; the rating reflects the full model under assessment.
  - Gate status: not applicable (convention, not content).

**Representation**
Lens: community, user.
- Rating: Moderate
- Evidence: Inferred
- Notes: Because the fields are per-expression, each language can state its own register, reading level, and conformance honestly, including where a translation targets a different audience. It does not by itself broaden representation, but it removes a place where languages are flattened together.

**Resilience**
Lens: owner, user.
- Rating: Strong
- Evidence: Inferred
- Notes: Built on open, durable vocabularies (Dublin Core, schema.org) with a recognized-versus-unrecognized flag, so references degrade gracefully: an unresolved standard is still legible as a labelled claim rather than a broken reference.

### Agency

**Sovereignty & Privacy**
Lens: owner, user. Telemetry gate does not apply to a metadata convention.
- Rating: Strong
- Evidence: Inferred
- Notes: Uses standard, portable terms rather than a proprietary schema, so conformance claims travel with the content to any tool or vector.
- Power-imbalance proxies (author↔platform):
  - **Exit cost:** Low. The metadata is plain YAML front matter in the content file.
  - **Data portability:** Yes. Dublin Core and schema.org are understood by any consumer; nothing is locked to this site.
  - **Terms-of-service volatility:** Not applicable.
  - **Pricing asymmetry:** Not applicable.
- Reversibility test: "If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?" Answer: near zero. `sat:work` to `dcterms:isVersionOf`, or `sat:style-guide` to `dcterms:conformsTo`, are mechanical renames; no information is lost.
- Gate status: not applicable.

**Interaction Patterns**
Lens: user.
- Rating: not applicable
- Evidence: not applicable
- Notes: No user-facing interaction; this is authoring-time metadata.

### Sustainability

**Environment, direct and indirect impacts**
Lens: environment.
- Rating: not applicable
- Evidence: not applicable
- Notes: A few lines of front matter; a build-time readability computation is negligible.

### Security
Lens: owner.
- Rating: not applicable
- Evidence: not applicable
- Notes: A metadata convention has no runtime. One editorial caution: conformance metadata is public, so it should assert only what is true, since a false accessibility claim is itself a harm. Assessment method: design review, 2026-08-10.
  - Gate status: not applicable.

**Network behaviour:** not applicable (no runtime).

**File system behaviour:** not applicable (front matter within the content file only).

**Content exposure:** not applicable (adds no external transmission).

### The Product or Service Itself

**Longevity**
Lens: owner, community.
- Rating: Strong
- Evidence: Inferred
- Notes: Dublin Core and schema.org are long-lived, widely implemented standards; a convention built on them outlives any single tool.

**Content endurance**
Lens: owner, community.
- Rating: Strong
- Evidence: Inferred
- Notes: Conformance and identity travel in the file itself, so the meaning survives a move to another vector or archive.

**Exit and portability**
Lens: owner, user. Gate applies if no exit exists.
- Rating: Strong
- Evidence: Inferred
- Notes: The metadata *is* the exit format; it is already in the shape you would leave with.
  - Gate status: Pass.

**Adjustability and support**
Lens: owner, community.
- Rating: Moderate
- Evidence: Inferred
- Notes: The interim is trivially adjustable. The full model's adjustability depends on the standards registry and reading-level vocabulary that do not yet exist.

### Market Position
Lens: community.
- Rating: not applicable
- Evidence: not applicable
- Notes: Internal approach, no market.

## Scorecard
| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Inferred | Makes reading level and a11y conformance first-class metadata (full model) |
| Multilingual integration | Moderate | Inferred | Per-expression fields; each language states its own register honestly |
| Economic and cognitive accessibility | Strong (cognitive) | Inferred | Reading-level + measured readability serve cognitive accessibility |
| Representation | Moderate | Inferred | Removes a flattening point; does not itself broaden representation |
| Compatibility | Strong | Inferred | Dublin Core + schema.org; any consumer understands it |
| Resilience | Strong | Inferred | Recognized/unrecognized flag degrades gracefully |
| Agency, sovereignty & privacy | Strong | Inferred | Standard, portable terms; no lock-in |
| Agency, power-imbalance proxies | Strong | Inferred | Renames are mechanical; near-zero exit cost |
| Agency, interaction patterns | N/A | N/A | No runtime interaction |
| Environment, direct and indirect | N/A | N/A | Negligible |
| Security | N/A | N/A | No runtime; assert only what is true |
| Longevity | Strong | Inferred | Built on durable standards |
| Content endurance | Strong | Inferred | Meaning travels in the file |
| Exit and portability | Strong | Inferred | The metadata is the exit format |
| Adjustability and support | Moderate | Inferred | Full model needs a registry + reading-level vocabulary |
| Market position | N/A | N/A | Internal approach |
| Gates | Pass (none applicable to a convention) | | Accessibility floor and telemetry gates apply to content/tools, not to this metadata |

## Relationship to project

Metadata layer of the publishing vector. Governs the front matter defined by *spec--content-format-for-the-publishing-vector* and consumed by *spec--content-ingress*; complements *vocabulary--multilingual-content-structure*. The interim `sat:style-guide` is in use now; the full model, if adopted, would extend the content-format spec.

## Status notes

- Last reviewed: 2026-08-10
- In assess: shipped interim is `sat:style-guide` (governing guide as a slug, per file); `dcterms:conformsTo` available as an optional list for recognized standards today.
- What would move it to trial: a prototype standards registry plus a build-time readability computation, tried on the legal and foundations sections to see whether recognized/unrecognized classification and a Verified reading grade hold up in practice.
- What would move it to adopt: a settled UC standards registry and identity scheme, a reading-level vocabulary, and a human evaluation pass recorded in `uc-radar_evaluation.human`.
- What would move it to hold: a decision to freeze at the interim if the full model's complexity outweighs its value.

## Links

- Dublin Core, Conforms To: https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/conformsTo/
- Dublin Core, Audience Education Level: https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/educationLevel/
- schema.org accessibilityFeature: https://schema.org/accessibilityFeature
- W3C Schema.org Accessibility Properties for Discoverability: https://www.w3.org/2021/a11y-discov-vocab/latest/
- Understanding WCAG SC 3.1.5 Reading Level: https://www.w3.org/WAI/WCAG21/Understanding/reading-level.html
- ASD-STE100 (Simplified Technical English): https://www.asd-ste100.org/

## License

This document, *Publishing Vector, Content Conformance Metadata*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial entry; the conformance-metadata problem, recognized vs unrecognized standards, the DC + schema.org candidate model, the shipped `sat:style-guide` interim, and evaluation as an approach with tool-only pillars marked N/A |

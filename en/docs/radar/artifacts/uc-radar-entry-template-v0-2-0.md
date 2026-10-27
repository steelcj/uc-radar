---
dc:title: "UC Radar Entry Template"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "documentation"
  - "template"
dc:description: "Markdown template for creating new uc-radar entries, structured around the Universal Cake Evaluation Metrics."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-24"
dcterms:modified: "2026-07-24"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "uc-radar--evaluation-lifecycle, universal-cake-evaluation-metrics"
dc:identifier: "uc-radar-entry-template"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-07-24"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Restructured the body of the entry template around the pillars
      of the Universal Cake Evaluation Metrics, Inclusive
      (Accessibility, Representation, Resilience), Agency (Sovereignty
      & Privacy, Interaction Patterns), Sustainability (Environment),
      Security, and The Product or Service Itself (Longevity, Content
      endurance, Exit and portability, Adjustability and support,
      Market Position), each with rating, evidence tag, and notes
      fields. Folded the former standalone Security assessment
      checklist into the Security pillar as its assessment method.
      Added the metrics document's scorecard table. Retained the
      uc-radar_evaluation front matter block introduced in 0.1.0.
      dcterms:relation updated to reference both the lifecycle
      document and the metrics document. sat:version_at_creation
      updated to 0.3.1 to match the current metrics document version.
  - version: "0.1.0"
    date: "2026-07-24"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Migrated from sat-radar-entry-template-v0.2.2, renamed to the
      uc-radar naming convention. Added the uc-radar_evaluation front
      matter block.
---

# UC Radar Entry Template

## Description

A raw markdown template used to create new uc-radar entries. The evaluation body follows the pillars defined in the current [Universal Cake Evaluation Metrics](#universal-cake-evaluation-metrics), so a completed entry and the metrics document use the same vocabulary throughout.

By default new entries go in the `assess` ring unless they document
an item that has been adopted informally and is currently relied upon
in dev, stage, or prod.

The radar uses five states, `assess`, `trial`, `adopt`, `hold`, and
`rejected`. See
[UC Radar Evaluation Lifecycle](#uc-radar--evaluation-lifecycle) for
the full state definitions and transitions.

## Notes

- The leading colon in the heading example below (`:hugo--static-site-generator`)
  is a legacy convention from an earlier naming version. Current slug
  convention per ADR-015 uses double hyphen as a grouping separator
  without a leading colon. This has not been reconciled with the
  no-version, double-hyphen-plus-tagline convention used by existing
  radar entries, see the open item in the radar README.
- `<calculated>` in front matter means the value is derived at
  document creation time — by the author, a tool, or the uc-radar
  preseed mechanism once implemented.
- Every evaluation pass, automated or human, is recorded in the
  `uc-radar_evaluation` block, not in prose alone.
- Rating, evidence tag, and gate conventions below are defined in
  full in the Universal Cake Evaluation Metrics document. This
  template does not restate that guidance, it only provides the
  fields to fill in.

---

## The Radar Entry Template

Copy the contents below and paste into a new markdown document to
create your radar entry.

`````markdown
---
dcterms:title: ""
dcterms:version: "0.1.0"
dcterms:creator: ""
dcterms:contributor: ""
dcterms:subject:
  - "radar"
  - ""
dcterms:description: ""
dcterms:publisher: ""
dcterms:created: ""
dcterms:modified: ""
dcterms:type: ""
dcterms:format: ""
dcterms:language: ""
sat:language_bcp47: ""
dcterms:source: ""
dcterms:relation: ""
dcterms:identifier: ""
dcterms:rightsHolder: ""
dcterms:rights: ""
sat:uuid: ""
sat:version_at_creation: ""
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: ""
    author: ""
    notes: "Initial entry."
uc-radar_evaluation:
  automated:
    - evaluator: ""
      evaluator_version: ""
      recommendation: "" # adopt / hold / rejected / high-risk
      reasoning: "" # criteria driving the recommendation
      risk: "" # low / moderate / high
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: ""
      evaluation_datetime: ""
  human: []
---

# <project>--<product_service_or_approach>--<product_service_or_approach_category>-v<document_version>

radar entry naming convention

````bash
<project>--<product_service_or_approach>--<product_service_or_approach_category>-v<document_version>
````
where:
````yaml
project: uc-radar
product_service_or_approach: hugo
product_service_or_approach_category
document_version: 0.1.0
````
So `<project>--<product_service_or_approach>--<product_service_or_approach_category>-v<document_version>` becomes something like:

````markdown
uc-radar--hugo--static-site-generator-v0.0.1
````

## What it is

A short neutral description. One or two sentences. Not a sales pitch —
just what it is.

## Why interesting

What problem it solves, and in what context.

## Concerns

Honest assessment of risks, limitations, maturity, licence, or fit
issues. If there are no concerns worth noting, say so explicitly
rather than leaving this section empty.

## Evaluation

Rate every applicable item Strong, Moderate, Weak, or Unknown, and tag
each rating Verified, Inferred, or Claimed. Items marked **[GATE]**
follow the gate rule, a failed gate is not averaged away by strength
elsewhere. State the stakeholder lens, owner, user, community, or
environment, where it changes the answer.

### Inclusive

**Accessibility**

- Rating:
- Evidence:
- Notes:

**Representation**

- Rating:
- Evidence:
- Notes:

**Resilience**

- Rating:
- Evidence:
- Notes:

### Agency

**Sovereignty & Privacy**, including the power-imbalance proxies, exit
cost, data portability, terms-of-service volatility, pricing asymmetry

- Rating:
- Evidence:
- Notes:

**Interaction Patterns**, supportive versus dark patterns

- Rating:
- Evidence:
- Notes:

### Sustainability

**Environment**, direct and indirect impacts

- Rating:
- Evidence:
- Notes:

### Security

- Rating:
- Evidence:
- Notes:

**Assessment method:**
How the above was tested — network monitor used, platform, version,
date.

**Network behaviour:**
- [ ] Outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent
- [ ] Affects networking or items created that will be transferred
      over a network
- [ ] Helps ensure that documents passed via network are clean
      and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

### The Product or Service Itself

**Longevity**

- Rating:
- Evidence:
- Notes:

**Content endurance**

- Rating:
- Evidence:
- Notes:

**Exit and portability**

- Rating:
- Evidence:
- Notes:

**Adjustability and support**

- Rating:
- Evidence:
- Notes:

**Market Position**, market concentration, take rate, API stability,
forkability, contributor concentration

- Rating:
- Evidence:
- Notes:

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | | | |
| Multilingual integration | | | |
| Economic and cognitive accessibility | | | |
| Representation | | | |
| Compatibility | | | |
| Resilience | | | |
| Agency, sovereignty & privacy | | | |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | | | |
| Agency, interaction patterns | | | |
| Environment, direct and indirect | | | |
| Security | | | |
| Longevity | | | |
| Content endurance | | | |
| Exit and portability | | | |
| Adjustability and support | | | |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | | | |
| Gates | Pass / Fail (name any failed gate) | | |

## Relationship to project

Where this sits in Universal Cake, or where it would graduate to on
adopt.

Examples:
- A tier: engine, driver, content tools, archive tools, API, web UI,
  legal/compliance, dev toolchain
- A docs area: language/, architecture/, guides/, specifications/

Note: a technique or approach may not map to a single tier, name the
area instead.

## Status notes

Why the entry is in its current state, assess, trial, adopt, hold, or
rejected. Nothing moves to trial, adopt, hold, or rejected without a
reason here and a corresponding entry in uc-radar_evaluation.

- Last reviewed:
- In assess: what evaluation is pending, and what recommendation, if
  any, has already come back from an automated pass.
- In trial: scope and environment, what is being deployed or
  evaluated, and where. Findings so far, what has been observed,
  confirmed, or contradicted relative to the assess entry. What would
  constitute a completed trial and move it to adopt or hold. Per the
  metrics document, all gate criteria and the Security pillar must
  carry Verified tags before an item moves to trial.
- In adopt: the destination it graduates to, for example `en/docs/`
  in the consuming project. The entry leaves the active radar once
  migrated.
- In hold: what named condition would need to change for it to be
  reconsidered. Reconsideration re-enters at assess, not at hold.
- In rejected: the reasoning is archived here and in
  uc-radar_evaluation. There is no re-entry, reconsideration is a new
  assess entry.

## Links

- <source / repository>

## License

This document, *<title>*, by **<author>**, with AI assistance from
**Claude (Anthropic)**, is licensed under the
[GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).
`````

---

## License

This document, *UC Radar Entry Template*, by **Christopher Steel**,
with AI assistance from **Claude (Anthropic)**, is licensed under the
[GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Restructured evaluation body around Universal Cake Evaluation Metrics pillars, folded Security assessment checklist into the Security pillar, added scorecard table, updated dcterms:relation and sat:version_at_creation |
| 0.1.0 | Draft | Migrated from sat-radar-entry-template-v0.2.2 to the uc-radar naming convention, added the uc-radar_evaluation front matter block |

---
dc:title: "UC Radar Evaluation Lifecycle"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "evaluation"
  - "lifecycle"
  - "governance"
dc:description: "Defines the uc-radar evaluation lifecycle, its states and transitions, and the uc-radar_evaluation front matter block used to record automated and human evaluation passes against a radar entry."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-24"
dcterms:modified: "2026-07-24"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "uc-radar-entry-template"
dc:identifier: "uc-radar--evaluation-lifecycle"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-24"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Initial version, superseding the earlier four-ring plus rejected
      draft. Reconciles the lifecycle with uc-radar's actual current
      scope, assess, automated evaluation, a conditional trial stage
      for high-risk items, human review, adopt, hold, rejected.
      Introduces the uc-radar_evaluation front matter block recording
      automated and human evaluation passes, each with its own
      recommendation, reasoning, and risk rating.
---

# UC Radar Evaluation Lifecycle

Version: 0.1.0
Status: Draft
Style Guide: markdown defaults, commas not em dashes, no heading numbers, no horizontal rules

## Purpose

uc-radar is the evaluation and decision layer for Universal Cake. It decides which third-party tools, approaches, and ideas get adopted for building any Universal Cake project, and it decides the shape of Universal Cake's own tools, including SAT and the osat-fluent line. Every umbrella project consumes uc-radar's decisions rather than maintaining its own evaluation process. This document defines the states an entry can occupy and the transitions between them.

## States

**assess**

The entry point. A developer, user, or other party has a new app, idea, or approach with some initial notion of what it needs to do. It is recorded here before any evaluation has produced a recommendation.

**trial**

A conditional, not guaranteed, stage. Entered only when an item is flagged high risk and a human reviewer decides hands on deployment or testing is warranted before a final call. An entry can be flagged high risk and still skip trial if the reviewer judges a decision can be made without it.

**adopt**

Approved and in active use.

**hold**

A reversible pause. The entry stays associated with the named condition that would need to change for it to be reconsidered. Reconsideration moves the entry back to assess, it does not resume mid lifecycle.

**rejected**

A terminal archive state, outside the active rings. Exists for due diligence record keeping, not active tracking. There is no re-entry path. If the same app, idea, or approach is reconsidered later, a fresh assess entry is started rather than reopening the rejected one.

## Flow

1. An entry is created in assess.
2. The latest version of the Universal Cake Evaluation Metrics is applied, automated where possible, manual where not. This is a best effort pass, not a final decision.
3. The evaluation produces a recommendation, adopt, hold, rejected, or high risk, together with stated reasoning and a risk rating. This is recorded in the entry's uc-radar_evaluation front matter block, see below.
4. If the evaluation does not flag the entry high risk, a human reviewer confirms or overrides the recommendation and moves the entry directly to adopt, hold, or rejected.
5. If the evaluation flags the entry high risk, human intervention is required. The reviewer most often, though not always, routes the entry through trial before deciding.
6. An entry in trial exits to adopt or hold based on what the trial found. A human records this evaluation pass in the same uc-radar_evaluation block, as a human entry.
7. An entry in hold returns to assess if and when the named condition changes.
8. An entry in rejected does not move again. Reconsideration is a new assess entry.

At every step, automation proposes and a human decides. No transition to adopt, hold, or rejected happens without a human evaluation pass recorded against the entry, even when that pass simply confirms the automated recommendation.

## The uc-radar_evaluation front matter block

Every evaluation pass, automated or human, is recorded as a list item under its type within a single `uc-radar_evaluation` block. An entry accumulates passes over its life rather than overwriting them, an automated pass now, a human confirmation later, a further pass if the entry is re-evaluated after a Universal Cake Evaluation Metrics version bump, each appended in place.

```yaml
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: adopt # / hold / rejected / high-risk
      reasoning: "Excellent fit for universal cake" # criteria driving the recommendation
      risk: low # / moderate / high
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: 0.1.0 # version of metrics applied
      evaluation_datetime:
  human:
    - evaluator: "Robert Jones"
      recommendation: adopt # / hold / rejected
      reasoning: ""
      risk: low # / moderate / high
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: 0.1.0
      evaluation_datetime:
```

`evaluator_version` is automated only, a human evaluator has no version string. `risk` is carried by both types, for an automated pass it flags whether human intervention is required, for a human pass it records the reviewer's own risk assessment as part of what they are accountable for.

## Gate handling

The Universal Cake Evaluation Metrics define gate criteria, a licence incompatible with the project, telemetry or content exposure that cannot be disabled, failure of the accessibility floor, no exit. A failed gate is not routed automatically. Automation does its best effort, records the recommendation and the specific reasoning, naming the gate that failed, and sets risk accordingly, usually high. The actual move to trial, hold, or rejected remains a human decision recorded as a separate evaluation pass.

## Open items

Two points remain undefined and are flagged here rather than assumed.

- What specifically qualifies an item as high risk beyond a failed gate, for example proximity to a gate failure, handling of user data, or production exposure, has not yet been settled.
- Whether the document identity fields already in use, `sat:uuid`, `sat:version_at_creation`, `sat:migration_status`, `sat:changelog`, should remain under the `sat:` namespace or move to a `uc-radar:` or `uc:` namespace now that uc-radar entries serve every umbrella project, not SAT alone, has not been settled.

## Resources

### Related documents

- [UC Radar Entry Template](#uc-radar-entry-template)
- [Universal Cake Evaluation Metrics](#universal-cake-evaluation-metrics)

## License

This document, *UC Radar Evaluation Lifecycle*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial version, supersedes earlier four-ring plus rejected draft, reconciles lifecycle with uc-radar's current scope, introduces uc-radar_evaluation front matter block |

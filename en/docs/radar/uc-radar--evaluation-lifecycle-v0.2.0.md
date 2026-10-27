---
dc:title: "UC Radar Evaluation Lifecycle"
dcterms:version: "0.2.0"
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
dcterms:modified: "2026-10-27"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics"
dc:identifier: "uc-radar--evaluation-lifecycle"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Aligned with Universal Cake Evaluation Metrics 0.6.0. Separated
      testing from trial: testing is hands-on checking inside assess
      that makes evidence Verified, and Verified gates and Security are
      now the condition for moving from assess to trial or adopt; trial is optional real use
      in a limited, named scope, no longer reserved for high-risk items.
      Made trial a recommendation value and retired high-risk as one,
      since risk already carries that signal; existing entries keep the
      values they recorded. Added a one-line summary of each state and
      aligned gate handling with the new exit condition.
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

Version: 0.2.0
Status: Draft
Style Guide: markdown defaults, commas not em dashes, no heading numbers, no horizontal rules

## Purpose

uc-radar is the evaluation and decision layer for Universal Cake. It decides which third-party tools, approaches, and ideas get adopted for building any Universal Cake project, and it decides the shape of Universal Cake's own tools, including SAT and the osat-fluent line. Every umbrella project consumes uc-radar's decisions rather than maintaining its own evaluation process. This document defines the states an entry can occupy and the transitions between them. The Universal Cake Evaluation Metrics say what each state requires of the evaluation itself.

## In brief

Assess asks what a product does. Trial asks how it lives with us. Testing happens inside assess, so an entry carries verified evidence before anyone relies on it, and trial is used only when real use can answer something testing cannot.

## States

**assess**

The entry point and the place where evaluation happens. An entry is recorded here with some initial notion of what it needs to do, then evaluated against the current metrics. Evaluation includes testing: hands-on checks on a disposable machine, such as a network trace, a screen reader pass, or a test export, that turn Inferred or Claimed evidence into Verified. Testing is work inside assess, not a state of its own, and its method and date go in the entry.

An entry moves from assess to trial or adopt only when every gate criterion and the Security section carries a Verified tag, or a named person has recorded an override in the entry, as the metrics describe. A move to hold or rejected for a failed gate needs only that failure verified.

**trial**

Optional. Real use in a limited, named scope, such as one project, one machine, one client, or a fixed period, with the end condition stated before the trial starts. Trial is for questions testing cannot answer: what something costs to run over time, whether it stays accessible in daily use, whether a process holds up in real work. High-risk entries usually go through trial; others may go from assess to adopt directly.

**adopt**

Approved and in active use.

**hold**

A reversible pause. The entry stays associated with the named condition that would need to change for it to be reconsidered. Reconsideration moves the entry back to assess, it does not resume mid lifecycle.

**rejected**

A terminal archive state, outside the active rings. Exists for due diligence record keeping, not active tracking. There is no re-entry path. If the same app, idea, or approach is reconsidered later, a fresh assess entry is started rather than reopening the rejected one.

## Flow

1. An entry is created in assess.
2. The latest version of the Universal Cake Evaluation Metrics is applied, automated where possible, manual where not.
3. Testing continues until every gate criterion and the Security section is Verified, a gate failure is verified, or a named person records an override.
4. The evaluation produces a recommendation, adopt, trial, hold, or rejected, together with stated reasoning and a risk rating. This is recorded in the entry's uc-radar_evaluation front matter block, see below.
5. A human reviewer confirms or overrides the recommendation and moves the entry. Where risk is high, the reviewer considers trial before adopt, and records why if trial is skipped.
6. An entry in trial exits to adopt, hold, or rejected based on what the trial found, measured against the end condition set when it started. A human records this evaluation pass in the same uc-radar_evaluation block, as a human entry.
7. An entry in hold returns to assess if and when the named condition changes.
8. An entry in rejected does not move again. Reconsideration is a new assess entry.

At every step, automation proposes and a human decides. No transition out of assess, or out of trial, happens without a human evaluation pass recorded against the entry, even when that pass simply confirms the automated recommendation.

## The uc-radar_evaluation front matter block

Every evaluation pass, automated or human, is recorded as a list item under its type within a single `uc-radar_evaluation` block. An entry accumulates passes over its life rather than overwriting them, an automated pass now, a human confirmation later, a further pass if the entry is re-evaluated after a Universal Cake Evaluation Metrics version bump, each appended in place.

```yaml
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: adopt # / trial / hold / rejected
      reasoning: "Excellent fit for universal cake" # criteria driving the recommendation
      risk: low # / moderate / high
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: 0.1.0 # version of metrics applied
      evaluation_datetime:
  human:
    - evaluator: "Robert Jones"
      recommendation: adopt # / trial / hold / rejected
      reasoning: ""
      risk: low # / moderate / high
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: 0.1.0
      evaluation_datetime:
```

`evaluator_version` is automated only, a human evaluator has no version string. `risk` is carried by both types, for an automated pass it flags whether human intervention is needed beyond confirmation, for a human pass it records the reviewer's own risk assessment as part of what they are accountable for. Entries made before version 0.2.0 may carry `high-risk` as a recommendation; read it as risk high, with trial recommended.

## Gate handling

The Universal Cake Evaluation Metrics define gate criteria, a licence incompatible with the project, telemetry or content exposure that cannot be disabled, failure of the accessibility floor, no exit. A failed gate is not routed automatically. Automation does its best effort, records the recommendation and the specific reasoning, naming the gate that failed, and sets risk accordingly, usually high. The move to hold or rejected, or an override that lets the entry continue, remains a human decision recorded as a separate evaluation pass.

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
| 0.2.0 | Draft, version provisional | Aligned with metrics 0.6.0: testing is part of assess and Verified gates and Security are the condition for moving on to trial or adopt; trial is optional real use in a limited, named scope; trial added as a recommendation value and high-risk retired as one; one-line summary of each state; gate handling aligned |
| 0.1.0 | Draft | Initial version, supersedes earlier four-ring plus rejected draft, reconciles lifecycle with uc-radar's current scope, introduces uc-radar_evaluation front matter block |

---
dcterms:title: "WeKan, Accessible Kanban Board"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "radar"
  - "wekan"
  - "kanban"
  - "project-management"
  - "accessibility"
  - "wcag"
dcterms:description: "Radar entry assessing WeKan, an MIT-licensed self-hosted Kanban board with recently verified WCAG 2.1 AA accessibility work, including keyboard-driven card reordering without drag-and-drop."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-07-25"
dcterms:modified: "2026-07-25"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: "https://github.com/wekan/wekan"
dcterms:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, leantime--cognitively-accessible-project-management"
dcterms:identifier: "wekan--accessible-kanban-board"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-25"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: "Initial entry, documentation and web review only, no installation or hands-on trial performed."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: adopt
      reasoning: >
        Strong fit for the stated need, a lightweight, accessible
        Kanban board for a tiny team. Recent WCAG 2.1 AA work
        (v9.46) addresses the hardest accessibility problem in
        Kanban, drag-and-drop, with keyboard-focusable move buttons.
        MIT licence is maximally permissive, no open-core concern.
        Self-hostable via Docker, Snap, or Sandstorm. Lighter than
        Leantime, closer to "just a board." No gate failures
        identified. The main concern is that the cognitive
        accessibility story is narrower than Leantime's, WeKan
        addresses WCAG compliance rather than neurodiversity as a
        founding design premise. Both are valid, they serve different
        aspects of accessibility.
      risk: low
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-07-25"
  human: []
---

# WeKan, Accessible Kanban Board

## What it is

WeKan is an open source, self-hosted Kanban board. It provides boards, lists, cards, swimlanes, checklists, due dates, member assignment, labels, and a rules engine, all behind a web interface that was recently updated to follow WCAG 2.1 AA guidelines (version 9.46). It is MIT-licensed, self-hostable via Docker, Snap, or Sandstorm, backed by a small team with community contributions, and has been in active development since 2014.

## Why interesting

WeKan is the closest match to the stated need, a four-column Kanban board for a tiny team, without excess. Where Leantime is a full project management platform that includes a Kanban view, WeKan is a Kanban board and not much else. That simplicity is a strength for the current use case.

The accessibility work in version 9.46 is worth specific attention. Drag-and-drop is the single hardest interaction to make accessible in a Kanban tool, and most tools ignore it entirely. WeKan solved it by adding visually hidden, keyboard-focusable "Move card up/down" buttons on cards and "Move list left/right" buttons on list headers, so screen reader and keyboard users can reorder without touching drag-and-drop at all. The same release added visible keyboard focus indicators, skip-to-main-content links, and ARIA landmark roles. This is not a claim of future accessibility, it is shipped, documented code with regression tests.

## Concerns

- **Cognitive accessibility is not the founding premise.** Unlike Leantime, WeKan was not designed from the ground up for neurodivergent users. Its WCAG 2.1 AA work addresses standards compliance, which is necessary but not sufficient for cognitive accessibility. The interface is functional and clean, but it was not shaped by the same design philosophy that produced Leantime's reduced-noise, urgency-grouped, cognitive-load-aware approach.
- **Community size and bus factor.** WeKan is maintained by a smaller team than Leantime. The project has been stable since 2014, which is a strong longevity signal, but current contributor activity and bus factor were not independently confirmed.
- **Meteor/Node.js stack.** WeKan uses the Meteor framework with MongoDB. This is a functional but somewhat dated stack. Meteor's own future as a framework has been a recurring community discussion. This does not affect current functionality but is a longevity consideration.
- **User access controls are available but their depth was not assessed.** WeKan offers board-level and user-level permissions, but the granularity and flexibility were not examined in this review.

## Evaluation

### Inclusive

**Accessibility**

- Rating: Strong
- Evidence: Inferred (documented in release notes with regression tests)
- Notes: Version 9.46 improved accessibility across all pages following WCAG 2.1 AA guidelines. Specific additions include visible keyboard focus indicators (fixing a prior CSS reset that stripped all focus outlines), skip-to-main-content link, ARIA landmark roles (main, navigation, search), and, critically, accessible card and list reordering without drag-and-drop via keyboard-focusable move buttons. Regression tests cover the accessibility additions. This is the most concrete, shipped, and tested accessibility work found in any self-hosted Kanban tool during this review.

**Representation**

- Rating: Unknown
- Evidence: Unknown
- Notes: WeKan's accessibility work is standards-driven (WCAG) rather than representation-driven (neurodiversity as a design lens). No public information was found on team composition or user research methodology. Recorded as Unknown per the metrics document's guidance.

**Resilience**

- Rating: Moderate
- Evidence: Inferred
- Notes: Self-hosted via Docker, Snap, or Sandstorm. Requires a running MongoDB instance and Node.js application server. Does not function offline once the server is down. Comparable to Leantime's resilience profile.

### Agency

**Sovereignty & Privacy**

- Rating: Strong
- Evidence: Inferred
- Notes: MIT-licensed, the most permissive option available. No open core model, no proprietary plugins, no SaaS tier that could shift the boundary. Self-hosted, data stays on your infrastructure in MongoDB. User access controls for boards and individual users. No telemetry was identified in the documentation but was not independently confirmed via network monitoring.

Power-imbalance proxies:

- Exit cost: Unknown, MongoDB data is extractable but no documented user-facing export was confirmed
- Data portability: Unknown, needs hands-on confirmation
- ToS volatility: Not applicable, MIT licence, no vendor relationship
- Pricing asymmetry: Not applicable, no pricing

**Interaction Patterns**

- Rating: Moderate
- Evidence: Inferred
- Notes: A clean, functional Kanban interface without dark patterns. No engagement traps, streaks, or manufactured urgency identified. The interface is straightforward rather than actively supportive, it does not manufacture goals for the user, but it also does not go out of its way to reduce cognitive load the way Leantime does. Honest defaults, easy exit, no confirmshaming.

### Sustainability

**Environment**

- Rating: Moderate
- Evidence: Inferred
- Notes: Node.js/MongoDB web application in Docker, comparable footprint to Leantime's PHP/MySQL stack. No new hardware requirement beyond what Docker hosting already needs.

### Security

- Rating: Unknown
- Evidence: Unknown
- Notes: No security assessment was performed. Version 9.46 release notes mention security fixes (header-login auth, proxy-bleed protections with regression tests), which is a positive signal for security responsiveness. Supply chain exposure (Node.js dependencies, Docker image provenance) was not examined. Requires hands-on trial before any move beyond assess.

**Assessment method:**
Documentation, GitHub repository, release notes (v9.46), and web search only. No installation performed, no network monitor used. Date of review: 2026-07-25.

**Network behaviour:**
- [ ] Outbound connections during normal use — not confirmed
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — none identified, not independently confirmed
- [ ] Licence server contact — none identified, MIT licence
- [ ] Affects networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory — MongoDB data directory, Docker volumes
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service — not identified
- [ ] Stores content in a cloud service by default — no, self-hosted
- [ ] Auto-save or backup features that copy content externally

**Assessment status:** Not assessed, requires hands-on trial.

### The Product or Service Itself

**Longevity**

- Rating: Moderate
- Evidence: Inferred
- Notes: Active since 2014, which is a strong stability signal. Current release activity confirmed (v9.46 release notes found). GitHub stars and fork count were not independently confirmed. The Meteor framework dependency is a longevity question, not a current concern.

**Content endurance**

- Rating: Moderate
- Evidence: Inferred
- Notes: Data lives in MongoDB on your own infrastructure. If WeKan is removed, the data remains in MongoDB and is queryable, but it is application-structured. Same profile as Leantime, an enhancement layer over your own data rather than a container.

**Exit and portability**

- Rating: Unknown
- Evidence: Unknown
- Notes: No documented open-format bulk export was confirmed during this review. MongoDB data is technically extractable. Needs hands-on confirmation, same gap as Leantime.

**Adjustability and support**

- Rating: Moderate
- Evidence: Inferred
- Notes: MIT licence, fork-and-fix path is fully open with no AGPL reciprocal obligation. Meteor/Node.js codebase is readable. Documentation exists. Community is smaller than Leantime's. No commercial support plans were identified.

**Market Position**

- Rating: Unknown
- Evidence: Unknown
- Notes: Not applicable in the same way as Leantime, WeKan is a pure utility tool with no platform dynamics. No market concentration concern, no take rate, no API that others depend on.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Inferred | WCAG 2.1 AA, keyboard reordering without drag-and-drop, tested |
| Multilingual integration | Unknown | Unknown | Not assessed |
| Economic and cognitive accessibility | Strong (economic), Moderate (cognitive) | Inferred | Free, MIT, self-hosted, functional but not neurodiversity-designed |
| Representation | Unknown | Unknown | Standards-driven, not representation-driven |
| Compatibility | Moderate | Inferred | Docker, Snap, Sandstorm, web-based |
| Resilience | Moderate | Inferred | Self-hosted, requires running infrastructure |
| Agency, sovereignty & privacy | Strong | Inferred | MIT, no open core, no vendor relationship |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Unknown / Strong | Unknown / Inferred | Exit unconfirmed, rest no concern |
| Agency, interaction patterns | Moderate | Inferred | Clean, no dark patterns, not actively cognitive-load-aware |
| Environment, direct and indirect | Moderate | Inferred | Node.js/MongoDB, modest footprint |
| Security | Unknown | Unknown | Not assessed, positive signal from documented fixes |
| Longevity | Moderate | Inferred | Active since 2014, Meteor dependency noted |
| Content endurance | Moderate | Inferred | Data in MongoDB on your infrastructure |
| Exit and portability | Unknown | Unknown | Needs hands-on confirmation |
| Adjustability and support | Moderate | Inferred | MIT, readable code, smaller community |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Unknown | Unknown | Not applicable, pure utility |
| Gates | Provisional Pass | | No telemetry identified (unconfirmed), MIT compatible, accessibility strong, exit unknown (check early) |

## Relationship to project

Dev toolchain, specifically the Kanban board for managing work across all Universal Cake projects as described in the UC Kanban Setup document. WeKan is the lighter-weight alternative to Leantime for the same role, trading cognitive accessibility depth for simplicity and a more permissive licence.

## Status notes

- Last reviewed: 2026-07-25
- In assess: documentation and web review complete, automated evaluation recommends adopt at low risk. Same two items as Leantime need hands-on confirmation before any move beyond assess: (1) exit and portability, and (2) security. Additionally, it would be worth testing the WCAG 2.1 AA accessibility claims hands-on with keyboard-only navigation and a screen reader, since this is WeKan's strongest differentiator and the evidence would upgrade from Inferred to Verified.
- What would move it to trial: same as Leantime, install the Docker image, confirm exit/portability and security, run the four-column board against it.
- What would move it to hold: a gate failure on exit/portability, a discovery of undisclosed telemetry, or a decision that Leantime's deeper cognitive accessibility work is worth the additional overhead, making WeKan redundant.

## Links

- https://github.com/wekan/wekan
- https://github.com/wekan/wekan/releases/tag/v9.46
- https://wekan.github.io/

## License

This document, *WeKan, Accessible Kanban Board*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial entry, documentation and web review only, no hands-on trial |

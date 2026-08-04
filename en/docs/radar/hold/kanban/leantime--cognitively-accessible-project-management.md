---
dc:title: "Leantime, Cognitively Accessible Project Management"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "leantime"
  - "kanban"
  - "project-management"
  - "accessibility"
  - "cognitive-accessibility"
  - "neurodiversity"
dc:description: "Radar entry assessing Leantime, an AGPL-3.0-licensed project management platform designed from the ground up for cognitive accessibility, with ADHD, autism, and dyslexia as first-class design constraints rather than afterthoughts."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-25"
dcterms:modified: "2026-07-25"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://leantime.io/"
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, wekan--accessible-kanban-board"
dc:identifier: "leantime--cognitively-accessible-project-management"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
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
        Strong alignment with Universal Cake's foundational values.
        Cognitive accessibility is the founding design premise, not a
        retrofit. AGPL-3.0 licence is compatible. Self-hostable via
        Docker with no per-seat fees for the open source version. No
        gate failures identified. The tool is larger than the current
        need (a lightweight Kanban board), but the cognitive
        accessibility work, the neurodiversity-informed UX, and the
        values alignment outweigh the overhead. The open core model
        (some features are proprietary marketplace plugins) is a
        sovereignty concern worth monitoring but does not currently
        fail a gate, the AGPL core includes Kanban, task management,
        and the My Work dashboard.
      risk: low
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-07-25"
  human: []
---

# Leantime, Cognitively Accessible Project Management

## What it is

Leantime is an open source project management platform designed for non-project managers, built from the ground up with ADHD, dyslexia, and autism as first-class design constraints. It provides Kanban boards, Gantt timelines, calendars, table views, time tracking, sprint planning, goal tracking, wikis, and retrospectives, all behind a "My Work" dashboard that consolidates tasks across projects and surfaces only what matters today. It is self-hostable via Docker, licensed under AGPL-3.0 (open core model), backed by Leantime Inc. (US, VC-backed via Techstars), with roughly 10,600 GitHub stars, active development as of July 2026, and a PHP/Laravel/MySQL stack.

## Why interesting

Two reasons, one practical and one philosophical.

Practically, Universal Cake needs a lightweight Kanban board for a tiny team working across multiple repositories. Leantime is more than that, but it includes a clean Kanban implementation and does not require using the features you don't need. It runs on Docker, which is already in the toolchain.

Philosophically, Leantime's founding design premise, cognitive accessibility as a structural commitment rather than a compliance checkbox, is the closest thing to a worked example of what Universal Cake's own Evaluation Metrics describe under Inclusive. The designers optimised for cognitive efficiency rather than maximum feature density. They use emoji scales instead of numerical priority systems to reduce cognitive load. They built a "My Work" view that groups tasks by urgency (overdue, due this week, due later) and eliminates visual noise. These are not accessibility features bolted onto a conventional tool, they are the tool's reason for existing. That alignment is worth more than the feature list.

## Concerns

- **Open core model.** The AGPL-3.0 core includes project management, Kanban, task tracking, and the My Work dashboard. Some features, strategy management, program management, whiteboards, custom fields, are proprietary marketplace plugins sold by Leantime Inc. This is a sovereignty concern under the metrics, the boundary between free core and paid plugins can shift. Monitor for gate-relevant changes on version upgrades.
- **Heavier than needed.** A four-column Kanban board for one to three people does not require Gantt charts, sprint planning, goal tracking, wikis, or retrospectives. The tool is larger than the stated need. Whether the cognitive accessibility benefits justify the overhead is a judgment call, not a gate concern.
- **VC-backed.** Leantime Inc. is backed by Techstars. VC funding introduces incentive pressure toward monetisation that may eventually conflict with the open core's scope. This is not a current concern, but it is a re-review trigger per the metrics document.
- **PHP/MySQL stack.** Adds a technology dependency outside Universal Cake's current Python/Perl/static-site toolchain. Docker encapsulation mitigates this, but it is still a dependency.
- **Accessibility claims are Claimed, not Verified.** The neurodiversity-informed design is well-documented and appears to be the genuine founding premise, but this review did not perform hands-on testing with assistive technology. All accessibility ratings below are tagged Inferred or Claimed accordingly.

## Evaluation

### Inclusive

**Accessibility**

- Rating: Strong
- Evidence: Claimed (vendor), Inferred (documentation review)
- Notes: Cognitive accessibility is the founding design premise, not an add-on. The interface was designed with ADHD, autism, and dyslexia as named constraints. Features include a My Work dashboard reducing cognitive load, emoji-based priority scales, clear task grouping by urgency, minimal visual noise, strong contrast, and multiple view options (Kanban, table, list, calendar) so users can choose what suits their cognitive style. WCAG conformance level was not independently confirmed in this review. No gate concern, the tool clearly increases rather than decreases the number of people who can access content.

**Representation**

- Rating: Moderate
- Evidence: Inferred
- Notes: The project's stated mission centres neurodivergent users as the primary design audience, which is an unusually direct instance of representation driving design decisions. The CTO has written publicly about building for cognitive accessibility as a core value. Whether the design/engineering team itself includes neurodivergent members was not confirmed, the public documentation treats neurodiversity as a design lens rather than documenting team composition.

**Resilience**

- Rating: Moderate
- Evidence: Inferred
- Notes: Self-hosted via Docker, operates independently of Leantime Inc.'s SaaS infrastructure once deployed. Requires a running MySQL database and PHP application server, so it depends on more infrastructure than a static file or a single script. Does not function offline once the server is down.

### Agency

**Sovereignty & Privacy**

- Rating: Moderate
- Evidence: Inferred
- Notes: Self-hostable, AGPL-3.0 core, data stays on your own infrastructure. The open core boundary is the standing concern, some features require proprietary plugins, and the boundary can shift. No telemetry was identified in the open source version during this review but was not independently confirmed via network monitoring. Exit cost is moderate, data lives in a MySQL database, export to open formats was not confirmed. LDAP and OIDC authentication support reduces lock-in on the identity side.

Power-imbalance proxies:

- Exit cost: Moderate, MySQL data is extractable but no documented open-format bulk export was confirmed
- Data portability: Unknown, needs hands-on confirmation
- ToS volatility: Not applicable to the self-hosted AGPL version, relevant only to the SaaS tier
- Pricing asymmetry: Low concern for the self-hosted version, public pricing exists for the SaaS tier, plugin marketplace pricing is published

**Interaction Patterns**

- Rating: Strong
- Evidence: Inferred
- Notes: The My Work dashboard is a worked example of several supportive patterns from the metrics document. Honest defaults (surfaces today's work, not everything). Natural stopping points (groups tasks by urgency with clear boundaries). Quiet by default (no engagement traps, streaks, or manufactured urgency in the documented interface). The tool appears designed to help you accomplish the goal you arrived with rather than manufacturing new goals. No dark patterns were identified in the documented interface.

### Sustainability

**Environment**

- Rating: Moderate
- Evidence: Inferred
- Notes: A PHP/MySQL web application running in Docker is a modest but non-trivial resource footprint, heavier than a static file or a CLI script, lighter than an Electron app or a full Java stack. No new hardware requirement beyond what Docker hosting already needs.

### Security

- Rating: Unknown
- Evidence: Unknown
- Notes: No security assessment was performed. The GitHub repository has a security contact (noted in the repository metadata). HIPAA-eligible and GDPR-ready claims are made for the self-hosted version. Supply chain exposure (PHP dependencies, Docker image provenance) was not examined. This section requires a hands-on trial before any move beyond assess.

**Assessment method:**
Documentation, GitHub repository, project homepage, third-party reviews, and web search only. No installation performed, no network monitor used. Date of review: 2026-07-25.

**Network behaviour:**
- [ ] Outbound connections during normal use — not confirmed, self-hosted version should be contained but needs verification
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — none identified, not independently confirmed
- [ ] Licence server contact — none identified
- [ ] Affects networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory — MySQL data directory, Docker volumes
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service — not identified, not confirmed
- [ ] Stores content in a cloud service by default — no, self-hosted
- [ ] Auto-save or backup features that copy content externally

**Assessment status:** Not assessed, requires hands-on trial.

### The Product or Service Itself

**Longevity**

- Rating: Strong
- Evidence: Inferred
- Notes: Active development, last updated July 2026, over 10,600 GitHub stars, 1,073 forks, packaged for Docker, listed on Packagist, backed by Leantime Inc. with Techstars funding. The VC backing is a double-edged signal, it provides current stability and introduces long-term incentive risk, both recorded.

**Content endurance**

- Rating: Moderate
- Evidence: Inferred
- Notes: Content (tasks, boards, project data) lives in a MySQL database on your own infrastructure. If Leantime is removed, the data remains in MySQL and is queryable, but it is application-structured rather than human-readable without the application layer. This is an enhancement layer over your own data rather than a container that takes data with it, but the enhancement is heavy enough that the data is not trivially portable.

**Exit and portability**

- Rating: Unknown
- Evidence: Unknown
- Notes: No documented open-format bulk export was confirmed during this review. MySQL data is technically extractable. Whether Leantime provides a user-facing export in open formats (CSV, JSON, or equivalent) needs hands-on confirmation. No gate failure identified yet, but this is the metric most likely to fail on closer inspection and should be checked early in any trial.

**Adjustability and support**

- Rating: Strong
- Evidence: Inferred
- Notes: AGPL-3.0 licence, fork-and-fix path is open. PHP/Laravel codebase is readable. Documentation exists. Community Discord is active. Commercial support plans are available. Plugin architecture allows extension without forking. MCP server integration exists (leantime-mcp), which is notable given Universal Cake's tooling context.

**Market Position**

- Rating: Moderate
- Evidence: Inferred
- Notes: Leantime occupies a niche, cognitively accessible open source project management, that has no dominant incumbent. It is not infrastructure that other projects depend on, so platform risk is low. Contributor concentration sits primarily with Leantime Inc., the bus factor is a real concern if the company pivots, mitigated by the AGPL licence allowing community forks.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Claimed / Inferred | Founding design premise, not a retrofit |
| Multilingual integration | Unknown | Unknown | Not assessed |
| Economic and cognitive accessibility | Strong | Inferred | Free self-hosted, cognitive accessibility is core |
| Representation | Moderate | Inferred | Neurodivergent users as primary design audience |
| Compatibility | Moderate | Inferred | Docker, web-based, browser-agnostic |
| Resilience | Moderate | Inferred | Self-hosted, requires running infrastructure |
| Agency, sovereignty & privacy | Moderate | Inferred | Self-hosted AGPL, open core boundary is the concern |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Unknown / Moderate | Unknown / Inferred | Exit and portability unconfirmed, rest low concern |
| Agency, interaction patterns | Strong | Inferred | Supportive patterns throughout documented interface |
| Environment, direct and indirect | Moderate | Inferred | Docker/PHP/MySQL, modest footprint |
| Security | Unknown | Unknown | Not assessed |
| Longevity | Strong | Inferred | Active, funded, growing community |
| Content endurance | Moderate | Inferred | Data in MySQL on your infrastructure, not trivially portable |
| Exit and portability | Unknown | Unknown | Needs hands-on confirmation, potential gate risk |
| Adjustability and support | Strong | Inferred | AGPL, readable code, active community, plugin architecture |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Inferred | Niche position, contributor concentration with Leantime Inc. |
| Gates | Provisional Pass | | No telemetry identified (unconfirmed), AGPL-3.0 compatible, accessibility strong, exit unknown (check early) |

## Relationship to project

Dev toolchain, specifically the Kanban board for managing work across all Universal Cake projects as described in the UC Kanban Setup document. Also a potential reference case for the Virtuous Iteration audit, Leantime is a worked example of cognitive accessibility as a founding design premise, the kind of outcome the audit's questions aim to produce.

## Status notes

- Last reviewed: 2026-07-25
- In assess: documentation and web review complete, automated evaluation recommends adopt at low risk. Two items need hands-on confirmation before any move beyond assess: (1) exit and portability, whether a user-facing open-format bulk export exists, this is the most likely gate risk, and (2) security, network behaviour and supply chain exposure of the Docker image.
- What would move it to trial: a decision to install the Docker image, stand up a test instance, confirm exit/portability and security, and run the four-column Kanban board from the UC Kanban Setup document against it for a defined period.
- What would move it to hold: a gate failure on exit/portability (no usable export), a discovery of undisclosed telemetry, or a decision that the tool is too heavy for the current need and that WeKan or a markdown file covers it.

## Links

- https://leantime.io/
- https://github.com/Leantime/leantime
- https://leantime.io/open-source-project-management-for-adhd-why-we-built-leantime-for-neurodivergent-productivity/
- https://packagist.org/packages/leantime/leantime

## License

This document, *Leantime, Cognitively Accessible Project Management*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial entry, documentation and web review only, no hands-on trial |

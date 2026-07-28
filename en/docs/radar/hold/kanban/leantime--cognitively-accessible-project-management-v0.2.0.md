---
dcterms:title: "Leantime, Cognitively Accessible Project Management"
dcterms:version: "0.2.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "radar"
  - "leantime"
  - "kanban"
  - "project-management"
  - "accessibility"
  - "cognitive-accessibility"
  - "neurodiversity"
dcterms:description: "Radar entry assessing Leantime, an AGPL-3.0-licensed project management platform designed for cognitive accessibility, with ADHD, autism, and dyslexia as first-class design constraints. Full evaluation against Universal Cake Evaluation Metrics v0.3.1. Moved to hold, WeKan adopted for the current need."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-07-25"
dcterms:modified: "2026-07-25"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: "https://leantime.io/"
dcterms:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, wekan--accessible-kanban-board"
dcterms:identifier: "leantime--cognitively-accessible-project-management"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-07-25"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Full evaluation against Universal Cake Evaluation Metrics v0.3.1.
      Resolved exit/portability from Unknown to Weak, export is CSV
      only for timesheets, tasks, and milestones, no JSON board export,
      no interop export paths. This is the finding that most clearly
      differentiates Leantime from WeKan. Security upgraded from
      Unknown to Weak based on two recent CVEs (privilege escalation
      CVE-2026-15510 and OIDC CSRF CVE-2026-59713 in v3.8.0), a
      published SECURITY.md and responsible disclosure policy, but a
      noted vendor non-responsiveness on one disclosure. Moved to hold,
      WeKan adopted for the current Kanban board need. Hold condition:
      reconsider if team size exceeds the scope of a standalone Kanban
      board or if cognitive accessibility becomes a gate criterion.
  - version: "0.1.0"
    date: "2026-07-25"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: "Initial entry, documentation and web review only."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: hold
      reasoning: >
        Cognitive accessibility is genuinely Strong and is the founding
        design premise, which aligns with UC values more closely than
        any other tool evaluated. However, exit/portability is Weak
        (CSV export only, no JSON board export, no interop paths, open
        feature request since 2020), security is Weak (two recent CVEs
        in v3.8.0 including privilege escalation, vendor
        non-responsiveness on one disclosure), the open-core model
        with VC backing introduces sovereignty concerns, and the tool
        is significantly heavier than the current need. WeKan passes
        all four gates cleanly and is a closer fit for the stated
        requirement. Recommend hold rather than rejected, Leantime's
        cognitive accessibility work is genuinely valuable and the
        right answer if UC's needs outgrow a standalone Kanban board.
      risk: moderate
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-07-25"
  human: []
---

# Leantime, Cognitively Accessible Project Management

## What it is

Leantime is an open source project management platform designed for non-project managers, built from the ground up with ADHD, dyslexia, and autism as first-class design constraints. It provides Kanban boards, Gantt timelines, calendars, table views, time tracking, sprint planning, goal tracking (OKR), Lean Canvas, SWOT boards, wikis, retrospectives, and idea boards, all behind a "My Work" dashboard that consolidates tasks across projects and surfaces only what matters today. It is self-hostable via Docker, licensed under AGPL-3.0 (open core model with proprietary marketplace plugins), backed by Leantime Inc. (US, VC-backed via Techstars), with roughly 10,600 GitHub stars, active development as of July 2026, and a PHP/Laravel/MySQL stack. Translated to 20+ languages via Crowdin with a dyslexia-friendly font toggle.

## Why interesting

Leantime's founding design premise, cognitive accessibility as a structural commitment rather than a compliance checkbox, is the closest thing to a worked example of what Universal Cake's own Evaluation Metrics describe under Inclusive. The designers optimised for cognitive efficiency rather than maximum feature density. They use emoji scales instead of numerical priority systems to reduce cognitive load. They built a "My Work" view that groups tasks by urgency (overdue, due this week, due later) and eliminates visual noise. The CTO has written publicly about building for cognitive accessibility as a core value, treating it as a different set of trade-offs rather than a feature to bolt on.

This alignment is philosophically stronger than any other tool on the radar. It is also, as this evaluation documents, not sufficient to overcome the practical concerns that emerge under the other metrics pillars.

## Concerns

- **Exit/portability is the critical weakness.** Export is limited to CSV for timesheets, tasks, and milestones. No JSON board export exists. No interop export paths to other tools exist. A feature request for comprehensive export capability has been open since May 2020 (GitHub issue #261) without resolution. This is the finding that most clearly separates Leantime from WeKan, which exports to JSON, CSV, TSV, Excel, PDF, and has interop paths with seven external tools.
- **Two recent security vulnerabilities.** A broken access control flaw (CWE-862, CVE-2026-15510) in v3.8.0 allowed any authenticated low-privilege user to escalate to Owner via the JSON-RPC API. An OIDC login CSRF vulnerability (CVE-2026-59713) in the `verifyState()` method unconditionally returned true without validating state parameters. On the privilege escalation disclosure, the vendor was reported as not responding.
- **Open core model.** The AGPL-3.0 core includes project management, Kanban, and the My Work dashboard. Strategy management, program management, whiteboards, custom fields, and other features are proprietary marketplace plugins. The boundary between free and paid can shift.
- **VC-backed.** Leantime Inc. is backed by Techstars. VC funding introduces incentive pressure toward monetisation that may eventually conflict with the open core's scope.
- **Heavier than needed.** A four-column Kanban board for one to three people does not require Gantt charts, sprint planning, OKR tracking, Lean Canvas, SWOT boards, wikis, or retrospectives.

## Evaluation

Evaluated against Universal Cake Evaluation Metrics v0.3.1. Each rating uses the scale Strong / Moderate / Weak / Unknown, tagged Verified / Inferred / Claimed. Stakeholder lens noted where it changes the answer.

### Inclusive

**Accessibility, alternative methods of interacting with content**

Lens: user. **[GATE]** if content becomes unreachable for people using assistive technology.

- Rating: Strong
- Evidence: Claimed (vendor), Inferred (documentation review)
- Notes: Cognitive accessibility is the founding design premise. The interface was designed with ADHD, autism, and dyslexia as named constraints. Features include a My Work dashboard reducing cognitive load, emoji-based priority scales, clear task grouping by urgency (overdue, due this week, due later), minimal visual noise, strong contrast, dark/light mode, dyslexia-friendly font toggle, and multiple view options (Kanban, table, list, calendar, Gantt) so users can choose what suits their cognitive style. WCAG conformance level was not independently confirmed. No specific WCAG success criteria are cited in the documentation the way WeKan's v9.46 release notes cite them. The accessibility story is strong on cognitive accessibility and undocumented on standards compliance, the reverse of WeKan's profile. No keyboard-specific navigation or screen reader-specific features were documented.
- Gate status: **Provisional Pass**. Content appears reachable, but the absence of documented WCAG compliance means this gate passes on inference rather than evidence. A hands-on trial with assistive technology would be needed to confirm.

**Multilingual integration**

Lens: user, community.

- Rating: Moderate
- Evidence: Inferred
- Notes: Translated to 20+ languages via Crowdin. Adding a language is feasible for the community via the Crowdin platform, which is a data-driven process rather than a code change. Significantly fewer languages than WeKan's 154. The localisation architecture is adequate but not exceptional.

**Economic accessibility**

Lens: user.

- Rating: Moderate
- Evidence: Inferred
- Notes: Free self-hosted version under AGPL-3.0, no per-seat fees for the open source edition. However, requires Docker and a MySQL database, which is a higher infrastructure bar than WeKan's standalone bundle. 1 GB RAM is not sufficient, the documentation recommends 4 GB for production use. Does not offer a no-Docker, no-external-database deployment path. The infrastructure requirements exclude users on low-end hardware or without container runtime experience.

**Cognitive accessibility**

Lens: user.

- Rating: Strong
- Evidence: Claimed (vendor), Inferred (documentation and CTO's published writing)
- Notes: This is Leantime's strongest differentiator. The My Work dashboard, emoji priority scales, urgency grouping, reduced visual noise, dyslexia-friendly font toggle, and the overall design philosophy of optimising for cognitive efficiency rather than feature density represent a genuine, founding commitment to cognitive accessibility. The CTO has published detailed writing on building for neurodivergent users as a core value. This is not a feature bolted on, it is the reason the tool exists.

**Compatibility**

Lens: owner, user.

- Rating: Moderate
- Evidence: Inferred
- Notes: Requires Docker and MySQL. Web-based interface works in any modern browser. No standalone bundle, no alternative to Docker for self-hosting. amd64 Docker image confirmed, arm64 and other architectures not independently confirmed.

### Representation

Lens: community, user.

- Rating: Moderate
- Evidence: Inferred
- Notes: The project's stated mission centres neurodivergent users as the primary design audience, which is an unusually direct instance of representation driving design decisions. The CTO has written publicly about building for cognitive accessibility as a core value. Whether the design/engineering team itself includes neurodivergent members was not confirmed from public information. The design philosophy is clearly informed by lived experience of neurodivergent needs, whether through team composition or through deliberate, sustained engagement with those communities. Rated Moderate rather than Unknown because the evidence of representation-driven design, while not formally documented as team composition data, is visible in the product itself and in the public writing.

### Resilience

Lens: owner, user.

- Rating: Moderate
- Evidence: Inferred
- Notes: Self-hosted via Docker, operates independently of Leantime Inc.'s SaaS infrastructure once deployed. Requires a running MySQL database and PHP application server, so it depends on more infrastructure than WeKan's standalone bundle. Does not function offline once the server is down. No standalone deployment path exists. The Docker dependency means resilience is contingent on the container runtime being available.

### Agency

**Sovereignty & Privacy**

Lens: owner, user.

- Rating: Moderate
- Evidence: Inferred
- Notes: Self-hostable, AGPL-3.0 core, data stays on your own infrastructure. The open core boundary is the standing concern: some features require proprietary plugins sold by Leantime Inc., and the boundary can shift. No telemetry was identified in the open source version but was not independently confirmed via network monitoring. LDAP and OIDC authentication support is available. The OIDC implementation had a CSRF vulnerability (CVE-2026-59713), now patched.
- Power-imbalance proxies (vendor↔user):
  - **Exit cost:** High. CSV export only, limited to timesheets, tasks, and milestones. No JSON board export. No interop paths to other tools. Migrating away would require manual reconstruction or database-level extraction. Estimated cost: days, not hours.
  - **Data portability:** Weak. No complete, documented, machine-readable export exists for the full project data model. The feature request (GitHub issue #261, May 2020) remains open. MySQL data is technically extractable but application-structured.
  - **Terms-of-service volatility:** Not applicable to the self-hosted AGPL version. Relevant only to the SaaS tier.
  - **Pricing asymmetry:** Low concern for the self-hosted version. Public pricing exists for the SaaS tier. Plugin marketplace pricing is published.
- Reversibility test: "If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?" Answer: export what CSV can capture (timesheets, tasks, milestones), manually recreate board structure and project metadata in a new tool, or extract and transform MySQL data directly. Estimated cost: days of work for a small project, potentially weeks for a larger one.
- Gate status: **Pass** (narrowly). Telemetry gate not failed, data technically exists in MySQL on your infrastructure. However, the practical exit cost is high enough that this is the weakest pass of any gate across both evaluated tools.

**Interaction Patterns**

Lens: user.

- Rating: Strong
- Evidence: Inferred
- Notes: Evaluated against the metrics document's supportive/dark pattern pairs:
  - Honest defaults / preselection tricks: the My Work dashboard defaults to showing today's work, not everything. Defaults serve the user. Pass.
  - Easy exit / roach motel: structural exit is hard (see exit cost above), but moment-to-moment interaction exit is easy, you can close the browser. Mixed.
  - Forgiveness / confirmshaming: the interface is forgiving of errors, uses plain language, and does not blame. Pass.
  - Natural stopping points / engagement traps: the urgency grouping (overdue, due this week, due later) creates natural boundaries. No infinite scroll, no autoplay, no engagement traps. Pass.
  - Quiet by default / attention farming: no manufactured urgency, no streaks, no attention farming. Integrations with Slack/Mattermost/Zulip/Discord are opt-in. Pass.
  - Plain asking / consent walls: no consent walls, no trick wording. Pass.
  - Visible costs / drip pricing: the open source version is free. Plugin pricing is published. Pass.
  - Leaves you whole / lock-in: this is where interaction patterns and sovereignty intersect, data does not exit easily. The interaction layer is supportive, the structural layer creates lock-in. Record the conflict.
  - Supplementary: the tool helps you accomplish the goal you arrived with. The cognitive-load-aware design is the strongest example of supportive interaction patterns found in any tool on this radar. The My Work dashboard, emoji priorities, and urgency grouping are a worked positive anchor for what the metrics document describes.

### Sustainability

**Environment, direct and indirect impacts**

Lens: environment, owner, user.

- Rating: Moderate
- Evidence: Inferred
- Notes: PHP/Laravel/MySQL web application in Docker. Recommended 4 GB RAM for production, heavier than WeKan's 1 GB minimum. No standalone bundle, Docker is the only self-hosted path. Does not require new hardware for most users with existing server infrastructure. Bandwidth is served once per page load and cached.

### Security

Lens: owner, user.

- Rating: Weak
- Evidence: Inferred
- Notes:
  - **What data does it collect, and where does that data rest?** Project, task, timesheet, and user data in a MySQL database on your own infrastructure. File storage on local filesystem or S3-compatible bucket. No data sent to external services by the open source version.
  - **How are vulnerabilities reported, and is there a published security policy?** Yes. A SECURITY.md exists in the repository. A responsible disclosure policy is published on the website. Reports are directed to GitHub Security Advisory form. The policy states a 3-business-day response time and a two-version delay before public disclosure.
  - **How quickly are security patches released?** Two recent CVEs in version 3.8.0 are documented: a privilege escalation (CVE-2026-15510, CWE-862) allowing any authenticated user to escalate to Owner via the JSON-RPC API, and an OIDC login CSRF (CVE-2026-59713) where state verification unconditionally returned true. On the privilege escalation disclosure, the advisory notes "the vendor was contacted early about this disclosure but did not respond in any way." This non-responsiveness is a concerning signal against the stated 3-day response policy.
  - **What is the supply chain exposure?** PHP/Laravel dependency tree, npm dependencies for frontend build, MySQL. Not independently audited. Docker image provenance not examined.
  - **Does adding it expand or shrink the attack surface?** Adding a PHP web application with a MySQL database and optional S3 storage expands the attack surface. The JSON-RPC API is the historically riskiest surface, as demonstrated by the privilege escalation CVE.
  - **Assessment method:** Documentation, GitHub repository, SECURITY.md, responsible disclosure policy, GitHub Advisory Database, third-party security research (Bytium). No installation performed, no network monitor used. Date: 2026-07-25.
  - Gate status: **Pass**. No telemetry or content exposure that cannot be disabled. Security concerns are severity and responsiveness issues, not gate failures.

**Network behaviour:**
- [ ] Outbound connections during normal use — not confirmed for the self-hosted version
- [ ] Update checks — not identified
- [ ] Telemetry or usage data — none identified, not independently confirmed
- [ ] Licence server contact — none identified
- [x] Affects networking or items created that will be transferred over a network — integrations with Slack, Mattermost, Zulip, Discord are opt-in
- [ ] Helps ensure that documents passed via network are clean and compliant

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content — MySQL data directory, Docker volumes
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service — not identified in the open source version
- [ ] Stores content in a cloud service by default — no, self-hosted, S3 storage is opt-in
- [ ] Auto-save or backup features that copy content externally

**Assessment status:** Provisional, Inferred. Hands-on trial with network monitoring recommended.

### The Product or Service Itself

**Longevity**

Lens: owner, community.

- Rating: Strong
- Evidence: Inferred
- Notes: Active development, last updated July 2026, over 10,600 GitHub stars, 1,073 forks, packaged for Docker, listed on Packagist and Crowdin. Backed by Leantime Inc. with Techstars funding, which provides current stability. The VC backing is a double-edged signal: it provides resources for continued development and introduces long-term incentive risk toward monetisation.

**Content endurance**

Lens: owner, community.

- Rating: Moderate
- Evidence: Inferred
- Notes: Content (tasks, boards, project data, wikis, timesheets) lives in a MySQL database on your own infrastructure. If Leantime is removed, the data remains in MySQL and is queryable but application-structured. The limited export capability (CSV only for specific data types) means extracting content in human-readable form before removal requires either CSV export of what it supports or direct database work for the rest. Content is authored inside the tool (wikis, tasks, goals) rather than merely enhanced by it, which means removal affects a larger surface area than a pure Kanban board would.

**Exit and portability**

Lens: owner, user. **[GATE]** if no exit exists.

- Rating: Weak
- Evidence: Inferred
- Notes: Export exists but is limited. CSV export is available for timesheets, tasks, and milestones. No JSON board export. No comprehensive project export. No interop export paths to other tools. A feature request for export capability (GitHub issue #261) has been open since May 2020 without resolution. The MySQL database is technically extractable, but the data is application-structured and would require significant transformation to be usable in another tool. No documented migration path to a successor exists. CSV import from Trello is available for migration in, but the reverse path (migration out) is not supported.
- Contrast with WeKan: WeKan exports to JSON (with base64 attachments), CSV, TSV, Excel, PDF, and has import/export interop with Kanboard, NextCloud Deck, OpenProject, GitHub, GitLab, Gitea, and Forgejo.
- Gate status: **Pass** (narrowly). Data exists on your infrastructure in MySQL, which is technically extractable. This is not "no exit," it is "expensive exit." The gate does not fail, but this is the single weakest result across both evaluated tools and the primary reason for the hold recommendation.

**Adjustability and support**

Lens: owner, community.

- Rating: Strong
- Evidence: Inferred
- Notes: AGPL-3.0 licence, fork-and-fix path is open (with reciprocal obligation). PHP/Laravel codebase is readable by any PHP developer. Documentation exists. Community Discord is active. Commercial support plans are available. Plugin architecture allows extension without forking. MCP server integration exists (leantime-mcp), notable given UC's tooling context.

**Market Position**

Lens: community, owner.

- Rating: Moderate
- Evidence: Inferred
- Notes: Leantime occupies a niche, cognitively accessible open source project management, that has no dominant incumbent. It is not infrastructure that other projects depend on.
  - Market concentration: niche position, no dominant market share in its category.
  - Take rate: none for the open source version. Plugin marketplace exists.
  - API stability and deprecation history: JSON-RPC API is the primary programmatic interface. The privilege escalation CVE was in the API layer.
  - Forkability: Moderate. AGPL-3.0 allows forking with reciprocal obligation. The open-core model means a fork of the AGPL core would lack the proprietary plugins. Community is smaller than WeKan's.
  - Contributor concentration: primarily Leantime Inc. Bus factor concern, mitigated by the AGPL licence.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Claimed / Inferred | Founding design premise, cognitive accessibility first, WCAG compliance undocumented |
| Multilingual integration | Moderate | Inferred | 20+ languages via Crowdin |
| Economic and cognitive accessibility | Strong (cognitive), Moderate (economic) | Inferred | Cognitive accessibility founding premise, Docker + MySQL required, 4 GB RAM recommended |
| Representation | Moderate | Inferred | Neurodivergent users as primary audience, team composition not confirmed |
| Compatibility | Moderate | Inferred | Docker required, web-based, no standalone bundle |
| Resilience | Moderate | Inferred | Self-hosted Docker, depends on MySQL and container runtime |
| Agency, sovereignty & privacy | Moderate | Inferred | AGPL, self-hosted, open-core boundary concern |
| Agency, power-imbalance proxies | Weak | Inferred | High exit cost, weak data portability, no ToS concern, no pricing concern |
| Agency, interaction patterns | Strong | Inferred | Best-in-class cognitive-load-aware supportive patterns |
| Environment, direct and indirect | Moderate | Inferred | Docker/PHP/MySQL, 4 GB recommended |
| Security | Weak | Inferred | Two recent CVEs in v3.8.0, vendor non-responsiveness on one, published SECURITY.md |
| Longevity | Strong | Inferred | Active, funded, growing community |
| Content endurance | Moderate | Inferred | MySQL data on your infrastructure, limited export |
| Exit and portability | Weak | Inferred | CSV only for specific data types, no JSON export, open feature request since 2020 |
| Adjustability and support | Strong | Inferred | AGPL, readable code, active community, plugin architecture, MCP server |
| Market position | Moderate | Inferred | Niche, contributor concentration with Leantime Inc. |
| Gates | **Pass** (all four, exit narrowly) | | Accessibility floor provisionally cleared, AGPL compatible, no telemetry, exit exists via MySQL but is expensive |

## Relationship to project

Dev toolchain, originally assessed as a Kanban board candidate. Leantime's cognitive accessibility work makes it a reference case for what UC's own Virtuous Iteration questions aim to surface. On hold as a Kanban tool because WeKan better fits the current need, but the cognitive accessibility alignment means Leantime remains the first candidate to reconsider if UC's project management needs outgrow a standalone Kanban board.

## Status notes

- Last reviewed: 2026-07-25
- In hold: WeKan adopted for the current Kanban board need. Leantime is on hold because:
  - Exit/portability is Weak (CSV only, no JSON export, no interop paths), the single strongest reason
  - Security shows two recent CVEs with a vendor non-responsiveness signal
  - The tool is significantly heavier than the current requirement
  - The open-core model introduces sovereignty monitoring overhead
- What would need to change for reconsideration:
  - Team size exceeds the scope of a standalone Kanban board and requires sprint planning, Gantt, OKR, wikis, or retrospectives
  - Cognitive accessibility becomes a gate criterion rather than a scored metric
  - Leantime ships comprehensive export (the open issue #261) that brings exit/portability to parity with WeKan
  - The security responsiveness concern is resolved by evidence of timely response to subsequent disclosures
- Reconsideration re-enters at assess per the lifecycle document.

## Links

- https://leantime.io/
- https://github.com/Leantime/leantime
- https://leantime.io/open-source-project-management-for-adhd-why-we-built-leantime-for-neurodivergent-productivity/
- https://leantime.io/responsible-disclosure-policy/
- https://github.com/Leantime/leantime/issues/261
- https://github.com/Leantime/leantime/security

## License

This document, *Leantime, Cognitively Accessible Project Management*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Hold | Full evaluation against UC Evaluation Metrics v0.3.1, exit/portability Weak, security Weak, moved to hold, WeKan adopted |
| 0.1.0 | Draft | Initial entry, documentation and web review only |

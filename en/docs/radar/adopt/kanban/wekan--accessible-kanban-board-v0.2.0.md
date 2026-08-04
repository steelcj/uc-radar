---
dc:title: "WeKan, Accessible Kanban Board"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "wekan"
  - "kanban"
  - "project-management"
  - "accessibility"
  - "wcag"
dc:description: "Radar entry assessing WeKan, an MIT-licensed self-hosted Kanban board with WCAG 2.1 AA accessibility, keyboard-driven card reordering without drag-and-drop, export to JSON/CSV/TSV/Excel/PDF, and a standalone bundle requiring no container runtime."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-25"
dcterms:modified: "2026-07-25"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/wekan/wekan"
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, leantime--cognitively-accessible-project-management"
dc:identifier: "wekan--accessible-kanban-board"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
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
      Resolved all Unknown ratings from v0.1.0 except Representation
      and cognitive accessibility, which remain Unknown and Moderate
      respectively by nature rather than by lack of research. Exit and
      portability upgraded from Unknown to Strong based on confirmed
      export to JSON, CSV, TSV, Excel, PDF, and interop with Kanboard,
      NextCloud Deck, OpenProject, GitHub, GitLab, Gitea, and Forgejo.
      Security upgraded from Unknown to Moderate based on the documented
      ProxyBleed fix with CVE, regression tests, and responsible
      disclosure process. Multilingual integration confirmed at 154
      languages via Transifex with community-editable data files.
      Standalone bundle (no Docker required) noted as a significant
      finding for resilience and economic accessibility. All gate
      criteria now assessed, all pass.
  - version: "0.1.0"
    date: "2026-07-25"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: "Initial entry, documentation and web review only."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: adopt
      reasoning: >
        Strong fit for Universal Cake. Accessibility is the strongest
        pillar, WCAG 2.1 AA with shipped, tested, regression-covered
        features including keyboard-only card/list reordering, skip
        links, ARIA landmarks, focus management, and accessible modal
        handling. All four gate criteria pass, MIT licence compatible,
        no telemetry identified, accessibility floor cleared, exit
        confirmed via multiple open formats. The standalone bundle
        (Node.js + FerretDB + SQLite, no Docker required) scores
        highest possible on economic accessibility and resilience.
        Export to JSON, CSV, TSV, Excel, PDF, plus interop with
        seven external tools, resolves the exit/portability unknown
        that was the most likely gate risk. 154 languages via
        community-editable Transifex files. 21,000 GitHub stars,
        active since 2014, MIT licence, no open core, no vendor
        relationship. The main limitation is that cognitive
        accessibility is standards-driven (WCAG) rather than
        neurodiversity-informed (contrast with Leantime), which is
        adequate but not actively advancing that value.
      risk: low
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-07-25"
  human: []
---

# WeKan, Accessible Kanban Board

## What it is

WeKan is an open source, self-hosted Kanban board application. It provides boards, lists, cards, swimlanes, checklists, due dates, member assignment, labels, WIP limits, card aging, a rules/automation engine, and import/export across multiple formats and tools, all behind a web interface that follows WCAG 2.1 AA guidelines as of version 9.46. It is MIT-licensed, has been in active development since 2014 with over 21,000 GitHub stars and translation to 154 languages. It is deployable via Docker, Snap, Sandstorm, Kubernetes, or as a standalone Windows/Mac/Linux bundle requiring no container runtime.

## Why interesting

Two reasons converge here.

First, accessibility. Kanban boards are built around drag-and-drop, which is the single hardest web interaction to make accessible. Most tools ignore this problem entirely. WeKan v9.46 solved it with keyboard-focusable "Move card up/down" and "Move list left/right" buttons, alongside visible focus indicators, skip-to-content links, ARIA landmarks, accessible modal handling, and an end-to-end accessibility test suite with regression coverage. This is shipped, tested, documented code citing specific WCAG success criteria, not a roadmap item.

Second, the standalone bundle. A .zip containing Node.js, FerretDB v1, and SQLite with a start script means the entire Kanban board runs without Docker, without an external database, without a container runtime, unzip and run. This is the deployment profile that scores highest on the metrics document's resilience, economic accessibility, and exit cost questions, and it opens the possibility of an osat-fluent-wekan installer that could pre-configure a board matching the UC Kanban Setup document.

## Concerns

- Cognitive accessibility is standards-driven, not neurodiversity-informed. WeKan's WCAG 2.1 AA work addresses compliance (keyboard navigation, screen reader support, focus management), which is necessary but not the same as designing for reduced cognitive load, as Leantime does. The interface is functional and clean, it was not shaped by the design philosophy that produces urgency-grouped, noise-reduced, cognitive-load-aware interfaces.
- The Meteor framework is the underlying platform. Meteor's own future as a framework has been a recurring community discussion. The move to Meteor 3.x / Node.js 24.x in v9.46 is a positive signal. This does not affect current functionality but is a longevity consideration.
- The standalone bundle with FerretDB/SQLite is new. Its stability and performance characteristics relative to the established Docker/MongoDB deployment path are not yet documented in production settings.
- WeKan's largest documented deployment is 30,000 users, far beyond UC's needs. At the other extreme, its suitability as a single-user or 2-3 person tool with the standalone bundle is precisely the use case that has the least community documentation.

## Evaluation

Evaluated against Universal Cake Evaluation Metrics v0.3.1. Each rating uses the scale Strong / Moderate / Weak / Unknown, tagged Verified / Inferred / Claimed. Stakeholder lens noted where it changes the answer.

### Inclusive

**Accessibility, alternative methods of interacting with content**

Lens: user. **[GATE]** if content becomes unreachable for people using assistive technology.

- Rating: Strong
- Evidence: Inferred (documented in v9.46 release notes with specific WCAG criteria cited, regression-tested, end-to-end accessibility test suite shipped)
- Notes: Version 9.46 improved accessibility across all pages following WCAG 2.1 AA guidelines. Specific shipped features, each citing the WCAG success criterion it addresses:
  - Visible keyboard focus indicator via `:focus-visible` outline, fixing a prior CSS reset that stripped all focus outlines (WCAG 2.4.7)
  - Skip-to-main-content link for keyboard and screen reader users to bypass header chrome (WCAG 2.4.1 Bypass Blocks)
  - ARIA landmark roles: `role="main"` on content, `role="navigation"` on header, `role="search"` on global search (WCAG 1.3.1)
  - Modals and popups marked as `role="dialog"` with `aria-modal="true"`, focus trapped on open, returned on close (WCAG 2.4.3 Focus Order)
  - Accessible names (`aria-label`) on all icon-only controls: close buttons, back buttons, search input and clear button (WCAG 4.1.2 Name, Role, Value)
  - Decorative icons marked `aria-hidden="true"` to prevent redundant screen reader announcements
  - Screen-reader-only `.sr-only` helper class
  - Keyboard-focusable "Move card up/down" buttons on cards and "Move list left/right" buttons on list headers, providing accessible reordering without drag-and-drop (fixes issue #459)
  - Fixed duplicate `id="header"` attributes that broke assistive technology navigation (WCAG 4.1.1)
  - Fixed My Cards table markup: header cells with `scope="col"`, caption added for correct screen reader announcement
  - Added missing `alt` text to user avatar images
  - End-to-end accessibility test suite checking page language, skip link, landmark roles, visible focus, dialog roles, accessible names, and absence of duplicate element IDs
  - Gate status: **Pass**. Content is reachable via keyboard and screen reader. The accessibility work is not cosmetic, it is structural and regression-tested.

**Multilingual integration**

Lens: user, community.

- Rating: Strong
- Evidence: Inferred
- Notes: Translated to 154 languages via Transifex. Adding a language is a community-editable data file at `imports/i18n/data/`, not a code change requiring maintainer access. New English strings for new features are added to `en.i18n.json`, non-English translations are contributed via the Transifex platform. This is the most accessible localisation architecture possible, a data file anyone can supply.

**Economic accessibility**

Lens: user.

- Rating: Strong
- Evidence: Inferred
- Notes: Free, MIT-licensed, no paid tiers, no per-seat fees, no feature gates. The standalone bundle runs on old or low-end hardware (1 GB RAM minimum documented). The bundle includes its own Node.js and database, no external dependencies to install. Works on slow or intermittent connections once loaded, as a self-hosted web application served from your own machine or local network.

**Cognitive accessibility**

Lens: user.

- Rating: Moderate
- Evidence: Inferred
- Notes: The interface is clean and functional, uses consistent layout, and does not employ dark patterns. It is learnable without formal training for anyone familiar with Kanban concepts. However, it was not designed with neurodivergent users as a named design constraint. It does not offer urgency grouping, cognitive load reduction, or emotional prioritisation features. It is adequate, with the named limitation that WCAG compliance and cognitive accessibility are overlapping but distinct concerns.

**Compatibility**

Lens: owner, user.

- Rating: Strong
- Evidence: Inferred
- Notes: Runs on Windows, macOS, and Linux via the standalone bundle or Docker. Web-based interface works in any modern browser. Multi-architecture support: amd64, arm64, s390x, ppc64le, riscv64 for Docker images. Snap for amd64/arm64 with additional architectures in progress. Input is keyboard, mouse, or touch. Output is any screen capable of rendering a web page.

### Representation

Lens: community, user.

- Rating: Unknown
- Evidence: Unknown
- Notes: WeKan's accessibility work is standards-driven (WCAG compliance) rather than representation-driven (neurodiversity as a founding design lens). No public information was found on team composition, advisory structures, or compensated user research from affected communities. Per the metrics document's guidance, the absence of representation information from public documentation is itself a data point. This is recorded as Unknown rather than rated.

### Resilience

Lens: owner, user.

- Rating: Strong
- Evidence: Inferred
- Notes: The standalone bundle is self-contained, no live services, no licence servers, no expiring tokens. Once deployed, WeKan does not depend on any external infrastructure. The bundle runs without Docker, without an external database, without a network connection to any third party. During network outages, the self-hosted instance continues to function on the local network. During power outages, it adds only one process (the WeKan server) beyond what already serves the content. The bundle does not go out of date in a way that prevents operation, older versions continue to run. The Docker/Snap deployment paths add a dependency on those runtimes but remain self-hosted.

### Agency

**Sovereignty & Privacy**

Lens: owner, user.

- Rating: Strong
- Evidence: Inferred
- Notes: MIT-licensed, the most permissive option available. No open core model, no proprietary plugins, no SaaS tier, no vendor relationship. The owner can self-host, modify, fork, and redistribute without permission. User data rests entirely on the owner's machine or infrastructure, never on a third party's. No telemetry, analytics, update checks, or licence server contact was identified in the documentation or release notes. The Snap deployment path has automatic updates enabled by default, which is a sovereignty consideration, the standalone bundle and Docker paths do not auto-update.
- Power-imbalance proxies (vendor↔user):
  - **Exit cost:** Low. Hours, not dollars. Export exists in multiple open formats (see Exit and portability below). Dropping WeKan leaves your data in exportable form. The standalone bundle is removable by deleting a folder.
  - **Data portability:** Yes. Complete, documented, machine-readable export exists in JSON (including attachments as base64), CSV, TSV, and Excel. Import/export interop with Kanboard, NextCloud Deck, OpenProject, GitHub, GitLab, Gitea, and Forgejo. REST API for programmatic export. This is the strongest data portability finding in any tool evaluated on this radar to date.
  - **Terms-of-service volatility:** Not applicable. MIT licence. No vendor relationship exists to impose terms.
  - **Pricing asymmetry:** Not applicable. No pricing exists. Commercial support is available from wekan.fi but is entirely optional and does not gate any features.
- Reversibility test: "If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?" Answer: export your boards (JSON, CSV, or interop format), import them into any of seven supported tools, delete the WeKan folder. Estimated cost: hours, not dollars.
- Gate status: **Pass**. No telemetry or content exposure identified. MIT licence compatible with AGPL-3.0-or-later.

**Interaction Patterns**

Lens: user.

- Rating: Moderate
- Evidence: Inferred
- Notes: Evaluated against the metrics document's supportive/dark pattern pairs:
  - Honest defaults / preselection tricks: defaults serve the user, no vendor-serving preselections identified. Pass.
  - Easy exit / roach motel: cancelling or deleting takes no more effort than starting. Export is accessible from the board sidebar. Pass.
  - Forgiveness / confirmshaming: undo does not exist for all actions (the release notes explicitly state "there is no undo yet"), destructive actions may not be fully recoverable. This is a named limitation.
  - Natural stopping points / engagement traps: the interface has endings, no infinite scroll, no autoplay, no engagement traps. Pass.
  - Quiet by default / attention farming: no manufactured urgency, no streaks, no attention farming. Notifications exist but are not aggressive. Pass.
  - Plain asking / consent walls: no consent walls, no trick wording, no nagging. Pass.
  - Visible costs / drip pricing: no costs, no hidden telemetry, no surprise charges. Pass.
  - Leaves you whole / lock-in: data exits in open formats. Pass.
  - Supplementary: the tool helps you accomplish the goal you arrived with (manage a Kanban board) and does not manufacture new goals. Actions are predictable. Completed tasks are respected as finished. You can stop and leave without penalty.
  - The "no undo" limitation and the absence of proactive cognitive-load reduction are what keep this at Moderate rather than Strong. The tool avoids all dark patterns but does not go out of its way to be actively supportive beyond the task at hand.

### Sustainability

**Environment, direct and indirect impacts**

Lens: environment, owner, user.

- Rating: Strong (standalone bundle), Moderate (Docker/MongoDB)
- Evidence: Inferred
- Notes: The standalone bundle (Node.js + FerretDB + SQLite) has a minimal footprint, one process, one lightweight database, no container overhead. It does not require new hardware, it runs on 1 GB RAM. The Docker/MongoDB deployment path is a standard web application stack with a modest but non-trivial footprint, comparable to any Node.js/MongoDB application. Neither path requires hardware upgrades for the user or others. Bandwidth is served once per page load and cached.

### Security

Lens: owner, user.

- Rating: Moderate
- Evidence: Inferred
- Notes:
  - **What data does it collect, and where does that data rest?** Board, card, and user data in the local database (MongoDB or SQLite via FerretDB). No data sent to external services. Data rests entirely on the owner's infrastructure.
  - **How are vulnerabilities reported, and is there a published security policy?** v9.46 documents a responsible disclosure process. The ProxyBleed vulnerability (GHSA-jggc-qvfc-jr6x, CWE-290, CWE-348) was fixed with a detailed writeup, specific commit references, operator action requirements, and regression tests. A Hall of Fame page exists at wekan.fi for security researchers. This is a positive signal for security responsiveness.
  - **How quickly are security patches released?** The ProxyBleed fix was shipped in the same release that disclosed it (v9.46), with regression tests covering the fix. The fix was backported to configuration documentation across all deployment paths (Docker, Snap, VirtualBox, devcontainer).
  - **What is the supply chain exposure?** Meteor/Node.js dependency tree was not independently audited. The standalone bundle vendors its own Node.js and FerretDB, which reduces but does not eliminate supply chain exposure. Install scripts (start-wekan) were not reviewed for arbitrary code execution.
  - **Does adding it expand or shrink the attack surface?** Adding a web application with a database expands the attack surface of any system it joins. The standalone bundle minimises this by containing everything in one folder with no system-wide installation. The header-login feature (reverse-proxy SSO) is the historically riskiest surface, now hardened.
  - **Assessment method:** Documentation review, v9.46 release notes, GitHub security advisory. No installation performed, no network monitor used. Date: 2026-07-25.
  - Gate status: **Pass**. No telemetry or content exposure that cannot be disabled.

**Network behaviour:**
- [x] Outbound connections during normal use — only to VCS remotes or configured integrations, not to WeKan's own infrastructure
- [ ] Update checks — Snap has automatic updates by default, standalone bundle does not auto-update
- [ ] Telemetry or usage data — none identified
- [ ] Licence server contact — none, MIT licence
- [ ] Affects networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via network are clean and compliant

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content — database files in the bundle directory
- [ ] Caches content outside the working directory — contained within the bundle/Docker volume
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

**Assessment status:** Provisional, Inferred. A hands-on trial with network monitoring would upgrade Security from Inferred to Verified.

### The Product or Service Itself

**Longevity**

Lens: owner, community.

- Rating: Strong
- Evidence: Inferred
- Notes: Active since 2014, over 21,000 GitHub stars, roughly 3,000 forks, last updated July 2026. Packaged for Docker, Snap, Sandstorm, Kubernetes, and as a standalone bundle. The largest documented deployment is 30,000 users. Translated to 154 languages. The Meteor framework dependency is a recurring community discussion but the recent migration to Meteor 3.x / Node.js 24.x demonstrates continued investment. Release cadence is frequent, with features and fixes shipped multiple times per month.

**Content endurance**

Lens: owner, community.

- Rating: Moderate
- Evidence: Inferred
- Notes: Content (boards, cards, project data) lives in the local database on your own infrastructure. If WeKan is removed, the data remains in the database and is queryable, but it is application-structured. The comprehensive export capabilities (JSON with base64-encoded attachments, CSV, TSV, Excel, PDF) mean content can be extracted in human-readable form before removal. WeKan is an enhancement layer over your own data rather than a container that takes data with it, but the enhancement is substantial enough that the data is not trivially useful without either the application or an export step.

**Exit and portability**

Lens: owner, user. **[GATE]** if no exit exists.

- Rating: Strong
- Evidence: Inferred
- Notes: Comprehensive exit exists. Board export to JSON (including attachments as base64), CSV, TSV, Excel (.xlsx), and PDF. Import/export interop with Kanboard, NextCloud Deck, OpenProject, GitHub, GitLab, Gitea, and Forgejo via a generalised REST API. The whole-board import API enables migrating all boards, workflows, and rules between WeKan instances. Trello and Jira import paths exist for migration in. The interop formats are round-trippable where the source tool's data model supports it. This is not just "data can be extracted," it is "data can be moved to seven named alternatives via documented, tested paths."
- Gate status: **Pass**. Exit exists in multiple usable, open formats.

**Adjustability and support**

Lens: owner, community.

- Rating: Strong
- Evidence: Inferred
- Notes: MIT licence, fork-and-fix path is fully open with no reciprocal obligation. The codebase is JavaScript/Meteor, readable by any web developer. Documentation was reorganised and expanded in v9.46. Commercial support is available from wekan.fi. The REST API is comprehensive and documented with OpenAPI, covering boards, lists, cards, rules, import/export, and attachments. The rules/automation engine allows customisation without code changes.

**Market Position**

Lens: community, owner.

- Rating: not applicable
- Evidence: not applicable
- Notes: WeKan is a self-hosted utility tool, not infrastructure that other projects depend on. There is no platform dynamic, no marketplace, no take rate, no API that third parties build on top of in a way that creates lock-in.
  - Market concentration: WeKan is one of several self-hosted Kanban options, no dominant market position.
  - Take rate: none.
  - API stability and deprecation history: the v9.46 release added significant API surface, breakage was not documented.
  - Forkability: Strong. MIT licence, no governance structure that could turn hostile. The project could survive its current steward disappearing.
  - Contributor concentration: not independently measured. The release notes credit xet7, rz1027, and Claude (AI-assisted development), suggesting a small core team. Bus factor is a consideration, mitigated by the MIT licence.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Inferred | WCAG 2.1 AA, keyboard reordering, ARIA landmarks, focus management, accessibility test suite |
| Multilingual integration | Strong | Inferred | 154 languages, community-editable data files via Transifex |
| Economic and cognitive accessibility | Strong (economic), Moderate (cognitive) | Inferred | Free, MIT, standalone bundle runs on 1 GB RAM, not neurodiversity-designed |
| Representation | Unknown | Unknown | Standards-driven, no public team composition or user research data |
| Compatibility | Strong | Inferred | Windows/Mac/Linux, multi-arch Docker, any modern browser |
| Resilience | Strong | Inferred | Standalone bundle is self-contained, no external dependencies |
| Agency, sovereignty & privacy | Strong | Inferred | MIT, no open core, no telemetry, self-hosted, data stays local |
| Agency, power-imbalance proxies | Strong | Inferred | Low exit cost, strong data portability, no ToS, no pricing |
| Agency, interaction patterns | Moderate | Inferred | No dark patterns, no undo, not actively cognitive-load-aware |
| Environment, direct and indirect | Strong / Moderate | Inferred | Standalone bundle minimal, Docker/MongoDB moderate |
| Security | Moderate | Inferred | ProxyBleed fix with CVE and regression tests, no formal audit |
| Longevity | Strong | Inferred | Active since 2014, 21k stars, frequent releases |
| Content endurance | Moderate | Inferred | Data on your infrastructure, export step needed before removal |
| Exit and portability | Strong | Inferred | JSON, CSV, TSV, Excel, PDF, interop with 7 external tools |
| Adjustability and support | Strong | Inferred | MIT, readable code, comprehensive REST API, commercial support available |
| Market position | N/A | N/A | Self-hosted utility, no platform dynamics |
| Gates | **Pass** (all four) | | Accessibility floor cleared (WCAG 2.1 AA), MIT compatible, no telemetry, exit confirmed in multiple formats |

## Relationship to project

Dev toolchain, specifically the Kanban board for managing work across all Universal Cake projects. The standalone bundle opens the possibility of an `osat-fluent-wekan` installer that could pre-configure a board matching the UC Kanban Setup document (four columns, project tags, Virtuous Iteration review checklist). Also a potential reference case for the Virtuous Iteration audit, WeKan is a tool that actively advances accessibility (Strong, Inferred) through shipped, tested, documented WCAG work, which is the kind of outcome the audit's questions aim to surface.

## Status notes

- Last reviewed: 2026-07-25
- In assess: full documentation-level evaluation complete against Universal Cake Evaluation Metrics v0.3.1. All four gate criteria pass. Automated evaluation recommends adopt at low risk. Two items would upgrade ratings from Inferred to Verified on a hands-on trial: (1) security, network monitoring to confirm no undisclosed telemetry and to audit supply chain exposure, and (2) accessibility, testing the WCAG 2.1 AA features hands-on with keyboard-only navigation and a screen reader.
- What would move it to trial: a decision to install the standalone bundle, stand up a test instance, confirm security via network monitoring, and test accessibility with assistive technology.
- What would move it to hold: a discovery of undisclosed telemetry (gate failure), a Meteor framework deprecation that threatens longevity, or a decision that Leantime's deeper cognitive accessibility is worth the additional overhead and open-core trade-offs.

## Links

- https://github.com/wekan/wekan
- https://github.com/wekan/wekan/releases/tag/v9.46
- https://wekan.github.io/
- https://wekan.fi/docs/
- https://app.transifex.com/wekan/wekan

## License

This document, *WeKan, Accessible Kanban Board*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Full evaluation against UC Evaluation Metrics v0.3.1, resolved all resolvable unknowns, all gates pass |
| 0.1.0 | Draft | Initial entry, documentation and web review only |

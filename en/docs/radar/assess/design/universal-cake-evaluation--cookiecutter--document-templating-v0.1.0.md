---
dc:title: "Universal Cake Evaluation, Cookiecutter, Document Templating and Multi Project Sync"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Sonnet 4.6 (Anthropic)"
dc:subject:
  - "evaluation"
  - "templating"
  - "documentation tooling"
dc:description: "Evaluation of Cookiecutter, and its companion tool cruft, against the Universal Cake Evaluation Metrics v0.3.0, scoped to keeping shared documents synchronized across every project that includes them."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-22"
dcterms:modified: "2026-07-22"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "sat-radar--cookiecutter--document-templating"
dc:identifier: "universal-cake-evaluation--cookiecutter--document-templating"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: ""
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-22"
    author: "Christopher Steel"
    notes: "Initial evaluation, documentation and repository review only, no hands on trial performed."
---

# universal-cake-evaluation--cookiecutter--document-templating-v0.1.0

## Scope

This evaluation covers Cookiecutter for the specific use case that prompted it, keeping a shared document, such as the SAT radar entry template or a style guide file, updated with its latest version across every project that includes it. Cookiecutter alone does not perform that update step, so this evaluation rates Cookiecutter honestly on its own terms and separately notes where its companion tool, cruft, changes the picture. Where a rating differs between the two, both are given.

All ratings carry an evidence tag per the metrics document. Everything here is **Inferred** or **Claimed** from documentation and repository metadata, nothing is **Verified** by direct SAT testing yet. That in itself is worth noting since Trial gate criteria require Verified tags on all gate criteria before an item can move to trial.

## Inclusive

### Accessibility

Lens: user (in this case, the person maintaining or extending a SAT project).

- Alternative methods of interacting with content. Command line only, plain text input and output throughout. Rating: **Moderate**, Inferred. Nothing actively blocks assistive technology, since it is ordinary terminal text, but there is no dedicated accessibility affordance either.
- Multilingual integration. No localization of the CLI's own prompts was identified in the documentation reviewed. Templates themselves can produce content in any language. Rating: **Unknown** for the CLI's own interface, **Strong** for template output language, Inferred.
- Economic accessibility. Free, open source, runs on modest hardware, works fully offline once a template is available locally. Rating: **Strong**, Verified against the licence and installation documentation.
- Cognitive accessibility. Using an existing template is a single command with interactive prompts. Writing or maintaining a template requires comfort with Jinja2 syntax and, for advanced cases, Python or shell scripting for hooks. Rating: **Moderate**, Inferred, the floor is low for users, higher for maintainers.
- Compatibility. Cross platform, Windows, macOS, Linux, Python 3.10 through 3.14. Rating: **Strong**, Verified against the repository's stated support matrix.

### Representation

No information was found on the composition of the Cookiecutter or cruft maintainer and contributor communities. Rating: **Unknown**, and per the metrics document that absence from public documentation is itself recorded as a data point rather than assumed neutral.

### Resilience

Once a template is available locally, rendering works fully offline, with no licence server or expiring token involved. Fetching a template from a remote Git repository does require network access at that one step. Rating: **Strong**, Inferred.

## Agency

### Sovereignty & Privacy

Both projects are open source and can be self hosted, forked, modified, and redistributed without permission, Cookiecutter under BSD-3-Clause, cruft under MIT. Both licences are permissive and compatible with SAT's own AGPL-3.0-or-later documents as dependencies. Generated output is ordinary files on the user's own machine, nothing rests with a third party. No telemetry, analytics, or phone home behaviour was identified in either project's documentation. Rating: **Strong**, Inferred, not independently network monitored.

**Power imbalance proxies (vendor to user):**

- Exit cost. Effectively zero. The output of either tool is plain files. Stopping use of Cookiecutter or cruft at any time leaves the already generated documents fully intact and usable, only the automated update capability is lost. Rating: **Strong**.
- Data portability. Full, the artifact is your own file tree, there is no export format question. Rating: **Strong**.
- Terms of service volatility. Not applicable, this is installed software, not a hosted service with terms of service.
- Pricing asymmetry. Not applicable, both tools are free and open source with no pricing.

### Interaction Patterns

The CLI asks direct questions and applies the answers, with no dark pattern equivalents identified, defaults are declared explicitly in each template's `cookiecutter.json`, and exit is simply not running the command again or deleting the output. Rating: **Strong**, Inferred.

## Sustainability

### Environment

Both tools run as short lived local processes with no ongoing service component. Energy and bandwidth impact is negligible beyond the one time fetch of a remote template. Rating: **Strong**, Inferred.

## Security

- Data collected: none identified in either tool's own operation.
- Vulnerability reporting: Cookiecutter's repository includes a published `SECURITY.md`, a `.bandit` static analysis configuration, and a `.safety-policy.yml` dependency vulnerability policy, indicating an active security practice. Rating: **Moderate to Strong**, Inferred from repository configuration, not from a review of actual advisory response times.
- Supply chain exposure: Cookiecutter's runtime dependency footprint is small, primarily Jinja2. The material risk is template hooks, pre generate and post generate scripts that run with the invoking user's full local permissions. This risk is inherent to the templating model itself, not a defect in Cookiecutter's code, but it means every third party template, and by extension every `cruft update` against that template, should be reviewed before first use the same way any other third party code would be. **[GATE relevant]**, this does not fail the telemetry or content exposure gate outright, since the behaviour is visible in the template source and not hidden or silent, but it is a live risk that should be named explicitly rather than assumed away.
- Attack surface: adding either tool to a project's toolchain adds a build time dependency, not a runtime one, once documents are generated the generated project carries no residual dependency on Cookiecutter itself.
- Assessment method: documentation and repository review only, see the paired radar entry's Security assessment section for the checklist format. No runtime network monitoring performed.

## The Product or Service Itself

- **Longevity.** Cookiecutter has 24.9k GitHub stars, 3,143 commits, an active release cadence with the latest release, v2.7.1, in March 2026, and has been a fixture of the Python packaging ecosystem for over a decade. Cruft is smaller, 1.6k stars, but has an active release history through late 2025 and December 2025 with signed, verified commits. Rating: **Strong** for Cookiecutter, **Moderate to Strong** for cruft, both Verified against repository metadata.
- **Content endurance.** This is the property that matters most for SAT's actual goal. Content is authored as plain files, Cookiecutter and cruft are enhancement layers that assemble and update those files, they are not containers the content lives inside. If either tool disappeared tomorrow, every document already generated remains exactly as it is, fully readable and editable with no special tooling. Rating: **Strong**, Verified by design, this follows directly from how the tools work rather than from a claim that needs checking.
- **Exit and portability. [GATE]** Output is plain markdown or text files in ordinary directories, no proprietary format anywhere in the chain. Gate passes cleanly. Rating: **Strong**.
- **Adjustability and support.** Both projects are source available with permissive licences, meaning the fork and fix path is open. Cookiecutter has an active Discord, GitHub issue tracker, and Stack Overflow tag. Rating: **Strong** for Cookiecutter, **Moderate** for cruft, Inferred from a smaller but still active community.
- **Market Position.** Cookiecutter functions closer to a shared convention within the Python community than to a chokepoint platform, there is no meaningful take rate, licensing tax, or ability to demote or clone dependents, since anyone can fork either project outright under its permissive licence. Rating: **Strong** (low imbalance risk), Inferred, contributor concentration for either project was not independently verified.

## Gates summary

| Gate | Status | Notes |
|------|--------|-------|
| Telemetry or content exposure that cannot be disabled | Pass | None identified in either tool |
| Licence incompatible with the project | Pass | BSD-3-Clause and MIT are both permissive and compatible with AGPL-3.0-or-later usage as a dependency |
| Accessibility floor (assistive technology reachability) | Pass, not directly applicable | CLI dev tooling, does not touch the accessibility of the documents it produces |
| No exit, data or content cannot be extracted in usable form | Pass | Plain files throughout, no proprietary format |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | CLI only, no dedicated accessibility affordance, nothing blocking |
| Multilingual integration | Unknown / Strong | Inferred | Unknown for CLI interface, Strong for template output language |
| Economic and cognitive accessibility | Strong / Moderate | Verified / Inferred | Free and offline capable, moderate learning curve for template authors |
| Representation | Unknown | N/A | No public information found on contributor composition |
| Compatibility | Strong | Verified | Windows, macOS, Linux, Python 3.10 to 3.14 |
| Resilience | Strong | Inferred | Offline capable once template is local |
| Agency, sovereignty & privacy | Strong | Inferred | Self hostable, forkable, no telemetry identified, not independently monitored |
| Agency, power-imbalance proxies | Strong | Inferred | Near zero exit cost, full data portability, no ToS or pricing to speak of |
| Agency, interaction patterns | Strong | Inferred | Explicit prompts, declared defaults, trivial exit |
| Environment, direct and indirect | Strong | Inferred | Short lived local process, negligible footprint |
| Security | Moderate | Inferred | Active security practice signals present, hook script execution is the standing risk |
| Longevity | Strong / Moderate-Strong | Verified | Cookiecutter well established, cruft smaller but active |
| Content endurance | Strong | Verified by design | Plain files, enhancement layer not a container |
| Exit and portability | Strong | Verified | Gate pass, open formats throughout |
| Adjustability and support | Strong / Moderate | Inferred | Active communities for both, cruft's is smaller |
| Market position | Strong (low imbalance) | Inferred | Convention, not a platform chokepoint |
| Gates | Pass | See Gates summary above | |

## Recommendation

Cookiecutter by itself is a solid, well governed, low lock-in tool, but it does not address the stated goal of keeping a document updated across every project that includes it, since it only renders once. Cruft is the piece that closes that gap, and scores similarly well on the metrics that matter most here, low exit cost, full content endurance, permissive licensing, no telemetry identified. The standing concern for either tool is the same one, template hook scripts run with full local permissions, which argues for reviewing any template's hooks before first use and before any `cruft update`, the same discipline SAT would apply to any other third party code.

This supports moving the paired candidate, Cookiecutter plus cruft, forward to a trial, specifically standing up this project's own shared documents as a tracked template and confirming `cruft check` and `cruft update` behave as documented against a real downstream project, with Verified evidence tags recorded from that trial per the lifecycle gate requirement.

## License

This document, *Universal Cake Evaluation, Cookiecutter, Document Templating and Multi Project Sync*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial evaluation against Universal Cake Evaluation Metrics v0.3.0, documentation and repository review only |

---
dc:title: "Universal Cake Evaluation, cruft, Template Synchronization for Generated Projects"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Sonnet 4.6 (Anthropic)"
dc:subject:
  - "evaluation"
  - "templating"
  - "documentation tooling"
dc:description: "Evaluation of cruft against the Universal Cake Evaluation Metrics v0.3.0, as a dedicated companion to the Cookiecutter evaluation."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-22"
dcterms:modified: "2026-07-22"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "sat-radar--cruft--document-template-sync"
dc:identifier: "universal-cake-evaluation--cruft--document-template-sync"
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

# universal-cake-evaluation--cruft--document-template-sync-v0.1.0

## Scope

This evaluation covers cruft on its own terms, as the companion tool that gives Cookiecutter the update capability it lacks. It shares a substrate with `universal-cake-evaluation--cookiecutter--document-templating`, and where a rating simply inherits from Cookiecutter's own behaviour (since cruft calls Cookiecutter directly), this document says so rather than repeating the full reasoning. Where cruft's own additions, `.cruft.json` tracking, `cruft check`, `cruft update`, change the picture, this document rates those specifically.

All ratings are **Inferred** or **Claimed** from documentation and repository metadata, none are **Verified** by direct SAT testing yet.

## Inclusive

### Accessibility

Command line only, same profile as Cookiecutter, plain text prompts and output. Rating: **Moderate**, Inferred, inherited from Cookiecutter.

### Representation

No information was found on the composition of the cruft maintainer and contributor community. Rating: **Unknown**, recorded as a data point per the metrics document rather than assumed neutral.

### Resilience

`cruft create` and `cruft update` both require network access to fetch the current state of the linked template repository, unlike a one time local Cookiecutter render which can work fully offline once a template is already local. This is a meaningful difference from Cookiecutter alone, since the entire point of cruft is checking against a remote source of truth. Rating: **Moderate**, Inferred, network dependent by design for its core function.

## Agency

### Sovereignty & Privacy

MIT licensed, self hostable, forkable, modifiable without permission. No telemetry or phone home behaviour identified in the documentation reviewed. Tracked projects retain a `.cruft.json` file recording the template source and commit, which is local metadata, not sent anywhere. Rating: **Strong**, Inferred, not independently network monitored.

**Power imbalance proxies (vendor to user):**

- Exit cost. Low, but not quite zero the way plain Cookiecutter output is. Removing cruft from a project means deleting `.cruft.json` and losing the ability to detect future template drift automatically, the generated files themselves remain fully intact and usable either way. Rating: **Strong**, with the caveat noted.
- Data portability. Full for the generated content itself. The `.cruft.json` tracking data is plain JSON, also fully portable. Rating: **Strong**.
- Terms of service volatility. Not applicable, installed software, no hosted service terms.
- Pricing asymmetry. Not applicable, free and open source.

### Interaction Patterns

`cruft update` surfaces conflicts explicitly for manual resolution rather than silently overwriting local changes, which is the supportive pattern (forgiveness, undo exists via the normal git working tree) rather than the dark pattern (silent overwrite). Rating: **Strong**, Inferred.

## Sustainability

### Environment

Short lived local process per invocation, similar footprint to Cookiecutter, with the addition of a network fetch on each check or update. Negligible either way. Rating: **Strong**, Inferred.

## Security

- Data collected: none identified in cruft's own operation.
- Vulnerability reporting: no dedicated `SECURITY.md` or equivalent was confirmed in the documentation reviewed for cruft specifically, unlike Cookiecutter which does publish one. Rating: **Unknown**, worth checking directly in the repository before trial.
- Supply chain exposure: cruft depends directly on Cookiecutter, so it inherits Cookiecutter's dependency footprint plus its own (primarily GitPython or equivalent for the diff and merge machinery). The material risk carried over from Cookiecutter, pre and post generate hook scripts executing with full local permissions, applies again on every `cruft update`, not only at project creation, since an update re-runs the generation step against whatever the template now contains. **[GATE relevant]**, same reasoning as the Cookiecutter entry, visible in template source rather than hidden, but a live risk to name rather than assume away, and arguably more relevant here since updates are meant to happen repeatedly and, per the documented CI pattern, potentially unattended.
- Attack surface: cruft's CI automation pattern (a scheduled job that runs `cruft check` and opens a pull request) is a build time and CI time dependency, it does not add anything to the generated project's own runtime footprint.
- Assessment method: documentation and repository review only, see the paired radar entry's Security assessment section. No runtime network monitoring performed.

## The Product or Service Itself

- **Longevity.** Roughly 1.6k GitHub stars and around 104 forks, a meaningfully smaller community than Cookiecutter's. Recent releases indicate ongoing maintenance, including a recent tagged release, but the contributor pool is smaller and single points of failure are more plausible. Rating: **Moderate**, Inferred.
- **Content endurance.** Same property as Cookiecutter and for the same reason, the tracked content is plain files, cruft is an enhancement layer that updates them, not a container. If cruft disappeared, every already generated document remains fully intact, the only loss is the automated drift detection going forward. Rating: **Strong**, Verified by design.
- **Exit and portability. [GATE]** Plain files, plain JSON tracking metadata, no proprietary format. Gate passes cleanly. Rating: **Strong**.
- **Adjustability and support.** Source available under MIT, fork and fix path open. Community support channels are narrower than Cookiecutter's, primarily GitHub issues, no dedicated chat community identified. Rating: **Moderate**, Inferred.
- **Market Position.** Not a platform in any meaningful sense, a utility with a permissive licence and an open fork path. Rating: **Strong** (low imbalance risk), Inferred.

## Gates summary

| Gate | Status | Notes |
|------|--------|-------|
| Telemetry or content exposure that cannot be disabled | Pass | None identified |
| Licence incompatible with the project | Pass | MIT is permissive and compatible with AGPL-3.0-or-later usage as a dependency |
| Accessibility floor (assistive technology reachability) | Pass, not directly applicable | CLI dev tooling, does not touch the accessibility of the documents it produces |
| No exit, data or content cannot be extracted in usable form | Pass | Plain files and plain JSON tracking metadata throughout |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | CLI only, inherited from Cookiecutter |
| Multilingual integration | Strong (output) | Inferred | Follows the template's own language, inherited from Cookiecutter |
| Economic and cognitive accessibility | Strong / Moderate | Inferred | Free, adds one new concept, `.cruft.json`, and a merge conflict workflow to learn |
| Representation | Unknown | N/A | No public information found on contributor composition |
| Compatibility | Strong | Inferred | Cross platform, same base as Cookiecutter |
| Resilience | Moderate | Inferred | Network dependent by design for check and update |
| Agency, sovereignty & privacy | Strong | Inferred | Self hostable, no telemetry identified, not independently monitored |
| Agency, power-imbalance proxies | Strong | Inferred | Low exit cost, full portability of both content and tracking metadata |
| Agency, interaction patterns | Strong | Inferred | Explicit conflict resolution rather than silent overwrite |
| Environment, direct and indirect | Strong | Inferred | Negligible footprint, small added network cost |
| Security | Unknown to Moderate | Inferred | No dedicated security policy confirmed, hook execution risk recurs on every update |
| Longevity | Moderate | Inferred | Smaller community than Cookiecutter, active but thinner |
| Content endurance | Strong | Verified by design | Plain files, enhancement layer not a container |
| Exit and portability | Strong | Inferred | Gate pass, open formats throughout |
| Adjustability and support | Moderate | Inferred | Fork and fix path open, narrower community channels |
| Market position | Strong (low imbalance) | Inferred | Utility, not a platform |
| Gates | Pass | See Gates summary above | |

## Recommendation

Cruft scores well where it matters most for SAT's actual goal, low exit cost, full content endurance, permissive licensing, explicit rather than silent conflict handling. It is meaningfully smaller and less battle tested than Cookiecutter itself, and it has no independently confirmed security policy, which is worth checking directly before trial rather than assuming it mirrors Cookiecutter's practice. Its core function requires network access on every check or update, a real difference from Cookiecutter's offline capable rendering, worth knowing going in rather than discovering later.

The recurring hook execution risk deserves particular attention here, since cruft's whole value proposition is running updates repeatedly, including on an unattended CI schedule per its documented Actions pattern. A template with a compromised or careless hook script would re-execute that risk on every update, not just once at creation. Before any unattended automation, the template's own hooks should be reviewed and trusted, the same way the source template itself would be.

Recommend the same path as the paired Cookiecutter entry, a trial that tracks this project's own shared documents, confirms `cruft check` and `cruft update` behave as documented, and specifically exercises a merge conflict case (a downstream project with a local edit to a tracked file) before either entry moves toward adopt.

## License

This document, *Universal Cake Evaluation, cruft, Template Synchronization for Generated Projects*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial evaluation against Universal Cake Evaluation Metrics v0.3.0, documentation and repository review only |

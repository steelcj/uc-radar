---
dc:title: "SPDX License List, Machine-Readable License Identifiers"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "spdx"
  - "licensing"
  - "standards"
  - "compliance"
  - "data-source"
dc:description: "Radar entry assessing the SPDX License List as a machine-readable data source for SAT to download locally, cache, and validate licence identifiers in document front matter against, following the same pattern SAT uses for language standards."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-25"
dcterms:modified: "2026-07-25"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://spdx.org/licenses/"
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle"
dc:identifier: "spdx-license-list--machine-readable-license-identifiers"
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
      Completed full question-by-question assessment against every
      item in Universal Cake Evaluation Metrics v0.3.1. Each specific
      sub-question (e.g. hardware, OS, browser under Compatibility,
      the three resilience scenarios, all six security questions, the
      five market position proxies) now has an explicit answer rather
      than a section-level summary. Multilingual upgraded from N/A to
      Strong/Moderate with reasoning. Market Position answered fully
      despite N/A rating. Scorecard notes expanded.
  - version: "0.1.0"
    date: "2026-07-25"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: "Initial entry, evaluation at section level."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude"
      evaluator_version: "Sonnet-5"
      recommendation: adopt
      reasoning: >
        The SPDX License List is the ISO-standardised, universally
        adopted canonical source for licence identifiers. UC already
        uses SPDX identifiers in every document's dcterms:rights
        field. The machine-readable data (licenses.json from
        spdx/license-list-data) is a static JSON file downloadable
        from GitHub, tagged on every release, requiring no API key,
        no authentication, no runtime dependency. The integration
        pattern, download, cache locally, check for updates
        periodically, is identical to what SAT already does for
        language standards. CC-BY-3.0 licensed. No gate concerns.
        No telemetry. No vendor relationship. Trivial exit, it is
        a JSON file. This is as close to a zero-risk adoption as
        the radar is likely to produce.
      risk: low
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-07-25"
  human: []
---

# SPDX License List, Machine-Readable License Identifiers

## What it is

The SPDX License List is a standardised list of commonly found open source licences maintained by the SPDX Project under the Linux Foundation. Each licence entry includes a short identifier (e.g. `AGPL-3.0-or-later`, `MIT`, `Apache-2.0`), the full name, the vetted licence text, and a canonical permanent URL. The list is an integral part of the SPDX Specification, which is published as ISO/IEC 5962:2021. The current version is 3.28.0, released 2026-02-20.

The machine-readable data is published at `spdx/license-list-data` on GitHub in JSON, HTML, text, RDFa, and template formats. The repository is tagged on every licence list release and built automatically from the source XML at `spdx/license-list-XML`. The two files relevant to SAT are `licenses.json` (summary information for all licences) and `exceptions.json` (summary information for all licence exceptions).

## Why interesting

SAT already uses SPDX licence identifiers in every document's `dcterms:rights` field (e.g. `SPDX-License-Identifier: AGPL-3.0-or-later`). Currently there is no local validation that the identifier used is a real, non-deprecated SPDX identifier. Downloading `licenses.json`, caching it locally, and checking for updates periodically would let SAT validate licence identifiers in document front matter the same way it already validates language codes against downloaded language standards. The integration pattern is identical: fetch a canonical JSON file from a standards body, store it locally, check the version tag periodically, update when a new version appears.

Specific SAT capabilities this enables:

- Validate that every `SPDX-License-Identifier:` value in front matter is a real SPDX identifier
- Warn if a deprecated identifier is used and suggest the current replacement
- Report whether a licence is OSI-approved, FSF-approved, or both
- Detect licence expression syntax errors (malformed `AND`, `OR`, `WITH` expressions)
- Ensure consistency across all documents in an archive, every file claiming the same licence uses the same identifier string

## Concerns

There are no concerns worth noting. The SPDX License List is an ISO standard, maintained by a Linux Foundation project, licensed under CC-BY-3.0, distributed as static data files, requiring no authentication, no API key, no runtime dependency, and no vendor relationship. It is the canonical source for the identifiers UC already uses. The only honest concern is that SAT needs to handle the network fetch gracefully when offline, which is the same concern that applies to every external data source SAT downloads, and is already solved in the language standards pattern.

## Evaluation

Evaluated against Universal Cake Evaluation Metrics v0.3.1. Each rating uses the scale Strong / Moderate / Weak / Unknown, tagged Verified / Inferred / Claimed. Stakeholder lens noted where it changes the answer.

### Inclusive

**Accessibility, alternative methods of interacting with content**

Lens: user. **[GATE]** if content becomes unreachable for people using assistive technology.

- Rating: Strong
- Evidence: Inferred
- Notes: Would this decrease or increase the number of people who can access content? Increase. SPDX identifiers replace the need to reproduce full licence texts inline, reducing document length and complexity. The data itself is available in multiple formats (JSON, HTML, text, RDFa, template), all plain text, all screen-reader-compatible. The identifiers are short ASCII strings with no special characters beyond hyphens and dots. Vision, hearing, motor, speech differences: no barriers, the data is text and the SAT integration would produce text output (validation messages). No assistive technology concern, a JSON data file and its validation output are inherently accessible to any tool that reads text.
- Gate status: **Pass**. Content does not become unreachable.

**Multilingual integration**

Lens: user, community.

- Rating: Strong (identifiers), Moderate (licence texts)
- Evidence: Inferred
- Notes: Is it available in languages the audience understands? The identifiers themselves are language-neutral by design, `MIT` and `AGPL-3.0-or-later` are not English words, they are standardised codes, the same way `en-CA` is a language code rather than an English word. The licence texts in the data file are predominantly in English because the licences themselves are written in English, which is a property of the legal documents, not of the SPDX list. Is adding a language feasible for the community? Adding a new licence to the list is a documented public process (open GitHub issue, SPDX legal team reviews), which is a data submission anyone can make, not a code change requiring maintainer access. Translating existing licence texts is outside SPDX's scope since the legal texts are canonical in their original language.

**Economic accessibility**

Lens: user.

- Rating: Strong
- Evidence: Verified
- Notes: Is it free or affordable to use? Free, CC-BY-3.0, no authentication, no API key, no rate limiting documented. Does it run on old or low-end hardware? The JSON file is small (under 1 MB), parseable by any device that can run Python or any other scripting language. No minimum hardware requirement. Does it work on slow or intermittent connections? The file is downloaded once and cached locally. On slow connections the initial download takes longer but succeeds. On intermittent connections, the cached local copy works offline indefinitely, and the update check can retry on the next available connection. This is the same pattern SAT already uses for language standards, and it already handles this gracefully.

**Cognitive accessibility**

Lens: user.

- Rating: Strong
- Evidence: Inferred
- Notes: Is it learnable without training? SPDX identifiers are designed to be human-readable short strings. `MIT` is self-evident. `AGPL-3.0-or-later` reads as plain English. The expression syntax (`AND`, `OR`, `WITH`, `+`) uses everyday words. Does it use plain language? The identifiers are plain language by design. The JSON data file's structure (`licenseId`, `name`, `isOsiApproved`, `isFsfLibre`, `isDeprecatedLicenseId`) uses self-documenting field names. Is it forgiving of errors, and does it explain them without blame? The data file itself does not produce errors, SAT's validation module would. The design intent for that module (described under Why interesting) is to warn on deprecated identifiers and suggest the current replacement, report whether a licence is OSI/FSF-approved, and detect expression syntax errors, all of which are corrective rather than punitive. Whether the SAT module achieves this depends on its implementation, not on the SPDX data.

**Compatibility**

Lens: owner, user.

- Rating: Strong
- Evidence: Verified
- Notes: Is it likely to be more or less compatible with:
  - My hardware: compatible with any hardware that can store and parse a text file
  - My operating system(s): JSON is OS-agnostic, SAT runs on Linux, macOS, and Windows, the data file works on all three
  - My web browser(s): not browser-dependent, though the HTML format is viewable in any browser
  - My input device(s): not relevant, the data file is consumed programmatically by SAT
  - My output device(s): validation output is text, displayable on any output device

### Representation

Lens: community, user.

- Rating: not applicable
- Evidence: not applicable
- Notes: Representation distance: the SPDX License List is maintained by the SPDX legal team, a working group of the Linux Foundation composed of lawyers, compliance professionals, and open source practitioners. The "designed-for" population is software developers and compliance teams worldwide. The designer/designed-for gap is minimal because the maintainers are themselves practitioners of the domain. However, this is a standards data file, not an interactive product, and the representation questions (disabled representation, compensated user research, reading level, device assumptions) target interactive products where design decisions create exclusion. A licence identifier list does not create that dynamic. Rated not applicable rather than Unknown because the question genuinely does not apply, not because information is missing.

### Resilience

Lens: owner, user.

- Rating: Strong
- Evidence: Inferred
- Notes: Does it continue to work when:
  - It goes out of date: yes. A cached copy of the SPDX License List remains valid indefinitely. Older versions can still validate every identifier that existed at the time of that version. SPDX endeavours to never change identifiers, only to add new ones or deprecate old ones (retaining the deprecated identifier as valid). An out-of-date local copy would fail to recognise only identifiers added after the cached version, which is a graceful degradation.
  - During power outages: it adds no infrastructure that can fail. The cached JSON file is a static file on disk, readable whenever the machine running SAT is powered on.
  - During network outages: the cached local copy functions offline with no degradation. The only network-dependent operation is checking for updates, which can be deferred until connectivity returns. This is identical to the language standards pattern SAT already handles.

### Agency

**Sovereignty & Privacy**

Lens: owner, user.

- Rating: Strong
- Evidence: Verified
- Notes:
  - Does this increase or decrease sovereignty? Increases. SAT gains the ability to validate its own licence compliance locally, without depending on any external service at runtime. The data is downloaded, cached, and owned locally.
  - Can the owner self-host, modify, fork, and redistribute without permission? Yes. CC-BY-3.0 permits all of these with attribution. The source XML is also open, so the entire generation pipeline can be forked if needed.
  - Where does user data rest? There is no user data. The data flow is one-directional, SAT downloads a public dataset. SAT sends nothing back.
  - Does it phone home? No. The data source has no telemetry, no analytics, no tracking. SAT controls when and whether to check for updates. **[GATE]** status: **Pass**.
  - When the user and the product disagree, who wins? The user. SAT can pin to any version, skip updates, modify the cached file, or stop using it entirely. There is no mechanism by which the data source can override the user's choices.
  - Reversibility test: "If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?" Answer: zero. Delete one JSON file. SAT continues to function, it just loses licence identifier validation.

Power-imbalance proxies (vendor↔user):

- **Exit cost.** Zero hours, zero dollars. Delete a file.
- **Data portability.** The data is the portable format. `licenses.json` is the export.
- **Terms-of-service volatility.** CC-BY-3.0 has been the licence since the project's inception. No changes in roughly fifteen years.
- **Pricing asymmetry.** Not applicable. No pricing exists.

**Interaction Patterns**

Lens: user.

- Rating: not applicable
- Evidence: not applicable
- Notes: A static data file consumed programmatically has no interaction surface. It does not present defaults, it does not present exit friction, it does not notify, it does not ask for consent. The supportive/dark pattern pairs from the metrics document do not apply. SAT's validation module, which will present messages to users, will have an interaction surface, but that is SAT's own code, not SPDX's data.

### Sustainability

**Environment, direct and indirect impacts**

Lens: environment, owner, user.

- Rating: Strong
- Evidence: Verified
- Notes:
  - Direct impacts, does it use more or less energy: negligible on both the user's system (one JSON parse, cached) and the service provider's system (one static file served from GitHub's CDN, cached).
  - Indirect impacts, does it require new hardware or hardware upgrades: no, for the product's host (GitHub already hosts it), for the user (SAT already runs, no new dependency), or for others (no third-party impact).
  - Bandwidth: the payload (one JSON file under 1 MB) is served once and cached. Periodic update checks compare a version tag, not re-download the full file unless the version has changed.

### Security

Lens: owner, user.

- Rating: Strong
- Evidence: Inferred
- Notes:
  - **What data does it collect, and where does that data rest?** It collects no data. SAT downloads a public dataset. The downloaded file rests in SAT's local cache directory, controlled by the user.
  - **How are vulnerabilities reported, and is there a published security policy?** The SPDX project is a Linux Foundation project and follows the Linux Foundation's security practices. The `license-list-data` repository is generated data, not executable software, so the vulnerability surface is the generation pipeline (`license-list-XML` and `LicenseListPublisher`), not the data files themselves. Security issues in the generation tools would be reported via the SPDX project's GitHub.
  - **How quickly are security patches released, and is the project responsive to reports?** Not independently confirmed for the generation tools. The data files themselves cannot contain vulnerabilities because they are not executable.
  - **What is the supply chain exposure?** Zero runtime dependencies. The data is a JSON file. It does not import, include, or execute anything. SAT's integration code will have its own supply chain, but that is SAT's code, not SPDX's data.
  - **Does adding it expand or shrink the attack surface of the system it joins?** Shrinks, on balance. The data file itself adds no attack surface (it is not executable). The validation capability it enables helps ensure licence compliance, which reduces the risk of unintentional licence violations, a legal rather than technical attack surface.
  - **Assessment method:** Documentation review, GitHub repository inspection, SPDX specification review, `licenses.json` file structure inspection. No network monitor used (not applicable, the data file is not an application). Date: 2026-07-25.

**Network behaviour:**
- [x] Outbound connections during normal use — one HTTPS GET to GitHub to download or check for updates, frequency controlled entirely by SAT, not by the data source
- [ ] Update checks — SAT controls the check schedule, the data source does not push, does not notify, and has no mechanism to initiate contact
- [ ] Telemetry or usage data — none, the data source has no way to know who downloaded it or when
- [ ] Licence server contact — none
- [ ] Affects networking or items created that will be transferred over a network
- [x] Helps ensure that documents passed via network are clean and compliant — validated licence identifiers in front matter reduce the risk of non-compliant documents being distributed

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content — no, the cached JSON file is placed where SAT decides
- [ ] Caches content outside the working directory — SAT controls the cache location, the data source has no opinion
- [ ] Stores recent file history outside the archive — no
- [ ] Writes to unexpected locations — no

**Content exposure:**
- [ ] Sends any content to a remote service — no, the data flow is strictly inbound
- [ ] Stores content in a cloud service by default — no
- [ ] Auto-save or backup features that copy content externally — no

**Assessment status:** Complete. The data file is not executable software, so the security assessment is inherently simpler than for tools or applications. SAT's integration code will need its own security review when written, but that review is of SAT's code, not of this data source.

### The Product or Service Itself

**Longevity**

Lens: owner, community.

- Rating: Strong
- Evidence: Verified
- Notes: Is this more or less supported over the longer term? More. The SPDX Specification is published as ISO/IEC 5962:2021. The licence list has been maintained continuously since 2011 and is currently at version 3.28.0 (released 2026-02-20). Institutional backing: Linux Foundation. Funding model: foundation-supported, not dependent on any single company. Contributor pool: the SPDX legal team includes representatives from multiple organisations. Release cadence: multiple releases per year (3.28.0 in February 2026, preview builds ongoing). The SPDX License List is used by GitHub, GitLab, npm, PyPI, crates.io, and effectively every major package manager and code hosting platform. This is foundational infrastructure for the entire open source ecosystem.

**Content endurance**

Lens: owner, community.

- Rating: Strong
- Evidence: Verified
- Notes: Would it make content behind the interface more or less likely to endure over time? More. SPDX endeavours to never change licence identifiers. Deprecated identifiers remain valid and their URLs are retained. A cached copy of any version remains usable indefinitely. If the SPDX project were removed, phased out, or replaced, the cached local data would continue to function. The identifiers are already embedded in millions of source files worldwide, making them a de facto permanent standard regardless of the SPDX project's future. Is content authored inside the tool, or merely enhanced by it? Neither, this is reference data, not a content tool. SAT's licence validation capability would be an enhancement layer over the documents in the archive, the documents exist independently and the enhancement layer leaves them whole if removed.

**Exit and portability**

Lens: owner, user. **[GATE]** if no exit exists.

- Rating: Strong
- Evidence: Verified
- Notes: Can data and content be exported in open formats? The data is the open format. `licenses.json` is JSON, readable by any tool in any language. Does it rely on open standards or proprietary ones? The SPDX Specification is an ISO standard. The data formats (JSON, HTML, text) are open. What is the realistic migration path to a successor? If a successor standard emerged, the identifiers themselves (being short strings) would likely be compatible or trivially mappable. The cached JSON file can be replaced with any equivalent dataset.
- Gate status: **Pass**. Exit is not a meaningful concept for a JSON file.

**Adjustability and support**

Lens: owner, community.

- Rating: Strong
- Evidence: Inferred
- Notes: If there is a problem or a change needed, is it likely or unlikely that adjustment could happen? Likely. The licence list source XML is open (CC-BY-3.0). The generation tools (`LicenseListPublisher`) are open source. The process for requesting new licences or reporting errors is documented and public (GitHub issue, SPDX legal team reviews, documented criteria for inclusion). The SPDX legal team is reachable and responsive. The fork-and-fix path is fully open, a local modification to the cached JSON file is trivial and carries no legal or technical barrier.

**Market Position**

Lens: community, owner.

- Rating: not applicable (standard, not platform)
- Evidence: not applicable
- Notes: SPDX is a standard, not a platform. There is no platform↔ecosystem imbalance because there is no platform.
  - **Market concentration.** SPDX is the only ISO-standardised licence identifier list. This is not market concentration in the sense the metrics document targets (a dominant player taxing, demoting, or cloning competitors), it is a standard that everyone uses because it is the standard, the same way UTC is the time standard.
  - **Take rate.** None.
  - **API stability and deprecation history.** SPDX endeavours to never change identifiers. Deprecation is handled by retaining the old identifier as valid while pointing to the replacement. The JSON schema has been stable across major versions. This is the strongest API stability finding on the radar.
  - **Forkability.** Fully forkable. CC-BY-3.0, open source generation tools, no governance structure that could turn hostile. The data could survive the SPDX project disappearing entirely.
  - **Contributor concentration.** The SPDX legal team includes representatives from multiple organisations under the Linux Foundation umbrella. No single company controls merge authority. This is foundation-governed, not single-vendor.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Inferred | Multiple formats, plain-text identifiers, increases number of people who can access compliant content |
| Multilingual integration | Strong / Moderate | Inferred | Identifiers are language-neutral codes, licence texts are English (property of the licences themselves), adding licences is a public data submission |
| Economic and cognitive accessibility | Strong | Verified / Inferred | Free, no auth, under 1 MB, runs on any hardware, cached offline, short memorable identifiers, plain-language expression syntax, self-documenting JSON fields |
| Representation | N/A | N/A | Standards data file, not an interactive product, designer/designed-for gap does not apply |
| Compatibility | Strong | Verified | JSON, every OS, every language, no binary format, no platform dependency |
| Resilience | Strong | Inferred | Self-contained once cached, works offline indefinitely, graceful degradation if out of date, no infrastructure added |
| Agency, sovereignty & privacy | Strong | Verified | CC-BY-3.0, no vendor, no telemetry, no tracking, user controls all fetch and cache behaviour, forkable |
| Agency, power-imbalance proxies | Strong | Verified | Zero exit cost, data is the portable format, CC-BY-3.0 unchanged since inception, no pricing |
| Agency, interaction patterns | N/A | N/A | Static data file consumed programmatically, no interaction surface |
| Environment, direct and indirect | Strong | Verified | One sub-1 MB file, cached, no new hardware, negligible bandwidth |
| Security | Strong | Inferred | Not executable, no supply chain, no attack surface expansion, HTTPS fetch, shrinks legal attack surface via compliance |
| Longevity | Strong | Verified | ISO/IEC 5962:2021, maintained since 2011, v3.28.0, Linux Foundation, used by every major platform |
| Content endurance | Strong | Verified | Identifiers never change, deprecated IDs retained, cached copies valid indefinitely, de facto permanent standard |
| Exit and portability | Strong | Verified | JSON file, trivially replaceable, ISO standard, not a meaningful exit scenario |
| Adjustability and support | Strong | Inferred | Open source, CC-BY-3.0, public submission process, responsive legal team, fork-and-fix fully open |
| Market position | N/A (standard) | N/A | Not a platform, foundation-governed, multi-org contributors, strongest API stability on radar, fully forkable |
| Gates | **Pass** (all four) | | CC-BY-3.0 compatible with AGPL-3.0-or-later, no telemetry (no mechanism for it), no accessibility concern, exit trivial |

## Relationship to project

SAT content tools, specifically the document validation pipeline. The SPDX License List data file would sit alongside the language standards data SAT already downloads, in the same local cache, checked on the same schedule, used for the same kind of front matter validation. On adopt, the integration work lands in the `sat` repository as a validation module, not in `sat-doc-automa` or `uc-radar`.

## Status notes

- Last reviewed: 2026-07-25
- In assess: full evaluation complete, automated recommendation is adopt at low risk. No unknowns remain. All gates pass. The only remaining step is writing the SAT integration code, which is implementation work, not evaluation work.
- What would move it to adopt: a human confirming the recommendation and writing the SAT module that downloads, caches, and validates against `licenses.json`.
- What would move it to hold: nothing plausible. The SPDX License List is the ISO standard for the identifiers UC already uses in every document.

## Links

- https://spdx.org/licenses/
- https://github.com/spdx/license-list-data
- https://github.com/spdx/license-list-XML
- https://spdx.dev/learn/handling-license-info/
- https://spdx.github.io/spdx-spec/v3.0.1/annexes/spdx-license-expressions/

## License

This document, *SPDX License List, Machine-Readable License Identifiers*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Completed full question-by-question assessment against every item in Universal Cake Evaluation Metrics v0.3.1, answering each specific sub-question rather than summarising at the section level |
| 0.1.0 | Draft | Initial entry, evaluation at section level |

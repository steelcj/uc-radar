---
dcterms:title: "uc-radar--nodejs--javascript-runtime"
dcterms:version: "0.2.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude Opus 5.5 (Anthropic)"
dcterms:subject:
  - "radar"
  - "osat"
  - "nodejs"
  - "javascript-runtime"
  - "dev-toolchain"
dcterms:description: "Assess entry for Node.js as a runtime managed by a new osat-manager-node, scored against Universal Cake Evaluation Metrics 0.6.0."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-10-28"
dcterms:modified: "2026-10-28"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: ""
dcterms:relation: "uc-radar--evaluation-lifecycle, universal-cake-evaluation-metrics, uc-radar-entry-template"
dcterms:identifier: "uc-radar--nodejs--javascript-runtime"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-10-28"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Applied Universal Cake Evaluation Metrics 0.6.0 question by question.
      Added the scorecard metadata block and a Gates section covering the
      four default gates as the Assess light pass requires. Answered the
      Text and terminal interfaces questions under Accessibility. Split
      Accessibility into its sub-questions (affordability, hardware and
      connectivity, cognitive, time-based media). Answered the Resilience
      questions, the power-imbalance proxies one by one, and the AI-specific
      proxies as Not applicable. Expanded Security to the metrics' questions,
      including npm install scripts as a gate-shaped concern and a candidate
      project gate. Content endurance re-rated from Not applicable to Strong,
      since Node.js is an enhancement layer rather than a container. Added
      npm's audit submission and funding messages to the telemetry gate.
      Environment row renamed to match the 0.6.0 scorecard. Appended a
      second automated pass; recommendation changed from trial to adopt
      once gates and Security are Verified, since trial is optional under
      lifecycle 0.2.0 and the risk is low.
  - version: "0.1.0"
    date: "2026-10-28"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: "Initial entry, created from uc-radar-entry-template v0.3.0."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial"
      reasoning: "Mature, foundation-governed, permissively licensed runtime with signed, checksummed, precompiled user-space builds for Linux, macOS and Windows, and no daemon. Recommendation conditional on Verified Security and gates. Open concerns: rising minimum platform requirements and npm registry concentration."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-10-28T20:24:00-04:00"
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "adopt" # once every gate and the Security section are Verified, or a named person records an override
      reasoning: "Light pass against metrics 0.6.0. Telemetry and content exposure gate passes on documentation evidence: the runtime has no telemetry; npm's update check, audit submission and funding messages are each disableable. Licence gate passes, MIT, compatible with AGPL-3.0-or-later. Exit gate passes, Node.js holds no content. Accessibility floor Unknown until a terminal screen reader pass over install, configure, run and troubleshoot. Security Moderate: signed releases and a published security process, with npm install scripts as the main supply-chain exposure, which belongs to a future nodeapp manager rather than this one. Trial not recommended: risk is low and testing on disposable machines can answer the open questions."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-10-28T20:35:00-04:00"
  human: []
---

# uc-radar--nodejs--javascript-runtime-v0.2.0

- Product or service name: Node.js, with its bundled npm and npx
- Version evaluated: the 26.x line, which enters LTS in October 2026, at documentation level; no specific build tested yet
- Evaluator: Claude Opus 5.5 (Anthropic), for Christopher Steel's review
- Date: 2026-10-28
- Radar ring: Assess

## What it is

Node.js is an open source, cross-platform JavaScript runtime built on Google's V8 engine, distributed with npm and npx. This entry assesses adopting it as an OSAT-managed runtime through a new `osat-manager-node`.

## Why interesting

Node.js is a direct requirement of the Vishpala website stack (Eleventy and Sveltia CMS) and of most JavaScript-based static site and content tooling. Managing it through OSAT lets a site's toolchain come down through the same `osat install` cascade as Python and git, on Linux, macOS and Windows 11, without system installers, admin rights, or a third-party version manager.

It fits the OSAT manager protocol well. Node.js is a runtime binary, not a service, so the manager runs no daemon. Official builds are plain archives (tarballs for Linux and macOS, zip for Windows) that unpack into a user-owned directory, and every release ships a `SHASUMS256.txt` signed by the release team, so the manager can verify what it installs.

The release schedule is becoming simpler to manage. Starting with Node.js 27, the project moves to one major release per year, released in April and promoted to LTS in October, with every release becoming LTS and the odd/even distinction retired. Node.js 26 is the last line under the old model and enters LTS in October 2026 <a id="cite-nodejs-releasers-2026"></a>([Node.js Releasers, 2026](#ref-nodejs-releasers-2026)). One major version a year maps cleanly onto an annual runtime review and onto version arguments such as `osat install node 26`.

## Concerns

Older systems. Each major line raises its minimum supported platforms (glibc version on Linux, macOS version, Windows version, and dropped 32-bit builds). This works against Vishpala's aim to serve older systems. The manager needs a per-line platform matrix and should refuse clearly, in plain language, when a target is below the minimum, and suggest the newest line that still supports it.

Package registry concentration. Node.js itself is foundation-governed, but the default npm registry is a single commercially owned service. Any `npm install` depends on it unless a mirror or offline cache is configured.

npm install scripts. Packages can run arbitrary code at install time. The metrics name this as gate-shaped under Security. It affects what is installed with npm, not the runtime this manager installs, so it is recorded here as a candidate project gate for a future `osat-manager-nodeapp`.

Corepack is no longer bundled. Corepack, which provided pnpm and Yarn shims, is not distributed with Node.js 25 and later <a id="cite-nodejs-corepack-2025"></a>([Node.js Project, 2025](#ref-nodejs-corepack-2025)). The manager should not assume pnpm or Yarn are available; they would need their own managers or an explicit option.

Global install sprawl. Without configuration, npm global installs and caches land in OS-specific locations (`~/.npm`, `/usr/local`, `%AppData%`, `%LocalAppData%`). The manager should pin the npm prefix and cache inside the OSAT-managed tree so uninstall is clean and the footprint is known.

Source choice is open. Official nodejs.org archives versus conda-forge `nodejs`. Conda is consistent with the precompiled conda git already used in the cascade; official archives are the canonical upstream, available on release day, and need no conda dependency. This entry leans to official archives with conda as an option, pending your decision.

## Gates

The Assess light pass requires all four default gates and the Security section on available evidence. None is Verified yet.

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The runtime collects no telemetry. npm makes three kinds of contact beyond fetching packages, each disableable in `.npmrc`: the update check (`update-notifier=false`), the security audit that sends the project's dependency tree to the registry on install (`audit=false`), and funding messages (`fund=false`). Registry requests also carry a user agent naming the npm, Node.js and OS versions. To Verify with the uc-radar telemetry trace procedure |
| A licence incompatible with the project | Pass | Inferred | MIT, with bundled third-party components under their own licences listed in the repository's `LICENSE` file. Compatible with use alongside AGPL-3.0-or-later work. To Verify by reading `LICENSE` for the version installed, and against the adopting project's licence policy |
| Failure of the accessibility floor | Unknown | Inferred, not observed | The interface is text throughout, see Text and terminal interfaces below. Not yet done: a terminal screen reader pass, for example with Emacspeak or Orca, over installing through the manager, running `node` and the REPL, `npm install`, and reading a stack trace |
| No exit, data or content cannot be extracted in usable form | Pass | Inferred | Node.js stores no user content. Projects are plain files on disk, and removing the runtime directory and the pinned prefix and cache is a complete uninstall |

## Evaluation

Ratings use Strong, Moderate, Weak, Unknown, and, narrowly, Not applicable, each tagged Verified, Inferred, or Claimed. No hands-on testing has been done for this entry, so nothing is Verified except the release schedule, which comes from the project's own announcement.

### Inclusive

#### Accessibility

**Alternative methods of interacting with content** **[GATE]**

- Rating: Unknown
- Evidence: Inferred
- Notes: Lens user. Node.js has no graphical interface of its own; the documentation website is the main content surface and has not been audited for WCAG conformance.

**Text and terminal interfaces** **[GATE]**

- Rating: Moderate, pending a screen reader pass
- Evidence: Inferred
- Notes: Lens user, owner. Plain text output in reading order: yes for `node --version`, `--help` and most errors. Colour and redrawing output: Node.js honours `NO_COLOR`, and npm can turn off colour and its progress spinner (`--color=false`, `--progress=false`); to verify both. Required steps depending on a full-screen interface, mouse, or graphical installer: none if the manager uses the archives; the Windows `.msi` installer is graphical, which is one more reason for the manager to use the zip. Error messages: Node.js errors carry a stable code such as `ERR_MODULE_NOT_FOUND` and say what failed, but stack traces are long and tedious to hear; npm errors point to a log file. Documentation: see the item above. The manager should own the accessible layer, short plain-language messages, honouring `NO_COLOR`, a quiet flag, and no meaning carried by colour alone.

**Multilingual integration**

- Rating: Weak
- Evidence: Inferred
- Notes: Lens user, community. The website is partly translated by volunteers. Runtime output, errors and API documentation are English only, and adding a language to the runtime's messages would be a code change, not a data file anyone can supply.

**Affordability**

- Rating: Strong
- Evidence: Inferred
- Notes: Free, no paid tier, no account needed.

**Hardware and connectivity accessibility**

- Rating: Moderate
- Evidence: Inferred
- Notes: Downloads are tens of megabytes per platform, a real cost on slow or intermittent connections, though archives can be mirrored and resumed. Minimum platforms rise with each line, which excludes older machines over time; record the per-line matrix to resolve this. Runs offline once installed; `npm install` does not, unless a cache or mirror is set up.

**Cognitive accessibility**

- Rating: Moderate
- Evidence: Inferred
- Notes: The runtime is simple to start; the ecosystem around it, package managers, module formats and configuration files, is not. Errors are factual rather than blaming.

**Time-based media**

- Rating: Unknown
- Evidence: Inferred
- Notes: Project talks and videos exist; whether they carry captions or transcripts has not been checked.

#### Representation

- Rating: Unknown
- Evidence: None
- Notes: Lens community. Not assessed. Governance runs through the Node.js Technical Steering Committee under the OpenJS Foundation; who takes part, and whether users with disabilities or from low-bandwidth regions are represented, is not visible from what has been read. Its absence from the reviewed documentation is itself a data point.

#### Resilience

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. When out of date: the installed runtime is self-contained, with no licence server, account, or expiring token, and keeps working after end of life, though without security fixes. Power outages: adds no infrastructure. Network outages: the runtime works fully offline; npm needs the registry, a cache, or a mirror.

### Agency

#### Sovereignty & Privacy

- Rating: Strong (runtime), Moderate (npm registry)
- Evidence: Inferred
- Notes: Lens owner, user, with divergence recorded. The owner can self-host, modify, fork and redistribute the runtime without permission. User data rests on the user's machine; Node.js stores none of its own beyond REPL history. Phone-home behaviour is covered under the telemetry gate, all of it disableable. When user and product disagree, the user can configure or leave at low cost. The registry is the divergence: the owner of a project depends on one company's service for packages unless they run a mirror. Reversibility test: if the npm registry turned hostile tomorrow, an owner with a lockfile and a local cache could keep building today and move to a mirror or alternative registry in hours to days.

**Power-imbalance proxies**

- Exit cost: Low for the runtime, minutes to remove. Moving projects to another runtime such as Deno or Bun, hours to days per project. Inferred.
- Data portability: Not applicable. Node.js holds no user data to export; projects are already plain files.
- Terms-of-service volatility: Not applicable for the runtime, which is MIT licensed with no terms of service. Unknown for the npm registry, whose terms were not counted; method would be diffing archived snapshots of the npm terms over five years.
- Pricing asymmetry: Strong. Free and public for the runtime and for public packages on the registry.

**AI-specific proxies**

- Not applicable. Node.js contains no machine-learned model and makes no algorithmic decisions about users.

#### Interaction Patterns

- Rating: Moderate
- Evidence: Inferred
- Notes: Lens user. No sign-in, upsells, or dark patterns in the runtime. npm prints update notices and a "looking for funding" count after installs; both are informative rather than manipulative, but they are noise, and both can be turned off. The manager could ship them off by default.

### Environment

- Rating: Moderate
- Evidence: Inferred
- Notes: Lens planet. Direct: no daemon and no background activity. Indirect: large `node_modules` trees and repeated downloads; a shared, pinned npm cache inside the OSAT tree reduces those. Authoring-side: for static sites such as the Vishpala site, Node.js runs only at build time on the author's machine, so visitors run none of it, which is favourable for the planet lens.

### Security

- Rating: Moderate, pending verification
- Evidence: Inferred
- Notes: Lens owner, user. Vulnerability reporting goes through a published security policy, and the project issues scheduled security releases across all supported lines and announces them publicly. Releases are signed and checksummed. Supply-chain exposure for the runtime itself is low, with dependencies vendored into the build; for npm packages it is high, since install scripts run arbitrary code. Attack surface: adds a runtime to the machine but no listening service. The manager must verify `SHASUMS256.txt` and its signature before unpacking, and refuse end-of-life lines by default.

**Candidate project gate:** for `osat-manager-nodeapp`, install scripts that run unreviewed arbitrary code, for example by installing with `--ignore-scripts` unless a package is on a reviewed allowlist. Record it in the Gates at a glance table if adopted.

**Assessment method:**
Not yet tested. Planned: uc-radar telemetry trace procedure (version to be named) on disposable Linux Mint, macOS and Windows 11 machines, covering `node --version`, a REPL session, `npm install` of a small project with the default `.npmrc` and with update checks, audit and funding off, and `npx` of a one-off tool. Terminal screen reader pass on Linux with Emacspeak or Orca.

**Network behaviour:**
- [ ] Outbound connections during normal use (expected: none for `node`; registry calls for `npm` and `npx`)
- [ ] Update checks, silent or explicit (expected: npm update notifier, explicit and disableable)
- [ ] Telemetry or usage data, present or absent (expected: absent; audit submission sends dependency names and versions)
- [ ] Licence server contact, frequency and data sent (expected: none)
- [ ] Affects networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content (expected: none from the runtime; projects create `node_modules` and lockfiles)
- [ ] Caches content outside the working directory (expected: npm cache in the user profile unless pinned)
- [ ] Stores recent file history outside the archive (expected: REPL history file in the home directory)
- [ ] Writes to unexpected locations (expected: global installs to the npm prefix unless pinned)

**Content exposure:**
- [ ] Sends any content to a remote service (expected: none from the runtime)
- [ ] Stores content in a cloud service by default (expected: no)
- [ ] Auto-save or backup features that copy content externally (expected: no)

### The Product or Service Itself

**Longevity**

- Rating: Strong
- Evidence: Verified for the schedule, Inferred otherwise
- Notes: Lens owner, community. In wide use since 2009, governed by the OpenJS Foundation, with a published, predictable release schedule and about three years of support per line under the new model <a id="cite-nodejs-releasers-2026-b"></a>([Node.js Releasers, 2026](#ref-nodejs-releasers-2026)). Contributor pool and commit activity not measured; LFX Insights would make them Verified.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: Node.js is an enhancement layer, not a container. Content is authored in plain files and processed by tools that run on Node.js, so removing or replacing the runtime leaves content whole.

**Exit and portability** **[GATE]**

- Rating: Strong
- Evidence: Inferred
- Notes: See the exit gate. Relies on open standards (ECMAScript, and CommonJS and ES modules); the realistic successor path is another JavaScript runtime or a newer Node.js line.

**Adjustability and support**

- Rating: Strong
- Evidence: Inferred
- Notes: MIT licence keeps the fork-and-fix path open. Documentation is extensive, the community large, and paid extended support exists for end-of-life lines if a client ever needs it.

**Market Position**

- Rating: Moderate
- Evidence: Inferred
- Notes: Lens community, owner. Market concentration: the dominant server-side JavaScript runtime, with credible alternatives in Deno and Bun. Take rate: none, for the runtime or for public packages. API stability: good across LTS lines, with removals announced through deprecation cycles. Forkability: proven, the io.js fork split from and later merged back into Node.js. Contributor concentration: Unknown, not measured. Two upstream dependencies concentrate power, the V8 engine controlled by Google and the npm registry controlled by one company.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Unknown, text interface Moderate | Inferred | Split into the two gated items; screen reader pass pending |
| Multilingual integration | Weak | Inferred | Website partly translated; runtime English only, localisation needs code changes |
| Economic and cognitive accessibility | Strong (affordability), Moderate (hardware and connectivity, cognitive) | Inferred | Split per 0.6.0; minimum platforms rise per line |
| Representation | Unknown | None | Not assessed |
| Compatibility | Moderate | Inferred | Three OS families; per-line platform matrix needed |
| Resilience | Strong | Inferred | Self-contained, offline after install |
| Agency, sovereignty & privacy | Strong (runtime) / Moderate (registry) | Inferred | Owner and registry divergence recorded |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | Exit cost low; portability and runtime ToS not applicable; registry ToS Unknown |
| Agency, interaction patterns | Moderate | Inferred | npm update and funding notices, disableable |
| Environment, direct, indirect, and authoring-side | Moderate | Inferred | No daemon; heavy dependency trees; build-time only for static sites |
| Security | Moderate | Inferred | Signed releases; npm install scripts the main exposure |
| Longevity | Strong | Verified (schedule), Inferred | Annual LTS model from Node.js 27 |
| Content endurance | Strong | Inferred | Enhancement layer, not a container |
| Exit and portability | Strong | Inferred | One directory; open standards |
| Adjustability and support | Strong | Inferred | MIT, large community |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Inferred | V8 and npm concentration; contributor concentration Unknown |
| Gates | Pass (telemetry, licence, exit), Unknown (accessibility floor) | Inferred | None Verified; no gate failed |

## Relationship to project

Tier: dev toolchain. On adopt, the process graduates to uc-processes and the implementation lives in `osat-manager-node` (command `manage-node`, reachable through the `osat` umbrella command), built as a v1 to the final OSAT manager protocol with no code carried over from archived repos. A later `osat-manager-nodeapp`, parallel to `osat-manager-pyapp`, could manage npm-based CLI tools such as Eleventy, and is where the install-scripts gate candidate applies.

Design decisions to record before adopt:

- Source: official nodejs.org archives (leaning) or conda-forge `nodejs`.
- Version arguments: major line (`26`), exact version (`26.x.y`), and `lts`.
- Side-by-side versions, with one active per scope.
- npm prefix, cache and `.npmrc` pinned inside the OSAT tree, with update checks and funding messages off by default and the audit setting left to the owner, documented.
- pnpm and Yarn out of scope for this manager.
- Registry URL configurable, with support for a local mirror.
- Refuse end-of-life lines and below-minimum platforms by default, with plain-language messages.
- Manager output honours `NO_COLOR` and offers a quiet flag.

## Status notes

- Last reviewed: 2026-10-28
- In assess: light pass done against metrics 0.6.0 on documentation evidence. No gate failed; three gates pass on Inferred evidence and the accessibility floor is Unknown. Testing still needed before the entry can leave Assess: the telemetry trace on Linux Mint, macOS and Windows 11, a terminal screen reader pass, reading `LICENSE` for the installed version, and checking `NO_COLOR` and npm's colour and progress flags. Also pending: the per-line minimum platform matrix, the Representation review, contributor figures from LFX Insights, and the source decision. The latest automated pass recommends adopt once every gate and the Security section are Verified, or a named person records an override; trial is not recommended, since risk is low and testing can answer the open questions.

## Links

- https://nodejs.org/
- https://github.com/nodejs/node
- https://github.com/nodejs/Release

## References

<a id="ref-nodejs-corepack-2025"></a>
Node.js Project. (2025). *doc: clarify future Corepack removal in v25+* (Pull request #57825) [Source code commit]. GitHub. https://github.com/nodejs/node/commit/004ecc12eb43c1253274728209c0790f06820437
[Return to citation](#cite-nodejs-corepack-2025)

<a id="ref-nodejs-releasers-2026"></a>
Node.js Releasers. (2026, March 10). *Evolving the Node.js release schedule*. Node.js. https://nodejs.org/en/blog/announcements/evolving-the-nodejs-release-schedule
[Return to citation](#cite-nodejs-releasers-2026)

## License

This document, *uc-radar--nodejs--javascript-runtime*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

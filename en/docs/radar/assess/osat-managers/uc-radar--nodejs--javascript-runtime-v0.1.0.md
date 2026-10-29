---
dcterms:title: "uc-radar--nodejs--javascript-runtime"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude Opus 5.5 (Anthropic)"
dcterms:subject:
  - "radar"
  - "osat"
  - "nodejs"
  - "javascript-runtime"
  - "dev-toolchain"
dcterms:description: "Assess entry for Node.js as a runtime managed by a new osat-manager-node, built as a v1 to the final OSAT manager protocol."
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
  - version: "0.1.0"
    date: "2026-10-28"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: "Initial entry, created from uc-radar-entry-template v0.3.0."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # adopt / trial / hold / rejected
      reasoning: "Mature, foundation-governed, permissively licensed runtime with signed, checksummed, precompiled user-space builds for Linux, macOS and Windows, and no daemon. Recommendation is conditional: no rating is yet Verified by testing, so the entry stays in assess until the Security pillar and gates are verified. Main open concerns are rising minimum platform requirements for older systems and concentration around the npm registry."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-10-28T20:24:00-04:00"
  human: []
---

# uc-radar--nodejs--javascript-runtime-v0.1.0

## What it is

Node.js is an open source, cross-platform JavaScript runtime built on Google's V8 engine, distributed with npm and npx. This entry assesses adopting it as an OSAT-managed runtime through a new `osat-manager-node`.

## Why interesting

Node.js is a direct requirement of the Vishpala website stack (Eleventy and Sveltia CMS) and of most JavaScript-based static site and content tooling. Managing it through OSAT lets a site's toolchain come down through the same `osat install` cascade as Python and git, on Linux, macOS and Windows 11, without system installers, admin rights, or a third-party version manager.

It fits the OSAT manager protocol well. Node.js is a runtime binary, not a service, so the manager runs no daemon. Official builds are plain archives (tarballs for Linux and macOS, zip for Windows) that unpack into a user-owned directory, and every release ships a `SHASUMS256.txt` signed by the release team, so the manager can verify what it installs.

The release schedule is becoming simpler to manage. Starting with Node.js 27, the project moves to one major release per year, released in April and promoted to LTS in October, with every release becoming LTS and the odd/even distinction retired. Node.js 26 is the last line under the old model and enters LTS in October 2026 <a id="cite-nodejs-releasers-2026"></a>([Node.js Releasers, 2026](#ref-nodejs-releasers-2026)). One major version a year maps cleanly onto an annual runtime review and onto version arguments such as `osat install node 26`.

## Concerns

Older systems. Each major line raises its minimum supported platforms (glibc version on Linux, macOS version, Windows version, and dropped 32-bit builds). This works against Vishpala's aim to serve older systems. The manager needs a per-line platform matrix and should refuse clearly, in plain language, when a target is below the minimum, and suggest the newest line that still supports it.

Package registry concentration. Node.js itself is foundation-governed, but the default npm registry is a single commercially owned service. Any `npm install` depends on it unless a mirror or offline cache is configured.

Corepack is no longer bundled. Corepack, which provided pnpm and Yarn shims, is not distributed with Node.js 25 and later <a id="cite-nodejs-corepack-2025"></a>([Node.js Project, 2025](#ref-nodejs-corepack-2025)). The manager should not assume pnpm or Yarn are available; they would need their own managers or an explicit option.

Global install sprawl. Without configuration, npm global installs and caches land in OS-specific locations (`~/.npm`, `/usr/local`, `%AppData%`, `%LocalAppData%`). The manager should pin the npm prefix and cache inside the OSAT-managed tree so uninstall is clean and the footprint is known.

Source choice is open. Official nodejs.org archives versus conda-forge `nodejs`. Conda is consistent with the precompiled conda git already used in the cascade; official archives are the canonical upstream, available on release day, and need no conda dependency. This entry leans to official archives with conda as an option, pending your decision.

## Evaluation

Rate every item Strong, Moderate, Weak, or Unknown, and tag each rating Verified, Inferred, or Claimed. No hands-on testing has been done for this entry yet, so ratings are Inferred or Claimed unless a source is cited. Gate markings are pending confirmation against metrics 0.6.0.

### Inclusive

**Accessibility**, including the text interface, commands, configuration, logs, errors, and documentation, for command-line tools and developer infrastructure

- Rating: Moderate
- Evidence: Inferred
- Notes: Everything is plain text, commands, flags, environment variables and config files, which works with screen readers and over slow remote sessions. Error messages and stack traces are dense and can be hard to parse aloud. The manager itself should own the accessible layer: short, plain-language messages, a quiet mode, and no colour-only signalling.

**Representation**

- Rating: Unknown
- Evidence: None yet
- Notes: Not assessed. Review governance and contributor diversity through the OpenJS Foundation and the Node.js TSC.

**Resilience**

- Rating: Moderate
- Evidence: Inferred
- Notes: Downloads are tens of megabytes per platform, a cost on low or intermittent bandwidth, but archives can be mirrored and cached locally, and the checksums allow resumable or mirrored fetches to be verified. Once installed, Node.js runs fully offline; `npm install` does not, unless an offline cache or mirror is set up. Older systems are the weak point, see Concerns.

### Agency

**Sovereignty & Privacy**, including the power-imbalance proxies, exit cost, data portability, terms-of-service volatility, pricing asymmetry

- Rating: Strong for the runtime, Moderate for the npm registry
- Evidence: Inferred
- Notes: The runtime is MIT licensed, foundation-governed, free of charge, and installable without any account. Exit cost is low, it is one directory to remove. The registry is the power imbalance: it is a single commercial service whose terms can change. Mitigation is a configurable registry URL and support for a local mirror in the manager.

**Interaction Patterns**, supportive versus dark patterns

- Rating: Strong
- Evidence: Inferred
- Notes: No sign-in prompts, upsells, or nagging in the runtime. npm prints an update notice when a newer npm is available, which is informative rather than manipulative and can be turned off.

### Sustainability

**Environment**, direct and indirect impacts

- Rating: Moderate
- Evidence: Inferred
- Notes: Direct footprint is small, no daemon and no background activity. Indirect impact comes from the ecosystem: large `node_modules` trees, repeated downloads, and build tooling. A shared, pinned npm cache inside the OSAT tree reduces repeat downloads.

### Security

- Rating: Moderate, pending verification
- Evidence: Inferred
- Notes: Signed checksums for every release, a published security release process, and security fixes across all supported lines. Supply-chain risk sits mainly in npm packages, not the runtime. The manager must verify `SHASUMS256.txt` and its signature before unpacking, and should refuse unsupported (end-of-life) lines by default.

**Assessment method:**
Not yet tested. Planned: uc-radar telemetry trace procedure (version to be named) on Linux Mint, macOS and Windows 11, covering `node --version`, a REPL session, `npm install` of a small project, and `npx` of a one-off tool.

**Network behaviour:**
- [ ] Outbound connections during normal use (expected: none for `node`; registry calls for `npm` and `npx`)
- [ ] Update checks, silent or explicit (expected: npm update notifier, explicit and disableable)
- [ ] Telemetry or usage data, present or absent (expected: absent)
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
- Notes: In wide use since 2009, governed by the OpenJS Foundation, with a published, predictable release schedule and roughly three years of support per line under the new model <a id="cite-nodejs-releasers-2026-b"></a>([Node.js Releasers, 2026](#ref-nodejs-releasers-2026)).

**Content endurance**

- Rating: Not applicable
- Evidence: Inferred
- Notes: Node.js stores no user content. Content endurance belongs to the tools that run on it, for example Eleventy, whose content stays in plain files.

**Exit and portability**

- Rating: Strong
- Evidence: Inferred
- Notes: A self-contained directory; removing it and the pinned prefix and cache is a complete uninstall. Projects are portable across platforms and across other JavaScript runtimes to a large degree.

**Adjustability and support**

- Rating: Strong
- Evidence: Inferred
- Notes: Open source with extensive documentation, a large community, and commercial extended support available for end-of-life lines if a client ever needs it.

**Market Position**, market concentration, take rate, API stability, forkability, contributor concentration

- Rating: Moderate
- Evidence: Inferred
- Notes: Dominant server-side JavaScript runtime, with credible alternatives (Deno, Bun). Forkable, and has been forked and re-merged before (io.js). Concentration risks are the V8 engine, controlled by Google, and the npm registry, controlled by one company. API stability across LTS lines is good, with removals announced through deprecation cycles.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | Plain-text CLI; dense errors |
| Multilingual integration | Weak | Inferred | Website partly translated; CLI, errors and docs English |
| Economic and cognitive accessibility | Moderate | Inferred | Free; ecosystem complexity is high |
| Representation | Unknown | None | Not assessed |
| Compatibility | Moderate | Inferred | Three OS families; rising minimum platforms |
| Resilience | Moderate | Inferred | Mirrorable; offline after install, npm needs registry |
| Agency, sovereignty & privacy | Strong / Moderate | Inferred | Runtime strong, registry moderate |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Moderate | Inferred | Low exit cost; registry ToS risk |
| Agency, interaction patterns | Strong | Inferred | No dark patterns |
| Environment, direct and indirect | Moderate | Inferred | No daemon; heavy dependency trees |
| Security | Moderate | Inferred | Signed checksums; npm supply chain |
| Longevity | Strong | Verified (schedule) | Annual LTS model from Node.js 27 |
| Content endurance | Not applicable | Inferred | Stores no user content |
| Exit and portability | Strong | Inferred | One directory |
| Adjustability and support | Strong | Inferred | Large community |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Inferred | V8 and npm concentration |
| Gates | Pending | | Not yet Verified; confirm gate list against metrics 0.6.0 |

## Relationship to project

Tier: dev toolchain. On adopt, the process graduates to uc-processes and the implementation lives in `osat-manager-node` (command `manage-node`, reachable through the `osat` umbrella command), built as a v1 to the final OSAT manager protocol with no code carried over from archived repos. A later `osat-manager-nodeapp`, parallel to `osat-manager-pyapp`, could manage npm-based CLI tools such as Eleventy.

Design decisions to record before trial:

- Source: official nodejs.org archives (leaning) or conda-forge `nodejs`.
- Version arguments: major line (`26`), exact version (`26.x.y`), and `lts`.
- Side-by-side versions, with one active per scope.
- npm prefix and cache pinned inside the OSAT tree.
- pnpm and Yarn out of scope for this manager.
- Registry URL configurable, with support for a local mirror.
- Refuse end-of-life lines and below-minimum platforms by default, with plain-language messages.

## Status notes

- Last reviewed: 2026-10-28
- In assess: drafted from documentation and the published release schedule only; no ratings are Verified by testing. Pending: the telemetry trace on Linux Mint, macOS and Windows 11, a per-line minimum platform matrix, the Representation review, the source decision, and confirmation of which items are gates under metrics 0.6.0. The automated pass recommends trial, conditional on the Security pillar and all gates being Verified or a named override being recorded. A suitable trial scope is the Vishpala Eleventy site on one machine, with an end condition of one full build and deploy cycle on each of the three OS families.

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

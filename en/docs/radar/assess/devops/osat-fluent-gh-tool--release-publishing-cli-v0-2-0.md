---
dc:title: "osat-fluent-gh-tool, a Fluent Release-Publishing CLI over gh"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Fable 5 (Anthropic)"
dc:subject:
  - "radar"
  - "devops"
  - "release-publishing"
  - "gh"
  - "tooling"
dc:description: "Radar entry and Universal Cake evaluation for a proposed osat-fluent tool wrapping the GitHub CLI (gh) for the fleet's release-publishing ceremony: Release creation, tarball and SHA256SUMS upload, and the deferred GPG-signing items, one answer across repositories instead of per-repo drift."
dc:publisher: "UniversalCake"
dcterms:created: "2026-08-02"
dcterms:modified: "2026-08-02"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "uc-radar-entry-template, universal-cake-evaluation-metrics, decision--gh-cli-for-release-asset-publishing, fairy--file-sync-cli"
dc:identifier: "osat-fluent-gh-tool--release-publishing-cli"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/devops/"
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-08-02"
    author: "Christopher Steel, Claude Fable 5 (Anthropic)"
    notes: "The fork is resolved: per decision--publish-release-shared-script-with-provider-interface, the capability proceeds as the shared script publish-release.py; the standalone-tool form is parked behind the four flip conditions. The script's first real publish (sat-doc-automa v0.1.4, 2026-08-02) carried both security gates to Verified. Status notes updated; the evaluation body is retained unchanged as the record of the standalone form."
  - version: "0.1.0"
    date: "2026-08-02"
    author: "Christopher Steel, Claude Fable 5 (Anthropic)"
    notes: "Initial entry. Captures the design as discussed, pre-implementation, and evaluates it against Universal Cake Evaluation Metrics v0.3.1. All ratings Inferred or Claimed, never Verified, per the fairy--file-sync-cli precedent for unbuilt tools."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Fable 5 (Anthropic)"
      evaluator_version: "claude-fable-5"
      recommendation: "assess"
      reasoning: >
        Pre-implementation design review. The need is real and already
        recorded twice (sat-doc-automa and osat-fluent-restic-tool GPG
        ROADMAP items both hang off publish scripts that do not exist),
        but a lighter alternative, a shared publish-release.py
        distributed by file-fairy rather than a standalone tool, has not
        been ruled out. That fork must be decided before trial. The gh
        dependency also imports GitHub-platform sovereignty concerns
        that the evaluation records rather than resolves.
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-08-02"
  human: []
---

# osat-fluent-gh-tool--release-publishing-cli

## Description

A raw radar entry for osat-fluent-gh-tool, a proposed member of the osat-fluent collection wrapping the GitHub CLI (`gh`) for the release-publishing ceremony the fleet's repositories share: creating the GitHub Release for a pushed tag, generating and uploading a byte-stable tarball and its `SHA256SUMS`, and, when a key is available, GPG-signing the checksum file.

## Notes

The tool does not exist as code yet. This entry captures the design as discussed and evaluates it on that basis. Ratings below are Inferred or Claimed, never Verified, until an implementation exists to test against, the same footing the `fairy--file-sync-cli` entry stood on before file-fairy was built.

The working name osat-fluent-gh-tool follows the collection's naming (`osat-fluent-restic-tool`); no registry collision check has been performed yet. The Ferry-to-Fairy rename recorded in the file-sync entry is the cautionary precedent: check PyPI and package registries before the name hardens.

Scope boundary, stated up front because it motivated this entry: this tool standardizes *ceremony*, not *access*. It wraps `gh`; it does not create, store, or manage credentials. Whether a human or an AI collaborator can push or publish is an authentication question (`gh auth login`, `GH_TOKEN`) that this tool inherits and must never try to solve itself, consistent with the decision record's rule that `gh` handles its own authentication and scripts never touch a token directly.

## What it is

A single-purpose CLI, planned as stdlib-plus-`gh` Python in the fluent style, that runs the release-publishing steps after `cut-release.py` has tagged and the maintainer has pushed: build the tarball deterministically, compute `SHA256SUMS`, create the GitHub Release via `gh release create`, upload the assets, and optionally sign the checksum file when `gpg` and a project key are present. Per repository it replaces a hand-run sequence of `gh` invocations with one command whose behaviour is identical across every repository that adopts it.

## Why interesting

Three standing records converge on this tool without any of them naming it. *Decision: GitHub CLI (gh) for Release Asset Publishing* chose `gh` over raw API calls and scoped it as a dev-only, release-side dependency, assuming per-repository `publish-release.py` scripts that have not been written. sat-doc-automa's ROADMAP holds a pending item to GPG-sign `SHA256SUMS` at publish time, and osat-fluent-restic-tool's ROADMAP holds the mirrored item to verify that signature at self-update time; the decision record explicitly warns the two "should be resolved together, or explicitly cross-referenced, rather than drifting into separate answers." A shared implementation is the strongest form of resolving together: the signing side and the verifying side are maintained against the same code, and every repository's release ceremony is one answer instead of N drifting copies.

The self-update pattern in the consuming tools is what makes the ceremony load-bearing rather than cosmetic: self-update verifies downloaded archives against `SHA256SUMS` from the Release, so the publish side must be byte-stable and complete every time, which is exactly what hand-run ceremony is bad at.

## Concerns

Honest assessment, since this has not been built:

- No implementation exists. Every behavioural claim below is design intention.
- The lighter alternative has not been ruled out: a shared `publish-release.py` distributed to each repository by file-fairy (the same road the devops scripts already travel) delivers most of the value without a new repository, a new VERSION, and a new maintenance surface. The fluent-tool form earns its keep only if the ceremony needs per-platform installation, self-update, or wrapper conventions the way restic did. This is the fork to decide before trial.
- `gh` is a vendor CLI for a proprietary, Microsoft-owned platform, and it requires `git` transitively (confirmed by the dnf install failure recorded in the decision record). The dependency is dev-only and release-side, but the sovereignty and market-position concerns of the platform itself do not disappear because the wrapper is open source; the evaluation records them below.
- Token and auth handling is a design gate: the tool must delegate authentication entirely to `gh` and must never read, store, log, or echo a token. A tool that touched credentials would fail the sovereignty posture this collection claims.
- Byte-stable tarball generation is harder than it looks (timestamps, file ordering, uid/gid); the restic tool's self-update verification will catch nondeterminism, but the determinism rules need specifying, not assuming.
- Single author, no users yet, no track record, the standing concern for every tool in this collection at birth.

## Evaluation

Applying *Universal Cake Evaluation Metrics* (v0.3.1) to the design as discussed. All ratings are pre-implementation and carry Inferred or Claimed tags, never Verified. Where the tool and its GitHub dependency pull a rating in different directions, both are recorded rather than averaged.

### Inclusive

**Accessibility**

- Rating: Moderate
- Evidence: Inferred
- Notes: Plain-text CLI output, naturally more compatible with assistive technology than a GUI, untested against a screen reader. Lens: user.

**Representation**

- Rating: Unknown
- Evidence:
- Notes: Single-author personal tooling; absence of a design or research process recorded as an absence, per the metrics document's instruction.

**Resilience**

- Rating: Weak
- Evidence: Inferred
- Notes: The honest divergence from file-fairy: publishing is online by definition. The tool is useless without GitHub reachable and an authenticated `gh`. Scoped narrowly, only the release moment needs the network; daily work is unaffected. Lens: owner.

### Agency

**Sovereignty & Privacy**

- Rating: Moderate
- Evidence: Inferred
- Notes: The wrapper itself sends nothing beyond what the operator explicitly publishes, keeps no telemetry, and touches no token. But it binds the release ceremony to GitHub's Release feature, which is a platform service, not a git primitive. Exit cost is moderate rather than low: repositories and tags are portable, published Releases and their download URLs (which self-update depends on) are not. Terms-of-service volatility and pricing asymmetry are GitHub's to change unilaterally. Recorded as the platform's concern inherited by the tool, per the power-imbalance proxies.

**Interaction Patterns**

- Rating: Strong
- Evidence: Claimed
- Notes: Planned in the fluent style: preflight checks (`gh` present, authenticated, tag pushed), show what will be published before publishing, fail specifically rather than generically. A claim about intended design, not observed use.

### Sustainability

**Environment**

- Rating: Strong
- Evidence: Inferred
- Notes: One short network operation per release, a rare event; no daemons, no polling, no new hardware. Consistent with the energy-conservation directive's posture.

### Security

- Rating: Unknown
- Evidence:
- Notes: No implementation to audit. The two named design gates, never touching tokens and deterministic archive generation, are open questions, not hypotheticals. Not to be rated above Unknown until a Verified pass exists; gate criteria must carry Verified tags before trial.

**Assessment method:**
Design review only. Revisit with a network monitor and filesystem trace once a working version exists; the expected profile is connections to github.com and uploads.github.com during publish, nothing at any other time.

**Network behaviour:**
- [x] Outbound connections during normal use, yes by design, publish-time only, to GitHub
- [ ] Update checks, silent or explicit, none planned for the tool itself
- [ ] Telemetry or usage data, none planned; `gh`'s own telemetry posture to be verified and documented
- [ ] Licence server contact, not applicable
- [x] Affects items transferred over a network, yes, the published artifacts; SHA256SUMS and optional GPG signature exist to keep them verifiable
- [x] Helps ensure documents passed via network are clean and compliant, the checksum-and-signature ceremony is the point

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content, none planned beyond the build directory
- [ ] Caches content outside the working directory, not planned
- [ ] Stores recent file history outside the archive, not planned
- [ ] Writes to unexpected locations, no; writes are the tarball, SHA256SUMS, and optional signature, in a declared build path

**Content exposure:**
- [x] Sends content to a remote service, yes, deliberately and only what the operator publishes
- [ ] Stores content in a cloud service by default, only the published Release assets
- [ ] Auto-save or backup features that copy content externally, none

### The Product or Service Itself

**Longevity**

- Rating: Weak
- Evidence: Claimed
- Notes: Single maintainer, unbuilt, no track record. The dependency cuts the other way: `gh` is actively maintained with official installers on all three platforms.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: The tool authors nothing and stores nothing; it publishes artifacts generated from a repository that remains the source of truth. Its disappearance leaves every repository, tag, and previously published Release intact.

**Exit and portability**

- Rating: Moderate
- Evidence: Inferred
- Notes: Gate: pass. Exit from the *tool* is trivial, run the `gh` commands by hand or fall back to raw API calls, the alternative the decision record already priced. Exit from the *platform* is the moderate part: Releases and asset URLs would need re-publishing elsewhere and self-update URLs would change. An exit exists, so the gate passes, but it is not free.

**Adjustability and support**

- Rating: Strong
- Evidence: Inferred
- Notes: A single readable Python script under AGPL, fork-and-fix open, same shape as the rest of the collection.

**Market Position**

- Rating: Weak
- Evidence: Inferred
- Notes: Not for the tool, which is a personal utility with no ecosystem, but for the dependency: GitHub is a concentrated, proprietary platform, `gh` is its vendor CLI, and forkability of the platform relationship is nil. Recorded so the adopt-ring decision is made with eyes open, not to block it; the decision record already weighed this and chose `gh` deliberately.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | Plain-text CLI, untested against assistive technology |
| Multilingual integration | Weak | Inferred | Hardcoded English strings, no localization path, same as the sibling tools |
| Economic and cognitive accessibility | Strong | Inferred | Free, one dependency (`gh`), one command replaces a memorized sequence |
| Representation | Unknown | | Single-author tool, absence recorded as such |
| Compatibility | Strong | Inferred | `gh` confirmed on macOS, Windows, Linux with official installers, per the decision record |
| Resilience | Weak | Inferred | Online by definition at publish time; scoped to the release moment only |
| Agency, sovereignty & privacy | Moderate | Inferred | Wrapper clean by design; platform binding is the inherited concern |
| Agency, power-imbalance proxies | Moderate | Inferred | Releases and asset URLs are platform features; repos and tags remain portable |
| Agency, interaction patterns | Strong | Claimed | Preflight, show-before-publish, specific failures; intended, not observed |
| Environment, direct and indirect | Strong | Inferred | One short network operation per release |
| Security | Unknown | | Unbuilt; token-handling and tarball-determinism gates named and open |
| Longevity | Weak | Claimed | Single maintainer, unbuilt; `gh` itself actively maintained |
| Content endurance | Strong | Inferred | Publishes artifacts; sources of truth unaffected by its disappearance |
| Exit and portability | Moderate | Inferred | Pass; tool exit trivial, platform exit real but priced |
| Adjustability and support | Strong | Inferred | Single readable AGPL script, fork-and-fix open |
| Market position | Weak | Inferred | The platform's concentration, recorded against the dependency, not the tool |
| Gates | Pass (tentative) | Inferred | No telemetry, compatible licence, accessibility floor met, exit exists; none Verified yet |

## Relationship to project

Dev toolchain tier, release-side only. It is the publishing half of the pattern whose consuming half (self-update verifying `SHA256SUMS` from a Release) already ships in osat-fluent-restic-tool. It complements, not overlaps, file-fairy: the fairy distributes files between local repositories; this tool publishes artifacts from a repository to its platform of record. If the shared-script fork is chosen instead, the script itself would be distributed by file-fairy, and this entry's evaluation transfers to it nearly unchanged.

## Status notes

- Last reviewed: 2026-08-02
- Resolution, 2026-08-02: the fork is decided. Per decision--publish-release-shared-script-with-provider-interface (sat-doc-automa, decisions/devops/), the capability proceeds as the shared script publish-release.py, distributed by file-fairy; this standalone-tool form is parked behind the four flip conditions recorded there. The script published its first real release, sat-doc-automa v0.1.4, on 2026-08-02: the token gate is Verified by construction and inspection, and the determinism gate is Verified by the 20-check suite and self-checked on every run. This entry stays in assess as the parked record of the standalone form; a fluent gh-binary acquirer, if the fourth flip condition ever triggers, is a new entry, not this one.
- In assess (as originally written): two things move it forward. First, the fork decision: standalone fluent tool versus a shared `publish-release.py` distributed by file-fairy; the entry deliberately does not pre-empt this, and the lighter form should win unless the ceremony proves to need installation, self-update, or per-platform wrapper conventions. Second, on either form, a working implementation exercised against a real release of at least one repository, with the token-handling and tarball-determinism gates carrying Verified tags. What would move it to hold: deciding hand-run `gh` commands documented in the commit-and-versioning workflow are sufficient ceremony for the fleet's release volume.
- In trial: not applicable yet.
- In adopt: a standalone tool repository (osat-fluent-gh-tool) or, on the script fork, a devops-scripts group entry in the sync manifests.
- In hold: nothing yet.

## Links

- No repository yet. Design captured across the decision record and this entry.
- `sat-doc-automa/en/docs/decisions/devops/decision--gh-cli-for-release-asset-publishing-v0-1-0.md`
- `sat-doc-automa/ROADMAP.md`, Pending: GPG-sign SHA256SUMS for published release assets
- `osat-fluent-restic-tool/ROADMAP.md`, Optional GPG verification
- `uc-radar/en/docs/radar/assess/sync/fairy--file-sync-cli.md`, the precedent entry for evaluating an unbuilt tool

## License

This document, *osat-fluent-gh-tool, a Fluent Release-Publishing CLI over gh*, by **Christopher Steel**, with AI assistance from **Claude Fable 5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Fork resolved: capability proceeds as the shared script per the publish-release decision record; standalone form parked behind the flip conditions. First real publish (sat-doc-automa v0.1.4) carried both security gates to Verified. |
| 0.1.0 | Draft | Initial entry, pre-implementation. UC evaluation against metrics v0.3.1, all ratings Inferred or Claimed. Records the tool-versus-shared-script fork as the decision gating trial. |

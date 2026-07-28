---
dc:title: "Fairy, a Config-Driven File Sync CLI"
dcterms:version: "0.1.1"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "tooling"
  - "file-sync"
  - "automation"
  - "git-hooks"
dc:description: "Standalone CLI, config-driven by named file groups and local targets, for copying selected files from a source project to one or more other local projects, with plan/apply/status verbs and drift tracking."
dc:publisher: ""
dc:date: "2026-07-28"
dc:modified: "2026-07-28"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en"
dc:source: ""
dc:relation: "sat-radar-entry-template"
dc:identifier: "fairy--file-sync-cli"
dc:rights: >
  Copyright 2026 Christopher Steel.
  SPDX-License-Identifier: AGPL-3.0-or-later
---

# fairy--file-sync-cli

## Description

A raw radar entry for Fairy, a proposed standalone command-line tool for copying named groups of files from a source project to one or more target projects on local disk, driven by a YAML manifest.

## Notes

Fairy does not exist as code yet. This entry captures the design as discussed and evaluates it on that basis. Ratings below are Inferred or Claimed, never Verified, until an implementation exists to test against.

Renamed from the working name Ferry to Fairy on 2026-07-28, after finding that `ferry` is an existing, actively distributed PyPI package (`jhorey/ferry`), not just a similarly-named repository. The rename is cosmetic, nothing in the design, the concerns, or the evaluation below changed as a result of it.

## What it is

Fairy is a config-driven CLI that copies selected files or globs, grouped under named labels in a YAML manifest, from one local project into one or more other local target projects. It is not tied to git hosting, GitHub Actions, or any particular source project, the manifest names groups and targets, and the tool computes a plan, applies it, or reports drift between what a target holds and what the source currently has.

## Why interesting

Several projects sharing standing conventions, house style rules, license blocks, CI config, need a way to keep those shared files current without hand-copying them or re-deriving the rules each time a target project is set up. Existing tools in this space (`repo-file-sync-action` and its forks, `kbrashears5/github-action-file-sync`, `netlify/file-sync-action`) all assume git hosting and either a scheduled poll or a push-triggered CI job that opens a pull request. Fairy is useful in the narrower case where the source and targets are local paths on the same machine and a pull request round-trip is unnecessary overhead, a plan-then-apply model run from the command line, optionally from a git hook, is a closer fit and a lighter one.

## Concerns

Honest assessment, since this has not been built:

- No implementation exists. Every claim below about behaviour is a design intention, not a tested fact.
- Path handling is unresolved. A target's `dest_map` remapping could in principle write outside the intended target root if a config is malformed or malicious; the design as discussed has not specified a guard against this.
- Drift detection (source changed vs. target changed locally) has been designed conceptually but the actual hashing and state-file format have not been specified.
- If installed via a `post-commit` hook, the hook itself lives in `.git/hooks/` and does not travel with the repository unless a hook manager or `core.hooksPath` is used, so its benefit is inconsistent across collaborators unless that extra step is taken.
- Single author, no users yet, no track record.

## Security assessment

Applies to tools and apps that run code or move data.

**Network behaviour:**
- [ ] Outbound connections during normal use, none by design, unimplemented
- [ ] Update checks, silent or explicit, none planned
- [ ] Telemetry or usage data, present or absent, none planned
- [ ] Licence server contact, frequency and data sent, not applicable
- [ ] Does have any affect on networking or items created that will be transferred over a network, not applicable, local filesystem only
- [ ] Helps ensure that documents passed via Network are clean and compliant, not applicable

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content, a per-target state file for drift tracking is part of the design
- [ ] Caches content outside the working directory, not planned
- [ ] Stores recent file history outside the archive, not planned
- [ ] Writes to unexpected locations, unresolved risk, see Concerns, no path-containment guard has been designed yet

**Content exposure:**
- [ ] Sends any content to a remote service, no
- [ ] Stores content in a cloud service by default, no
- [ ] Auto-save or backup features that copy content externally, no

**Assessment method:**
Design review only. No code exists to inspect, run, or monitor. Revisit with an actual network monitor and a filesystem trace once a working version exists.

**Assessment status:** Unassessed, pre-implementation, design intent only.

## Relationship to project (SAT as an example)

Dev toolchain tier. Not itself a documentation format or archive component, it is a utility that could be used to keep an automa's `defaults/` files current across projects that adopt sat-doc-automa's conventions, alongside other independent uses.

## Status notes

- Last reviewed: 2026-07-28
- In assess: what would move it to adopt is a working implementation exercised against at least two real target projects, a specified and tested drift-state format, and a resolved answer to the path-containment concern above. What would move it to hold is deciding an existing tool (for example a `copier`/`cruft`-style template updater, or one of the GitHub-Action-based syncers) already covers the need well enough that a new tool is not warranted.
- In adopt: not applicable yet, this is a standalone tool repository, not content that migrates into `en/docs/`.
- In hold: nothing yet, no reason has arisen to hold this.

## Links

- No repository yet. Design captured across this conversation.

## Universal Cake evaluation

Applying *Universal Cake Evaluation Metrics* (v0.3.1) to the design as discussed. All ratings are pre-implementation and carry the Inferred or Claimed evidence tag, never Verified, per the metrics document's own gate for what qualifies as Verified.

**Inclusive.** Accessibility (alternative interaction), Moderate, Inferred, plain-text CLI output is naturally more compatible with assistive technology than an equivalent GUI, but this has not been tested against a screen reader. Multilingual integration, Weak, Inferred, all planned output strings are hardcoded English with no localization path designed. Economic and cognitive accessibility, Strong, Inferred, the tool is planned as free, dependency-light (stdlib plus PyYAML), and runs entirely offline on modest hardware. Representation, Unknown, recorded as an absence, single-author personal tool with no design or research process to assess. Compatibility, Moderate, Inferred, a pure-Python script is broadly portable, but the optional git-hook integration is POSIX-shell-shaped by default and would need explicit handling for Windows environments. Resilience, Strong, Inferred, no live services, licence servers, or expiring tokens are part of the design, it works fully offline by construction.

**Agency.** Sovereignty and privacy, Strong, Inferred, no phone-home, no telemetry, all data stays on the machines involved. The vendor-user power-imbalance proxies do not really apply here, there is no vendor, the person running Fairy and the person configuring it are the same person, so exit cost is near zero (delete the script, the hook, and the state file) and there is no pricing, terms of service, or portability asymmetry to score. Interaction patterns, Strong, Claimed, the plan-then-apply model is designed specifically around honest defaults and plain asking, showing what would change before writing anything, but this is a claim about intended design, not an observed pattern from real use.

**Sustainability.** Environment, Strong, Inferred, a single local diff-and-copy operation triggered only when relevant files change, no new hardware, no network bandwidth at all in the local-path design.

**Security.** Unknown, Moderate concern flagged, no implementation exists to audit, and the one specific design gap already identified, unresolved path-containment on `dest_map` remapping, is a real open question rather than a hypothetical one. This section should not be rated above Unknown until a Verified pass exists, per the metrics document's own requirement that Trial-ring gate criteria carry Verified tags.

**The product or service itself.** Longevity, Weak, Claimed, single maintainer, no institutional backing, does not yet exist. Content endurance, Strong, Inferred, Fairy does not author or store content itself, it copies files that live independently in their own repositories, so its disappearance leaves both source and target content intact. Exit and portability, Strong, Inferred, Pass, nothing is locked in, stopping use of the tool costs nothing beyond deleting a script and a state file. Adjustability and support, Strong, Inferred, a single readable script under an open licence with the fork-and-fix path open, once actually published. Market position, not applicable, this is a personal utility, not a platform with an ecosystem.

### Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | Plain-text CLI, not tested against assistive technology |
| Multilingual integration | Weak | Inferred | Hardcoded English strings, no localization path |
| Economic and cognitive accessibility | Strong | Inferred | Free, minimal dependencies, works offline |
| Representation | Unknown | | Single-author tool, absence of data recorded as such |
| Compatibility | Moderate | Inferred | Pure Python is portable; hook integration needs Windows handling |
| Resilience | Strong | Inferred | No live services or tokens, fully offline by design |
| Agency, sovereignty & privacy | Strong | Inferred | No telemetry, no phone-home, data stays local |
| Agency, power-imbalance proxies | Not applicable | | No vendor-user relationship exists |
| Agency, interaction patterns | Strong | Claimed | Plan-then-apply designed around honest defaults, unverified in use |
| Environment, direct and indirect | Strong | Inferred | Single local operation, no new hardware, no network use |
| Security | Unknown | | No implementation to audit; path-containment gap identified and unresolved |
| Longevity | Weak | Claimed | Single maintainer, no track record, unbuilt |
| Content endurance | Strong | Inferred | Copies content that exists independently; tool's disappearance leaves it whole |
| Exit and portability | Strong | Inferred | Pass, trivial exit, nothing proprietary retains data |
| Adjustability and support | Strong | Inferred | Single readable script, open licence intended |
| Market position | Not applicable | | Personal utility, no ecosystem |
| Gates | Pass (tentative) | Inferred | All four default gates pass on design intent; none Verified yet |

## License (for this document)

This document, *Fairy, a Config-Driven File Sync CLI*, by **Christopher Steel**, with AI assistance from **Claude Sonnet 4.6 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

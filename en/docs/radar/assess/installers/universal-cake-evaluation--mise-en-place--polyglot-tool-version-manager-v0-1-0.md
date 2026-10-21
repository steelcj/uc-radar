---
dc:title: "Universal Cake Evaluation: mise-en-place, Polyglot Tool Version Manager"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "mise"
  - "version manager"
  - "installers"
  - "conda-forge"
  - "osat-manager"
dc:description: "Universal Cake evaluation of mise-en-place (mise) 2026.10.17, a single-binary manager for developer tools, environment variables and tasks, read as prior art for OSAT managers and as a candidate for developer workstations."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-21"
dcterms:modified: "2026-10-21"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/jdx/mise"
dc:relation: "universal-cake-evaluation-metrics, git-on-windows--options-for-osat-manager-git, tool-installation-pattern--versioned-local-binaries-and-wrappers-for-nix, osat-manager-git"
dc:identifier: "universal-cake-evaluation--mise-en-place--polyglot-tool-version-manager"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/installers/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-21"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of mise 2026.10.17 against
      Universal Cake Evaluation Metrics 0.5.1, from the repository,
      its security policy, its conda backend source and the release
      assets. mise was not installed or run.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # prior art for OSAT managers now; a workstation trial is reasonable with the conditions in the entry
      reasoning: "MIT licence, a large and active project, one static binary per platform for Linux, macOS and Windows, release checksums signed three ways (GPG, minisign, Sigstore packslip), and built-in verification of tool downloads with a lock file. Its conda backend installs conda-forge packages without conda, solving, checking SHA-256, relocating prefixes and re-signing on macOS through the rattler library, which is exactly what osat-manager-git needs on Linux and macOS. Against that: one person holds write access to the core, the registry still routes some tools through asdf plugins, which its own policy calls dangerous, and its design, one general tool for every tool, is the opposite of OSAT's one governed manager per tool. Not a dependency for OSAT managers, which are standard-library Python."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-21T14:00:00-04:00"
  human: []
---

# Universal Cake Evaluation: mise-en-place, Polyglot Tool Version Manager

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

mise-en-place, usually called mise, is a single command-line program that installs and switches versions of developer tools (Node.js, Python, git, ripgrep and hundreds more), sets environment variables per directory, and runs project tasks. This entry evaluates it for two uses: as prior art for OSAT managers, especially the conda-forge route proposed for osat-manager-git, and as a tool a developer might install on their own workstation. It passes all four default gates. Its download verification and lock file are stronger than most tools in its class. Its risks are a single maintainer with write access to the core and a registry that still leans on asdf plugins for some tools. We recommend keeping it in Assess, studying its conda backend while osat-manager-git is designed, and allowing a workstation trial under the conditions below.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts come from the mise repository's README and `SECURITY.md` <a name="apa-mise-security-citation"></a>([mise Project, n.d.-b](#apa-mise-security-reference)), its conda backend documentation <a name="apa-mise-conda-docs-citation"></a>([mise Project, n.d.-a](#apa-mise-conda-docs-reference)) and source file `src/backend/conda.rs` <a name="apa-mise-conda-src-citation"></a>([mise Project, n.d.-c](#apa-mise-conda-src-reference)), read on 2026-10-21, and from the GitHub API's description of the repository and of release v2026.10.17 <a name="apa-mise-release-citation"></a>([mise Project, 2026](#apa-mise-release-reference)). The date the conda backend left experimental status comes from a third-party mirror of the release notes and is tagged Claimed. mise was not installed or run. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate mise v2026.10.17, released on 2026-10-21, as a developer tool on a person's own machine and as a design reference for OSAT managers. Its task runner and environment features are described but not rated in depth. Tools that mise installs are out of scope; each needs its own entry.

## Scorecard metadata

- Product or service name: mise-en-place (mise)
- Version evaluated: v2026.10.17; repository `jdx/mise` as of 2026-10-21
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-21
- Radar ring: Assess
- Test platform: none; documents, source and release metadata only

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The security policy names one regular contact, an occasional check of `mise.jdx.dev/VERSION` for a new release. No telemetry is described. Not traced |
| A licence incompatible with the project | Pass | Verified | MIT, per the GitHub licence detection and the repository |
| Failure of the accessibility floor | Pass | Inferred | A command-line tool with plain-text output and plain-text configuration. Not tested with a screen reader |
| No exit | Pass | Inferred | Configuration is plain TOML (`mise.toml`) or `.tool-versions`; tools are installed into ordinary directories. Leaving means deleting mise's directories and its shell activation line |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Command line only, with long-form commands and a documentation site. Progress output and colour can usually be turned off; not tested with assistive technology |
| Multilingual integration | Weak | Inferred | Messages and documentation are English only, as far as the repository shows |
| Economic and cognitive accessibility | Moderate | Inferred | Free. One binary. But it is three tools in one (versions, environment, tasks), with many backends and settings to understand |
| Representation | Unknown | None | Not assessable from public information |
| Compatibility | Strong | Verified | Release assets for Linux x64, arm64 and armv7 (glibc and musl), macOS x64 and arm64, and Windows x64 and arm64. Reads `.tool-versions` from asdf and version files from other managers |
| Resilience | Moderate | Inferred | Installed tools work offline. Installing new versions needs the network, and the registry depends on GitHub and each tool's upstream |
| Agency, sovereignty and privacy | Strong | Inferred | Local tool; no account. The only contact the policy names is the version check |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms, pricing or account. Configuration is portable text |
| Agency, interaction patterns | Moderate | Inferred | Shell activation changes `PATH` on every prompt, which is powerful and can surprise. `mise use` writes configuration files into the current project |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | A native binary with low overhead. Shims and activation add little at runtime |
| Security | Moderate | Verified, Inferred | Strengths, verified from the release and policy: `SHASUMS256.txt` and `SHASUMS512.txt` signed with GPG and minisign, a Sigstore `packslip.sigstore.json`, a signed install script, and built-in verification of tool downloads (checksums, Cosign, minisign, SLSA, GitHub attestations) with a lock file recording checksums. Weaknesses, from the policy: a single developer holds write access to the core, and asdf plugins remain in the registry for some tools, which the policy itself calls dangerous. Known advisories were not checked |
| Longevity | Strong | Verified | Repository created 2023-01-09, 35,241 stars and 1,491 forks on 2026-10-21, and frequent releases under calendar versioning, v2026.10.17 being patch 17 of October 2026 |
| Content endurance | Strong | Inferred | It stores configuration only, in plain text |
| Exit and portability | Strong | Inferred | Tools can be reinstalled with any other manager; `.tool-versions` is shared with asdf |
| Adjustability and support | Strong | Inferred | MIT, issues tracked publicly, behaviour set in plain configuration. Support is community and one maintainer |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | Widely used among developer version managers. Contributor concentration is high at the top: one person has core write access. Calendar versioning, so stability is judged per change rather than by major version |
| Gates | Pass | Mixed | All four pass; licence Verified, the others Inferred |

## Findings

### The conda backend, and why it matters for OSAT

mise can install packages from conda-forge with no conda, mamba or micromamba on the machine <a name="apa-mise-conda-docs-citation-2"></a>([mise Project, n.d.-a](#apa-mise-conda-docs-reference)). It picks the host's subdir (`linux-64`, `linux-aarch64`, `osx-64`, `osx-arm64` or `win-64`), solves the package and its dependencies from a single channel into one isolated prefix per tool, and puts only the requested package's commands on `PATH`. The source shows it uses rattler, prefix.dev's Rust conda library, for solving, extraction and linking. Linking rewrites text and binary prefix placeholders and re-signs on macOS. Each download is checked against the SHA-256 from repodata, and a locked install takes every package URL and checksum from `mise.lock` rather than solving again <a name="apa-mise-conda-src-citation-2"></a>([mise Project, n.d.-c](#apa-mise-conda-src-reference)).

That list is the work osat-manager-git has to do on Linux and macOS, so mise is the closest working reference. It shows that the approach works in practice, and in which order the steps go: lock, download, verify, extract, relocate, re-sign. It also shows the hard parts: packages from more than one channel are not supported, and the host still supplies native requirements such as glibc. The backend left experimental status in release 2026.5.0 (claimed, from a mirror of the release notes).

mise is not a candidate dependency for OSAT managers, which are standard-library Python by contract. What transfers is the design, and the use of a lock as the trust anchor when conda-forge publishes hashes but no signatures.

### Verification

Release v2026.10.17 carries `SHASUMS256.txt` and `SHASUMS512.txt`, each with a GPG signature (`.asc`) and a minisign signature (`.minisig`), a Sigstore bundle `packslip.sigstore.json`, and signatures for `install.sh` and `install.ps1` (verified from the asset list). The policy says mise verifies tool downloads itself, without external `cosign`, `gpg` or `minisign` programs, and that a failed check stops the install. This was read, not tested.

### Maintainer concentration

The policy states plainly that only the author has write access to the core repository. That limits who could leak keys, and it creates a bus factor of one, which the policy acknowledges and answers with named successors on the author's GitHub account. For a workstation tool this is acceptable with the lock file in use. For anything a client depends on, it is a reason to prefer OSAT managers.

### asdf plugins

The registry prefers packslip, aqua and GitHub release backends, and keeps the remaining asdf plugins in an organization the author controls. asdf plugins run arbitrary shell code from third parties at install time, and `mise.lock` does not cover them. Anyone trialling mise should avoid tools that resolve to an asdf plugin; `mise tool <name>` shows which backend and checks apply.

## Recommendation

Keep mise in Assess.

- **For OSAT.** Use its conda backend as the reference design for osat-manager-git on Linux and macOS, and cite it in that manager's decision record. Do not depend on it.
- **For a developer's own workstation**, a trial is reasonable: commit `mise.lock`, avoid tools whose backend is asdf, review `mise.toml` changes like code, and keep OSAT managers for anything clients or other managers rely on.

Re-review triggers: a second person with core write access, the asdf backend removed from the registry, a change to how the conda backend verifies packages, or conda-forge publishing package signatures.

## Relationship to project

Dev toolchain. mise covers the same ground as the *versioned local binaries and wrappers* installer pattern in `adopt/installers/` and as OSAT managers, but for every tool at once. Its conda backend informs the osat-manager-git project plan in uc-automa (`en-ca/projects/osat-manager-git/`).

## Status notes

- Last reviewed: 2026-10-21
- In assess: an automated pass from documents and release metadata only. A trial would install mise on a test container, trace its network contact, and install one conda-forge tool and one aqua tool from a lock file.

## License

This document, *Universal Cake Evaluation: mise-en-place, Polyglot Tool Version Manager*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### mise

- [Conda backend documentation](#apa-mise-conda-docs-reference)
- [Security policy](#apa-mise-security-reference)
- [Conda backend source](#apa-mise-conda-src-reference)
- [Release v2026.10.17](#apa-mise-release-reference)

## References

<a name="apa-mise-conda-docs-reference"></a>mise Project. (n.d.-a). *Conda backend*. mise-en-place documentation. Retrieved October 21, 2026, from https://mise.jdx.dev/dev-tools/backends/conda.html
[Return to citation](#apa-mise-conda-docs-citation)

<a name="apa-mise-security-reference"></a>mise Project. (n.d.-b). *SECURITY.md*. GitHub. Retrieved October 21, 2026, from https://github.com/jdx/mise/blob/main/SECURITY.md
[Return to citation](#apa-mise-security-citation)

<a name="apa-mise-conda-src-reference"></a>mise Project. (n.d.-c). *conda.rs* [Source code]. GitHub. Retrieved October 21, 2026, from https://github.com/jdx/mise/blob/main/src/backend/conda.rs
[Return to citation](#apa-mise-conda-src-citation)

<a name="apa-mise-release-reference"></a>mise Project. (2026). *mise v2026.10.17* [Software release]. GitHub. https://github.com/jdx/mise/releases/tag/v2026.10.17
[Return to citation](#apa-mise-release-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of mise 2026.10.17 from documents, source and release metadata. Automated recommendation: Assess; reference design for osat-manager-git's conda route; workstation trial under conditions |

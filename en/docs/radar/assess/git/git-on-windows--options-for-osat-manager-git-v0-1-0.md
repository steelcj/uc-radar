---
dc:title: "Git on Windows, Options for osat-manager-git"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "radar"
  - "git"
  - "windows"
  - "git for windows"
  - "mingit"
  - "osat-manager-git"
dc:description: "Comparative evaluation of the ways to obtain Git on Windows, the git-scm.com installer, PortableGit, winget, MinGit and source builds, as the Windows source for a proposed osat-manager-git and for client machines. Recommends MinGit from Git for Windows releases for the manager, with named conditions and flip conditions."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-11"
dcterms:modified: "2026-10-11"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://git-scm.com/install/windows"
dc:relation: "universal-cake-evaluation-metrics, universal-cake-evaluation--git-cola--git-graphical-client, osat-manager-python"
dc:identifier: "git-on-windows--options-for-osat-manager-git"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/version-control/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-11"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial comparison of the Windows Git options against Universal
      Cake Evaluation Metrics 0.5.1. MinGit 2.56.0.2 downloaded, its
      checksum matched against the release notes, and its contents
      inspected on Linux. No Windows binary was run.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # MinGit as osat-manager-git's Windows source; other options ringed individually in the entry
      reasoning: "MinGit is Git for Windows' own build for applications that bundle Git: a plain zip, published for x64, ARM64 and 32-bit, with a SHA-256 for every asset in the release notes and a GPG-signed release tag. The downloaded 2.56.0.2 zip matched its published hash. It fits the osat-manager pattern without an installer, and it removes dugite-native, a repackaging of MinGit for GitHub Desktop, from the Windows dependency chain. Conditions: its system configuration enables autocrlf, requires a Git LFS filter that MinGit does not ship, and includes the configuration of any full Git for Windows install on the same machine, so the manager must decide each of these. Checksums are a markdown table in release notes rather than a SHA256SUMS file. Older Windows can only run frozen releases with no further security fixes."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-11T17:15:00-04:00"
  human: []
---

# Git on Windows, Options for osat-manager-git

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This entry compares the ways to obtain Git on Windows, for two uses: as the Windows source for a proposed osat-manager-git, which would install Git into user space beside osat-manager-python, and as Git for client machines, where git-cola and other tools expect it. It recommends MinGit, taken directly from Git for Windows releases, as the manager's source, and the standard installer for client machines that a person will use directly. The git-scm.com download page does not list MinGit, so the recommendation rests on the release itself, whose checksums and signed tag make it verifiable. Three defaults in MinGit's system configuration need a deliberate decision before the manager ships.

## Sources and Acknowledgements

The comparison applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). The options come from the git-scm.com Windows download page <a name="apa-gitscm-windows-citation"></a>([Git Project, n.d.](#apa-gitscm-windows-reference)) and the Git for Windows 2.56.0.2 release behind it <a name="apa-gfw-release-citation"></a>([Git for Windows Project, 2026](#apa-gfw-release-reference)). Support periods come from the Git for Windows FAQ and its 32-bit support page. `MinGit-2.56.0.2-64-bit.zip` was downloaded, hashed, and unpacked on Linux to inspect its layout and configuration; no Windows binary was run. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

The subject is Git for Windows 2.56.0.2, released on 2026-10-05, in each of the forms it is offered. Git's own behaviour, and Git on Linux and macOS, are out of scope; for those platforms, dugite-native remains the candidate, pending its own entry. Network behaviour and accessibility of the Windows builds were not tested, since nothing was run on Windows.

## The options

| Option | Artifact | What it is | Use |
|---|---|---|---|
| MinGit | `MinGit-2.56.0.2-64-bit.zip`, also ARM64 and 32-bit | Git for Windows' minimal build for applications that bundle Git: `git.exe` and its helpers, Git Credential Manager, no Bash, no GUI tools. 39.8 MB zip, 93 MB unpacked (verified) | osat-manager-git |
| MinGit, busybox variant | `MinGit-2.56.0.2-busybox-64-bit.zip`, also ARM64 and 32-bit | MinGit with BusyBox in place of the standard Unix tools; smaller, less compatible | None |
| PortableGit | `PortableGit-2.56.0.2-64-bit.7z.exe`, also ARM64 | Full Git for Windows, including Git Bash, unpacked into any folder with no registry or PATH changes; a self-extracting 7-Zip archive that 7-Zip can open without running it | Optional full variant for the manager |
| Full archive | `Git-2.56.0.2-64-bit.tar.bz2`, also ARM64 | The full Git as a plain archive | Alternative to PortableGit |
| Standalone installer | `Git-2.56.0.2-64-bit.exe`, also ARM64 | Full Git for Windows with Git Bash, Git GUI, gitk, Explorer menus, and PATH changes | Client machines, git-cola |
| winget | `winget install --id Git.Git -e --source winget` | The standalone installer, through Microsoft's package manager | Client setup where winget is present |
| Build from source | Git for Windows repository | Compile it yourself | None |

The git-scm.com page lists only the installer, PortableGit, winget, and source. MinGit, its busybox variants, and the full archives appear only on the release.

## The decisive observation

The osat-manager pattern needs an artifact it can download, verify against a published checksum, unpack into a versioned directory, and point an alias at, with no installer and no system changes. MinGit is exactly that, and it is published by Git for Windows itself for this purpose. The installer and winget fail the pattern by design, since they decide where things go. PortableGit fits, but carries Git Bash and much more than a manager needs, so it suits an optional "full" variant rather than the default.

dugite-native, the other candidate surveyed earlier, builds its Windows packages by repackaging MinGit. Taking MinGit at its source removes GitHub Desktop's packaging from the Windows dependency chain without losing anything.

## Gates for Git for Windows

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | Git has no telemetry. Git Credential Manager contacts a hosting service only when credentials are requested for it. Not traced on Windows |
| A licence incompatible with the project | Pass, for use | Verified | Git is GPL-2.0-only; the bundled `LICENSE.txt` states that only version 2 applies. Using MinGit raises no issue, and redistributing it carries the GPL's usual source obligations; Git's code cannot be combined into AGPL-3.0-or-later or GPL-3.0-or-later works |
| Failure of the accessibility floor | Pass | Inferred | MinGit is a command-line tool with no interface of its own. The installer's accessibility was not tested |
| No exit | Pass | Verified | Repositories are plain Git. Removing MinGit means deleting its directory |

## Verification

The release notes carry a SHA-256 table covering every asset, the release tag is GPG-signed by the Git for Windows maintainer, and the executables carry Authenticode signatures (claimed by the release page). The downloaded MinGit zip hashed to `da35e72aa21c005a5a0d298cfbae110bc1609a815730ea0dde84b01a1b3cd3be`, matching the release notes (verified). The git-scm.com page itself publishes no checksums.

The table is markdown inside the release notes, not a SHA256SUMS file, so the manager must parse it, unlike osat-manager-python, which reads python-build-standalone's SHA256SUMS. GitHub's release API may report a per-asset digest, which would be simpler; that could not be checked here and should be confirmed when the manager is built.

## Older systems

Git for Windows supports Windows 8.1 and later, on x64 and ARM64 <a name="apa-gfw-faq-citation"></a>([Git for Windows Project, n.d.-a](#apa-gfw-faq-reference)). Older systems can only run frozen releases.

| System | Last Git for Windows release |
|---|---|
| Windows 7 and 8 | 2.46.2 |
| Windows Vista and Server 2008 | 2.37.1 |
| 32-bit, installer and PortableGit | 2.48.1 |
| 32-bit, MinGit only | Supported until April 2029 |

The 32-bit dates come from the project's support page <a name="apa-gfw-32bit-citation"></a>([Git for Windows Project, n.d.-b](#apa-gfw-32bit-reference)). A manager could pin 2.46.2 for a Windows 7 client, as the psutil entry suggests pinning 7.2.x for older Python, but that machine gets no further Git security fixes. Client-facing guidance should say so plainly.

## Recommendation

| Option | Ring | For |
|---|---|---|
| MinGit, from Git for Windows releases | Trial | osat-manager-git's Windows source |
| PortableGit | Assess | An optional full variant of the manager, for users who want Git Bash |
| Standalone installer, or winget | Assess | Client machines a person uses directly, and git-cola, whose README asks for it |
| MinGit busybox variants | Hold | Less compatible, no offsetting benefit |
| Building from source | Hold | The official builds already meet the need |
| Frozen releases for Windows 7, 8, or Vista | Hold | Only as a declared bridge, with the security cost stated |

MinGit moves to Adopt when osat-manager-git installs, switches, and removes it end to end on real Windows hardware, with these decisions made and recorded in its proposal:

- **Line endings.** MinGit's system configuration sets `core.autocrlf = true`. For SAT content repositories shared between Linux and Windows, decide whether the manager keeps this default or sets `autocrlf` per repository, preferably with a `.gitattributes` policy in the repositories themselves.
- **Git LFS.** The same configuration declares the `lfs` filter with `required = true`, but MinGit ships no `git-lfs` executable (verified). A checkout of any repository that uses LFS will fail. The manager should either install Git LFS beside MinGit or tell the user plainly.
- **Inherited configuration.** MinGit's system configuration includes `C:/Program Files/Git/etc/gitconfig` and its 32-bit counterpart, so on a machine that also has a full Git for Windows install, MinGit inherits that install's settings. The manager should document this or override it.
- **Paths.** The 2.56 layout places Git's runtime under `ucrt64`, after a migration from `mingw64`. Aliases should target `cmd\git.exe`, the stable entry point, rather than paths inside the runtime directory.

## Flip conditions

- If Git for Windows stops publishing 64-bit MinGit, the manager falls back to the full `tar.bz2` archive, which needs no installer either.
- If users mostly want Git Bash, PortableGit becomes the default and MinGit the minimal option.
- If checksum parsing from release notes proves fragile, and no API digest is available, verify against the signed tag by building, or pin by hash per version as osat-manager-python's archive already allows.

## Relationship to project

This entry gives osat-manager-git its Windows half. On Linux and macOS the candidate remains dugite-native, whose builds need relocation variables in the alias wrapper, link the system's libcurl for HTTPS, and require glibc 2.34; that candidate needs its own entry. git-cola, evaluated separately, needs Git on the path and would be a natural consumer of the manager.

## Status notes

- Last reviewed: 2026-10-11
- In assess: the automated pass recommends MinGit for Trial as the manager's Windows source. What moves it to Adopt is the manager working end to end on real Windows, with the four decisions above recorded in its proposal.
- Re-review triggers: a change in how Git for Windows publishes checksums, any change to MinGit's default system configuration, and the end of 32-bit MinGit in April 2029.

## License

This document, *Git on Windows, Options for osat-manager-git*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### Git for Windows

- [Install Git on Windows, git-scm.com](#apa-gitscm-windows-reference)
- [Release 2.56.0.windows.2](#apa-gfw-release-reference)
- [FAQ, supported Windows versions](#apa-gfw-faq-reference)
- [32-bit support](#apa-gfw-32bit-reference)

## References

<a name="apa-gfw-faq-reference"></a>Git for Windows Project. (n.d.-a). *Frequently asked questions*. https://gitforwindows.org/faq
[Return to citation](#apa-gfw-faq-citation)

<a name="apa-gfw-32bit-reference"></a>Git for Windows Project. (n.d.-b). *32-bit support of Git for Windows*. https://gitforwindows.org/32-bit
[Return to citation](#apa-gfw-32bit-citation)

<a name="apa-gfw-release-reference"></a>Git for Windows Project. (2026). *Git for Windows 2.56.0(2)* [Software release]. GitHub. https://github.com/git-for-windows/git/releases/tag/v2.56.0.windows.2
[Return to citation](#apa-gfw-release-citation)

<a name="apa-gitscm-windows-reference"></a>Git Project. (n.d.). *Install Git on Windows*. https://git-scm.com/install/windows
[Return to citation](#apa-gitscm-windows-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial comparison of the Windows Git options. Automated recommendation: MinGit from Git for Windows releases for Trial as osat-manager-git's Windows source, with four conditions for Adopt and three flip conditions |

---
dc:title: "Git on Windows, Options for osat-manager-git"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "radar"
  - "git"
  - "windows"
  - "git for windows"
  - "mingit"
  - "conda-forge"
  - "osat-manager-git"
dc:description: "Comparative evaluation of the ways to obtain Git on Windows, conda-forge's git package, MinGit, PortableGit, the git-scm.com installer, winget and source builds, as the Windows source for osat-manager-git and for client machines. Records the 2026-10-21 decision to use conda-forge's git on Windows, Linux and macOS, with MinGit and dugite-native set aside, and the conditions for Adopt."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-11"
dcterms:modified: "2026-10-21"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/conda-forge/git-feedstock"
dc:relation: "universal-cake-evaluation-metrics, universal-cake-evaluation--git-cola--git-graphical-client, universal-cake-evaluation--mise-en-place--polyglot-tool-version-manager, osat-manager-python, osat-manager-git, urn:uuid:19425b6e-28ad-4d57-b7c1-dd4d56a7c06c"
dc:identifier: "git-on-windows--options-for-osat-manager-git"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/git/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-10-21"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Brought into line with Christopher Steel's decision of 2026-10-21
      and osat-manager-git decision 0001: conda-forge's git package is
      the manager's source on Windows, Linux and macOS. conda-forge git
      moves to Trial for the manager; MinGit moves to Assess as the
      fallback; dugite-native moves to Hold. The conda-forge win-64
      package 2.56.0 was downloaded, its checksum matched against
      repodata, and its contents inspected on Linux; the four Windows
      questions are restated against it, with the configuration design of
      decision 0001. Corrected sat:path. No Windows binary was run.
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
      recommendation: "trial" # conda-forge git as osat-manager-git's source on all three platforms; other options ringed individually in the entry
      reasoning: "conda-forge's git package for win-64 and win-arm64 is Git for Windows' PortableGit, repackaged by the git-feedstock from the upstream release with its SHA-256 pinned in the recipe. It is self-contained, declares no dependencies, carries one text placeholder and no binary ones, and ships git-lfs, which MinGit lacks. Using it gives osat-manager-git one source kind and one verification path on every platform, chosen by Christopher Steel on 2026-10-21 and recorded in osat-manager-git decision 0001. Against that: the package is unsigned, so trust rests on a reviewed lock file; it adds a link between Git for Windows and the user; it was built from 2.56.0.windows.1 while Git for Windows is at .windows.2; and it is about 121 MB compressed and 419 MB unpacked, against MinGit's 40 MB and 93 MB. Its system configuration still sets autocrlf and requires the LFS filter, so the manager must own that file."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-21T20:45:00-04:00"
  human: []
---

# Git on Windows, Options for osat-manager-git

Version: 0.2.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This entry compares the ways to obtain Git on Windows, for two uses: as the Windows source for osat-manager-git, which installs Git into user space beside osat-manager-python, and as Git for client machines, where git-cola and other tools expect it. Version 0.1.0 recommended MinGit from Git for Windows releases. On 2026-10-21 Christopher Steel chose conda-forge's git package instead, on Windows as on Linux and macOS, so the manager has one source kind and one verification path. This version records that choice, the evidence behind it from the conda-forge Windows package itself, what it costs, and how osat-manager-git decision 0001 handles the package's system configuration. MinGit stays as the documented fallback. The standard installer remains the recommendation for client machines a person uses directly.

## Sources and Acknowledgements

The comparison applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026a](#apa-uc-metrics-reference)). The options come from the git-scm.com Windows download page <a name="apa-gitscm-windows-citation"></a>([Git Project, n.d.](#apa-gitscm-windows-reference)), the Git for Windows 2.56.0.2 release behind it <a name="apa-gfw-release-citation"></a>([Git for Windows Project, 2026](#apa-gfw-release-reference)), and conda-forge's git-feedstock <a name="apa-git-feedstock-citation"></a>([conda-forge, n.d.](#apa-git-feedstock-reference)). The decision this version follows is osat-manager-git decision 0001 <a name="apa-decision-0001-citation"></a>([Steel, 2026b](#apa-decision-0001-reference)). Support periods come from the Git for Windows FAQ and its 32-bit support page.

On 2026-10-11, `MinGit-2.56.0.2-64-bit.zip` was downloaded, hashed, and unpacked on Linux. On 2026-10-21, `win-64/git-2.56.0-h57928b3_0.conda` was downloaded, hashed against conda-forge's repodata, and unpacked on Linux, and the repodata for `win-arm64`, `linux-64` and `osx-arm64` was read. No Windows binary was run. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

The subject is Git for Windows 2.56.0, in each of the forms it is offered, including conda-forge's repackaging. Git's own behaviour is out of scope. Linux and macOS are covered only as far as the source decision reaches them: the manager uses conda-forge's git there too, and dugite-native, the candidate this entry named in 0.1.0, is set aside. The mise-en-place entry covers how conda-forge packages are installed without conda on those platforms. On 2026-10-21 mise 2026.10.17 locked `win-64` git with the SHA-256 given below, but could not solve `win-arm64`, which its conda backend does not map; `win-arm64` has no working reference implementation. Network behaviour and accessibility of the Windows builds were not tested, since nothing was run on Windows.

## The options

| Option | Artifact | What it is | Use |
|---|---|---|---|
| conda-forge git | `win-64/git-2.56.0-h57928b3_0.conda`, also `win-arm64` | PortableGit repackaged by conda-forge: full Git for Windows with Git Bash, Git Credential Manager and git-lfs, under `Library/`. 120.8 MB package, 419 MB and 9,612 files unpacked (verified) | osat-manager-git |
| MinGit | `MinGit-2.56.0.2-64-bit.zip`, also ARM64 and 32-bit | Git for Windows' minimal build for applications that bundle Git: `git.exe` and its helpers, Git Credential Manager, no Bash, no GUI tools, no git-lfs. 39.8 MB zip, 93 MB unpacked (verified) | Fallback for the manager |
| MinGit, busybox variant | `MinGit-2.56.0.2-busybox-64-bit.zip`, also ARM64 and 32-bit | MinGit with BusyBox in place of the standard Unix tools; smaller, less compatible | None |
| PortableGit | `PortableGit-2.56.0.2-64-bit.7z.exe`, also ARM64 | Full Git for Windows unpacked into any folder with no registry or PATH changes; a self-extracting 7-Zip archive that 7-Zip can open without running it. This is what conda-forge repackages | Covered by conda-forge git |
| Full archive | `Git-2.56.0.2-64-bit.tar.bz2`, also ARM64 | The full Git as a plain archive | None |
| Standalone installer | `Git-2.56.0.2-64-bit.exe`, also ARM64 | Full Git for Windows with Git Bash, Git GUI, gitk, Explorer menus, and PATH changes | Client machines, git-cola |
| winget | `winget install --id Git.Git -e --source winget` | The standalone installer, through Microsoft's package manager | Client setup where winget is present |
| Build from source | Git for Windows repository | Compile it yourself | None |

The git-scm.com page lists only the installer, PortableGit, winget, and source. MinGit, its busybox variants, and the full archives appear only on the release. conda-forge's package appears only on conda-forge's channel.

## The decisive observation

The osat-manager pattern needs an artifact it can download, verify against a published checksum, unpack into a versioned directory, and point an alias at, with no installer and no system changes. Both MinGit and conda-forge's package meet that on Windows. What separates them is the rest of the manager: on Linux and macOS, Git publishes no portable binaries, so the manager needs conda-forge there regardless. Taking conda-forge on Windows too means one source kind, one lock file format, one verification path and one set of tests, rather than a conda path on two platforms and a release-notes parser on the third.

The Windows package costs little extra code. It declares no dependencies, and one text file, `Library/etc/profile.d/env.sh`, carries the build prefix; no binary does (verified from `info/has_prefix`). Relocation on Windows is a single text substitution in a Bash profile script.

What it costs instead is size and distance from upstream, covered under Trade-offs.

## Gates for Git for Windows, as packaged by conda-forge

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | Git has no telemetry. Git Credential Manager contacts a hosting service only when credentials are requested for it. Not traced on Windows |
| A licence incompatible with the project | Pass, for use | Verified | Git is GPL-2.0-only; the bundled `COPYING` and `LICENSE.txt` state that only version 2 applies. conda-forge's metadata labels the package `GPL-2.0-or-later and LGPL-2.1-or-later`, which is wrong for Git itself; osat-manager-git records `GPL-2.0-only`. Using it raises no issue; redistributing it carries the GPL's usual source obligations, and Git's code cannot be combined into AGPL-3.0-or-later or GPL-3.0-or-later works |
| Failure of the accessibility floor | Pass | Inferred | Used as a command-line tool with no interface of its own. The bundled Git GUI and gitk were not tested and are not what the manager exposes |
| No exit | Pass | Verified | Repositories are plain Git. Removing an installation means deleting its directory |

## Verification and chain of custody

conda-forge publishes each package's SHA-256 and size in the channel's repodata, served over HTTPS. The downloaded `win-64` package hashed to `908bc78bf15591b9d093cb4e4c465f75f691f36bf5caa28971394dfa89d0fea0`, matching repodata (verified). The package itself carries no signature.

The rendered recipe inside the package names its upstream: `PortableGit-2.56.0-64-bit.7z.exe` from the Git for Windows `v2.56.0.windows.1` release, with that file's SHA-256 pinned, and Git's `COPYING` from the same tag, also pinned (verified). So the chain is Git for Windows' signed release, then the feedstock's pinned hash, then conda-forge's build, then repodata over HTTPS, then osat-manager-git's lock file.

The lock file is where trust is anchored. Per decision 0001, each git version and platform gets a lock naming every package's URL, SHA-256 and size, produced at release time, reviewed when it changes, and signed with the release key. Sigstore attestations under CEP 27 become required once conda-forge publishes them for every platform.

MinGit, by comparison, is one link shorter: a SHA-256 table in the release notes and a GPG-signed release tag, both from Git for Windows itself. Its checksums are a markdown table rather than a SHA256SUMS file, so the manager would have had to parse them.

## Trade-offs against MinGit

- **A longer chain.** conda-forge stands between Git for Windows and the user. A compromised or careless feedstock build would reach users who review only the lock.
- **Lag.** The 2.56.0 package was built from `v2.56.0.windows.1`, while Git for Windows has since released `v2.56.0.windows.2`. Windows patch releases reach users only after the feedstock rebuilds and a new lock is reviewed.
- **Size.** About 121 MB per archived version and 419 MB unpacked, against MinGit's 40 MB and 93 MB. The manager keeps a verified archive of each installed version, so this multiplies. Size is not only a Windows cost: on Linux, mise installed the same git version as 26 packages, 55 MB downloaded and 255 MB installed, perl and icu among them (observed 2026-10-21).
- **Long paths.** 9,612 files under `Library/`, some deep in `ucrt64/share/doc`. The manager unpacks below a short install root, since long-path support is not assumed.
- **More than the manager needs.** Git Bash, Git GUI, gitk and tig come along. That is also an advantage for users who want Git Bash, which 0.1.0 would have served with an optional PortableGit variant.

## The four Windows questions, for the conda-forge package

0.1.0 listed four decisions MinGit needed before Adopt. The conda-forge package was inspected for each.

- **Line endings.** `Library/etc/gitconfig` sets `core.autocrlf = true`, as MinGit does (verified). Under decision 0001 the package's file stays as delivered, and the manager writes its own system configuration for each version, which includes the package's file and then sets the baseline, read by git through `GIT_CONFIG_SYSTEM`. The proposed baseline sets `core.autocrlf = false`, with line endings governed by each repository's `.gitattributes`. Still to confirm: the baseline value itself.
- **Git LFS.** The same file declares the `lfs` filter with `required = true`. Unlike MinGit, the package ships `ucrt64/bin/git-lfs.exe` (verified), so an LFS checkout does not fail for want of the executable. On Linux and macOS, git-lfs is a separate conda-forge package (3.8.0 on every platform checked), so a git-lfs companion to manage-git is still wanted there.
- **Inherited configuration.** MinGit's system configuration includes `C:/Program Files/Git/etc/gitconfig`. The conda-forge package's `Library/etc/gitconfig` has no include lines (verified). Because the managed git reads the manager's file through `GIT_CONFIG_SYSTEM`, it does not read another installation's system file. Git for Windows may still read machine-wide configuration from `C:\ProgramData\Git\config`; whether `GIT_CONFIG_SYSTEM` suppresses that was not tested, and the Windows end-to-end run settles it.
- **Paths.** The package keeps the 2.56 layout under `Library/ucrt64`, and its stable entry point is `Library\cmd\git.exe` (verified). Decision 0001 points aliases there.

A fifth item surfaced in the inspection: the system configuration sets `credential.helper = helper-selector`, which asks the user to pick a helper through a dialog. `credential.helper` is a list, so the manager's file writes an empty value first, which clears the package's helper; this was verified with a stand-in helper and `git credential fill` on Linux. The baseline still has to name the helper, likely Git Credential Manager, which the package ships, so no dialog appears in unattended use.

## Older systems

Git for Windows supports Windows 8.1 and later, on x64 and ARM64 <a name="apa-gfw-faq-citation"></a>([Git for Windows Project, n.d.-a](#apa-gfw-faq-reference)). Older systems can only run frozen releases.

| System | Last Git for Windows release |
|---|---|
| Windows 7 and 8 | 2.46.2 |
| Windows Vista and Server 2008 | 2.37.1 |
| 32-bit, installer and PortableGit | 2.48.1 |
| 32-bit, MinGit only | Supported until April 2029 |

The 32-bit dates come from the project's support page <a name="apa-gfw-32bit-citation"></a>([Git for Windows Project, n.d.-b](#apa-gfw-32bit-reference)). conda-forge publishes no 32-bit Windows git, and its oldest git across every platform in current repodata is 2.49.0, so the conda-forge route cannot serve Windows 7, 8 or 32-bit machines at all. MinGit is the only route for 32-bit Windows; frozen releases remain the only route for Windows 7 and 8, with no further Git security fixes. Client-facing guidance should say so plainly.

## Recommendation

| Option | Ring | For |
|---|---|---|
| conda-forge git, `win-64` and `win-arm64` | Trial | osat-manager-git's source on Windows, as on Linux and macOS |
| MinGit, from Git for Windows releases | Assess | The fallback if a flip condition below is met, and the only route for 32-bit Windows |
| dugite-native | Hold | Set aside on all platforms: it repackages MinGit on Windows, needs glibc 2.34 and the system's libcurl on Linux, and is built for GitHub Desktop |
| PortableGit, directly | Hold | conda-forge's package is PortableGit, with a checksum in repodata and a lock to pin it |
| Standalone installer, or winget | Assess | Client machines a person uses directly, and git-cola, whose README asks for it |
| MinGit busybox variants | Hold | Less compatible, no offsetting benefit |
| Building from source | Hold | The official builds already meet the need |
| Frozen releases for Windows 7, 8, or Vista | Hold | Only as a declared bridge, with the security cost stated |

conda-forge git moves to Adopt when osat-manager-git installs, switches, and removes it end to end on real Windows hardware, x64 first, with these settled and recorded in decision 0001 or its successors:

- The baseline's `core.autocrlf` and credential helper values, confirmed.
- No configuration inherited from another Git installation on the same machine, shown by the end-to-end run.
- A signed lock for each released version, reviewed against repodata and the anaconda.org API.
- An HTTPS clone in every end-to-end run, proving Git Credential Manager and the bundled certificates work from the managed prefix.

## Flip conditions

- If conda-forge's Windows builds lag Git for Windows' security releases by more than a reasonable window, or stop being published for `win-64`, MinGit becomes the Windows source and the manager carries a second source kind.
- If the size of archived versions becomes a burden on low-storage or low-bandwidth clients, MinGit becomes the default and conda-forge an optional full variant.
- If conda-forge publishes signed attestations for its packages, the trust gap with MinGit narrows and the lock's role becomes a cross-check.
- If 32-bit Windows clients need managed Git, MinGit is the only option and is added for that platform alone.

## Relationship to project

This entry gives osat-manager-git its Windows source and records why the same source serves Linux and macOS. The uc-automa project plan for osat-manager-git and decision 0001 in the osat-manager-git repository carry the build details: lock files, relocation, re-signing on macOS, and the baseline. The mise-en-place entry is the reference design for installing conda-forge packages without conda. git-cola, evaluated separately, needs Git on the path and would be a natural consumer of the manager.

## Status notes

- Last reviewed: 2026-10-21
- In assess: the automated pass recommends conda-forge git for Trial as the manager's source on all three platforms, following the decision of 2026-10-21. What moves it to Adopt is the manager working end to end on real Windows, with the conditions above met.
- Re-review triggers: a Git for Windows security release, to measure how long the feedstock takes to follow; any change to the package's system configuration; conda-forge publishing attestations; and the end of 32-bit MinGit in April 2029.

## License

This document, *Git on Windows, Options for osat-manager-git*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### osat-manager-git

- [Decision 0001: Build manage-git](#apa-decision-0001-reference)

### conda-forge

- [git-feedstock](#apa-git-feedstock-reference)

### Git for Windows

- [Install Git on Windows, git-scm.com](#apa-gitscm-windows-reference)
- [Release 2.56.0.windows.2](#apa-gfw-release-reference)
- [FAQ, supported Windows versions](#apa-gfw-faq-reference)
- [32-bit support](#apa-gfw-32bit-reference)

## References

<a name="apa-git-feedstock-reference"></a>conda-forge. (n.d.). *git-feedstock* [Computer software]. GitHub. Retrieved October 21, 2026, from https://github.com/conda-forge/git-feedstock
[Return to citation](#apa-git-feedstock-citation)

<a name="apa-gfw-faq-reference"></a>Git for Windows Project. (n.d.-a). *Frequently asked questions*. https://gitforwindows.org/faq
[Return to citation](#apa-gfw-faq-citation)

<a name="apa-gfw-32bit-reference"></a>Git for Windows Project. (n.d.-b). *32-bit support of Git for Windows*. https://gitforwindows.org/32-bit
[Return to citation](#apa-gfw-32bit-citation)

<a name="apa-gfw-release-reference"></a>Git for Windows Project. (2026). *Git for Windows 2.56.0(2)* [Software release]. GitHub. https://github.com/git-for-windows/git/releases/tag/v2.56.0.windows.2
[Return to citation](#apa-gfw-release-citation)

<a name="apa-gitscm-windows-reference"></a>Git Project. (n.d.). *Install Git on Windows*. https://git-scm.com/install/windows
[Return to citation](#apa-gitscm-windows-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026a). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

<a name="apa-decision-0001-reference"></a>Steel, C. (2026b). *Decision: Build manage-git* (Version 0.1.0) [Decision record]. osat-manager-git. urn:uuid:19425b6e-28ad-4d57-b7c1-dd4d56a7c06c
[Return to citation](#apa-decision-0001-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft, version provisional | Follows the 2026-10-21 decision: conda-forge git for Trial as osat-manager-git's source on Windows, Linux and macOS; MinGit to Assess as fallback; dugite-native and PortableGit to Hold. Adds the conda-forge package's chain of custody, trade-offs including the Linux prefix's size, and the four Windows questions restated against it and against decision 0001's configuration design, plus the credential helper and the `win-arm64` gap. Corrects `sat:path` |
| 0.1.0 | Draft, version provisional | Initial comparison of the Windows Git options. Automated recommendation: MinGit from Git for Windows releases for Trial as osat-manager-git's Windows source, with four conditions for Adopt and three flip conditions |

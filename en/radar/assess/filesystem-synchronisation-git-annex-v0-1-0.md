---
title: "SAT radar: git-annex"
date: 2026-06-22
description: >
  Universal Cake SAT radar entry for git-annex. Assess. Strongest sovereignty
  and integrity model in the libre file synchronisation landscape; structurally
  unsuitable for broad public-facing deployment due to English-only UI, absent
  iOS client, degraded Android experience, and CLI-primary interaction model.
subject: file synchronisation, git, versioning, accessible technology, open source
type: radar
ring: assess
language: en-CA
rights: CC BY 4.0
source: https://git-annex.branchable.com/
relation: filesystem-synchronisation-overview-v0-1-0.md
version: "0.1.0"
---

# SAT radar: git-annex

**Ring:** Assess
**Category:** File synchronisation
**Licence:** GPL3+ / AGPL3+
**Language:** Haskell
**Maintainer:** Joey Hess (solo)
**First release:** 2010
**Last release:** 10.20251114 (November 2025)
**Platforms:** Linux, macOS, Windows (beta), Android (degraded); no iOS native client

---

## What it is

git-annex is a distributed file synchronisation system that manages large file collections using Git for metadata and a separate content-addressable object store for file data. It does not store file contents in Git history; instead it commits a symlink (or pointer file on filesystems without symlink support) and tracks content locations on a dedicated `git-annex` branch. Users can clone a repository and selectively fetch only the files they need, leaving the rest as metadata-only pointers.

The tool was funded by a Kickstarter campaign in 2012–13 to build the git-annex assistant — a background daemon with a web UI intended to give non-technical users a Dropbox-like experience. That assistant remains available but is not the primary interface for most users.

---

## Universal Cake criteria

| Criterion | Rating | Notes |
|---|---|---|
| Zero cost | Strong | Free, no accounts, no freemium tier |
| Accessibility / disability | Weak | CLI-primary; webapp reported slow (bug open Dec 2024); no WCAG audit |
| Device reach | Partial | Linux/macOS/Windows strong; Android degraded; no iOS native client |
| Multilingual | Weak | English only; no i18n infrastructure in codebase |
| Data sovereignty | Strong | No cloud intermediary; user controls all storage locations |
| Low setup burden | Weak | Requires Git knowledge; metadata/content sync distinction is non-obvious |

---

## Strengths

**Sovereign by design.** No accounts, no corporate dependency. Data lives exactly where the user decides — local drives, SSH servers, NAS, S3-compatible storage, WebDAV, or physical media. Over 50 special remote backends are supported.

**Git-native versioning and integrity.** Every file operation is a Git commit. Content is addressed by cryptographic hash (SHA256 by default). Checksums are verified on transfer. The `numcopies` setting prevents dropping the last copy of a file. This is the strongest integrity model of any libre synchronisation tool reviewed.

**Partial presence.** A device can hold full metadata for a multi-terabyte collection while storing only the files it needs. `git annex get` fetches on demand; `git annex drop` frees space while preserving the pointer. No other libre tool in the UC comparison handles this cleanly.

**Future-proof file access.** Files sit on disk as ordinary files, with symlinks pointing at annex objects. Neither Git nor git-annex is required to read the raw content — a meaningful advantage for archival and long-term accessibility use cases. A drive placed in a time capsule remains readable without the tool.

**Actively maintained.** Monthly releases throughout 2024–2025. Active devblog documents substantive ongoing work: stall detection and cancellation, concurrent checksum and download pools, `git-remote-annex`, p2phttp server. Not at risk of abandonment in the short term.

**Scales to large collections.** Hundreds of thousands of files are not a problem. Interrupted transfers resume automatically. Memory usage is designed to be constant rather than proportional to repository size.

---

## Areas needing work

**No iOS client; degraded Android.** No native iOS client exists. The Android port is present but forum threads document persistent performance problems — unusably slow on moderately sized repositories, constant high CPU usage — that have been open since 2016 without resolution.

**English only.** The CLI, webapp, and assistant are English-only. No i18n infrastructure exists in the codebase. This is a structural gap for any UC deployment serving francophone, allophone, or multilingual communities. Closing it would require upstream architectural work, not just translation strings.

**No accessibility audit; CLI-primary.** The tool is CLI-first. The webapp exists but was filed as "terribly slow" in December 2024, with the bug still open at time of writing. No WCAG conformance work has been performed. Screen reader experience is untested. For a non-technical or disabled user, git-annex is not a realistic direct recommendation without significant operator scaffolding.

**Windows is second-class.** The Windows port works for CLI use but carries documented limitations: no SSH connection caching (making per-file SSH transfers noticeably slower), Unicode filename encoding issues (mojibake with non-ASCII filenames), no gcrypt support, and webapp reliability concerns. The port is community-maintained via DataLad CI rather than first-party.

**Solo maintainer risk.** Joey Hess is the single maintainer of a 15-year-old Haskell codebase. The Haskell build toolchain adds packaging complexity that raises the barrier to outside contribution. DataLad provides meaningful secondary stewardship (Windows CI, active bug reporting) but is not a co-maintainer. If Joey stepped away, the succession path is unclear.

**Mental model complexity.** The distinction between `git annex sync` (syncs metadata only) and `git annex sync --content` (also transfers file content) is a persistent source of user confusion documented throughout the forum. The `annex.largefiles` / `.gitattributes` interaction for mixed repositories adds a further layer. These are not insurmountable but require documentation investment for any general-public deployment.

**FAT/exFAT and non-symlink filesystems.** git-annex uses symlinks as its core mechanism. On filesystems without symlink support — FAT32, exFAT, some Android paths, some NAS shares — performance degrades significantly or a workaround bare-repo configuration is required. USB drives formatted for cross-platform use are a common case where this matters in practice.

**Not a backup tool.** Joey Hess is explicit: git-annex is for file distribution and archiving, not backup. `numcopies` tracks copy count but does not continuously verify integrity. A UC recommendation must pair it with an explicit backup layer and plain-language guidance to avoid user misconceptions.

---

## UC positioning

git-annex is an operator-layer tool, not a client-layer recommendation. It is appropriate for:

- technically capable operators managing large archival collections
- knowledge infrastructure workflows where Git-native versioning and integrity matter (SAT-adjacent, municipal document management)
- scenarios where partial presence across many devices and storage backends is a genuine requirement

It is not appropriate as a direct recommendation to general community members using whatever device they have, until the multilingual gap, iOS absence, and accessibility baseline are addressed.

The partial presence and future-proof access properties are genuinely unique in the libre landscape and represent a meaningful UC-aligned capability worth tracking.

---

## See also

- [git-annex official site](https://git-annex.branchable.com/)
- [DataLad](https://www.datalad.org/) — scientific data management layer built on git-annex
- `filesystem-synchronisation-syncthing-v0-1-0.md`
- `filesystem-synchronisation-nextcloud-v0-1-0.md`
- `filesystem-synchronisation-overview-v0-1-0.md`

---

*SAT radar entry v0.1.0. Reviewed 2026-06-22 against Universal Cake criteria: libre licensing, active maintenance, zero cost to end users, disability accessibility, cross-platform device reach, multilingual UI, and data sovereignty.*

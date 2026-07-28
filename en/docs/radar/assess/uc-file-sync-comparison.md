---
title: "Universal Cake file synchroniser comparison"
date: 2026-06-22
description: >
  Evaluation of cross-platform libre file synchronisation tools for
  suitability as Universal Cake recommendations, scored against cost,
  disability accessibility, device reach, multilingual support, and
  data sovereignty criteria.
subject: file synchronisation, accessible technology, open source
type: evaluation
language: en-CA
rights: CC BY 4.0
source: https://en.wikipedia.org/wiki/Comparison_of_file_synchronization_software
version: "0.1.0"
---

# Universal Cake file synchroniser comparison

## Evaluation criteria

Universal Cake recommendations weight the following traits for this category. All must be considered together; no single criterion is sufficient.

- **Zero cost** — free to all users, no freemium trap or per-user licensing
- **Accessibility / disability** — screen reader compatible, keyboard navigable, plain UI; WCAG 2.1 AA where possible
- **Device reach** — meaningful coverage of Windows, macOS, Linux, Android, and iOS
- **Multilingual** — localised UI; English-only tools disadvantage francophone, allophone, and multilingual communities
- **Data sovereignty** — no third-party cloud intermediary; user controls where data lives
- **Low setup burden** — non-technical users can start with plain-language guidance

Source table: [Comparison of file synchronization software](https://en.wikipedia.org/wiki/Comparison_of_file_synchronization_software) (Wikipedia, accessed 2026-06-22). Only libre-licensed, cross-platform tools are considered; freeware and commercial entries are excluded.

---

## Adopt

### Syncthing

| Attribute | Detail |
|---|---|
| Language | Go |
| Licence | MPL v2 |
| Last release | 2025 (actively maintained) |
| Platforms | Windows, macOS, Linux, Android; iOS via Möbius Sync (third-party, free, open source) |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free | Partial | No iOS native | 30+ languages | Full | Moderate |

Strongest overall UC fit. Completely free with no accounts and no cloud intermediary — data never leaves devices the user controls. Over 30 language translations are maintained by the community. Windows, macOS, Linux, and Android all have first-party clients.

The iOS gap is a meaningful concern for broad public deployments: the third-party Möbius Sync client is free and open source, but it adds a step and a dependency that may be a barrier for users with lower technical confidence. For any UC deployment where a significant share of users are on iPhones, this should be flagged explicitly in onboarding documentation.

The web UI is keyboard-navigable but has not been audited against WCAG; screen reader experience is untested. A one-page plain-language setup guide (sharing device IDs, approving connections) closes the setup barrier for most users. The sovereignty story is the strongest of anything in this comparison.

---

## Assess

### Nextcloud

| Attribute | Detail |
|---|---|
| Language | PHP, JavaScript, Vue, Python, Shell |
| Licence | AGPLv3 |
| Last release | 2023 (actively maintained) |
| Platforms | Windows, macOS, Linux, Android, iOS (server: Linux, FreeBSD) |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free* | WCAG effort | All five platforms | 40+ languages | Self-hosted | High (server required) |

Best accessibility and device coverage of any option in the list; highest setup barrier. Nextcloud has made documented WCAG 2.1 AA efforts and has an active accessibility working group with issues tracked publicly — the furthest along of any libre tool here. Native clients exist for all five major platforms including iOS. Fully self-hosted means full sovereignty.

The asterisk on cost: clients are free, but someone must run or pay for a server. For a UC deployment serving a community — a municipality, a co-operative, a project like Yes Montréal — one hosted instance serves all users at effectively zero per-user cost. This model makes Nextcloud the appropriate recommendation when a trusted operator can run the server.

**Not** appropriate to recommend to individuals who must self-host from scratch without technical support.

**Practical UC positioning:** Nextcloud (operator-hosted) is the recommendation for community deployments requiring a consistent, accessible, multilingual interface for a general public who should not need to manage their own setup.

---

### rclone

| Attribute | Detail |
|---|---|
| Language | Go |
| Licence | MIT |
| Last release | 2025 (actively maintained) |
| Platforms | Linux, Windows, macOS, FreeBSD, NetBSD, OpenBSD, Plan 9, Solaris |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free | CLI only | No mobile | English UI | Full | Technical |

Assess for operator and administrator use only; not an end-user recommendation. rclone is a CLI tool with no GUI and no mobile clients — inaccessible to non-technical and disabled users as a direct recommendation. It supports over 50 cloud, protocol, and virtual backends including S3 and SFTP, and pairs naturally with ZFS snapshots as the versioning layer.

Valuable at the UC operator layer for scripted backup and cloud-bridging workflows. Should never be the tool a community member is told to install themselves.

This represents a significant repositioning from an infrastructure-first evaluation: rclone drops from Adopt to Assess when broad client accessibility is the primary criterion.

---

## Hold

### FreeFileSync

| Attribute | Detail |
|---|---|
| Language | C++ |
| Licence | GPL up to version 12.5 (libre releases stalled July 2023) |
| Last libre release | 12.5, July 2023 |
| Platforms | Windows, macOS, Linux (desktop only) |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free* | GUI, untested | Desktop only | Many | Local only | Moderate |

GUI-only and desktop-only; excludes all mobile users. The libre licence stalled at version 12.5 in July 2023; upstream has moved to donation-ware with non-libre builds, creating long-term uncertainty. A past adware history (OpenCandy bundling in pre-2018 versions) is a trust concern for accessibility-first recommendations.

The multilingual GUI is a genuine positive. Acceptable as a desktop-to-desktop tool for a technically confident user with no mobile sync requirement, but not appropriate as a broad UC recommendation.

---

### rsync

| Attribute | Detail |
|---|---|
| Language | C |
| Licence | GPL v3 |
| Last release | 2025 (actively maintained) |
| Platforms | Windows, macOS, Linux, BSD |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free | CLI only | No mobile, no Mac GUI | English CLI | Full | Technical |

CLI-only, no conflict detection, no rename detection, no mobile. Inaccessible to the broad client base UC targets. No bidirectional sync. Hold as an end-user tool.

rsync remains valuable as an infrastructure primitive — rclone wraps rsync semantics with a better interface, and `zfs send/receive` is more appropriate than rsync for ZFS snapshot replication. Keep in the operator toolkit; do not surface to end users.

---

### Unison

| Attribute | Detail |
|---|---|
| Language | OCaml |
| Licence | GPL |
| Last release | 2024 (actively maintained) |
| Platforms | Windows, macOS, Linux |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free | Interactive CLI | No mobile | English only | Full | Technical |

Academically rigorous conflict resolution model and a long pedigree. Desktop and CLI only, English only, no mobile clients. Its interactive conflict resolution prompts — a strength for expert users — are a barrier for anyone using assistive technology or unfamiliar with terminal workflows.

Drops from Assess to Hold under accessibility criteria. Appropriate for a two-machine archival workflow between technically confident users; not appropriate for a general public recommendation.

---

### Seafile (community edition)

| Attribute | Detail |
|---|---|
| Language | C99, Python |
| Licence | AGPLv3 (server), Apache 2.0 (client) |
| Last release | 2020 per Wikipedia record |
| Platforms | Linux, Windows, macOS, Android, iOS (server: Linux, Windows) |

**Criteria matrix**

| Cost | A11y | Devices | Languages | Sovereignty | Setup |
|---|---|---|---|---|---|
| Free | GUI, untested | All five | Unknown | Self-hosted | High (server) |

Broad platform coverage including iOS is a positive, but the Wikipedia record shows no releases since 2020. Maintenance uncertainty alongside the server requirement makes this a hold. Nextcloud is a better-supported alternative for any deployment requiring a server platform.

---

## Avoid

The following tools from the Wikipedia comparison table are unsuitable for UC recommendation. All fail on one or more disqualifying grounds: abandonment, CLI/desktop-only with no mobile support, Java dependency, or architectural mismatch with deterministic user-facing file sync.

| Tool | Last release | Primary reason to avoid |
|---|---|---|
| SparkleShare | 2017 | Abandoned; Git-backed sync concept is interesting but unmaintained |
| DirSync Pro | 2018 | Stale; Java dependency; Windows-primary GUI |
| Synkron | 2011 | Effectively abandoned |
| luckyBackup | 2018 | rsync GUI front-end; stale; superseded by rclone |
| Kubo (IPFS) | 2022 | Not a conventional sync tool; conflict detection unimplemented; mismatched architecture |
| Conduit | 2010 | Abandoned |
| SymmetricDS | 2018 | Database sync framework, not a file sync tool for end users |

---

## Summary and practical UC two-tier recommendation

The accessibility lens reshapes ranking significantly from an infrastructure-first view. Two findings hold across both framings:

1. **Sovereignty and cost align.** The tools with the best sovereignty story (Syncthing, Nextcloud self-hosted) are also the ones with zero per-user cost. Commercial freeware tools are where those two criteria diverge.

2. **The accessibility audit gap is a known weakness across all libre options.** None have published WCAG conformance reports. Nextcloud is furthest along; all others are untested or CLI-only. This is an open gap requiring community contribution or workaround documentation in any formal UC deployment.

**Recommended two-tier deployment model:**

- **Syncthing** — for technically capable users and peer-to-peer scenarios between known devices. Requires a plain-language onboarding guide and an explicit note about the iOS limitation.
- **Nextcloud (operator-hosted)** — for community deployments where a single trusted instance serves many users. Provides consistent, accessible, multilingual interfaces without requiring each person to manage their own infrastructure.

---

*Evaluated against source data from the Wikipedia comparison table (accessed 2026-06-22). Ratings reflect Universal Cake criteria: libre licensing, active maintenance, zero cost to end users, disability accessibility, cross-platform device reach, multilingual UI, and data sovereignty. This document is version 0.1.0; criteria weightings and tool ratings should be reviewed when the SAT radar process is applied formally.*

---
dc:title: "Universal Cake Evaluation: psutil, System Monitoring Library"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "psutil"
  - "python"
  - "system monitoring"
dc:description: "Universal Cake evaluation of psutil 7.2.2, the cross-platform Python library for process and system information, as a general Python dependency."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-08"
dcterms:modified: "2026-10-08"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/giampaolo/psutil"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template"
dc:identifier: "universal-cake-evaluation--psutil--system-monitoring-library"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-08"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of psutil 7.2.2 against Universal
      Cake Evaluation Metrics 0.5.1, scoped to use as a general Python
      dependency installed from PyPI wheels, with network and file
      system behaviour verified on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # once a consuming use is named and the two Inferred gates are verified
      reasoning: "All four default gates pass, telemetry and licence Verified. No runtime dependencies, no network contact, no stored data, BSD-3-Clause. Main risks are a single maintainer (86% of all commits, 91% over the last two years), releases uploaded manually without PyPI provenance attestations, breaking changes and dropped support for Windows before 10 and Python 3.6 and 3.7 in the coming 8.0, and third-party analytics on the documentation site."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-08T10:42:00-04:00"
  human: []
---

# Universal Cake Evaluation: psutil, System Monitoring Library

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates psutil, the cross-platform Python library for reading process and system information, against the Universal Cake Evaluation Metrics. It records the gate results, a scorecard, the findings that shaped the ratings, and a recommendation for the radar. psutil passes all four default gates. Its strengths are structural: a permissive licence, no runtime dependencies, no network contact, and no data of its own. Its risks sit almost entirely in the project around it: one person maintains it, releases are uploaded from that person's machine without provenance attestations, and the coming 8.0 release drops support for older Windows and Python versions.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about psutil come from files in the psutil repository, read directly at commit `48285695` and at the `release-7.2.2` tag on 2026-10-08, from the 7.2.2 wheel published on PyPI, and from a network and file system trace of that wheel. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate psutil 7.2.2, the current release on PyPI, as a Python library dependency installed from a binary wheel. No consuming project was named for this evaluation, so it is scoped to the general case: Python scripts and tools that read process, CPU, memory, disk, network, and sensor information from the local machine. Network and file system behaviour were verified on Linux only. Windows, macOS, and the BSDs were assessed from source and documentation.

psutil reads information; it does not collect, store, or transmit it. Whatever a consuming tool does with that information, such as logging it or sending it to a monitoring service, is a property of that tool and is out of scope here.

## Scorecard metadata

- Product or service name: psutil
- Version evaluated: 7.2.2, `psutil-7.2.2-cp36-abi3-manylinux2010_x86_64.manylinux_2_12_x86_64.manylinux_2_28_x86_64.whl`, SHA-256 `076a2d2f923fd4821644f5ba89f059523da90dc9014e85f8e45a5774ca5bc6f9`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-08
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, CPython 3.13

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry exists to disable. A trace of every network system call while exercising the main APIs showed only local kernel and local socket traffic, see Findings. Source inspection found no outbound network code on any platform |
| A licence incompatible with the project | Pass | Verified | BSD-3-Clause, read from the repository. Compatible with AGPL-3.0-or-later and GPL-3.0-or-later projects. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | A library with no user interface. Its documentation site has labelled landmarks but was not tested with a keyboard or screen reader |
| No exit | Pass | Inferred | psutil holds no data. Leaving means rewriting the call sites that use it |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | No interface of its own. The documentation theme provides a `main` landmark, labelled `nav` landmarks, and text labels on icon buttons (verified by reading the templates), but no skip link, and a Shift+D shortcut toggles dark mode. Keyboard and screen reader testing were not done |
| Multilingual integration | Moderate | Verified | English only, in documentation and error messages. The documentation templates wrap their strings for translation, so the theme could be localised, but no translations exist |
| Economic and cognitive accessibility | Strong | Verified, Inferred | Free. The Linux wheel is 155,560 bytes and 504 KB installed, with no runtime dependencies (verified). The API is small and consistent, and the documentation includes recipes and shell and standard-library equivalents |
| Representation | Unknown | None | Not assessable from public information. The project is maintained by one person, and nothing in its public documentation addresses who is represented in its design. No machine-learned models are part of the product, so the v0.5.0 training-data proxies do not apply to it |
| Compatibility | Strong | Verified, Claimed | Linux, Windows, macOS, FreeBSD, OpenBSD, NetBSD, Solaris, and AIX; wheels for x86_64 and ARM64 on Linux, Windows, and macOS, plus ppc64le and s390x on Linux; CPython 3.6 and later, and PyPy. The 8.0 release, in development, raises the floor to Python 3.8 and Windows 10, see Findings |
| Resilience | Strong | Verified | Works fully offline. No services, licence server, accounts, or tokens. Reads the operating system's own interfaces |
| Agency, sovereignty and privacy | Strong | Verified | BSD-3-Clause allows self-hosting, modification, forking, and redistribution without permission. psutil stores no data and makes no outbound network contact. Information it can read about other processes is limited by the operating system's own permissions |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms of service and no pricing. Data portability is not applicable, since psutil holds no data. Exit cost scales with the number of call sites and platforms a consumer supports, typically hours, not days |
| Agency, interaction patterns | Moderate | Verified | The library itself is Strong: explicit `AccessDenied` and `NoSuchProcess` exceptions, no prompts, no defaults that serve the vendor. The documentation site loads Google Analytics and GoatCounter with no notice or consent, a hidden-telemetry pattern under Visible costs. Lens: user (documentation reader) differs from owner |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | Compiled C reading kernel interfaces, with no daemon of its own. Energy cost depends on how often the consumer polls. Runs on old and low-end hardware. Authoring-side load is not applicable |
| Security | Moderate | Verified, Claimed | Published security policy routes private reports through Tidelift (verified). One CVE in its history, CVE-2019-18874, a double free fixed in 5.6.6 in 2019 (verified). No runtime dependencies (verified). A C extension, so memory-safety bugs are possible. Releases are uploaded from the maintainer's machine with `twine`; PyPI holds no provenance attestation for 7.2.2 and release tags are unsigned (verified). Patch speed was not measured |
| Longevity | Moderate | Verified, Claimed | In continuous development since 2008, with 369 commits in the last 90 days and 969 in the last two years (verified). The project describes itself as maintained by a single developer in his spare time, funded by sponsorship and Tidelift (claimed) |
| Content endurance | Strong | Inferred | psutil stores and authors nothing. Removing it leaves all content whole |
| Exit and portability | Strong | Inferred | See the No exit gate. Open standards are not at issue; replacement paths include `/proc` and `sysfs` on Linux, the standard library for some functions, and platform tools |
| Adjustability and support | Moderate | Verified, Inferred | BSD-3-Clause keeps the fork-and-fix path open, the code is well organised by platform, and documentation is extensive. Support is GitHub issues answered by one maintainer, with priority response offered to sponsors |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | The de facto standard in its niche, which the project reports as a top-100 PyPI package (claimed). No take rate. 8.0 introduces breaking API changes, documented in a migration guide with deprecation warnings for most renamed fields (verified). Forkable by licence, but governance is one person's account. Contributor concentration: 86% of all non-merge commits and 91% over the last two years are by the maintainer (verified). No foundation-model dependency in the product |
| Gates | Pass | Mixed | All four pass. Telemetry and licence Verified; accessibility floor and exit Inferred |

## Findings

### Network and file system behaviour

We installed the 7.2.2 wheel from PyPI and ran a script that called the main system APIs, including CPU, memory, disk, network interface, sensor, and user functions, then iterated over every process requesting names, users, command lines, memory, CPU times, and open files. The run was traced with `strace`, recording `connect`, `sendto`, `sendmsg`, and `bind`, and separately all file opens, creations, renames, and deletions.

The network trace held five calls. Three were netlink messages to the Linux kernel, used to list network interfaces and addresses. Two were attempts to reach the local name service cache socket, made by the C library when resolving user names, which failed because that service was not running. There were no Internet Protocol connections. The file system trace held no writes. Source inspection agreed: the Python layer imports `socket` only for address constants, address formatting, and a local IPv6 support check, and the C layer opens datagram sockets only to query interface properties, never calling `connect`.

### Licence and dependencies

psutil is licensed BSD-3-Clause <a name="apa-psutil-license-citation"></a>([psutil Project, n.d.-a](#apa-psutil-license-reference)). The 7.2.2 wheel's metadata declares no runtime dependencies; every `Requires-Dist` entry belongs to the optional `dev` or `test` extras. The Linux wheel uses the stable ABI, so one file serves every supported CPython version on that platform <a name="apa-psutil-pypi-citation"></a>([psutil Project, 2026](#apa-psutil-pypi-reference)).

### Security and supply chain

The security policy asks reporters to use the Tidelift security contact, which coordinates the fix and its disclosure, rather than public issues <a name="apa-psutil-security-citation"></a>([psutil Project, n.d.-b](#apa-psutil-security-reference)). The project's one recorded CVE, a double free caused by mishandled reference counts in a loop that builds Python objects from system data, affected releases through 5.6.5 <a name="apa-debian-cve-citation"></a>([Debian Security Tracker, n.d.](#apa-debian-cve-reference)) and was fixed in 5.6.6.

Wheels are built in GitHub Actions with `cibuildwheel`. The wheel-building workflow references its third-party actions by version tag rather than by commit hash, so a moved tag could change what runs; the issue-triage workflow, which handles secrets, does pin by hash. Publishing, however, is a manual `twine upload` run by the maintainer, followed by an unsigned annotated tag <a name="apa-psutil-makefile-citation"></a>([psutil Project, n.d.-h](#apa-psutil-makefile-reference)). PyPI returns no provenance attestation for the 7.2.2 files, so a consumer cannot confirm that a wheel on PyPI is the one CI built. Pinning by hash, for example with `pip install --require-hashes`, protects against a later substitution but not against a compromised first upload.

### Platform support and older systems

The 7.2.x series supports CPython 3.6 and later and Windows Vista and later. psutil 8.0, in development on the main branch, raises the minimum to Python 3.8 and Windows 10 or Server 2016, and the platform documentation names 7.2.2 as the last release for Python 3.6 and 3.7 <a name="apa-psutil-platform-citation"></a>([psutil Project, n.d.-c](#apa-psutil-platform-reference)); the migration guide names the 7.2.x series as the last for Windows before 10 <a name="apa-psutil-migration-citation"></a>([psutil Project, n.d.-e](#apa-psutil-migration-reference)). Linux support reaches back to kernel 2.6.13 with graceful fallbacks, and prebuilt macOS Intel wheels will require 10.15. For deployments that serve older systems, pinning to 7.2.x is a workable bridge, but that series will not receive the fixes and features that go into 8.x.

The 8.0 changelog lists breaking changes, among them reorganised process memory functions, changed named-tuple fields, and new enum classes, most with deprecation warnings rather than silent breakage <a name="apa-psutil-changelog-citation"></a>([psutil Project, n.d.-d](#apa-psutil-changelog-reference)). Upgrading consumers will need to read the migration guide.

### Maintainer concentration and funding

Of 6,542 non-merge commits since 2008, 5,633 (86%) are by Giampaolo Rodola, recorded under two spellings of his name. Over the two years to 2026-10-08, he wrote 885 of 969 (91%), with 54 other people and Dependabot contributing the rest. The project's funding page describes it as maintained by a single developer in his spare time and asks companies that depend on it to sponsor, offering priority response in return <a name="apa-psutil-funding-citation"></a>([psutil Project, n.d.-f](#apa-psutil-funding-reference)). Current activity is high, with an 8.0 release in progress. The risk is not inactivity but a bus factor of one: the repository sits under a personal account, and there is no published governance or succession arrangement. The licence makes a community fork possible if needed.

### Documentation site analytics

The documentation theme for psutil.io loads Google Analytics and GoatCounter on every page, with no notice or consent mechanism, and blog posts embed giscus, a third-party comment widget <a name="apa-psutil-layout-citation"></a>([psutil Project, n.d.-g](#apa-psutil-layout-reference)). This does not affect the library, and it does not fail the telemetry gate, which concerns the product. It does affect readers: anyone consulting the documentation is tracked by a third party unless they block those scripts. The documentation source is reStructuredText in the repository and builds offline with Sphinx, which gives a tracker-free alternative.

### AI in the development process

The project uses Anthropic's Claude through its API in CI to suggest labels for new issues and pull requests and, on request, to draft changelog and credits entries <a name="apa-psutil-changelog-bot-citation"></a>([psutil Project, n.d.-j](#apa-psutil-changelog-bot-reference)), and some recent commits carry Claude co-author trailers. The contributing guide tells reporters that a bot labels issues but does not say that issue text is sent to a third-party model provider <a name="apa-psutil-contributing-citation"></a>([psutil Project, n.d.-i](#apa-psutil-contributing-reference)). This is a property of the project's workflow rather than of the shipped library, which contains no models. Commit trailers give partial output provenance; the issue-text disclosure is the gap.

## Recommendation

Keep psutil in Assess for now, and move it to Trial as soon as a consuming use is named, on these conditions:

- Verify the two Inferred gates for that use, as the Trial ring requires: test the documentation site with a keyboard and a screen reader, and confirm the exit path for the specific call sites.
- Pin psutil by version and hash in the consuming project's lock file, since PyPI holds no provenance attestations.
- Decide per deployment between 7.2.x and 8.x. Pin to 7.2.x where Windows before 10, or Python 3.6 or 3.7, must be supported; otherwise plan the 8.0 migration when it is released.
- Read the documentation from a local Sphinx build, or with third-party scripts blocked, where reader tracking matters.

Re-review triggers specific to psutil: the 8.0 release; any change in maintainership, repository ownership, or governance; and any new CVE.

## License

This document, *Universal Cake Evaluation: psutil, System Monitoring Library*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### psutil repository

- [Licence](#apa-psutil-license-reference)
- [Security policy](#apa-psutil-security-reference)
- [Platform support](#apa-psutil-platform-reference)
- [Changelog](#apa-psutil-changelog-reference)
- [Migration guide](#apa-psutil-migration-reference)
- [Funding](#apa-psutil-funding-reference)
- [Documentation layout template](#apa-psutil-layout-reference)
- [Makefile](#apa-psutil-makefile-reference)
- [Contributing guide](#apa-psutil-contributing-reference)
- [Changelog bot](#apa-psutil-changelog-bot-reference)

### Releases and advisories

- [psutil 7.2.2 on PyPI](#apa-psutil-pypi-reference)
- [CVE-2019-18874](#apa-debian-cve-reference)

## References

<a name="apa-debian-cve-reference"></a>Debian Security Tracker. (n.d.). *CVE-2019-18874*. https://security-tracker.debian.org/tracker/CVE-2019-18874
[Return to citation](#apa-debian-cve-citation)

<a name="apa-psutil-license-reference"></a>psutil Project. (n.d.-a). *LICENSE* [Software licence]. GitHub. https://github.com/giampaolo/psutil/blob/master/LICENSE
[Return to citation](#apa-psutil-license-citation)

<a name="apa-psutil-security-reference"></a>psutil Project. (n.d.-b). *SECURITY.md* [Security policy]. GitHub. https://github.com/giampaolo/psutil/blob/master/SECURITY.md
[Return to citation](#apa-psutil-security-citation)

<a name="apa-psutil-platform-reference"></a>psutil Project. (n.d.-c). *Platform support*. GitHub. https://github.com/giampaolo/psutil/blob/master/docs/platform.rst
[Return to citation](#apa-psutil-platform-citation)

<a name="apa-psutil-changelog-reference"></a>psutil Project. (n.d.-d). *Changelog*. GitHub. https://github.com/giampaolo/psutil/blob/master/docs/changelog.rst
[Return to citation](#apa-psutil-changelog-citation)

<a name="apa-psutil-migration-reference"></a>psutil Project. (n.d.-e). *Migration guide*. GitHub. https://github.com/giampaolo/psutil/blob/master/docs/migration.rst
[Return to citation](#apa-psutil-migration-citation)

<a name="apa-psutil-funding-reference"></a>psutil Project. (n.d.-f). *Funding*. GitHub. https://github.com/giampaolo/psutil/blob/master/docs/funding.rst
[Return to citation](#apa-psutil-funding-citation)

<a name="apa-psutil-layout-reference"></a>psutil Project. (n.d.-g). *layout.html* [Documentation template]. GitHub. https://github.com/giampaolo/psutil/blob/master/docs/_templates/layout.html
[Return to citation](#apa-psutil-layout-citation)

<a name="apa-psutil-makefile-reference"></a>psutil Project. (n.d.-h). *Makefile* [Build script]. GitHub. https://github.com/giampaolo/psutil/blob/master/Makefile
[Return to citation](#apa-psutil-makefile-citation)

<a name="apa-psutil-contributing-reference"></a>psutil Project. (n.d.-i). *CONTRIBUTING.md*. GitHub. https://github.com/giampaolo/psutil/blob/master/CONTRIBUTING.md
[Return to citation](#apa-psutil-contributing-citation)

<a name="apa-psutil-changelog-bot-reference"></a>psutil Project. (n.d.-j). *changelog_bot.py* [Source code]. GitHub. https://github.com/giampaolo/psutil/blob/master/.github/workflows/changelog_bot.py
[Return to citation](#apa-psutil-changelog-bot-citation)

<a name="apa-psutil-pypi-reference"></a>psutil Project. (2026). *psutil 7.2.2* [Software release]. PyPI. https://pypi.org/project/psutil/7.2.2/
[Return to citation](#apa-psutil-pypi-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of psutil 7.2.2 against Universal Cake Evaluation Metrics 0.5.1, scoped to use as a general Python dependency installed from PyPI wheels, with network and file system behaviour verified on Linux |

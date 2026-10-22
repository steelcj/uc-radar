---
dc:title: "Universal Cake Evaluation: git-cola, Git Graphical Client"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "git-cola"
  - "git"
  - "version control"
  - "graphical user interface"
dc:description: "Universal Cake evaluation of git-cola 4.19.0, the open source Git graphical client written in Python and Qt, as a desktop tool for Linux, Windows and macOS."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-11"
dcterms:modified: "2026-10-11"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/git-cola/git-cola"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template"
dc:identifier: "universal-cake-evaluation--git-cola--git-graphical-client"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-11"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of git-cola 4.19.0 against Universal
      Cake Evaluation Metrics 0.5.1, scoped to use as a desktop Git client,
      with the PyPI release run headless on Linux under network tracing
      and its main window inspected for labelled controls.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for own and developer use with Gravatar turned off, once a screen reader pass is done
      reasoning: "All four default gates pass; telemetry Verified. With default settings the commit view sends a SHA-256 hash of each commit author's email address to gravatar.com; one setting turns this off, and with it off no outbound connections were observed. GPL-2.0-only, 17 translations with French complete, works with plain git repositories and leaves nothing to migrate. Main concerns are icon-only toolbar buttons with no accessible name and no keyboard focus, a Windows installer that bundles GPL-3.0-only PyQt6 alongside GPL-2.0-only code, releases uploaded manually without PyPI provenance, a lead maintainer with 85% of all commits, and an experimental unauthenticated remote-control server on the main branch, not yet released."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-11T17:20:00-04:00"
  human: []
---

# Universal Cake Evaluation: git-cola, Git Graphical Client

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates git-cola, an open source graphical client for Git written in Python with the Qt toolkit, against the Universal Cake Evaluation Metrics. It records the gate results, a scorecard, the findings that shaped the ratings, and a recommendation for the radar. git-cola passes all four default gates. It works directly on ordinary Git repositories, so there is nothing to export or migrate, and its French translation is complete. Its main privacy issue is a default: the commit view looks up author pictures on gravatar.com, which sends a hash of each author's email address to a third party until the user turns it off. Its main accessibility issue is a row of icon-only toolbar buttons that carry no accessible name and cannot be reached with the Tab key. The project is mature and active, but it has one dominant maintainer, and its Windows installer raises a licence question for anyone who redistributes it.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about git-cola come from files in the git-cola repository, read directly at the `v4.19.0` tag and at commit `6a516dd0` on the main branch on 2026-10-11; from the 4.19.0 wheel published on PyPI, installed with PyQt6 and run headless under `strace`; from an inspection of its main window's widgets; and from the project's download page, release page, and OpenSSF Best Practices record. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate git-cola 4.19.0, the current release, as a desktop Git client for a person who wants to stage, commit, browse history, and manage branches without the command line. Two uses are in view: Christopher's own and other developers' work, and possible use by Vishpala clients who maintain content in Git repositories. The PyPI wheel was installed into a virtual environment with PyQt6 and run on Linux with Qt's offscreen platform, so nothing was drawn on a real display. Network behaviour and labelled controls were tested this way. Visual presentation, keyboard use, and screen reader behaviour were not tested. The Windows installer and macOS were assessed from source and documentation.

git-cola is a front end to the `git` command; it does not include Git. Git's own behaviour, and that of any remote host a repository uses, is out of scope.

## Scorecard metadata

- Product or service name: git-cola
- Version evaluated: 4.19.0, `git_cola-4.19.0-py3-none-any.whl`, SHA-256 `a9cfb144ef6297762db666e8c2ff13921d537e5454ac93a63e665c50380c4338`; repository tag `v4.19.0`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-11
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, CPython 3.13.16, PyQt6 6.11.0 with Qt 6.11.2, Git 2.43, Qt offscreen platform

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry. The one outbound feature, Gravatar author pictures, is on by default and sends a hash of each viewed commit author's email address to gravatar.com. It is turned off with one preference or `git config --global cola.gravatar false`; with it off, no outbound connections were observed, see Findings |
| A licence incompatible with the project | Pass, for use | Verified, Inferred | GPL-2.0-only. Running it raises no issue. Its code cannot be combined into AGPL-3.0-or-later or GPL-3.0-or-later works, and redistributing the Windows installer, which bundles GPL-3.0-only PyQt6, needs licence review first, see Findings |
| Failure of the accessibility floor | Pass | Inferred | A Qt application, so it inherits Qt's support for platform accessibility interfaces, and it defines 126 keyboard shortcuts. But 15 of the 16 visible buttons in the main window are icon-only, with no text and no accessible name, and none of the 15 accepts keyboard focus. Not tested with a screen reader |
| No exit | Pass | Verified | git-cola works on ordinary Git repositories and keeps only its own settings, in `~/.config/git-cola/` and `cola.*` keys in Git configuration. Leaving means uninstalling it; the repositories are untouched |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | 126 keyboard shortcuts and a published hotkey reference (verified). In the main window, 15 icon-only buttons have tooltips but no accessible name and no keyboard focus, and the commit summary and description fields have placeholder text but no accessible name (verified by widget inspection). Whether a screen reader announces the tooltips or placeholders instead was not tested. Font size and themes, including custom style sheets, are configurable |
| Multilingual integration | Strong | Verified | 17 interface translations as gettext `.po` files. Against the 4.19.0 template of 995 strings, French and Japanese are 100% and 97% complete, Tamil 91%, Polish 86%; Italian and Swedish under 10%. French is `fr`, not a Canadian variety. New languages are added as files |
| Economic and cognitive accessibility | Moderate | Verified, Inferred | Free. The wheel is 1.2 MB, but a PyPI install with PyQt6 took 285 MB, almost all of it Qt; distribution packages share the system's Qt. The interface uses Git's own vocabulary (stage, amend, rebase), so it eases the mechanics of Git without hiding its concepts, which suits developers more than occasional users |
| Representation | Unknown | None | Not assessable from public information. No machine-learned models in the product |
| Compatibility | Strong | Verified, Claimed | Linux, Windows, macOS, and FreeBSD. Packaged by Debian, Ubuntu, Fedora, openSUSE, Arch, Gentoo, FreeBSD, Homebrew, and winget (claimed). Works with PyQt5, PyQt6, or PySide through QtPy. Needs Git 2.2 and Python 3.9 or later (claimed). PyQt6 wheels from PyPI need glibc 2.34 (verified), so older Linux systems should use their distribution's packages, which lag: Debian 12 ships 3.12, Debian 13 ships 4.12 |
| Resilience | Strong | Verified | Works fully offline. No accounts, services, or licence server. With Gravatar off it makes no network contact of its own |
| Agency, sovereignty and privacy | Moderate | Verified | GPL-2.0-only keeps modification and forking open. Holds no data of its own. The default Gravatar lookup reveals to a third party which commit authors a user views and when, from the user's IP address; SHA-256 hashes of common email addresses can be matched by guessing. Lens: the people exposed are commit authors, who did not choose the tool |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms, pricing, or account. Repositories stay plain Git. Exit cost is learning another client |
| Agency, interaction patterns | Moderate | Verified | No nags or dark patterns; the About dialog carries the licence notice. The Gravatar default is the issue: an outbound third-party lookup enabled before the user has been asked, with the setting described only by a tooltip in preferences |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | A desktop application that runs only when opened. It can watch the working tree for changes with inotify, which is cheap. The Qt footprint is large on disk but shared when installed from a distribution |
| Security | Moderate | Verified, Claimed | Tags are GPG-signed (verified locally as present; GitHub reports the signature verified, claimed). Releases are uploaded from the maintainer's machine with `twine`, and PyPI holds no provenance attestation for 4.19.0 (verified). The Windows release offers no checksums. The 4.19.0 Windows installer bundles PyQt6 6.9.1 and Python 3.12.10, older than current. OpenSSF Best Practices passing badge, last updated 2020 (claimed); it names public issues, not a private channel, for vulnerability reports. No security policy file. No CVEs found in the Debian security tracker. An experimental server mode on the main branch offers unauthenticated remote control, see Findings |
| Longevity | Strong | Verified | In development since November 2007. 815 non-merge commits in the two years to 2026-10-11 and 137 in the last 90 days, from 29 authors. Regular releases: 4.17.0 in December 2025, 4.18.0 in March 2026, 4.19.0 in July 2026 |
| Content endurance | Strong | Verified | Authors nothing in a format of its own; every change it makes is an ordinary Git commit or configuration value |
| Exit and portability | Strong | Verified | See the No exit gate. Any other Git client, or the command line, opens the same repositories |
| Adjustability and support | Strong | Verified | Copyleft keeps the fork-and-fix path open. Pure Python, runnable from its source tree without installation, with user-defined custom actions and style sheet themes. Support is GitHub issues; the project is active |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | One of the older open source Git clients, listed on git-scm.com and packaged widely. No take rate. Forkable by licence. Contributor concentration: 85% of all non-merge commits (7,445 of 8,772) and 78% over the last two years (638 of 815) are by David Aguilar (verified). Funding is personal sponsorship through GitHub Sponsors and Patreon (verified). No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry and exit Verified; licence Verified with a redistribution caveat; accessibility floor Inferred |

## Findings

### Network behaviour and the Gravatar default

We created a three-commit test repository, installed the 4.19.0 wheel from PyPI with PyQt6, and ran the history browser, `git-dag`, and the main window, `git-cola`, under `strace`, recording every `connect` call. Opening the history browser with default settings produced one outbound connection attempt, to `gravatar.com` on port 443, made through the test environment's proxy, which blocked it. Setting `cola.gravatar` to `false` in Git configuration and repeating the run produced no outbound connections. The main window, which shows no commit details until a commit is selected, made no connections either way.

The source explains the request. When a commit's details are shown, git-cola trims and lower-cases the author's email address, hashes it with SHA-256, and requests `https://gravatar.com/avatar/<hash>`, passing the address of a default picture hosted on git-cola's own site <a name="apa-cola-gravatar-citation"></a>([git-cola Project, n.d.-c](#apa-cola-gravatar-reference)). The preference defaults to enabled <a name="apa-cola-prefs-citation"></a>([git-cola Project, n.d.-d](#apa-cola-prefs-reference)), and the preferences dialog describes it in a tooltip. The hash is not the address itself, but hashes of predictable addresses can be matched by trying candidates, and each lookup also tells Gravatar's operator which authors a user is viewing, from which IP address, and when. For Vishpala use this setting should be turned off at installation.

### Experimental server mode on the main branch

The unreleased changelog announces a `git cola server` sub-command and a matching `git cola connect` <a name="apa-cola-changes-citation"></a>([git-cola Project, n.d.-h](#apa-cola-changes-reference)). The server accepts operations over WebSockets with no authentication, binds to `127.0.0.1` by default, and prints its own warning that it is experimental and insecure, that binding to `0.0.0.0` lets anyone on the network run commands, and that it should be tunnelled over SSH instead <a name="apa-cola-server-citation"></a>([git-cola Project, n.d.-e](#apa-cola-server-reference)). It needs two optional packages and must be started by hand. None of this is in 4.19.0, but it is the first feature that listens on the network, so the release that ships it is a re-review trigger.

### Licence

The repository's licence file is the GPL version 2, and the About dialog states that the program may be redistributed under version 2 as published by the Free Software Foundation, with no "or later" clause <a name="apa-cola-about-citation"></a>([git-cola Project, n.d.-b](#apa-cola-about-reference)). The package metadata agrees, declaring `GPL-2.0` <a name="apa-cola-pypi-citation"></a>([git-cola Project, 2026-a](#apa-cola-pypi-reference)). One file, the image viewer, is dual-licensed with MIT.

For running git-cola, none of this matters. Two cases need care. First, GPL-2.0-only code cannot be incorporated into an AGPL-3.0-or-later or GPL-3.0-or-later work. Second, the Windows installer bundles PyQt6 <a name="apa-cola-pynsist-citation"></a>([git-cola Project, n.d.-f](#apa-cola-pynsist-reference)), whose package metadata declares `GPL-3.0-only` <a name="apa-pyqt6-pypi-citation"></a>([Riverbank Computing, 2026](#apa-pyqt6-pypi-reference)). GPL-2.0-only and GPL-3.0-only terms are not generally compatible when combined in one distributed work. Whether a Riverbank exception or the separation between the two packages resolves this was not established here. It does not affect a user installing the program for themselves, but if Vishpala hands the Windows installer to clients, that is distribution and should go through licence review first. Running git-cola on PySide, which is LGPL-licensed, sidesteps the question.

### Accessibility evidence

We built the main window with Qt's offscreen platform and inspected its widgets. Of 16 visible buttons, 15 show only an icon. Each has a tooltip, such as "Refresh", "Diff Options", and "Toggle the paths filter", but no text and no accessible name, and all 15 are set to receive no keyboard focus. The three visible text inputs, including the commit summary and description, have no accessible name; the first two have placeholder text. Whether Qt passes tooltips or placeholders to a screen reader as names depends on the Qt version and platform and was not tested. The repository contains no explicit accessible names anywhere in its widget code.

Much of the application can be driven from the keyboard: the hotkey module defines 126 shortcuts, and the project publishes a hotkey reference. Commands reached only through those icon buttons would still need checking. A pass with Orca on Linux and NVDA on Windows is the condition for moving it to Trial.

### Localisation

Translations are gettext `.po` files in the repository; adding a language means adding a file, and the contributing guide asks translators to leave Git's own command jargon untranslated <a name="apa-cola-contributing-citation"></a>([git-cola Project, n.d.-a](#apa-cola-contributing-reference)). Counting translated entries against the 995-string template in 4.19.0, French covers every string, which makes it usable for French-speaking clients, though in European rather than Canadian French. Seven of the 17 languages are below two thirds complete.

### Supply chain and maintenance

Releases follow a recipe in the project's task file: `twine upload` of the wheel and source archive, and a signed tag <a name="apa-cola-garden-citation"></a>([git-cola Project, n.d.-g](#apa-cola-garden-reference)). CI runs tests but does not build or publish releases, so PyPI holds no provenance attestation, and the Windows zip on the release page has no published checksum <a name="apa-cola-release-citation"></a>([git-cola Project, 2026-b](#apa-cola-release-reference)). The signed tag lets a consumer build from verified source.

The OpenSSF Best Practices record shows a passing badge, last updated in March 2020, and its vulnerability-reporting evidence points to public GitHub issues <a name="apa-openssf-cola-citation"></a>([OpenSSF Best Practices, 2020](#apa-openssf-cola-reference)). The repository has no security policy file. The Debian security tracker lists no CVEs for the package <a name="apa-debian-cola-citation"></a>([Debian Security Tracker, n.d.](#apa-debian-cola-reference)).

The project is healthy by activity: 137 commits in the last 90 days and a release every three to four months. It is not by concentration: David Aguilar wrote 85% of all commits and 78% of those in the last two years. The repository sits under a `git-cola` organisation rather than a personal account, which eases a handover if one is ever needed.

### Fit with the OSAT managers and older systems

git-cola needs `git` on the path, and on Windows it asks the user to install Git for Windows separately. That makes it a natural consumer of a future osat-manager-git. It is pure Python, so it could also run on a manage-python runtime, but PyQt6 wheels from PyPI need glibc 2.34. On older Linux systems the distribution's PyQt5 packages, or the distribution's own git-cola package, are the practical route, at the cost of an older git-cola.

## Recommendation

Keep git-cola in Assess, and move it to Trial for own and developer use on these conditions:

- Turn Gravatar off on every installation with `git config --global cola.gravatar false`, or set it in preferences.
- Run a screen reader and keyboard pass with Orca and NVDA, covering the toolbar buttons and the commit message fields, and report the unlabelled controls upstream.
- Install from the distribution's packages, or from PyPI pinned by version and hash; on Windows, build from the signed tag or record the installer's checksum on first download.
- Do not redistribute the Windows installer to clients until the PyQt6 licence question has been reviewed.

For Vishpala clients who are not developers, Trial should wait for the accessibility pass; the interface assumes familiarity with Git's concepts.

Worth reporting upstream: the unlabelled, unfocusable toolbar buttons and unnamed text fields, the Gravatar default, and the absence of a private vulnerability-reporting channel and release checksums.

Re-review triggers specific to git-cola: the first release that includes the server mode, any change to the Gravatar default, a change of Qt binding in the Windows installer, and any change in maintainership.

## License

This document, *Universal Cake Evaluation: git-cola, Git Graphical Client*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### git-cola repository

- [Contributing guide](#apa-cola-contributing-reference)
- [About dialog and licence notice](#apa-cola-about-reference)
- [Gravatar lookup, gravatar.py](#apa-cola-gravatar-reference)
- [Preference defaults, prefs.py](#apa-cola-prefs-reference)
- [Server mode, server.py](#apa-cola-server-reference)
- [Windows installer configuration, pynsist.cfg](#apa-cola-pynsist-reference)
- [Task recipes, garden.yaml](#apa-cola-garden-reference)
- [Changelog, CHANGES.rst](#apa-cola-changes-reference)

### Releases and packages

- [git-cola 4.19.0 on PyPI](#apa-cola-pypi-reference)
- [git-cola 4.19.0 release](#apa-cola-release-reference)
- [PyQt6 6.11.0 on PyPI](#apa-pyqt6-pypi-reference)

### Security records

- [OpenSSF Best Practices, project 251](#apa-openssf-cola-reference)
- [Debian security tracker, git-cola](#apa-debian-cola-reference)

## References

<a name="apa-debian-cola-reference"></a>Debian Security Tracker. (n.d.). *Information on source package git-cola*. https://security-tracker.debian.org/tracker/source-package/git-cola
[Return to citation](#apa-debian-cola-citation)

<a name="apa-cola-contributing-reference"></a>git-cola Project. (n.d.-a). *CONTRIBUTING.md*. GitHub. https://github.com/git-cola/git-cola/blob/main/CONTRIBUTING.md
[Return to citation](#apa-cola-contributing-citation)

<a name="apa-cola-about-reference"></a>git-cola Project. (n.d.-b). *about.py* [Source code]. GitHub. https://github.com/git-cola/git-cola/blob/main/cola/widgets/about.py
[Return to citation](#apa-cola-about-citation)

<a name="apa-cola-gravatar-reference"></a>git-cola Project. (n.d.-c). *gravatar.py* [Source code]. GitHub. https://github.com/git-cola/git-cola/blob/main/cola/gravatar.py
[Return to citation](#apa-cola-gravatar-citation)

<a name="apa-cola-prefs-reference"></a>git-cola Project. (n.d.-d). *prefs.py* [Source code]. GitHub. https://github.com/git-cola/git-cola/blob/main/cola/models/prefs.py
[Return to citation](#apa-cola-prefs-citation)

<a name="apa-cola-server-reference"></a>git-cola Project. (n.d.-e). *server.py* [Source code]. GitHub. https://github.com/git-cola/git-cola/blob/main/cola/server.py
[Return to citation](#apa-cola-server-citation)

<a name="apa-cola-pynsist-reference"></a>git-cola Project. (n.d.-f). *pynsist.cfg* [Build configuration]. GitHub. https://github.com/git-cola/git-cola/blob/main/pynsist.cfg
[Return to citation](#apa-cola-pynsist-citation)

<a name="apa-cola-garden-reference"></a>git-cola Project. (n.d.-g). *garden.yaml* [Build script]. GitHub. https://github.com/git-cola/git-cola/blob/main/garden.yaml
[Return to citation](#apa-cola-garden-citation)

<a name="apa-cola-changes-reference"></a>git-cola Project. (n.d.-h). *CHANGES.rst*. GitHub. https://github.com/git-cola/git-cola/blob/main/CHANGES.rst
[Return to citation](#apa-cola-changes-citation)

<a name="apa-cola-pypi-reference"></a>git-cola Project. (2026-a). *git-cola 4.19.0* [Software release]. PyPI. https://pypi.org/project/git-cola/4.19.0/
[Return to citation](#apa-cola-pypi-citation)

<a name="apa-cola-release-reference"></a>git-cola Project. (2026-b). *Git Cola v4.19.0* [Software release]. GitHub. https://github.com/git-cola/git-cola/releases/tag/v4.19.0
[Return to citation](#apa-cola-release-citation)

<a name="apa-openssf-cola-reference"></a>OpenSSF Best Practices. (2020). *git-cola* (Project 251). https://www.bestpractices.dev/en/projects/251
[Return to citation](#apa-openssf-cola-citation)

<a name="apa-pyqt6-pypi-reference"></a>Riverbank Computing. (2026). *PyQt6 6.11.0* [Software release]. PyPI. https://pypi.org/project/PyQt6/6.11.0/
[Return to citation](#apa-pyqt6-pypi-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of git-cola 4.19.0 against Universal Cake Evaluation Metrics 0.5.1, scoped to use as a desktop Git client, with the PyPI release run headless on Linux under network tracing and its main window inspected for labelled controls |

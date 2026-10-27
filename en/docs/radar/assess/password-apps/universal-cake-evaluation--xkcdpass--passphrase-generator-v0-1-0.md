---
dc:title: "Universal Cake Evaluation: xkcdpass, Passphrase Generator"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "xkcdpass"
  - "python"
  - "passphrases"
  - "diceware"
dc:description: "Universal Cake evaluation of xkcdpass 1.30.0, the Python command-line tool and library for generating random multi-word passphrases, as a passphrase generator and as an independent copy of the EFF long wordlist."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-22"
dcterms:modified: "2026-10-22"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/redacted/XKCD-password-generator"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, generating-a-strong-passphrase"
dc:identifier: "universal-cake-evaluation--xkcdpass--passphrase-generator"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-22"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of xkcdpass 1.30.0 against Universal
      Cake Evaluation Metrics 0.5.1, scoped to two uses: generating
      passphrases on a trusted machine, and serving as an independent copy
      of the EFF long wordlist for the cross-check in the uc-processes
      passphrase guide. Network and file system behaviour verified on
      Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for both scoped uses, once the Inferred accessibility gate is verified
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. No runtime dependencies, no network contact, no stored data, BSD-3-Clause code, words chosen with random.SystemRandom. Its bundled copy of the EFF long list is unchanged in git since 2017, which suits it to the wordlist cross-check. Risks: a single maintainer with low activity, no security policy, no CI, unsigned tags and no PyPI provenance attestations; a missing --wordfile silently falls back to the default list (open since 2022); default length filters trim the EFF list to 7,227 words; and the package's BSD-3-Clause licence metadata does not cover bundled wordlists under GPL-3.0, CC BY-SA 3.0 and Apache-2.0."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-22T22:40:00-04:00"
  human: []
---

# Universal Cake Evaluation: xkcdpass, Passphrase Generator

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates xkcdpass, a Python command-line tool and library that generates passphrases from random words, against the Universal Cake Evaluation Metrics. It records the gate results, a scorecard, the findings that shaped the ratings, and a recommendation for the radar. xkcdpass passes all four default gates. Its strengths are structural: permissive licence, no runtime dependencies, no network contact, no stored data, a cryptographically secure random source for word choice, and a small, readable codebase that anyone could fork. Its risks sit in the project around it and in a few sharp edges of the tool: one maintainer with little recent activity, no security policy or automated testing, unverifiable releases, a silent fallback when a requested wordlist is missing, and licence metadata that does not describe all of the bundled wordlists.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about xkcdpass come from the xkcdpass repository, read directly at commit `b9813612` and at the `xkcdpass-1.30.0` tag on 2026-10-22; from the 1.30.0 wheel and release metadata published on PyPI; from the project's GitHub issue tracker; and from a network and file system trace of the installed wheel. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate xkcdpass 1.30.0, the current release on PyPI, installed from its wheel, for two uses that arose while writing the uc-processes guide *Generating a strong passphrase*:

- **As a passphrase generator**, run by a person on a trusted machine to make a passphrase for something such as an SSH signing key.
- **As an independent copy of the EFF long wordlist**, downloaded but not installed, so that the words in a copy fetched from eff.org can be compared against a copy that reached the user through a different channel.

The second use relies only on the wordlist inside the package, not on running the tool. Network and file system behaviour were verified on Linux only. Windows and macOS were assessed from source and documentation.

## Scorecard metadata

- Product or service name: xkcdpass
- Version evaluated: 1.30.0, `xkcdpass-1.30.0-py3-none-any.whl`, SHA-256 `3653a4a1e13de230808bcaf11f8c04207a5d3df8e2f7e1de698e11c262b5b797`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-22
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, CPython 3.13.17, strace 6.8

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry exists to disable. A trace of every network system call while generating passphrases recorded none, see Findings. The source has no network code |
| A licence incompatible with the project | Pass | Verified | The code is BSD-3-Clause, read from the repository, and compatible with AGPL-3.0-or-later and GPL-3.0-or-later projects. Bundled wordlists carry other licences, see Findings; this matters only if xkcdpass or its wordlists are redistributed. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | A command-line tool that prints plain text, with no colour, layout or interactive screen. Not tested with a screen reader |
| No exit | Pass | Verified | xkcdpass stores nothing: the trace showed no file writes. A generated passphrase is plain text the user keeps elsewhere. Replacing the tool takes a few lines of Python |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | Plain-text output suits screen readers and braille displays (inferred, not tested). Interaction is a single command, with an optional prompt-driven mode. Documentation is the GitHub README and a manual page that the project's own tracker reports as outdated (verified) |
| Multilingual integration | Strong | Verified | Ships 18 wordlists covering English, French, German, Finnish, Italian, Norwegian Bokmål, Portuguese, Brazilian Portuguese, Spanish and Swedish. Adding a language is a data file anyone can contribute, and the README invites it. The interface and help text are English only |
| Economic and cognitive accessibility | Moderate | Verified | Free. Simple for a technical user; requires a terminal and Python, so not suitable on its own for non-technical people. The wheel is 2.7 MB and 9.2 MB installed, almost all of it wordlists, where the code is 18.6 KB and the default list 62 KB |
| Representation | Unknown | None | Not assessable from public information. Contributions come from a small group of volunteers. No machine-learned models are part of the product, so the v0.5.0 training-data proxies do not apply |
| Compatibility | Strong | Verified, Claimed | Pure Python, declared for CPython 3.6 and later and any operating system (claimed). Ran and passed its 9 tests on CPython 3.13 (verified). Debian unstable packages an older release, 1.19.3, repackaged, per Debian's package tracker (claimed, not inspected) |
| Resilience | Strong | Verified | Works fully offline. No services, accounts, licence server or tokens |
| Agency, sovereignty and privacy | Strong | Verified | BSD-3-Clause allows self-hosting, modification, forking and redistribution without permission. Stores no data and makes no network contact. The passphrase is printed only to the terminal |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms of service and no pricing. Data portability is not applicable, since xkcdpass holds no data. Exit cost is minutes: the same method is a few lines with Python's `secrets` module |
| Agency, interaction patterns | Moderate | Verified | No dark patterns, prompts or defaults that serve the vendor. Two weak spots: a missing `--wordfile` silently falls back to the default list, so the user cannot predict what the command will do (open issue since 2022); and the `--interactive` mode offers "Accept? [yN]" after each passphrase, which invites rerolling until a passphrase looks pleasing |
| Environment, direct, indirect, and authoring-side | Strong | Verified, Inferred | Runs once per use and exits, in 0.06 seconds here, with no daemon. Runs on old and low-end hardware. The 2.7 MB download is large for what it does, which matters on slow or metered connections. Authoring-side load is not applicable |
| Security | Moderate | Verified | Words and random delimiters use `random.SystemRandom`, the operating system's secure source, and falling back to an insecure generator requires an explicit flag (verified). No runtime dependencies. Weaknesses: no security policy, no CI, unsigned release tags, no PyPI provenance attestations; `--case random` uses the non-cryptographic `random` module; default length filters reduce the EFF list to 7,227 words. No vulnerabilities found in a web search; the OSV database could not be queried from the test environment |
| Longevity | Moderate | Verified | In development since 2011, with 47 releases on PyPI. Activity is low: 13 commits in the two years to 2026-10-22, with releases in March 2025 and January 2026. Volunteer-run, with no stated funding |
| Content endurance | Strong | Inferred | xkcdpass stores and authors nothing. Removing it leaves every passphrase and stored secret untouched |
| Exit and portability | Strong | Verified | See the No exit gate. The wordlists are plain text files, one word per line |
| Adjustability and support | Moderate | Verified | One 567-line module, readable and BSD-licensed, so the fork-and-fix path is short. Support is the GitHub issue tracker, with 8 open issues, the oldest from 2020; contributed pull requests are merged in occasional batches |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | A well-known tool in a small niche, with about 1,400 stars and 192 forks on GitHub (claimed by GitHub's page). Many alternatives, including KeePassXC's passphrase generator and the standard library. No take rate. Command-line options have been stable. Highly forkable. Contributor concentration: 58% of all non-merge commits and 7 of the last 13 are by the maintainer, who alone tags releases from a personal account. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### Network and file system behaviour

We installed the 1.30.0 wheel from PyPI into a virtual environment and generated passphrases while tracing the process with `strace`, recording `connect`, `sendto`, `sendmsg` and `bind`, and separately every file open, creation, rename, deletion and directory creation. The network trace recorded no calls at all. The file system trace showed only reads, of Python's own files and the wordlist; there were no writes. Source inspection agreed: the module imports nothing that reaches the network <a name="apa-xkcdpass-source-citation"></a>([xkcdpass Project, n.d.-b](#apa-xkcdpass-source-reference)).

### Randomness

Word choice, random delimiters and acrostic words all draw from `random.SystemRandom`, which uses the operating system's cryptographically secure source. If that is unavailable, the tool stops unless the user sets `XKCDPASS_ALLOW_WEAKRNG` or passes `--allow-weak-rng`, so a weak generator is never used silently ([xkcdpass Project, n.d.-b](#apa-xkcdpass-source-reference)). The README describes the same behaviour <a name="apa-xkcdpass-readme-citation"></a>([xkcdpass Project, n.d.-a](#apa-xkcdpass-readme-reference)).

One exception: `--case random`, which capitalises some words, uses the module-level `random` generator, which is not cryptographically secure. The tool's own strength report ignores capitalisation, so it does not overstate strength, but anyone counting that option as extra strength should not.

### Default word filtering and reported strength

By default, xkcdpass keeps only words of 5 to 9 characters. Applied to the EFF long list, that leaves 7,227 of its 7,776 words, about 12.82 bits per word instead of 12.92. With `-V`, the tool reports this itself: 7 words give about 89 bits with the defaults, and about 90 with `--min 1 --max 20`, which keeps the whole list. The difference is small, but it means an xkcdpass passphrase is not exactly a Diceware passphrase from the EFF list, and dice numbers from the EFF file cannot be mapped onto it. The README asks for at least six words with this list ([xkcdpass Project, n.d.-a](#apa-xkcdpass-readme-reference)). A separate open issue reports that the strength calculation is wrong for acrostic passphrases <a name="apa-xkcdpass-issue-127-citation"></a>([xkcdpass Project, 2020](#apa-xkcdpass-issue-127-reference)).

### Silent fallback for a missing wordlist

If `--wordfile` names a file that does not exist, xkcdpass does not report an error. It falls back to its default list, and beyond that to system dictionaries such as `/usr/share/dict/words`, and generates a passphrase anyway. We confirmed this by passing a path that did not exist: the command exited with status 0 and used the default list. The behaviour has been an open issue since 2022 <a name="apa-xkcdpass-issue-147-citation"></a>([xkcdpass Project, 2022](#apa-xkcdpass-issue-147-reference)). A typing mistake in the path therefore changes the wordlist, and so the strength, without any warning. Running with `-V`, which prints the size of the list actually used, catches it.

### The bundled EFF list as a cross-check source

The package's `eff-long` file holds the 7,776 words of the EFF long list, one per line, without dice numbers. Its git history shows a single commit, from 2017-05-01, when it was added and made the default, so it has not changed since. The copy in the 1.30.0 wheel is identical to the one at the release tag. Sorted and hashed, its words give SHA-256 `6d557f0693958fb5e650b68b5bee585eb82cf4da32965505c789e924743bc522`, the value the uc-processes passphrase guide uses for its cross-check.

Because the copy arrives through PyPI rather than eff.org, matching words in the two copies show that neither has been altered without the other. The match cannot rule out an error introduced before 2017 that both share, which is unlikely given how widely the EFF list is used.

### Licence and dependencies

The code is licensed BSD-3-Clause <a name="apa-xkcdpass-license-citation"></a>([xkcdpass Project, n.d.-c](#apa-xkcdpass-license-reference)), and the wheel declares no runtime dependencies. The bundled wordlists, however, come from many sources, and the README records their terms: the EFF lists under CC BY 3.0 US, an Italian list derived from Wikipedia under CC BY-SA 3.0, a German list based on a GPL-3.0 list, a Finnish list under LGPL, EUPL 1.1 and CC BY 3.0, a Brazilian Portuguese list under the Apache License, and others in the public domain or under BSD or MIT terms ([xkcdpass Project, n.d.-a](#apa-xkcdpass-readme-reference)). The wheel's metadata nevertheless declares only `BSD-3-Clause` and includes only the BSD licence file, leaving out the Apache licence file that the repository keeps for the Brazilian Portuguese list.

For running the tool, or for using its EFF list as a cross-check, none of this matters. For redistributing xkcdpass or copying its wordlists into another project, it does: each wordlist's own licence applies, and some require attribution or share-alike terms. Debian ships a repackaged version, 1.19.3+dfsg1-3, where the `+dfsg` suffix marks a source modified to meet Debian's free software guidelines <a name="apa-debian-tracker-citation"></a>([Debian Package Tracker, n.d.](#apa-debian-tracker-reference)); which files were removed was not checked.

### Security and supply chain

The repository has no security policy and no CI workflows. The 1.30.0 release tag is an unsigned annotated tag, and a test fix landed after it, on 2026-01-24. PyPI returns no provenance attestation for either 1.30.0 file; the source archive was uploaded on 2026-01-11 and the wheel on 2026-01-12 <a name="apa-xkcdpass-pypi-citation"></a>([Tobin, 2026](#apa-xkcdpass-pypi-reference)). A consumer therefore cannot confirm that a file on PyPI matches the repository, except by comparing contents, as we did for the EFF list. Pinning by hash, for example with `pip install --require-hashes`, protects against a later substitution but not against a compromised first upload.

The attack surface is small: pure Python, no dependencies, no network, no writes, and a single module that can be read in full in under an hour.

### Maintenance and documentation

Of 330 non-merge commits since 2011, 192 (58%) are by the maintainer, Steven Tobin, with the rest from 51 other people. In the two years to 2026-10-22 there were 13 commits, 7 of them his. Contributed pull requests are merged in batches, most recently in January 2026, when the project moved its build to `pyproject.toml` and released 1.21.0 and 1.30.0 on the same day. Open issues include an outdated manual page <a name="apa-xkcdpass-issue-176-citation"></a>([xkcdpass Project, 2026](#apa-xkcdpass-issue-176-reference)) and outdated bash completion. The project is stable rather than active: what it does has not needed to change, but fixes such as the wordlist fallback wait years.

## Recommendation

Keep xkcdpass in Assess for now, and move it to Trial for both scoped uses once the accessibility floor is verified with a screen reader, as the Trial ring requires.

For the **wordlist cross-check** in the uc-processes passphrase guide, xkcdpass is fit for purpose as it stands: the file it supplies is unchanged since 2017, the guide only downloads the package and never installs or runs it, and the comparison does not depend on trusting PyPI alone.

For **generating passphrases**, it is a reasonable choice for technical users, on these conditions:

- Pin it by version and hash, `3653a4a1e13de230808bcaf11f8c04207a5d3df8e2f7e1de698e11c262b5b797` for the 1.30.0 wheel, since PyPI holds no provenance attestations.
- Always run with `-V` and check the reported word count, which catches a mistyped `--wordfile` path.
- Ask for 7 words with `-n 7`, and add `--min 1 --max 20` to use the whole EFF list.
- Avoid `--interactive` for anything that matters, and do not count `--case random` toward strength.

For the passphrase guide's own generator, the standard library's `secrets` module remains the simpler choice, since it adds no package to trust.

Re-review triggers specific to xkcdpass: a release that changes the default wordlist, the default length filters or the random source; a fix for the wordlist fallback; and any change in maintainership or repository ownership.

## License

This document, *Universal Cake Evaluation: xkcdpass, Passphrase Generator*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### xkcdpass repository

- [README](#apa-xkcdpass-readme-reference)
- [Source code](#apa-xkcdpass-source-reference)
- [Licence](#apa-xkcdpass-license-reference)
- [Issue 127, acrostic strength calculation](#apa-xkcdpass-issue-127-reference)
- [Issue 147, missing wordlist fallback](#apa-xkcdpass-issue-147-reference)
- [Issue 176, outdated manual page](#apa-xkcdpass-issue-176-reference)

### Releases and packages

- [xkcdpass 1.30.0 on PyPI](#apa-xkcdpass-pypi-reference)
- [xkcdpass in Debian](#apa-debian-tracker-reference)

## References

<a name="apa-debian-tracker-reference"></a>Debian Package Tracker. (n.d.). *xkcdpass*. Retrieved October 22, 2026, from https://tracker.debian.org/pkg/xkcdpass
[Return to citation](#apa-debian-tracker-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

<a name="apa-xkcdpass-pypi-reference"></a>Tobin, S. (2026). *xkcdpass* (Version 1.30.0) [Computer software]. PyPI. https://pypi.org/project/xkcdpass/1.30.0/
[Return to citation](#apa-xkcdpass-pypi-citation)

<a name="apa-xkcdpass-issue-127-reference"></a>xkcdpass Project. (2020, November 16). *Entropy calculation with acrostic is wrong* (Issue No. 127) [GitHub issue]. GitHub. https://github.com/redacted/XKCD-password-generator/issues/127
[Return to citation](#apa-xkcdpass-issue-127-citation)

<a name="apa-xkcdpass-issue-147-reference"></a>xkcdpass Project. (2022, April 25). *Option '--wordlist' help text fails to mention the wordfile is ignored if missing* (Issue No. 147) [GitHub issue]. GitHub. https://github.com/redacted/XKCD-password-generator/issues/147
[Return to citation](#apa-xkcdpass-issue-147-citation)

<a name="apa-xkcdpass-issue-176-reference"></a>xkcdpass Project. (2026, February 18). *Manual page outdated for current options* (Issue No. 176) [GitHub issue]. GitHub. https://github.com/redacted/XKCD-password-generator/issues/176
[Return to citation](#apa-xkcdpass-issue-176-citation)

<a name="apa-xkcdpass-readme-reference"></a>xkcdpass Project. (n.d.-a). *README.rst*. GitHub. https://github.com/redacted/XKCD-password-generator/blob/master/README.rst
[Return to citation](#apa-xkcdpass-readme-citation)

<a name="apa-xkcdpass-source-reference"></a>xkcdpass Project. (n.d.-b). *xkcd_password.py* [Source code]. GitHub. https://github.com/redacted/XKCD-password-generator/blob/master/xkcdpass/xkcd_password.py
[Return to citation](#apa-xkcdpass-source-citation)

<a name="apa-xkcdpass-license-reference"></a>xkcdpass Project. (n.d.-c). *LICENSE.BSD* [Software licence]. GitHub. https://github.com/redacted/XKCD-password-generator/blob/master/LICENSE.BSD
[Return to citation](#apa-xkcdpass-license-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of xkcdpass 1.30.0 against Universal Cake Evaluation Metrics 0.5.1, scoped to passphrase generation and to use as an independent copy of the EFF long wordlist, with network and file system behaviour verified on Linux |

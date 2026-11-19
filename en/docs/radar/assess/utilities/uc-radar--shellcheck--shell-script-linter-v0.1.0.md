---
dcterms:title: "uc-radar--shellcheck--shell-script-linter"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude Opus 5.5 (Anthropic)"
dcterms:subject:
  - "radar"
  - "shellcheck"
  - "shell"
  - "linter"
  - "static-analysis"
  - "dev-toolchain"
dcterms:description: "Assess entry for ShellCheck, the static analysis tool for sh and bash scripts, scored against Universal Cake Evaluation Metrics 0.6.0."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-11-15"
dcterms:modified: "2026-11-15"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: "https://github.com/koalaman/shellcheck"
dcterms:relation: "uc-radar--evaluation-lifecycle, universal-cake-evaluation-metrics, uc-radar-entry-template"
dcterms:identifier: "uc-radar--shellcheck--shell-script-linter"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-15"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial entry, created from uc-radar-entry-template v0.3.0. Light pass
      against metrics 0.6.0 on documentation and source evidence, with
      contributor figures measured from the upstream git history.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "adopt" # once every gate and the Security section are Verified, or a named person records an override
      reasoning: "Light pass against metrics 0.6.0. Telemetry gate passes: the command-line tool makes no network connections; only the optional shellcheck.net website receives script content. Licence gate passes, GPL-3.0-or-later, used as a tool and not linked into any project. Exit gate passes, ShellCheck holds no content; its directives are plain shell comments. Accessibility floor Unknown, pending a terminal screen reader pass; the default tty output draws carets under code, but the one-line gcc format is likely to pass. Security Strong: it reads scripts and never runs them. Main concern is maintainer concentration. Trial not recommended: risk is low and testing on a disposable machine can answer the open questions."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-15T21:20:00-05:00"
  human: []
---

# uc-radar--shellcheck--shell-script-linter-v0.1.0

- Product or service name: ShellCheck, the `shellcheck` command
- Version evaluated: 0.11.0, released 2025-08-03, the latest release, at documentation level, with the upstream source read at commit `1046c00` (2026-11-08); no build tested yet
- Evaluator: Claude Opus 5.5 (Anthropic), for Christopher Steel's review
- Date: 2026-11-15
- Radar ring: Assess

## What it is

ShellCheck is an open source static analysis tool for sh, bash, dash and ksh scripts, written in Haskell. It reports syntax errors, quoting mistakes, and common pitfalls, each with a stable code such as `SC2086` that links to an explanation page on the project wiki.

## Why interesting

Shell scripts carry much of the automation behind Vishpala and Universal Cake: install and provisioning steps around the Ventoy work and `ventoy-raw-node`, release helpers, Ansible `shell` tasks, and small glue scripts across `uc-automa` and the SAT tooling. They are also where quiet failures hide, such as an unquoted variable that breaks on a path with a space or deletes the wrong files. ShellCheck catches that class of error before the script runs.

It fits the Universal Cake approach well. It runs offline, needs no account, never executes the script, and works the same on a developer laptop, an old machine, or in CI. Its explanations are written for people who are not shell experts, which makes it a teaching tool as much as a gate.

## Concerns

Maintainer concentration. Vidar Holen wrote 1,285 of 1,926 non-merge commits (67%) across the project's history, and makes the releases. Other contributors are active: in the last 24 months there were 39 authors, and the top contributor in that window was Eisuke Kawashima with 49 commits to Holen's 47. Releases come roughly every 10 months to 1.5 years, so the project is steady rather than fast.

Default output is hard to listen to. The default `tty` format prints the source line and draws `^--` carets under it. That is useful visually but noisy for a screen reader. The `gcc` format gives one line per finding, `file:line:column: level: message [SC####]`, and is the likely accessible default.

Version drift. New releases add checks, so the same script can pass on one version and fail on the next. Debian 13 ships 0.10.0, with 0.11.0 in backports. The project itself recommends pinning a version to avoid surprise build breaks. Any CI or pre-commit use should pin the version.

The website is a different product. Pasting a script into shellcheck.net sends it to a server. Scripts can hold hostnames, paths, or secrets. The command-line tool does not do this.

Editor plugins are third-party. Integrations for VS Code and other editors are maintained outside the project, and some download a binary themselves. Each would need its own check.

## Gates

The Assess light pass requires all four default gates and the Security section on available evidence. None is Verified yet.

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The command-line tool has no network code; the only URLs in its entry point are printed text. shellcheck.net is a separate, optional website that receives pasted scripts. To Verify with the uc-radar telemetry trace procedure |
| A licence incompatible with the project | Pass | Inferred | GPL-3.0-or-later, read from the source header. ShellCheck is run as a tool, not linked or distributed with project code, so it places no terms on the scripts it checks. To Verify against the adopting project's licence policy |
| Failure of the accessibility floor | Unknown | Inferred, not observed | Text interface throughout. Not yet done: a terminal screen reader pass, for example with Emacspeak or Orca, over the `tty` and `gcc` formats, `--help`, and following an `SC` code to its wiki page |
| No exit, data or content cannot be extracted in usable form | Pass | Inferred | ShellCheck stores no content. Its only footprint in a project is optional `# shellcheck disable=...` comments and a `.shellcheckrc` file, both plain text that other tools ignore |

## Evaluation

Ratings use Strong, Moderate, Weak, Unknown, and, narrowly, Not applicable, each tagged Verified, Inferred, or Claimed. No hands-on testing has been done for this entry. The contributor figures were measured from the upstream git history on 2026-11-15 and are tagged Verified.

### Inclusive

#### Accessibility

**Alternative methods of interacting with content** **[GATE]**

- Rating: Unknown
- Evidence: Inferred
- Notes: Lens user. No graphical interface of its own. The main reading surfaces are the terminal output and the wiki page for each `SC` code, which is a GitHub wiki and inherits GitHub's accessibility, not audited here.

**Text and terminal interfaces** **[GATE]**

- Rating: Moderate, pending a screen reader pass
- Evidence: Inferred
- Notes: Lens user, owner. Plain text output in reading order: yes with `-f gcc`, one finding per line, level written as a word; the default `tty` format draws carets under code. Colour: `--color=never` turns it off, and `--color=auto`, the default, already turns it off when `TERM` is `dumb` or unset; ShellCheck does not read `NO_COLOR`, and since 0.8.0 the warning level is printed as text as well as colour, so meaning is not carried by colour alone. Defaults can be set once with the `SHELLCHECK_OPTS` environment variable, for example `SHELLCHECK_OPTS='-f gcc --color=never'`. Required steps depending on a full-screen interface, mouse, or graphical installer: none. Error messages: short and specific, each with a code to look up. Machine-readable formats (`json1`, `checkstyle`, `diff`) let other tools present results in whatever form suits the reader.

**Multilingual integration**

- Rating: Weak
- Evidence: Inferred
- Notes: Lens user, community. Messages and wiki pages are English only, and the messages are compiled into the binary, so adding a language would be a code change, not a data file anyone can supply.

**Affordability**

- Rating: Strong
- Evidence: Inferred
- Notes: Free, no paid tier, no account needed.

**Hardware and connectivity accessibility**

- Rating: Strong
- Evidence: Inferred
- Notes: Packaged in Debian, Ubuntu, Fedora, EPEL, Arch, the BSDs, Homebrew, conda-forge and others. Statically linked binaries are published for Linux and also offered for macOS and Windows; conda-forge packages are a few megabytes. Works fully offline after install. Runs in well under a second on typical scripts, so old hardware is not excluded.

**Cognitive accessibility**

- Rating: Strong
- Evidence: Inferred
- Notes: One command, one argument. Each finding names the problem in plain words and links to a page explaining why it matters and how to fix it, with examples of problem and fix. Messages describe the code, not the person.

**Time-based media**

- Rating: Not applicable
- Evidence: Inferred
- Notes: The project publishes no video or audio as part of its documentation.

#### Representation

- Rating: Unknown
- Evidence: None
- Notes: Lens community. Not assessed. The project is run by one maintainer, with no published governance; who takes part in decisions beyond that is not visible. Its absence from the reviewed documentation is itself a data point.

#### Resilience

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. When out of date: a self-contained binary with no licence server, account, or expiring token; an old version keeps working and only misses newer checks. Power outages: adds no infrastructure. Network outages: works fully offline; only the wiki explanations need a connection, and the short message is printed locally.

### Agency

#### Sovereignty & Privacy

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. The owner can build, modify, fork and redistribute without permission. Scripts stay on the user's machine. ShellCheck reads `.shellcheckrc` or `shellcheckrc` from the script's directory and each parent directory, and `.editorconfig`, unless `--norc` is given. Reversibility test: if the project disappeared tomorrow, installed binaries and distribution packages would keep working, and the source could be built by anyone with GHC.

**Power-imbalance proxies**

- Exit cost: Low, minutes. Removing it costs only the checking it provided. Inferred.
- Data portability: Not applicable. ShellCheck holds no user data.
- Terms-of-service volatility: Not applicable. GPL-3.0-or-later, with no terms of service for the tool. The shellcheck.net website's terms were not reviewed.
- Pricing asymmetry: Strong. Free for every use.

**AI-specific proxies**

- Not applicable. ShellCheck contains no machine-learned model; its checks are hand-written rules.

#### Interaction Patterns

- Rating: Strong
- Evidence: Inferred
- Notes: Lens user. No sign-in, prompts, update nags, or upsells. Findings can be silenced per line, per file, or per project with a written directive, which keeps the decision visible to the next reader.

### Environment

- Rating: Strong
- Evidence: Inferred
- Notes: Lens planet. Direct: runs for a moment on demand, no daemon, no background activity. Indirect: one small download per version. Authoring-side: catching errors before a script runs on a server saves repeated failed runs and rebuilds.

### Security

- Rating: Strong, pending verification
- Evidence: Inferred
- Notes: Lens owner, user. ShellCheck parses scripts and never runs them. With `-x` or `external-sources=true` it also reads files named in `source` statements, still without running them. Attack surface: no listening service, no network code, no writes. Supply chain: distribution packages are signed by their archives; the upstream static binaries are not published with checksums or signatures that were found, so the distribution package is the safer source. No dedicated security policy file was found in the repository. One behaviour to note: a `.shellcheckrc` in a parent directory can disable checks, so a repository can quietly silence warnings for anyone who checks it out; `--norc` in CI removes that.

**Assessment method:**
Not yet tested. Planned: uc-radar telemetry trace procedure (version to be named) on a disposable Linux Mint machine, covering `shellcheck --version`, checking a sample script in each output format, and checking with `-x` against a script that sources another. File system check with `strace -e trace=file`. Terminal screen reader pass with Emacspeak or Orca.

**Network behaviour:**
- [ ] Outbound connections during normal use (expected: none)
- [ ] Update checks, silent or explicit (expected: none)
- [ ] Telemetry or usage data, present or absent (expected: absent)
- [ ] Licence server contact, frequency and data sent (expected: none)
- [ ] Affects networking or items created that will be transferred over a network (expected: no)
- [ ] Helps ensure that documents passed via network are clean and compliant (expected: yes, scripts are checked before they are shipped to servers)

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content (expected: none)
- [ ] Caches content outside the working directory (expected: none)
- [ ] Stores recent file history outside the archive (expected: none)
- [ ] Writes to unexpected locations (expected: none; reads `.shellcheckrc` and `.editorconfig` up the directory tree, and `~/.shellcheckrc` or the XDG equivalent)

**Content exposure:**
- [ ] Sends any content to a remote service (expected: none from the command-line tool; shellcheck.net receives anything pasted into it)
- [ ] Stores content in a cloud service by default (expected: no)
- [ ] Auto-save or backup features that copy content externally (expected: no)

### The Product or Service Itself

**Longevity**

- Rating: Strong
- Evidence: Verified for activity, Inferred otherwise
- Notes: Lens owner, community. First commit 2012-11-03; 2,345 commits to date; latest commit 2026-11-08. Releases have followed for 13 years, the latest 0.11.0 on 2025-08-03 <a id="cite-holen-2025"></a>([Holen, 2025](#ref-holen-2025)). Widely depended on: packaged in every major distribution and required by many other packages' build and test steps.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: An enhancement layer, not a container. Scripts are plain files and remain whole with or without ShellCheck.

**Exit and portability** **[GATE]**

- Rating: Strong
- Evidence: Inferred
- Notes: See the exit gate. Its output formats include checkstyle and JSON, so results can feed other tools. Successors exist in part, for example language servers that wrap ShellCheck, but none replaces it outright.

**Adjustability and support**

- Rating: Moderate
- Evidence: Inferred
- Notes: The GPL keeps the fork-and-fix path open, but changes need Haskell, a smaller pool of contributors than most languages. Each check can be enabled, disabled, or tuned per line, file, or project. Support is community-based, through GitHub issues and the wiki; no paid support.

**Market Position**

- Rating: Moderate
- Evidence: Verified for contributor figures, Inferred otherwise
- Notes: Lens community, owner. Market concentration: the de facto standard shell linter, with no close equivalent, so it is a single point of reliance for this kind of check. Take rate: none. API stability: the command-line flags and `SC` codes are stable across releases; new checks arrive with each version. Forkability: GPL-3.0-or-later, forkable, though not yet tested in practice. Contributor concentration: one maintainer wrote 67% of non-merge commits across the history, 199 authors in total, and 39 in the last 24 months.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Unknown, text interface Moderate | Inferred | `gcc` format one finding per line; screen reader pass pending |
| Multilingual integration | Weak | Inferred | English only, compiled into the binary |
| Economic and cognitive accessibility | Strong | Inferred | Free; plain explanations per code |
| Representation | Unknown | None | Single maintainer, no published governance |
| Compatibility | Strong | Inferred | Linux, macOS, Windows, BSDs; static binaries |
| Resilience | Strong | Inferred | Offline, self-contained |
| Agency, sovereignty & privacy | Strong | Inferred | No network code in the CLI |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | Exit cost minutes; portability and ToS not applicable |
| Agency, interaction patterns | Strong | Inferred | No prompts or nags; visible, written suppressions |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | Runs on demand, prevents failed runs |
| Security | Strong | Inferred | Never executes scripts; upstream binaries unsigned |
| Longevity | Strong | Verified (activity), Inferred | 13 years, active in November 2026 |
| Content endurance | Strong | Inferred | Enhancement layer |
| Exit and portability | Strong | Inferred | Nothing to extract |
| Adjustability and support | Moderate | Inferred | Haskell narrows the contributor pool; community support only |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified (contributors), Inferred | De facto standard; one maintainer holds 67% of commits |
| Gates | Pass (telemetry, licence, exit), Unknown (accessibility floor) | Inferred | None Verified; no gate failed |

## Relationship to project

Tier: dev toolchain. On adopt, the process graduates to `uc-processes` as a short procedure for checking shell scripts, and the tool is installed from the distribution package or, where a pinned version is needed, through OSAT or the versioned-local-binaries pattern already in `adopt/installers/`.

Design decisions to record before adopt:

- Source: distribution package (signed by the archive) by default; pinned static binary or conda-forge package where a fixed version matters.
- Version: pin the version used in CI and pre-commit, and bump it deliberately.
- Defaults: a project `.shellcheckrc` that sets the shell and any agreed exclusions, with each exclusion explained in a comment.
- Output: `-f gcc --color=never` as the documented accessible default, set through `SHELLCHECK_OPTS`.
- CI: run with `--norc` plus explicit flags, or with the reviewed project `.shellcheckrc` only, so a stray parent configuration cannot silence checks.
- Scope: start with scripts in `uc-automa`, the Ventoy and `ventoy-raw-node` work, and release helpers.

## Status notes

- Last reviewed: 2026-11-15
- In assess: light pass done against metrics 0.6.0 on documentation and source evidence. No gate failed; three gates pass on Inferred evidence and the accessibility floor is Unknown. Testing still needed before the entry can leave Assess: the telemetry trace and `strace` file check on Linux Mint, a terminal screen reader pass over the `tty` and `gcc` formats, and confirming the version shipped in Ubuntu 24.04 and Linux Mint 22. Also pending: the Representation review. The automated pass recommends adopt once every gate and the Security section are Verified, or a named person records an override; trial is not recommended, since risk is low and testing can answer the open questions.

## Links

- https://www.shellcheck.net/
- https://github.com/koalaman/shellcheck
- https://github.com/koalaman/shellcheck/wiki
- https://tracker.debian.org/pkg/shellcheck

## References

<a id="ref-holen-2025"></a>
Holen, V. (2025). *ShellCheck changelog: v0.11.0, 2025-08-03* [Computer software documentation]. GitHub. https://github.com/koalaman/shellcheck/blob/master/CHANGELOG.md
[Return to citation](#cite-holen-2025)

## License

This document, *uc-radar--shellcheck--shell-script-linter*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

---
dc:title: "Universal Cake Evaluation: mingit, Git Command Alias"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "mingit"
  - "git"
  - "shell"
  - "developer tools"
dc:description: "Universal Cake evaluation of mingit 2.4.0, a shell function and completion bundle that shortens common git commands, installed from npm or by hand."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-11"
dcterms:modified: "2026-10-11"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/evansendra/mingit"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template"
dc:identifier: "universal-cake-evaluation--mingit--git-alias"
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
      Initial Assess-ring evaluation of mingit 2.4.0 against Universal
      Cake Evaluation Metrics 0.5.1, scoped to use as a personal
      developer shell tool, with the npm install, the g function,
      and the uninstall script tested in bash on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # not recommended for Trial; git's own aliases with __git_complete cover the same need
      reasoning: "All four default gates pass for personal use; telemetry Verified, licence Verified with a redistribution caveat. No network contact and no data. But the npm install runs lifecycle scripts that append to shell start-up files without a leading newline, which corrupted an existing line in testing; reinstalling duplicates the line; npm 7 and later never run the uninstall script, and running it by hand leaves a dangling source line that errors at every shell start. Arguments containing spaces are split for every command except commit, and the documented g r does not work. Licence is ISC in package.json only, with no LICENSE file, alongside bundled GPL-2.0 git completion scripts. Dependencies are unpinned, the project has one author (99% of commits) and three commits in two years."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-11T16:05:00-04:00"
  human: []
---

# Universal Cake Evaluation: mingit, Git Command Alias

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates mingit, a small shell function and completion bundle that lets a developer type `g s` for `git status`, `g c` for `git commit`, and so on, against the Universal Cake Evaluation Metrics. It records the gate results, a scorecard, the findings that shaped the ratings, and a recommendation for the radar. mingit passes all four default gates for personal use: it makes no network contact, holds no data, and is trivially replaced. Its problems are in how it gets onto and off a machine, and in its own correctness. The npm installer edits shell start-up files in ways that can damage them, the documented uninstall path does not work with current npm, arguments containing spaces are split apart, and one documented command fails. The licence situation is also untidy. We recommend leaving it in Assess and meeting the same need with git's own alias mechanism.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about mingit come from files in the mingit repository, read directly at commit `f893664a` on 2026-10-11, from the 2.4.0 package published on npm, from the npm registry's metadata for mingit and its dependencies, and from installing, using, and uninstalling that package under a test home directory with every relevant system call traced. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate mingit 2.4.0, the current release on npm, as a personal developer tool for interactive shells. No consuming project was named, and mingit is not a dependency anything else would build on, so the evaluation is scoped to a developer installing it on their own workstation. The npm install, the `g` function, and the uninstall script were tested in bash 5.2 on Linux. The zsh paths for macOS and the Oh My Zsh plugin, and the Git Bash path for Windows, were assessed from source only.

mingit wraps git; it does not change what git itself does. git's own behaviour is out of scope.

## Scorecard metadata

- Product or service name: mingit
- Version evaluated: 2.4.0, `mingit-2.4.0.tgz` from npm, SHA-256 `0d808d5a7935922d7a96d6ad4ce20932ef76ad57bdff46d9a5554164ff5bbbe8`; repository commit `f893664a007b6bc1241c005a8427058facd5f942`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-11
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, GNU bash 5.2.21, Node.js 22.22, npm 10.9.4, git 2.43

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | A trace of mingit's own install scripts recorded no network calls; every connection during `npm install` came from npm itself, fetching packages from the registry. The `g` function only calls `git`, see Findings |
| A licence incompatible with the project | Pass, for use only | Verified, Inferred | `package.json` declares ISC, but the repository has no LICENSE file. The package bundles git's completion scripts, which are GPL-2.0. Using mingit raises no issue. Redistributing it inside an AGPL-3.0-or-later or GPL-3.0-or-later project would combine GPL-2.0-only code and needs licence review first, see Findings |
| Failure of the accessibility floor | Pass | Inferred | A shell function with no interface of its own; all output is git's. Abbreviations reduce typing but are not self-describing. Not tested with a screen reader |
| No exit | Pass | Verified | mingit holds no data. Leaving means deleting `~/.mingit.sh` and the lines it added to shell start-up files, then typing `git` again. The automated uninstall path is broken, so this exit is manual |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Fewer keystrokes help anyone for whom typing is costly. In exchange, commands like `pll`, `psh`, and `rsto` must be memorised, and tab completion is kept, which offsets some of that. Documentation is a single README on GitHub. Not tested with assistive technology |
| Multilingual integration | Moderate | Verified | Adds no messages of its own beyond install notices, so git's own localisation passes through. The abbreviations and README are English only |
| Economic and cognitive accessibility | Strong | Verified | Free. The package is 289 KB unpacked, most of it bundled completion scripts and logo images; the function itself is about 1 KB. Fifteen commands to learn |
| Representation | Unknown | None | Not assessable from public information. One author; no machine-learned models in the product |
| Compatibility | Weak | Verified | The installer chooses by operating system, not by shell: every Linux install writes to `~/.bashrc`, so zsh and fish users on Linux are not served. Bash on macOS needs `package.json` edited before install, which is impractical for a global npm install. The Linux script hard-codes `/usr/share/bash-completion/completions/git`. The README says it was tested on Mac OS X El Capitan (claimed). Requires Node.js and npm to install a shell script |
| Resilience | Strong | Verified | Works offline once installed. No services, accounts, or tokens |
| Agency, sovereignty and privacy | Moderate | Verified | Stores and transmits nothing (verified). But installing it edits the user's shell start-up files without asking, and the `g` name overrides any existing `g` command or alias |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms, pricing, or data. Exit cost is unlearning fifteen abbreviations and removing a few lines |
| Agency, interaction patterns | Weak | Verified | Not malicious, but its defaults do not respect the user's files: the install appends to `~/.bashrc` without a leading newline and without checking for an existing entry, and the documented uninstall does not run under current npm. Commands silently mis-handle arguments containing spaces. Lens: user and owner are the same person |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | A shell function; negligible runtime cost. Pulls five npm packages at install time to copy two files |
| Security | Weak | Verified | npm install-time lifecycle scripts write to shell start-up files, a persistence location. Dependencies are declared with an empty version range, so any future release of `fs-extra` or `homedir` is accepted, and no lock file is published. `homedir` was last released in 2014 and duplicates Node's built-in `os.homedir`. No npm provenance attestation, no git tags for any release, no security policy; 119 of 123 commits are unsigned. The zsh install runs `compinit -u`, which skips zsh's check for insecure completion directories. Known advisories were not checked |
| Longevity | Weak | Verified | Started in 2015. Three commits in the two years to 2026-10-11, all on 2025-06-20, and none in the last 90 days. 31 npm releases since 2016, the latest 2.4.0 in June 2025 |
| Content endurance | Strong | Inferred | mingit stores and authors nothing |
| Exit and portability | Moderate | Verified | No data to move, and replacements are built into git, see Findings. Exit is manual: `npm uninstall` leaves `~/.mingit.sh` and the start-up line in place, and the bundled uninstall script, run by hand, leaves a dangling line that errors on every new shell |
| Adjustability and support | Moderate | Verified, Inferred | About 1 KB of shell, easy to read and change by hand. Support is GitHub issues to one person; no open issues were listed on 2026-10-11. The missing LICENSE file clouds the fork-and-fix path |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | A small niche convenience, 79 GitHub stars and 8 forks (claimed by the GitHub page). Overlaps with git aliases and Oh My Zsh's git plugin, which its own README says it overrides. Contributor concentration: 122 of 123 commits (99%) by the author, under two name spellings; the other is one 2015 commit. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass for personal use. Telemetry and exit Verified; licence Verified with a redistribution caveat; accessibility floor Inferred |

## Findings

### Network and file system behaviour

We installed the npm 2.4.0 package globally into a separate prefix with a test home directory, under `strace` recording `connect`, `sendto`, file opens, renames, deletions, and process starts. All 32 connections in the run came from npm's own processes, to the npm registry's address range and DNS; none came from mingit's `install.js`, `postinstall.js`, or the shell script they start. Tracing those scripts on their own confirmed it: no network calls, and exactly two writes, creating `~/.mingit.sh` and appending to `~/.bashrc`.

At runtime, mingit is a shell function named `g` that maps an abbreviation to a git subcommand and passes anything else straight to `git` <a name="apa-mingit-bashrc-citation"></a>([mingit Project, n.d.-c](#apa-mingit-bashrc-reference)). It has no network or storage code of its own.

### Changes to shell start-up files

On Linux, the installer copies the function to `~/.mingit.sh` and appends `source ~/.mingit.sh` to `~/.bashrc` with `echo ... >>` <a name="apa-mingit-installsh-citation"></a>([mingit Project, n.d.-d](#apa-mingit-installsh-reference)). That has three consequences we reproduced.

First, the append does not begin with a newline. Our test `~/.bashrc` ended with `export FOO=1` and no trailing newline, as files saved by some editors do. After install the line read `export FOO=1source ~/.mingit.sh`; a new shell set `FOO` to `1source`, printed an error, and did not load mingit. The user's own setting was silently changed.

Second, the install does not check for an existing entry, so reinstalling or upgrading adds a second `source` line each time.

Third, removal does not work as documented. The README notes that npm 7 and later do not run uninstall scripts <a name="apa-mingit-readme-citation"></a>([mingit Project, n.d.-a](#apa-mingit-readme-reference)); with npm 10, `npm uninstall -g mingit` left both `~/.mingit.sh` and the `source` line in place. Running the bundled `uninstall.js` by hand, as the README suggests, deleted `~/.mingit.sh` but removed only the first `source` line <a name="apa-mingit-uninstall-citation"></a>([mingit Project, n.d.-e](#apa-mingit-uninstall-reference)). After a reinstall, the remaining line made every new bash session print a "No such file or directory" error.

On Windows the installer appends with no newline at all, and on macOS with zsh it appends a block that also runs `compinit -u` <a name="apa-mingit-install-citation"></a>([mingit Project, n.d.-b](#apa-mingit-install-reference)). These were read from source, not run.

### Correctness of the function

Every case in the `g` function except `c` passes arguments as unquoted `$@`, so bash splits any argument containing a space and expands any glob. In testing, `g a "my file.txt"` ran `git add my file.txt` and failed with "pathspec 'my' did not match any files". The same applies to `g co`, `g d`, `g psh`, and the rest. The `c` case was quoted when commit was fixed, which suggests the issue is an oversight rather than a design. zsh does not split unquoted parameters by default, so zsh users are probably unaffected (inferred, not tested).

The README lists `g r` for `git reset`, but the function matches `rst`; `g r` falls through to `git r`, which git rejects. The Linux manual install instruction, `cat ./.bashrc >> ~/.bashrc`, refers to a file that does not exist; the file is `scripts/bashrc`. The Windows variant lacks the completion mapping for `g f`.

### Licence

The only licence statement is `"license": "ISC"` in `package.json` <a name="apa-mingit-package-citation"></a>([mingit Project, n.d.-f](#apa-mingit-package-reference)). The repository has no LICENSE file and GitHub shows no detected licence. ISC requires its copyright and permission notice to accompany copies, and without the text there is no notice to carry, so the grant rests on a metadata field.

The package also bundles copies of git's bash completion script and zsh completion wrapper, plus an older bash-completion library inside `scripts/bash_profile`. The bash script's header says it is distributed under GPL version 2.0 <a name="apa-mingit-completion-citation"></a>([mingit Project, n.d.-g](#apa-mingit-completion-reference)), and git as a whole is GPL-2.0-only. None of this matters for running mingit. It does matter for redistribution: an ISC label over a package that contains GPL-2.0 files misstates the package's terms, and GPL-2.0-only code cannot be combined into an AGPL-3.0-or-later or GPL-3.0-or-later work. Anyone shipping mingit inside a Vishpala project should drop the bundled completion scripts and rely on the system's copy, which the Linux variant already does.

### Supply chain and maintenance

The npm package's code files match the repository at `f893664a` byte for byte. The package also contains IDE configuration directories, `.idea` and `.vscode`, that are not in the repository, which shows it was published from the author's working copy rather than from a clean checkout or CI. There are no git tags, so a version can only be matched to a commit by comparing contents, as we did. npm holds a registry signature for 2.4.0 but no provenance attestation <a name="apa-mingit-npm-citation"></a>([mingit Project, 2025](#apa-mingit-npm-reference)).

The two runtime dependencies are declared with empty version strings, which npm treats as "any version", and the lock file is in `.gitignore`. Our install resolved `fs-extra` 11.4.1 and `homedir` 0.6.0. `homedir` was last published in 2014 <a name="apa-homedir-npm-citation"></a>([Moore, 2014](#apa-homedir-npm-reference)) and does what Node's built-in `os.homedir` has done for years. A shell script that needs to copy two files does not need either package, and every install fetches whatever their maintainers have most recently published.

Activity is low. Of 123 commits since July 2015, 122 are by the author; the three in the last two years were made on a single day in June 2025, adding `g dst` and updating the README.

### Replacement path

git provides the same capability without an installer. `git config --global alias.s status`, and similar lines for each abbreviation, give `git s` with full completion and correct argument handling. For a one-letter `g`, bash's git completion provides `__git_complete g git`, to be run after `alias g=git` (verified in bash 5.2 with git 2.43) <a name="apa-git-completion-citation"></a>([Git Project, n.d.](#apa-git-completion-reference)). Both live in files the user already controls, can be kept in a dotfiles repository, and involve no third-party code.

## Recommendation

Keep mingit in Assess and do not move it to Trial. For the need it addresses, prefer git's own aliases, set in `~/.gitconfig` or a managed dotfiles repository, with `__git_complete` for a one-letter wrapper.

If mingit itself is wanted on a workstation:

- Do not install it through npm. Copy the relevant function file by hand, and add the `source` line yourself.
- Quote `"$@"` throughout the function before use, and remove the `g r` line from any notes.
- Back up shell start-up files before installing, and check them afterwards.
- To remove an npm install, delete `~/.mingit.sh` and every `source ~/.mingit.sh` line by hand, and check the line before each for a missing newline.
- Do not redistribute it in any project until it has a LICENSE file and the bundled GPL-2.0 completion scripts are removed.

Worth reporting upstream: the missing leading newline on append, duplicate entries on reinstall, the unquoted `$@`, the `g r` mismatch, the manual install path, the missing LICENSE file, the bundled GPL-2.0 files under an ISC label, and the unpinned dependencies.

Re-review triggers specific to mingit: a LICENSE file added, an install method that no longer runs npm lifecycle scripts or that edits start-up files safely, or renewed maintenance activity.

## License

This document, *Universal Cake Evaluation: mingit, Git Command Alias*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### mingit repository

- [README](#apa-mingit-readme-reference)
- [Install script, install.js](#apa-mingit-install-reference)
- [Linux function, scripts/bashrc](#apa-mingit-bashrc-reference)
- [Linux install script](#apa-mingit-installsh-reference)
- [Uninstall script](#apa-mingit-uninstall-reference)
- [package.json](#apa-mingit-package-reference)
- [Bundled git completion script](#apa-mingit-completion-reference)

### Packages and replacements

- [mingit 2.4.0 on npm](#apa-mingit-npm-reference)
- [homedir on npm](#apa-homedir-npm-reference)
- [git-completion.bash](#apa-git-completion-reference)

## References

<a name="apa-git-completion-reference"></a>Git Project. (n.d.). *git-completion.bash* [Source code]. GitHub. https://github.com/git/git/blob/master/contrib/completion/git-completion.bash
[Return to citation](#apa-git-completion-citation)

<a name="apa-mingit-readme-reference"></a>mingit Project. (n.d.-a). *README.md*. GitHub. https://github.com/evansendra/mingit/blob/master/README.md
[Return to citation](#apa-mingit-readme-citation)

<a name="apa-mingit-install-reference"></a>mingit Project. (n.d.-b). *install.js* [Source code]. GitHub. https://github.com/evansendra/mingit/blob/master/scripts/install.js
[Return to citation](#apa-mingit-install-citation)

<a name="apa-mingit-bashrc-reference"></a>mingit Project. (n.d.-c). *bashrc* [Source code]. GitHub. https://github.com/evansendra/mingit/blob/master/scripts/bashrc
[Return to citation](#apa-mingit-bashrc-citation)

<a name="apa-mingit-installsh-reference"></a>mingit Project. (n.d.-d). *install-linux-bashrc.sh* [Source code]. GitHub. https://github.com/evansendra/mingit/blob/master/scripts/install-linux-bashrc.sh
[Return to citation](#apa-mingit-installsh-citation)

<a name="apa-mingit-uninstall-reference"></a>mingit Project. (n.d.-e). *uninstall.js* [Source code]. GitHub. https://github.com/evansendra/mingit/blob/master/scripts/uninstall.js
[Return to citation](#apa-mingit-uninstall-citation)

<a name="apa-mingit-package-reference"></a>mingit Project. (n.d.-f). *package.json* [Package manifest]. GitHub. https://github.com/evansendra/mingit/blob/master/package.json
[Return to citation](#apa-mingit-package-citation)

<a name="apa-mingit-completion-reference"></a>mingit Project. (n.d.-g). *git-completions.bash* [Source code]. GitHub. https://github.com/evansendra/mingit/blob/master/scripts/git-completions.bash
[Return to citation](#apa-mingit-completion-citation)

<a name="apa-mingit-npm-reference"></a>mingit Project. (2025). *mingit 2.4.0* [Software release]. npm. https://www.npmjs.com/package/mingit/v/2.4.0
[Return to citation](#apa-mingit-npm-citation)

<a name="apa-homedir-npm-reference"></a>Moore, W. (2014). *homedir 0.6.0* [Software release]. npm. https://www.npmjs.com/package/homedir/v/0.6.0
[Return to citation](#apa-homedir-npm-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of mingit 2.4.0 against Universal Cake Evaluation Metrics 0.5.1, scoped to use as a personal developer shell tool, with the npm install, the g function, and the uninstall script tested in bash on Linux |

---
dc:title: "Universal Cake Evaluation: pass, Password Manager"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "pass"
  - "password-store"
  - "password managers"
  - "gnupg"
dc:description: "Universal Cake evaluation of pass (password-store) 1.7.4, the command-line password manager that keeps each secret in a GnuPG-encrypted file in a directory tree, optionally versioned with git."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://git.zx2c4.com/password-store/"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--gopass--password-manager, universal-cake-evaluation--passage--password-manager"
dc:identifier: "universal-cake-evaluation--pass--password-manager"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-19"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of pass 1.7.4 against Universal Cake
      Evaluation Metrics 0.6.0, one of three password manager evaluations
      prepared together with gopass and passage. Network and file system
      behaviour, the text interface and a full export verified on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for a single technical user or a small technical team, once the Inferred accessibility gate is verified
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. No network contact, GPL-2.0-or-later, secrets are ordinary OpenPGP files in a directory tree, so exit cost is minutes. A small, readable shell script with a long record and a same-day fix for its one critical CVE. Risks: no release since June 2021 while fixes accumulate unreleased; no integrity protection for the store and entry names in plain text; GnuPG setup is a steep barrier for non-technical people; output forces colour codes and tree drawing even when redirected; and GnuPG 2.4 keys encrypt with OCB AEAD by default, which some mobile clients cannot read."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T01:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: pass, Password Manager

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates pass, also called password-store and "the standard unix password manager", against the Universal Cake Evaluation Metrics. pass keeps each secret in its own file, encrypted with GnuPG, inside an ordinary directory tree, and can version that tree with git. It passes all four default gates. Its strengths are structural: a free licence, no network contact, no service to depend on, a file format any OpenPGP tool can read, and a single shell script small enough to read in an afternoon. Its risks are a project that has not released since 2021, a store whose entry names and structure are not protected, and a GnuPG setup that excludes most non-technical people.

This is one of three evaluations prepared together. The others cover gopass and passage, both of which descend from pass and use its storage layout.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about pass come from the project website <a id="cite-pass-site"></a>([Donenfeld, n.d.](#ref-pass-site)); from the source repository, read at commit `f554f79` and at the signed `1.7.4` tag through its GitHub mirror on 2026-11-19 <a id="cite-pass-source"></a>([Donenfeld, 2021](#ref-pass-source)); from the Ubuntu 24.04 package; from public vulnerability records; and from a network and file system trace of the installed package. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate pass 1.7.4 as packaged by Ubuntu 24.04 (`pass 1.7.4-6`), used from the command line by one person on their own machine, with or without a git remote for synchronization. Graphical front ends, browser extensions and mobile apps that read pass stores are mentioned where they affect compatibility but are not evaluated. Network and file system behaviour were verified on Linux only; macOS and the BSDs were assessed from source and documentation.

## Scorecard metadata

- Product or service name: pass (password-store)
- Version evaluated: 1.7.4, Ubuntu package `pass 1.7.4-6`, `/usr/bin/pass` SHA-256 `b0da432e8d377a67c7a74a9111c6b32889cce62f6e4f506a7bbdba817a268632`; upstream source read at commit `f554f79` (2026-10-27)
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, GnuPG 2.4.4, git 2.43.0, tree 2.1.1, bash 5.2.21, strace 6.8

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry exists. A trace of `init`, `git init`, `insert`, `generate`, `show` and `ls` recorded no internet socket calls, only Unix-socket connections to the local `gpg-agent`. Content leaves the machine only when the user runs `pass git push` to a remote they chose |
| A licence incompatible with the project | Pass | Verified | GPL-2.0-or-later, read from `COPYING` and the script header. Running it as a tool places no obligation on an AGPL-3.0-or-later or GPL-3.0-or-later project; copying its code into one is also compatible through the "or later" clause. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | Every task can be done at the command line with plain text in and out. Two output habits may get in the way of a terminal screen reader, see Findings. Not tested with a screen reader |
| No exit | Pass | Verified | Each secret is a standard OpenPGP file. A ten-line loop decrypted the whole test store to plain text, and `gpg -d` reads any single file without pass |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | Plain-text commands, prompts and errors, with errors that say what went wrong ("web/x is not in the password store"). `pass ls` always draws a tree with colour codes and `pass generate` always wraps the password in bold and colour codes, even when output is redirected and with `NO_COLOR` set (verified). Documentation is a manual page and a web page. Not tested with Orca, Speakup or Emacspeak (inferred) |
| Multilingual integration | Weak | Verified | Interface, help and manual are English only, and the script has no translation mechanism. Secrets themselves can hold text in any language |
| Economic and cognitive accessibility | Weak | Verified, Inferred | Free. Before the first secret can be stored, a person must create a GnuPG key pair, find its ID and run `pass init` with it; recovery depends on understanding and backing up that key. Reasonable for technical users, a real barrier for most others, including the older and less technical people Vishpala aims to serve |
| Representation | Unknown | None | Not assessable from public information. A volunteer project led by one maintainer. No machine-learned models are part of the product, so the AI proxies are Not applicable |
| Compatibility | Moderate | Verified, Claimed | Runs wherever bash, GnuPG and tree run: Linux, macOS, FreeBSD and OpenBSD, with a Cygwin platform file for Windows (verified in source); otherwise Windows needs WSL or a third-party client. Widely packaged, including Debian and Ubuntu (verified). A wide ecosystem of clients reads the format (claimed), but files written by GnuPG 2.4 with its default keys use OCB AEAD encryption (verified), which OpenKeychain-based Android clients cannot decrypt (claimed by those clients' documentation) |
| Resilience | Strong | Verified | Works fully offline. No accounts, services, licence server or tokens. Git synchronization is optional and degrades to a local store |
| Agency, sovereignty and privacy | Strong | Verified | GPL allows self-hosting, modification, forking and redistribution. Data rests on the user's machine and on any git remote the user chooses, encrypted. Entry names and folder structure are plain file names, visible to anyone who can see the store or its remote |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | No terms of service and no pricing. Exit cost is minutes: a loop over `pass show` produces a complete plain-text export (human-readable), and the store itself is OpenPGP files in a directory tree (machine-readable) that gopass and other clients read unchanged |
| Agency, interaction patterns | Strong | Verified, Inferred | No dark patterns, accounts or prompts that serve a vendor. The clipboard is cleared after 45 seconds by default, `rm` asks before deleting, and with git enabled every change is a commit that can be undone. `generate` prints the new password to the screen unless `--clip` is given, which a person in a shared space may not expect |
| Environment, direct, indirect, and authoring-side | Strong | Verified | A 25.8 KB shell script with no daemon beyond the `gpg-agent` that GnuPG already uses. A decrypt took about two hundredths of a second here. Runs on old and low-end hardware; editing happens in the user's own editor |
| Security | Moderate | Verified, Claimed | GnuPG for encryption, `/dev/urandom` for generation, a check that rejects paths escaping the store, and optional signature checks on `.gpg-id` files and extensions through `PASSWORD_STORE_SIGNING_KEY` (verified in source). Release tags are PGP-signed (signature present, not checked against the maintainer's key). Weaknesses: no integrity protection for the store as a whole, a design limit recorded as CVE-2020-28086 (claimed); a 2018 critical CVE in the signature check, fixed the day it was disclosed; no written security policy; fixes on the main branch since 2021 unreleased |
| Longevity | Moderate | Verified | In development since 2012, with 522 commits. Five commits in the two years to 2026-11-19, the latest four on 2026-10-27; the last release, 1.7.4, was in June 2021. Volunteer-run with no stated funding. Stable rather than active |
| Content endurance | Strong | Verified | Secrets are standard OpenPGP files in a plain directory tree. If pass disappeared tomorrow, `gpg` alone would still read every one of them |
| Exit and portability | Strong | Verified | See the No exit gate. The format is open, documented by example and reimplemented by gopass, passage and many clients |
| Adjustability and support | Moderate | Verified | One readable 711-line bash script, GPL-licensed, with an extension mechanism, so the fork-and-fix path is short. Support is a mailing list; patches are merged in occasional batches |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Strong | Verified, Claimed | The pass format is a de facto standard in its niche, implemented by gopass, passage and clients on every major platform. No take rate. Commands and file layout have been stable for over a decade. Highly forkable, as passage shows. Contributor concentration: 325 of 522 non-merge commits (62%) are by the maintainer, Jason A. Donenfeld, who alone tags releases. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### Network and file system behaviour

We created a GnuPG key in a disposable home directory and ran `pass init`, `pass git init`, `pass insert`, `pass generate`, `pass show` and `pass ls` under `strace`, recording `connect`, `sendto`, `sendmsg` and `bind`. No call used an internet socket. The only connections were Unix-socket connections to `gpg-agent` in the test home directory. The source contains no network code; git is called only when the store is a git repository, and pushes only when the user asks.

A trace of `pass insert` showed writes only inside the store: the new `.gpg` file, and with git enabled, the repository's objects, index and logs. Each commit message names the entry ("Add given password for web/example.org to store."), so the git history repeats entry names in plain text.

### Text interface and assistive technology

pass reads and writes plain text, and its errors are plain sentences, for example "Error: nothere is not in the password store." with exit status 1. Two habits work against people using screen readers or braille displays, and against anyone piping output into another program:

- `pass ls` and `pass find` call `tree` with `-C`, which forces colour codes even when output is redirected, and draw the hierarchy with tree characters (`|--`, `` `-- ``) whose meaning depends on position. We confirmed both with output redirected to a file and with `NO_COLOR=1` set.
- `pass generate` prints the new password wrapped in bold and bright-yellow codes, also unconditionally.

A terminal screen reader working in a real terminal will usually not speak the colour codes, since the terminal interprets them; the tree characters will be spoken. In Emacs shell buffers, logs and pipelines, the codes appear as text. A workaround for listing is `find ~/.password-store -name '*.gpg'`, or the Emacs package `password-store.el`, which ships in the pass repository and may suit Emacspeak users better than the shell (not tested). A test with Orca or Emacspeak is still needed before the accessibility gate can be called Verified.

### GnuPG setup and key choices

pass delegates all cryptography to GnuPG. That is a strength for security, since GnuPG is mature, and the main barrier for people: a new user must generate a key pair, protect it with a passphrase, find its ID, run `pass init` with that ID, and back up the private key somewhere safe, because losing it loses every secret.

One key choice matters for compatibility. A key created with GnuPG 2.3 or later, as on Ubuntu 24.04, advertises support for OCB AEAD encryption, and GnuPG then uses it. The test store's files were AEAD-encrypted (`gpg --list-packets` reports an "aead encrypted packet", cipher 9, AEAD mode 2). The Android Password Store documentation states that OpenKeychain cannot decrypt such files, and recommends removing OCB from the key's preferences before running `pass init` <a id="cite-aps-issues"></a>([Password Store for Android, n.d.](#ref-aps-issues)). Anyone planning to read the store on Android should make that change first, and re-encrypt with `pass init` if the store already exists.

### Security record and integrity

The project's one critical vulnerability, CVE-2018-12356, let an attacker spoof the GnuPG signatures that pass checks on `.gpg-id` files and extensions, through an incomplete regular expression. Its discoverer also called attention to the missing integrity protection for password files <a id="cite-brinkmann"></a>([Brinkmann, 2018](#ref-brinkmann)). The fix was released as 1.7.2 on 2018-06-14, the day of disclosure, which shows a responsive maintainer.

The second point stands. pass encrypts each file's contents, but nothing protects which files exist, their names, or where they sit. Someone who can write to a shared git remote can rename or swap entries, so that a password is later used for the wrong site; this is recorded as CVE-2020-28086 against pass through 1.7.3 <a id="cite-arch-pass"></a>([Arch Linux, n.d.](#ref-arch-pass)). Signed git commits and a trusted remote reduce the risk; pass itself does not address it. The same limit applies to gopass and passage.

There is no written security policy or contact. Past reports went to the oss-security list and the project's mailing list.

### Maintenance and releases

Of 522 non-merge commits since 2012, 325 are by Jason A. Donenfeld, who also maintains WireGuard. The last release, 1.7.4, was tagged on 2021-06-11. Since then, the main branch has gained Emacs package fixes and, on 2026-10-27, a reworked Wayland clipboard and menu support, none of it released. Distributions therefore ship 1.7.4 with their own patches. The project is stable rather than active: the core has needed little change, but improvements reach users slowly.

### Ecosystem

The website lists clients for Android, iOS, Windows and macOS, plus browser extensions and graphical front ends ([Donenfeld, n.d.](#ref-pass-site)). The best-known Android client, Android Password Store, has been archived by its maintainers <a id="cite-aps-archived"></a>([Android Password Store, n.d.](#ref-aps-archived)); community forks continue on F-Droid. Because gopass and many clients read pass stores unchanged, a store can outlive any one client.

## Recommendation

Keep pass in Assess for now, and move it to Trial for a single technical user or a small technical team once a terminal screen reader pass verifies the accessibility floor, as the lifecycle requires.

For that Trial, on these conditions:

- Use a GnuPG key with OCB removed from its preferences if any Android client will read the store, and record the key's backup location.
- Enable git (`pass git init`), sign commits if the remote is shared, and keep the remote under the owner's control.
- Avoid revealing names: entry names and folders are visible wherever the store is copied.
- Expect no upstream releases; track the distribution package and note fixes on the main branch that it lacks.

pass is not suitable on its own for non-technical people, given the GnuPG setup it requires. For them, the question becomes which front end or which other product, not whether to use pass.

Re-review triggers specific to pass: a new release; a change of maintainer; a fix for store integrity; and any change in how GnuPG chooses encryption modes by default.

## License

This document, *Universal Cake Evaluation: pass, Password Manager*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### pass

- [Website](#ref-pass-site)
- [Source repository](#ref-pass-source)

### Security records

- [CVE-2018-12356 disclosure](#ref-brinkmann)
- [Arch Linux security tracker for pass](#ref-arch-pass)

### Clients

- [Android Password Store, archived](#ref-aps-archived)
- [Password Store for Android, common issues](#ref-aps-issues)

## References

<a id="ref-aps-archived"></a>Android Password Store. (n.d.). *Android Password Store*. Open Collective. Retrieved November 19, 2026, from https://opencollective.com/Android-Password-Store
[Return to citation](#cite-aps-archived)

<a id="ref-arch-pass"></a>Arch Linux. (n.d.). *pass* [Security tracker entry]. Retrieved November 19, 2026, from https://security.archlinux.org/package/pass
[Return to citation](#cite-arch-pass)

<a id="ref-brinkmann"></a>Brinkmann, M. (2018, June 14). *CVE-2018-12356 breaking signature verification in pass (Simple Password Store)* [Mailing list post]. oss-security. https://www.openwall.com/lists/oss-security/2018/06/14/3
[Return to citation](#cite-brinkmann)

<a id="ref-pass-site"></a>Donenfeld, J. A. (n.d.). *pass: The standard unix password manager*. Retrieved November 19, 2026, from https://www.passwordstore.org/
[Return to citation](#cite-pass-site)

<a id="ref-pass-source"></a>Donenfeld, J. A. (2021). *password-store* (Version 1.7.4) [Computer software]. https://git.zx2c4.com/password-store/
[Return to citation](#cite-pass-source)

<a id="ref-aps-issues"></a>Password Store for Android. (n.d.). *Common issues*. Retrieved November 19, 2026, from https://docs.passwordstore.app/docs/users/common-issues
[Return to citation](#cite-aps-issues)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of pass 1.7.4 against Universal Cake Evaluation Metrics 0.6.0, with network and file system behaviour, the text interface and a full export verified on Linux |

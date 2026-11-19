---
dc:title: "Universal Cake Evaluation: passage, Password Manager"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "passage"
  - "password managers"
  - "age"
dc:description: "Universal Cake evaluation of passage 1.7.4a2, the fork of pass that replaces GnuPG with age for encrypting each secret in a directory tree."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/FiloSottile/passage"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--pass--password-manager, universal-cake-evaluation--gopass--password-manager"
dc:identifier: "universal-cake-evaluation--passage--password-manager"
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
      Initial Assess-ring evaluation of passage 1.7.4a2 against Universal
      Cake Evaluation Metrics 0.6.0, one of three password manager
      evaluations prepared together with pass and gopass. Network and
      file system behaviour, the text interface and a direct export with
      age verified on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # remain in assess; not recommended for trial beyond the evaluator's own experiments
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. No network contact, GPL-2.0-or-later, each secret is a standard age file that `age -d` reads, and age is simpler to set up and reason about than GnuPG, with hardware-key plugins. Risks outweigh this for adoption: only alpha tags, two commits since 2022 and none since August 2024; no init command, which fails with a shell error and exit status 0; no manual page, security policy, CI or signed tags; not packaged by Debian or Ubuntu, where `passage` is an unrelated game; the signature checks pass offers were removed; and few mobile or graphical clients read age stores."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T01:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: passage, Password Manager

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates passage, a fork of pass by Filippo Valsorda that encrypts each secret with age instead of GnuPG, against the Universal Cake Evaluation Metrics. passage keeps pass's commands, directory layout and git integration, and replaces GnuPG keys, trust and agents with age identities and recipient files, including hardware-backed identities through age plugins. It passes all four default gates. Its design is the simplest of the three tools evaluated together. Its weakness is the project around it: alpha tags only, very little activity, missing pieces such as an init command and a manual page, and almost no distribution packaging.

This is one of three evaluations prepared together. The others cover pass and gopass.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about passage come from its repository and README, read at commit `4e4c5ae` (the `1.7.4a2` tag) on 2026-11-19 <a id="cite-passage-source"></a>([Valsorda, 2024](#ref-passage-source)); from the age project <a id="cite-age"></a>([Valsorda & Cartwright-Cox, n.d.](#ref-age)); from distribution package listings; and from a network and file system trace of passage installed from that commit. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate passage at its latest tag, `1.7.4a2`, installed with `make install`, used from the command line by one person with a software age identity, and with git enabled. Hardware identities through `age-plugin-yubikey` or `age-plugin-se` are discussed from documentation but were not tested. Network and file system behaviour were verified on Linux only; macOS and the BSDs were assessed from source and documentation.

## Scorecard metadata

- Product or service name: passage
- Version evaluated: `1.7.4a2`, commit `4e4c5ae` (2024-08-30), installed `/usr/local/bin/passage` SHA-256 `8247b22fd65ddef56c50841cbdc0fc5285debe30eb1ec1bcf32ed2e04eedf0ab`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, age 1.1.1, git 2.43.0, tree 2.1.1, bash 5.2.21, strace 6.8

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry. Traces of `git init`, `insert`, `generate`, `show`, `ls` and `reencrypt` recorded no socket calls of any kind; age needs no agent. Content leaves the machine only when the user pushes to a git remote they chose |
| A licence incompatible with the project | Pass | Verified | GPL-2.0-or-later, inherited from pass and read from `COPYING` and the script header. Compatible with AGPL-3.0-or-later and GPL-3.0-or-later projects. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | Every task can be done at the command line with plain text. Output has the same forced colour codes and tree drawing as pass, and setup must be done by hand, see Findings. Not tested with a screen reader |
| No exit | Pass | Verified | Each secret is a standard age file. `age -d -i ~/.passage/identities` decrypted an entry directly, without passage |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | Plain-text commands, prompts and errors. As in pass, `ls` forces colour codes and tree characters and `generate` forces bold and colour, even when redirected and with `NO_COLOR` set (verified). There is no manual page; the README and the built-in usage text are the only documentation (verified). Not tested with Orca, Speakup or Emacspeak (inferred) |
| Multilingual integration | Weak | Verified | Interface and README are English only, with no translation mechanism |
| Economic and cognitive accessibility | Weak | Verified | Free. Setup is manual: create `~/.passage/identities` with `age-keygen`, optionally protect it with a passphrase, and write `.age-recipients` files by hand. `passage init` does not exist; running it prints a shell "command not found" error and exits with status 0. age keys are much simpler than GnuPG keys, but the missing guidance leaves setup to technical users |
| Representation | Unknown | None | Not assessable from public information. A small volunteer project. No machine-learned models are part of the product, so the AI proxies are Not applicable |
| Compatibility | Moderate | Verified, Claimed | Runs wherever bash, age and tree run: Linux, macOS, FreeBSD and OpenBSD, with a Cygwin platform file for Windows (inferred from the platform files, not tested). No native Windows support. Not packaged by Debian or Ubuntu; Ubuntu's package named `passage` is a game (verified). Packaged by Alpine, MacPorts and the Arch User Repository, and by Nix at an older 2022 commit (claimed by package listings). Few mobile or graphical clients read age stores |
| Resilience | Strong | Verified | Works fully offline, with no daemon, accounts, services or tokens |
| Agency, sovereignty and privacy | Strong | Verified | GPL allows self-hosting, modification, forking and redistribution. Data rests on the user's machine and chosen git remote, encrypted. Entry names and folders are plain file names, as in pass |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | No terms of service and no pricing. Exit cost is minutes: every entry is an age file in a directory tree (machine-readable) and a loop over `passage show` produces a plain-text export (human-readable). Moving to pass or gopass with GnuPG needs a decrypt and re-encrypt, an hour or less for a personal store |
| Agency, interaction patterns | Moderate | Verified | No dark patterns. Same supportive defaults as pass: clipboard cleared after 45 seconds, confirmation before deleting, git history for undo. A failed `init` that reports success breaks the user's ability to predict what a command did |
| Environment, direct, indirect, and authoring-side | Strong | Verified | A 22.5 KB shell script plus the age binary. A decrypt took about a hundredth of a second here. Runs on old and low-end hardware |
| Security | Moderate | Verified, Claimed | age is a small, modern, well-specified format (claimed by its authors), and passage needs no agent or keyring. Hardware identities through age plugins are supported (claimed). Weaknesses: the README's simplest setup stores the identity unencrypted on disk; pass's `PASSWORD_STORE_SIGNING_KEY` checks on recipient files and extensions are gone (verified in source); the store has no integrity protection; no security policy, no CI, and tags are unsigned lightweight tags (verified). No known vulnerabilities found in a web search |
| Longevity | Weak | Verified | Forked in December 2021. 15 commits since the fork, two of them in 2024 and none since 2024-08-30. Only alpha tags (`1.7.4a0` to `1.7.4a2`). The maintainer is well known for age and Go cryptography work, which keeps the project visible but has not kept it active |
| Content endurance | Strong | Verified | Secrets are standard age files; age itself has several independent implementations (claimed) |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Moderate | Verified | One readable 663-line bash script, GPL-licensed, close to pass, so the fork-and-fix path is short. Pull requests on GitHub wait a long time; the two 2024 commits were contributed pull requests |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | A niche tool with a recognizable name. No take rate. Commands match pass. Highly forkable. Contributor concentration: 6 of 15 post-fork commits are by Filippo Valsorda, who merges all changes. gopass's age backend and other tools compete for the same users. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### Network and file system behaviour

We installed passage from the `1.7.4a2` commit, created an age identity in a disposable home directory, and ran `passage git init`, `insert`, `generate`, `show`, `ls` and `reencrypt` under `strace`, recording `connect`, `sendto`, `sendmsg` and `bind`. No socket call of any kind was recorded, not even a Unix socket, since age reads the identity file directly and needs no agent. A trace of `passage insert` showed writes only inside the store and its git repository. Commit messages name each entry, as in pass.

### Setup and the missing init command

pass users start with `pass init`. passage has no such command: the dispatcher still routes `init` to a function that the fork removed. With an identity present, `passage init` printed `cmd_init: command not found` and the script exited with status 0, because it ends with an unconditional `exit 0`. A script or a person checking the exit status would believe the store was set up.

Setup follows the README instead <a id="cite-passage-readme"></a>([Valsorda, 2024](#ref-passage-source)): create `~/.passage/identities` with `age-keygen`, and optionally a `.age-recipients` file in the store or any folder. Without a recipients file, passage encrypts to the identities file itself. The README offers three patterns: a plain identity on disk, an identity protected by an age passphrase, and an identity on a YubiKey through `age-plugin-yubikey`. Only the first was tested here. With a passphrase-protected identity there is no agent to remember it, so every decryption asks for the passphrase.

### Text interface and assistive technology

passage inherits pass's output habits: `ls` and `find` call `tree -C`, forcing colour codes and positional tree characters, and `generate` wraps the password in colour codes, even when redirected and with `NO_COLOR` set. We verified both. There is no manual page, so the README on GitHub and `passage help` are the only references. A test with Orca or Emacspeak is needed before the accessibility gate can be called Verified.

### Security design

age encrypts each file to one or more recipients with modern primitives and has no configuration to get wrong, which removes most of the GnuPG pitfalls described in the pass and gopass evaluations, including the OCB AEAD compatibility problem ([Valsorda & Cartwright-Cox, n.d.](#ref-age)). Recipient files are per folder, so sharing a folder means listing a teammate's public key in that folder's `.age-recipients`.

Three trade-offs come with the fork:

- The plainest setup keeps the private identity unencrypted at `~/.passage/identities`, protected only by file permissions. The README presents this as suitable when the store is synced somewhere less trusted than the local disk. An owner protecting against theft of the machine should use the passphrase or hardware pattern.
- pass can verify GnuPG signatures on `.gpg-id` files and extensions through `PASSWORD_STORE_SIGNING_KEY`. passage removed this, and age offers no signatures, so a tampered `.age-recipients` file in a shared remote would be used without warning. Reviewing recipient changes in git is the only defence.
- As with pass, nothing protects the store's file names or structure.

There is no security policy, no CI, and the three tags are lightweight tags with no signature, so a download cannot be checked against anything but the repository itself.

### Packaging and naming

passage is not in Debian or Ubuntu. Ubuntu 24.04 does have a package named `passage`, described as "game about the passage through life" (verified with `apt-cache`). Alpine packages `1.7.4_alpha2`, MacPorts packages `1.7.4a2`, the Arch User Repository offers `passage-git`, and Nix carried a 2022 snapshot <a id="cite-alpine"></a>([Alpine Linux, n.d.](#ref-alpine)). Guix packages it under the name `pass-age` to avoid the clash <a id="cite-guix"></a>([Guix Patches, n.d.](#ref-guix)). For most Linux users, installation means cloning the repository and running `make install`.

### Maintenance

Filippo Valsorda created passage in December 2021 by replacing GnuPG with age in the pass script. Fifteen commits followed, mostly in 2021 and 2022; in August 2024, two contributed changes were merged, adding `reencrypt` and fixing git diff configuration. Nothing has changed since. The design is small enough that little needs to change, but missing pieces such as `init` and a manual page have stayed missing, and issues and pull requests wait.

## Recommendation

Keep passage in Assess. It is not recommended for Trial on behalf of clients or a team at this time.

It is worth watching, and worth experimenting with personally, because age removes the hardest part of pass, GnuPG, and hardware-backed identities make a well-protected setup straightforward. For a personal experiment:

- Install from a reviewed commit and record its hash, since there are no signed releases.
- Protect the identity with a passphrase or a hardware plugin, and keep a second recipient as a recovery key in each `.age-recipients` file.
- Review every change to `.age-recipients` files in git, especially in a shared remote.
- Do not rely on `passage init` or on exit statuses for setup.

If an age-based store is wanted for a team, gopass's age backend is the more maintained route to the same idea, at the cost of a much larger tool.

Re-review triggers specific to passage: a non-alpha release; any new commit activity or a change of maintainer; packaging in Debian; and a maintained mobile client for age stores.

## License

This document, *Universal Cake Evaluation: passage, Password Manager*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### passage and age

- [passage repository and README](#ref-passage-source)
- [age](#ref-age)

### Packaging

- [Alpine Linux package](#ref-alpine)
- [Guix packaging discussion](#ref-guix)

## References

<a id="ref-alpine"></a>Alpine Linux. (n.d.). *passage* (Version 1.7.4_alpha2-r2) [Package details]. Retrieved November 19, 2026, from https://pkgs.alpinelinux.org/package/v3.24/community/x86/passage
[Return to citation](#cite-alpine)

<a id="ref-guix"></a>Guix Patches. (n.d.). *Comment on packaging passage as pass-age* [Patch review comment]. Retrieved November 19, 2026, from https://patches.guix-patches.cbaines.net/comment/5445
[Return to citation](#cite-guix)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

<a id="ref-passage-source"></a>Valsorda, F. (2024). *passage* (Version 1.7.4a2) [Computer software]. GitHub. https://github.com/FiloSottile/passage
[Return to citation](#cite-passage-source)
[Return to second citation](#cite-passage-readme)

<a id="ref-age"></a>Valsorda, F., & Cartwright-Cox, B. (n.d.). *age* [Computer software]. GitHub. Retrieved November 19, 2026, from https://github.com/FiloSottile/age
[Return to citation](#cite-age)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of passage 1.7.4a2 against Universal Cake Evaluation Metrics 0.6.0, with network and file system behaviour, the text interface and a direct export with age verified on Linux |

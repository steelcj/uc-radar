---
dc:title: "Universal Cake Evaluation: gopass, Password Manager"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "gopass"
  - "password managers"
  - "gnupg"
  - "age"
dc:description: "Universal Cake evaluation of gopass 1.17.4, the Go command-line password manager that extends the pass storage layout with team features, several encryption and storage backends, and native builds for Linux, macOS and Windows."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/gopasspw/gopass"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--pass--password-manager, universal-cake-evaluation--passage--password-manager"
dc:identifier: "universal-cake-evaluation--gopass--password-manager"
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
      Initial Assess-ring evaluation of gopass 1.17.4 against Universal
      Cake Evaluation Metrics 0.6.0, one of three password manager
      evaluations prepared together with pass and passage. Release
      signature, network and file system behaviour, the text interface,
      a full export and pass compatibility verified on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for teams and for owners who need Windows, once the Inferred accessibility gate is verified
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. The only network contact outside the user's own git remote is an update check run by `gopass version`, which can be disabled, and an opt-in breach lookup that sends a five-character hash prefix. MIT licence, pass-compatible store, signed releases with SBOMs, an active project with a written security model. Risks: one person holds final say and most commits; a 22 MB binary built from about 100 Go modules; a run of path-traversal and related fixes in 2026; autopush on by default; emoji and progress bars in output; and Ubuntu's `gopass` package is a different, older project."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T01:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: gopass, Password Manager

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates gopass, a password manager written in Go that began as a team-oriented rewrite of pass, against the Universal Cake Evaluation Metrics. gopass keeps the pass storage layout, so a pass store and a gopass store are interchangeable, and adds multiple mounted stores, per-folder recipients, a setup wizard, an age backend alongside GnuPG, a choice of git or plain-file storage, and native builds for Windows. It passes all four default gates. It is the most actively maintained of the three tools evaluated together, with the strongest release engineering. Its risks come with its size: a large dependency tree, a single decision-maker, a run of security fixes in 2026, and an interface whose emoji and progress bars are less friendly to screen readers than plain text.

This is one of three evaluations prepared together. The others cover pass and passage.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about gopass come from its repository, read at commit `5f4d4032` and at the `v1.17.4` tag on 2026-11-19 <a id="cite-gopass-source"></a>([gopass Authors, 2026a](#ref-gopass-source)), including its security document <a id="cite-gopass-security"></a>([gopass Authors, n.d.-b](#ref-gopass-security)), configuration reference <a id="cite-gopass-config"></a>([gopass Authors, n.d.-a](#ref-gopass-config)), governance file and changelog <a id="cite-gopass-changelog"></a>([gopass Authors, 2026b](#ref-gopass-changelog)); from the signed 1.17.4 release archive; from public vulnerability records; and from a network and file system trace of the release binary. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate gopass 1.17.4, the current release, installed from the project's Linux release archive after verifying its signature, and used from the command line with the GnuPG backend and git storage. The age backend, the browser bridge (`gopass-jsonapi`), the Secret Service daemon and other companion tools are mentioned where they matter but are not evaluated. Network and file system behaviour were verified on Linux only; Windows and macOS were assessed from source and documentation.

## Scorecard metadata

- Product or service name: gopass
- Version evaluated: 1.17.4, `gopass-1.17.4-linux-amd64.tar.gz`, SHA-256 `fe63584ba2275409c54d060d7f601de24d03ca980b11e617cd30cd9c4814597a`, checksum file signed by the "Gopass Release Signing Key 2025" (fingerprint `A1FAC0FD50ACA8DE1E8C44717EA70A3567E6E8D2`)
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, GnuPG 2.4.4, git 2.43.0, strace 6.8; gopass reports itself as built with go1.26.0

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry. Traces of `init`, `insert`, `generate`, `show`, `ls`, `audit` and `fsck` recorded no internet socket calls. `gopass version` contacts the GitHub API to check for a newer release; setting `updater.check` to false or `CHECKPOINT_DISABLE` stops it, which we verified. The breach check in `audit` is off by default. With a git remote configured, autopush and autosync send encrypted entries to that remote by default; both can be turned off |
| A licence incompatible with the project | Pass | Verified | MIT, read from `LICENSE`. Compatible with AGPL-3.0-or-later and GPL-3.0-or-later projects, as a tool or as code. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | Every task can be done at the command line. Emoji, box-drawing characters and progress bars appear in output, see Findings; `--flat` listing and colour-free output when redirected help. Not tested with a screen reader |
| No exit | Pass | Verified | Secrets are standard OpenPGP files in a directory tree. `gpg -d` decrypted a gopass entry directly, and pass 1.7.4 read the gopass store unchanged |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Inferred | Plain-text commands and prompts. Colour is dropped when output is not a terminal (verified), and the source honours `NO_COLOR` (inferred). `gopass ls --flat` gives one entry per line (verified). Against that, messages open with emoji (🍭, 🔑, ✅, 🏁), which screen readers speak by name; the default `ls` draws a tree with box-drawing characters; and `audit` and `fsck` write progress-bar control codes even when output is redirected (verified). Not tested with Orca, Speakup or Emacspeak (inferred) |
| Multilingual integration | Weak | Verified | Interface and documentation are English only, with no translation mechanism in the code. Secrets can hold text in any language |
| Economic and cognitive accessibility | Moderate | Verified, Inferred | Free. `gopass setup` walks a person through creating keys and a store, and `generate` does not print the new password unless asked (verified), which is safer in shared spaces. The surface is large (mounts, recipients, templates, OTP, several backends), and GnuPG or age concepts are still needed |
| Representation | Unknown | None | Not assessable from public information. No machine-learned models are part of the product, so the AI proxies are Not applicable. The repository carries instructions for AI coding agents (`AGENTS.md`), and a few commits come from an AI agent account; this concerns development, not the product |
| Compatibility | Strong | Verified, Claimed | Release builds for Linux, macOS, Windows, FreeBSD and OpenBSD (verified in the release configuration). Reads and writes pass stores (verified with pass 1.7.4). A browser bridge and a git credential helper are available (claimed). Caution: Ubuntu 24.04's package named `gopass` is a different project, `aviau/gopass` 1.5.0 (verified with `apt-cache show`); installing it by name does not give this gopass. The GnuPG 2.4 OCB AEAD issue described in the pass evaluation applies here too (verified that entries are AEAD-encrypted) |
| Resilience | Strong | Verified | Works fully offline; every test ran without a remote. No accounts, services or licence server. Sync failures do not block local use (inferred from design) |
| Agency, sovereignty and privacy | Strong | Verified | MIT allows self-hosting, modification, forking and redistribution. Data rests on the user's machine and on git remotes the user chooses, encrypted. Entry names and folders are plain file names, as the project's own security document states ([gopass Authors, n.d.-b](#ref-gopass-security)) |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | No terms of service and no pricing. Exit cost is minutes: the store is readable by pass and by `gpg` alone, and `gopass show` in a loop gives a plain-text export |
| Agency, interaction patterns | Moderate | Verified | No dark patterns that serve a vendor. Two defaults act without asking: `core.autopush` and `core.autosync` are on, so commits are pushed to a configured remote automatically. Once a day at most, gopass prints a reminder to check for updates, run `fsck` or run `audit` when 90 days have passed; the reminders are local and can be turned off with `core.noreminder` |
| Environment, direct, indirect, and authoring-side | Moderate | Verified | A single static 22 MB binary, an 8.3 MB download. A decrypt took about two hundredths of a second here, with no daemon unless the optional age agent is used. The download size matters on slow or metered connections; the binary runs on modest hardware |
| Security | Moderate | Verified, Claimed | Strong release engineering: GnuPG-signed checksums (verified), Sigstore bundles and SBOMs, CodeQL, OpenSSF Scorecard, Grype and Dependabot in CI. A written threat model, recipient-file hash pinning, `crypto/rand` for generation, no shell-string command building, temporary files in `/dev/shm` (claimed by the security document). Weaknesses: about 106 Go modules in `go.mod` (45 direct); several path-traversal and related fixes in 1.17.0 and 1.17.4, including CVE-2026-118756 (claimed); no store integrity protection, which the project states openly; no written vulnerability-reporting route found in the repository |
| Longevity | Strong | Verified | In development since 2017, with 2,354 non-merge commits; 311 in the year to 2026-11-19 and five releases since August 2026. A governance document and a sponsorship link exist; no institutional funding is stated |
| Content endurance | Strong | Verified | gopass enhances a store it does not own: the files remain standard OpenPGP or age files that other tools read |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Strong | Verified, Inferred | Active issue tracker, extensive documentation in the repository, architecture decision records, MIT licence. The codebase is large Go, so the fork-and-fix path is longer than for a shell script |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified | The leading pass-compatible implementation for teams. No take rate. Command-line interface follows semantic versioning; the Go package API is best-effort stable. Forkable under MIT. Contributor concentration: 1,334 of 1,913 human non-merge commits (70%) are by Dominik Schulz, who is named as the project's Benevolent Dictator For Life. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### Release verification

The 1.17.4 checksum file carries a detached GnuPG signature. We imported the two public keys embedded in gopass's own updater source and checked it: the signature is good, from the "Gopass Release Signing Key 2025 (GitHub Actions Only)". The archive's SHA-256 matched the signed list. The release configuration also produces Sigstore bundles and SBOMs. Of the three tools evaluated, gopass offers the most ways to confirm that a download is what the project built.

### Network behaviour

We traced every command in the test for `connect`, `sendto`, `sendmsg` and `bind`. Only `gopass version` opened an internet connection, to the GitHub releases API through the test machine's proxy. Setting `CHECKPOINT_DISABLE=1`, or `gopass config updater.check false`, removed it. Source inspection found three other places that reach the network, none active by default:

- `gopass update`, run explicitly, which downloads and verifies a new release.
- `gopass audit` with `audit.hibp-use-api` set, which queries the Have I Been Pwned range API with the first five characters of each password's SHA-1 hash, a k-anonymity scheme the configuration reference describes as leaking "a few bits" ([gopass Authors, n.d.-a](#ref-gopass-config)).
- The age backend, which fetches public SSH keys from GitHub when a recipient is written as `github:username`.

Git operations reach whatever remote the user configured. Because autopush and autosync default to on, an owner who adds a remote should decide deliberately whether to keep them.

### File system behaviour

Apart from the store, gopass wrote a configuration file under `~/.config/gopass/` and reminder timestamps under `~/.cache/gopass/`. `gopass init` also exported the recipient's public key into the store, under `.public-keys/`, so collaborators can import it. Commit messages name each entry ("Save secret web/example.org: Insert user supplied password"), so the git history repeats entry names in plain text, as with pass.

### Text interface and assistive technology

gopass is friendlier than pass when its output is piped: colour is dropped when output is not a terminal, and `gopass ls --flat` lists one entry path per line. Three habits remain that a terminal screen reader user would meet:

- Status messages begin with emoji. A screen reader may announce "lollipop Initializing a new password store", "key Searching for usable private keys", and so on.
- The default `gopass ls` draws a tree with box-drawing characters.
- `gopass audit` and `gopass fsck` draw a progress bar using terminal control codes, and wrote those codes even when we redirected output to a file.

None of these blocks a task, which is why the gate is inferred to pass, but each adds noise. A test with Orca or Emacspeak should cover `setup`, `insert`, `show`, `ls --flat`, `audit` and `fsck`.

### Security model and 2026 fixes

The project's security document sets out goals, a threat model that assumes no attacker on the local machine, and a list of engineering choices: recipient-file hash pinning so that a teammate cannot silently add a key, checks on the age agent socket, clipboard clearing that only erases what gopass placed, elision of secrets from debug logs, and RAM-backed temporary files ([gopass Authors, n.d.-b](#ref-gopass-security)). It also states plainly that gopass protects the content of secrets but not their names or the integrity of the store.

The changelog for 1.17.0, released 2026-08-30, has a Security section listing fixes for a path traversal in the file system storage layer, symlink traversal outside the store, secret access from the template engine, editor path validation, and new warnings when `gopass env` exposes secrets through environment variables; 1.17.4 adds a further symlink fix ([gopass Authors, 2026b](#ref-gopass-changelog)). One of these is recorded as CVE-2026-118756: a collaborator with write access to a shared store could craft entry names so that a victim's `gopass rm -r` deletes directories outside the store <a id="cite-mend"></a>([Mend.io, 2026](#ref-mend)). The fixes were prompt and documented; their number shows that a larger feature set carries a larger attack surface, especially for shared stores. Anyone on a release before 1.17.0 should upgrade.

### Packaging and naming

Ubuntu 24.04 offers a package named `gopass`, version 1.5.0, whose homepage is `github.com/aviau/gopass`, a different and much smaller project. Installing "gopass" from Ubuntu's archive therefore does not install this gopass. Owners should install from the project's signed release archives, or from a distribution whose package points to `gopasspw/gopass`, and confirm with `gopass --version`.

### Maintenance and governance

gopass is active: 311 non-merge commits in the year to 2026-11-19, of which 117 are dependency updates by Dependabot, and releases 1.17.0 through 1.17.4 between August and October 2026. The governance file names Dominik Schulz as the Benevolent Dictator For Life, who decides when consensus fails, and he wrote 70% of all human commits. The project is healthier than its contributor concentration suggests, but its direction rests on one person.

## Recommendation

Keep gopass in Assess for now, and move it to Trial once a terminal screen reader pass verifies the accessibility floor. It is the best fit of the three for two scopes: a small team sharing a store, and an owner who needs the same tool on Windows.

For that Trial, on these conditions:

- Install 1.17.0 or later from the signed release archive, verify the checksum signature, and pin the version.
- Decide the update-check policy and set `updater.check` accordingly; leave `audit.hibp-use-api` off unless the owner accepts the prefix lookup.
- Decide deliberately on `core.autopush` and `core.autosync` before adding a remote, and enable `recipients.check` for shared stores.
- Use a GnuPG key with OCB removed from its preferences if Android clients will read the store, or use the age backend with clients that support it.
- Prefer `gopass ls --flat` in documentation and scripts, and set `core.noreminder` where reminders would be noise.

Re-review triggers specific to gopass: a change of BDFL or governance; a new security section in the changelog; a change to default network behaviour; and the arrival of a translation mechanism or an accessibility option for emoji and progress output.

## License

This document, *Universal Cake Evaluation: gopass, Password Manager*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### gopass

- [Source repository](#ref-gopass-source)
- [Security, known limitations and caveats](#ref-gopass-security)
- [Configuration reference](#ref-gopass-config)
- [Changelog](#ref-gopass-changelog)

### Security records

- [CVE-2026-118756](#ref-mend)

## References

<a id="ref-gopass-config"></a>gopass Authors. (n.d.-a). *Configuration* [Documentation]. GitHub. Retrieved November 19, 2026, from https://github.com/gopasspw/gopass/blob/master/docs/config.md
[Return to citation](#cite-gopass-config)

<a id="ref-gopass-security"></a>gopass Authors. (n.d.-b). *Security, known limitations, and caveats* [Documentation]. GitHub. Retrieved November 19, 2026, from https://github.com/gopasspw/gopass/blob/master/docs/security.md
[Return to citation](#cite-gopass-security)

<a id="ref-gopass-source"></a>gopass Authors. (2026a). *gopass* (Version 1.17.4) [Computer software]. GitHub. https://github.com/gopasspw/gopass
[Return to citation](#cite-gopass-source)

<a id="ref-gopass-changelog"></a>gopass Authors. (2026b). *CHANGELOG.md*. GitHub. https://github.com/gopasspw/gopass/blob/master/CHANGELOG.md
[Return to citation](#cite-gopass-changelog)

<a id="ref-mend"></a>Mend.io. (2026, October 28). *CVE-2026-118756* [Vulnerability database entry]. https://www.mend.io/vulnerability-database/CVE-2026-118756/
[Return to citation](#cite-mend)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of gopass 1.17.4 against Universal Cake Evaluation Metrics 0.6.0, with release signature, network and file system behaviour, the text interface, a full export and pass compatibility verified on Linux |

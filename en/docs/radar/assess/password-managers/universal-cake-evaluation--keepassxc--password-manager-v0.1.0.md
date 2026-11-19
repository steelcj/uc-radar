---
dc:title: "Universal Cake Evaluation: KeePassXC, Password Manager"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "keepassxc"
  - "kdbx"
  - "password managers"
  - "passphrase generators"
dc:description: "Universal Cake evaluation of KeePassXC 2.8.0, the cross-platform desktop and command-line password manager that stores secrets in a single encrypted KDBX file, including a close look at its password and passphrase generators and its security record."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/keepassxreboot/keepassxc"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--pass--password-manager, universal-cake-evaluation--gopass--password-manager, universal-cake-evaluation--gopass--password-generation, universal-cake-evaluation--xkcdpass--passphrase-generator"
dc:identifier: "universal-cake-evaluation--keepassxc--password-manager"
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
      Initial Assess-ring evaluation of KeePassXC 2.8.0 against Universal
      Cake Evaluation Metrics 0.6.0, with a section on its password and
      passphrase generators. Command-line network and file system
      behaviour, exports, database format and an independent read of the
      database verified on Linux; graphical start-up network behaviour
      traced under a virtual display.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for individuals and small offices, including non-technical people, once a screen reader pass verifies the accessibility floor
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. Offline by design: the command-line tool made no socket calls, and the desktop app reaches the network only for an update check it asks about first, an opt-in breach check and favicon downloads on request. The KDBX format is open and read by an independent library. A team of maintainers, a written security policy, an independent review and an ANSSI certification, careful generators using the system random source, and 47 interface languages. Risks: the command-line `db-create` still writes KDBX 3.1 with AES-KDF; the bundled passphrase list is a modified EFF list; a large C++ and Qt codebase with eight security fixes in 2.8.0; distribution packages lag, Ubuntu 24.04 ships 2.7.6; and the AppImage's command-line tool needs desktop graphics libraries."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T02:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: KeePassXC, Password Manager

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates KeePassXC, a community-maintained desktop password manager for Windows, macOS and Linux, against the Universal Cake Evaluation Metrics. KeePassXC keeps all secrets in one encrypted file in the KeePass KDBX format, offers a graphical application and a command-line tool, `keepassxc-cli`, and includes password and passphrase generators. It passes all four default gates. Of the password managers evaluated so far, it is the one most likely to suit people who are not technical: it needs no GnuPG keys or git, has a full graphical interface in 47 languages, and has a security record backed by an independent review and a national certification. Its risks are a weak default in the command-line database creator, a large codebase, and distribution packages that lag behind releases.

It was evaluated alongside pass, gopass and passage, and alongside a separate evaluation of gopass's password generators, so the generator findings here are written to be compared with those.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about KeePassXC come from its repository, read at the signed `2.8.0` tag and at commit `cd422365` (2026-11-17) <a id="cite-kpxc-source"></a>([KeePassXC Team, 2026](#ref-kpxc-source)), including its security policy, changelog and user documentation; from the project's security review page and blog <a id="cite-kpxc-audits"></a>([KeePassXC Team, n.d.](#ref-kpxc-audits)); from public reporting; and from tests of the 2.8.0 Linux AppImage. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate KeePassXC 2.8.0, released 2026-10-21, as distributed in the project's Linux AppImage, for one person or a small office keeping their own database on their own devices. The command-line tool was tested fully. The graphical application was started under a virtual display to trace its network behaviour at start-up, and assessed from source and documentation otherwise. Browser integration, the SSH agent, the Secret Service integration, KeeShare and passkeys are covered only where they affect the gates and security. Mobile apps that read KDBX files are separate products and are not evaluated.

## Scorecard metadata

- Product or service name: KeePassXC
- Version evaluated: 2.8.0, `KeePassXC-2.8.0-x86_64.AppImage`, SHA-256 `b52889fd627ae7a1c609b91b2526ccd5d847ec6707ac6eb634553186dee4f705`; source tag `2.8.0`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, Xvfb for the graphical start-up traces, strace 6.8, pykeepass 4.2.0 for the independent read

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | No telemetry. The command-line tool made no socket calls in any test. The graphical app made one internet connection at start-up when the update check was on, and none when it was off. It asks before enabling the check on first start. The breach check and favicon downloads run only when the user asks. Builds can omit all network code |
| A licence incompatible with the project | Pass | Verified | GPL-2.0-only or GPL-3.0-only, at the user's choice, read from `COPYING`. Note: no "or later" clause. Running it as a tool places no obligation on an AGPL-3.0-or-later project, and GPL-3.0 code can be combined with AGPL-3.0 code. Recheck per adopting project |
| Failure of the accessibility floor | Pass | Inferred | The command-line tool reads and writes plain text with no colour codes (verified). The graphical app uses Qt widgets, and the AppImage's Qt library carries the AT-SPI bridge that Linux screen readers use (verified); an Orca user reports it fully usable (claimed). Not tested with a screen reader |
| No exit | Pass | Verified | `keepassxc-cli export` wrote complete CSV, XML and HTML exports, and an independent library, pykeepass, opened the KDBX file and read every entry |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Strong | Verified, Inferred, Claimed | Command line: plain-text prompts, output and errors ("Invalid credentials were provided, please try again."), no colour, a `-q` flag to silence prompts (verified). Graphical app: Qt widgets with AT-SPI support (verified present), keyboard shortcuts that can now be configured, adjustable font size, and a project history of accessibility work (claimed). Not tested with Orca, NVDA or VoiceOver |
| Multilingual integration | Strong | Verified | 47 interface translations ship with the source, including Canadian French (`fr_CA`). Translation is done by volunteers on Transifex, through a web interface, without code changes |
| Economic and cognitive accessibility | Strong | Inferred | Free. Setting up needs only a master password; there are no keys, accounts or servers. A strength meter, a built-in generator and database reports guide choices. The full feature set (auto-type, browser integration, SSH agent, KeeShare) is large but off by default |
| Representation | Unknown | None | Not assessable from public information. Volunteer team; an earlier accessibility project with Internews brought in screen reader users (claimed). No AI features in the product; the project uses AI tools in development, see Findings, so the AI proxies are Not applicable to the product |
| Compatibility | Strong | Verified, Claimed | Builds for Windows (including Arm64), macOS and Linux (AppImage, Flatpak, Snap and distribution packages). The KDBX format is shared with KeePass and read by mobile apps on Android and iOS (claimed). Cautions: Ubuntu 24.04 ships 2.7.6 from 2023 (verified), and the AppImage's `keepassxc-cli` failed to start on a server without `libOpenGL.so.0` (verified) |
| Resilience | Strong | Verified | Works fully offline. One file holds everything; no services, accounts or licence server. Saves are atomic by default (verified in source) |
| Agency, sovereignty and privacy | Strong | Verified | GPL allows self-hosting, modification, forking and redistribution. Data rests in one file wherever the user puts it. Unlike pass-style stores, entry names are inside the encryption |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | No terms of service and no pricing. Exit cost is minutes: CSV and HTML exports are human-readable, XML and the KDBX file itself are machine-readable and open, and importers exist for 1Password, Bitwarden, Proton Pass and others (claimed) |
| Agency, interaction patterns | Strong | Verified | Plain asking: the update check is offered on first start, the breach check explains what it will send before it runs. Quiet defaults: the clipboard clears after 10 seconds and the database locks after 15 minutes idle; integrations are off until enabled. One nudge: the first-start question defaults to Yes |
| Environment, direct, indirect, and authoring-side | Moderate | Verified | The AppImage is 52 MB and unpacks to 138 MB, since it carries Qt 6. It runs without a daemon. Fine for most hardware; heavy on slow or metered connections, where a distribution package is lighter. Each graphical launch of the AppImage copies a desktop file and icon into the user's home directory |
| Security | Strong | Verified, Claimed | Written security policy with three private reporting routes; an independent review of 2.7.4 (2023) and an ANSSI first-level certification of 2.7.9 on Windows (2025) (claimed); cryptography from the Botan library; generators use the system random source with unbiased sampling (verified). 2.8.0 fixed eight security issues, including a use-after-free in the legacy KDB importer (CVE-2026-69150) and two in browser integration. Weak spots: `keepassxc-cli db-create` writes KDBX 3.1 with AES-KDF even with `-t` (verified); a large C++ attack surface; the release signature could not be checked here, see Findings |
| Longevity | Strong | Verified | Forked from KeePassX in 2016, itself from 2010; 3,294 non-merge commits since 2016 and 172 in the year to 2026-11-19; regular releases, 2.8.0 in October 2026. Five maintainers can merge, two of them core maintainers (claimed). No institutional funding is stated |
| Content endurance | Strong | Verified | KDBX is documented by its implementations and read by several independent ones, including pykeepass here |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Strong | Verified | GitHub issues, Matrix channels, an extensive user guide in the repository, Windows Group Policy overrides for administrators, and settings in plain INI files |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Strong | Verified, Claimed | Part of the large KeePass format ecosystem, with many independent clients, so no single project controls access to users' data. No take rate. The format is stable across versions. Forkable under the GPL. Contributor concentration since 2016: Jonathan White 875 and Janek Bevendorff 582 of 3,294 non-merge commits (44% together), with a broad tail. No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### Network behaviour

We traced `keepassxc-cli` running `db-create`, `add` with generation, `show`, `ls`, `generate`, `diceware` and `analyze` for `connect`, `sendto`, `sendmsg` and `bind`. There were no socket calls of any kind.

We then started the graphical application under Xvfb three times with a fresh profile, tracing the same calls:

- First start, with no settings: no internet connection in 15 seconds. The source shows that on first start the app asks "Would you like KeePassXC to check for updates on startup?" before checking, with Yes as the default button.
- Update check enabled: one connection, to the GitHub releases API through the test machine's proxy.
- Update check disabled: no internet connection.

Other network features run only on request. The Have I Been Pwned report shows a caution that explains what will be sent, the first five characters of each password's SHA-1 hash and the number of passwords, and has its own "Perform Online Analysis" button. Favicons are downloaded when the user asks, and the fallback to a third-party favicon service is off by default. A build option, `KPXC_FEATURE_NETWORK`, removes all network code.

### Database format defaults

KeePassXC supports two file formats: KDBX 4, which uses Argon2 for key derivation and stronger header protection, and the older KDBX 3.1, which uses AES-KDF. The graphical new-database wizard offers "KDBX 4 (recommended)" with Argon2d.

The command-line tool does not. A database created with `keepassxc-cli db-create` was KDBX 3.1 (file header version `0x00030001`) with "KDF: AES (1000000 rounds)". With `-t 1000` to target a one-second unlock, it was still KDBX 3.1, now with 66,666,668 AES rounds. The cause is in the source: the core database object defaults to AES-KDF, and the wizard, not the database, switches it to Argon2d. The 2023 security review recommended moving to the latest format <a id="cite-ghacks"></a>([Brinkmann, 2023](#ref-ghacks)). Argon2's memory cost is what slows attackers using graphics cards, so this default matters for anyone scripting database creation. Opening such a database in the graphical app and changing it under Database Security fixes it.

### Exports and an independent read

`keepassxc-cli export` produced XML in the KeePass 2 format, CSV with group, title, user name, password, URL, notes, TOTP and dates, and an HTML page; each contained the test entry's password. pykeepass 4.2.0, an independent Python implementation of KDBX, opened the database with the master password and listed both test entries with their groups, user names, passwords and URLs. Exports are plain text, so they should be written only to encrypted or temporary storage.

### Password and passphrase generators

KeePassXC's two generators share one random source. On Linux it is Botan's `System_RNG`, which reads from the operating system; Botan's auto-seeded generator is used only where no system source exists. Random integers are drawn by rejection sampling, which removes modulo bias, and the character generator's "pick from every group" option shuffles the result with a Fisher-Yates shuffle driven by the same source. We read each of these in the source and found no use of a non-cryptographic generator.

Defaults:

- Character passwords: 20 characters in the graphical app, 32 in the library, from lower case, upper case, digits and symbols, with look-alike characters excluded and at least one character from every group. Well over 100 bits at the graphical defaults.
- Passphrases: 7 words, separated by spaces, lower case, from a bundled list called `eff_large.wordlist`. With 7,772 words that is 12.92 bits a word and about 90 bits for 7 words, as `keepassxc-cli diceware` and the strength meter report.

The bundled list is not the EFF long list. Compared word by word with the EFF list carried by xkcdpass, it lacks 30 EFF words and adds 26 others. The changes came in three commits: three words judged offensive were removed in 2021; in 2022 about two dozen words, including brand names such as dropbox, ebay, google, ipad, lego and xbox, were replaced with other words; and in 2025 four hyphenated words were removed. The file also drops the dice numbers. The strength cost is negligible, less than 0.001 bits a word, and the edits are reasonable, but there are two consequences. Dice rolls cannot be looked up in KeePassXC's list, and the list cannot serve as an independent copy of the EFF list for the cross-check in the uc-processes passphrase guide. A custom list, such as the original EFF file, can be loaded in the generator settings; signed lists are accepted.

The strength meter uses zxcvbn. It credited the famous "correct horse battery staple" with about 70 bits, because it does not know the phrase; like every meter, it estimates how hard a password looks, not how it was made, and it is reliable only for passwords the generator produced.

### Security record

The project's security policy names three private reporting routes, a GitHub security advisory, encrypted Matrix messages to named maintainers, or email, and asks reporters not to file CVEs before the team confirms an issue. An independent consultant reviewed 2.7.4 in 2023, free of charge, and found that KeePassXC provides sufficient cryptographic protection given a strong master key <a id="cite-alternativeto"></a>([AlternativeTo, 2023](#ref-alternativeto)). In November 2025, France's national cybersecurity agency awarded a first-level security certification to 2.7.9 on Windows, valid until 2028 ([KeePassXC Team, n.d.](#ref-kpxc-audits)).

2.8.0 lists eight security fixes: a use-after-free and an integer overflow in the legacy KDB importer, two flaws in browser integration (entry-reference injection and out-of-bounds writes in native messaging), randomized names for the shared memory used with Pageant on Windows, protected attributes leaking through Secret Service listings, a clipboard exclusion format on Windows, and better clearing of decrypted fragments from memory. Two were reported by the NATO Cyber Security Centre. The number reflects both a large attack surface, a C++ application with browser, SSH, Secret Service and import features, and active scrutiny. The risky features are off by default.

We confirmed the AppImage's SHA-256 against the published digest, but could not fetch the KeePassXC release key in the test environment, so the detached signature was not verified. The release tag is signed. An owner installing it should verify the `.sig` file against the project's published key.

### AI in development

In late 2025 the project added a policy on contributions made with generative AI, and disclosed that maintainers use GitHub Copilot for code review and to draft small, focused pull requests. After criticism, the maintainers stated that the application has no AI features and will not get any, and that every change is reviewed by a human <a id="cite-lwn"></a>([LWN.net, 2025](#ref-lwn)). Five maintainers can merge code, two of them with administrative access <a id="cite-linuxiac"></a>([Linuxiac, 2025](#ref-linuxiac)). This affects how the code is made, not what the product does, so the metrics' AI proxies do not apply. It is worth knowing for anyone whose policy restricts AI-assisted code in their dependencies.

### Packaging

Ubuntu 24.04 offers KeePassXC 2.7.6, from August 2023. Distribution maintainers may backport fixes, but owners on Ubuntu LTS who want 2.8.0's fixes should use the AppImage, Flatpak or the project's PPA. The AppImage's command-line tool needs the system's OpenGL library, which servers often lack; on a headless machine, a distribution package or Flatpak is the simpler route for scripting.

## Recommendation

Keep KeePassXC in Assess for now, and move it to Trial once a screen reader pass, with Orca on Linux and NVDA on Windows, verifies the accessibility floor for the graphical app. It is the strongest candidate so far for individuals and small offices, including non-technical people, which fits Vishpala's intended clients better than pass, gopass or passage.

For that Trial, on these conditions:

- Install 2.8.0 or later, from the AppImage, Flatpak or PPA rather than Ubuntu 24.04's archive, and verify the release signature.
- Create databases in the graphical app with KDBX 4 and Argon2, or upgrade any database created with `keepassxc-cli db-create` under Database Security before use.
- Keep the database and a key file or hardware key, if used, backed up separately, and test a restore.
- Leave browser integration, the SSH agent and Secret Service off unless needed; decide the update-check answer deliberately on first start.
- For passphrases, keep the default of 7 words; load the original EFF list if dice compatibility matters.

Re-review triggers specific to KeePassXC: a change in the command-line database defaults; a new external review or certification; a change in maintainers or the AI contribution policy; and a change to the bundled wordlist.

## License

This document, *Universal Cake Evaluation: KeePassXC, Password Manager*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### KeePassXC

- [Source repository, security policy and changelog](#ref-kpxc-source)
- [Security reviews and certifications](#ref-kpxc-audits)

### Reporting

- [2023 review, AlternativeTo](#ref-alternativeto)
- [2023 review, gHacks](#ref-ghacks)
- [AI contribution policy, LWN.net](#ref-lwn)
- [AI contribution policy, Linuxiac](#ref-linuxiac)

## References

<a id="ref-alternativeto"></a>AlternativeTo. (2023, April). *Password manager KeePassXC receives positive audit from independent security consultant*. https://alternativeto.net/news/2023/4/password-manager-keepassxc-receives-positive-audit-from-independent-security-consultant
[Return to citation](#cite-alternativeto)

<a id="ref-ghacks"></a>Brinkmann, M. (2023, April 16). *KeePassXC security audit published, recommends this security setting*. gHacks Tech News. https://www.ghacks.net/2023/04/16/keepassxc-security-audit-published-recommends-this-security-setting/
[Return to citation](#cite-ghacks)

<a id="ref-kpxc-audits"></a>KeePassXC Team. (n.d.). *Security reviews and certifications*. Retrieved November 19, 2026, from https://keepassxc.org/audits
[Return to citation](#cite-kpxc-audits)

<a id="ref-kpxc-source"></a>KeePassXC Team. (2026). *KeePassXC* (Version 2.8.0) [Computer software]. GitHub. https://github.com/keepassxreboot/keepassxc
[Return to citation](#cite-kpxc-source)

<a id="ref-linuxiac"></a>Linuxiac. (2025, November). *KeePassXC clarifies AI policy: Used only in development, never in the app*. https://linuxiac.com/keepassxc-clarifies-ai-policy-used-only-in-development-never-in-the-app/
[Return to citation](#cite-linuxiac)

<a id="ref-lwn"></a>LWN.net. (2025, November). *About KeePassXC's code quality control* [News brief]. https://lwn.net/Articles/1045807/
[Return to citation](#cite-lwn)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of KeePassXC 2.8.0 against Universal Cake Evaluation Metrics 0.6.0, with its generators, command-line network and file system behaviour, exports, database format and an independent read verified on Linux, and graphical start-up traced under a virtual display |

---
dc:title: "Universal Cake Evaluation: gopass, Password Generation and Graphical Front Ends"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "gopass"
  - "password generators"
  - "passphrase generators"
  - "graphical front ends"
dc:description: "Universal Cake evaluation of the password and passphrase generators in gopass 1.17.4, with a security review of each generator, and a light pass over the graphical and integration front ends available for gopass."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/gopasspw/gopass"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--gopass--password-manager, universal-cake-evaluation--keepassxc--password-manager, universal-cake-evaluation--xkcdpass--passphrase-generator"
dc:identifier: "universal-cake-evaluation--gopass--password-generation"
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
      Initial Assess-ring evaluation of the generators in gopass 1.17.4
      against Universal Cake Evaluation Metrics 0.6.0, scoped as a
      feature evaluation that builds on the gopass password manager
      evaluation, with a light pass over graphical front ends. Generator
      source read, outputs, defaults, network behaviour and debug logging
      verified on Linux.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # the default character generator and xkcd mode with 7 or more words, within a gopass trial; not the memorable generator; front ends assessed separately
      reasoning: "Every generator draws from Go's crypto/rand with uniform sampling, nothing reaches the network, and the default character generator gives about 143 bits at 24 characters. Weak points are in defaults and edge cases: xkcd mode defaults to 4 words, about 52 bits, and its effective separator differs from the documentation; the memorable generator gives about 57 bits at its usual length and sometimes 43; any length down to one character is accepted without warning; site rules chosen from the entry name can shorten a password, and `--force` both disables those rules and overwrites; rejected candidate passwords are written to the debug log in plain text; and the xkcd library keeps a fallback to a non-cryptographic generator, unreachable with current Go."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T02:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: gopass, Password Generation and Graphical Front Ends

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates the password and passphrase generators built into gopass, and surveys the graphical and integration front ends available for it. It builds on the *Universal Cake Evaluation: gopass, Password Manager*, which covers gopass as a whole; rows that would only repeat that evaluation are marked as such.

gopass has four generators: a character generator, used by default; an xkcd-style passphrase generator using the EFF long list; a "memorable" generator that joins short words and digits; and a hook for an external program. The three built-in generators use Go's cryptographic random source, and none reaches the network. The design is sound. The weaknesses are defaults and edge cases: a four-word default for passphrases, a memorable mode that can fall to about 43 bits, no floor on length, site rules that can shorten passwords without asking, and rejected candidates written to the debug log.

For graphical use, QtPass is the maintained option, and works with gopass stores; the gopass-specific Gopass UI has not been updated since 2023. gopass's own browser bridge and its new built-in Secret Service daemon cover browser and desktop-application use.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about the generators come from the gopass source at the `v1.17.4` tag and commit `5f4d4032` <a id="cite-gopass-source"></a>([gopass Authors, 2026](#ref-gopass-source)), its configuration reference <a id="cite-gopass-config"></a>([gopass Authors, n.d.-a](#ref-gopass-config)) and security document <a id="cite-gopass-security"></a>([gopass Authors, n.d.-b](#ref-gopass-security)); from the goxkcdpwgen library at commit `e51aa57` <a id="cite-goxkcd"></a>([Hoefling, 2023](#ref-goxkcd)); and from tests of the signed 1.17.4 Linux release. Facts about front ends come from the gopass setup guide and features list, project pages and package listings, cited where used. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate the generators of gopass 1.17.4 as reached through `gopass generate` and `gopass pwgen`: the default character generator with and without `--strict` and symbols, the xkcd generator with its separator, language, capitalization and number options, the memorable generator, the external generator hook, and the per-site password rules. Each is judged on its random source, its defaults, the strength it delivers, and how it behaves at the edges. The front-end survey is a light pass from documentation and public listings; none of the front ends was installed except gopass run under the name `pass`.

## Scorecard metadata

- Product or service name: gopass password generation (`gopass generate`, `gopass pwgen`)
- Version evaluated: gopass 1.17.4, same release archive and signature as the gopass password manager evaluation; goxkcdpwgen at commit `e51aa57` (2023-11-22), as pinned in gopass's `go.mod`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, GnuPG 2.4.4, strace 6.8, CPython 3.12 for entropy arithmetic and a simulation

## Gates

The gates apply to gopass as a whole and are recorded in the gopass password manager evaluation: all four pass, with the accessibility floor Inferred. For the generators specifically:

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | `gopass pwgen` and `gopass generate` made no internet socket calls. The per-site rules are compiled into the binary; the code that downloads them runs only when developers regenerate the file |
| A licence incompatible with the project | Pass | Verified | gopass and goxkcdpwgen are both MIT. The EFF wordlist inside goxkcdpwgen is published by the EFF under CC BY 3.0 US (claimed by the xkcdpass README, which bundles the same list); this matters only for redistribution |
| Failure of the accessibility floor | Pass | Inferred | Output is plain text; see the gopass evaluation for emoji and progress codes in other commands |
| No exit | Pass | Verified | Generated passwords are stored as ordinary entries; see the gopass evaluation |

## Scorecard

Rows that concern gopass as a whole, such as multilingual integration, sovereignty and longevity, are not repeated here; see the gopass password manager evaluation. The rows below are scored for the generators.

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified | Plain-text output. `gopass generate` does not print the password unless `-p` is given, which helps in shared spaces. Status lines begin with emoji. `pwgen` output ends each password with a trailing space; the short flag `-1` is not equivalent to `--one-per-line`, and printed 20 passwords when asked for 2 |
| Economic and cognitive accessibility | Moderate | Verified | Free. The xkcd and memorable modes produce passwords people can type and read aloud. The interactions between `generate.length`, `pwgen.xkcd-len`, site rules and `--force` are hard to predict without reading the source |
| Agency, interaction patterns | Moderate | Verified | Honest about what it stores. But site rules are applied automatically from the entry name, and when a requested length is outside a site's limits gopass asks again with the site's maximum as the default; a non-interactive run accepted that default, turning a request for 20 characters into 8 |
| Environment, direct, indirect, and authoring-side | Strong | Verified | Generation is instant and offline |
| Security, random source | Strong | Verified | The three built-in generators, character, xkcd and memorable, draw from Go's `crypto/rand` through `rand.Int`, which samples without bias; the external hook uses whatever the named program does. The xkcd library falls back to `math/rand` if `crypto/rand` returns an error, printing a warning; with gopass built on Go 1.26, `crypto/rand` does not return errors, so the fallback cannot run (inferred from the Go release notes) |
| Security, defaults and strength | Moderate | Verified | Character generator: 24 characters from 62, about 143 bits. xkcd: 4 words, about 52 bits, below the 6 to 7 words commonly advised. Memorable at 24 characters: usually 4 words, about 57 bits, and in about 2.4% of cases 3 words, about 43 bits. No minimum: a 1-character password was generated without warning |
| Security, secret handling | Moderate | Verified | Final generated passwords did not appear in the debug log. Rejected candidates did: when a site rule rejects a candidate, the candidate is logged in plain text, although the security document says secrets reach the debug log only with `GOPASS_DEBUG_LOG_SECRETS=1`. The external generator hook runs whatever command `GOPASS_EXTERNAL_PWGEN` names |
| Content endurance | Not applicable | Verified | Generators store nothing of their own |
| Adjustability and support | Strong | Verified | Lengths, word counts, separators, languages and the external hook are configurable; the code is short and readable |
| Gates | Pass | Mixed | See Gates above and the gopass evaluation |

## Findings

### The four generators

The character generator, used by default, picks each character uniformly from digits, upper case and lower case, adding 32 ASCII symbols with `-s`. `--strict` regenerates until every class is present, up to 64 tries. With the default length of 24 and no symbols, a password carries about 142.9 bits.

The xkcd generator, selected with `--generator xkcd` in `generate` or `--xkcd` in `pwgen`, picks words from a list chosen by `--xkcd-lang`. gopass passes `en` or `de`. The `en` list in goxkcdpwgen is the EFF long list: we compared its source file word by word with the EFF list carried by xkcdpass and found them identical, all 7,776 words, about 12.92 bits a word. Unlike KeePassXC's modified copy, this list matches the EFF file, so dice numbers map onto it. `--xkcd-numbers` appends a number from 0 to 10 to each word, adding about 3.46 bits a word. `--xkcd-capitalize` adds no strength, since every word is capitalized the same way.

The memorable generator joins words from the 2,048-word BIP-39 list, each followed by one random digit and, with symbols, one random symbol, until a minimum length is reached. Each word and digit pair carries about 14.32 bits. Because it stops on length rather than word count, the number of words varies with the words drawn. A simulation of 200,000 runs at the default 24 characters gave 3 words in 2.4% of runs, 4 words in 77.1%, 5 in 20.4% and 6 in 0.05%, or roughly 43, 57, 72 and 86 bits. A length target is a poor proxy for strength here.

The external generator runs the program named in `GOPASS_EXTERNAL_PWGEN` with the requested length appended and uses its output. It is useful for plugging in a tool such as xkcdpass, and it means the environment variable is part of the trust boundary.

### Defaults in practice

In a store with no generator settings, running non-interactively:

- `gopass generate --generator xkcd` produced four capitalized words joined with no separator, for example `EffectsDisarmAmendmentPummel`. The configuration reference lists a space as the default separator ([gopass Authors, n.d.-a](#ref-gopass-config)), but the code treats an unset separator as empty and then capitalizes. In an interactive terminal, gopass asks how many words to use and suggests 4.
- `gopass generate` with no length produced 24 characters; interactively it asks, suggesting 24.
- `gopass generate --strict -s` with length 4 produced `Mz#<`, and length 1 produced `u`, with no warning.

Four EFF words, about 52 bits, is enough against online guessing but weak for anything an attacker can test offline, such as a disk-encryption or SSH key passphrase. The EFF and the uc-processes passphrase guide both ask for six words or more. Setting `pwgen.xkcd-len` to 7 gives about 90 bits, the same as KeePassXC's default.

### Site password rules

gopass carries rules for 342 sites from Apple's open password-rules project, compiled into the binary and last refreshed on 2025-11-13. When an entry's name, or any folder in its path, matches a domain in that list, `gopass generate` uses the site's rules instead of the chosen generator, and says so ("Using password rules for aeon.co.jp ..."). For `t/aeon.co.jp` with a requested length of 20, the site's maximum of 8 applied; in a non-interactive run, the prompt for a new length took the maximum as its default, and the result was an 8-character password of about 47.6 bits. The rules exist because sites reject passwords that break them, so this is a reasonable feature, but it shortens passwords silently when no one answers the prompt, and it ignores `--generator xkcd`.

`--force` turns off the site rules. It also means "overwrite an existing entry without asking". A person who only wants the second may get the first.

### Debug logging

gopass's security document says secret values reach the debug log only when `GOPASS_DEBUG_LOG_SECRETS=1` is set ([gopass Authors, n.d.-b](#ref-gopass-security)). With `GOPASS_DEBUG_LOG` set and a site rule that rejected candidates, the log contained lines such as `generated invalid password "dbmvLBrp", trying again (1/64)`. Final passwords were not logged in our tests. A rejected candidate is not the password kept, so the direct risk is small, but it is a break in a documented promise and could matter if the same code path is reused. The fix is to log through gopass's secret-eliding type.

### The xkcd library

goxkcdpwgen is a small MIT library by Martin Hoefling, who also co-wrote gopass's browser bridge. Its last commit is from 2023-11-22, and gopass pins that commit. Its random function copies an early gopass helper: it uses `crypto/rand` and, if that returns an error, prints "WARNING: No crypto/rand available. Falling back to PRNG" to standard output and uses `math/rand`. Since Go 1.24, `crypto/rand` never returns an error, so with gopass 1.17.4 built on Go 1.26 the fallback is unreachable. It remains a pattern to avoid: a password tool should stop rather than fall back. The library also bundles German and short EFF lists that gopass's `--xkcd-lang` can reach by name.

## Graphical and integration front ends

gopass's setup guide notes that, because gopass stores are pass stores, pass front ends work with it, and names QtPass, the Android Password Store app and Pass for iOS, plus Gopass UI, which was written for gopass <a id="cite-gopass-setup"></a>([gopass Authors, n.d.-c](#ref-gopass-setup)). This is a light pass, not a full evaluation of each.

| Front end | What it is | Status | Fit for gopass |
|---|---|---|---|
| QtPass | Qt desktop app for pass stores on Linux, BSD, macOS and Windows; GPL-3.0 | Active: Debian accepted 1.8.4 on 2026-10-30 <a id="cite-qtpass-debian"></a>([Debian, n.d.](#ref-qtpass-debian)); translations on Weblate. Ubuntu 24.04 ships 1.4.0 (verified) | Best general option. It can call `pass` or use `gpg` and `git` directly; gopass run under the name `pass` handled `show`, `insert -m` and `generate` in our test, but its `generate` prints no password and its `ls` uses box-drawing characters, so behaviour may differ. A Qt app, so screen reader support should be similar to KeePassXC's; not tested |
| Gopass UI | Electron app written for gopass by codecentric; MIT | Last release 0.8.0; last push 2023-03-03 <a id="cite-gopassui"></a>([Ecosyste.ms, n.d.](#ref-gopassui)) | Not recommended: an Electron app bundles a browser engine, and three years without releases means three years of unpatched Chromium |
| gopassbridge with gopass-jsonapi | Browser extension for Firefox and Chromium-based browsers; MIT; talks to gopass through native messaging | Maintained by the gopass project; about 300 Firefox users (claimed by the add-on page) | Fills logins in the browser. Setup requires `gopass-jsonapi configure`; reviews describe setup as hard. Its native messaging is a privileged channel worth reviewing |
| gopass Secret Service daemon | `gopass secret-service`, new in 1.17.4, implementing the freedesktop Secret Service API | Built in (verified) | Lets desktop apps such as browsers, VS Code and NetworkManager keep their secrets in the gopass store. New code that grants other applications access; trial it separately |
| dmenu or rofi scripts | One-line pipelines from gopass's setup guide | Documented by gopass | Light and keyboard-driven; relies on `xdotool`, which does not work on Wayland |
| Android Password Store forks, Pass for iOS | Mobile pass clients | The original Android app is archived; community forks continue; see the pass evaluation | Read gopass stores with the GnuPG backend; mind the OCB AEAD issue described in the pass evaluation |
| Void and other new wrappers | Small Electron wrappers around the gopass CLI | One found with a single star | Not assessed; too new and too small to trust with a password store |

A naming caution again: `cortex/gopass` on GitHub is an unrelated, unmaintained pass GUI from 2016, whose author moved on to Ripasso. It is not a front end for this gopass.

For people who need a graphical password manager first and foremost, KeePassXC, evaluated alongside this document, is a more complete choice than any gopass front end; gopass's strengths are team stores and git workflows.

## Recommendation

Within a gopass Trial, use the generators on these conditions:

- Keep the default character generator for site passwords, with a length of at least 20.
- For passphrases, use xkcd mode with `pwgen.xkcd-len` set to 7 and `pwgen.xkcd-sep` set explicitly, for example to a space or a hyphen, so the result matches the documentation.
- Avoid the memorable generator for anything that matters, or ask for 32 characters or more.
- Name entries knowing that site rules follow the name; check the length reported after generation, and use `--force` only when both of its effects are wanted.
- Do not enable `GOPASS_DEBUG_LOG` on machines where logs are kept or shared.
- Set `GOPASS_EXTERNAL_PWGEN` only to a reviewed program.

For a graphical front end, trial QtPass from a current package, not Ubuntu 24.04's 1.4.0; do not use Gopass UI. Trial the Secret Service daemon and the browser bridge separately, each as its own radar entry, since both open the store to other programs.

Reporting the separator mismatch, the debug-log leak and the dual meaning of `--force` upstream would improve gopass for everyone.

Re-review triggers specific to the generators: a change to the xkcd default word count or separator; an update of goxkcdpwgen or a move away from it; a refresh of the site rules; and a fix for candidate logging.

## License

This document, *Universal Cake Evaluation: gopass, Password Generation and Graphical Front Ends*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### gopass

- [Source repository](#ref-gopass-source)
- [Configuration reference](#ref-gopass-config)
- [Security, known limitations and caveats](#ref-gopass-security)
- [Setup guide, including graphical front ends](#ref-gopass-setup)
- [goxkcdpwgen](#ref-goxkcd)

### Front ends

- [QtPass in Debian](#ref-qtpass-debian)
- [Gopass UI project metadata](#ref-gopassui)

## References

<a id="ref-qtpass-debian"></a>Debian. (n.d.). *qtpass* [Package tracker]. Retrieved November 19, 2026, from https://tracker.debian.org/qtpass
[Return to citation](#cite-qtpass-debian)

<a id="ref-gopassui"></a>Ecosyste.ms. (n.d.). *codecentric/gopass-ui* [Project metadata]. Retrieved November 19, 2026, from https://awesome.ecosyste.ms/projects/github.com%2Fcodecentric%2Fgopass-ui
[Return to citation](#cite-gopassui)

<a id="ref-gopass-config"></a>gopass Authors. (n.d.-a). *Configuration* [Documentation]. GitHub. Retrieved November 19, 2026, from https://github.com/gopasspw/gopass/blob/master/docs/config.md
[Return to citation](#cite-gopass-config)

<a id="ref-gopass-security"></a>gopass Authors. (n.d.-b). *Security, known limitations, and caveats* [Documentation]. GitHub. Retrieved November 19, 2026, from https://github.com/gopasspw/gopass/blob/master/docs/security.md
[Return to citation](#cite-gopass-security)

<a id="ref-gopass-setup"></a>gopass Authors. (n.d.-c). *Setup* [Documentation]. GitHub. Retrieved November 19, 2026, from https://github.com/gopasspw/gopass/blob/master/docs/setup.md
[Return to citation](#cite-gopass-setup)

<a id="ref-gopass-source"></a>gopass Authors. (2026). *gopass* (Version 1.17.4) [Computer software]. GitHub. https://github.com/gopasspw/gopass
[Return to citation](#cite-gopass-source)

<a id="ref-goxkcd"></a>Hoefling, M. (2023). *goxkcdpwgen* [Computer software]. GitHub. https://github.com/martinhoefling/goxkcdpwgen
[Return to citation](#cite-goxkcd)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of the generators in gopass 1.17.4 against Universal Cake Evaluation Metrics 0.6.0, with source, outputs, defaults, network behaviour and debug logging verified on Linux, and a light pass over graphical and integration front ends |

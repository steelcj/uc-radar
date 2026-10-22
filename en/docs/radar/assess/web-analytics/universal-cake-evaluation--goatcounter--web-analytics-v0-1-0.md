---
dc:title: "Universal Cake Evaluation: GoatCounter, Web Analytics"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "goatcounter"
  - "web analytics"
  - "privacy"
dc:description: "Universal Cake evaluation of GoatCounter 2.7.0, the open source, privacy-oriented web analytics platform, both self-hosted and as the goatcounter.com hosted service."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-08"
dcterms:modified: "2026-10-08"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/arp242/goatcounter"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, privacy-oriented-website-analytics"
dc:identifier: "universal-cake-evaluation--goatcounter--web-analytics"
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
      Initial Assess-ring evaluation of GoatCounter 2.7.0 against
      Universal Cake Evaluation Metrics 0.5.1, covering the self-hosted
      binary, tested on Linux, and the goatcounter.com hosted service,
      assessed from its published policies.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # self-hosted, once the accessibility floor is verified with assistive technology
      reasoning: "All four default gates pass; telemetry, licence and exit Verified for the self-hosted binary. Data collection is minimal by default and no outbound connections were observed. Main concerns are that the in-memory session map, which holds raw IP addresses and User-Agents, is written to the database on clean shutdown despite documentation saying these are never stored to disk; no published security policy or release checksums; a single maintainer (96% of all commits); and two settings-form labels not associated with their inputs."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-08T11:52:00-04:00"
  human: []
---

# Universal Cake Evaluation: GoatCounter, Web Analytics

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates GoatCounter, an open source web analytics platform offered both as a self-hosted application and as the free, donation-supported goatcounter.com service, against the Universal Cake Evaluation Metrics. It records the gate results, a scorecard, the findings that shaped the ratings, and a recommendation for the radar. GoatCounter passes all four default gates and is unusually strong on data minimisation, localisation, and exit. Testing turned up one material gap between what the documentation promises and what the software does: raw IP addresses and User-Agent strings are written to the database when the server shuts down cleanly. The other risks are the ones common to small projects: one maintainer, no published security policy, and release binaries with no checksums.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about GoatCounter come from files in the GoatCounter repository, read directly at commit `c031008a` and at the `v2.7.0` tag on 2026-10-08, from the published 2.7.0 Linux binary, run and traced locally, and from the goatcounter.com privacy policy, terms, and help pages. The radar's earlier survey of privacy-oriented analytics placed GoatCounter first in the evaluation queue; this entry is that evaluation. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate GoatCounter 2.7.0, the current release, in its two forms:

- **Self-hosted.** The statically linked `goatcounter-v2.7.0-linux-amd64` release binary with an SQLite database, run and traced on Linux.
- **Hosted.** The goatcounter.com service, which its changelog says generally runs the latest commit on the main branch. The hosted service was assessed from its published policies only; we did not create an account.

Where the two forms differ, we record both. The measurement script, `count.js`, is common to both and was read from source.

The lenses matter more here than for most tools. The owner is whoever runs the website being measured; the users whose data is processed are that website's visitors, who never interact with GoatCounter directly and usually do not know it is there.

## Scorecard metadata

- Product or service name: GoatCounter
- Version evaluated: 2.7.0, `goatcounter-v2.7.0-linux-amd64.gz`, SHA-256 `98d221cb9c8ef2bf76d8daa9cca647839f8d8b0bb5bc7400ff9337c5da834511`; goatcounter.com as described by its policies on 2026-10-08
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-08
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, SQLite database

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | The self-hosted server made no outbound connections during the trace, see Findings. Its only outbound features, email, ACME certificates, and MaxMind GeoIP updates, are each off until configured. The help pages contain a leftover script tag reporting to goatcounter.com, which the server's own Content Security Policy blocks |
| A licence incompatible with the project | Pass | Verified | EUPL-1.2 with a modified Compatible Licences appendix that keeps AGPL-3.0 and OSL 2.1 and 3.0; `count.js` is ISC. Compatible with AGPL-3.0-or-later projects. The modification itself is unusual, see Findings |
| Failure of the accessibility floor | Pass | Inferred | Statistics are presented as HTML tables alongside the charts, the page declares its language, and form fields are labelled. Not tested with a screen reader or keyboard. Two settings labels are not associated with their inputs |
| No exit | Pass | Verified | Self-hosted data is an ordinary SQLite or PostgreSQL database, read directly during testing. CSV export exists in 2.7.0 and JSON export on main; the hosted service offers the same exports (claimed) |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Verified, Claimed | The project calls accessibility a high priority and says the interface works with screen readers (claimed). Markup review of the 2.7.0 dashboard: `lang="en"`, labelled login fields, `aria-label` on icon buttons, statistics in tables beside `canvas` charts (verified). No `main` landmark and no skip link; two settings labels point at the wrong `id` (verified). Not tested with assistive technology |
| Multilingual integration | Strong | Verified | 27 interface translations, stored as TOML data files and editable by any account holder through a built-in translation page. French covers about 96% of entries; it is `fr-FR`, not a Canadian French variety |
| Economic and cognitive accessibility | Strong | Verified, Claimed | Self-hosting is free; the hosted service is free for reasonable public use (claimed). `count.js` is 3,246 bytes gzipped (verified), and a JavaScript-free pixel and log-file import exist for visitors and sites that cannot run scripts. The dashboard is deliberately small |
| Representation | Unknown | None | Not assessable from public information. One maintainer; nothing public on who was consulted. No machine-learned models in the product |
| Compatibility | Moderate | Verified | Release binaries for Linux amd64 and arm64 and FreeBSD amd64 only, plus Docker images for amd64 and arm64. Other platforms must build from source, which needs cgo for SQLite. The tracker works in any browser, including with JavaScript off via the pixel |
| Resilience | Strong | Verified, Inferred | The tracker loads asynchronously, so a GoatCounter outage does not break the measured site (inferred). Self-hosted, it is one binary and one database file, with a GeoIP country database built in; it needs no external service to run (verified) |
| Agency, sovereignty and privacy | Moderate | Verified | Strong by default on what it collects: no cookies for visitors, individual pageviews off, and IP addresses absent from all statistics tables (verified). But on clean shutdown the in-memory session map, raw User-Agent plus IP, is written to the database's `store` table and kept until the next start, contrary to the documentation (verified), see Findings. Hosted: one operator in Ireland, servers at Hetzner in Finland and Germany (claimed). Owner lens Strong when self-hosted; visitor lens Moderate |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified, Claimed | Exit cost is low: export, then import into a self-hosted instance, or keep the database. Portability: CSV is both machine- and human-readable. Terms last changed January 2020 and require two weeks' email notice of changes (claimed). Pricing is public: free, donations invited |
| Agency, interaction patterns | Strong | Verified | No cookies, banners, or nags. Donation links sit in the footer only. Visitor lens: the script does not honour Do Not Track or Global Privacy Control; owners can exclude their own visits |
| Environment, direct, indirect, and authoring-side | Strong | Verified, Inferred | 3.2 KB script per page view, or a pixel. Self-hosted, a single 29 MB binary on SQLite suits small hardware. Individual hits are not stored by default, only hourly and daily aggregates (verified), so storage grows slowly |
| Security | Moderate | Verified | Strict Content Security Policy on all dashboard pages (verified). No published security policy or reporting address in the repository (verified absence). No CVEs found (inferred from search). Release binaries ship without checksums or signatures; the release commit carries an SSH signature, not checked locally. 44 Go modules, 24 of them the maintainer's own. Patch speed not measured |
| Longevity | Moderate | Verified, Claimed | In development since 2019, 29 commits in the last 90 days, 334 in the last two years (verified). Releases are infrequent: 2.5.0 in December 2023, 2.6.0 in June 2025, 2.7.0 in December 2025. The hosted service is funded by GitHub Sponsors donations to one person (claimed) |
| Content endurance | Strong | Inferred | An enhancement layer: removing the script leaves the site whole. Collected statistics live in a standard database the owner controls when self-hosted |
| Exit and portability | Strong | Verified | See the No exit gate. Log-file import also lets an owner rebuild statistics from their own web server logs |
| Adjustability and support | Moderate | Verified, Inferred | Copyleft keeps the fork-and-fix path open; the Go code is compact; built-in help is thorough. Support is GitHub issues and one maintainer |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | A small player against Google Analytics, with no take rate. The API is versioned as `v0`. Forkable by licence, but a fork inherits 24 of the maintainer's own modules. Contributor concentration: 96% of all non-merge commits and 94% over the last two years are by the maintainer (verified). No foundation-model dependency |
| Gates | Pass | Mixed | All four pass. Telemetry, licence, and exit Verified; accessibility floor Inferred |

## Findings

### Network behaviour of the self-hosted server

We created a site with the 2.7.0 binary and ran `goatcounter serve` under `strace`, recording every `connect` system call. During the run we loaded the login, dashboard, and help pages, logged in, and recorded a pageview carrying a referrer, campaign, screen size, and a public IP address in `X-Forwarded-For`, which the built-in GeoIP database resolved to a country. The trace recorded no outbound connections at all.

Source inspection agrees. The only code paths that reach the network are sending email through an operator-configured SMTP relay, requesting certificates through ACME when `-tls acme` is set, and downloading MaxMind's city database when a MaxMind licence key is given to `-geodb` <a name="apa-gc-readme-citation"></a>([GoatCounter Project, n.d.-b](#apa-gc-readme-reference)). Each is off until configured.

One leftover is worth reporting upstream. Outside development mode, the help-page template includes GoatCounter's own `count.js` from `gc.zgo.at`, set to report to `goatcounter.goatcounter.com` <a name="apa-gc-helptpl-citation"></a>([GoatCounter Project, n.d.-e](#apa-gc-helptpl-reference)). That tag is served on self-hosted instances too. The server's Content Security Policy limits scripts to the instance's own origin, so a conforming browser will refuse to load it, and we observed that header on every page; we did not confirm the browser's behaviour directly. It does not fail the telemetry gate, but it relies on the policy to stop something the template should not ask for.

### What is stored, and the shutdown exception

With the default settings, a pageview produced rows only in aggregate tables: hourly and daily counts, browser, operating system, screen size, country, referrer, and campaign. The table of individual pageviews stayed empty, since storing them is off by default, and no statistics table has a column for an IP address or a full User-Agent string.

The documentation describes how unique visits are counted: the site, User-Agent, and IP address are combined into a key, mapped in memory to a random identifier for eight hours, and it states plainly that the IP address and User-Agent are never stored to the database or disk <a name="apa-gc-sessions-citation"></a>([GoatCounter, n.d.-b](#apa-gc-sessions-reference)). The privacy policy says the same about sessions <a name="apa-gc-privacy-citation"></a>([GoatCounter, 2025](#apa-gc-privacy-reference)).

That is not true of a clean shutdown. When the server stops, it serialises the whole session map, raw keys included, into the `store` table, and on the next start it reads that row back and deletes it <a name="apa-gc-memstore-citation"></a>([GoatCounter Project, n.d.-c](#apa-gc-memstore-reference)). After stopping our test server, the database file contained the test IP address and the full User-Agent string. Between a shutdown and the next start, which could be minutes or months, any backup, copy, or snapshot of the database carries visitors' IP addresses. After deletion, SQLite may also leave the bytes in free pages until the file is vacuumed.

The design reason is clear: preserving sessions across a restart avoids counting returning visitors twice. The fix is also clear: store only the random identifiers and a keyed hash of each session key, or accept slightly inflated visit counts after a restart. Until then, the documentation's claim should be treated as Claimed, not Verified, and self-hosters should handle the database as containing personal data whenever the server is stopped. This is exactly the case the metrics document's principle to measure reversibility, not promises, exists for.

### Licence

GoatCounter uses the European Union Public Licence 1.2 with one change: the appendix of Compatible Licences is cut down to the GNU Affero General Public License 3.0 and the Open Software License 2.1 and 3.0 <a name="apa-gc-license-citation"></a>([GoatCounter Project, n.d.-a](#apa-gc-license-reference)). The EUPL is a copyleft licence whose obligations extend to software made available over a network, so its effect is close to the AGPL's, and the trimmed appendix keeps the AGPL as an outbound path. For Universal Cake's AGPL-3.0-or-later work this is compatible.

Two cautions. The EUPL's own appendix says changes other than the Commission's updates require a new EUPL version, so a modified appendix sits awkwardly with the licence's own terms, and the result is not the OSI-approved EUPL text as published. This does not matter for running GoatCounter unmodified. It might matter for redistributing a modified version, which is worth confirming with whoever handles licence review before Vishpala ships a fork. The radar's earlier survey listed the licence simply as EUPL-1.2, which should be refined. The tracking script, the only part embedded in client sites, is ISC, so including it raises no copyleft question.

### Accessibility evidence

The 2.7.0 dashboard markup has good foundations: a declared language, labelled login fields, text labels on icon-only buttons, and every chart accompanied by a table of the same numbers, which matters because the charts are drawn on `canvas` and carry no text alternative of their own. It lacks a `main` landmark and a skip link. In the site settings form, the label for data retention points at `data_retention` while the input's `id` is `limits_page`, and the label for dashboard visibility points at `settings.public` while the select's `id` is `settings-public` <a name="apa-gc-settings-citation"></a>([GoatCounter Project, n.d.-d](#apa-gc-settings-reference)). A screen reader user tabbing to either field hears no label. The first is still present on the main branch. Both are one-line fixes worth sending upstream.

### Localisation

Translations are TOML files, one per language, and the help pages invite any logged-in account holder to edit or add them through a translation page in the application <a name="apa-gc-translating-citation"></a>([GoatCounter Project, n.d.-f](#apa-gc-translating-reference)). The repository holds 27 translations; counting entries against the English template, French is about 96% complete, Dutch about 93%, and German, Spanish, and Japanese about 80%. This is the kind of localisation the metrics ask for: a data file a community can supply, not a code change only maintainers can make.

### Hosted service terms and operator

The hosted service is operated from the Republic of Ireland and is free for reasonable public use, which the terms illustrate as personal sites and small to medium businesses but not millions of pageviews a day. Changes to the terms require two weeks' email notice, and the terms were last changed in January 2020 <a name="apa-gc-terms-citation"></a>([GoatCounter, 2020](#apa-gc-terms-reference)). The privacy policy names Hetzner servers in Finland and Germany as the only third party and was last changed in June 2025. Funding is voluntary contributions through GitHub Sponsors <a name="apa-gc-contribute-citation"></a>([GoatCounter, n.d.-a](#apa-gc-contribute-reference)). The terms name no legal entity, only the service.

### Supply chain and maintenance

The 2.7.0 release offers three binaries, for Linux amd64 and arm64 and FreeBSD amd64, alongside source archives, with no checksum or signature files <a name="apa-gc-release-citation"></a>([GoatCounter Project, 2025](#apa-gc-release-reference)). The release commit is SSH-signed. The binaries do not appear to be built in CI, which builds only the Docker images, so a consumer cannot tie a downloaded binary to a reproducible build; building from the signed tag avoids this. Of 44 Go modules, 24 are the maintainer's own `zgo.at` modules, many pinned to untagged commits, which concentrates both review and risk in one person.

The changelog lists features and major changes but not security fixes as a category <a name="apa-gc-changelog-citation"></a>([GoatCounter Project, n.d.-g](#apa-gc-changelog-reference)), and the repository has no security policy, so there is no stated way to report a vulnerability privately.

## Recommendation

Keep GoatCounter in Assess, and move the self-hosted form to Trial once the accessibility floor is Verified with a screen reader and keyboard pass over the dashboard and settings. For Trial and beyond:

- Self-host it, ideally in Canada for Quebec clients, so visitor data never leaves infrastructure the owner controls.
- Serve `count.js` from the instance's own domain rather than `gc.zgo.at`, so visitors' browsers contact only the owner's server.
- Treat the database as containing personal data whenever the server is stopped, and keep it out of backups taken between a shutdown and the next start, or run `VACUUM` after start-up, until the session-storage behaviour changes upstream.
- Report upstream: the session-storage discrepancy, the two unassociated labels, the help-page script tag, and the absence of a security policy and release checksums.
- Set a data retention period per site rather than relying on the default of keeping data indefinitely.
- Build from the SSH-signed release tag, or pin the binary by the checksum recorded in this entry.

The hosted service is reasonable for personal and small sites where EU hosting by one individual is acceptable to the owner. It should not be the default for Vishpala clients, since it adds a cross-border transfer and a single-person operator to the relationship.

Re-review triggers specific to GoatCounter: a change in how sessions are persisted, any change to the licence or its appendix, a change of operator for the hosted service, and the first published security advisory.

## License

This document, *Universal Cake Evaluation: GoatCounter, Web Analytics*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### GoatCounter service

- [Contribute financially](#apa-gc-contribute-reference)
- [Sessions and visitors](#apa-gc-sessions-reference)
- [Terms of service](#apa-gc-terms-reference)
- [Privacy policy](#apa-gc-privacy-reference)

### GoatCounter repository

- [Licence](#apa-gc-license-reference)
- [README](#apa-gc-readme-reference)
- [Session store, memstore.go](#apa-gc-memstore-reference)
- [Site settings template](#apa-gc-settings-reference)
- [Help page template](#apa-gc-helptpl-reference)
- [Translating GoatCounter](#apa-gc-translating-reference)
- [Changelog](#apa-gc-changelog-reference)
- [Release v2.7.0](#apa-gc-release-reference)

## References

<a name="apa-gc-contribute-reference"></a>GoatCounter. (n.d.-a). *Contribute financially*. https://www.goatcounter.com/contribute
[Return to citation](#apa-gc-contribute-citation)

<a name="apa-gc-sessions-reference"></a>GoatCounter. (n.d.-b). *Sessions and visitors*. https://www.goatcounter.com/help/sessions
[Return to citation](#apa-gc-sessions-citation)

<a name="apa-gc-terms-reference"></a>GoatCounter. (2020). *Terms of service*. https://www.goatcounter.com/terms
[Return to citation](#apa-gc-terms-citation)

<a name="apa-gc-privacy-reference"></a>GoatCounter. (2025). *Privacy policy*. https://www.goatcounter.com/privacy
[Return to citation](#apa-gc-privacy-citation)

<a name="apa-gc-license-reference"></a>GoatCounter Project. (n.d.-a). *LICENSE* [Software licence]. GitHub. https://github.com/arp242/goatcounter/blob/main/LICENSE
[Return to citation](#apa-gc-license-citation)

<a name="apa-gc-readme-reference"></a>GoatCounter Project. (n.d.-b). *README.md*. GitHub. https://github.com/arp242/goatcounter/blob/main/README.md
[Return to citation](#apa-gc-readme-citation)

<a name="apa-gc-memstore-reference"></a>GoatCounter Project. (n.d.-c). *memstore.go* [Source code]. GitHub. https://github.com/arp242/goatcounter/blob/main/memstore.go
[Return to citation](#apa-gc-memstore-citation)

<a name="apa-gc-settings-reference"></a>GoatCounter Project. (n.d.-d). *settings_main.gohtml* [Source code]. GitHub. https://github.com/arp242/goatcounter/blob/main/tpl/settings_main.gohtml
[Return to citation](#apa-gc-settings-citation)

<a name="apa-gc-helptpl-reference"></a>GoatCounter Project. (n.d.-e). *help.gohtml* [Source code]. GitHub. https://github.com/arp242/goatcounter/blob/main/tpl/help.gohtml
[Return to citation](#apa-gc-helptpl-citation)

<a name="apa-gc-translating-reference"></a>GoatCounter Project. (n.d.-f). *translating.md* [Help page source]. GitHub. https://github.com/arp242/goatcounter/blob/main/tpl/help/translating.md
[Return to citation](#apa-gc-translating-citation)

<a name="apa-gc-changelog-reference"></a>GoatCounter Project. (n.d.-g). *CHANGELOG.md*. GitHub. https://github.com/arp242/goatcounter/blob/main/CHANGELOG.md
[Return to citation](#apa-gc-changelog-citation)

<a name="apa-gc-release-reference"></a>GoatCounter Project. (2025). *GoatCounter v2.7.0* [Software release]. GitHub. https://github.com/arp242/goatcounter/releases/tag/v2.7.0
[Return to citation](#apa-gc-release-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of GoatCounter 2.7.0 against Universal Cake Evaluation Metrics 0.5.1, covering the self-hosted binary, tested on Linux, and the goatcounter.com hosted service, assessed from its published policies |

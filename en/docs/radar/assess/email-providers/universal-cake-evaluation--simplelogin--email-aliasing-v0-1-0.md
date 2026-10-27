---
dc:title: "Universal Cake Evaluation: SimpleLogin, Email Aliasing"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "simplelogin"
  - "email aliasing"
  - "privacy"
  - "self-hosting"
  - "proton"
dc:description: "Universal Cake evaluation of SimpleLogin 4.83.2, an AGPL-3.0 email alias service owned by Proton AG, as both the hosted service at simplelogin.io and a self-hosted server."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-26"
dcterms:modified: "2026-10-26"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/simple-login/app"
dc:relation: "universal-cake-evaluation-metrics, beluga-universal-cake-evaluation, northmail-preliminary-universal-cake-evaluation"
dc:identifier: "universal-cake-evaluation--simplelogin--email-aliasing"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/email-aliasing/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-26"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of SimpleLogin 4.83.2 against
      Universal Cake Evaluation Metrics 0.5.1, from the repository
      source at main dc4130a (2026-10-26), its git history for the year
      to 2026-10-26, its security policy, the simplelogin.io privacy
      policy, Proton's announcements and a third-party pricing summary.
      SimpleLogin was not installed, run or signed up for.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # hosted service with a custom domain is trial-ready for personal use; self-hosting needs its own work first
      reasoning: "AGPL-3.0, an active codebase from a well-established privacy company, no email storage after delivery, short log retention, and a self-hostable server with no telemetry unless the operator configures it. Aliases can be exported as CSV and JSON, and with a custom domain the user keeps the addresses when leaving. Against that: aliases on SimpleLogin's own domains cannot leave with the user, the hosted web app runs cookieless Plausible analytics with no in-app switch, PGP protection of forwarded mail is premium only, the web interface is English only, the self-hosting guide is years out of date, and the server's lock file pins Flask 1.1.2, SQLAlchemy 1.3.24 and gunicorn 20.0.4, all long past support. Two Proton engineers wrote over 90% of the past year's commits."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-26T13:00:00-04:00"
  human: []
---

# Universal Cake Evaluation: SimpleLogin, Email Aliasing

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

SimpleLogin gives each website or contact its own email address, an alias, that forwards to the user's real mailbox and lets the user reply without revealing it. An alias that starts receiving spam, or that a company sells, can be switched off without touching the real address. SimpleLogin joined Proton in 2022 and is offered as a hosted service at simplelogin.io, with browser extensions and mobile apps, and as an AGPL-3.0 server anyone can host. This entry evaluates release 4.83.2 in both forms, for personal use and as something Vishpala might recommend or host for clients. It passes all four default gates, one of them narrowly for the hosted service. Its strengths are a clear privacy posture, an open licence and a real exit for users with their own domain. Its weaknesses are lock-in for aliases on SimpleLogin's domains, an English-only interface, a stale self-hosting guide and old server dependencies. We recommend keeping it in Assess, with a trial of the hosted service on a custom domain.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts come from the SimpleLogin repository at commit `dc4130a` on main, 2026-10-26, matching tag v4.83.2 <a id="cite-sl-repo"></a>([SimpleLogin, 2026](#ref-sl-repo)); its `SECURITY.md` <a id="cite-sl-security"></a>([SimpleLogin, n.d.-b](#ref-sl-security)); the simplelogin.io privacy policy, last updated 2025-09-15 <a id="cite-sl-privacy"></a>([SimpleLogin, n.d.-a](#ref-sl-privacy)); Proton's announcement of SimpleLogin joining it <a id="cite-proton-join"></a>([Proton, 2022](#ref-proton-join)); a report of the Proton Foundation becoming Proton's primary shareholder <a id="cite-startupticker"></a>([Startupticker.ch, 2024](#ref-startupticker)); and a third-party summary of current plans <a id="cite-toolradar"></a>([Toolradar, n.d.](#ref-toolradar)), tagged Claimed. Source facts were read on 2026-10-26. Nothing was installed or run, no account was created, and no traffic was traced. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

The web application and mail server in `simple-login/app`, in two deployments: the hosted service operated by Proton AG, and a self-hosted instance following the repository's guide. The browser extensions, mobile apps and Proton Pass integration live in other repositories and are out of scope, except where the hosted service depends on them.

Where the two deployments diverge, both answers are given, per the stakeholder lenses: the hosted service is mostly a user question, self-hosting mostly an owner question.

## Scorecard metadata

- Product or service name: SimpleLogin
- Version evaluated: v4.83.2 (tag of 2026-10-26); repository `simple-login/app`, main at `dc4130a`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-26
- Radar ring: Assess
- Test platform: none; source, git history and public documents only

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Self-hosted: Pass. Hosted: Pass, narrowly | Verified (source), Claimed (policy) | Self-hosted: Sentry, Plausible and New Relic run only when the operator sets their keys, and `static/js/an.js` loads Plausible only on `simplelogin.io` hosts. Hosted: the web app loads Plausible through a first-party proxy, which the policy describes as cookieless and free of personal data. There is no setting to turn it off in the app (a `localStorage` flag exists but no control was found), so it is disabled only by browser blocking. Content exposure: forwarded mail passes through Proton's servers in readable form unless the mailbox has a PGP key, which is premium only. This is inherent to forwarding, and the policy says mail is deleted after delivery. Christopher's call whether these clear the gate; see Findings |
| A licence incompatible with the project | Pass | Verified | AGPL-3.0 (the front-end `package.json` says MIT; the repository licence governs) |
| Failure of the accessibility floor | Pass | Inferred | Server-rendered HTML forms on a Bootstrap-based theme, with `lang="en"`, 87 `<label>` elements for 195 inputs and 54 `aria-label`s. No `sr-only` text, icon fonts used for some controls. Not tested with a screen reader |
| No exit | Pass | Verified (source) | Settings offer alias export; the API exposes `/api/export/data` (JSON: aliases, custom domains, apps) and `/api/export/aliases` (importable CSV). Mail is not stored, so there is no mail to export. Aliases on SimpleLogin's domains cannot move; see Exit cost |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Plain server-rendered forms are a good base. Half the inputs lack an associated `<label>` in the templates (some are labelled by placeholder or `aria-label`), and icon-only controls depend on icon fonts. No accessibility statement found |
| Multilingual integration | Weak | Verified | The web app has no translation framework (no Babel or gettext); every template is English. Adding French would be a code change across 164 templates, not a data file |
| Economic and cognitive accessibility | Moderate | Claimed, Inferred | Free plan with 10 aliases and one mailbox; Premium about 36 USD a year (Claimed). The alias idea is simple once understood, but replying through reverse aliases, mailboxes and custom domains takes explanation, especially for older users |
| Hardware and connectivity accessibility | Strong | Inferred | A light server-rendered web app, extensions and mobile apps. Forwarding happens on the server, so the user's device and connection do not matter. Self-hosting needs a small Linux server with about 2 GB of RAM and port 25 open |
| Representation | Unknown | None | Not assessable from public information. No machine-learned models are involved, so the AI proxies do not apply |
| Compatibility | Strong | Inferred | Works with any mailbox provider and any mail client, since it is plain forwarding; extensions for Chrome, Firefox and Safari, apps for Android and iOS |
| Resilience | Moderate | Inferred | When SimpleLogin is down, mail to aliases is delayed or bounced, a new point of failure in front of every alias. Self-hosting moves that point to the owner's server, which then has to be kept reachable on port 25 |
| Agency, sovereignty and privacy | Self-hosted: Strong. Hosted: Moderate | Verified (source), Claimed (policy) | Hosted: metadata (sender, recipient, IP, subject, times) is kept; Postfix and Nginx logs are deleted after seven days; processors include Proton, UpCloud, Paddle, hCaptcha, Cloudflare, Zendesk and Plausible; operated from Switzerland with data centres in Germany, Switzerland, the Netherlands and France. Self-hosted: everything stays with the owner, at the cost of running a mail server |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Moderate | Inferred, Claimed | Exit cost depends on the domain: with the user's own domain, changing MX records moves every alias in minutes; with SimpleLogin's domains, each alias must be changed by hand at every site that uses it, hours for a few dozen aliases. Portability: machine-readable Yes (JSON, CSV); human-readable Yes for the CSV. Pricing is public. Terms history was not diffed |
| Agency, interaction patterns | Moderate | Inferred | Honest core design: an alias can be switched off with one click and the user is told what each alias received. Free-plan limits are stated in advance (Claimed). PGP, the strongest privacy feature, sits behind the paid plan |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | One forwarding hop per message on shared servers, no client-side load, no new hardware. Self-hosting adds one small always-on server |
| Security | Moderate | Verified, Inferred | Strengths: TOTP and WebAuthn for accounts, DKIM, SPF and DMARC handling, optional PGP, reports go to Proton's bug bounty programme. Weaknesses: the image is built from `uv.lock`, which pins Flask 1.1.2, SQLAlchemy 1.3.24 and gunicorn 20.0.4; gunicorn versions before 22.0.0 and 23.0.0 have published request-smuggling advisories (CVE-2024-1135, CVE-2024-6827); whether they are reachable behind the documented Nginx setup was not assessed. Front-end uses Vue 2, past end of life. Only the latest minor version gets security fixes. The self-hosting guide generates 1024-bit DKIM keys |
| Longevity | Strong | Verified, Claimed | Backed by Proton AG since April 2022; the Proton Foundation became Proton's primary shareholder in June 2024. 306 commits in the year to 2026-10-26 and releases about monthly, v4.81.3 in June to v4.83.2 in October |
| Content endurance | Strong | Inferred | SimpleLogin holds no content: mail lands in the user's own mailbox. What it holds is the address mapping, which the CSV export captures |
| Exit and portability | Moderate | Verified (source), Inferred | An exit exists and is cheap with a custom domain. On shared domains the addresses themselves stay behind |
| Adjustability and support | Moderate | Inferred | AGPL-3.0, readable Python and Jinja templates, public issues and discussions. But the self-hosting guide still targets Ubuntu 18.04 and image `simplelogin/app:3.4.0`, while current builds go to `simplelogin/app-ci` at 4.83.x, so self-hosters work from community knowledge |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | One of a few alias services (addy.io, Firefox Relay, Apple's Hide My Email, DuckDuckGo). Bundled with Proton plans, which strengthens Proton's position. Contributor concentration is very high: 15 author addresses in the year, 89% of commits from `@proton.ch` or `@simplelogin.io` addresses, and two engineers made over 91%. Forkable by licence; a fork would inherit the old dependency stack |
| Gates | Pass | Mixed | All four pass; the hosted telemetry and content gate narrowly, as noted |

Row changes from the template: Hardware and connectivity accessibility is split out of Economic and cognitive accessibility, and two rows carry separate hosted and self-hosted answers.

## Findings

### Who owns the address decides the exit

The reversibility test gives two very different answers. A user who creates aliases on their own domain can leave SimpleLogin by pointing that domain's MX records at another forwarder or mail host, and every alias keeps working. A user who creates aliases on SimpleLogin's shared domains has given those addresses to Proton: the export lists them, but they cannot be taken elsewhere, and leaving means changing the address at every service that uses one. Custom domains are a premium feature (Claimed). For Vishpala clients, the advice follows directly: use SimpleLogin only with a domain the client owns.

### The hosted gate is a judgment call

Two facts sit close to the telemetry and content gate for the hosted service. First, the web app runs Plausible, proxied through SimpleLogin's own domain, with no switch in the interface. Plausible counts page views and a few named events, such as completed registration, without cookies or personal data according to the privacy policy <a id="cite-sl-privacy-2"></a>([SimpleLogin, n.d.-a](#ref-sl-privacy)). Second, any forwarder must read mail to forward it. SimpleLogin's answer is PGP: with a key on the mailbox, it encrypts mail before forwarding, so the copy at rest in the user's mailbox is protected. That option is premium only. Neither is the kind of exposure the gate exists to catch, but neither can be switched off by a free user, so this entry records a narrow pass and leaves the call to the human reviewer.

### Self-hosting works from an old map

The server itself is current and self-hosting stays fully featured, since there is no paid tier to unlock: the guide makes an account premium with one SQL update. But the guide still assumes Ubuntu 18.04, which reached end of standard support in 2023, pins `simplelogin/app:3.4.0`, and calls 3.4.0 the latest version in `docs/upgrade.md`. Current images are published by CI to `simplelogin/app-ci`. Anyone hosting it would need to write their own deployment, with current Postfix, TLS and DKIM settings (2048-bit keys rather than the guide's 1024), and treat the README as background only.

### An old dependency stack under an active project

Development is steady, but the Python stack has not moved: Flask 1.1.2 from 2020, SQLAlchemy 1.3.24, whose branch is no longer supported, and gunicorn 20.0.4, which predates fixes for two request-smuggling advisories. Proton runs the hosted service on the same lock file, presumably with its own mitigations at the edge. Self-hosters inherit the stack without those. This is the main reason the Security row is Moderate rather than Strong.

### Concentration and stewardship

Two Proton engineers write almost all of the code. That is a small team for a service many people route their mail through, but it sits inside a larger company whose largest voting shareholder is a non-profit foundation that must consent to any change of control <a id="cite-startupticker-2"></a>([Startupticker.ch, 2024](#ref-startupticker)). The risk is less that SimpleLogin disappears than that it is folded further into Proton's bundles, as the announcement in 2022 foresaw <a id="cite-proton-join-2"></a>([Proton, 2022](#ref-proton-join)).

## Recommendation

Keep SimpleLogin in Assess.

- **For personal use**, a trial of the hosted service is reasonable now, on a custom domain you control, with a PGP key on the mailbox if Premium is used.
- **For clients**, recommend the hosted service only with the client's own domain, so the exit stays open, and explain reverse aliases before they need them.
- **For self-hosting**, not yet: first write and test a current deployment (supported Ubuntu or Debian, the `app-ci` image pinned by digest, current Postfix and TLS, 2048-bit DKIM), and check whether the gunicorn advisories are reachable behind it.

To move to Trial, verify the gates: trace the hosted web app's network requests, test sign-up, alias creation and switching off with a screen reader and keyboard only, and export and re-import aliases.

Re-review triggers: a dependency upgrade to supported Flask, SQLAlchemy and gunicorn versions, a refreshed self-hosting guide, PGP or custom domains moving to the free plan, a change to the privacy policy's analytics or retention, a security incident, or a change in Proton's ownership.

## Relationship to project

Email. SimpleLogin sits in front of a mailbox rather than replacing one, so it complements the Quebec email provider evaluations of Beluga and Northmail in `assess/email-providers/` and `assess/email-providers-quebec/`: a client could keep a local provider and still give each service its own address.

## Status notes

- Last reviewed: 2026-10-26
- In assess: an automated pass from source, git history and public documents. Nothing was installed, run or signed up for. A trial would create an account on a test domain, trace the web app, test it with assistive technology, and export and re-import the aliases.

## License

This document, *Universal Cake Evaluation: SimpleLogin, Email Aliasing*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### SimpleLogin

- [Repository](#ref-sl-repo)
- [Security policy](#ref-sl-security)
- [Privacy policy](#ref-sl-privacy)
- [Proton and SimpleLogin announcement](#ref-proton-join)
- [Proton Foundation report](#ref-startupticker)
- [Plan summary](#ref-toolradar)

## References

<a id="ref-proton-join"></a>Proton. (2022, April 8). *Proton and SimpleLogin join forces*. Proton blog. https://proton.me/it/blog/proton-and-simplelogin-join-forces
[Return to citation](#cite-proton-join)

<a id="ref-sl-privacy"></a>SimpleLogin. (n.d.-a). *Privacy policy*. Retrieved October 26, 2026, from https://simplelogin.io/privacy/
[Return to citation](#cite-sl-privacy)

<a id="ref-sl-security"></a>SimpleLogin. (n.d.-b). *SECURITY.md*. GitHub. Retrieved October 26, 2026, from https://github.com/simple-login/app/blob/master/SECURITY.md
[Return to citation](#cite-sl-security)

<a id="ref-sl-repo"></a>SimpleLogin. (2026). *SimpleLogin* (Version 4.83.2) [Computer software]. GitHub. https://github.com/simple-login/app
[Return to citation](#cite-sl-repo)

<a id="ref-startupticker"></a>Startupticker.ch. (2024). *Proton founders establish foundation as primary shareholder*. Retrieved October 26, 2026, from https://startupticker.ch/en/news/proton-founders-establish-foundation-as-primary-shareholder
[Return to citation](#cite-startupticker)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

<a id="ref-toolradar"></a>Toolradar. (n.d.). *SimpleLogin pricing*. Retrieved October 26, 2026, from https://toolradar.com/tools/simplelogin/pricing
[Return to citation](#cite-toolradar)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of SimpleLogin 4.83.2, hosted and self-hosted, from source, git history and public documents. Automated recommendation: Assess; hosted trial on a custom domain; self-hosting after a current deployment is written |

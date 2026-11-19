---
dc:title: "Universal Cake Evaluation: gopassbridge, Browser Extension for gopass"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "gopassbridge"
  - "gopass-jsonapi"
  - "gopass"
  - "browser extensions"
  - "native messaging"
dc:description: "Universal Cake evaluation of gopassbridge 2.1.1, the Firefox and Chromium extension that fills logins from a gopass store, together with gopass-jsonapi 1.17.4, the native messaging host it depends on."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-19"
dcterms:modified: "2026-11-19"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/gopasspw/gopassbridge"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--gopass--password-manager, universal-cake-evaluation--gopass--password-generation, universal-cake-evaluation--keepassxc--password-manager"
dc:identifier: "universal-cake-evaluation--gopassbridge--browser-extension"
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
      Initial Assess-ring evaluation of gopassbridge 2.1.1 with
      gopass-jsonapi 1.17.4 against Universal Cake Evaluation Metrics
      0.6.0. Extension source and manifests reviewed and its unit tests
      run; the native host configured and driven with crafted native
      messaging requests, with network and file behaviour verified on
      Linux. The extension was not run inside a browser.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # for technical gopass users who need browser filling, with submit-after-fill and auth-request handling turned off, once the accessibility gate is verified
      reasoning: "All four default gates pass, telemetry, licence and exit Verified. The extension makes no network requests of its own, the native host made no internet connections, and only the two official extension IDs may talk to the host. Site suggestions match whole host names after stripping subdomains up to the public suffix, so look-alike domains got no suggestions in testing. Filling happens only when the user picks an entry. Risks: the host gives the extension every entry in every mounted store, with no per-site check at fill time; submit-after-fill and HTTP authentication interception are on by default; gopass-jsonapi computes a TOTP code from the password for entries without an OTP secret when the password happens to be valid base32; per-site search history sits unencrypted in the browser profile; the host's release checksums are unsigned; the Firefox build is still Manifest V2; icon buttons lack accessible names; and the project has two active people and about 1,300 users."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-19T11:30:00-05:00"
  human: []
---

# Universal Cake Evaluation: gopassbridge, Browser Extension for gopass

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates gopassbridge, a browser extension for Firefox and Chromium-based browsers that searches a gopass store and fills login forms, against the Universal Cake Evaluation Metrics. gopassbridge cannot work alone: it talks, through the browsers' native messaging channel, to gopass-jsonapi, a separate program from the gopass project that reads the store on the extension's behalf. The two are evaluated together as one product.

The pair passes all four default gates. The design keeps secrets local: no network requests, no cloud, and a native host that answers only the official extension. Its site matching is careful about look-alike domains. The risks are in what the bridge is allowed to do and what it does by default: it can read every entry, it submits forms automatically after filling, it intercepts HTTP authentication prompts, and its native host shows a misleading one-time code for some entries. It is a small project with two active people.

This evaluation builds on the gopass password manager evaluation, and was listed as a follow-up in the gopass generators and front ends evaluation.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts about gopassbridge come from its repository at the `v2.1.1` tag and at commit `5da4522` (2026-05-31) <a id="cite-gpb-source"></a>([Hoefling & Harböck, 2025](#ref-gpb-source)), including its manifests, security policy and privacy statement, and from its store listings <a id="cite-amo"></a>([Mozilla, n.d.](#ref-amo)) <a id="cite-cws"></a>([Chrome Web Store, n.d.](#ref-cws)). Facts about gopass-jsonapi come from its repository at the `v1.17.4` tag <a id="cite-gpj-source"></a>([gopass Authors, 2026](#ref-gpj-source)) and from tests of its 1.17.4 Linux release. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate gopassbridge 2.1.1, published on 2025-11-28, with gopass-jsonapi 1.17.4 and gopass 1.17.4 on Linux, for one person filling logins in their own browser. The extension's source, both manifests and its unit tests were examined. The native host was installed from the project's release archive, configured with `gopass-jsonapi configure`, and driven directly with native messaging requests built to match the extension's. The extension itself was not run inside Firefox or Chrome, because the test environment cannot reach the extension stores; behaviour inside the browser is assessed from source. Windows and macOS were assessed from documentation.

## Scorecard metadata

- Product or service name: gopassbridge (browser extension) with gopass-jsonapi (native messaging host)
- Version evaluated: gopassbridge 2.1.1, source tag `v2.1.1`; Firefox add-on ID `{eec37db0-22ad-4bf1-9068-5ae08df8c7e9}`, Chrome extension ID `kkhfnlkhiapbiehimabddjbimfaijdhk`; gopass-jsonapi 1.17.4, `gopass-jsonapi-1.17.4-linux-amd64.tar.gz`, SHA-256 `c51f9b31ce6cb630d9166ad11d6853ec86a94d41a18d88ef118675b352ca9dee`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-11-19
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, gopass 1.17.4, GnuPG 2.4.4, strace 6.8, Node.js with pnpm 11.1.2 and Vitest for the extension's unit tests, a Python script for native messaging requests

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified | The extension's code contains no `fetch` or `XMLHttpRequest` calls; the Firefox listing declares no data collection. gopass-jsonapi made no internet connections while configuring or answering requests. The extension keeps, per site, the last search term and the last entry opened in the browser's local extension storage, unencrypted, and keeps its settings in the browser's synced storage, which travels with a Firefox or Google account if sync is on. No secrets are stored by the extension |
| A licence incompatible with the project | Pass | Verified | Both are MIT, read from their `LICENSE` files; the bundled WebExtension polyfill and URL regular expression are MIT too |
| Failure of the accessibility floor | Pass | Inferred | Every action is a button in the popup, reachable by keyboard, with a shortcut to open it (`Ctrl+Shift+U`) and Enter to fill when there is one match. The icon buttons have no accessible names, see Findings. Not tested with a screen reader |
| No exit | Pass | Verified | The extension holds no data of its own; all secrets stay in the gopass store, which has its own exit (see the gopass evaluation). Uninstalling the extension and removing two manifest files and one script undoes the setup |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Weak | Verified, Inferred | No ARIA attributes anywhere. Each search result has four buttons, fill, open, copy and details, and all four carry the entry name as their text, with the action only in a tooltip; a screen reader would likely announce the same name four times. Fill, copy and open also have keyboard modifiers (Shift, Ctrl, Alt with Enter or click). Not tested with Orca or NVDA |
| Multilingual integration | Weak | Verified | English and German only |
| Economic and cognitive accessibility | Weak | Verified, Claimed | Free. Setup needs gopass, a second program (gopass-jsonapi) downloaded separately, and `gopass-jsonapi configure` answered correctly for the right browser; store reviewers describe hours lost to setup (claimed). We found that the configure prompts accept a relative wrapper path without complaint, and that answering yes to "Install for all users?" writes system-wide manifests pointing into one user's home |
| Representation | Unknown | None | Not assessable from public information. No machine-learned models; the AI proxies are Not applicable |
| Compatibility | Moderate | Verified, Claimed | Firefox and Chrome are supported; Edge is listed as unsupported and other browsers as best-effort, though `configure` also knows Floorp and several Chromium browsers. The Chrome build uses Manifest V3; the Firefox build still uses Manifest V2. gopass-jsonapi has builds for Linux, macOS, Windows, FreeBSD and OpenBSD. Requires gopass 1.8.5 or later |
| Resilience | Strong | Verified | Works offline; everything runs on the local machine |
| Agency, sovereignty and privacy | Strong | Verified | MIT; nothing leaves the machine. The stored per-site search history reveals which sites the user has logins for to anyone with access to the browser profile |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | No terms beyond the stores' own, no pricing, no account; removing it costs minutes |
| Agency, interaction patterns | Moderate | Verified | Filling happens only when the user picks an entry; there is no autofill on page load. But two defaults act beyond the click: `submitafterfill` submits the form right after filling, and `handleauthrequests` intercepts every HTTP authentication prompt and opens the extension's popup instead of the browser's dialog. Both can be turned off in the options |
| Environment, direct, indirect, and authoring-side | Strong | Verified | 133 KB extension; the host is a 14 MB Go binary that runs only while answering a request |
| Security | Moderate | Verified, Inferred | Strengths: only the two official extension IDs are allowed by the native manifests (verified); host-name suggestions refuse look-alike and suffix-appended domains (verified); content scripts fill only visible fields in the top page and same-origin frames, and only on request; the extension's pages have a strict content security policy; a security policy with a private reporting route; 208 unit tests passed. Weaknesses: the host will return any entry to the extension, with no check at fill time that the entry belongs to the page; it computes a TOTP from the password for entries without an OTP secret when the password is valid base32 (verified); the filler writes the password into the field's `value` attribute as well as its value; the host's release checksums are not signed, unlike gopass's own; the Chrome private key is copied into the folder zipped for upload to the Chrome Web Store |
| Longevity | Moderate | Verified | Since 2017; 360 non-merge commits. gopassbridge's last release was 2025-11-28; in the year to 2026-11-19, 26 of its 27 human commits came from one person, Patrick Harböck. gopass-jsonapi is released with each gopass version, mostly by Dominik Schulz |
| Content endurance | Not applicable | Verified | Stores no content |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Moderate | Verified | Options for every behaviour above; readable plain JavaScript of about 1,400 lines; GitHub issues and a mailing list. Small team, so fixes depend on one or two people |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Claimed, Verified | About 1,000 Chrome users and 300 Firefox users (claimed by the store listings). No take rate. The native messaging protocol is small and stable, seven request types. Forkable under MIT, but a fork needs its own extension ID added to the native manifest. Contributor concentration: Martin Hoefling and Patrick Harböck wrote 259 of 312 human commits (83%) |
| Gates | Pass | Mixed | All four pass. Telemetry, licence and exit Verified; accessibility floor Inferred |

## Findings

### How the pieces fit

The extension cannot read files, so every request goes through the browser to gopass-jsonapi. `gopass-jsonapi configure` writes a native messaging manifest for the chosen browser, `com.justwatch.gopass.json`, and a small shell script, `gopass_wrapper.sh`, which sets up the GnuPG agent and runs `gopass-jsonapi listen`. The browser starts that script for each request, passing one length-prefixed JSON message on standard input and reading one reply.

The host understands seven requests: `getVersion`, `query` (search by text), `queryHost` (search by site), `getLogin` (user name and password), `getData` (all fields except the password), `create` (a new entry, with an optional generated password) and `copyToClipboard`. We sent each of them, built as the extension builds them, and recorded the replies. The configure and request runs made no internet connections.

### Who may talk to the host

The manifests that `configure` wrote list exactly one extension each: `{eec37db0-22ad-4bf1-9068-5ae08df8c7e9}` for Firefox and `chrome-extension://kkhfnlkhiapbiehimabddjbimfaijdhk/` for Chrome. A look-alike extension in a store, under a different ID, cannot reach the store, which is a strong protection. Two caveats follow from how browsers work rather than from gopassbridge. On Chrome, the public key in the extension's source fixes its ID, so anyone who can load an unpacked extension in developer mode on the user's machine can claim that ID (inferred). And the host trusts its caller completely: whatever the extension asks for, it returns, without its own prompt. GnuPG's agent may ask for a passphrase when its cache is cold, which is the only other check.

### Site matching

When the popup opens, the extension asks for entries matching the page's host name. gopass-jsonapi looks for an entry path component equal to the host, and if none is found, strips the leftmost label and tries again, stopping at the public suffix. Our results:

- `example.org` and `login.example.org` both found `web/example.org`.
- `example.org.evil.net` and `evil-example.org` found nothing.

This is good protection against look-alike domains: a person is not shown the real site's entry on a phishing page. It is not enforcement. The user can type any search, pick any entry and fill it into the page they are on; at fill time, neither the extension nor the host checks that the entry belongs to the page. Typing a search on an unfamiliar page should be a warning sign.

### Filling and submitting

Filling runs only when the user picks an entry. The content script looks for the first visible password field and a user name field in the same form, sets both, fires the input events many sites need, and, by default, submits the form. Visibility checks are simple (at least 30 by 10 pixels and not `visibility: hidden`), and frames are searched only when they share the page's origin. The script also sets the password as the field's `value` attribute, not only its live value, which makes it visible to style rules on the page; a page could already read the field with script, so the added exposure is small (inferred).

Submitting automatically saves a click, and removes the moment where a person might notice they are on the wrong site. We recommend turning `submitafterfill` off.

The extension also listens for HTTP authentication requests on every site and, with `handleauthrequests` on, opens its own popup in place of the browser's dialog. It checks that the credentials are sent back to the same popup that was opened, a sound protection against crossed requests. Turning it off restores the browser's own dialog.

### A one-time code that is not one

`getData` returns an entry's fields for the details view. It also tries to compute a current TOTP code. When an entry has no `otpauth`, `totp` or `hotp` field, gopass's OTP code falls back to treating the password as a TOTP secret, and if the password happens to be valid base32 it succeeds. Our test entry with the password `hunter2`, and another with `abcdefghij`, both came back with a `current_totp`, although neither had any OTP set up; entries with digits such as 0, 1, 8 or 9 in the password did not. Passphrases made of lower-case letters without separators, as gopass's own xkcd mode can produce, are valid base32.

Two harms follow. The details view shows a six-digit "current TOTP" that looks real and is meaningless, which can confuse anyone who does use OTP. And the code is derived from the password: someone who sees a code and knows roughly when it was shown can test guesses at a weak password offline (inferred). The fix belongs in gopass-jsonapi: compute a code only when an OTP field exists.

### Release and supply chain

gopass-jsonapi's release lists SHA-256 checksums, and our archive matched them, but unlike gopass itself there is no signature on the checksum file, so the download can be checked only against the same GitHub release. The extension is distributed through Mozilla's and Google's stores, which sign what they deliver. The release process copies the Chrome signing key into the folder that is zipped and uploaded to the Chrome Web Store; Google has supported including a key on first upload to keep an extension's ID, but uploading it with every release gives the store a copy of the private key each time, and it would be worth asking the maintainers whether that step is still needed (inferred).

The extension's 208 unit tests passed under Vitest. Mozilla's linter flags one dynamic `innerHTML` assignment; the project's privacy file explains that its two `innerHTML` uses insert only the extension's own translation strings, and our reading agrees.

### Small gaps

- The Firefox build is still on Manifest V2. Firefox continues to support it, but the project noted in 2024 that the Firefox move would follow separately.
- The repository's sample `gopass_wrapper.sh` uses `source` under `/bin/sh`, which fails on systems where `/bin/sh` is dash, such as Debian and Ubuntu, but only when `~/.gpg-agent-info` exists. The wrapper that `configure` generates has the same line.
- The wrapper is written from an HTML template, so a path containing characters such as `&` or `'` would be escaped for HTML, not for a shell (inferred from source).

## Recommendation

Keep gopassbridge in Assess for now. Move it to Trial for technical gopass users who need browser filling, once a screen reader pass with Orca or NVDA shows the popup is usable, on these conditions:

- Install gopassbridge only from the official store listings and check the extension ID: `{eec37db0-22ad-4bf1-9068-5ae08df8c7e9}` in Firefox, `kkhfnlkhiapbiehimabddjbimfaijdhk` in Chrome.
- Install gopass-jsonapi 1.17.4 or later, matching the gopass version, and compare its checksum.
- Run `gopass-jsonapi configure --browser <browser> --global=false --path <absolute path>`, answering the prompts deliberately, and keep the wrapper and manifests readable only by the user.
- In the extension's options, turn off "submit after fill" and, unless needed, "handle authentication requests".
- Treat a manual search on an unfamiliar page as a warning sign; prefer the site suggestions.
- Ignore the "current TOTP" field for entries without OTP set up, until upstream fixes it.
- Clear the extension's storage when a browser profile is shared or handed over.

For non-technical people, or anyone who wants browser filling without a second program to install, KeePassXC with its own browser extension is a simpler route, and will be worth its own evaluation.

Reporting the TOTP fallback, the unsigned host checksums and the configure prompts upstream would improve the bridge for everyone.

Re-review triggers specific to gopassbridge: a Firefox Manifest V3 release; a change in maintainers; a change to the native messaging protocol or allowed IDs; and fixes for the TOTP fallback or the release signing.

## License

This document, *Universal Cake Evaluation: gopassbridge, Browser Extension for gopass*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### gopassbridge and gopass-jsonapi

- [gopassbridge source repository](#ref-gpb-source)
- [gopass-jsonapi source repository](#ref-gpj-source)

### Store listings

- [Firefox add-on listing](#ref-amo)
- [Chrome Web Store listing](#ref-cws)

## References

<a id="ref-cws"></a>Chrome Web Store. (n.d.). *Gopass Bridge* [Browser extension listing]. Retrieved November 19, 2026, from https://chromewebstore.google.com/detail/gopass-bridge/kkhfnlkhiapbiehimabddjbimfaijdhk
[Return to citation](#cite-cws)

<a id="ref-gpj-source"></a>gopass Authors. (2026). *gopass-jsonapi* (Version 1.17.4) [Computer software]. GitHub. https://github.com/gopasspw/gopass-jsonapi
[Return to citation](#cite-gpj-source)

<a id="ref-gpb-source"></a>Hoefling, M., & Harböck, P. (2025). *gopassbridge* (Version 2.1.1) [Computer software]. GitHub. https://github.com/gopasspw/gopassbridge
[Return to citation](#cite-gpb-source)

<a id="ref-amo"></a>Mozilla. (n.d.). *Gopass Bridge* [Firefox add-on listing]. Retrieved November 19, 2026, from https://addons.mozilla.org/firefox/addon/gopass-bridge/
[Return to citation](#cite-amo)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of gopassbridge 2.1.1 with gopass-jsonapi 1.17.4 against Universal Cake Evaluation Metrics 0.6.0; extension reviewed from source with unit tests run, native host driven with crafted requests on Linux |

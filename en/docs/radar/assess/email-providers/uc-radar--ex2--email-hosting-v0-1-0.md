---
dc:title: "Ex2 Email Hosting, Universal Cake Evaluation"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "radar"
  - "email hosting"
  - "email provider"
  - "ex2"
dc:description: "Assess-ring evaluation of Ex2 Email hosting, custom-domain mailboxes on cPanel with Roundcube, against the Universal Cake Evaluation Metrics v0.6.0."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-06"
dcterms:modified: "2026-11-06"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, uc-radar--evaluation-lifecycle, uc-radar--ex2--vps-hosting, beluga-universal-cake-evaluation"
dc:identifier: "uc-radar--ex2--email-hosting"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-06"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial entry. Light pass on public evidence only: every gate and
      the Security pillar are Inferred or Claimed, none Verified.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial"
      reasoning: >
        Standard protocols (IMAP, SMTP, CalDAV, CardDAV) make exit cheap
        and let people use the accessible client of their choice, and
        prices are low and public. The content-exposure gate is Unknown,
        not failed: Ex2's privacy policy is silent on staff access to
        and scanning of mail, and its anti-spam provider is unnamed.
        Trial only after Ex2 answers those questions in writing and a
        test export is Verified, and only for mail that is not
        sensitive. Where data sovereignty matters most, Béluga, already
        on the radar, documents more.
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-06T15:30:00-05:00"
  human: []
---

# uc-radar--ex2--email-hosting-v0.1.0

## Abstract

Ex2 Email is custom-domain mailbox hosting sold by Ex2 Inc., a hosting company in Quebec City, built on cPanel with Roundcube webmail. This entry evaluates it against the Universal Cake Evaluation Metrics v0.6.0 as an Assess-ring light pass on public documents. Its strengths are open protocols, a cheap and documented exit, low published prices, trilingual support and a choice of server country. Its weak points are on the content side: the privacy policy does not say whether staff can read or scan mail, the anti-spam provider is unnamed, two-factor authentication for mailboxes is not mentioned, and the terms are undated and carry no change-notice clause. No gate fails on this evidence, but the content-exposure gate is Unknown until Ex2 answers.

## Sources and Acknowledgements

The evaluation follows the Universal Cake Evaluation Metrics <a id="cite-uc-metrics"></a>([Steel, 2026a](#ref-uc-metrics)) and the UC radar entry template <a id="cite-uc-template"></a>([Steel, 2026b](#ref-uc-template)). Facts about the product come from Ex2's email hosting page <a id="cite-ex2-email"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)), its terms <a id="cite-ex2-terms"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)), privacy policy <a id="cite-ex2-privacy"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)), about page <a id="cite-ex2-about"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)) and knowledge base. Vendor statements are tagged Claimed throughout; nothing has yet been tested hands-on. Company-level findings are shared with the Ex2 Cloud VPS entry, uc-radar--ex2--vps-hosting.

## What it is

Hosted mailboxes on your own domain, managed in cPanel, read with any IMAP or POP3 client or with Roundcube webmail. Plans run from Mini (1 mailbox, 1 GB, US$1.67 a month billed yearly) to Unlimited (unlimited mailboxes, 100 GB, US$10.65 a month billed yearly) <a id="cite-ex2-email-2"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)). Calendars and address books are shared over CalDAV and CardDAV.

## Why interesting

A low-cost, standards-based mailbox for individuals, small businesses and NGOs, the client groups Vishpala starts with, from a Quebec company with French-first support and a Canadian server option. Because it uses open protocols and a domain the owner keeps, it can sit beside the ex2-base static-site server without tying a client to either.

## Concerns

- **Mail content handling is undocumented.** The privacy policy does not address whether Ex2 staff can access mailbox content or how mail is scanned, and it shares data with "email or anti-spam providers" without naming them <a id="cite-ex2-privacy-2"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)). Every incoming message passes antivirus and antispam filtering <a id="cite-ex2-email-3"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)); where that happens, and whether a third party sees the content, is unknown.
- **No mention of two-factor authentication for mailboxes or webmail.** The Ex2 client account supports it <a id="cite-ex2-2fa"></a>([Ex2, 2023](#ref-ex2-2fa)); mailbox logins appear to rely on passwords alone.
- **Backups are short and their location unstated.** Daily backups are kept for 7 days plus day 15, on "a separate infrastructure" whose location is not given <a id="cite-ex2-email-4"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)). The terms disclaim responsibility for backup failure and data loss <a id="cite-ex2-terms-2"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)).
- **Data deleted at expiry, with no retrieval window.** Backups "can be permanently deleted" when an account expires, and Ex2 "will not attempt to retain data" <a id="cite-ex2-terms-3"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)). A missed renewal can cost mail that is not also held in a local client.
- **Undated legal documents with no change-notice clause**, and renewal at the rates in force at renewal, as for the VPS.

## Evaluation

Ratings use the v0.6.0 scale and evidence tags. Lens is user unless stated.

### Inclusive

**Accessibility**

- Rating: Moderate
- Evidence: Inferred
- Notes: Because mail, calendars and contacts use IMAP, SMTP, CalDAV and CardDAV, a person can use whichever client works with their assistive technology, for example Thunderbird with NVDA or Orca, or a terminal client such as mutt with a braille display. That keeps the main tasks, reading and sending mail, open regardless of Ex2's own interfaces. The interfaces Ex2 requires, ordering, the client centre and cPanel for creating mailboxes and filters, have not been tested, and no accessibility statement was found. Roundcube's accessibility depends on version and skin, which Ex2 does not state.

**Multilingual integration**

- Rating: Strong
- Evidence: Claimed
- Notes: Live chat and 24/7 support in French, English and Spanish <a id="cite-ex2-email-5"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)). Roundcube itself is available in many languages.

**Affordability, hardware and connectivity, cognitive accessibility**

- Rating: Affordability Strong; hardware and connectivity Strong; cognitive Moderate.
- Evidence: Claimed (prices); Inferred (the rest).
- Notes: From US$1.67 a month. IMAP clients work offline and sync when connected, which suits slow or intermittent links and old machines. cPanel is a dense interface for a non-technical owner creating mailboxes, aliases and filters, though it is the common hosting panel many have met before.

**Representation**

- Rating: Unknown
- Evidence: none
- Notes: No public information on disabled representation or user research.

**Compatibility**

- Rating: Strong
- Evidence: Claimed
- Notes: POP3, IMAP and SMTP with Outlook, Apple Mail and Thunderbird; CalDAV and CardDAV; SPF, DKIM, DMARC and ARC; TLS with an automatically renewed certificate <a id="cite-ex2-email-6"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)). ActiveSync is not mentioned, so some phone setups need separate CalDAV and CardDAV accounts.

**Resilience**

- Rating: Moderate
- Evidence: Claimed and Inferred
- Notes: Mail held in an IMAP client stays readable during an outage, and new mail queues at senders for days. Against that: no SLA, short backup retention, and deletion at expiry.

### Agency

**Sovereignty & Privacy**, including the power-imbalance proxies

- Rating: Moderate
- Evidence: Claimed and Inferred
- Notes:
  - Control and location: the owner chooses France, Canada or the US at order time <a id="cite-ex2-email-7"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)), under a Quebec company citing Law 25, PIPEDA and the GDPR <a id="cite-ex2-privacy-3"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)). The domain stays the owner's, so the owner, not Ex2, decides where mail goes next.
  - Weaknesses: content access and scanning undocumented, anti-spam provider unnamed, backup location unstated. The vendor can see the content of every message stored on its servers, while the user cannot see the vendor's practices, which the metrics treat as a measurable asymmetry.
  - Exit cost: low. Change the MX records and copy mailboxes over IMAP. Estimate 1 to 2 hours per mailbox for a careful copy, and half a day for a five-person organization, plus DNS propagation. If the domain was also registered at Ex2, add a transfer of 7 to 10 days <a id="cite-ex2-transfer"></a>([Ex2, n.d.](#ref-ex2-transfer)). Inferred.
  - Data portability: Strong. Machine-readable: IMAP for mail, CalDAV and CardDAV for calendars and contacts, giving standard `.eml` or mbox, `.ics` and `.vcf`. Human-readable: the same files open in ordinary mail and calendar programs. Ex2's own migration guide describes exporting with Thunderbird and the ImportExportTools NG add-on, or to an Outlook PST <a id="cite-ex2-transfer-2"></a>([Ex2, n.d.](#ref-ex2-transfer)).
  - Terms-of-service volatility: Unknown; undated terms and policy.
  - Pricing asymmetry: Moderate. Public prices with monthly and yearly options; renewal at the rates then in force; setup help is "on quote".
  - AI-specific proxies: Not applicable; no model is part of the email product.

**Interaction Patterns**

- Rating: Moderate
- Evidence: Claimed
- Notes: Email is a quiet product by nature, and nothing manipulative is described. The 30-day guarantee excludes monthly plans and domains <a id="cite-ex2-email-8"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)), and deletion at expiry turns a missed payment into possible data loss, the hostage-data pattern in its mildest form: disclosed, but only in the terms. The order flow has not been walked through.

### Environment

**Direct, indirect and authoring-side impacts** (lens: planet)

- Rating: Moderate
- Evidence: Claimed
- Notes: The same company-level figures as the VPS entry: low-carbon grids in Quebec and France, about 15 kg CO₂e per server per month, 1% of revenue to environmental non-profits <a id="cite-ex2-about-2"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)), with offset claims that differ between pages and no audit. Choosing the Canadian or French location matters more for email than any feature. IMAP clients cache mail locally, so repeated downloads are rare.

### Security

- Rating: Moderate
- Evidence: Claimed
- Notes:
  - Data held: all mail, calendars and contacts, on Ex2's servers and backups.
  - Transport and sender authentication: TLS between clients and server; SPF, DKIM, DMARC and ARC <a id="cite-ex2-email-9"></a>([Ex2 Inc., n.d.-b](#ref-ex2-email)).
  - Account security: 2FA on the Ex2 client account; not mentioned for mailboxes, webmail or cPanel.
  - Disclosure: no vulnerability disclosure policy or security contact found; no certification advertised <a id="cite-clubic-fiche"></a>([Clubic, n.d.](#ref-clubic-fiche)).
  - Patching: the cPanel, mail server and Roundcube stack is Ex2's to patch; cadence unknown.
  - Supply chain: cPanel and Roundcube are third-party software; cPanel is proprietary.
  - Assessment method: document review only, 2026-11-06.

**Network behaviour, file system behaviour, content exposure:** for a hosted mail service, content exposure to the provider is inherent; the open question is to whom else. To be answered by Ex2 in writing, see Status notes.

### The Product or Service Itself

**Longevity**

- Rating: Moderate
- Evidence: Claimed
- Notes: Company founded in 2013 with more than 10,000 customers claimed <a id="cite-ex2-about-3"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)); ownership and funding not stated. The software stack, cPanel and Roundcube, is widely used and will outlive any one host.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: Mail is stored in standard formats and, with an IMAP client, also on the user's own machine. Ex2's webmail is an interface, not a container: if it went away, the mail would not.

**Exit and portability** [GATE]

- Rating: Strong; gate pass
- Evidence: Inferred
- Notes: Open protocols, own domain, and a documented export path in Ex2's own help centre.

**Adjustability and support**

- Rating: Moderate
- Evidence: Claimed
- Notes: 24/7 trilingual chat and tickets. Configuration is limited to what cPanel exposes; anything beyond that is not adjustable on a shared mail platform.

**Market Position**

- Rating: Moderate
- Evidence: Inferred
- Notes: A small provider in a crowded market, so concentration is low and alternatives are many. Take rate: Not applicable. API stability: no public API found. Forkability and contributor concentration: Not applicable to the service; the protocols are the real guarantee. Dependency on cPanel, a proprietary platform whose licence costs are set by its vendor, is the main upstream risk to price. Foundation-model dependency: Not applicable.

## Gates

| Gate | Result | Evidence | Notes |
|------|--------|----------|-------|
| Telemetry or content exposure that cannot be disabled | Unknown | none | Mail scanning, staff access and the anti-spam provider are undocumented; whether filtering can be turned off is unknown |
| Licence incompatible with the project | No failure found | Inferred | The terms make the customer responsible for content; no clause claiming rights over it was found |
| Accessibility floor | No failure found | Inferred | Standard protocols allow any accessible client; cPanel and Roundcube untested |
| Exit | Pass | Inferred | IMAP, CalDAV, CardDAV, own domain, documented export |

No gate has failed. The content-exposure gate is Unknown, not passed, and none is Verified, so the entry stays in Assess.

## Scorecard

- Product or service name: Ex2 Email hosting
- Version evaluated: plans and terms as published on 2026-11-06 (undated)
- Evaluator: Claude Opus 5.5 (Anthropic), automated pass for Christopher Steel
- Date: 2026-11-06
- Radar ring: Assess

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | Any accessible client over standard protocols; cPanel and Roundcube untested |
| Multilingual integration | Strong | Claimed | French, English, Spanish |
| Economic and cognitive accessibility | Strong / Moderate | Claimed / Inferred | From US$1.67 a month; cPanel is dense |
| Representation | Unknown | none | Nothing public |
| Compatibility | Strong | Claimed | IMAP, POP3, SMTP, CalDAV, CardDAV; no ActiveSync mentioned |
| Resilience | Moderate | Claimed / Inferred | Local IMAP copies; short backups; deletion at expiry |
| Agency, sovereignty & privacy | Moderate | Claimed / Inferred | Country choice and own domain; content handling undocumented |
| Agency, power-imbalance proxies | Strong / Unknown | Inferred | Exit cost about half a day for five mailboxes; full portability; ToS volatility unknown |
| Agency, interaction patterns | Moderate | Claimed | Quiet product; costs and deletion terms only in the fine print |
| Environment, direct, indirect and authoring-side | Moderate | Claimed | Low-carbon grids; inconsistent offset claims |
| Security | Moderate | Claimed | TLS, SPF, DKIM, DMARC; no mailbox 2FA mentioned; no disclosure policy |
| Longevity | Moderate | Claimed | Since 2013 |
| Content endurance | Strong | Inferred | Standard formats, local copies |
| Exit and portability | Strong | Inferred | Gate pass |
| Adjustability and support | Moderate | Claimed | 24/7 trilingual; limited to cPanel |
| Market position | Moderate | Inferred | Many substitutes; cPanel dependency |
| Gates | No failure found | Inferred | Content exposure Unknown |

## Relationship to project

A candidate mailbox provider for Vishpala's individual, small-business and NGO clients, and for the domains whose sites run on the ex2-base server. It overlaps with the [Béluga evaluation](beluga-universal-cake-evaluation.md) in this folder. Read side by side, Béluga documents its sovereignty and accessibility work in more detail and runs its own Quebec infrastructure, while Ex2 is cheaper, more established and fully standards-based. A client who needs documented data handling should be pointed at Béluga first; a client who mainly needs cheap, portable mail on their own domain can use Ex2.

## Status notes

- Last reviewed: 2026-11-06
- In assess: light pass done on public documents. No gate fails; the content-exposure gate is Unknown; none is Verified. Still needed:
  - Ask Ex2 in writing: whether staff can access mailbox content and under what process; where spam and virus filtering runs and which provider performs it; where backups are stored; whether 2FA or app passwords exist for mailboxes, webmail and cPanel; and how term changes are announced. The first two answers decide the content-exposure gate.
  - Test export on a trial mailbox: Thunderbird with ImportExportTools NG to mbox and `.eml`, plus calendar and contact export over CalDAV and CardDAV, then re-import elsewhere.
  - Screen reader pass, Orca or NVDA, on ordering, the client centre, cPanel's email pages and Roundcube.
  - TLS check of the IMAP and SMTP endpoints, for example with `testssl.sh`.
  - ToS volatility: diff archived snapshots of the terms and privacy policy.
- Proposed trial, once the gate questions are answered and the export is Verified: one non-sensitive domain on the Small plan in the Canadian location, for 90 days. End condition: 90 days pass, any gate fails, or mail is lost. Questions for the trial: deliverability to large providers, filtering accuracy, and support response.

## Resources

### Universal Cake

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)
- [UC Radar Entry Template](#ref-uc-template)

### Ex2 documents

- [Ex2 email hosting page](#ref-ex2-email)
- [Ex2 terms and conditions](#ref-ex2-terms)
- [Ex2 privacy policy](#ref-ex2-privacy)
- [Ex2 about page](#ref-ex2-about)
- [Ex2 email transfer article](#ref-ex2-transfer)
- [Ex2 two-factor authentication article](#ref-ex2-2fa)

### Third-party reviews

- [Clubic product sheet](#ref-clubic-fiche)

## References

<a id="ref-clubic-fiche"></a>Clubic. (n.d.). *Qu'est-ce que Ex2 ?* Retrieved November 6, 2026, from https://www.clubic.com/telecharger-fiche439395-ex2.html
[Return to citation](#cite-clubic-fiche)

<a id="ref-ex2-2fa"></a>Ex2. (2023, February 13). *How to enable the two-factor authentication on your Ex2 account?* Ex2 Knowledge Base. https://help.ex2.com/en/article/u50cy1/
[Return to citation](#cite-ex2-2fa)

<a id="ref-ex2-transfer"></a>Ex2. (n.d.). *How to transfer your emails to Ex2?* Ex2 Knowledge Base. Retrieved November 6, 2026, from https://help.ex2.com/en/article/t7xljv/
[Return to citation](#cite-ex2-transfer)

<a id="ref-ex2-about"></a>Ex2 Inc. (n.d.-a). *About us*. Retrieved November 6, 2026, from https://www.ex2.com/en/company/about-us?lang=en
[Return to citation](#cite-ex2-about)

<a id="ref-ex2-email"></a>Ex2 Inc. (n.d.-b). *Your custom email addresses in just minutes*. Retrieved November 6, 2026, from https://www.ex2.com/en/hosting/email
[Return to citation](#cite-ex2-email)

<a id="ref-ex2-privacy"></a>Ex2 Inc. (n.d.-c). *Privacy policy*. Retrieved November 6, 2026, from https://www.ex2.com/privacy-policy?lang=en
[Return to citation](#cite-ex2-privacy)

<a id="ref-ex2-terms"></a>Ex2 Inc. (n.d.-d). *Terms and conditions*. Retrieved November 6, 2026, from https://www.ex2.com/terms-and-conditions?lang=en
[Return to citation](#cite-ex2-terms)

<a id="ref-uc-metrics"></a>Steel, C. (2026a). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake, uc-radar repository. https://github.com/steelcj/uc-radar
[Return to citation](#cite-uc-metrics)

<a id="ref-uc-template"></a>Steel, C. (2026b). *UC radar entry template* (Version 0.3.0) [Unpublished manuscript]. UniversalCake, uc-radar repository. https://github.com/steelcj/uc-radar
[Return to citation](#cite-uc-template)

## License

This document, *Ex2 Email Hosting, Universal Cake Evaluation*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring light pass against metrics 0.6.0, on public evidence only |

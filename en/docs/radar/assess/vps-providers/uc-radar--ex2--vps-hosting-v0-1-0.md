---
dc:title: "Ex2 Cloud VPS, Universal Cake Evaluation"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "radar"
  - "vps"
  - "hosting provider"
  - "ex2"
dc:description: "Assess-ring evaluation of the Ex2 Cloud VPS service (Cloud Performance line) against the Universal Cake Evaluation Metrics v0.6.0, for hosting static sites built with ex2-base."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-06"
dcterms:modified: "2026-11-06"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, uc-radar--evaluation-lifecycle, uc-radar--ex2--email-hosting"
dc:identifier: "uc-radar--ex2--vps-hosting"
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
        No gate fails on public evidence, exit cost is low because the
        server is rebuilt from provider-neutral Ansible, and the hosted
        content is a copy of an archive kept elsewhere. Trial only after
        the Assess testing listed in Status notes turns the gates and
        Security into Verified, in particular the accessibility of the
        out-of-band console and an outbound-traffic trace of a fresh
        instance.
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-06T15:30:00-05:00"
  human: []
---

# uc-radar--ex2--vps-hosting-v0.1.0

## Abstract

Ex2 Cloud VPS is an unmanaged virtual server sold by Ex2 Inc., a hosting company in Quebec City. This entry evaluates the Cloud Performance line, the plan chosen for the ex2-base static-site server, against the Universal Cake Evaluation Metrics v0.6.0. It is an Assess-ring light pass: all gates and the Security pillar are rated on public documents only. No gate fails on that evidence. The open risks are an untested out-of-band console that may not work with a screen reader, terms and a privacy policy that name no sub-processors and carry no dates, and no provider backups for VPS plans.

## Sources and Acknowledgements

The evaluation follows the Universal Cake Evaluation Metrics <a id="cite-uc-metrics"></a>([Steel, 2026a](#ref-uc-metrics)) and the UC radar entry template <a id="cite-uc-template"></a>([Steel, 2026b](#ref-uc-template)). Facts about Ex2 come from its own terms <a id="cite-ex2-terms"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)), privacy policy <a id="cite-ex2-privacy"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)) and about page <a id="cite-ex2-about"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)), from its knowledge base, and from third-party plan listings and reviews, cited where used. Vendor statements are tagged Claimed throughout; nothing has yet been tested hands-on.

## What it is

A cloud virtual server with root access, sold monthly; Ex2 does not name its virtualization platform. The Cloud Performance 1 vCPU plan has an AMD EPYC vCPU, 1 GB RAM, 25 GB NVMe storage and 2 TB monthly transfer, listed at €10.20 a month <a id="cite-whtop-perf"></a>([WHTop, 2026a](#ref-whtop-perf)). Ex2 offers servers in Canada, France and the United States <a id="cite-ex2-about-2"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)).

## Why interesting

It is the provider chosen for the ex2-base static-site server: a Quebec company, French-first support, a Canadian data centre option on a low-carbon grid, and plan features (custom ISO, console, IPv6) that let a minimal, hardened Debian 13 run without a control panel. Because ex2-base rebuilds the server from Ansible and the site content is deployed from an archive kept elsewhere, the provider is a replaceable layer rather than a home for the content, which is the property the metrics reward most.

## Concerns

- **Out-of-band console accessibility is unknown.** The console is the recovery path when SSH fails. Browser consoles of this kind usually draw the screen as an image, which a screen reader cannot read. If so, a blind administrator could run the server over SSH but could not recover it, a task the product requires when things go wrong.
- **No provider backups for VPS.** The terms state that dedicated servers and VPS are not externally backed up and that the customer must keep its own backups <a id="cite-ex2-terms-2"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)).
- **Thin, undated legal documents.** The terms and privacy policy carry no effective date; the terms contain no clause on how changes are announced, and renewal is "according to the terms and rates applied by Ex2 on the date of its renewal" <a id="cite-ex2-terms-3"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)). The privacy policy names no sub-processors <a id="cite-ex2-privacy-2"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)).
- **Data centre operators are not named.** Ex2 names countries, not facilities. A 2023 review lists Beauharnois, Roubaix, Strasbourg, Gravelines, Paris and New York <a id="cite-clubic"></a>([Clubic, 2023](#ref-clubic)); the first four match OVHcloud's sites, so Ex2 likely runs on OVHcloud infrastructure. This is Inferred, not confirmed.
- **No SLA.** Ex2 "will seek to provide 24/7 access" without an uptime figure or service credits <a id="cite-ex2-terms-4"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)).
- **Feature gaps unconfirmed:** snapshots, a provider firewall, reverse DNS, cloud-init and a Debian 13 image are not documented publicly.

## Evaluation

Ratings use the v0.6.0 scale and evidence tags. Lens is owner unless stated.

### Inclusive

**Accessibility**, including the text interface

- Rating: Unknown overall. Server side Strong; provider web surfaces Unknown.
- Evidence: Inferred (server side); none (web surfaces).
- Notes: Once running, the server is Debian over SSH, a plain-text interface that works with terminal screen readers and braille displays, and ex2-base adds no full-screen tools. The provider surfaces a person must use, ordering, the client centre, the VPS panel and the console, have not been tested, and no accessibility statement was found on ex2.com. The console is the gate risk described under Concerns. Lens conflict: strong for an owner who never needs the console, unknown for one who does.

**Multilingual integration**

- Rating: Strong
- Evidence: Claimed
- Notes: Site and 24/7 support in French, English and Spanish <a id="cite-ex2-about-3"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)). French is a first language of the company, not a translation.

**Affordability, hardware and connectivity, cognitive accessibility**

- Rating: Affordability Strong; hardware and connectivity Strong; cognitive Weak for non-technical owners, Strong for this owner.
- Evidence: Claimed (price); Inferred (the rest).
- Notes: €10.20 a month for the smallest Performance plan. An SSH session works on old hardware and slow, intermittent links. An unmanaged VPS assumes systems administration skill; for a client owner without it, the cognitive load is high unless a managed layer is added. Record both answers rather than blending them.

**Representation**

- Rating: Unknown
- Evidence: none
- Notes: No public information on disabled representation or user research. Its absence from public documentation is itself a data point.

**Compatibility**

- Rating: Strong
- Evidence: Claimed
- Notes: Broad OS catalogue including Linux and Windows, rebuilds, custom ISO and real-time console <a id="cite-whtop-regular"></a>([WHTop, 2026b](#ref-whtop-regular)); IPv6 is listed for the Performance line <a id="cite-whtop-perf-2"></a>([WHTop, 2026a](#ref-whtop-perf)). Custom ISO means a Debian netinst install is possible even without a Debian 13 image.

**Resilience**

- Rating: Moderate
- Evidence: Inferred
- Notes: The built site is static files with no licence server or live dependency, and it degrades to "unreachable", never to "corrupted". Against that: no provider backups for VPS, no SLA, and snapshots unconfirmed. The site survives a provider outage only because its source lives elsewhere, which ex2-base already assumes.

### Agency

**Sovereignty & Privacy**, including the power-imbalance proxies

- Rating: Moderate
- Evidence: Claimed and Inferred
- Notes:
  - Control: full root; the owner installs and configures everything, so what runs on the server is the owner's choice.
  - Data location: chosen by the owner at order time (Canada, France or US). The privacy policy places Ex2's main data centres in Canada and France and cites Quebec's Law 25, PIPEDA and the GDPR <a id="cite-ex2-privacy-3"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)).
  - Weaknesses: sub-processors described only by type (data centres, payment processors, email or anti-spam providers); the data centre operator is undisclosed; the US location is available and would bring US law into play.
  - Phone-home: provider images may include guest agents; not checked. A custom-ISO install avoids any provider-supplied software.
  - Exit cost: low. ex2-base rebuilds the server on any Debian 13 VPS from the repo, and content is redeployed by rsync. Estimate 2 to 4 hours plus DNS propagation for one static-site server. Inferred.
  - Data portability: Strong. Machine-readable and human-readable are the same thing here: plain files reachable over SSH and rsync, or a whole-disk image.
  - Terms-of-service volatility: Unknown. The terms and privacy policy carry no dates or version numbers, so change history can only be reconstructed from archived snapshots. The lack of dating is itself an information asymmetry.
  - Pricing asymmetry: Moderate. Prices are public in CAD, USD and EUR and fixed for the 12-month period, but renewals use the rates in force at renewal <a id="cite-ex2-terms-5"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)).
  - AI-specific proxies: Not applicable; the VPS product contains no model. Ex2 sells an AI website builder separately, which is out of scope here.

**Interaction Patterns**

- Rating: Moderate
- Evidence: Claimed (from the terms)
- Notes: Nothing manipulative found in the terms, but several costs sit in the fine print: monthly plans have no trial and are not refundable, the 30-day money-back guarantee excludes them, card processing fees of up to 5% are not refunded, and a fixed-term plan renews unless cancelled by written notice at least 7 days before expiry <a id="cite-ex2-terms-6"></a>([Ex2 Inc., n.d.-d](#ref-ex2-terms)). These are disclosed, which matches the Visible costs side of the table, but only in the terms. The order flow has not been walked through.

### Environment

**Direct, indirect and authoring-side impacts** (lens: planet)

- Rating: Moderate
- Evidence: Claimed
- Notes: Ex2 publishes grid figures of 0.025 kg CO₂e per kWh for Quebec hydropower and 0.026 for France, an estimate of about 15 kg CO₂e per server per month, and a 1% of revenue donation to environmental non-profits <a id="cite-ex2-about-4"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)). Claims are inconsistent across pages: the Canadian landing page says Ex2 "offsets its carbon emissions by 200%" <a id="cite-ex2-ca"></a>([Ex2 Inc., n.d.-b](#ref-ex2-ca)), while the about page makes no offset claim. No third-party audit is cited. Choosing the Canadian location keeps the server on hydro power; the US location would not. A 1 GB static-file server is about as light as hosting gets, and nothing heavy runs on the authoring side. The Green Web Foundation listing can be checked directly to move part of this to Verified.

### Security

- Rating: Moderate
- Evidence: Claimed
- Notes:
  - Data held by Ex2: account, billing and website-analytics data <a id="cite-ex2-privacy-4"></a>([Ex2 Inc., n.d.-c](#ref-ex2-privacy)); the server's contents are under the owner's control.
  - Account security: two-factor authentication is available for the Ex2 client account <a id="cite-ex2-2fa"></a>([Ex2, 2023](#ref-ex2-2fa)).
  - Disclosure: no vulnerability disclosure policy, security contact or `security.txt` was found, and Ex2 advertises no security certification <a id="cite-clubic-fiche"></a>([Clubic, n.d.](#ref-clubic-fiche)).
  - Patching: OS patching is the owner's (ex2-base runs unattended-upgrades); hypervisor and host patching cadence is unknown.
  - Attack surface: adding the VPS adds a public server, kept to sshd and Caddy by ex2-base and checked by its listener audit.
  - Assessment method: document review only, 2026-11-06. Not yet tested; see Status notes.

**Network behaviour, file system behaviour, content exposure:** not yet traced. The checklist items are to be filled from the outbound-traffic trace of a fresh instance listed in Status notes.

### The Product or Service Itself

**Longevity**

- Rating: Moderate
- Evidence: Claimed
- Notes: Founded in Quebec City in 2013, with more than 10,000 customers and 20,000 sites claimed <a id="cite-ex2-about-5"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)). Ownership, funding and team size are not stated.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: Nothing is authored on the server; it holds deployed copies of content kept in the owner's archive. If Ex2 disappeared, no content would be lost, only the copy.

**Exit and portability** [GATE]

- Rating: Strong; gate pass
- Evidence: Inferred
- Notes: Full root access allows any export: rsync of files, database dumps, or a disk image. Open formats throughout. The migration path is any other Debian VPS plus the ex2-base playbooks.

**Adjustability and support**

- Rating: Moderate
- Evidence: Claimed
- Notes: 24/7 chat and tickets in three languages <a id="cite-ex2-about-6"></a>([Ex2 Inc., n.d.-a](#ref-ex2-about)); one review notes phone support is reserved for dedicated servers <a id="cite-hellosafe"></a>([HelloSafe, n.d.](#ref-hellosafe)). No public provisioning API was found, so servers cannot be created from code, only configured once running.

**Market Position**

- Rating: Moderate
- Evidence: Inferred
- Notes: A small provider in a commodity market, so market concentration and take rate are low, and take rate is Not applicable: no transactions flow through the service. API stability: no public API found. Forkability and contributor concentration: Not applicable to a proprietary service; the equivalent protection is that the product is a commodity with many substitutes. The main dependency is upstream: if Ex2 runs on OVHcloud, a change at OVHcloud reaches Ex2's customers. Foundation-model dependency: Not applicable.

## Gates

| Gate | Result | Evidence | Notes |
|------|--------|----------|-------|
| Telemetry or content exposure that cannot be disabled | No failure found | Inferred | The owner controls the OS; a custom-ISO install avoids provider agents. Needs the outbound-traffic trace |
| Licence incompatible with the project | No failure found | Inferred | The terms make the customer responsible for content; no clause claiming rights over it was found |
| Accessibility floor | Unknown, at risk | none | Server side passes over SSH; the console used for recovery is untested and may be image-only |
| Exit | Pass | Inferred | Full root, open formats, provider-neutral rebuild |

No gate has failed, and none is Verified, so the entry stays in Assess.

## Scorecard

- Product or service name: Ex2 Cloud VPS, Cloud Performance line
- Version evaluated: plans listed April 2026; terms and privacy policy as published on 2026-11-06 (undated)
- Evaluator: Claude Opus 5.5 (Anthropic), automated pass for Christopher Steel
- Date: 2026-11-06
- Radar ring: Assess

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Unknown | none | Web surfaces and console untested; no accessibility statement |
| Accessibility, text and terminal interfaces | Strong | Inferred | Debian over SSH; row split from the one above, as allowed |
| Multilingual integration | Strong | Claimed | French, English, Spanish |
| Economic and cognitive accessibility | Strong / Weak | Claimed / Inferred | Cheap; unmanaged VPS is hard for non-technical owners |
| Representation | Unknown | none | Nothing public |
| Compatibility | Strong | Claimed | Custom ISO, broad OS list, IPv6 |
| Resilience | Moderate | Inferred | Static content; no provider backups, no SLA |
| Agency, sovereignty & privacy | Moderate | Claimed / Inferred | Root control, Law 25; sub-processors and DC operator unnamed |
| Agency, power-imbalance proxies | Strong / Unknown | Inferred | Exit cost 2 to 4 hours; full portability; ToS volatility unknown; renewal at current rates |
| Agency, interaction patterns | Moderate | Claimed | Costs disclosed only in terms; monthly plans non-refundable |
| Environment, direct, indirect and authoring-side | Moderate | Claimed | Low-carbon grids published; inconsistent offset claims |
| Security | Moderate | Claimed | 2FA on account; no disclosure policy or certification found |
| Longevity | Moderate | Claimed | Since 2013, ownership not stated |
| Content endurance | Strong | Inferred | Server holds copies only |
| Exit and portability | Strong | Inferred | Gate pass |
| Adjustability and support | Moderate | Claimed | 24/7 trilingual chat; no provisioning API |
| Market position | Moderate | Inferred | Commodity market; possible upstream dependency on OVHcloud |
| Gates | No failure found | Inferred | Accessibility floor Unknown, at risk |

## Relationship to project

Hosting layer for the ex2-base static-site server and, through it, for sites published from the SAT archive. On adopt, the entry graduates to the ex2-base documentation as the provider record, alongside the open questions for Ex2 in its README.

## Status notes

- Last reviewed: 2026-11-06
- In assess: light pass done on public documents. No gate fails; none is Verified. Testing still needed, on a disposable instance:
  - Outbound-traffic trace of a fresh instance from Ex2's Debian image and from a custom-ISO install, 24 hours with nothing running, using the uc-radar telemetry trace procedure; list any guest agents with `dpkg -l`.
  - Screen reader pass, Orca or NVDA, on ordering, the client centre, the VPS panel and the console, including a login and a reboot from the console. This decides the accessibility gate.
  - Test export: rsync the whole filesystem off, and confirm a disk image can be downloaded or taken.
  - Ask Ex2 in writing: the data centre operator for each location, the sub-processor list, whether snapshots, a provider firewall, reverse DNS and cloud-init are offered, and how term changes are announced.
  - ToS volatility: diff archived snapshots of the terms and privacy policy to count changes over the available history.
- Proposed trial, once the above is Verified: one static site on the Cloud Performance 1 vCPU plan in the Canadian location, deployed with ex2-base, for 90 days. End condition: 90 days pass, or any gate fails, or an outage longer than 24 hours. Questions for the trial: real uptime, support response in practice, and whether unattended-upgrades reboots and the console behave as documented.

## Resources

### Universal Cake

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)
- [UC Radar Entry Template](#ref-uc-template)

### Ex2 documents

- [Ex2 terms and conditions](#ref-ex2-terms)
- [Ex2 privacy policy](#ref-ex2-privacy)
- [Ex2 about page](#ref-ex2-about)
- [Ex2 Canadian landing page](#ref-ex2-ca)
- [Ex2 two-factor authentication article](#ref-ex2-2fa)

### Third-party listings and reviews

- [WHTop, Cloud Performance plan](#ref-whtop-perf)
- [WHTop, Cloud regular plan](#ref-whtop-regular)
- [Clubic review](#ref-clubic)
- [Clubic product sheet](#ref-clubic-fiche)
- [HelloSafe review](#ref-hellosafe)

## References

<a id="ref-clubic-fiche"></a>Clubic. (n.d.). *Qu'est-ce que Ex2 ?* Retrieved November 6, 2026, from https://www.clubic.com/telecharger-fiche439395-ex2.html
[Return to citation](#cite-clubic-fiche)

<a id="ref-clubic"></a>Clubic. (2023). *Avis Ex2 : un hébergement francophone de premier plan*. https://www.clubic.com/pro/it-business/hebergement-site-web/hebergement-dedie-internet/avis-410916-avis-ex2-test-2023-un-hebergement-francophone-de-premier-plan.html
[Return to citation](#cite-clubic)

<a id="ref-ex2-2fa"></a>Ex2. (2023, February 13). *How to enable the two-factor authentication on your Ex2 account?* Ex2 Knowledge Base. https://help.ex2.com/en/article/u50cy1/
[Return to citation](#cite-ex2-2fa)

<a id="ref-ex2-about"></a>Ex2 Inc. (n.d.-a). *About us*. Retrieved November 6, 2026, from https://www.ex2.com/en/company/about-us?lang=en
[Return to citation](#cite-ex2-about)

<a id="ref-ex2-ca"></a>Ex2 Inc. (n.d.-b). *Canada landing page*. Retrieved November 6, 2026, from https://www.ex2.com/ca-landing.php
[Return to citation](#cite-ex2-ca)

<a id="ref-ex2-privacy"></a>Ex2 Inc. (n.d.-c). *Privacy policy*. Retrieved November 6, 2026, from https://www.ex2.com/privacy-policy?lang=en
[Return to citation](#cite-ex2-privacy)

<a id="ref-ex2-terms"></a>Ex2 Inc. (n.d.-d). *Terms and conditions*. Retrieved November 6, 2026, from https://www.ex2.com/terms-and-conditions?lang=en
[Return to citation](#cite-ex2-terms)

<a id="ref-hellosafe"></a>HelloSafe. (n.d.). *Faut-il choisir Ex2 pour héberger son site web au Canada ?* Retrieved November 6, 2026, from https://hellosafe.ca/hebergeurs/ex2
[Return to citation](#cite-hellosafe)

<a id="ref-uc-metrics"></a>Steel, C. (2026a). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake, uc-radar repository. https://github.com/steelcj/uc-radar
[Return to citation](#cite-uc-metrics)

<a id="ref-uc-template"></a>Steel, C. (2026b). *UC radar entry template* (Version 0.3.0) [Unpublished manuscript]. UniversalCake, uc-radar repository. https://github.com/steelcj/uc-radar
[Return to citation](#cite-uc-template)

<a id="ref-whtop-perf"></a>WHTop. (2026a, April 3). *Ex2 Cloud Performance 1 vCPU 1 GB 25 GB*. https://www.whtop.com/plans/ex2.com/146020
[Return to citation](#cite-whtop-perf)

<a id="ref-whtop-regular"></a>WHTop. (2026b, April 3). *Ex2 Cloud 2 vCPU 4 GB 80 GB*. https://www.whtop.com/plans/ex2.com/146017
[Return to citation](#cite-whtop-regular)

## License

This document, *Ex2 Cloud VPS, Universal Cake Evaluation*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring light pass against metrics 0.6.0, on public evidence only |

---
dcterms:title: "uc-radar--desec--dns-hosting"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude Opus 5.5 (Anthropic)"
dcterms:subject:
  - "radar"
  - "dns"
  - "dnssec"
  - "dns-hosting"
  - "dyndns"
  - "desec"
dcterms:description: "Assess entry for deSEC, a free, non-profit, DNSSEC-by-default authoritative DNS hosting service run by deSEC e.V. in Berlin, scored against Universal Cake Evaluation Metrics 0.6.0."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-11-03"
dcterms:modified: "2026-11-03"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: ""
dcterms:relation: "uc-radar--evaluation-lifecycle, universal-cake-evaluation-metrics, uc-radar-entry-template"
dcterms:identifier: "uc-radar--desec--dns-hosting"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-03"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial entry, created from uc-radar-entry-template v0.3.0 and scored
      against Universal Cake Evaluation Metrics 0.6.0. Light pass on
      documentation and source evidence: the desec-stack repository as of
      commit of 2026-10-13, its API documentation, terms of use and privacy
      policy as published in that repository, plus public incident records.
      Terms-of-service volatility and contributor concentration were
      measured from git history and are tagged Verified. No hands-on test of
      the live service yet.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # once every gate and the Security section are Verified, or a named person records an override
      reasoning: "Light pass against metrics 0.6.0. Telemetry gate passes on policy and source evidence: no cookies, trackers or externally loaded dependencies; server logs only. Licence gate passes, MIT. Exit gate passes: full zonefile export through API and web interface, standard format, multi-signer support for a no-downtime move; DNSSEC private keys stay with deSEC, so leaving needs a DS change at the registrar. Accessibility floor Unknown: a complete text path exists through the API, curl and the Debian-packaged desec-dns client, but the web interface and the captcha path are unaudited. Security Strong in architecture, Moderate in disclosure process. Trial recommended rather than direct adopt because DNS is critical-path infrastructure for clients, the service is best effort with no warranty, and about 80 percent of the last year's human commits come from one person."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-03T15:45:00-05:00"
  human: []
---

# uc-radar--desec--dns-hosting-v0.1.0

- Product or service name: deSEC, hosted authoritative DNS with automatic DNSSEC and dynDNS (dedyn.io), at https://desec.io/
- Version evaluated: the hosted service as described by the desec-stack repository at its 2026-10-13 commit, API v1 (stable); no account created or zone tested yet
- Evaluator: Claude Opus 5.5 (Anthropic), for Christopher Steel's review
- Date: 2026-11-03
- Radar ring: Assess

## What it is

deSEC is a free authoritative DNS hosting service run by deSEC e.V., a registered non-profit association in Berlin. Every zone is signed with DNSSEC automatically, and records are managed through a REST API or a web interface. It is not a registrar: domain owners keep their domain at their registrar and point its nameservers at deSEC. The whole service stack, PowerDNS behind a Django API with a Vue web interface, is published as open source under the MIT licence <a id="cite-desec-stack-2026"></a>([deSEC e.V., 2026](#ref-desec-stack-2026)).

## Why interesting

DNS is the one piece of a client's web presence that has to stay independent of the web host. Putting it with a non-profit that charges nothing, has no premium tier and cannot be acquired in the usual commercial sense removes a common lock-in point, and it fits the Vishpala aim of infrastructure that can be migrated over time.

DNSSEC without effort. deSEC's own pitch is "Let's Encrypt, but for DNSSEC": key generation, signing and re-signing are automatic, and the service publishes CDS and CDNSKEY records so registries that support it can pick up the chain of trust without manual DS entry <a id="cite-thomassen-2023"></a>([Thomassen, 2023](#ref-thomassen-2023)). DNSSEC also unlocks DANE and TLSA records for mail and other services.

Automation-first. Everything the web interface does is available through a documented, versioned API, and the ecosystem already has the pieces the house stack needs: a Caddy DNS module (`caddy-dns/desec`, built on `libdns/desec`) for ACME DNS-01 certificates, which matters given Caddy's strong score on these metrics; a certbot plugin; an octoDNS provider; and the `desec-dns` command-line client, packaged in Debian <a id="cite-debian-desec-dns"></a>([Debian Python Team, n.d.](#ref-debian-desec-dns)).

Resilience by design. Two separate anycast networks serve every zone, `ns1.desec.io` and `ns2.desec.org`, under two different top-level domains, with frontends on several continents, including New York, Dallas and Los Angeles for North American clients <a id="cite-desec-stack-2026-b"></a>([deSEC e.V., 2026](#ref-desec-stack-2026)). The May 2026 `.de` DNSSEC outage, caused by a faulty key rollover at the registry, showed why nameserver names that do not all depend on one TLD are worth having <a id="cite-denic-2026"></a>([DENIC, 2026](#ref-denic-2026)).

## Concerns

Best effort, no warranty. The terms state plainly that availability is not guaranteed and that quality of service is not assured, even at a low level <a id="cite-desec-terms"></a>([deSEC e.V., n.d.](#ref-desec-terms)). For a client whose site carries critical information, for example an emergency procedures site, this is the deciding concern, not a footnote.

Small team, high concentration. In the 12 months to 2026-11-03, 98 of roughly 120 human commits to desec-stack, about 80 percent, came from one person, Peter Thomassen; across the repository's history since 2014 his share is about 68 percent (1,214 of 1,777 non-bot commits). Bus factor is effectively two. Funding is donations and grants (NLnet NGI Assure, a 2023 RIPE NCC Community Projects Fund grant, earlier a German federal research grant) plus support from SSE, Thomassen's security consultancy, and individual and company sponsors who adopt anycast frontends.

Clients will need updates. The terms reserve the right to make breaking changes, usually announced weeks ahead, with time-critical changes on shorter or no notice ([deSEC e.V., n.d.](#ref-desec-terms)). For automation built on the API, pin to the stable v1 API, which per the published lifecycle is supported at least six months after v2 is declared stable.

Account and domain policies to plan around. A new account can create one domain at first; the limit grows as domains are delegated with a working DNSSEC chain, and support can raise it for legitimate use. Domains that do not delegate to deSEC or fail to establish DNSSEC may be deleted after four weeks' warning, and dynDNS names not updated for six months are deleted after a warning. Users must read and respond to deSEC's emails, so the account email belongs on a monitored, shared mailbox, not one person's inbox.

English only. The web interface and documentation are English, with no localisation framework in the web app; the legally binding privacy policy is in German.

Leaving means a DNSSEC hand-off. Records export cleanly, but signing keys stay with deSEC. Moving away means changing DS records at the registrar in the right order, or using deSEC's multi-signer support (RFC 8901) for a no-downtime transition. Done wrong, a DNSSEC move takes a domain offline for validating resolvers.

Watch item, `.io`. One of the two nameserver names sits under `.io`, whose long-term administration has been under discussion since the 2024 agreement on the Chagos Islands. The second network under `.org` mitigates this; record it as a re-review trigger rather than a concern today.

Quebec Law 25. Account data, essentially an email address and connection logs, rests in Germany under the GDPR. Zone data is public by nature, but where a Quebec client's own account is created, the privacy assessment for communicating personal information outside Quebec should note deSEC. This is Inferred and needs checking against the client's own Law 25 practice.

## Gates

The Assess light pass requires all four default gates and the Security section on available evidence. None is Verified by hands-on testing yet.

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The privacy policy states no cookies, no tracking or behavioural analytics, and no dependencies loaded from external hosts; the web app self-hosts its fonts and icons. Server logs record IP address and browser details for operation and security. Account registration facts (email, IP, user agent) can be passed to a separate abuse "gatekeeper" service, added September 2026; the shipped arbiter is a local email blocklist, but the design lets the operator consult outside sources. Zone records are public DNS data by design, not content exposure. Outreach emails are on by default and can be turned off. To Verify with the uc-radar telemetry trace procedure on the web interface and API |
| A licence incompatible with the project | Pass | Inferred | MIT for desec-stack. Using a hosted service raises no licence obligations for the adopting project; MIT only matters if self-hosting or forking. To check against the adopting project's licence policy |
| Failure of the accessibility floor | Unknown | Inferred, not observed | A complete text path exists: account registration, domains, records, tokens and export through the API with `curl` or `desec-dns`, with documentation on Read the Docs that does not need JavaScript. Registration needs a captcha; the web interface offers an audio captcha, and the API's captcha model supports an audio kind, but the API documentation only describes the image captcha. Activation is by email link or a `POST`. The Vue 3 and Vuetify 3 web interface was migrated in July 2026 with styling fixes in September and has not been audited. To test: a screen reader pass on registration (audio captcha), login with 2FA, adding a record and exporting a zone, and a terminal screen reader pass on the API path |
| No exit, data or content cannot be extracted in usable form | Pass | Inferred | Zonefile export through `GET /api/v1/domains/{name}/zonefile/` and a download button per domain in the web interface, plain-text zone format; records are also readable as JSON. Zonefile import exists for the reverse direction. Private DNSSEC keys are not exportable, see Exit and portability. To Verify with a test export and re-import into another server such as PowerDNS or Knot |

## Evaluation

Ratings use Strong, Moderate, Weak, Unknown, and, narrowly, Not applicable, each tagged Verified, Inferred, or Claimed. Verified items were measured from git history of the public repository on 2026-11-03; nothing has been tested against the live service.

### Inclusive

#### Accessibility

##### Alternative methods of interacting with content

- Rating: Unknown
- Evidence: Inferred
- Notes: Lens user (here, the owner or administrator editing DNS; site visitors never see deSEC). The web interface is a JavaScript single-page app built on Vuetify 3, which has reasonable accessible defaults but is only as good as its use; a quick count found few explicit ARIA attributes in the app source. The world map of anycast servers has alt text. An audio captcha alternative is a good sign that assistive technology users were considered. Not audited for WCAG 2.2.

##### Text and terminal interfaces

- Rating: Strong, pending a screen reader pass
- Evidence: Inferred
- Notes: Lens user, owner. Every task can be done in text: the API returns JSON with human-readable error messages, and throttled requests return a plain explanation plus a `Retry-After` header. The `desec-dns` client and the Rust `desec_cli` both offer quiet options; `desec-dns` handles rate limits by waiting and retrying. No required step depends on a full-screen interface or mouse, with the open question of how a terminal user obtains and plays the audio captcha, which is undocumented. Documentation is plain Sphinx on Read the Docs.

##### Multilingual integration

- Rating: Weak
- Evidence: Verified for the web app (no localisation library in `www/webapp/package.json`, `lang="en"`), Inferred otherwise
- Notes: Lens user, community. English only for interface and docs. Adding a language would be a code change, not a data file. For Quebec clients this affects only the owner-facing interface, since DNS is invisible to visitors.

##### Affordability

- Rating: Strong
- Evidence: Claimed (deSEC), Inferred (no paid tier in source or docs)
- Notes: Free, with no paid tier. Donations welcome, never required.

##### Hardware and connectivity accessibility

- Rating: Strong
- Evidence: Inferred
- Notes: The API works from any machine with `curl`, on slow connections, with tiny payloads. The web interface is a heavier JavaScript bundle but optional.

##### Cognitive accessibility

- Rating: Moderate
- Evidence: Inferred
- Notes: Automatic DNSSEC removes the hardest part of secure DNS. DNS itself remains specialist knowledge, and the one manual step, entering DS records at the registrar where CDS automation is not supported, is where people go wrong. Error messages are factual and explain what to do.

##### Time-based media

- Rating: Not applicable for the product, Unknown for talks
- Evidence: Inferred
- Notes: The service has no audio or video content apart from the audio captcha. Conference talks about deSEC exist; captions not checked.

##### Resizing and reflow

- Rating: Unknown
- Evidence: None
- Notes: Vuetify layouts are responsive in principle; not tested at 200 percent zoom.

##### Compatibility

- Rating: Strong
- Evidence: Inferred
- Notes: Standards-based DNS; works with any registrar that accepts nameserver and DS changes. IPv4 and IPv6. Integrations exist for Caddy, certbot, lego, octoDNS, Plesk and Nextcloud VM.

#### Representation

- Rating: Unknown
- Evidence: None
- Notes: Lens community. A small volunteer and consultancy-backed team; no public information on disabled representation or user research. Its absence from public documentation is itself a data point.

#### Resilience

- Rating: Moderate
- Evidence: Inferred
- Notes: Lens owner, user. Out of date: not applicable in the usual sense, since DNS is a live service; there is no licence server or expiring token, but the service must exist for names to resolve. Power and network outages: two anycast networks under different TLDs with frontends on several continents, and resolvers cache answers for the TTL, so short deSEC outages are cushioned. Public incident records show a 7-hour DNSSEC replication problem in June 2020, a 43-minute DDoS in January 2023, and a 2-hour API and web outage in May 2026 with no published cause <a id="cite-pingoru-2026"></a>([Pingoru, 2026](#ref-pingoru-2026)); whether DNS answers were affected in May 2026 is Unknown. No documented way to add a user's own secondary nameserver; multi-signer with a second provider is the route to provider redundancy. No warranty.

### Agency

#### Sovereignty & Privacy

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. The domain owner keeps control at the registrar and can point nameservers elsewhere at any time. The stack can be self-hosted, modified and forked under MIT, though running it is a serious undertaking (Docker Compose, PowerDNS, a VPN to frontends, anycast). Data at rest: account data in Germany under the GDPR; zone data is public. No phone-home concerns for a hosted service beyond server logs; see the telemetry gate. When the user and deSEC disagree, the user can leave at low cost. Reversibility test: if deSEC turned hostile tomorrow, an owner could export zones in minutes and move to another provider in a few hours per domain, most of it waiting out TTLs and doing the DNSSEC hand-off carefully.

##### Power-imbalance proxies

- Exit cost: Low. Minutes to export; one to a few hours of careful work per domain to move, including the DS change. Inferred.
- Data portability: Machine-readable, yes, RFC 1035 zone file and JSON. Human-readable, yes, the zone file is plain text a person can read. DNSSEC private keys excluded by design. Inferred.
- Terms-of-service volatility: Strong. One material change in the five years to 2026-11-03: in December 2024 the transfer-to-proven-owner clause changed so that a domain claimed by its registry-proven owner may now be transferred with its records rather than always purged, and the dynDNS expiry clause was merged into the server-load section. All other edits to the terms file were styling and refactoring. Verified, method: `git log --follow` and diff of `www/webapp/src/views/TermsPage.vue` in desec-stack, 2026-11-03. Note that the terms do not describe how users are told of changes.
- Pricing asymmetry: Strong. Free for everyone, public limits, no negotiated tiers. Inferred.

##### AI-specific proxies

- Not applicable. deSEC contains no machine-learned model and makes no algorithmic decisions about users, beyond a rule-based abuse gate on registrations.

#### Interaction Patterns

- Rating: Strong
- Evidence: Inferred
- Notes: Lens user. No upsells, no engagement features, no ads. Two small named limitations: the outreach-email preference defaults to on (no ads, opt-out available), which is a mild vendor-serving default; and account deletion requires deleting all domains first, which is reasonable protection against accidents but more steps than signing up.

### Environment

- Rating: Strong
- Evidence: Inferred
- Notes: Lens planet. Direct: authoritative DNS is one of the lightest services on the internet, and deSEC's frontends are shared across tens of thousands of zones. Indirect: no new hardware for owners. Authoring-side: the API is lighter than the JavaScript web interface; both are modest.

### Security

- Rating: Strong (architecture), Moderate (disclosure and incident communication), pending verification
- Evidence: Inferred
- Notes: Lens owner, user. Data collected: email address, hashed password, connection logs. Architecture: signing keys are generated and held on a hidden primary (`nslord`); the stealth server that feeds the public frontends receives only signed zones and has no key access; replication to frontends runs over a VPN. Accounts support TOTP two-factor authentication, scoped API tokens with per-domain and per-record policies, and PBKDF2 password hashing. Vulnerability reporting: `security.txt` names `security@desec.io`, but there is no published security policy, response-time commitment or advisory history that this pass found. Patching: dependabot is active and commits are frequent (75 in September and October 2026). Supply chain: a standard Python and npm stack with pinned containers; not inspected in depth. Attack surface: moving DNS off the web server shrinks the web host's attack surface and separates the two failure domains. Incidents: see Resilience; communication on the May 2026 outage was open and resolve only.

#### Candidate project gate

For clients with critical-information sites: no DNS provider without a tested exit and DNSSEC hand-off runbook. This is a process gate for Vishpala rather than a property of deSEC.

#### Assessment method

Not yet tested. Done: source reading of desec-stack (API, gatekeeper, web app, terms and privacy pages) and git history analysis on 2026-11-03; public incident records. Planned: a test account on a disposable Linux machine; the uc-radar telemetry trace procedure (version to be named) on the web interface and API; registration with the audio captcha using a screen reader; creating a test zone, adding records, exporting and re-importing into PowerDNS or Knot; a DS hand-off rehearsal on a throwaway domain; checking that `caddy-dns/desec` obtains a certificate with a narrowly scoped token.

#### Network behaviour

- [ ] Outbound connections during normal use (expected: only to desec.io; no third-party hosts)
- [ ] Update checks, silent or explicit (expected: not applicable, hosted service)
- [ ] Telemetry or usage data, present or absent (expected: absent beyond server logs)
- [ ] Licence server contact, frequency and data sent (expected: none)
- [x] Affects networking or items created that will be transferred over a network (DNS answers for every service on the domain)
- [ ] Helps ensure that documents passed via network are clean and compliant (DNSSEC signs answers; DANE and TLSA support mail transport security)

#### File system behaviour

- [ ] Creates hidden or metadata files alongside content (expected: not applicable)
- [ ] Caches content outside the working directory (expected: `desec-dns` stores its token under `$XDG_CONFIG_HOME/desec/`)
- [ ] Stores recent file history outside the archive (expected: none)
- [ ] Writes to unexpected locations (expected: none)

#### Content exposure

- [ ] Sends any content to a remote service (expected: zone records, which are public by design)
- [ ] Stores content in a cloud service by default (expected: yes, that is the service; zones are public DNS)
- [ ] Auto-save or backup features that copy content externally (expected: none)

### The Product or Service Itself

#### Longevity

- Rating: Moderate
- Evidence: Verified for commit activity, Inferred otherwise
- Notes: Lens owner, community. Repository since November 2014, public service since about 2019, hosting tens of thousands of zones and around four billion queries a month by 2023 with three- to fourfold annual growth ([Thomassen, 2023](#ref-thomassen-2023)). Active and accelerating development in 2026. Weighed against that: donation and grant funding, no warranty, and a bus factor of about two. A non-profit is protected from acquisition but not from burnout or a funding gap.

#### Content endurance

- Rating: Strong
- Evidence: Inferred
- Notes: deSEC holds DNS records, not content. If it disappeared, records are recoverable from the owner's own export or from the zone itself, and site content is untouched. Keeping a current export in the client's archive, for example through octoDNS configuration in git, makes this complete.

#### Exit and portability

- Rating: Strong
- Evidence: Inferred
- Notes: See the exit gate. Open standards throughout (DNS, DNSSEC, RFC 8078 CDS and CDNSKEY, RFC 8901 multi-signer). The realistic successor path is any other authoritative DNS host or self-hosted Knot or PowerDNS, with the DS hand-off as the one step needing care.

#### Adjustability and support

- Rating: Strong
- Evidence: Inferred
- Notes: MIT licence keeps the fork-and-fix path open; the codebase is readable Django and is well tested. Documentation is clear and complete. Support through a community forum and email; maintainers are publicly reachable and present at RIPE and ICANN meetings. No paid support tier.

#### Market Position

- Rating: Moderate
- Evidence: Verified for contributor concentration, Inferred otherwise
- Notes: Lens community, owner. Market concentration: deSEC is a small player in a market dominated by Cloudflare and the large clouds, which is a benefit for the community lens, since it is not a platform that can tax anyone. Take rate: none. API stability: v1 stable since launch, with a written lifecycle and a guaranteed notice period before deprecation. Forkability: the code is forkable, but the service's value is its anycast network and nameserver names, which a fork could not take; owners are protected by low exit cost rather than by forkability. Contributor concentration: about 80 percent of human commits in the last 12 months and 68 percent all-time from one person (Verified, `git log --no-merges` on desec-stack, 2026-11-03).

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Unknown (web interface), Strong (text interface) | Inferred | Split into the two gated items; audio captcha present; no audit yet |
| Multilingual integration | Weak | Verified (web app), Inferred | English only; binding privacy policy in German |
| Economic and cognitive accessibility | Strong (affordability, hardware), Moderate (cognitive) | Inferred | Free; DS step at registrar is the hard part |
| Representation | Unknown | None | Not assessed |
| Compatibility | Strong | Inferred | Standards-based; Caddy, certbot, octoDNS integrations |
| Resilience | Moderate | Inferred | Two anycast networks on two TLDs; best effort, no warranty |
| Agency, sovereignty & privacy | Strong | Inferred | Owner keeps registrar control; GDPR jurisdiction |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified (ToS), Inferred | One material terms change in five years |
| Agency, interaction patterns | Strong | Inferred | Outreach email on by default, opt-out |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | Very light service |
| Security | Strong (architecture), Moderate (disclosure) | Inferred | Hidden signer, 2FA, scoped tokens; no published security policy |
| Longevity | Moderate | Verified (activity), Inferred | Active, but donation-funded with bus factor about two |
| Content endurance | Strong | Inferred | Holds records only; keep exports |
| Exit and portability | Strong | Inferred | Zone export; keys stay, DS hand-off needed |
| Adjustability and support | Strong | Inferred | MIT, good docs, reachable maintainers |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified (contributors), Inferred | Small player, no take rate; high contributor concentration |
| Gates | Pass (telemetry, licence, exit), Unknown (accessibility floor) | Inferred | None Verified; no gate failed |

## Relationship to project

Tier: hosting and infrastructure, DNS layer, beneath the web server. For Vishpala it would become the default DNS host for client domains that want DNSSEC and no commercial DNS dependency, with Caddy obtaining certificates through `caddy-dns/desec` where DNS-01 is needed. On adopt, the process graduates to uc-processes, for example `uc-processes/uc-radar/dns/`, covering:

- Account setup on a shared, monitored mailbox, with 2FA and scoped API tokens per client and per use (one for Caddy, one for octoDNS).
- Zones managed as code with octoDNS in the client's repository, so the repository is the source of truth and the export is always current.
- A tested exit and DNSSEC hand-off runbook, rehearsed once, including the multi-signer route.
- A rule for when deSEC is not enough on its own: for critical-information clients, add a second provider through multi-signer, or use a paid provider with a service level, recorded per client.

## Status notes

- Last reviewed: 2026-11-03
- In assess: light pass done against metrics 0.6.0 on source and documentation evidence. No gate failed; three gates pass on Inferred evidence and the accessibility floor is Unknown. Testing still needed before the entry can leave Assess: telemetry trace of the web interface and API, a screen reader pass on registration with the audio captcha and on record editing, a terminal pass on the API path, and a test export and re-import. Also pending: Representation, whether DNS answers were affected in the May 2026 outage, and a funding check. The automated pass recommends trial once gates and Security are Verified, scoped to one non-critical Vishpala-owned domain for three months, end condition stated before start: no unexplained resolution failures, one completed DS hand-off rehearsal, and Caddy certificate renewal working through the DNS module. Adopt for client domains after that, with critical-information clients assessed separately.

## Links

- https://desec.io/
- https://desec.readthedocs.io/
- https://github.com/desec-io/desec-stack
- https://talk.desec.io/
- https://github.com/s-hamann/desec-dns
- https://github.com/caddy-dns/desec

## References

<a id="ref-debian-desec-dns"></a>
Debian Python Team. (n.d.). *desec-dns* (Version 1.5.0-2) [Debian package control file]. Debian Package Tracker. https://tracker.debian.org/media/packages/d/desec-dns/control-1.5.0-2
[Return to citation](#cite-debian-desec-dns)

<a id="ref-denic-2026"></a>
DENIC. (2026). *Final report: DNS outage of 5 May 2026*. DENIC Blog. https://blog.denic.de/en/final-report-dns-outage-of-5-may-2026/
[Return to citation](#cite-denic-2026)

<a id="ref-desec-stack-2026"></a>
deSEC e.V. (2026). *desec-stack* [Source code, commit of 2026-10-13]. GitHub. https://github.com/desec-io/desec-stack
[Return to citation](#cite-desec-stack-2026)

<a id="ref-desec-terms"></a>
deSEC e.V. (n.d.). *Terms of use*. Retrieved November 3, 2026, from https://desec.io/terms
[Return to citation](#cite-desec-terms)

<a id="ref-pingoru-2026"></a>
Pingoru. (2026). *deSEC incident: API and web services outage*. https://pingoru.io/providers/desec/incidents/1455601
[Return to citation](#cite-pingoru-2026)

<a id="ref-thomassen-2023"></a>
Thomassen, P. (2023, May 23). *deSEC: Secure DNSSEC hosting, continued operations and development* [Conference presentation]. RIPE 86, Rotterdam, Netherlands. https://ripe86.ripe.net/presentations/30-2023-05-23-RIPE-86-deSEC.pdf
[Return to citation](#cite-thomassen-2023)

## License

This document, *uc-radar--desec--dns-hosting*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

---
dc:title: "Universal Cake Evaluation: NetBird, Mesh Network"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Fable 5.2 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "netbird"
  - "mesh network"
  - "wireguard"
dc:description: "Assess-ring evaluation of self-hosted NetBird, Community Edition, against Universal Cake Evaluation Metrics 0.6.0, based on source inspection and documentation. No trace or screen reader pass has been run yet."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-12"
dcterms:modified: "2026-11-12"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/netbirdio/netbird, https://github.com/netbirdio/dashboard"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, uc-radar--evaluation-lifecycle, uc-radar--telemetry-trace-procedure, universal-cake-evaluation--headscale-and-tailscale--mesh-network"
dc:identifier: "universal-cake-evaluation--netbird--mesh-network"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-12"
    author: "Claude Fable 5.2 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial entry. Assess-ring pass against metrics 0.6.0 from source
      inspection of the server, client and dashboard repositories, a
      twelve-month commit count, and vendor and third-party
      documentation. Licence gate Verified by inspection. Telemetry,
      accessibility-floor and exit gates and the Security section are
      Inferred and need testing before the entry can leave Assess.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Fable 5.2 (Anthropic)"
      evaluator_version: "claude-fable-5-2"
      recommendation: "trial" # after Assess testing verifies the gates and Security; second choice behind Headscale with Tailscale for the controller-reach use
      reasoning: "No gate fails on available evidence. Licences Verified by inspection: BSD-3-Clause client, AGPL-3.0 server components and dashboard. The self-hosted server pushes anonymous usage metrics to the vendor every twelve hours by default; a documented switch turns this off. Two further contacts, a version check and a dashboard announcements feed, had no switch that this read could find, so the gate needs a trace. Accessibility floor Unknown: administration is mainly a web dashboard that has not been audited. Exit is re-enrolment plus rewriting policies. Main risks: a larger server-side attack surface, three advisories in thirteen months including one critical, a fast release train with fixes only in the latest version, one relicensing already made possible by a contributor agreement, and a product scope growing well beyond a mesh."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-12T21:00:00-05:00"
  human: []
---

# Universal Cake Evaluation: NetBird, Mesh Network

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates self-hosted NetBird, Community Edition, against version 0.6.0 of the Universal Cake Evaluation Metrics. The use in view is narrow: giving an Ansible controller authenticated reach to nodes wherever they sit. The evidence is source inspection of the server, client and dashboard repositories, a twelve-month commit count, and published documentation and advisories. No gate fails on that evidence, and the licence gate is Verified. NetBird's strengths over the alternative evaluated alongside it are a complete web dashboard, a client interface translated into sixteen languages including French, and a copyleft server licence that matches Universal Cake's own. Its costs are a heavier server, a larger attack surface, and a vendor relationship with more leverage on the vendor's side: usage metrics are on by default, the licence has changed once, and several operational features sit behind a commercial licence. Testing must settle the telemetry gate and the accessibility of the dashboard before the entry can leave Assess.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Source evidence comes from the NetBird repository <a id="cite-netbird-repo"></a>([NetBird, 2026a](#ref-netbird-repo)) at commit `ffd315e` and the dashboard repository <a id="cite-netbird-dashboard"></a>([NetBird, 2026b](#ref-netbird-dashboard)) at commit `3dea546`, both read on 2026-11-12. Licensing history comes from the vendor's announcement <a id="cite-netbird-agpl"></a>([NetBird, 2025](#ref-netbird-agpl)), and the split between free and commercial features from the vendor's licence documentation <a id="cite-netbird-enterprise"></a>([NetBird, n.d.](#ref-netbird-enterprise)) and a third-party summary <a id="cite-wzit-netbird"></a>([WZ-IT, 2026](#ref-wzit-netbird)). Advisory details come from the vendor's own write-up <a id="cite-netbird-advisory"></a>([NetBird, 2026c](#ref-netbird-advisory)) and public advisory databases, cited where used. The evaluation was prepared with AI assistance from Claude Fable 5.2 (Anthropic).

## Scope

NetBird is a mesh network built on WireGuard. Each node runs a client. A management server holds accounts, peers, groups and access policies; a signal server helps peers find each other; a relay carries traffic when a direct connection cannot be made. A web dashboard is the main administrative interface, and an embedded identity provider handles logins. NetBird GmbH, in Berlin, develops it and also sells a hosted service and a commercial self-hosted licence.

In scope:

- **The self-hosted Community Edition server**, latest tag 0.83.0: the combined server, which bundles management, signal and relay, with the embedded identity provider.
- **The dashboard**, latest tag 2.97.0.
- **The Linux client**, the `netbird` command and daemon, same version as the server.

Not in scope: NetBird Cloud, the commercial self-hosted licence, the graphical desktop and mobile clients beyond their translation files, and features outside mesh connectivity, such as the reverse-proxy services, browser SSH and RDP, and the newer AI agent features.

## Scorecard metadata

- Product or service name: NetBird, self-hosted Community Edition
- Version evaluated: NetBird 0.83.0; dashboard 2.97.0. Source read at the head of each main branch on 2026-11-12
- Evaluator: Claude Fable 5.2 (Anthropic), drafted for Christopher Steel's review. No human pass yet
- Date: 2026-11-12
- Radar ring: Assess, light pass and source inspection complete, testing not started
- Test platform: none yet. Source was read in a disposable Linux workspace; nothing was installed or run
- Assessment method: source and configuration inspection with `git` and `grep`; commit counts from `git shortlog` over a partial clone limited to commits since 2025-11-12; documentation review. The uc-radar telemetry trace procedure, version 0.1.0, has not been run

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass, provisional | Inferred, from source inspection | The management server pushes anonymous usage metrics to `metrics.netbird.io` every twelve hours by default. `disableAnonymousMetrics: true` in the server configuration turns this off. Two other contacts had no switch that this read could find: a version check by server and client against `pkgs.netbird.io`, and an announcements feed the dashboard fetches from GitHub in the administrator's browser. Neither carries data about the network, but both reveal an address and that NetBird is in use. See Findings |
| A licence incompatible with the project | Pass | Verified, by inspection, 2026-11-12 | Client and shared code BSD-3-Clause. The `management`, `signal`, `relay`, `combined` and `proxy` directories and the dashboard are AGPL-3.0, each read from its own `LICENSE` file. Compatible with AGPL-3.0-or-later work. Running an unmodified server adds no obligation; a modified one must offer its source to its network users. Confirm against Vishpala's own licence policy |
| Failure of the accessibility floor | Unknown | Inferred | Two interfaces need testing. The dashboard is a React web application and was not audited; no accessibility statement was looked for or found. The client command line has JSON and YAML output for status; no `NO_COLOR` handling was found in its command source. A public API and setup keys suggest administration and enrolment are possible without the dashboard, not confirmed. Needed: a screen reader pass on the dashboard and a terminal screen reader pass on the client |
| No exit | Pass | Inferred | Nothing but accounts, peers, groups and policies is stored, in SQLite by default. Leaving means enrolling nodes with another control plane and rewriting policies, which are not in a format another product reads |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Unknown | Inferred | See the accessibility-floor gate. More people can administer a mesh through a dashboard than through a command line, but only if the dashboard works with assistive technology |
| Multilingual integration | Moderate | Verified, by inspection | The client interface has sixteen locale files, among them French, Spanish, German, Hebrew, Japanese and Chinese, managed through Crowdin, so the community can add languages without code changes. No translation framework was found in the dashboard's dependencies, so the administrative interface appears to be English only |
| Economic and cognitive accessibility | Moderate | Inferred | The Community Edition is free. The server needs a public domain and a machine able to run several services. The dashboard lowers the skill needed for day-to-day administration. There are more concepts to learn than a controller-reach mesh requires |
| Representation | Unknown | Inferred | Nothing public on disabled representation or user research. Per the metrics, that absence is itself a data point |
| Compatibility | Strong | Claimed, project README | Clients listed for Linux, FreeBSD, macOS, Windows, Android, iOS, OpenWRT and Docker |
| Resilience | Moderate | Claimed, vendor documentation | Peer traffic runs directly between peers or through the relay and does not pass through the management server. The default deployment is one combined container, and restarting it drops relayed connections. High availability needs the commercial licence |
| Agency, sovereignty and privacy | Strong, once configured | Inferred | The owner runs the whole control plane and holds accounts, keys and policy. Out of the box the server reports usage counts to the vendor |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Moderate | Mixed | Exit cost is an estimate: half a day to a day, for a re-enrolment play plus rewriting policies. Portability through the API, machine-readable. One licence change in the last five years, material, from BSD-3 to AGPL-3.0 on the server side. Commercial self-hosted pricing is reported by a third party, not read from the vendor |
| Agency, interaction patterns | Moderate | Inferred, from source inspection | Usage metrics are opt-out, a default that serves the vendor, though the server logs that it is doing so. The dashboard shows vendor announcements and suggests installing the client by piping a script into a shell. Tracker hooks exist in the dashboard and appear inert unless configured |
| Environment, direct, indirect, and authoring-side | Moderate | Claimed (kernel WireGuard), Inferred (the rest) | The client uses kernel WireGuard where available, the efficient path. The server side is heavier: combined server, dashboard, identity provider and a reverse proxy. The dashboard's load on a modest administrator machine was not measured |
| Security | Moderate | Mixed, see notes | Published security policy with private reporting and public advisories, Verified by inspection of `SECURITY.md`. Three advisories in thirteen months, one critical and one high. Fixes ship only in the latest release. The server exposes more services than the alternative. See Findings |
| Longevity | Moderate | Verified (commit counts), Claimed (funding) | Company-backed, with a US$10 million Series A in January 2026 <a id="cite-techeu-netbird"></a>([Tech.eu, 2026](#ref-techeu-netbird)). Very active: 1,193 commits from 89 authors in twelve months, and about thirty minor versions since August 2025. Still pre-1.0 |
| Content endurance | Not applicable | Inferred | Absent: authored content. The product carries traffic and stores only accounts, peers, groups and policies |
| Exit and portability | Moderate to Strong | Inferred | See the No exit gate. The data plane is WireGuard, an open protocol; the control protocol and policy model are NetBird's own |
| Adjustability and support | Moderate | Verified in part, by inspection | The fork-and-fix path is open under BSD-3 and AGPL-3.0. Contributions require signing a contributor agreement with NetBird GmbH. Documentation is extensive |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified (commit counts), Inferred (the rest) | Open core under one company. The top five authors wrote about 70% of the last year's commits; their employer cannot be read from commit addresses, so concentration by company is Unknown and probably high. No take rate. Foundation model dependency is not applicable while the AI agent features are unused |
| Gates | Pass, one Unknown | One Verified, three Inferred | Licence Verified. Telemetry and exit pass on inspection only. Accessibility floor Unknown pending screen reader passes |

## Findings

### Usage metrics are on by default

The self-hosted management server sends a usage report to `metrics.netbird.io` every twelve hours unless told not to. The source lists what the report contains: about seventy properties, almost all of them counts. They include the number of accounts, users, peers, groups, policies, routes and nameservers; server and peer versions; uptime; the database engine and identity provider type; and usage counters for newer features. No hostnames, addresses or policy contents appear in the list. The vendor still learns the server's address from the connection itself, and how large and how active the deployment is.

The switch is `disableAnonymousMetrics: true` in the combined server's configuration, with an equivalent command-line flag and the `NETBIRD_DISABLE_ANONYMOUS_METRICS` variable in the older deployment files. The example configuration ships with it set to false.

This passes the gate, because it can be disabled. Under Interaction Patterns it is a preselected default that serves the vendor. Set the switch before the server first starts.

### Other outbound contacts

Four more were found in source. A trace should confirm each one:

- **Version check.** The management server and the client both fetch the latest version number from `pkgs.netbird.io`. No configuration switch was found in this read. It could be blocked at the firewall.
- **GeoLite database.** The management server downloads a geolocation database from `pkgs.netbird.io` for its location features. `disableGeoliteUpdate: true` turns this off.
- **Dashboard announcements.** The dashboard fetches an announcements file from GitHub in the administrator's browser, cached locally between fetches. No switch was found.
- **Tracker hooks in the dashboard.** The dashboard source has hooks for Hotjar, Google Tag Manager, Google Analytics, Matomo and HubSpot, each driven by an environment variable with no default value. They appear to exist for the vendor's hosted service and to do nothing in a self-hosted build unless set. Confirm in the trace by watching the browser's network requests.

### The licence changed once, and can change again

In August 2025, with version 0.53.0, NetBird moved its management, signal and relay components and the dashboard from BSD-3-Clause to AGPL-3.0, keeping the client under BSD-3 ([NetBird, 2025](#ref-netbird-agpl)). For Universal Cake the new licence is a better fit than the old one. What matters for the power-imbalance proxies is how it was done: the announcement says the contributor agreement gave the company the right to relicense the collective work. That right remains. A future move to a licence that is not open source would leave the last AGPL release forkable, but the community would have to carry it.

The commercial boundary has also been drawn. High availability, SCIM provisioning and traffic-flow logging on a self-hosted server need the commercial licence, and that edition contacts a vendor licence service ([NetBird, n.d.](#ref-netbird-enterprise)). A third party reports the entry tier at 2,000 euros a year ([WZ-IT, 2026](#ref-wzit-netbird)). None of those features is needed for controller reach.

### Three advisories in thirteen months

- **October 2025, critical.** CVE-2025-10678: installations made with the vendor's script left a default administrator password in place in the identity provider the script set up, fixed in 0.57.0 <a id="cite-osv-10678"></a>([OSV, 2025](#ref-osv-10678)).
- **April 2026, medium.** A race condition let an administrator escalate to owner <a id="cite-gitlab-rxmp"></a>([GitLab Advisory Database, 2026](#ref-gitlab-rxmp)).
- **July 2026, high.** The client daemon's local control interface had no authentication, so any local user could change security settings, disconnect the node or move it to another account. Every client release from 0.5.0 in March 2022 to 0.75.1 was affected. It was fixed in 0.76.0 on 2026-07-29, with an advisory and a plain-language write-up published together ([NetBird, 2026c](#ref-netbird-advisory)).

The handling of the third is to the vendor's credit: clear, prompt and public. The findings themselves say something about maturity. A missing access check on a root daemon went unnoticed for four years, and an install script shipped a default password.

Two structural points add to this. The security policy supports only the latest release, so every fix arrives bundled with whatever else changed, and the release train is fast. And the server exposes more than a mesh needs: a management API, a dashboard, an identity provider, a relay and a reverse proxy, with reverse-proxy services, browser-based remote access and AI agent features now in the same codebase. Each is more code facing the network.

### The product is growing beyond a mesh

The metrics payload is a fair map of where development is going: counters for proxy clusters and custom domains, for AI agent networks with token and cost tracking, and for MCP tool calls. None of that is a problem in itself, and none of it needs to be enabled. It does mean the product being evaluated is becoming a broader access platform. For a practice that wants a small, stable transport under Ansible, that direction adds upkeep and review work without adding value. For a client who wants one console for remote access, it may be the reason to choose NetBird.

### Administration depends on the dashboard

NetBird's main advantage over a command-line control server is also its accessibility risk. If the dashboard works well with a screen reader, keyboard-only use and zoom, NetBird widens who can run a mesh. If it does not, a blind administrator is pushed to the API, and the product's central convenience is unavailable to them. Nothing in this pass can tell which is true. The dashboard test is therefore the most informative single test for this entry.

## Recommendation

Keep in Assess and proceed to testing. For Vishpala's own controller-to-node reach, NetBird is the second choice behind Headscale with the Tailscale client: it does the same job with more moving parts, more exposed services and more vendor leverage. It becomes the first choice where a deployment needs a web console, several administrators, or an interface in French or another supported language, provided the dashboard passes the accessibility floor.

If testing verifies the gates and the Security section, move to Trial, not straight to Adopt, for the same reason as the alternative: this is high-risk infrastructure.

Configuration to test and, if it holds, to carry into Trial:

- Set `disableAnonymousMetrics: true` and `disableGeoliteUpdate: true` before first start.
- Do not use the quick-start script unreviewed. Read it, pin image versions, and confirm no default credentials remain.
- Run 0.76.0 or newer on every client, and plan for frequent upgrades, since fixes are not backported.
- Leave the NetBird SSH server, reverse-proxy services, browser remote access and AI agent features off.
- Install the client from the signed apt repository at a pinned version, not by piping an install script into a shell.
- Enrol with setup keys, short-lived and single-use where possible, and write a least-privilege policy before enrolling the first node.

Testing needed to leave Assess:

- A telemetry trace of the server, a client and a dashboard session with the configuration above, using the uc-radar telemetry trace procedure. Confirm what the version check sends and whether it can be stopped.
- A screen reader pass on the dashboard, and a terminal screen reader pass on client install and enrolment.
- A test teardown: export peers and policies through the API, move one node to another control plane, and record the time taken.
- A short outage test: restart the combined server and confirm what keeps working.

Re-review triggers specific to NetBird: a further licence change, movement of a currently free feature behind the commercial licence, a funding round or acquisition, a new default outbound contact, and any advisory in the management server or identity provider.

## Relationship to project

Dev toolchain and operations: an alternative transport layer under the Ansible converge pipeline, and a candidate for client deployments that need a web console. On adopt, the tested configuration and the enrolment play would graduate to `uc-processes` as a versioned process.

## Status notes

- Last reviewed: 2026-11-12.
- In assess: light pass and source inspection done. Licence gate Verified. Still needed before the entry moves to trial: Verified tags on the telemetry, accessibility-floor and exit gates and on the Security section, by the four tests listed under Recommendation, or a recorded override by a named person; and a human evaluation pass in `uc-radar_evaluation`.
- Compared in the same pass: Headscale with the Tailscale client, evaluated separately. ZeroTier was set aside as a likely licence-gate failure and has no entry yet.

## License

This document, *Universal Cake Evaluation: NetBird, Mesh Network*, by **Christopher Steel**, with AI assistance from **Claude Fable 5.2 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### NetBird

- [NetBird repository](#ref-netbird-repo)
- [NetBird dashboard repository](#ref-netbird-dashboard)
- [AGPLv3 announcement](#ref-netbird-agpl)
- [Enterprise Commercial License documentation](#ref-netbird-enterprise)
- [Third-party summary of the commercial licence](#ref-wzit-netbird)
- [Series A announcement](#ref-techeu-netbird)

### Advisories

- [CVE-2025-10678](#ref-osv-10678)
- [GHSA-rxmp-8h9v-56cx](#ref-gitlab-rxmp)
- [Client daemon privilege escalation, vendor write-up](#ref-netbird-advisory)

## References

<a id="ref-gitlab-rxmp"></a>GitLab Advisory Database. (2026). *GHSA-rxmp-8h9v-56cx: NetBird has race condition on UpdateUser function, resulting in privilege escalation from admin to owner* [Vulnerability record]. https://advisories.gitlab.com/golang/github.com/netbirdio/netbird/GHSA-rxmp-8h9v-56cx/
[Return to citation](#cite-gitlab-rxmp)

<a id="ref-netbird-enterprise"></a>NetBird. (n.d.). *NetBird Enterprise Commercial License*. Retrieved November 12, 2026, from https://docs.netbird.io/selfhosted/enterprise
[Return to citation](#cite-netbird-enterprise)

<a id="ref-netbird-agpl"></a>NetBird. (2025). *NetBird is embracing the AGPLv3 license*. https://netbird.io/knowledge-hub/netbird-agpl-announcement
[Return to citation](#cite-netbird-agpl)

<a id="ref-netbird-repo"></a>NetBird. (2026a). *NetBird* (Version 0.83.0) [Computer software]. GitHub. https://github.com/netbirdio/netbird
[Return to citation](#cite-netbird-repo)

<a id="ref-netbird-dashboard"></a>NetBird. (2026b). *NetBird dashboard* (Version 2.97.0) [Computer software]. GitHub. https://github.com/netbirdio/dashboard
[Return to citation](#cite-netbird-dashboard)

<a id="ref-netbird-advisory"></a>NetBird. (2026c). *Security advisory: Local privilege escalation via the NetBird client daemon IPC*. https://netbird.io/knowledge-hub/security-advisory-daemon-ipc-local-privilege-escalation
[Return to citation](#cite-netbird-advisory)

<a id="ref-osv-10678"></a>OSV. (2025). *GHSA-g3j4-58mp-3x25* [Vulnerability record for CVE-2025-10678]. https://osv.dev/vulnerability/GHSA-g3j4-58mp-3x25
[Return to citation](#cite-osv-10678)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

<a id="ref-techeu-netbird"></a>Tech.eu. (2026, January 13). *NetBird announces $10M Series A to expand open source VPN alternative*. https://tech.eu/2026/01/13/netbird-announces-10m-series-a-to-expand-open-source-vpn-alternative/
[Return to citation](#cite-techeu-netbird)

<a id="ref-wzit-netbird"></a>WZ-IT. (2026). *NetBird self-hosting licence 2026: What the Commercial Starter at 2,000 euros changes*. https://wz-it.com/en/blog/netbird-self-hosting-license-commercial-starter/
[Return to citation](#cite-wzit-netbird)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring entry against metrics 0.6.0, from source inspection, a twelve-month commit count and documentation. Licence gate Verified; telemetry, accessibility-floor and exit gates and Security await testing |

---
dc:title: "Universal Cake Evaluation: Headscale and Tailscale, Mesh Network"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Fable 5.2 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "headscale"
  - "tailscale"
  - "mesh network"
  - "wireguard"
dc:description: "Assess-ring evaluation of a self-hosted Headscale control server with the open source Tailscale client, against Universal Cake Evaluation Metrics 0.6.0, based on source inspection and documentation. No trace or screen reader pass has been run yet."
dc:publisher: "UniversalCake"
dcterms:created: "2026-11-12"
dcterms:modified: "2026-11-12"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/juanfont/headscale, https://github.com/tailscale/tailscale"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, uc-radar--evaluation-lifecycle, uc-radar--telemetry-trace-procedure, universal-cake-evaluation--netbird--mesh-network"
dc:identifier: "universal-cake-evaluation--headscale-and-tailscale--mesh-network"
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
      inspection of both repositories, a twelve-month commit count, and
      vendor and third-party documentation. Licence gate Verified by
      inspection. Telemetry, accessibility-floor and exit gates and the
      Security section are Inferred and need testing before the entry
      can leave Assess.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Fable 5.2 (Anthropic)"
      evaluator_version: "claude-fable-5-2"
      recommendation: "trial" # after Assess testing verifies the gates and Security; not before
      reasoning: "No gate fails on available evidence. Licence BSD-3-Clause for both parts, Verified by inspection. Telemetry can be switched off on paper: Headscale tells clients not to upload logs by default, and the client has its own opt-out, but that server-side switch did not work before Headscale 0.29.5, and Headscale's default relay map points at Tailscale's servers, so the gate needs a trace. Accessibility floor Unknown pending a terminal screen reader pass; both command lines honour NO_COLOR and offer structured output. Exit is re-enrolment of nodes. Main risks: one maintainer wrote about 85% of Headscale's commits in the last year, the protocol is controlled by Tailscale Inc., and 2026 security bulletins cluster in Tailscale SSH and Serve, which this use does not need."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-12T21:00:00-05:00"
  human: []
---

# Universal Cake Evaluation: Headscale and Tailscale, Mesh Network

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates a self-hosted Headscale control server, used with the open source Tailscale client, against version 0.6.0 of the Universal Cake Evaluation Metrics. The use in view is narrow: giving an Ansible controller authenticated reach to nodes wherever they sit, without port forwarding or static addresses. The evidence is source inspection of both repositories, a twelve-month commit count, and published documentation and advisories. No gate fails on that evidence, and the licence gate is Verified. Three things need testing before the entry can leave Assess: whether a correctly configured deployment contacts any Tailscale-run host, whether install and enrolment can be done with a terminal screen reader, and a test teardown. The inspection found two defaults that matter. Headscale's default relay map sends relayed traffic through Tailscale's servers, and an empty policy allows all traffic between nodes. It also found that the switch which stops clients uploading logs to Tailscale Inc. did not work before Headscale 0.29.5.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Source evidence comes from the Headscale repository <a id="cite-headscale-repo"></a>([Headscale contributors, 2026b](#ref-headscale-repo)) at commit `520eb6f` and the Tailscale client repository <a id="cite-tailscale-repo"></a>([Tailscale Inc., 2026](#ref-tailscale-repo)) at commit `1d32249`, both read on 2026-11-12. Release history comes from Headscale's changelog <a id="cite-headscale-changelog"></a>([Headscale contributors, 2026a](#ref-headscale-changelog)). Client logging behaviour is described in Tailscale's own documentation <a id="cite-tailscale-logging"></a>([Tailscale Inc., n.d.](#ref-tailscale-logging)). The account of the October 2026 security findings draws on a third-party analysis <a id="cite-corgea-tailworm"></a>([Corgea, 2026](#ref-corgea-tailworm)). The evaluation was prepared with AI assistance from Claude Fable 5.2 (Anthropic).

## Scope

Headscale is an open source implementation of the Tailscale control server. Tailscale is a mesh network built on WireGuard: each node runs a client, the control server hands out keys, addresses and access policy, and nodes then connect to each other directly where they can and through a relay where they cannot. With Headscale, the owner runs the control server and the nodes run the standard Tailscale client.

The two are evaluated together because neither is being adopted alone. The telemetry question in particular only makes sense for the pair: the client is written by Tailscale Inc. and is built to talk to Tailscale's services, and Headscale's job is to replace those services.

In scope:

- **Headscale** 0.29.7, released 2026-10-30, as a host install from the project's `.deb` package.
- **The Tailscale client for Linux**, `tailscaled` and the `tailscale` command, latest stable tag 1.104.7.

Not in scope: Tailscale's hosted service, the Windows, macOS and iOS clients, whose graphical wrappers are not open source, Tailscale SSH, Serve and Funnel, and third-party web interfaces for Headscale such as Headplane.

## Scorecard metadata

- Product or service name: Headscale with the Tailscale client
- Version evaluated: Headscale 0.29.7; Tailscale client 1.104.7. Source read at the head of each main branch on 2026-11-12
- Evaluator: Claude Fable 5.2 (Anthropic), drafted for Christopher Steel's review. No human pass yet
- Date: 2026-11-12
- Radar ring: Assess, light pass and source inspection complete, testing not started
- Test platform: none yet. Source was read in a disposable Linux workspace; nothing was installed or run
- Assessment method: source and configuration inspection with `git` and `grep`; commit counts from `git shortlog` over a partial clone limited to commits since 2025-11-12; documentation review. The uc-radar telemetry trace procedure, version 0.1.0, has not been run

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass, provisional | Inferred, from source inspection | Every contact with a Tailscale-run host that was found has a documented switch, but none has been traced. Log upload: off by default under Headscale, and the client has its own opt-out, `TS_NO_LOGS_NO_SUPPORT=true`. Relays: the default map is Tailscale's, and `derp.urls: []` with the embedded relay removes it. Update check: `disable_check_updates: true`. Client captive-portal detection and DNS fallback can also reach Tailscale-run hosts and are the least certain. See Findings |
| A licence incompatible with the project | Pass | Verified, by inspection, 2026-11-12 | BSD-3-Clause, read from the `LICENSE` file of each repository. Compatible with use alongside AGPL-3.0-or-later work. Confirm against Vishpala's own licence policy, as the Gates at a glance table asks |
| Failure of the accessibility floor | Unknown | Inferred, from source inspection | The interface is text: two command-line tools, a YAML configuration file, a HuJSON policy file, logs and documentation. Found in source: `headscale` honours `NO_COLOR` and offers `--output json`, `json-line` or `yaml`; the `tailscale` command honours `NO_COLOR` and has a JSON option on many subcommands; pre-authentication keys allow enrolment with no browser step. Not yet done: a terminal screen reader pass over install, configure, enrol, and one failure, for example an expired key |
| No exit | Pass | Inferred | Nothing but node records, keys and policy is stored. The store is SQLite by default and the policy is a file the owner keeps. Leaving means enrolling nodes with another control plane, which Ansible can do |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Unknown | Inferred | See the accessibility-floor gate. No graphical interface is required on Linux. The documentation site was not audited |
| Multilingual integration | Weak | Inferred | No translations were found in Headscale's documentation tree, and command output is English. Usual for infrastructure tooling, but it limits who can operate the mesh |
| Economic and cognitive accessibility | Strong (affordability), Moderate (cognitive) | Inferred | Free, with no paid tier. Runs as one binary with SQLite on a small server. Operating it needs comfort with YAML, TLS, DNS and a policy language. Hardware and bandwidth needs were not measured |
| Representation | Unknown | Inferred | Neither project publishes anything on disabled representation or user research. Per the metrics, that absence is itself a data point |
| Compatibility | Strong | Claimed, project documentation | Headscale packages cover Debian 12 or newer and Ubuntu 22.04 or newer. Client support is listed for Linux, the BSDs, Windows, macOS, Android and iOS, for the last ten client releases |
| Resilience | Moderate | Inferred | One control server and, if enabled, one embedded relay: each is a single point of failure for new connections. Established direct tunnels are expected to survive a short control-server outage, not tested. No licence server or expiring vendor token |
| Agency, sovereignty and privacy | Strong, once configured | Inferred | The owner runs the control plane and holds the keys, node list and policy. Out of the box, relayed traffic and the relay map depend on Tailscale Inc. See Findings |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No terms of service or pricing apply to self-hosted use. Exit cost is an estimate: two to four hours to write a re-enrolment play, then minutes per node. Portability: SQLite database and a HuJSON policy file, both machine-readable; the policy is also human-readable |
| Agency, interaction patterns | Moderate to Strong | Inferred | No accounts, prompts or upsell. Two defaults favour convenience over the owner: the relay map, and allow-all traffic when no policy is set. Both are stated plainly in the example configuration |
| Environment, direct, indirect, and authoring-side | Moderate to Strong | Inferred | Small footprint on the server. On nodes, the client runs WireGuard in userspace, which costs more processor time per packet than the kernel implementation. That matters little for administrative traffic and was not measured. No authoring interface |
| Security | Moderate | Mixed, see notes | Tailscale publishes a security policy with private reporting and coordinated disclosure, Verified by inspection of `SECURITY.md`. No security policy file was found in the Headscale repository, Verified by inspection. Bulletins in 2026 cluster in Tailscale SSH and Serve. Headscale shipped a security release five days after an uncoordinated disclosure. The control server must be reachable from the internet. See Findings |
| Longevity | Moderate | Verified (commit counts), Claimed (funding) | Headscale: seven patch releases from 2026-06-18 to 2026-10-30, still pre-1.0, with about 85% of the last year's commits from one person. Tailscale Inc. is venture-funded, most recently US$160 million in April 2025 <a id="cite-osler-tailscale"></a>([Osler, Hoskin & Harcourt LLP, 2025](#ref-osler-tailscale)) |
| Content endurance | Not applicable | Inferred | Absent: authored content. The product carries traffic and stores only node records, keys and policy |
| Exit and portability | Strong | Inferred | See the No exit gate. The data plane is WireGuard, an open protocol; the control protocol is Tailscale's own |
| Adjustability and support | Moderate | Verified in part, by inspection | Permissive licence and readable Go. The project states a deliberately narrow scope, accepts pull requests only from vouched contributors, and does not support running behind a reverse proxy or in a container |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified (contributor concentration), Inferred (the rest) | Headscale implements a protocol that Tailscale Inc. defines and can change. Contributor concentration is high in both projects. No take rate. Not a machine-learned product, so the foundation model proxy is not applicable |
| Gates | Pass, one Unknown | One Verified, three Inferred | Licence Verified. Telemetry and exit pass on inspection only. Accessibility floor Unknown pending a terminal screen reader pass |

## Findings

### The log switch exists, and did not work until 0.29.5

A Tailscale client streams its logs to Tailscale Inc. by default, including connection events between machines ([Tailscale Inc., n.d.](#ref-tailscale-logging)). Headscale's configuration has a `logtail.enabled` option, false by default, which tells nodes not to. The changelog for Headscale 0.29.5, released 2026-10-13, lists a fix for clients uploading logs to Tailscale Inc. while that option was false ([Headscale contributors, 2026a](#ref-headscale-changelog)). So for some period before that release the default did not do what it said.

Two consequences follow. Run 0.29.5 or newer. And set the client's own opt-out, `TS_NO_LOGS_NO_SUPPORT=true` in `/etc/default/tailscaled`, on every node, so the protection does not depend on the control server alone. Headscale's documentation notes that the client-side setting takes effect at startup, before the node has connected to the control server. This finding is the strongest argument for tracing the gate instead of trusting the configuration.

### Relays default to Tailscale's servers

When two nodes cannot connect directly, traffic goes through a relay, which Tailscale calls DERP. Headscale's example configuration sets `derp.urls` to Tailscale's public relay map and leaves the embedded relay disabled. Its own comment says that with this default Headscale fetches the map from Tailscale and clients relay through Tailscale's servers, and that an empty list keeps to the owner's relays. The map is re-fetched every three hours.

Relayed traffic stays encrypted end to end by WireGuard, so this is not content exposure. It does expose metadata to a third party: which addresses are talking, when, and how much. For a sovereign deployment, set `derp.urls: []` and enable the embedded relay, which needs TLS and a public UDP port for STUN.

### Other contacts with Tailscale-run hosts

Three further behaviours were found in source and need a trace to settle:

- **Headscale update check.** Commands check for a newer release at start unless `disable_check_updates` is true.
- **Captive-portal detection.** The client probes relay nodes over HTTP to detect captive portals. Before it has a relay map it uses a built-in list of Tailscale-run servers, and the source names two Tailscale hosts as further endpoints. A control-plane attribute exists to turn detection off. Whether a Headscale policy can set it was not confirmed.
- **DNS fallback.** When system DNS fails at startup, the client can resolve names through a built-in list of Tailscale-run servers.

Client updates, when requested or when auto-update is on, come from Tailscale's package server. That is an ordinary package source, and the owner can mirror or pin it.

### An empty policy allows all traffic

The example configuration states that an empty `policy.path` starts Headscale without a policy, which allows all traffic between nodes. For a mesh whose purpose is controller-to-node reach, that default is too wide: a compromised test node could reach every other node. Write the policy first. It needs one rule, the controller may reach nodes on the SSH port, and nothing else.

### Security bulletins in 2026 cluster in features this use does not need

Tailscale's bulletins for 2026 were numbered up to TS-2026-017 by the end of October. The ones reviewed mostly concern Tailscale SSH, Serve, Funnel and Services, with others in the macOS client, the client's local web interface and the hosted service. On 2026-10-25 a researcher published an eleven-finding audit of `tailscaled` 1.104.2 and Headscale 0.29.5 without coordinating first. Tailscale released 1.104.4 with fixes, and four bulletins, over 2026-10-27 and 28 ([Corgea, 2026](#ref-corgea-tailworm)). Two of the findings were denial-of-service problems in Headscale itself. Headscale 0.29.7 followed on 2026-10-30 as a security release covering that report and twelve findings from Anthropic's oss-scanner project; the maintainers describe none as critical ([Headscale contributors, 2026a](#ref-headscale-changelog)).

The response times are good. The lesson for this deployment is to keep the attack surface small: leave Tailscale SSH and Serve off, and run ordinary OpenSSH over the mesh address, which is all Ansible needs.

Two points about the control server's exposure. It must be reachable from the internet over HTTPS, so it is a public service that an attacker can probe without credentials, and the two Headscale findings above were of exactly that kind. And the project does not support reverse proxies or containers, which rules out the usual ways of shielding such a service. Whether an Incus system container counts as a container for this purpose is worth asking the maintainers; it runs a full init system and the `.deb` package unchanged.

### One person wrote about 85% of the last year's commits

Counted from git history on 2026-11-12, Headscale had 1,608 commits from 64 authors since 2025-11-12. Kristoffer Dalby wrote 1,361 of them, about 85%, under two addresses, one of them at tailscale.com. The next contributor wrote 92, about 6%. The README confirms that one active maintainer is employed by Tailscale and may spend work hours on the project, with his contributions reviewed by other maintainers.

This cuts both ways. Tailscale's support keeps Headscale compatible and well maintained, and the project is very active. It also means Headscale's health depends on one person and on the continued goodwill of the company whose hosted product it replaces. The Tailscale client had 1,846 commits from 188 authors over the same period, about 74% from tailscale.com addresses.

If either relationship soured, the fallback is real but costly: both codebases are BSD-3-Clause and could be forked, and the exit described under the No exit gate would remain open.

## Recommendation

Keep in Assess and proceed to testing. If testing verifies the gates and the Security section, move to Trial, not straight to Adopt: remote reach to every node is high-risk infrastructure, and the open questions about resilience and upkeep are ones only real use answers. A suitable Trial scope is one test fleet for a fixed period, with the end condition written down first.

Configuration to test and, if it holds, to carry into Trial:

- Run Headscale 0.29.7 or newer, and Tailscale clients 1.104.4 or newer.
- Set `derp.urls: []`, enable the embedded relay, and set `disable_check_updates: true`.
- Leave `logtail.enabled` false, and also set `TS_NO_LOGS_NO_SUPPORT=true` on every node.
- Write a least-privilege policy file before enrolling the first node.
- Enrol with pre-authentication keys, short-lived and single-use where possible.
- Do not enable Tailscale SSH, Serve or Funnel.
- Install the client from Tailscale's signed apt repository at a pinned version, not by piping an install script into a shell.
- Back up the SQLite database, the policy file and the server's private keys together.

Testing needed to leave Assess:

- A telemetry trace on a disposable node and server with the configuration above, using the uc-radar telemetry trace procedure, with attention to startup, a DNS failure, and a network change.
- A terminal screen reader pass for the accessibility floor.
- A test teardown: move one node to a second control server and record the time taken.
- A short outage test: stop Headscale and confirm what keeps working.

Re-review triggers specific to this pair: a licence change in either repository, a change in Tailscale's stance toward Headscale or in the maintainer's employment, the client dropping support for third-party control servers, a new default contact with a Tailscale-run host, and any security bulletin touching the control protocol or the relay.

## Relationship to project

Dev toolchain and operations: the transport layer under the Ansible converge pipeline. On adopt, the tested configuration and the enrolment play would graduate to `uc-processes` as a versioned process.

## Status notes

- Last reviewed: 2026-11-12.
- In assess: light pass and source inspection done. Licence gate Verified. Still needed before the entry moves to trial: Verified tags on the telemetry, accessibility-floor and exit gates and on the Security section, by the four tests listed under Recommendation, or a recorded override by a named person; and a human evaluation pass in `uc-radar_evaluation`.
- Compared in the same pass: NetBird, evaluated separately. ZeroTier was set aside as a likely licence-gate failure and has no entry yet.

## License

This document, *Universal Cake Evaluation: Headscale and Tailscale, Mesh Network*, by **Christopher Steel**, with AI assistance from **Claude Fable 5.2 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### Headscale

- [Headscale repository](#ref-headscale-repo)
- [Headscale changelog](#ref-headscale-changelog)

### Tailscale

- [Tailscale client repository](#ref-tailscale-repo)
- [Tailscale logging overview](#ref-tailscale-logging)
- [Tailscale Series C financing](#ref-osler-tailscale)

### Advisories

- [Analysis of bulletins TS-2026-014 to TS-2026-017](#ref-corgea-tailworm)

## References

<a id="ref-corgea-tailworm"></a>Corgea. (2026). [Analysis of Tailscale security bulletins TS-2026-014 to TS-2026-017 and the Tailworm report]. https://corgea.com/research/ts-2026-014-015-016-017-tailscale-tailworm-ssh-unix-socket-root-escalation/
[Return to citation](#cite-corgea-tailworm)

<a id="ref-headscale-changelog"></a>Headscale contributors. (2026a). *Changelog*. GitHub. https://github.com/juanfont/headscale/blob/main/CHANGELOG.md
[Return to citation](#cite-headscale-changelog)

<a id="ref-headscale-repo"></a>Headscale contributors. (2026b). *Headscale* (Version 0.29.7) [Computer software]. GitHub. https://github.com/juanfont/headscale
[Return to citation](#cite-headscale-repo)

<a id="ref-osler-tailscale"></a>Osler, Hoskin & Harcourt LLP. (2025). *Tailscale in its US$160-million Series C financing*. https://www.osler.com/en/about-us/representative-work/tailscale-2/
[Return to citation](#cite-osler-tailscale)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.6.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

<a id="ref-tailscale-logging"></a>Tailscale Inc. (n.d.). *Logging overview*. Retrieved November 12, 2026, from https://tailscale.com/kb/1011/log
[Return to citation](#cite-tailscale-logging)

<a id="ref-tailscale-repo"></a>Tailscale Inc. (2026). *Tailscale* (Version 1.104.7) [Computer software]. GitHub. https://github.com/tailscale/tailscale
[Return to citation](#cite-tailscale-repo)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring entry against metrics 0.6.0, from source inspection, a twelve-month commit count and documentation. Licence gate Verified; telemetry, accessibility-floor and exit gates and Security await testing |

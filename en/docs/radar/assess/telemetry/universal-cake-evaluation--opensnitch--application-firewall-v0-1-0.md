---
dc:title: "Universal Cake Evaluation: OpenSnitch, Application Firewall"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "opensnitch"
  - "firewall"
  - "telemetry"
  - "network monitoring"
dc:description: "Universal Cake evaluation of OpenSnitch 1.8.0, the open source per-application firewall for GNU/Linux, as an end-user privacy tool and as an instrument for verifying the telemetry gate in other evaluations."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-27"
dcterms:modified: "2026-10-27"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/evilsocket/opensnitch"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, universal-cake-evaluation--fileverse-ddocs--encrypted-document-editor"
dc:identifier: "universal-cake-evaluation--opensnitch--application-firewall"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of OpenSnitch 1.8.0 against Universal
      Cake Evaluation Metrics 0.5.1. The daemon was built from the signed
      v1.8.0 tag and run on Linux with the iptables backend; interception,
      per-process enforcement, crash behaviour, clean shutdown, its own
      outbound connections and the UI socket were tested. The Qt interface
      was checked by loading its dialog definitions headlessly; no screen
      reader test was performed.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # as internal evaluator tooling on a single-user machine, by recorded human override of the Unknown accessibility gate; Assess for client deployment
      reasoning: "Telemetry, licence and exit gates pass, telemetry Verified for the daemon by tracing. Accessibility floor Unknown: 168 of 263 focusable controls in the dialog definitions have no accessible name, including the checkboxes in the connection prompt, and the prompt times out after at most 100 seconds. By design it fails open: with no UI connected the default action is allow, and after a daemon crash queued traffic bypasses it. With the default socket and authentication, an unprivileged local user who creates /tmp/osui.sock first receives the root daemon's connection, which carries every process's path, arguments and environment. One maintainer wrote 85% of commits over two years; no commits since July 2026. Ubuntu 24.04 ships 1.5.8.1 from 2023."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-27T10:30:00-04:00"
  human: []
---

# Universal Cake Evaluation: OpenSnitch, Application Firewall

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates OpenSnitch, a per-application firewall for GNU/Linux modelled on Little Snitch, against the Universal Cake Evaluation Metrics. OpenSnitch asks the user, program by program, whether an outgoing connection should be allowed, and records what every program on the machine connects to. It matters to Universal Cake twice: as a privacy tool someone might run, and as the instrument that turns a Claimed telemetry rating into a Verified one in other evaluations. It passes the telemetry, licence and exit gates; its own daemon made no outbound connections in testing. Testing also confirmed three things an adopter needs to know. It fails open by design. With its default socket settings, any local user can stand in for the interface and see what every process is doing. And its dialogs leave most form controls without accessible names. The project is healthy in output but rests on one maintainer, and the version in Ubuntu's archive is three years old.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about OpenSnitch come from its repository, read at the PGP-signed `v1.8.0` tag (commit `b404c4c6`) and at `master` (commit `a1353848`) on 2026-10-27; from its wiki, read the same day; from the daemon, built from the `v1.8.0` tag and run on Linux; from the Ubuntu 24.04 package and its changelog; and from the Debian package tracker. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate OpenSnitch 1.8.0, released in December 2025 and still the current release. It has two parts:

- **The daemon**, `opensnitchd`, written in Go, which runs as root, queues new outgoing connections through netfilter's NFQUEUE, identifies the process behind each one, and applies rules.
- **The interface**, `opensnitch-ui`, written in Python with Qt 6, which runs as the desktop user, shows a prompt for each unknown connection, and holds the events database and rule editor. One interface can manage several daemons over the network.

Two uses are in scope, and the lenses differ between them. For Vishpala's own evaluation work, the owner and user are the evaluator, and the question is whether OpenSnitch is a trustworthy measuring instrument. For a client deployment, the owner is whoever administers the machine and the user is whoever answers the prompts, often someone with no networking background.

## Scorecard metadata

- Product or service name: OpenSnitch
- Version evaluated: 1.8.0, daemon built from tag `v1.8.0` (commit `b404c4c6`) with Go 1.24.7; interface dialog definitions from the same tag
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-27
- Radar ring: Assess
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, iptables-legacy backend, `/proc` process monitor

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified, Inferred | Traced under `strace`, the daemon's only connections were to the local UI socket (verified). The interface's network features are plugins that stay off unless the user adds a configuration file: a release checker that queries GitHub, a VirusTotal lookup that needs the user's own API key, and blocklist downloads from URLs the user chooses (verified in source, interface not traced) |
| A licence incompatible with the project | Pass | Verified | GPL-3.0. Running it imposes nothing on Universal Cake's work, and GPL-3.0 code can be combined with AGPL-3.0-or-later code under section 13 of both licences |
| Failure of the accessibility floor | Unknown | Inferred | 168 of 263 focusable controls in the seven main dialogs have no accessible name by Qt's naming rules, including four checkboxes and three drop-downs in the connection prompt. Buttons are named. Not tested with a screen reader, see Findings |
| No exit | Pass | Verified | Rules are plain JSON files, one per rule, in `/etc/opensnitchd/rules/`; events are in an SQLite database; logs can go to syslog or a remote collector. Stopping the daemon with SIGTERM removed all of its firewall rules (verified) |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Weak | Inferred | See the accessibility-floor gate. No label buddies or accessible names are set anywhere in the dialog definitions or interface code. Qt 6 exposes widgets to Orca through AT-SPI, so named buttons and drop-downs that announce their current value will be partly usable. The prompt counts down and applies its default action; the timeout can be raised only to 100 seconds, or prompts can be replaced by notifications |
| Multilingual integration | Strong | Verified | 25 interface translations as Qt Linguist `.ts` files. Of the strings in the current catalogue, French covers 98%, and Brazilian Portuguese, Swedish, Indonesian and Simplified Chinese 98 to 100%; three have no finished translations. The wiki explains how to add or update a language. French is `fr_FR`, not a Canadian variety |
| Economic and cognitive accessibility | Moderate | Verified, Inferred | Free, and light: the daemon used 27 MB of memory and about 1% CPU in testing. Cognitively demanding: each prompt asks the user to judge a process path, host, IP address and port, which most people cannot do reliably. A preset rule set managed by someone else lowers that burden |
| Representation | Unknown | None | Not assessable from public information. Built by and for technical Linux users. No machine-learned models in the product |
| Compatibility | Moderate | Verified | GNU/Linux only. Needs NFQUEUE in the kernel. On the test kernel, which lacks `CONFIG_NFT_QUEUE`, the default nftables backend failed and the iptables backend worked. The eBPF process monitor needs separately compiled modules; without them the daemon fell back to `/proc`, as designed. Desktop interface needs Qt 6 |
| Resilience | Moderate | Verified | Fully local and offline. Fails open: with no interface connected the daemon applies its default action, allow, and after a crash queued traffic passes freely (both verified). It never blocks the machine's networking by failing, but it stops enforcing without warning |
| Agency, sovereignty and privacy | Moderate | Verified | Strong in purpose and control: the user decides what each program may reach, every decision is a readable file, and nothing leaves the machine by default. Weakened by the default UI socket: `/tmp/osui.sock` with no authentication, which a non-root user can create first and so receive the root daemon's connection, including every process's arguments and environment variables (verified that the connection is made; contents verified in source), see Findings. Documented mitigations exist: a per-user socket path and TLS |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified | Exit cost is near zero: stop the service and the rules are removed. Rules are JSON, readable by a person and a program. No terms of service, no pricing, no account |
| Agency, interaction patterns | Moderate | Verified, Inferred | Honest defaults on the interface: an unanswered prompt denies until restart. Plain asking in every prompt. But prompts interrupt by nature, and a new install raises many of them until rules settle; there is no first-run learning mode. Nothing manipulative: no nags, no upsell, donation link only in the repository |
| Environment, direct, indirect, and authoring-side | Strong | Verified | 23.6 MB daemon binary, 27 MB resident memory, about 1% CPU under light traffic. Runs on old hardware. Blocking trackers and unwanted updates reduces bandwidth |
| Security | Moderate | Verified, Inferred | Release tag PGP-signed (verified). No `SECURITY.md` or private reporting channel (verified absence). No CVEs found for OpenSnitch (inferred from search). Pins old Go modules, including gRPC 1.32.0 from 2020; the daemon uses it only as a client on a local socket, which limits exposure (inferred). The default socket weakness and fail-open behaviour are covered in Findings |
| Longevity | Moderate | Verified | Created by Simone Margaritelli in 2017, maintained by Gustavo Iñiguez Goia since 2019. 512 non-merge commits in the last two years, but none on `master` since 27 July 2026. Six releases from February to December 2025. Funding is GitHub Sponsors to the maintainer |
| Content endurance | Strong | Inferred | An enhancement layer: removing it leaves the system and its applications whole. Rules and events stay in open formats |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Moderate | Verified | Copyleft and readable Go and Python; a detailed wiki covering configuration, rules, known problems and SIEM integration. Support through GitHub issues and a Discord server, which in practice means one maintainer |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | The main open source tool of its kind on Linux; alternatives such as Portmaster and Little Snitch differ in licence or platform. No take rate. Forkable by licence. Contributor concentration: 108 authors, but Gustavo Iñiguez Goia wrote 76% of all non-merge commits and 85.5% over the last two years (verified). No foundation-model dependency |
| Gates | Unknown | Mixed | Telemetry, licence and exit pass; accessibility floor Unknown |

## Findings

### Interception and enforcement

We built the daemon from the signed `v1.8.0` tag and ran it with the iptables backend and a test rule directory. It installed a single `NFQUEUE` rule for new and related connections in the mangle table's `OUTPUT` chain, with `--queue-bypass`. With no interface running, a connection from `curl` to a local test server was intercepted, attributed, and allowed by the default action. We then added one rule denying `/usr/bin/curl` on port 8099. `curl` was blocked, and the daemon logged the denial against the rule by name, while a Python client reaching the same port was allowed. Process attribution and per-process enforcement work as documented <a name="apa-os-rules-citation"></a>([OpenSnitch Project, n.d.-d](#apa-os-rules-reference)).

### Its own network behaviour

Over two traced runs, `strace` recorded every `connect`, `sendto` and `sendmsg` call made by the daemon. The only destination was the Unix socket `/tmp/osui.sock`, where it looks for the interface. The daemon's source contains three dialling code paths: the UI client, an optional remote logger the owner configures, and the local audit socket for the optional auditd monitor.

The interface was not traced, but its code was read. Its outbound features are plugins loaded only from JSON files in the user's `~/.config/opensnitch/actions/` directory, which starts empty apart from built-in colour themes. The release checker's own default is disabled. When enabled, it queries the GitHub API every 12 hours and ignores the URL in its own configuration file, a minor bug. The VirusTotal plugin needs the user's API key, and its documentation warns that domains, IP addresses and file hashes are sent to VirusTotal <a name="apa-os-vt-citation"></a>([OpenSnitch Project, n.d.-f](#apa-os-vt-reference)). This is what an instrument for verifying other products' telemetry should look like.

### It fails open

Three defaults decide what happens when something goes wrong. Testing confirmed the first two; the third is from the interface's source. When no interface is connected, the daemon applies its `DefaultAction`, which ships as `allow`. When the daemon is killed, its `NFQUEUE` rule stays in place, but `--queue-bypass` passes traffic straight through; a connection matching our deny rule succeeded with the daemon dead. When the interface is connected and the user does not answer, it denies the connection until restart.

Failing open is a defensible choice for a desktop tool, since a crash never cuts a machine off from the network. It means OpenSnitch is a visibility and consent tool, not a security boundary. For evaluation work this is harmless as long as the daemon is running. For clients it must be explained, and an owner who wants enforcement can set `DefaultAction` to deny and `QueueBypass` to false, at the cost of losing networking when the daemon stops <a name="apa-os-config-citation"></a>([OpenSnitch Project, n.d.-b](#apa-os-config-reference)).

### The default UI socket

The daemon, running as root, connects as a gRPC client to the interface, which listens on `unix:///tmp/osui.sock` with `simple` authentication, meaning none. `/tmp` is writable by every user. We created an unprivileged test account, had it listen on `/tmp/osui.sock` before the daemon started, and started the daemon. The daemon connected to that listener and began an HTTP/2 session.

The daemon sends a prompt for each unknown connection, and the source shows that each prompt carries the process path, arguments, working directory and full environment of the process making it <a name="apa-os-conn-citation"></a>([OpenSnitch Project, n.d.-c](#apa-os-conn-reference)). A listener answering those prompts can therefore see command lines and environment variables, which often hold tokens, for every user's processes, and decide which connections are allowed. We did not build a full impostor interface, so that last step is inferred from the protocol, not demonstrated.

The window is narrow on a single-user laptop: it exists only while the real interface is not running, because once the interface holds the socket, `/tmp`'s sticky bit stops another user from replacing it. It is wide on a shared computer, a headless server, or a machine where the interface starts after login. The wiki says the socket should be readable only by the interface's user, suggests moving it to `/run/user/1000/opensnitch/osui.sock`, and documents TLS authentication between daemon and interface <a name="apa-os-config-2-citation"></a>([OpenSnitch Project, n.d.-b](#apa-os-config-reference)), <a name="apa-os-auth-citation"></a>([OpenSnitch Project, n.d.-a](#apa-os-auth-reference)). Neither is the default. Changing the default socket to a per-user runtime directory and sending the environment only when a rule needs it would close most of this. It is worth raising upstream privately before discussing it in public.

### Accessibility evidence

PyQt6 does not wrap Qt's accessibility interface, so we loaded each dialog definition headlessly and applied Qt's naming rules directly: an explicit accessible name, then a button's own text, then the text of a label set as the control's buddy. Ignoring Qt's internal sub-widgets, the seven main dialogs hold 263 focusable controls, and 168 have no accessible name. No dialog sets a single label buddy, and the interface code sets no accessible names. Some of the gap is filled at run time: the prompt's Allow and Deny buttons receive their text in code. But the prompt's four option checkboxes have no text, and their meaning comes from a neighbouring drop-down, so a screen reader announces only "check box, not checked". The statistics window, where events and rules are reviewed, has 45 unnamed controls out of 48.

The prompt also has a time limit. It defaults to 30 seconds and can be set to at most 100, against WCAG's expectation that a time limit can be turned off or extended tenfold <a name="apa-wcag-citation"></a>([World Wide Web Consortium, 2023](#apa-wcag-reference)). The alternative is to turn prompts off and decide afterwards from the events window, which is itself the least accessible dialog. A screen reader pass with Orca would settle the gate; we expect it to show the product as usable with difficulty rather than unreachable.

### Packaging lag

Ubuntu 24.04 LTS, and so the Linux Mint 22 releases built on it, ships OpenSnitch 1.5.8.1, released in March 2023. Its three later Ubuntu revisions are rebuilds for Go updates <a name="apa-ubuntu-citation"></a>([Ubuntu, 2025](#apa-ubuntu-reference)). Debian stable ships 1.6.9. Debian's tracker shows the package is looking for help, and 1.6.9 is marked for removal from testing on 31 October 2026 over a release-critical bug <a name="apa-debian-citation"></a>([Debian, n.d.](#apa-debian-reference)). Upstream publishes `.deb` and `.rpm` packages for each release <a name="apa-os-install-citation"></a>([OpenSnitch Project, n.d.-e](#apa-os-install-reference)). Anyone installing from a distribution archive should check which version they are getting.

### Kernel requirements

The daemon's `-check-requirements` option reports missing kernel features clearly. On the test kernel, `CONFIG_NFT_QUEUE` is not set, so the default nftables backend could not add its interception rules, while the iptables backend, using the older NFQUEUE target, worked. Most distribution kernels include both. The more precise eBPF process monitor needs its compiled modules; without them the daemon logged the failure and used `/proc`, which is slower and can miss very short-lived processes.

### Maintenance and supply chain

The project is active in output, with 275 non-merge commits in the twelve months to October 2026, six releases in 2025, and a wiki updated in July 2026. It is concentrated: one maintainer wrote 85.5% of the last two years' commits, and there have been no commits on `master` since 27 July 2026. The release tag is PGP-signed. The daemon pins gRPC 1.32.0, protobuf 1.26.0 and fsnotify 1.4.7, all several years old. There is no security policy, so there is no stated way to report the socket issue above privately except by contacting the maintainer directly.

## Recommendation

For Vishpala's own evaluation work, move OpenSnitch to Trial as the standard way to verify the telemetry gate, on a single-user machine. The accessibility-floor gate is Unknown, and it concerns people who would answer prompts, not the evaluator. The metrics allow a named person to proceed past such a gate, and that override should be recorded in the human evaluation entry if Christopher agrees. Setup for that use:

- Install upstream 1.8.0 packages rather than the 1.5.8.1 in Ubuntu's archive.
- Move the UI socket to a per-user runtime directory, for example `unix:///run/user/1000/opensnitch/osui.sock`, in both the daemon configuration and the interface preferences, or enable TLS authentication.
- Keep the version checker and VirusTotal plugins off, so the instrument adds no traffic of its own.
- Record the OpenSnitch version and configuration in each evaluation's assessment method, as the metrics' Security section asks.

For clients, keep OpenSnitch in Assess. It suits a technical owner who manages rules for others, for example a preset deny list on a shared computer, more than end users answering prompts. Before offering it more widely: run an Orca screen reader pass; explain the fail-open behaviour in plain language; never deploy it on a shared machine with the default socket; and report upstream the socket default, the unnamed controls and buddies, the 100-second timeout cap, and the release checker's ignored URL.

Re-review triggers specific to OpenSnitch: a change to the default socket or authentication; a new maintainer or a long gap in maintenance; a first published security advisory; and Debian dropping the package.

## License

This document, *Universal Cake Evaluation: OpenSnitch, Application Firewall*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)
- [Web Content Accessibility Guidelines 2.2](#apa-wcag-reference)

### OpenSnitch documentation

- [Nodes authentication](#apa-os-auth-reference)
- [Configurations](#apa-os-config-reference)
- [Rules](#apa-os-rules-reference)
- [Installation](#apa-os-install-reference)
- [VirusTotal plugin](#apa-os-vt-reference)

### OpenSnitch source

- [Connection serialisation, connection.go](#apa-os-conn-reference)

### Distribution packaging

- [Debian package tracker](#apa-debian-reference)
- [Ubuntu package](#apa-ubuntu-reference)

## References

<a name="apa-debian-reference"></a>Debian. (n.d.). *opensnitch* [Package tracker]. https://tracker.debian.org/pkg/opensnitch
[Return to citation](#apa-debian-citation)

<a name="apa-os-auth-reference"></a>OpenSnitch Project. (n.d.-a). *Nodes authentication* [Wiki page]. GitHub. https://github.com/evilsocket/opensnitch/wiki/Nodes-authentication
[Return to citation](#apa-os-auth-citation)

<a name="apa-os-config-reference"></a>OpenSnitch Project. (n.d.-b). *Configurations* [Wiki page]. GitHub. https://github.com/evilsocket/opensnitch/wiki/Configurations
[Return to citation](#apa-os-config-citation)

<a name="apa-os-conn-reference"></a>OpenSnitch Project. (n.d.-c). *connection.go* [Source code]. GitHub. https://github.com/evilsocket/opensnitch/blob/master/daemon/conman/connection.go
[Return to citation](#apa-os-conn-citation)

<a name="apa-os-rules-reference"></a>OpenSnitch Project. (n.d.-d). *Rules* [Wiki page]. GitHub. https://github.com/evilsocket/opensnitch/wiki/Rules
[Return to citation](#apa-os-rules-citation)

<a name="apa-os-install-reference"></a>OpenSnitch Project. (n.d.-e). *Installation* [Wiki page]. GitHub. https://github.com/evilsocket/opensnitch/wiki/Installation
[Return to citation](#apa-os-install-citation)

<a name="apa-os-vt-reference"></a>OpenSnitch Project. (n.d.-f). *VirusTotal plugin README* [Documentation]. GitHub. https://github.com/evilsocket/opensnitch/blob/master/ui/opensnitch/plugins/virustotal/README.md
[Return to citation](#apa-os-vt-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

<a name="apa-ubuntu-reference"></a>Ubuntu. (2025). *opensnitch 1.5.8.1-1ubuntu0.24.04.3* [Software package changelog]. Canonical. https://launchpad.net/ubuntu/+source/opensnitch
[Return to citation](#apa-ubuntu-citation)

<a name="apa-wcag-reference"></a>World Wide Web Consortium. (2023). *Web Content Accessibility Guidelines (WCAG) 2.2*, Success Criterion 2.2.1 Timing Adjustable. https://www.w3.org/TR/WCAG22/#timing-adjustable
[Return to citation](#apa-wcag-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of OpenSnitch 1.8.0 against Universal Cake Evaluation Metrics 0.5.1; daemon built from the signed tag and tested on Linux with the iptables backend; interface dialogs checked headlessly; no screen reader test |

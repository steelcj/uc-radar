---
dcterms:title: "uc-radar--foomuuri--nftables-firewall-manager"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude Opus 5.5 (Anthropic)"
dcterms:subject:
  - "radar"
  - "foomuuri"
  - "firewall"
  - "nftables"
  - "server-baseline"
  - "infrastructure"
dcterms:description: "Assess entry for Foomuuri, the zone-based nftables firewall manager, as the firewall layer of the Ventoy and Ansible server baseline, scored against Universal Cake Evaluation Metrics 0.6.0."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-11-15"
dcterms:modified: "2026-11-15"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: "https://github.com/FoobarOy/foomuuri"
dcterms:relation: "uc-radar--evaluation-lifecycle, universal-cake-evaluation-metrics, uc-radar-entry-template"
dcterms:identifier: "uc-radar--foomuuri--nftables-firewall-manager"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-11-15"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial entry, created from uc-radar-entry-template v0.3.0. Light pass
      against metrics 0.6.0 on documentation and source evidence, with
      contributor figures measured from the upstream git history.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # after every gate and the Security section are Verified, or a named person records an override
      reasoning: "Light pass against metrics 0.6.0. Telemetry gate passes: no telemetry; network use is limited to iplist downloads, DNS lookups and connectivity pings that the owner configures. Licence gate passes, GPL-2.0-or-later. Exit gate passes and is a strength: configuration is plain text and the generated ruleset is plain nftables that can be saved and loaded without Foomuuri. Accessibility floor Unknown, pending a terminal screen reader pass. Security Moderate: a root daemon whose D-Bus interface lacked authorization until 0.31 (CVE-2025-67603, CVE-2025-67858), fixed quickly upstream but reaching Ubuntu 24.04 months later. Trial recommended rather than adopt, because a firewall failure locks out remote machines, the project is pre-1.0 with breaking changes in recent releases, distribution versions range from 0.22 to 0.34, and coexistence with Incus networking can only be answered on a real host."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-11-15T21:40:00-05:00"
  human: []
---

# uc-radar--foomuuri--nftables-firewall-manager-v0.1.0

- Product or service name: Foomuuri, the `foomuuri` command and its systemd units, with the optional `foomuuri-firewalld` package
- Version evaluated: 0.34, released 2026-10-16, the latest release, at documentation level, with the upstream source read at commit `e5ac100` (2026-11-10); no build tested yet
- Evaluator: Claude Opus 5.5 (Anthropic), for Christopher Steel's review
- Date: 2026-11-15
- Radar ring: Assess

## What it is

Foomuuri is an open source firewall generator for Linux nftables, written in Python by Kim B. Heino at Foobar Oy in Finland. You describe zones (for example `localhost`, `public`, `lan`) and the traffic allowed between each pair of zones in plain text files under `/etc/foomuuri/`; Foomuuri turns that into an nftables ruleset and loads it.

## Why interesting

The Ventoy server baseline already ends with nftables, and the plan is to apply firewall policy with Ansible after install. Foomuuri sits at that point: one readable configuration that an Ansible role can template, checked with `foomuuri check` before it is loaded.

Several features match Universal Cake's priorities directly:

- Fail-safe loading. `foomuuri start-or-good` falls back to the last ruleset that loaded successfully, and `foomuuri-boot.service` loads that saved "good" ruleset early at boot, before the network is up. A bad edit is less likely to leave a remote machine open or unreachable.
- Bidirectional by design. Outgoing traffic is filtered as well as incoming, so a host can be limited to the connections it is meant to make, which supports the agency and privacy aims behind entries such as OpenSnitch.
- Plain nftables underneath. Foomuuri writes to its own table, `inet foomuuri`, and the result can be inspected with `foomuuri list` or `nft list ruleset`.
- Scales from laptop to router. The same tool covers a host firewall, a site router with NAT, and multiple internet connections with a connectivity monitor that switches between them. That fits the small-municipality resilience work as well as single servers.
- NetworkManager integration through firewalld emulation, for desktops such as Linux Mint where the zone changes with the network.

## Concerns

Small project, one main developer. Kim B. Heino wrote 721 of 853 non-merge commits (85%) across the project's history; eight people have contributed in total, seven in the last 24 months. The project is three and a half years old (first commit 2023-02-27). Development is very active, with a release every few months, but continuity rests on one person and one company.

Pre-1.0, with breaking changes. The changelog marks five breaking changes, including changes to iplist settings in 0.34 and a default that will change again in 0.35 <a id="cite-heino-2026"></a>([Heino, 2026](#ref-heino-2026)). Configuration written for one version may need edits for the next.

Distribution versions vary widely. Ubuntu 24.04, and so Linux Mint 22, ships 0.22; Debian 13 ships 0.27, with 0.31 or later in trixie-backports; Fedora carries current releases; upstream is at 0.34. One Ansible role targeting both Debian and Ubuntu would meet different syntax and features unless the version is pinned, for example by installing the same release everywhere.

D-Bus vulnerabilities in January 2026. A SUSE security review found that the D-Bus service, which runs as root, let any local user change which zone a network interface belonged to, and did not validate interface names (CVE-2025-67603, CVE-2025-67858). Upstream confirmed the report the next day and shipped fixes in 0.31 on 2026-01-07, adding Polkit authorization, input checks and systemd hardening <a id="cite-gerstner-2026"></a>([Gerstner, 2026](#ref-gerstner-2026)). Debian patched its stable release at once; Ubuntu's fix for 24.04 followed on 2026-05-27 <a id="cite-canonical-2026"></a>([Canonical, 2026](#ref-canonical-2026)). The response was good; the lag downstream is the lesson.

Coexistence with other rule writers. Incus, Docker, libvirt and similar tools add their own nftables rules. Foomuuri keeps to its own table, which is a good sign, but the install guide tells you to run `nft flush ruleset` once, and how forwarding policy in Foomuuri interacts with Incus's bridges is untested. This matters for the planned Incus host.

Replacing the existing firewall. Linux Mint ships ufw and its graphical front end; those must be disabled, and a desktop user who relied on the graphical tool loses it.

## Gates

The Assess light pass requires all four default gates and the Security section on available evidence. None is Verified yet.

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | No telemetry or update checks found in the source. Network use is only what the owner configures: iplist URL downloads (refreshed by `foomuuri-iplist.timer` every 15 minutes, needing the optional `python3-urllib3`), DNS lookups for hostname-based rules, and connectivity pings by the monitor (using the optional `fping`). With none configured, nothing is fetched. To Verify with the uc-radar telemetry trace procedure |
| A licence incompatible with the project | Pass | Inferred | GPL-2.0-or-later, read from the source header and `COPYING`. Run as a system tool, not linked into project code; the "or later" clause also allows GPL-3.0 terms. To Verify against the adopting project's licence policy |
| Failure of the accessibility floor | Unknown | Inferred, not observed | Text interface throughout: configuration files, `foomuuri check`, `foomuuri list`, journal logs. Not yet done: a terminal screen reader pass over writing a configuration, checking it, reading an error, and reading the active ruleset |
| No exit, data or content cannot be extracted in usable form | Pass | Inferred | Configuration is plain text. The generated ruleset is standard nftables and can be saved with `nft list ruleset` to `/etc/nftables.conf` and loaded by the stock `nftables.service`, a complete exit with no translation step |

## Evaluation

Ratings use Strong, Moderate, Weak, Unknown, and, narrowly, Not applicable, each tagged Verified, Inferred, or Claimed. No hands-on testing has been done for this entry. The contributor figures were measured from the upstream git history on 2026-11-15 and are tagged Verified.

### Inclusive

#### Accessibility

**Alternative methods of interacting with content** **[GATE]**

- Rating: Unknown
- Evidence: Inferred
- Notes: Lens owner. No graphical interface. The documentation site is generated from Markdown in the repository and has not been audited for WCAG conformance.

**Text and terminal interfaces** **[GATE]**

- Rating: Moderate, pending a screen reader pass
- Evidence: Inferred
- Notes: Lens owner, user. Plain text output in reading order: likely, the command prints plain lines and no colour handling was found in the source; to verify. Required steps depending on a full-screen interface, mouse, or graphical installer: none. Configuration: short, readable blocks such as `public-localhost { ssh; }`, which read aloud reasonably well, and named service macros such as `ssh` or `https` replace port numbers. Error messages: `foomuuri check` reports the file and line; how clear the wording is has not been checked. The rendered ruleset from `foomuuri list` is nftables syntax, long and dense to listen to, which is true of every nftables tool.

**Multilingual integration**

- Rating: Weak
- Evidence: Inferred
- Notes: Lens owner, community. Messages and documentation are English only; adding a language would be a code change.

**Affordability**

- Rating: Strong
- Evidence: Inferred
- Notes: Free, no paid tier, no account needed.

**Hardware and connectivity accessibility**

- Rating: Strong
- Evidence: Inferred
- Notes: The Debian package is about 35 kB, 171 kB installed, plus Python and nftables, which a server baseline already has. Runs on any Linux kernel with nftables, which includes old hardware still supported by current distributions. Works offline; only optional iplist downloads and DNS-based rules need a connection.

**Cognitive accessibility**

- Rating: Moderate
- Evidence: Inferred
- Notes: The zone model is easier to reason about than raw nftables chains, and the host firewall example is short. Firewalls are still unforgiving: mistakes can cut off access. `foomuuri check` and the fallback to the last good ruleset reduce that risk.

**Time-based media**

- Rating: Not applicable
- Evidence: Inferred
- Notes: The project publishes no video or audio.

#### Representation

- Rating: Unknown
- Evidence: None
- Notes: Lens community. Not assessed. One company and one main developer decide; community help is through GitHub discussions and `#foomuuri` on Libera.Chat. No published governance.

#### Resilience

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. When out of date: no licence server or account; an old version keeps filtering. Power outages: the saved good ruleset loads at boot before the network starts, so a host comes back protected. Network outages: the firewall needs no network; the multi-ISP monitor exists precisely to keep a site connected when one link fails. iplists that fail to download keep their previous entries until they expire, with a `missing_ok` setting to choose between a warning and an error.

### Agency

#### Sovereignty & Privacy

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner, user. The owner can modify, fork and redistribute. All configuration and state stay on the host, under `/etc/foomuuri/` and `/var/lib/foomuuri/`. Outgoing filtering gives the owner control over what the host contacts. Reversibility test: if the project stopped tomorrow, the last generated ruleset could be saved as plain nftables and kept running indefinitely.

**Power-imbalance proxies**

- Exit cost: Low, under an hour per host to save the ruleset as plain nftables; a few hours to rewrite the policy for another manager such as firewalld. Inferred.
- Data portability: Strong. Configuration is plain text, and the output is the standard nftables format.
- Terms-of-service volatility: Not applicable. GPL-2.0-or-later, no terms of service.
- Pricing asymmetry: Strong. Free for every use.

**AI-specific proxies**

- Not applicable. Foomuuri contains no machine-learned model and makes no algorithmic decisions about users.

#### Interaction Patterns

- Rating: Strong
- Evidence: Inferred
- Notes: Lens owner. No sign-in, nags, or upsells. Changes are explicit commands; nothing is applied without `start` or `reload`.

### Environment

- Rating: Strong
- Evidence: Inferred
- Notes: Lens planet. Direct: the rules run in the kernel; Foomuuri itself runs briefly to load them, plus small optional daemons (D-Bus, monitor) and a 15-minute iplist timer. Indirect: iplist downloads, if configured, repeat on that timer; refresh intervals are configurable. No measurable cost otherwise.

### Security

- Rating: Moderate, pending verification
- Evidence: Inferred
- Notes: Lens owner, user. Foomuuri runs as root, as any firewall loader must. Vulnerability handling was tested in practice in December 2025: a coordinated report, confirmation the next day, patches within a week, and fixes released on the agreed date, with backports to Debian stable. Since 0.31, D-Bus methods need Polkit admin authorization, or root alone if Polkit is disabled, and every unit runs with `ProtectSystem=full`. Attack surface: the D-Bus service, which `foomuuri.service` starts by default (it `Wants=foomuuri-dbus.service`) but which is only needed for NetworkManager or firewalld emulation, so it can be masked on servers; iplist parsing of downloaded content, now with stricter address checks. Supply chain: Python standard library plus optional packages; distribution packages are signed by their archives; no upstream release signatures were found. The downstream lag for Ubuntu 24.04, about five months, is the main exposure for Mint and Ubuntu hosts on the distribution package.

**Candidate project gate:** for firewall and other root-level network tools, a local privileged interface (D-Bus, socket, or HTTP) that accepts changes without authorization. Foomuuri would now pass; versions before 0.31 would fail. Record it in the Gates at a glance table if adopted.

**Assessment method:**
Not yet tested. Planned: on a disposable Debian 13 virtual machine and a Linux Mint 22 virtual machine, install from the distribution package and from the 0.34 release, apply the host firewall example, run the uc-radar telemetry trace procedure (version to be named) with no iplists configured and then with one URL iplist, check `busctl` that unprivileged D-Bus calls are refused, reboot to confirm the good ruleset loads before networking, and break the configuration on purpose to confirm `start-or-good` falls back. On an Incus test host, confirm container networking still works with Foomuuri active. Terminal screen reader pass with Emacspeak or Orca.

**Network behaviour:**
- [ ] Outbound connections during normal use (expected: none unless iplists, hostname rules, or the monitor are configured)
- [ ] Update checks, silent or explicit (expected: none)
- [ ] Telemetry or usage data, present or absent (expected: absent)
- [ ] Licence server contact, frequency and data sent (expected: none)
- [ ] Affects networking or items created that will be transferred over a network (expected: yes, it is the firewall; it decides what traffic passes)
- [ ] Helps ensure that documents passed via network are clean and compliant (expected: not applicable)

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content (expected: none)
- [ ] Caches content outside the working directory (expected: generated rulesets and iplist state under `/var/lib/foomuuri/` and `/run/foomuuri/`)
- [ ] Stores recent file history outside the archive (expected: none)
- [ ] Writes to unexpected locations (expected: none; `/etc` is read-only to its units)

**Content exposure:**
- [ ] Sends any content to a remote service (expected: none; DNS queries for configured hostnames)
- [ ] Stores content in a cloud service by default (expected: no)
- [ ] Auto-save or backup features that copy content externally (expected: no)

### The Product or Service Itself

**Longevity**

- Rating: Moderate
- Evidence: Verified for activity, Inferred otherwise
- Notes: Lens owner, community. First commit 2023-02-27; 926 commits; 19 releases since 0.16 in February 2023, the latest 0.34 on 2026-10-16. Packaged in Debian, Ubuntu, Fedora and EPEL, with an Arch AUR package, so distributions carry it. Young, and dependent on one company.

**Content endurance**

- Rating: Strong
- Evidence: Inferred
- Notes: Foomuuri holds no content. Its configuration is the only artefact, and it is plain text that a person can read and translate to another tool.

**Exit and portability** **[GATE]**

- Rating: Strong
- Evidence: Inferred
- Notes: See the exit gate. Relies on the nftables standard in the Linux kernel; successors include firewalld, plain nftables files, and Shorewall-style generators.

**Adjustability and support**

- Rating: Strong
- Evidence: Inferred
- Notes: Python, one main source file, GPL, so fixes are within reach of most administrators. Raw nftables rules can be added where the rule language falls short, and start and stop hooks run local commands. Support is community-based plus direct contact with the developer; commercial support from Foobar Oy is not documented.

**Market Position**

- Rating: Moderate
- Evidence: Verified for contributor figures, Inferred otherwise
- Notes: Lens community, owner. Market concentration: a small player beside firewalld, ufw and plain nftables, so alternatives are plentiful. Take rate: none. API stability: the rule language is mostly stable, with breaking changes announced in the changelog, five so far. Forkability: GPL-2.0-or-later, small codebase, readily forkable. Contributor concentration: high, 85% of non-merge commits from one developer, eight contributors in total.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Unknown, text interface Moderate | Inferred | Plain-text configuration and output; screen reader pass pending |
| Multilingual integration | Weak | Inferred | English only |
| Economic and cognitive accessibility | Strong (affordability, hardware), Moderate (cognitive) | Inferred | Tiny package; firewalls remain unforgiving |
| Representation | Unknown | None | One company, no published governance |
| Compatibility | Moderate | Inferred | Any nftables Linux; distribution versions range from 0.22 to 0.34 |
| Resilience | Strong | Inferred | Good ruleset at boot; multi-ISP monitor |
| Agency, sovereignty & privacy | Strong | Inferred | Local only; outgoing filtering |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | Exit to plain nftables in under an hour |
| Agency, interaction patterns | Strong | Inferred | Explicit commands only |
| Environment, direct, indirect, and authoring-side | Strong | Inferred | Kernel rules; optional 15-minute iplist refresh |
| Security | Moderate | Inferred | Root daemon; D-Bus CVEs fixed in 0.31; Ubuntu lag |
| Longevity | Moderate | Verified (activity), Inferred | Three and a half years, active, widely packaged |
| Content endurance | Strong | Inferred | Plain-text configuration |
| Exit and portability | Strong | Inferred | Standard nftables output |
| Adjustability and support | Strong | Inferred | Python, raw rules, hooks |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified (contributors), Inferred | One developer holds 85% of commits; five breaking changes |
| Gates | Pass (telemetry, licence, exit), Unknown (accessibility floor) | Inferred | None Verified; no gate failed |

## Relationship to project

Tier: infrastructure, the firewall layer of the server baseline. On adopt, the process graduates to `uc-processes`, and the implementation lives in an Ansible role applied after the Ventoy install, alongside the `raw node` contract in `uc-ventoy`. It would also serve the router and multi-ISP needs of the small-municipality resilience work.

Design decisions to record before adopt:

- Version: pin one Foomuuri release across Debian and Ubuntu hosts (0.31 or later, for the D-Bus fixes), rather than taking each distribution's package.
- Installation source: trixie-backports on Debian 13; the upstream release or a locally built package on Ubuntu 24.04 and Linux Mint 22.
- D-Bus: `foomuuri-dbus.service` masked on servers, since it starts by default; enabled with Polkit on desktops that use NetworkManager zones.
- Baseline policy: a host firewall template with SSH from management networks only, outgoing traffic limited to what the role lists, and logging of drops.
- Change procedure: `foomuuri check`, then `start-or-good`, with a timed rollback or out-of-band console when changing remote hosts.
- Incus hosts: a tested zone layout for Incus bridges, recorded once the trial confirms it.
- Exit: document saving the ruleset to `/etc/nftables.conf` as the fallback if Foomuuri is ever removed.

## Status notes

- Last reviewed: 2026-11-15
- In assess: light pass done against metrics 0.6.0 on documentation and source evidence. No gate failed; three gates pass on Inferred evidence and the accessibility floor is Unknown. Testing still needed before the entry can leave Assess: the telemetry trace, the D-Bus authorization check, the reboot and fallback checks on Debian 13 and Linux Mint 22, and a terminal screen reader pass. Also pending: the Representation review and the version pinning decision. The automated pass recommends trial, not adopt, once gates and Security are Verified: a firewall failure on a remote host is costly, the project is pre-1.0 with recent breaking changes, and coexistence with Incus networking can only be shown on a real host. Suggested trial scope: one non-production server and one Incus test host for a named period, ending when the baseline role has applied, rebooted and been upgraded across one Foomuuri release without manual repair.

## Links

- https://foomuuri.foobar.fi/latest/
- https://github.com/FoobarOy/foomuuri
- https://github.com/FoobarOy/foomuuri/blob/main/CHANGELOG.md
- https://tracker.debian.org/pkg/foomuuri
- https://security.opensuse.org/2026/01/07/foomuuri-lack-of-dbus-authorization.html

## References

<a id="ref-canonical-2026"></a>
Canonical. (2026, May 27). *USN-8326-1: Foomuuri vulnerabilities* [Security notice]. Ubuntu. https://ubuntu.com/security/notices/USN-8326-1
[Return to citation](#cite-canonical-2026)

<a id="ref-gerstner-2026"></a>
Gerstner, M. (2026, January 7). *Foomuuri: Lack of client authorization and input verification allow control over firewall configuration (CVE-2025-67603, CVE-2025-67858)*. SUSE Security Team Blog. https://security.opensuse.org/2026/01/07/foomuuri-lack-of-dbus-authorization.html
[Return to citation](#cite-gerstner-2026)

<a id="ref-heino-2026"></a>
Heino, K. B. (2026). *Foomuuri changelog: 0.34, 2026-10-16* [Computer software documentation]. GitHub. https://github.com/FoobarOy/foomuuri/blob/main/CHANGELOG.md
[Return to citation](#cite-heino-2026)

## License

This document, *uc-radar--foomuuri--nftables-firewall-manager*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

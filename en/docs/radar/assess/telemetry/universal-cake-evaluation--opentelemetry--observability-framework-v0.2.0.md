---
dc:title: "Universal Cake Evaluation: OpenTelemetry, Observability Framework"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "opentelemetry"
  - "observability"
  - "telemetry"
dc:description: "Re-score of OpenTelemetry against Universal Cake Evaluation Metrics 0.5.1, with the telemetry and content exposure gate verified by tracing the Collector 0.164.0 and the Java agent 2.33.1 under strace and OpenSnitch."
dc:publisher: "UniversalCake"
dcterms:created: "2026-08-21"
dcterms:modified: "2026-10-27"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/open-telemetry"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, uc-radar--evaluation-lifecycle, universal-cake-evaluation--opensnitch--application-firewall, uc-radar--telemetry-trace-procedure"
dc:identifier: "universal-cake-evaluation--opentelemetry--observability-framework"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Re-scored against Universal Cake Evaluation Metrics 0.5.1, which
      widened the telemetry gate to cover content exposure. The gate is
      now Verified by trace: the Collector 0.164.0 core distribution and
      an application under the Java agent 2.33.1 were run under strace
      with OpenSnitch 1.8.0 as a system-wide second instrument, with a
      negative control and a canary test for content exposure. Moved
      CVE-2026-54704 into the gate notes, added CVE-2026-54712, refreshed
      release, governance and contributor figures, and moved the scorecard
      into the house document format. First run of the working draft
      telemetry trace procedure.
  - version: "0.1.0"
    date: "2026-08-21"
    author: "Claude (Anthropic), drafted for Christopher Steel's review"
    notes: >
      Version number assigned retroactively to the unversioned
      opentelemetry-scorecard-2026-08-21 Assess-ring light pass against
      metrics 0.5.0.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude (Anthropic)"
      evaluator_version: ""
      recommendation: "trial"
      reasoning: "Assess-ring light pass of 2026-08-21, recorded in the earlier scorecard. No gate failed; telemetry gate rated on design evidence only."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.0"
      evaluation_datetime: "2026-08-21"
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "adopt" # as the observability standard for Vishpala-run infrastructure, with the deployment conditions in Recommendation; pending a human pass on the accessibility-floor gate
      reasoning: "Telemetry and content exposure gate passes, Verified by trace: the Collector core distribution made no outbound connections and the Java agent connected only to the local Collector. The Java agent exports process command-line arguments by default; password-like -D values are masked but positional arguments are not, and the attribute can be disabled (verified). Licence Apache-2.0, exit gate passes by design. Accessibility floor has no end-user content surface to test; recorded as not applicable, needs a human decision. Three 2026 CVEs in the Java instrumentation, all fixed in the version traced."
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-27T10:45:00-04:00"
  human: []
---

# Universal Cake Evaluation: OpenTelemetry, Observability Framework

Version: 0.2.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document re-scores OpenTelemetry against version 0.5.1 of the Universal Cake Evaluation Metrics. The earlier Assess-ring pass rated the telemetry gate Verified on the strength of the project's design. Under the evidence rules, that was really Inferred. This pass verifies it by trace. The Collector 0.164.0 core distribution and an application running under the Java agent 2.33.1 were traced under `strace`, with OpenSnitch 1.8.0 watching the whole machine. Neither made any connection beyond the local machine. The trace also found the one content-exposure default worth knowing about: the Java agent attaches every process's command-line arguments to the telemetry it exports. It masks values that look like passwords, but it does not mask positional arguments. That attribute can be switched off, and switching it off was verified. All four gates pass, or do not apply, and the recommendation is Adopt for Vishpala-run infrastructure under the conditions set out below.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a id="cite-uc-metrics"></a>([Steel, 2026a](#ref-uc-metrics)) and builds on the Assess-ring scorecard of 2026-08-21. The trace used OpenSnitch as described in its own evaluation <a id="cite-uc-opensnitch"></a>([Steel, 2026b](#ref-uc-opensnitch)). Release versions come from the project's git tags, read on 2026-10-27. Maturity and contributor figures come from the CNCF project page, which draws on LFX Insights, and vulnerability details from public advisory databases, all read the same day. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

OpenTelemetry is a CNCF project that provides vendor-neutral APIs, SDKs and a Collector for capturing traces, metrics and logs, together with an open wire protocol, OTLP. It is developer infrastructure rather than an end-user product. Its telemetry is data that the owner chooses to collect about their own systems. The gate question is narrower: does any part of OpenTelemetry send data anywhere the owner did not choose, and does any part expose content the owner would not expect to share?

The trace covered two components, chosen because they are what a typical first deployment runs:

- **The Collector core distribution**, `otelcol` 0.164.0, released 2026-10-26, with OTLP receivers, the batch processor and the debug exporter.
- **The Java agent**, `opentelemetry-javaagent` 2.33.1, attached to a small Java HTTP application with nothing configured except a service name.

Not traced: the contrib distribution and its 200-plus components, the SDKs for other languages, the Operator, and exporters to remote backends. A remote exporter sends data where the owner points it, so it is the intended behaviour rather than telemetry about the owner. Its security, such as transport encryption and the credentials it holds, belongs to each deployment's own Security row.

## Scorecard metadata

- Product or service name: OpenTelemetry
- Version evaluated: Collector core distribution 0.164.0; Java agent 2.33.1; specification 1.62.0, OTLP 1.11.1, semantic conventions 1.44.0 <a id="cite-otel-status"></a>([OpenTelemetry Authors, n.d.](#ref-otel-status))
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-27
- Radar ring: Trial, gate verification. See Status notes on how this relates to the lifecycle document
- Test platform: Ubuntu 24.04.5 LTS, Linux kernel 6.18, OpenJDK 21.0.12, OpenSnitch 1.8.0 upstream package with the iptables-legacy backend and `/proc` process monitor
- Assessment method: working draft of the uc-radar telemetry trace procedure, version 0.1.0, first run

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified, 2026-10-27 | Telemetry: over a seven-minute run the Collector bound only to `127.0.0.1` on ports 4317 and 4318 (OTLP) and 8888 (its own metrics, served locally for scraping, never pushed). It made no outbound connections. The Java agent connected only to `127.0.0.1:4318`, its default endpoint. OpenSnitch attributed every external connection on the machine during the run to the test host's own management processes, none to OpenTelemetry. Content exposure: the Java agent adds `process.command_args` to every export by default. Password-like `-D` property values are masked, but positional arguments are exported as given. Environment variables are not exported. Disabling the process resource provider removes the attribute (verified). CVE-2026-54704, clear-text database passwords in JDBC spans, is fixed in 2.28.0 and so absent from the version traced. See Findings |
| A licence incompatible with the project | Pass | Verified | Apache-2.0, read from the repository's `LICENSE` and the Java agent's bundled licence report. Compatible with use alongside AGPL-3.0-or-later work. Confirm against the adopting project's own licence policy, as the Gates at a glance table asks |
| Failure of the accessibility floor | Not applicable | Inferred | OpenTelemetry has no end-user content surface: its interfaces are configuration files, environment variables and command lines. The gate cannot be failed as the metrics define it, and it cannot be passed either. A human decision is needed on whether to record this as a pass, or as not applicable with a note, see Status notes |
| No exit | Pass | Verified | OTLP is an open, versioned protocol. In this trace the same telemetry went to the debug exporter with no change to the application; any OTLP-capable backend could receive it instead |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Not applicable | Inferred | See the accessibility-floor gate. The documentation site was not audited for WCAG conformance; that would be the only end-user surface to check |
| Multilingual integration | Strong | Verified, 2026-08-21 | Carried over: documentation in eleven languages with a language switcher. Software output is English only, as is usual for developer tooling |
| Economic and cognitive accessibility | Strong (affordability), Moderate (hardware and connectivity) | Verified (affordability), Verified in part (hardware) | Free, with no paid tier. The Collector core binary is 184 MB, large for older or storage-constrained machines. The agent adds a 26.5 MB jar to each Java process. Overhead was not benchmarked under load |
| Representation | Moderate | Verified, 2026-08-21 | Carried over: open SIG-based governance. The project's own 2025 contributor survey reported onboarding friction for newer contributors. Not a machine-learned product |
| Compatibility | Strong | Verified | SDKs for twelve or more languages. The core distribution and the agent ran on the test platform with no changes or extra dependencies |
| Resilience | Strong | Verified in part | Nothing in the trace depended on an outside service: the traced processes had no route to the internet, and both components worked normally. Collector buffering through a backend outage was not tested |
| Agency, sovereignty and privacy | Strong | Verified | The owner chooses every destination, and nothing phones home. Telemetry stays on the owner's machines until an exporter sends it to a backend the owner names. The caution is about what the owner's telemetry contains: command-line arguments by default, and whatever instrumentation captures. Data sent to a third-party backend should be reviewed with that in mind |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Verified (portability, pricing), Verified in part (exit cost) | Swapping the backend needs no change to instrumented code, demonstrated in this trace for one exporter. No terms of service or pricing |
| Agency, interaction patterns | Not applicable | Not applicable | No end-user interaction surface |
| Environment, direct, indirect, and authoring-side | Moderate | Inferred | Observability adds compute, memory and network load by nature. The Collector's binary size is a small indirect cost. No authoring interface |
| Security | Moderate to Strong | Verified | Published security policy and coordinated disclosure. Three 2026 advisories in the Java instrumentation: CVE-2026-33701, remote code execution through RMI deserialization, fixed in 2.26.1; CVE-2026-54712, denial of service through RMI context propagation, fixed in 2.27.0; CVE-2026-54704, clear-text passwords in JDBC spans, fixed in 2.28.0. All are fixed in the version traced. Collector releases are signed with cosign. The signatures and the agent's PGP signature were downloaded but not verified in this run |
| Longevity | Strong | Verified | CNCF Graduated on 2026-05-11, accepted 2019. Per the CNCF project page, which draws on LFX Insights, 24,739 contributors and 4,653 organizations in total, both lower than the previous year, by 16% and 25% <a id="cite-cncf-otel"></a>([Cloud Native Computing Foundation, n.d.](#ref-cncf-otel)). The decline is worth watching, not alarming at that scale. Collector minor releases roughly every two weeks through August to October 2026 |
| Content endurance | Strong | Inferred | Telemetry is in OTLP, an open protocol, and semantic conventions evolve through a public specification process |
| Exit and portability | Strong | Verified | See the No exit gate |
| Adjustability and support | Strong | Verified, 2026-08-21 | Carried over: Apache-2.0, large component ecosystem, SIGs, public documentation, Slack and mailing lists |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Strong | Verified, 2026-08-21 | Carried over: per LFX Insights, contribution spread across many individuals and organizations, no take rate, forkable under multi-vendor CNCF governance. Not refreshed in this pass |
| Gates | Pass, one not applicable | Verified | Telemetry and content exposure, licence and exit pass; accessibility floor not applicable |

## Findings

### The Collector makes no connections of its own

The Collector ran for about seven minutes under `strace`, which recorded every `connect`, `sendto`, `sendmsg`, `bind` and `listen` call made by it and its threads. It bound three sockets, all on `127.0.0.1`: the OTLP gRPC and HTTP receivers on ports 4317 and 4318, and its own metrics endpoint on port 8888. It made no outbound calls at all, not even a DNS lookup. The metrics endpoint is a local listener that a scraper may read. It is not a push to anyone.

### The Java agent talks only to the Collector

With no configuration except a service name, the Java agent connected to `127.0.0.1:4318`, its default OTLP endpoint, twice in 90 seconds. Its other socket calls belonged to the test application. It made no DNS lookups for outside hosts. The Collector logged 19 trace batches and 2 metric batches from it, so the pipeline was working.

### OpenSnitch saw the same, and more

OpenSnitch ran with its default action set to allow, so it logged connections without blocking them. During the run it recorded connections from the test host's own management processes to their service and to a logging endpoint. It attributed each one by process ID, and none of them came from OpenTelemetry. This is the main methodological lesson of the run: a system-wide instrument sees everything on the machine, so only attribution to a process makes its output evidence. A negative control confirmed that both instruments catch a real outbound attempt: `curl` to an outside site showed up in both, as a DNS query in `strace` and as a named connection in OpenSnitch.

### Command-line arguments are exported by default

A canary test ran the application with a marker in a positional argument, another in a `-Ddb.password=` system property, and a third in an environment variable. The Collector's detailed output showed `process.command_args` on every batch. The positional marker was exported as given. The property's value was replaced with `***`. The environment variable did not appear. With the process resource provider disabled through `OTEL_JAVA_DISABLED_RESOURCE_PROVIDERS`, none of the markers appeared, and only runtime name and version remained.

This is a content-exposure default, not a gate failure: it goes only where the owner sends telemetry, and it can be switched off. It matters for any deployment that passes secrets or personal data as program arguments, and for any deployment exporting to a third-party backend. The masking of password-like properties shows the project knows about the risk. CVE-2026-54704, where JDBC instrumentation leaked passwords into spans until 2.28.0 <a id="cite-cve-54704"></a>([OpenCVE, 2026](#ref-cve-54704)), belongs in the same class. Instrumentation sees whatever the process sees, so what it exports needs reviewing before it leaves the owner's machines.

### Security advisories in 2026

Three advisories affected the Java instrumentation this year. CVE-2026-33701 allowed remote code execution through unsafe deserialization in RMI instrumentation, fixed in 2.26.1 <a id="cite-cve-33701"></a>([National Institute of Standards and Technology, 2026](#ref-cve-33701)). CVE-2026-54712 allowed an oversized RMI context payload to exhaust memory, fixed in 2.27.0 <a id="cite-cve-54712"></a>([OSV, 2026](#ref-cve-54712)). CVE-2026-54704 is described above. All three were disclosed and fixed, and all are absent from 2.33.1. The pattern argues for keeping the agent current, and for disabling instrumentations a deployment does not need.

## Recommendation

Adopt OpenTelemetry as the observability standard for infrastructure Vishpala runs, pending a human pass on the accessibility-floor gate. Conditions:

- Use the core distribution of the Collector unless a contrib component is needed. Contrib components were not traced, so each one needs its own trace before use.
- Bind receivers to `127.0.0.1` or a private interface, as in this trace. Newer releases default to localhost, but configurations copied from older examples may not.
- For Java services, disable the process resource provider, or at least `process.command_args`, unless command lines are known to be free of secrets.
- Keep the Java agent at or above 2.28.0, and disable instrumentations the service does not use.
- Verify release signatures, cosign for the Collector and PGP for the agent, before deployment. This run did not, see the procedure's trial notes.
- Before exporting to any third-party backend, review a sample of the exported data with the debug exporter, as this trace did.

Re-review triggers specific to OpenTelemetry: a component that dials out on its own appearing in the core distribution, a change to default resource attributes in any SDK, a further security advisory in the instrumentation, and a continued fall in contributors or contributing organizations.

## Status notes

- Last reviewed: 2026-10-27.
- In trial: scope as above, on a disposable test host. The trace found nothing contradicting the Assess pass, and added the command-line default to it. For a completed trial: a human evaluation pass recorded in `uc-radar_evaluation`, a decision on the accessibility-floor gate, and a trace of any contrib component before it is used.
- Lifecycle note: the metrics document treats Trial as the gate-verification ring every item passes through. The lifecycle document, version 0.1.0, treats Trial as conditional, for high-risk items only. OpenTelemetry is not high risk, so under the lifecycle document it could have gone from Assess straight to a human adopt decision. The two documents should be reconciled. This entry uses Trial in the metrics document's sense.
- Gate fit: for the second time, the accessibility floor has nothing to grip on developer infrastructure. Adding Not applicable to the rating scale, with a rule for when it may be used, would settle this for future entries.

## License

This document, *Universal Cake Evaluation: OpenTelemetry, Observability Framework*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)
- [OpenSnitch evaluation](#ref-uc-opensnitch)

### OpenTelemetry

- [Specification status](#ref-otel-status)
- [CNCF project page](#ref-cncf-otel)

### Advisories

- [CVE-2026-33701](#ref-cve-33701)
- [CVE-2026-54704](#ref-cve-54704)
- [CVE-2026-54712](#ref-cve-54712)

## References

<a id="ref-cncf-otel"></a>Cloud Native Computing Foundation. (n.d.). *OpenTelemetry* [Project page]. Retrieved October 27, 2026, from https://www.cncf.io/projects/opentelemetry/
[Return to citation](#cite-cncf-otel)

<a id="ref-cve-33701"></a>National Institute of Standards and Technology. (2026). *CVE-2026-33701* [Vulnerability record]. National Vulnerability Database. https://nvd.nist.gov/vuln/detail/CVE-2026-33701
[Return to citation](#cite-cve-33701)

<a id="ref-cve-54704"></a>OpenCVE. (2026). *CVE-2026-54704* [Vulnerability record]. https://app.opencve.io/cve/CVE-2026-54704
[Return to citation](#cite-cve-54704)

<a id="ref-otel-status"></a>OpenTelemetry Authors. (n.d.). *Specification status summary*. Retrieved October 27, 2026, from https://opentelemetry.io/docs/specs/status/
[Return to citation](#cite-otel-status)

<a id="ref-cve-54712"></a>OSV. (2026). *CVE-2026-54712* [Vulnerability record]. https://osv.dev/vulnerability/CVE-2026-54712
[Return to citation](#cite-cve-54712)

<a id="ref-uc-metrics"></a>Steel, C. (2026a). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

<a id="ref-uc-opensnitch"></a>Steel, C. (2026b). *Universal Cake evaluation: OpenSnitch, application firewall* (Version 0.1.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-opensnitch)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft, version provisional | Re-scored against metrics 0.5.1; telemetry and content exposure gate verified by trace of the Collector 0.164.0 and Java agent 2.33.1; command-line argument export found and its disabling verified; CVE-2026-54704 moved to the gate, CVE-2026-54712 added; release, governance and contributor figures refreshed; moved to house document format |
| 0.1.0 | Assess, assigned retroactively | Assess-ring light pass against metrics 0.5.0, 2026-08-21 |

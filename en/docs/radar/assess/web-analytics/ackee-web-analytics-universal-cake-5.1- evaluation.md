Using **Universal Cake Evaluation Metrics v0.5.1**, and evaluating the current **Ackee v3.6.0** release, here is the assessment. The framework explicitly says the ratings should **not be aggregated into a single score**, so I have kept the dimensions separate.

## Ackee evaluation

**Product:** Ackee
**Version evaluated:** 3.6.0
**Evaluator:** Christopher Steel / Universal Cake
**Date:** 2026-09-15
**Radar ring:** Assess

Ackee is a self-hosted Node.js/MongoDB web-analytics application designed around anonymized, privacy-oriented analytics rather than individual visitor tracking. It is MIT-licensed and currently has a v3.6.0 release dated 2026-04-08. ([GitHub](https://github.com/electerious/ackee?utm_source=chatgpt.com))

| Metric area                            | Rating                                                    | Evidence          | Notes                                                        |
| -------------------------------------- | --------------------------------------------------------- | ----------------- | ------------------------------------------------------------ |
| Accessibility, alternative interaction | **Unknown**                                               | Inferred          | No sufficient current WCAG/AT evidence found. The accessibility floor therefore cannot be verified. |
| Multilingual integration               | **Unknown**                                               | Inferred          | No adequate evidence of interface/localization architecture or community language contribution process. |
| Economic accessibility                 | **Strong**                                                | Verified          | Open-source and self-hosted; no service subscription required. |
| Cognitive accessibility                | **Moderate**                                              | Inferred          | The project describes a minimal, focused interface, but this is not an accessibility assessment. ([GitHub](https://github.com/electerious/ackee?utm_source=chatgpt.com)) |
| Hardware/connectivity accessibility    | **Moderate**                                              | Inferred          | Server/client architecture is relatively conventional; current requirements include Node.js 24 and MongoDB. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com)) |
| Representation                         | **Unknown**                                               | Unknown           | Insufficient public evidence concerning affected-user representation, research practices, or accessibility representation. |
| Compatibility                          | **Moderate**                                              | Inferred          | Browser-based interface and conventional Node.js deployment are broadly portable, but Node.js 24 raises the server-side baseline. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com)) |
| Resilience                             | **Moderate**                                              | Inferred          | Self-hosting removes dependence on Ackee's hosted service, but operation depends on Node.js, MongoDB and the deployment infrastructure. ([GitHub](https://github.com/ckipp01/Ackee/blob/master/docs/Get started.md?utm_source=chatgpt.com)) |
| Agency, sovereignty & privacy          | **Strong**                                                | Verified/Claimed  | Self-hosting, no cookies, anonymization, and local control are central design properties. Ackee states that it does not store browsing history and that detailed tracking is optional and off by default. ([GitHub](https://github.com/electerious/ackee?utm_source=chatgpt.com)) |
| Power-imbalance proxies                | **Strong**                                                | Inferred          | Exit cost is comparatively low because the application and data are self-hosted and the software is MIT-licensed. However, export completeness and migration tooling require direct testing. |
| Interaction patterns                   | **Moderate**                                              | Inferred          | The product's stated purpose is measurement rather than engagement optimization. However, the actual UI has not been systematically tested against the framework's dark-pattern pairs. |
| Environment                            | **Moderate**                                              | Inferred          | Lightweight analytics architecture and a relatively small UI are positive signals, but no measured energy/bandwidth assessment was found. |
| Security                               | **Moderate**                                              | Verified/Inferred | Active releases include dependency/security updates; v3.5.0 specifically fixed a ReDoS vulnerability and v3.6.0 updated dependencies and Node/MongoDB support. The project also documents HTTPS and CORS configuration. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com)) |
| Longevity                              | **Moderate**                                              | Verified          | There is ongoing release activity, including v3.5.0, v3.5.1 and v3.6.0. The project has a substantial community footprint, but contributor concentration remains worth measuring. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com)) |
| Content endurance                      | **Strong**                                                | Inferred          | Analytics data resides in the deployer's MongoDB rather than in a vendor-controlled hosted service. The underlying data therefore remains under operator control. |
| Exit and portability                   | **Moderate**                                              | Inferred          | Self-hosting substantially reduces vendor lock-in, and a documented GraphQL API exists. However, I would want to test whether a complete dataset can be exported into a genuinely independent, human- and machine-readable representation. ([GitHub](https://github.com/electerious/ackee?utm_source=chatgpt.com)) |
| Adjustability and support              | **Strong**                                                | Verified/Inferred | MIT licence permits modification and redistribution. Source, documentation, API and deployment methods are public. ([GitHub](https://github.com/electerious/Ackee/blob/master/LICENSE?utm_source=chatgpt.com)) |
| Market position                        | **Moderate**                                              | Inferred          | Ackee itself does not appear to impose a significant platform dependency. It is an application rather than a dominant infrastructure platform. Contributor concentration and ecosystem dependency should still be measured. |
| Gates                                  | **Potentially pass; accessibility requires verification** | Mixed             | No evidence of mandatory telemetry, incompatible licensing, or absence of exit. Accessibility remains insufficiently established. |

## The interesting part: privacy and agency

Ackee aligns unusually closely with the **structural agency** portion of the Universal Cake framework.

Its architecture puts the analytics server under the operator's control rather than requiring visitor data to be sent to a third-party analytics company. The project explicitly describes itself as self-hosted and privacy-oriented, and its default model avoids cookies and unique-user tracking. ([GitHub](https://github.com/electerious/ackee?utm_source=chatgpt.com))

The tracker also makes the distinction between ordinary and detailed tracking explicit:

```text
detailed: false
```

The project recommends obtaining permission before enabling detailed tracking. ([GitHub](https://github.com/electerious/ackee-tracker?utm_source=chatgpt.com))

That is important under the Universal Cake model because **privacy is not simply the absence of surveillance**. It is also about who possesses the ability to decide what is collected, where it goes, and whether the relationship can be reversed.

Ackee gives the deploying organization substantially more control over those decisions than a conventional third-party analytics service.

## The important caveat

There is a subtle distinction between **"privacy-oriented"** and **"privacy verified."**

Ackee's own documentation says that it anonymizes tracked data and does not store browsing history. ([GitHub](https://github.com/electerious/Ackee/blob/master/docs/FAQ.md?utm_source=chatgpt.com))

But Universal Cake asks us to go one step further:

> *Measure reversibility, not promises.*

So I would want to verify, rather than simply accept, at least:

- exactly what the tracker sends;
- exactly what Ackee stores;
- whether IP addresses or derived identifiers ever persist;
- retention/deletion behavior;
- whether event tracking can introduce identifiable information;
- whether custom events can accidentally become personal-data collection;
- what MongoDB contains before and after TTL expiry;
- what the GraphQL API exposes;
- whether administrator access exposes more information than the visitor-facing model implies.

The framework explicitly distinguishes **Verified**, **Inferred**, and **Claimed** evidence.

## Security deserves particular attention

Ackee has a reasonably healthy recent maintenance signal. v3.5.0 included dependency updates and fixed a ReDoS vulnerability, while v3.5.1 included further security/dependency updates. v3.6.0 subsequently moved the supported Node.js baseline to 24 and updated the codebase for newer Node.js and MongoDB versions. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com))

There is also a useful architectural security property: the official Docker image was changed to run as a **non-root user** in v3.4.2. ([GitHub](https://github.com/electerious/Ackee/releases?utm_source=chatgpt.com))

I would nevertheless keep Security at **Moderate** rather than Strong until we actually inspect:

- dependency/SBOM exposure;
- container privileges;
- MongoDB authentication/TLS;
- HTTP security headers;
- authentication/session implementation;
- GraphQL exposure;
- tracker endpoint behavior;
- rate limiting;
- vulnerability disclosure process.

## One notable weakness: accessibility evidence

This is probably the largest gap in the current evaluation.

The Universal Cake framework makes accessibility a **gate** when content becomes unreachable for people using assistive technology.

I did not find enough authoritative evidence to say that Ackee's administrative interface satisfies that floor.

That does **not** mean Ackee is inaccessible. It means the appropriate v0.5.1 rating is **Unknown**, rather than assuming accessibility from the project's otherwise good engineering characteristics.

For an actual **Trial** evaluation, this would need direct keyboard/screen-reader testing.

## Overall pattern

Ackee's profile is interesting because its strongest properties are precisely the ones Universal Cake's **Agency** pillar was designed to expose:

**Self-hosting → control → reversibility → reduced vendor/user asymmetry.**

Its MIT licence further reinforces that relationship: the software can be copied, modified, forked and redistributed without permission from the original steward. ([GitHub](https://github.com/electerious/Ackee/blob/master/LICENSE?utm_source=chatgpt.com))

The main unresolved areas are **accessibility, representation, measurable environmental impact, and empirically verified data portability/security**.

So I would record the current radar entry as:

> **Ackee — Assess**

with the principal next step being a **Verified pass of the gates and Security section**, followed by direct accessibility and data-handling testing. This follows the framework's own lifecycle rule that Assess may contain Unknown/Inferred evidence, whereas Trial requires Verified gate and Security criteria.

One particularly useful next step would be to do a **deep Ackee evaluation against every individual v0.5.1 question**, rather than the condensed scorecard. That would give you a much better apples-to-apples comparison with Beluga and NorthMail.
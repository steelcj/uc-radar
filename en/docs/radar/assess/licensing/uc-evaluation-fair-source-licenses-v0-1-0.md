---
dc:title: "Universal Cake Evaluation: Fair Source Licenses"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "evaluation"
  - "licensing"
  - "fair source"
  - "power imbalances"
dc:description: "Universal Cake Evaluation Metrics v0.5.1 applied to the three licenses recommended by fair.io: the Functional Source License (FSL), the Fair Core License (FCL), and the Business Source License (BUSL)."
dc:publisher: "UniversalCake"
dcterms:created: "2026-09-30"
dcterms:modified: "2026-09-30"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:relation: "universal-cake-evaluation-metrics v0.5.1"
dc:identifier: "uc-evaluation-fair-source-licenses"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:version_at_creation: "0.1.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-09-30"
    author: "Claude (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      First evaluation of FSL-1.1, FCL-1.0, and BUSL-1.1 against the
      Universal Cake Evaluation Metrics v0.5.1, at the Assess ring.
---

# Universal Cake Evaluation: Fair Source Licenses

## Summary

None of the three licenses passes the licence-compatibility gate for code that would be incorporated into an AGPL-3.0-or-later project today, so all three sit on hold for that use; each version becomes compatible once it converts to Open Source. For running Fair Source software as a separate tool, FSL is the strongest of the three, FCL adds a license-key dependency that needs a per-product telemetry check, and BUSL depends almost entirely on each project's Additional Use Grant.

## Scope and assumptions

The item evaluated is a licence, not a product, so this is an evaluation of an approach, which the radar covers alongside products and services. A licence sets the ceiling on several metric areas (sovereignty, exit, forkability, affordability) and says nothing about others (accessibility of an interface, energy use, telemetry actually shipped). Rows a licence cannot affect were removed from the scorecards, per the metrics' row-granularity rule, and are listed under Rows not scored.

Two assumptions shape the results:

- **Perspective.** Ratings take the adopter's view: Universal Cake or Vishpala using software released under these licences. The producer view, releasing your own work under them, is noted briefly at the end.
- **Project licence policy.** The licence-compatibility gate is checked against an assumed policy of OSI-approved licences compatible with AGPL-3.0-or-later, the licence of the metrics document itself. If the actual project policy differs, re-check that gate.

Evidence tags: licence terms were read directly from the published licence texts on 2026-09-30, so ratings resting on the text are tagged Verified (method: reading the licence text). Consequences drawn from the text are Inferred. Statements from licence stewards' websites are Claimed.

## At a glance

| Metric area | FSL-1.1 | FCL-1.0 | BUSL-1.1 |
| --- | --- | --- | --- |
| Affordability | Strong | Moderate | Weak |
| Cognitive accessibility (of the licence) | Strong | Moderate | Weak |
| Multilingual integration | Unknown | Unknown | Unknown |
| Resilience | Strong | Weak | Strong |
| Agency, sovereignty | Moderate | Moderate | Weak |
| Agency, power-imbalance proxies | Moderate | Moderate | Weak |
| Longevity | Moderate | Unknown | Moderate |
| Content endurance | Strong | Strong | Moderate |
| Exit and portability | Strong | Strong | Moderate |
| Adjustability and support | Strong | Moderate | Weak |
| Market position (forkability) | Moderate | Moderate | Weak |
| Gates | Fail (licence, for incorporation) | Fail (licence, for incorporation); telemetry Unknown | Fail (licence, for incorporation) |

Ratings are not summed; read each row as its own tradeoff.

## Functional Source License (FSL)

- Product or service name: Functional Source License
- Version evaluated: FSL-1.1 (ALv2 and MIT variants)
- Evaluator: Claude (Anthropic), for Christopher Steel's review
- Date: 2026-09-30
- Radar ring: Assess

| Metric area | Rating | Evidence | Notes |
| --- | --- | --- | --- |
| Affordability | Strong | Verified | No fee for any Permitted Purpose, which is everything except a Competing Use <a id="cite-fsl-text-1"></a>[(Sentry, n.d.-b)](#ref-fsl-text). |
| Cognitive accessibility | Strong | Verified | About 580 words, one fixed set of terms, no per-project parameters to look up. |
| Multilingual integration | Unknown | Inferred | No official translations found on fsl.software. Resolve by checking the GitHub repository for translations. |
| Resilience | Strong | Verified | No licence server, key, or expiring token; the grant does not depend on the licensor staying in business. |
| Agency, sovereignty | Moderate | Verified | Use, copy, modify, and redistribute without asking, for any non-competing purpose, including internal use and professional services to a licensee. Limitation: no competing commercial product or service. |
| Agency, power-imbalance proxies | Moderate | Inferred | Exit cost: the licence adds no fee to leave, and older versions are already Apache 2.0 or MIT. ToS volatility: each version's terms are fixed once released, and the future grant is stated as irrevocable; the licensor can still relicense future versions. Pricing asymmetry: none in the licence itself. Main risk: "Competing Use" is defined by what the licensor offers at release, so the line can be unclear for a hosting practice like Vishpala. |
| Longevity | Moderate | Claimed | Stewarded by Sentry and promoted as fair.io's flagship licence <a id="cite-fairio-licenses-1"></a>[(Fair Source, n.d.)](#ref-fairio-licenses); listed on the SPDX licence list as FSL-1.1-MIT and FSL-1.1-ALv2. Young (2023 onward). |
| Content endurance | Strong | Verified | Every released version converts to Apache 2.0 or MIT two years after release, so tooling around stored content outlives the licensor. |
| Exit and portability | Strong | Verified | Nothing in the licence restricts extraction or migration; data formats remain a per-product question. |
| Adjustability and support | Strong | Verified | The fork-and-fix path is open for internal and non-competing use, and patches can be proposed upstream. Includes a patent licence, with defensive termination if you sue over patents. |
| Market position (forkability) | Moderate | Inferred | A community can fork freely only from versions at least two years old, or for non-competing purposes. Contributor concentration is typically a single company. |
| Gates | Fail (licence, for incorporation) | Inferred | Not OSI Open Source until conversion, so incompatible with AGPL-3.0-or-later for code incorporated into the project. Telemetry: Pass, the licence imposes none. Exit: Pass. Converted versions pass the licence gate, since Apache 2.0 and MIT are AGPLv3-compatible. |

## Fair Core License (FCL)

- Product or service name: Fair Core License
- Version evaluated: FCL-1.0 (ALv2 and MIT variants)
- Evaluator: Claude (Anthropic), for Christopher Steel's review
- Date: 2026-09-30
- Radar ring: Assess

FCL-1.0 is FSL-1.1 plus a Limitations clause, a stricter notice clause, and a void-if-disclaimer-unenforceable clause <a id="cite-fcl-text-1"></a>[(Keygen, n.d.-b)](#ref-fcl-text). Rows identical to FSL are noted as such.

| Metric area | Rating | Evidence | Notes |
| --- | --- | --- | --- |
| Affordability | Moderate | Verified | The core is free for non-competing use; features behind a license key require payment until each version converts. |
| Cognitive accessibility | Moderate | Verified | About 650 words; the license-key clause means users must know which features are key-protected. |
| Multilingual integration | Unknown | Inferred | No official translations found on fcl.dev. |
| Resilience | Weak | Inferred | Commercial features depend on license keys. If the licensor shuts down and keys cannot be renewed, those features are lost until the version converts, up to two years. Core features are unaffected. |
| Agency, sovereignty | Moderate | Verified | As FSL, but you must not move, change, disable, or circumvent the license-key functionality, or modify key-protected code to unlock or remove it. The owner cannot fully control what they run. |
| Agency, power-imbalance proxies | Moderate | Inferred | As FSL, plus pricing asymmetry: commercial feature pricing is set by the licensor outside the licence. |
| Longevity | Unknown | Claimed | Stewarded by Keygen; drafted by Heather Meeker <a id="cite-fcl-site-1"></a>[(Keygen, n.d.-a)](#ref-fcl-site). Adoption numbers not checked; resolve with the adopter list the FCL site links to. |
| Content endurance | Strong | Verified | Same two-year conversion as FSL, and it covers the commercial features too, unlike Open Core. |
| Exit and portability | Strong | Verified | As FSL. |
| Adjustability and support | Moderate | Verified | Fork-and-fix is open, except for key-protected code, which cannot be altered to remove or bypass the key. |
| Market position (forkability) | Moderate | Inferred | As FSL. |
| Gates | Fail (licence, for incorporation); telemetry Unknown | Inferred | Licence gate as FSL. Telemetry gate: the licence forbids disabling license-key functionality. If a product's key check contacts a licence server, that contact cannot lawfully be disabled, which would fail the gate. Resolve per product with a network monitor. |

## Business Source License (BUSL)

- Product or service name: Business Source License
- Version evaluated: BUSL-1.1
- Evaluator: Claude (Anthropic), for Christopher Steel's review
- Date: 2026-09-30
- Radar ring: Assess

BUSL is a template: each project fills in an Additional Use Grant, a Change Date, and a Change License <a id="cite-busl-text-1"></a>[(MariaDB, n.d.-b)](#ref-busl-text). The ratings below are for the template's baseline; any specific BUSL product must also be rated on its own parameters.

| Metric area | Rating | Evidence | Notes |
| --- | --- | --- | --- |
| Affordability | Weak | Verified | The base grant covers non-production use only. Production use needs the project's Additional Use Grant, which may be "None", or a purchased commercial licence. |
| Cognitive accessibility | Weak | Verified | Short text, but meaning depends on three per-project parameters; the FSL authors note each BUSL deployment is effectively a new licence <a id="cite-fsl-site-1"></a>[(Sentry, n.d.-a)](#ref-fsl-site). |
| Multilingual integration | Unknown | Inferred | No official translations found. |
| Resilience | Strong | Verified | No key or licence-server mechanism in the licence itself. |
| Agency, sovereignty | Weak | Verified | Copy, modify, and redistribute are granted, but production use is not, unless the Additional Use Grant allows it. |
| Agency, power-imbalance proxies | Weak | Verified | Pricing asymmetry: out-of-scope use requires buying a commercial licence at undisclosed terms. Termination: any violation ends rights for the current and all other versions. The licensor sets the Change Date per version. |
| Longevity | Moderate | Claimed | Stewarded by MariaDB since 2017 and widely adopted. |
| Content endurance | Moderate | Verified | Each version converts on its Change Date or four years after release, whichever comes first. |
| Exit and portability | Moderate | Inferred | The licence does not restrict extraction, but non-production-only terms can make running the tool long enough to migrate out a licensing question. |
| Adjustability and support | Weak | Verified | You can modify the code, but you can only run modified code in production if the Additional Use Grant allows it. No patent grant. |
| Market position (forkability) | Weak | Inferred | A fork can only start from converted versions, up to four years old. When HashiCorp moved Terraform to BUSL in 2023, the community fork OpenTofu was taken from the last MPL-licensed release rather than from BUSL code <a id="cite-opentofu-1"></a>[(OpenTofu, n.d.)](#ref-opentofu). |
| Gates | Fail (licence, for incorporation) | Verified | Not Open Source before the Change Date. After conversion, the Change License must be GPL-2.0-or-later or GPL-compatible; a GPL-2.0-only Change License would still be incompatible with AGPLv3, so check each product. Telemetry: Pass. Exit: Pass. |

## Rows not scored

These rows were removed because a licence cannot affect them; they must be rated per product: Accessibility (alternative interaction, time-based media, resizing and reflow), Compatibility, Representation, Agency interaction patterns, Environment, Security, and the AI-specific proxies. None of the three licences addresses training-data use of the code.

One licence-level point touches Security. All three licences make source publicly readable, so Security questions for Fair Source products can carry Verified tags from direct inspection, which closed-source products usually cannot.

## Radar placement

| Use | FSL | FCL | BUSL |
| --- | --- | --- | --- |
| Incorporating current code into an AGPL-3.0-or-later project | Hold | Hold | Hold |
| Incorporating converted versions | Assess (Apache 2.0 or MIT) | Assess (Apache 2.0 or MIT) | Assess, if the Change License is AGPLv3-compatible |
| Running as a separate tool, internal use or client hosting | Assess | Assess, after a telemetry check | Assess, only where the Additional Use Grant covers production |

For a hosting practice, check each product's Competing Use line carefully. FSL explicitly permits professional services to a licensee, but hosting that substitutes for the licensor's own hosted offering would be a Competing Use.

## Producer view

If Universal Cake or Vishpala released its own work under one of these licences, the ratings flip in places. FSL would protect a hosting business from direct clones while giving clients source access and a two-year Open Source guarantee. However, the work would not be Open Source at release, which conflicts with the practice's open source positioning and with AGPL-licensed dependencies it may use.

## References

<a id="ref-fairio-licenses"></a>Fair Source. (n.d.). *Fair Source licenses*. Retrieved September 30, 2026, from <https://fair.io/licenses/> [↩](#cite-fairio-licenses-1)

<a id="ref-fcl-site"></a>Keygen. (n.d.-a). *FCL: Fair Core License*. Retrieved September 30, 2026, from <https://fcl.dev/> [↩](#cite-fcl-site-1)

<a id="ref-fcl-text"></a>Keygen. (n.d.-b). *Fair Core License, version 1.0, ALv2 future license* [Licence text]. GitHub. Retrieved September 30, 2026, from <https://github.com/keygen-sh/fcl.dev/tree/master/FCL-1.0-ALv2.md> [↩](#cite-fcl-text-1)

<a id="ref-busl-text"></a>MariaDB. (n.d.-b). *Business Source License 1.1* [Licence text]. Retrieved September 30, 2026, from <https://mariadb.com/bsl11/> [↩](#cite-busl-text-1)

<a id="ref-opentofu"></a>OpenTofu. (n.d.). *The OpenTofu manifesto*. Retrieved September 30, 2026, from <https://opentofu.org/manifesto> [↩](#cite-opentofu-1)

<a id="ref-fsl-site"></a>Sentry. (n.d.-a). *FSL: Functional Source License*. Retrieved September 30, 2026, from <https://fsl.software/> [↩](#cite-fsl-site-1)

<a id="ref-fsl-text"></a>Sentry. (n.d.-b). *Functional Source License, version 1.1, ALv2 future license* [Licence text]. GitHub. Retrieved September 30, 2026, from <https://github.com/getsentry/fsl.software/blob/main/FSL-1.1-ALv2.template.md> [↩](#cite-fsl-text-1)

Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1). UniversalCake.

## License

This document, *Universal Cake Evaluation: Fair Source Licenses*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
| --- | --- | --- |
| 0.1.0 | Provisional, pending confirmation | First evaluation of FSL-1.1, FCL-1.0, and BUSL-1.1 at the Assess ring |

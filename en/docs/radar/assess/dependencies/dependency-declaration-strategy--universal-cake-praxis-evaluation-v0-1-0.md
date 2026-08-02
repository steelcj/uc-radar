---
dcterms:title: "Dependency Declaration Strategy: Universal Cake Praxis Evaluation"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "evaluation"
  - "metrics"
  - "dependencies"
  - "dev-toolchain"
dcterms:description: "A universal-cake praxis evaluation of the layered, co-located requirements strategy, scored against the universal-cake evaluation metrics v0.3.1 scorecard."
dcterms:created: "2026-08-02"
dcterms:modified: "2026-08-02"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:identifier: "dependency-declaration-strategy--universal-cake-praxis-evaluation"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:changelog:
  - version: "0.1.0"
    note: "First evaluation, scored against universal-cake evaluation metrics v0.3.1."
---

# dependency-declaration-strategy--universal-cake-praxis-evaluation

## Abstract

This evaluation scores the proposed dependency-declaration strategy against the universal-cake evaluation metrics, version 0.3.1. The subject is an internal developer-toolchain technique rather than a consumer product, so several pillars written for products in the wild, such as accessibility, representation, interaction patterns, environment, and market position, are marked N/A with a recorded reason. The load-bearing rows for a packaging convention are compatibility, resilience, agency in the form of exit and portability, security as supply-chain posture, longevity, adjustability, and the gates.

## Subject summary

The strategy declares each component's third-party dependencies in a co-located `<component>.requirements.txt`, composes them into two audience aggregators (`requirements.txt` for runtime and users, `requirements-dev.txt` for developers), keeps package-abstract dependencies in `pyproject.toml`, and makes the installer apply the runtime aggregator so the resolved virtual environment is the single licence-audit surface. The full description and its risks are in the companion radar entry.

## How this was scored

Ratings use the metrics scale of Strong, Moderate, Weak, or Unknown, and each carries an evidence tag of Verified, Inferred, or Claimed. Verified means confirmed by direct inspection or testing in this project, for example the observed `mdformat` install gap and the confirmed behaviour of `pip install "./en/lib/satlib[markdown]"`. Inferred means a reasonable conclusion from the design that has not been separately tested. N/A rows state why the pillar does not apply to an internal packaging technique.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | N/A | Verified | Internal dev tooling with no end-user interaction surface. |
| Multilingual integration | N/A | Verified | Declaration files carry no natural-language content. |
| Economic and cognitive accessibility | Moderate | Inferred | Plain-text files and one-command install lower developer cognitive load, but the compose model must be learned. |
| Representation | N/A | Verified | No designer-to-designed-for relationship in scope. |
| Compatibility | Strong | Verified | Standard pip requirements, portable anywhere pip runs; the extras-install path was confirmed here. |
| Resilience | Moderate | Inferred | Additive growth and small blast radius, offset by drift risk between two declaration sites. |
| Agency, sovereignty and privacy | Strong | Inferred | Plain-text, vendor-neutral, no service dependency, fully forkable. |
| Agency, power-imbalance proxies (exit cost, portability) | Strong | Verified | Zero exit cost, a decades-stable open format, trivially portable. |
| Agency, interaction patterns | N/A | Verified | No runtime user interaction, so no dark-pattern surface. |
| Environment, direct and indirect | N/A | Inferred | Negligible; a declaration convention consumes no runtime resources of its own. |
| Security | Moderate | Inferred | Pinning plus a generated licence ledger improve supply-chain posture; absent hash-locking, unpinned ranges are a residual risk. |
| Longevity | Strong | Inferred | The requirements-file format is stable and widely supported over the long term. |
| Content endurance | N/A | Verified | The subject is tooling configuration, not archived content. |
| Exit and portability | Strong | Verified | Greppable, diffable, copyable plain text with no proprietary encoding. |
| Adjustability and support | Strong | Inferred | Extending coverage is one new file plus one include line, no installer change. |
| Market position | N/A | Verified | An internal convention, not a product in a market. |
| Gates | Pass | Verified | No telemetry or content exposure, licence-compatible and licence-improving, accessibility floor N/A, exit is trivial. No gate tripped. |

## Interpretation

The strategy scores Strong on every axis that matters for an internal packaging technique: compatibility, longevity, exit and portability, adjustability, and agency. It carries no failed gate. The two honest limitations are the drift risk between `pyproject.toml` and the requirements layer, and the absence of true lockfile determinism, since plain requirements files pin direct versions but do not lock the transitive graph. Both are addressable and neither is a gate. On the evidence, the technique is a sound default for the current stage, with a clear path to hardening later.

## Recommended interventions

- Implement the split: repurpose `requirements.txt` as the runtime ledger, move current pins to `requirements-dev.txt`, and add `satlib.requirements.txt` referencing `./en/lib/satlib[markdown]`.
- Point the installer at `pip install -r requirements.txt` and harden its smoke test to assert `import mdformat`, so a venv that cannot ingress never verifies green.
- Generate `legal/DEPENDENCIES.md` from the resolved runtime virtual environment with pip-licenses, closing the licence-tracking gap the same change opens.
- Treat a lockfile tool, pip-tools or uv, as a tracked follow-on to add transitive determinism if reproducibility needs grow.

## Links

- Companion radar entry: dependency-declaration-strategy--radar-entry-v0-1-0.md
- Metrics framework: universal-cake evaluation metrics, version 0.3.1
- Related specification: en/docs/specifications/dependency-licence-management.md

## License (for this document)

Copyright 2026 Christopher Steel. SPDX-License-Identifier: AGPL-3.0-or-later

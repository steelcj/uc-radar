---
dc:title: "Universal Cake Evaluation: Sveltia CMS vs SveltyCMS"
dcterms:version: "0.0.1"
dc:creator: "Christopher Steel"
dc:contributor: "Augure"
dc:subject:
 - "evaluation"
 - "CMS"
 - "headless CMS"
 - "git-based CMS"
 - "inclusive design"
 - "agency"
 - "sovereignty"
dc:description: >
 Assess-ring light pass evaluating Sveltia CMS (sveltiacms.app) and
 SveltyCMS (sveltycms.com) against the Universal Cake Evaluation Metrics
 v0.5.1. Ratings are predominantly Inferred or Unknown at this stage; the
 document records what would resolve each Unknown before a Trial-gate pass.
dc:publisher: "UniversalCake"
dcterms:created: "2026-08-25"
dcterms:modified: "2026-08-25"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "universal-cake-evaluation-metrics"
dc:identifier: "universal-cake-evaluation-sveltia-vs-sveltycms"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
 Copyright 2026 Christopher Steel / UniversalCake.
 SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.0.1"
sat:migration_status: pre-sat
sat:changelog:
 - version: "0.0.1"
 date: "2026-08-25"
 author: "Augure, drafted for Christopher Steel's review"
 notes: >
 VERSION NUMBER PROVISIONAL, pending Christopher's confirmation per
 house convention that version bumping is his call. Initial
 Assess-ring light pass comparing Sveltia CMS and SveltyCMS against
 the v0.5.1 metrics. Both pass the four default gates on available
 evidence, with the Exit and portability gate for SveltyCMS recorded
 as Unresolved pending verification of its export path. Notes the
 one-letter name similarity between the two projects as a hazard for
 the adoption record. All ratings are Inferred or Unknown except
 where the architecture itself guarantees the outcome; no Verified
 tags carried at this stage.
---

# Universal Cake Evaluation: Sveltia CMS vs SveltyCMS

## Metadata

- Product or service name: Sveltia CMS (sveltiacms.app) vs SveltyCMS (sveltycms.com)
- Version evaluated: latest as of 2026-08-25, unspecified by either vendor
- Evaluator: Christopher Steel (drafted by Augure)
- Date: 2026-08-25
- Radar ring: Assess

Method note: both are web-based CMS products with confusingly similar
names separated by one letter. Sveltia CMS is a git-based CMS positioned
as a drop-in successor to Netlify/Decap CMS. SveltyCMS is a headless CMS
built on Svelte 5, marketed against Strapi and Payload. This architectural
difference drives most of the divergence below.

## Scorecard

| Metric area | Sveltia CMS | SveltyCMS |
|-------------|-------------|-----------|
| Accessibility, alternative interaction | Unknown. No VPAT or ATAG conformance published. Modern UI claimed, not verified. | Unknown. No conformance data located. |
| Multilingual integration | Moderate (Inferred). Decap heritage supports localization via data files; not directly verified for this fork. | Unknown. Localization approach undocumented in what we reviewed. |
| Economic and cognitive accessibility | Strong / Moderate (Inferred). Free and open source. Markdown editing is learnable and forgiving. | Unknown. Pricing and hosting model unverified. |
| Representation | Unknown. No disclosed team composition or user-research practices. No machine learning involved, so training-data proxies do not apply. | Unknown. No disclosed team composition. Machine-learning use unverified; if present, training-data and bias-testing proxies apply. |
| Compatibility | Moderate (Inferred). Browser-based; depends on a git host and its auth. | Unknown. Database-backed; runtime requirements unverified. |
| Resilience | Strong (Inferred). Content is plain markdown in a git repo. No licence server, expiring tokens, or mandatory live service identified. Works offline once loaded. | Unknown. Requires a live server and database; offline behavior unverified. |
| Agency, sovereignty and privacy | Strong (Inferred). Self-hostable. Data rests in the user's own repository. No mandatory telemetry identified, but phone-home behavior not yet inspected in the build. | Unknown. Self-host versus SaaS model unverified. Telemetry and data-residency unverified. |
| Agency, power-imbalance proxies | Strong (Inferred). Exit cost near zero: content is plain markdown, machine-readable and human-readable. Pricing is public. Terms-of-service volatility not yet measured. | Unknown. Export mechanism, pricing transparency, and ToS history unverified. |
| Agency, interaction patterns | Moderate (Inferred). Editing UI with no evident dark or engagement patterns. Supplementary questions not yet applied to a live session. | Unknown. Editor experience and interaction patterns unverified. |
| Environment | Moderate (Inferred). Client-side Svelte bundle implies some authoring-side load; no ML training or inference. Bandwidth profile unverified. | Unknown. Persistent database and API runtime imply ongoing server-side energy; no figures available. |
| Security | Moderate (Inferred). Auth delegated to the git host. Runtime dependency count and supply-chain pinning unverified. | Unknown. Attack surface and dependency posture unverified. |
| Longevity | Weak to Moderate (Inferred). Small, likely concentrated contributor base. Institutional backing unverified. | Unknown. Newer project; contributor pool and funding unverified. |
| Content endurance | Strong (Verified by architecture). Content is authored as markdown and survives removal of the tool. Enhancement layer, not container. | Moderate (Inferred). Content lives in a database; survivability depends on export features. |
| Exit and portability **[GATE]** | Pass. Markdown and git provide trivial, open-format extraction. | Unresolved. No verified export path located. Must resolve before a Trial-gate pass. |
| Adjustability and support | Strong (Inferred). Open and forkable. Licence presumed MIT from Decap heritage but not yet confirmed. Maintainer reachability unverified. | Unknown. Fork path and licence unverified. |
| Market position | Mixed. Ecosystem-level forkability appears good. Contributor concentration likely high. Take rate not applicable. | Unknown. Foundation-model dependency unverified; market position not assessable. |
| Gates | Pass on available evidence. | Pass except Exit and portability, recorded as Unresolved. |

## Notes and observations

- Row granularity follows the v0.5.1 default template. The Algorithmic
 and AI-behavior proxies are scored within their home rows rather than
  as separate rows, per the v0.5.0 note.
- The decisive divergence is architectural. Sveltia stores content as
 markdown in the user's repository, so sovereignty, portability, and
  content endurance are structural strengths. SveltyCMS is a
  database-backed API CMS, so those same axes hinge on its export
  features and hosting model, none of which we could verify.
- The one-letter name similarity between the two projects is a genuine
 hazard. Flag it in the adoption record to prevent future confusion.
- Reversibility test, per the measuring-power-asymmetry principle:
 if Sveltia's steward turned hostile tomorrow, migrating costs near
  zero because content is already in plain markdown. For SveltyCMS the
  equivalent cost is unknown until its export path is verified.

## Outstanding items to reach Trial

All gate criteria and the Security section must carry Verified tags
before a Trial-gate pass. What would resolve the Unknowns above:

1. Exact SPDX licence for each project, read from each repository.
2. Telemetry, analytics, update-check, or licence-server behavior in
 each shipped build, confirmed by inspection.
3. SveltyCMS export formats and whether a complete, documented export
 exists, machine-readable and human-readable.
4. Commit concentration for both projects via LFX Insights, to
 solidify Longevity and Market position ratings.
5. Whether either project uses machine-learned models, and if so,
 the training-data and model-supply-chain proxies.
6. SveltyCMS pricing model and hosting options.

## References

Sources reviewed for this pass:

- Sveltia CMS documentation, successor positioning relative to
 Netlify/Decap CMS
- SveltyCMS marketing site, positioning against Strapi and Payload
- Community discussion listing Sveltia CMS showcase sites

Standards referenced by the framework: WCAG 2.2, ATAG 2.0, UAAG 1.0,
CVSS, OWASP Top Ten, GDPR Article 20, SPDX, OSI, FSF, AGPL. None of the
two products' conformance to these was verified in this pass.
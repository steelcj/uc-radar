---
dcterms:title: "Locale-Neutral Image Pool with Native Markdown and a Decorative-Alt Sentinel"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "radar"
  - "images"
  - "i18n"
  - "content-architecture"
  - "sveltia"
dcterms:description: "Radar entry for the content-image-handling convention recommended in survey--image-handling-sveltia-cms-sites-conclusions-v0.1.0: a flat, locale-neutral media pool, native markdown image syntax with Pandoc-style attributes, per-locale alt text, an explicit decorative-image sentinel, and build-time check:* validation in place of structural prevention."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-08-14"
dcterms:modified: "2026-08-14"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: ""
dcterms:relation: "survey--image-handling-sveltia-cms-sites, survey--image-handling-sveltia-cms-sites-conclusions, overview--ingress-process, universal-cake-evaluation-metrics"
dcterms:identifier: "uc-radar--markdown-image-pool--content-architecture-v0-1-0"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-08-14"
    author: "Christopher Steel, Claude (Anthropic)"
    notes: >
      Initial entry. Documents the image-handling approach recommended in
      survey--image-handling-sveltia-cms-sites-conclusions-v0.1.0, following
      a four-site survey of comparable Sveltia CMS projects. Evaluation
      filled in against universal-cake-evaluation-metrics-v0.3.1. No
      implementation exists yet — this is an assess-ring entry for an
      architectural recommendation, not a shipped feature.
uc-radar_evaluation:
  automated: []
  human:
    - evaluator: "Christopher Steel, Claude (Anthropic)"
      evaluator_version: "n/a — manual pass"
      recommendation: "hold-for-trial" # see Status notes: no clean fit in the adopt/hold/rejected/high-risk vocabulary; recorded literally rather than forced into a mismatched value, per the open-conventions precedent already noted in the entry template itself
      reasoning: >
        Pattern is sound and preferred over the two alternatives surveyed,
        but the Security pillar and the Exit-and-portability gate cannot
        yet carry Verified tags because no implementation exists. Per the
        metrics document's lifecycle guidance, an item cannot move to trial
        until gate criteria and Security are Verified.
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-08-14"
---

# uc-radar--markdown-image-pool--content-architecture-v0.1.0

## What it is

A content-image-handling convention, not a third-party product: a single, flat, locale-neutral media pool referenced by native markdown `![alt](src)` syntax extended with Pandoc-style attributes; alt text carried per-locale by virtue of markdown bodies already being per-locale files; an explicit, controlled "decorative" sentinel for images with no meaningful alt text; and build-time `check:*`-style validation (resolvability, orphan-detection, cross-locale parity) rather than structural prevention of malformed references.

## Why interesting

Recommended after surveying four real Sveltia CMS sites (IDRC/Eleventy, APU Viajes de Montaña/SvelteKit, Green Software Foundation/Astro, Al-Qadisiyah's QA department/Hugo) for how they actually solve this problem. Two structurally distinct approaches were found working in production — mediated authoring through a custom shortcode (IDRC), and native markdown against a shared pool with build-time drift checks (APU) — plus a third, native field-locking (Green Software Foundation), that this project had already ruled out on other grounds. This entry documents the hybrid recommended in `survey--image-handling-sveltia-cms-sites-conclusions-v0.1.0`: APU's structure, with IDRC's decorative-sentinel idea grafted on.

It matters to this project specifically because it keeps content authored as plain, portable markdown — the "content drives the site" principle this whole design thread started from — and because it extends a build-guard habit (`check:i18n`, and the drift-checking instinct behind `generate-cms-i18n.js`) this project already has, rather than introducing a new one.

## Concerns

- **Does not solve the problem that motivated the survey.** A pre-existing, non-CMS-authored seed file — a Typora export, say, with sidecar-relative image paths — has no accommodation in this convention. Every site surveyed assumed content originates inside its own authoring surface; this recommendation inherits that same limitation rather than closing it. It makes the eventual gap *smaller* (a relative markdown path is easier to detect and rewrite than a call to a site-specific shortcode would have been) but not solved.
- **Nothing described here is built yet.** The `check:images`/`check:orphan-images`/`check:portfolio-parity`-style validators are named by precedent, not implemented. Until they exist, a malformed or broken image reference will not be caught by anything.
- **The decorative-sentinel syntax is unspecified.** APU's precedent uses Pandoc image attributes; IDRC's uses a shortcode argument. This entry recommends the pattern (a controlled value meaning "no meaningful alt text," never a blank or omitted attribute) without settling its exact expression, which depends on a markdown pipeline choice this project hasn't made yet.
- **Pipeline-dependent.** The whole convention assumes a markdown rendering pipeline that supports Pandoc-style attribute expansion and build-time responsive-image injection (per APU's `rendered-pages.ts`). If this project's eventual pipeline (Eleventy's own, or something custom) doesn't support that, the decorative-sentinel mechanism needs a different expression, and the responsive-image generation needs a different implementation.

## Evaluation

Ratings below are necessarily softer than a typical radar entry for a shipped third-party product — this is an architectural recommendation with no implementation, so most evidence is Inferred rather than Verified, and several metrics (Representation, Market Position) don't map cleanly onto an internal convention rather than a vendor relationship. Recorded as such rather than forced.

### Inclusive

**Accessibility**

- Rating: Strong
- Evidence: Inferred
- Notes: Alt text is a first-class, per-locale element by construction, and the decorative sentinel (borrowed from IDRC's precedent) prevents "no alt text" from silently becoming a blank or missing attribute — directly serves the accessibility floor gate. Inferred rather than Verified: nothing exists yet to test with actual assistive technology.

**Representation**

- Rating: Unknown
- Evidence: Unknown
- Notes: This metric concerns who is on the team making design decisions and whether affected-community research is compensated — not something an architecture pattern itself determines. Recorded as Unknown rather than skipped, per the metrics document's own instruction that an unanswerable question is itself a data point.

**Resilience**

- Rating: Moderate
- Evidence: Inferred
- Notes: Responsive image variants generated and committed at build time (per APU's precedent) mean the *built* site has no runtime dependency on a live image-processing service. Moderate rather than Strong because the *build* itself depends on whichever pipeline and image-processing library this project eventually chooses — a dependency not evaluated here since the pipeline isn't chosen yet.

### Agency

**Sovereignty & Privacy**

- Rating: Strong
- Evidence: Verified
- Notes: Content stays in plain markdown in this project's own repository; no vendor relationship is introduced by this convention specifically. Exit cost, per the metrics document's own reversibility test, is low — leaving this convention means changing how paths are written and processed, not re-authoring content.
- Power-imbalance proxies (exit cost, data portability, ToS volatility, pricing asymmetry): not applicable — there is no vendor↔user relationship here to score. Recorded as N/A rather than rated.

**Interaction Patterns**

- Rating: Strong
- Evidence: Inferred
- Notes: For the content editor (the relevant "user"), native markdown is the honest default — no vendor-serving preselection, no lock-in. The one real cost is a small, disclosed, one-time learning curve for the decorative-attribute syntax, which is a legibility cost, not a dark pattern.

### Sustainability

**Environment**

- Rating: Moderate
- Evidence: Inferred
- Notes: Generating multiple responsive variants at build time (per APU's precedent: WebP and AVIF at several widths) trades a heavier build for a lighter, better-cached payload at request time. The net bandwidth effect is very likely positive but was not measured against a no-variants baseline.

### Security

- Rating: Moderate
- Evidence: Unknown

**Assessment method:** Not tested; no implementation exists. Answered from the design as specified, not from observed behaviour.

**Network behaviour:**

- [ ] Outbound connections during normal use — none introduced by this convention
- [ ] Update checks — none
- [ ] Telemetry or usage data — none
- [ ] Licence server contact — none
- [x] Affects networking or items created that will be transferred over a network — yes, the generated responsive variants are what gets served
- [ ] Helps ensure that documents passed via network are clean and compliant — not applicable

**File system behaviour:**

- [x] Creates hidden or metadata files alongside content — generated srcset/dimension manifests, per APU's precedent, are build artifacts, not hand-edited
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**

- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

### The Product or Service Itself

**Longevity**

- Rating: Strong
- Evidence: Verified
- Notes: An internal convention, not a third-party dependency with its own funding model or contributor pool to worry about — its longevity is entirely this project's own to maintain. The primitives it rests on (CommonMark, Pandoc-style attributes) are broadly established, stable specifications.

**Content endurance**

- Rating: Strong
- Evidence: Verified
- Notes: Content is authored directly as markdown, not merely enhanced by a tool that could disappear. Per the metrics document's own container-versus-enhancement-layer distinction, removing any part of the build pipeline leaves the markdown, and the images it references, fully intact.

**Exit and portability [GATE]**

- Rating: Strong
- Evidence: Verified
- Notes: Gate passes. Markdown with standard image syntax is portable to essentially any static site generator or markdown renderer; the media pool is just a directory of ordinary image files.

**Adjustability and support**

- Rating: Strong
- Evidence: Verified
- Notes: Fully within this project's own control — no external maintainer to petition, no licence to negotiate. Adjusting it means changing this project's own build scripts.

**Market Position**

- Rating: N/A
- Evidence: N/A
- Notes: Not applicable in the platform↔ecosystem sense this subsection is defined for (market concentration, take rate, API stability of a third-party platform) — this is an internal convention, not infrastructure belonging to an external operator. The nearest real analogue — this project's dependency on Sveltia CMS's own governance and continuity — is a separate, out-of-scope evaluation.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Strong | Inferred | Alt text + decorative sentinel first-class |
| Multilingual integration | Strong | Verified | Falls out of per-locale markdown bodies |
| Economic and cognitive accessibility | Unknown | Unknown | Not evaluated at this pass |
| Representation | Unknown | Unknown | Not determined by an architecture pattern |
| Compatibility | Strong | Inferred | Plain markdown + standard image formats |
| Resilience | Moderate | Inferred | Build depends on an unchosen pipeline |
| Agency, sovereignty & privacy | Strong | Verified | No vendor relationship introduced |
| Agency, power-imbalance proxies | N/A | N/A | No vendor↔user relationship to score |
| Agency, interaction patterns | Strong | Inferred | Honest default; small learning curve |
| Environment, direct and indirect | Moderate | Inferred | Net bandwidth effect likely positive, unmeasured |
| Security | Moderate | Unknown | Untested; no implementation exists |
| Longevity | Strong | Verified | Internal convention, stable primitives |
| Content endurance | Strong | Verified | Content authored in the durable format itself |
| Exit and portability | Strong | Verified | **[GATE] Pass** |
| Adjustability and support | Strong | Verified | Fully within project's own control |
| Market position | N/A | N/A | Not a platform/vendor relationship |
| Gates | **Pass** | — | Exit-and-portability gate passes; accessibility-floor and telemetry gates not yet testable (no implementation) |

## Relationship to project

Content architecture / i18n content pattern. Feeds directly into `overview--ingress-process-v0-1-0.md` §4 (images) once implemented, and into whichever markdown-rendering pipeline this project eventually adopts. Docs area: `architecture/` or `specifications/`.

## Status notes

- Last reviewed: 2026-08-14
- **In assess:** recommended following a four-site survey (`survey--image-handling-sveltia-cms-sites-v0-4-0.md`) and its conclusions (`survey--image-handling-sveltia-cms-sites-conclusions-v0-1-0.md`). No implementation exists. Pending before this can move toward trial: choice of markdown rendering pipeline, a settled syntax for the decorative sentinel, and the build-time `check:*` validators themselves. Per the metrics document's lifecycle guidance, all gate criteria and the Security pillar must carry Verified tags before an item moves to trial — Security here is Moderate/Unknown, so this entry is not yet eligible, regardless of how sound the pattern reads on paper.

## Links

- `docs/en/docs/reference/survey--image-handling-sveltia-cms-sites-v0-1-0.md`
- `docs/en/docs/reference/survey--image-handling-sveltia-cms-sites-conclusions-v0-1-0.md`
- `docs/en/docs/reference/overview--ingress-process-v0-1-0.md`

## License

This document, *Locale-Neutral Image Pool with Native Markdown and a Decorative-Alt Sentinel*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial entry, documenting the image-handling approach recommended in survey--image-handling-sveltia-cms-sites-conclusions-v0.1.0, evaluated against universal-cake-evaluation-metrics-v0.3.1. Recommendation recorded as hold-for-trial, a value outside the template's adopt/hold/rejected/high-risk vocabulary, flagged rather than forced, since Security and the accessibility-floor gate cannot yet carry Verified tags with no implementation to test |

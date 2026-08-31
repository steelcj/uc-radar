# SAT radar entries: CMS accessibility comparison (ATAG vs. WCAG)

This applies Universal Cake Evaluation Metrics v0.5.1 to the four tools with real, checkable accessibility evidence: Craft CMS, Sveltia CMS, Decap CMS (legacy), and WordPress/Gutenberg. It follows the same standalone-scorecard pattern as the OpenTelemetry radar entry, not an edit to the metrics document itself.

## Framework note before scoring

The ATAG/WCAG split you're drawing is correct, and it exposes a real gap in v0.5.1: the Accessibility pillar's "alternative methods of interacting with content" bullet does not currently distinguish authoring-tool accessibility from output accessibility, even though Environment already makes exactly this distinction for a different concern (the v0.5.0 "Authoring-side load" question, split out from output-facing direct/indirect impacts). Accessibility deserves the same split, for the same reason: a CMS's admin panel and a CMS's published output are different artifacts with different accessibility owners, evaluated against different W3C standards (ATAG 2.0 for the former, WCAG for the latter), and a tool can pass one while failing the other. This scorecard splits the Accessibility row into two sub-rows to reflect that, using the row-granularity flexibility v0.4.0 already permits, but the split itself is worth proposing as a permanent addition to the main document rather than something reinvented per evaluation. Flagging it here rather than editing the document, since that edit is your call.

A second, sharper question this evidence raises: **which side of that split does the default accessibility-floor gate apply to?** As written, "content is unreachable for people using assistive technology" reads most naturally as output-side (WCAG), but an editor who cannot operate the admin panel is just as excluded from the product, on the authoring side (ATAG). The Decap entry below is a concrete case where this ambiguity has teeth: its keyboard-inaccessible media picker plausibly fails an authoring-side accessibility floor even though it says nothing about whatever the resulting published site looks like. Worth deciding whether this is one gate with two evaluation surfaces, or two independent gates.

## Scorecard: Craft CMS

- **Product or service name:** Craft CMS
- **Version evaluated:** Current release as of August 2026; Accessibility Conformance Report and ATAG 2.0 report are updated quarterly per vendor
- **Evaluator:** Claude (Anthropic), drafted for Christopher Steel's review, incorporating accessibility research already gathered
- **Date:** 2026-08-21
- **Radar ring:** Assess

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, authoring side (ATAG) | Strong | Claimed (vendor-published, methodology disclosed) | Publishes a formal WCAG 2.1/2.2 AA Accessibility Conformance Report (VPAT-based) and a separate ATAG 2.0 report, updated quarterly, with named methodology (axe-core plus manual testing across NVDA/Firefox, JAWS/Chrome, VoiceOver/Safari). Openly lists what is not yet compliant. Strongest public paper trail found on this list. Rated Claimed rather than Verified since no independent third-party audit was located; the disclosed methodology and quarterly cadence are real credibility signals but do not substitute for outside verification. |
| Accessibility, output side (WCAG) | Not applicable to the CMS itself | N/A | Per your own framing: output accessibility is a template/theme/developer decision the CMS can only constrain or enable, not guarantee. No product-level output claim was found, and none would be meaningful without naming a specific theme. |
| Agency, sovereignty & privacy | Moderate | Inferred | Proprietary, paid license with a capped free tier, not copyleft. Content is exportable via GraphQL/API (documented, Verified in an earlier evaluation pass), which keeps exit realistic even though the license itself is a real cost. |
| Security | Unknown | N/A | Not covered in this pass; carry forward as a gap before Trial. |
| Market position / Longevity | Unknown | N/A | Not covered in this pass; carry forward as a gap before Trial. |
| Gates | Pass (accessibility floor); Unknown (others) | | Authoring-side accessibility floor: pass, on the strength of the disclosed ACR/ATAG evidence, though still Claimed-tag rather than Verified. Telemetry, licence-compatibility, and exit gates not evaluated this pass. |

## Scorecard: Sveltia CMS

- **Product or service name:** Sveltia CMS
- **Version evaluated:** Pre-1.0, as of August 2026
- **Evaluator:** Claude (Anthropic), drafted for Christopher Steel's review, incorporating accessibility research already gathered
- **Date:** 2026-08-21
- **Radar ring:** Assess

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, authoring side (ATAG) | Strong | Verified (independent, non-vendor source) | No formal ACR yet (one is planned pre-1.0, which is itself an honest, checkable commitment rather than a claim of completion). Architecturally: a custom accessible component library rather than a UI kit bolted on, full WAI-ARIA, keyboard-complete editing including drag-and-drop alternatives. Independently vetted by IDRC/FLOE, an accessibility research group, which cites it as best-in-class among static-site CMSes. This upgrades the rating from the Inferred tag given in an earlier evaluation pass: third-party research-group corroboration is stronger evidence than architecture alone, even without a formal conformance document. |
| Accessibility, output side (WCAG) | Not applicable to the CMS itself | N/A | Same reasoning as Craft: this is an authoring layer over a static site; output accessibility depends on the site's own templates. |
| Agency, sovereignty & privacy | Strong | Verified | MIT license, git-native, fully self-hostable; no phone-home behavior found. |
| Agency, power-imbalance proxies (exit cost) | Strong | Inferred | Content lives as markdown/YAML in the owner's own repository; migrating off Sveltia does not require migrating the content. |
| Market position, forkability and contributor concentration | Unknown | N/A | Not established in this pass. Sveltia is maintained by a small team rather than a multi-vendor governance body like CNCF; bus factor specifically was not checked, and is worth checking before Trial given how much weight this scorecard otherwise places on the project. |
| Gates | Pass (accessibility floor, authoring side); Unknown (others) | | Track the pending formal audit as a re-review trigger in its own right: treat "Sveltia publishes its ACR/ATAG report" as an event that should prompt re-review and a rating upgrade from Claimed/Verified-by-proxy to Verified-by-conformance-document, per your note about not calling it UC-conformant on this pillar until that lands. |

## Scorecard: Decap CMS (legacy)

- **Product or service name:** Decap CMS (formerly Netlify CMS)
- **Version evaluated:** Current stable release as of August 2026
- **Evaluator:** Claude (Anthropic), drafted for Christopher Steel's review, incorporating accessibility research already gathered
- **Date:** 2026-08-21
- **Radar ring:** Assess, treated as a migration-source rather than a forward candidate

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, authoring side (ATAG) | Weak | Verified | Multiple open, some years-old GitHub issues: unlabelled checkboxes, a keyboard-inaccessible media picker, and broken focus order. This is the specific, named reason Sveltia exists. |
| Gates, accessibility floor (authoring side) | Likely fail, for at least some workflows | Verified | The keyboard-inaccessible media picker is a plausible authoring-side accessibility-floor failure for a screen-reader- or keyboard-only editor trying to insert media, independent of whatever the resulting output looks like. This is the concrete case referenced in the framework note above: it argues for treating the accessibility floor as checkable on the authoring side on its own terms, not only as a proxy for output quality. |
| Accessibility, output side (WCAG) | Not applicable to the CMS itself | N/A | Same reasoning as Craft and Sveltia. |
| Agency, power-imbalance proxies (exit cost) | Low | Inferred | Content is markdown/YAML in the owner's repository, same format family Sveltia reads; migration to Sveltia is close to a drop-in swap for common configurations. |

**Radar recommendation:** hold at Assess, not for new adoption. Existing installs are re-review candidates with a named migration target (Sveltia) rather than items to re-evaluate on their own trajectory.

## Scorecard: WordPress / Gutenberg

- **Product or service name:** WordPress (core), with the Gutenberg block editor as the default authoring surface
- **Version evaluated:** 7.0.3 (current stable as of August 2026)
- **Evaluator:** Claude (Anthropic), drafted for Christopher Steel's review
- **Date:** 2026-08-21
- **Radar ring:** Assess

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, authoring side (ATAG) | Weak | Verified (independent audit) | A 2019 WPCampus/Tenon audit found "significant and pervasive" problems and called the block editor a regression from the Classic editor. Remediation has continued but is documented as incomplete through 2026, seven years on. Rated Weak rather than Moderate because an unresolved, independently-documented regression of this age against a prior, better baseline is a substantive finding, not a minor gap; this is a judgment call worth revisiting if a more recent independent audit surfaces. |
| Accessibility, output side (WCAG) | Weak to Moderate | Verified (independent, large-scale scan) | WebAIM's 2026 Million-site scan places WordPress pages below Squarespace and Wix on average per-page error count. Per your own framing, this reflects the ecosystem's typical themes and practices more than a floor set by WordPress core itself, since output accessibility here is almost entirely plugin/theme dependent; recorded as a real signal about what the ecosystem in practice produces, not a claim about core software. |
| Security | Moderate | Verified | In July 2026, an unauthenticated, pre-auth remote-code-execution chain ("wp2shell," CVE-2026-63030 and CVE-2026-60137) was disclosed against WordPress core, estimated to be reachable on the roughly 500 million sites running WordPress. The security team shipped patches across three release lines (6.8.6, 6.9.5, 7.0.2) within days and enabled forced automatic security updates for affected installs; a further release (7.0.3) followed in August with twelve additional security fixes. Rated Moderate rather than Strong for the same reason given in the OpenTelemetry scorecard: a fast, transparent disclosure and patch process is a real strength, but it does not cancel out that a vulnerability of this severity and this blast radius existed and was actively exploited. If WordPress were already an adopted item on the radar, this event is exactly what the v0.5.0 "security incident or breach" re-review trigger exists for. |
| Agency, sovereignty & privacy | Strong | Verified | GPLv2-or-later for core; derivative themes and plugins inherit the GPL. Fully self-hostable, no core vendor lock-in, broad and mature export tooling (WXR and others). |
| Market position, contributor concentration | Moderate | Verified | Historically concentrated: for the WordPress 6.6 release, Automattic alone supplied over half of all core contributions. In January 2025, Automattic sharply reduced its sponsored core contributions amid an external legal dispute (the WP Engine conflict), redirecting Automatticians toward Automattic's own commercial products instead. This is a textbook instance of the v0.5.0 Market Position re-review trigger ("a material shift in any Market Position proxy, e.g., a funding round, an acquisition, a governance change"), and it argues for rating this Moderate rather than Strong: the project's own governance already demonstrated that a large share of its contribution base can move in response to one company's external conflicts, which is precisely the kind of single-point-of-influence risk this proxy exists to catch, independent of the project's genuinely large total contributor base. |
| Longevity | Strong | Verified | Over two decades of continuous development, powers a very large share of the web by essentially every independent measure, large and active plugin/theme ecosystem. |
| Gates | Unresolved, needs Verified-tag testing before Trial | Inferred | Accessibility floor (authoring side): the WPCampus/Tenon language ("significant and pervasive") suggests real barriers on core editing tasks for some assistive-technology users, but does not establish outright unreachability; this needs direct testing rather than inference before a Trial-ring pass. Telemetry: WordPress core has some update-check and optional usage-data behavior; whether every channel of it can be fully disabled was not verified in this pass and should be resolved before Trial. Licence-compatibility: pass, GPLv2-or-later is broadly compatible. Exit: pass. |

## Where this leaves "everything else"

For Strapi, Directus, Payload, Ghost, Sanity, Contentful, Statamic, TinaCMS, and the remaining tools on the jamstack.org list: rate Accessibility (both axes) as **Unknown**, not Weak, per the v0.5.1 rating scale's own definition, insufficient information to rate. What resolves it is named directly: a public conformance report, an independent audit, or a credible third-party citation of the kind Sveltia has. Absence of a claim is not itself damning, per your framing, but under an evidence-over-claims standard it also does not earn a Strong or Moderate rating just because nothing negative surfaced either. This is exactly what the Aggregation section is for: an Unknown sitting next to a Strong elsewhere in a tool's scorecard is a specific, named gap to track, not something to average toward the middle.

## Radar recommendation

- **Sveltia CMS**: promote toward Trial for a git-based, editor-facing use case, with the explicit caveat you named, that its formal ACR/ATAG report is still pending. Treat that report's publication as a standing re-review trigger for this item specifically.
- **Craft CMS**: viable Trial candidate where a non-git, editor-heavy option is genuinely needed and the license cost is acceptable; its accessibility paper trail is the strongest on the list, even though the rest of its scorecard (Security, Market Position) needs filling in before a real Trial-gate pass.
- **Decap CMS**: hold, not for new adoption; existing installs are migration candidates to Sveltia, not independent re-review subjects.
- **WordPress/Gutenberg**: stays at Assess. The authoring-side accessibility floor needs direct Verified-tag testing rather than reliance on a 2019 audit, and the July 2026 security incident, however well-handled, is a live re-review-trigger-shaped event worth watching rather than something to treat as closed just because it was patched quickly.

# Jamstack CMS Accessibility Ratings

Five jamstack-leaning CMS platforms rated against the Universal Cake framework, focused on the Accessibility and Agency pillars. Ratings use the framework's scale: **Strong**, **Moderate**, **Weak**, **Unknown**. Evidence tags: **Verified**, **Inferred**, **Claimed**.

## The five

1. **Sveltia CMS** — an independent, from-scratch rewrite that has become the de facto successor to Decap CMS (formerly Netlify CMS). Decap itself is now a largely stagnant rebrand maintained by a Netlify agency partner, so it is treated below as legacy context rather than a live contender.
2. **TinaCMS** — git-backed, visual/inline editing, framework-agnostic.
3. **Craft CMS** — not jamstack by default, but commonly run headless/API-first against static frontends. Included because its accessibility documentation is unusually rigorous.
4. **Statamic** — flat-file (git-friendly) Laravel CMS, self-hosted, with active a11y tracking.
5. **Payload CMS** — TypeScript/React headless CMS and application framework, API-first, MIT-licensed.

## Decap to Sveltia: why this list treats them as one lineage

Sveltia was not built by forking Decap's code. It is a ground-up rewrite in Svelte, started in November 2022, that predates Decap's own February 2023 rebrand announcement. Its own documentation frames the split explicitly: Decap is the "official" successor to Netlify CMS by name, but Sveltia has become the de facto successor by activity, having resolved several hundred issues that sat open in the predecessor's tracker. In most casual setups, an existing Decap config file can be reused with a one-line change, which keeps exit cost low for anyone migrating.

Practically, this means: new jamstack builds should default to Sveltia, and existing Decap installs should be read as migration candidates rather than a stable long-term choice.

## Accessibility, alternative interaction methods [GATE]

| CMS | Rating | Evidence | Notes |
| --- | --- | --- | --- |
| Sveltia CMS | Strong | Inferred | Built with full keyboard operability, WAI-ARIA screen reader support, and WCAG 2.2 conformance as explicit design goals, including respecting OS-level reduced-motion settings. No independent third-party audit confirmed; verify before a Trial-gate adoption. |
| Decap CMS (legacy) | Weak | Verified | A long-open, still-cited issue documents that buttons, checkboxes, and edit fields lack accessible labels for screen readers, and that menus do not receive keyboard focus on open. Filed in 2020, unresolved as of the most recent references found. |
| TinaCMS | Weak | Verified | A user attempting to evaluate Tina for screen-reader-dependent content managers found that form labels lack a programmatic `for`/`id` relationship to their inputs, a basic WCAG 1.3.1 failure, flagged by Chrome DevTools and unresolved in the thread. |
| Craft CMS | Strong | Claimed (vendor-published, methodology disclosed) | Publishes an Accessibility Conformance Report and an ATAG report targeting WCAG 2.2 / ATAG 2.0 AA, tests the full control panel for contrast, supports 200% zoom without functionality loss, and does dedicated screen-reader testing across NVDA/Firefox, JAWS/Chrome, and VoiceOver/Safari. Vendor-authored, not independently audited, hence Claimed rather than Verified. |
| Statamic | Unknown | Inferred | Maintains a public repository dedicated to discussing and tracking control-panel accessibility improvements, which signals active attention but sets no conformance target and reports no completion state. No independent audit found. |
| Payload CMS | Moderate | Verified (self-reported, with a public defect log) | Publishes a dedicated accessibility page stating an active push toward WCAG 2.2 AA, with axe integrated into its end-to-end test suite and custom keyboard/focus utilities. Its own public a11y report lists open issues, including interactive-element nesting violations and undersized touch targets, which is unusually candid but confirms real gaps remain. |

Gate check: none of the six fail outright with content fully unreachable, but Decap and TinaCMS carry Verified, open defects in exactly the areas the framework gates on (screen reader labeling, keyboard focus). Neither should clear the Trial gate on this criterion without a fix or a documented workaround.

## A note on accessibility overlay add-ons

Both Decap and Statamic have third-party "accessibility widget" add-ons on the market claiming to lift WCAG compliance automatically. This lines up with a well-documented industry pattern worth flagging under Interaction Patterns: overlays sit on top of a page attempting to patch issues at render time, and in practice often introduce new barriers such as broken focus order, screen-reader interference, and keyboard traps. Treat reliance on these as Weak under Interaction Patterns, since it substitutes a promise for a verified fix, rather than crediting them toward the Accessibility score itself.

## Exit and portability [GATE], Sovereignty and Privacy

All five active contenders are meaningfully git- or file-based, which is the structural feature that makes jamstack CMS attractive under this framework: content lives as files you control, not locked inside a proprietary database.

| CMS | Exit cost | Data portability | License and self-hosting |
| --- | --- | --- | --- |
| Sveltia CMS | Low — content is markdown/YAML in your own git repo | Strong, human- and machine-readable | MIT, fully self-hostable |
| TinaCMS | Low to moderate — git-backed, but the optional Tina Cloud hosted layer adds a vendor dependency for real-time collaboration | Strong for the git path, weaker if using hosted Tina Cloud | Open core (Apache-2.0); Tina Cloud is the paid layer |
| Craft CMS | Moderate — proprietary license cost to exit, though the content model is exportable via GraphQL/API | Strong (documented API) | Proprietary, paid license per project with a capped free tier; not copyleft |
| Statamic | Moderate — flat-file content is portable, but the Pro tier needed for most real sites is a paid license | Strong (files are yours) | Proprietary dual-license: free Solo tier, paid Pro tier |
| Payload CMS | Low to moderate — content sits in your own MongoDB or Postgres database rather than flat files, so export is a data dump rather than a file copy | Strong via API, but not file-native the way the others are | MIT, fully self-hostable |

Craft and Statamic both introduce pricing asymmetry and licensing costs that the framework flags explicitly under its power-imbalance proxies, worth weighing against their stronger documentation and maturity.

## Bottom line

- Best documented accessibility claim: Craft CMS, with a formal ATAG/WCAG report, though it is vendor-Claimed rather than third-party Verified, and it is proprietary and paid. That is a genuine cross-pillar tension this framework is built to surface: an accessibility win offset by a Sovereignty and Privacy cost.
- Most candid about its own gaps: Payload CMS, which publishes an open defect log against its own accessibility push rather than only marketing claims.
- Best jamstack purity plus accessibility intent: Sveltia CMS. Free, MIT-licensed, git-native, and the only flat-file option built with accessibility as a stated architectural goal from day one, though it lacks Craft's or Payload's formal audit trail.
- Should not clear a Trial gate as-is: Decap CMS and TinaCMS, both carrying open, Verified accessibility defects in exactly the areas this framework gates on.
- Genuinely Unknown, not Weak: Statamic. The open tracking repository is a positive signal, but there is no conformance target to check it against, so per the framework's own instructions this stays Unknown rather than being scored down.

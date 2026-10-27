# Changelog

All notable changes to the uc-radar repository are recorded here. This is the repository-level changelog: it records what each tagged release contained and why. Each document additionally carries its own changelog, in its frontmatter and in a Changelog table, for changes internal to that document.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions track the `VERSION` file and the git tags. Dates are ISO 8601.

## [Unreleased]

## [0.3.0] - 2026-10-27

### Added

- *Universal Cake Evaluation: mise-en-place* 0.1.1 (provisional): what running its conda backend on git showed, where mise acts beyond user space, and uc-automa 0034's use of mise. Ring unchanged, Assess.
- A `trial/` ring, for optional real use in a limited, named scope, with its README and the *UC Radar Telemetry Trace Procedure* 0.1.1 (draft), with 0.1.0 beside it and a copy under `assess/telemetry/`
- *UC Radar Evaluation Lifecycle* 0.2.0, *UC Radar Entry Template* 0.3.0 and *Universal Cake Evaluation Metrics* 0.6.0; the versions they replace, 0.1.0, 0.2.0 and 0.5.1, move to `radar/artifacts/`
- Assess entries: Emacspeak, Jan, SimpleLogin, Fileverse dDocs, xkcdpass, OpenSnitch, OpenTelemetry 0.2.0 and 0.2.1, a production-ready droplet for multi-site Caddy and Eleventy, and two on Windows Recall
- Personal FOSS stack notes under `assess/` for AI, browsers, documents, messengers, music, desktop operating systems, storage, VPNs and wallets, and an Incus Ansible role note
- Two metrics examples under `en/docs/guides/metrics/examples/`

### Changed

- *Git on Windows, Options for osat-manager-git* 0.2.0 (provisional): conda-forge's git is now the recommended source for osat-manager-git on Windows, Linux and macOS, following the 2026-10-21 decision and osat-manager-git decision 0001. MinGit moves to Assess as the fallback; dugite-native moves to Hold.
- The radar README describes four rings, assess, trial, adopt and hold, for Universal Cake and its projects; the assess README sets out testing in assess and the criteria for leaving it
- Incus entries move to `assess/containers/incus/`

## [0.2.4] - 2026-09-29

- Added a number of assessments

## [0.2.3] - 2026-08-31

### Added

```bash
	modified:   .gitignore
	new file:   en/docs/automa/versioning/README.md
	new file:   en/docs/automa/versioning/defaults/versioning-postfix-rules-v0-1-0.md
	renamed:    en/docs/radar/adopt/accessibility/universal-cake-evaluation--infusion--accessibility-bar-v0.1.0.md -> en/docs/radar/adopt/accessibility/universal-cake-evaluation--infusion--accessibility-bar-v0-1-0.md
	renamed:    en/docs/radar/adopt/atproto/atproto--universal-cake-praxis-evaluation-v0.1.0.md -> en/docs/radar/adopt/atproto/atproto--universal-cake-praxis-evaluation-v0-1-0.md
	renamed:    en/docs/radar/adopt/iac/shims--options-for-provider-agnostic-approaches-to-provisioning--v0.1.0.md -> en/docs/radar/adopt/iac/shims--options-for-provider-agnostic-approaches-to-provisioning-v0-1-0.md
	renamed:    en/docs/radar/adopt/iac/shims--options-for-provider-agnostic-approaches-to-provisioning--v0.1.1.md -> en/docs/radar/adopt/iac/shims--options-for-provider-agnostic-approaches-to-provisioning-v0-1-1.md
	renamed:    en/docs/radar/adopt/iam/opensource-identity-systems--iam--radar-entry-v0.1.0.md -> en/docs/radar/adopt/iam/opensource-identity-systems--iam--radar-entry-v0-1-0.md
	renamed:    en/docs/radar/adopt/iam/opensource-identity-systems--iam--radar-entry-v0.1.1.md -> en/docs/radar/adopt/iam/opensource-identity-systems--iam--radar-entry-v0-1-1.md
	renamed:    en/docs/radar/adopt/installers/tool-installation-pattern--versioned-local-binaries-and-wrappers-for-nix-v0.1.0.md -> en/docs/radar/adopt/installers/tool-installation-pattern--versioned-local-binaries-and-wrappers-for-nix-v0-1-0.md
	renamed:    en/docs/radar/adopt/kanban/uc--kanban-setup-v0.1.0.md -> en/docs/radar/adopt/kanban/uc--kanban-setup-v0-1-0.md
	renamed:    en/docs/radar/adopt/kanban/wekan--accessible-kanban-board-v0.2.0.md -> en/docs/radar/adopt/kanban/wekan--accessible-kanban-board-v0-2-0.md
	renamed:    en/docs/radar/adopt/linux/server-os--linux-server-operating-system--radar-entry-v0.1.0.md -> en/docs/radar/adopt/linux/server-os--linux-server-operating-system--radar-entry-v0-1-0.md
	renamed:    en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0.1.1.md -> en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0-1-1.md
	renamed:    en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0.1.2.md -> en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0-1-2.md
	renamed:    en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0.2.0.md -> en/docs/radar/adopt/metadata/1b-sat-dublin-core-metadata-usage-current-state-and-recommended-improvements-v0-2-0.md
	renamed:    en/docs/radar/adopt/metadata/dublin-core-metadata-usage-in-sat-v0.1.2.md -> en/docs/radar/adopt/metadata/dublin-core-metadata-usage-in-sat-v0-1-2.md
	new file:   en/docs/radar/adopt/metadata/metadata-dublinc-core-dcmes-1-1-was-frozen-at-15-elements.md
	renamed:    en/docs/radar/adopt/proxies-and-reverse-proxies/reverse-proxies--options-for-http-routing-and-tls-termination--v0.1.0.md -> en/docs/radar/adopt/proxies-and-reverse-proxies/reverse-proxies--options-for-http-routing-and-tls-termination-v0-1-0.md
	new file:   en/docs/radar/adopt/sveltia-cms/uc-radar--markdown-image-pool--content-architecture-v0-1-0.md
	renamed:    en/docs/radar/adopt/webservers/webservers--options-for-static-site-hosting--v0.1.0.md -> en/docs/radar/adopt/webservers/webservers--options-for-static-site-hosting-v0-1-0.md
	renamed:    en/docs/radar/assess/accessibility/built-in-screen-readers-v0.1.0.md -> en/docs/radar/assess/accessibility/built-in-screen-readers-v0-1-0.md
	new file:   en/docs/radar/assess/accessibility/cms-accessibility-scorecards-2026-08-21.md
	renamed:    en/docs/radar/assess/accessibility/fluid-infusion-technical-overview-v0.1.0.md -> en/docs/radar/assess/accessibility/fluid-infusion-technical-overview-v0-1-0.md
	new file:   en/docs/radar/assess/accessibility/jamstack-cms-accessibility-ratings.md
	renamed:    en/docs/radar/assess/libre-screen-readers-talking-browsers-v0.1.0.md -> en/docs/radar/assess/accessibility/screenreaders/libre-screen-readers-talking-browsers-v0-1-0.md
	renamed:    en/docs/radar/assess/screen-readers.md -> en/docs/radar/assess/accessibility/screenreaders/screen-readers.md
	renamed:    en/docs/radar/assess/universal-cake-evaluation--infusion--accessibility-bar-v0.1.0.md -> en/docs/radar/assess/accessibility/universal-cake-evaluation--infusion--accessibility-bar-v0-1-0.md
	renamed:    en/docs/radar/assess/sat-radar-entries--atproto-and-pds-v0.1.0.md -> en/docs/radar/assess/at-protocol/sat-radar-entries--atproto-and-pds-v0-1-0.md
	renamed:    en/docs/radar/assess/static-cms-frontmatter-and-sidecar-metadata-survey.md -> en/docs/radar/assess/cms/static-cms-frontmatter-and-sidecar-metadata-survey.md
	renamed:    en/docs/radar/assess/static-site-generator-frontmatter-and-sidecar-metadata-survey.md -> en/docs/radar/assess/cms/static-site-generator-frontmatter-and-sidecar-metadata-survey.md
	new file:   en/docs/radar/assess/cms/universal-cake-evaluation-sveltia-vs-sveltycms-v0-0-1.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/eu-ai-act-compliance-file-structure-and-roadmap.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/eu-ai-act-compliance-process-for-small-developer-teams-using-ai-internally.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/eu-ai-act-current-compliance-status.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/eu-ai-act-s-transparency-rules-for-using-ai-to-collaboratively-produce-website-content.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/eu-ai-act-transparency-requirements-for-small-development-teams.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase1-risk-assessment/risk-assessment-interview-notes.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase2-legal-review/compliance-scope-memo--eu-ai-act-article-50-phase-2-legal.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase3-technical-audit/eu-ai-act-compliance-process-3-technical-audit.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase4-guidance-engagement/eu-ai-act-compliance-process-4--guidance-engagement.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/phase5-disclosure-infrastructure/eu-ai-act-compliance-process-5-disclosure-infrastructure.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/quarterly-review-template.md
	new file:   en/docs/radar/assess/compliance/eu-ai-act/universal-cake-evaluation--eu-ai-act-article-50--ai-transparency-v0-1-0.md
	renamed:    en/docs/radar/assess/incus--open-source-system-container-and-virtual-machine-manager.md -> en/docs/radar/assess/containers/incus--open-source-system-container-and-virtual-machine-manager.md
	renamed:    en/docs/radar/assess/incus--universal-cake-praxis-evaluation-v0.1.0.md -> en/docs/radar/assess/containers/incus--universal-cake-praxis-evaluation-v0-1-0.md
	renamed:    en/docs/radar/assess/inclusive-design-for-learning--creating-flexible-and-adaptable-content-with-learners.md -> en/docs/radar/assess/design/inclusive-design-for-learning--creating-flexible-and-adaptable-content-with-learners.md
	renamed:    en/docs/radar/assess/design/universal-cake-evaluation--cookiecutter--document-templating-v0.1.0.md -> en/docs/radar/assess/design/universal-cake-evaluation--cookiecutter--document-templating-v0-1-0.md
	renamed:    en/docs/radar/assess/design/universal-cake-evaluation--cruft--document-template-sync-v0.1.0.md -> en/docs/radar/assess/design/universal-cake-evaluation--cruft--document-template-sync-v0-1-0.md
	new file:   en/docs/radar/assess/dual-mode-language-authority--standard-aware-not-standard-dependent-002.md
	new file:   en/docs/radar/assess/licensing/licensing-research-ansible-and-svelte-ecosystem-v0-1-0.md
	renamed:    en/docs/radar/assess/metadata/dcterms-rdf-conformance-cost-analysis-v0.1.0.md -> en/docs/radar/assess/metadata/dcterms-rdf-conformance-cost-analysis-v0-1-0.md
	new file:   en/docs/radar/assess/metadata/publishing-vector--content-conformance-metadata--metadata-v0-1-0.md
	modified:   en/docs/radar/assess/metadata/representing-uuids-with-dublin-core-and-dcterms.md
	renamed:    en/docs/radar/assess/prometheus--universal-cake-praxis-evaluation-v0.1.0.md -> en/docs/radar/assess/monitoring/prometheus--universal-cake-praxis-evaluation-v0-1-0.md
	renamed:    en/docs/radar/text-normalisation-rough-list.md -> en/docs/radar/assess/normalization/text-normalisation-rough-list.md
	renamed:    en/docs/radar/assess/online-editors/euro-office-evaluating-on-incus-v0.1.0.md -> en/docs/radar/assess/online-editors/euro-office-evaluating-on-incus-v0-1-0.md
	renamed:    en/docs/radar/assess/pandoc--universal-document-converter.md -> en/docs/radar/assess/pandoc/pandoc--universal-document-converter.md
	renamed:    en/docs/radar/assess/pybtex-apa7--python-bibtex-processor.md -> en/docs/radar/assess/research/pybtex-apa7--python-bibtex-processor.md
	renamed:    en/docs/radar/assess/zotero-plus-better-bibtex-reference.md -> en/docs/radar/assess/research/zotero-plus-better-bibtex-reference.md
	renamed:    en/docs/radar/assess/snow-special-needs-opportunity-windows--an-overview.md -> en/docs/radar/assess/resources/snow-special-needs-opportunity-windows--an-overview.md
	renamed:    en/docs/radar/assess/snow-special-needs-opportunity-windows-source-and-license-attribution-example.md -> en/docs/radar/assess/resources/snow-special-needs-opportunity-windows-source-and-license-attribution-example.md
	renamed:    en/docs/radar/assess/scancode-toolkit.md -> en/docs/radar/assess/scancode/scancode-toolkit.md
	new file:   en/docs/radar/assess/sustainability/conserving-bandwidth-and-compute-with-claude.md
	deleted:    en/docs/radar/assess/sync/analysis--ansible-builtin-modules-as-file-fairy-feature-candidates-v0-1-0-X.md
	renamed:    en/docs/radar/assess/open-source-file-synchronization-tools.md -> en/docs/radar/assess/sync/open-source-file-synchronization-tools.md
	renamed:    en/docs/radar/assess/uc-file-sync-comparison.md -> en/docs/radar/assess/sync/uc-file-sync-comparison.md
	new file:   en/docs/radar/assess/telemetry/opentelemetry-scorecard-2026-08-21.md
	renamed:    en/docs/radar/assess/fastapi--web-framework.md -> en/docs/radar/assess/web-frameworks/fastapi--web-framework.md
	renamed:    en/docs/radar/assess/webservers/caddy--web-server--manual-installation-and-configuration-on-a-production-server-v0.1.1.md -> en/docs/radar/assess/webservers/caddy--web-server--manual-installation-and-configuration-on-a-production-server-v0-1-1.md
	renamed:    en/docs/radar/assess/webservers/caddy--web-server-radar-entry-v0.1.1-draft.md -> en/docs/radar/assess/webservers/caddy--web-server-radar-entry-v0-1-1.md
	renamed:    en/docs/radar/assess/webservers/caddy--webserver--universal-cake-praxis-evaluation-v0.1.0.md -> en/docs/radar/assess/webservers/caddy--webserver--universal-cake-praxis-evaluation-v0-1-0.md
	renamed:    en/docs/radar/hold/kanban/leantime--cognitively-accessible-project-management-v0.2.0.md -> en/docs/radar/hold/kanban/leantime--cognitively-accessible-project-management-v0-2-0.md
	renamed:    en/docs/radar/prompt--universal-cake-praxis-evaluation-v0.1.1.md -> en/docs/radar/prompt--universal-cake-praxis-evaluation-v0-1-1.md
	renamed:    en/docs/radar/uc-radar--evaluation-lifecycle-v0.1.0.md -> en/docs/radar/uc-radar--evaluation-lifecycle-v0-1-0.md
	renamed:    en/docs/radar/uc-radar-entry-template-v0.2.0.md -> en/docs/radar/uc-radar-entry-template-v0-2-0.md
	renamed:    en/docs/radar/universal-cake-evaluation-metrics-v0.3.1.md -> en/docs/radar/universal-cake-evaluation-metrics-v0-3-1.md
	new file:   en/docs/radar/universal-cake-evaluation-metrics-v0-4-0-draft.md
	new file:   en/docs/radar/universal-cake-evaluation-metrics-v0-5-0-draft.md
	new file:   en/docs/radar/universal-cake-evaluation-metrics-v0-5-1.md
```

## [0.2.2] - 2026-08-10

### Added

* Added new items to assess
* Added new references

## [0.2.1] - 2026-08-03

### Added

* Created .gitignore and added dist

## [0.2.0] - 2026-08-03

### Added

* Shared zone received from sat-doc-automa via file-fairy: the release-ceremony scripts, the devops guides, the markdown and AI-collaboration automa, the license blocks, and the CLAUDE.md signpost block. Synced inventory in .file-fairy-state.yaml.
* Committed file fairy syncs with sat-docs-automa scripts and docs

## [0.1.2] - 2026-08-03

### Added

- Shared zone received from sat-doc-automa via file-fairy: the release-ceremony scripts, the devops guides, the markdown and AI-collaboration automa, the license blocks, and the CLAUDE.md signpost block. Synced inventory in .file-fairy-state.yaml.

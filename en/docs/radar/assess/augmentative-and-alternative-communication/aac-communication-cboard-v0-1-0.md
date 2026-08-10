---
title: "SAT radar: Cboard"
date: 2026-08-06
description: >
  Universal Cake SAT radar entry for Cboard. Adopt. Libre AAC (augmentative
  and alternative communication) system with browser, Android, and iOS
  clients, 40-language support, and thousands of symbols from major AAC
  libraries. Strongest open-source alternative to proprietary symbol-sequencing
  talkers such as Let Me Talk.
subject: augmentative and alternative communication, accessibility, assistive technology, open source
type: radar
ring: adopt
language: en-CA
rights: CC BY 4.0
source: https://github.com/cboard-org/cboard
relation: aac-communication-overview-v0-1-0.md
version: "0.1.0"
---

# SAT radar: Cboard

**Ring:** Adopt
**Category:** Augmentative and alternative communication (AAC)
**Licence:** GPLv3
**Language:** JavaScript / React (web); Flutter (mobile)
**Maintainer:** Cboard org / Assistive Technology LLC and community
**First release:** 2017
**Last release:** actively maintained through 2024–2025
**Platforms:** Browser (any OS), Android, iOS, Windows (via browser/PWA)

---

## What it is

Cboard is a browser-based AAC application that lets people with speech and language impairments — including autism and cerebral palsy — build spoken sentences by arranging symbols on a communication board. It uses the browser's Speech Synthesis API for text-to-speech on the web client and a Flutter text-to-speech package on the native mobile apps. Thousands of symbols are available from established AAC symbol libraries (including ARASAAC and Global Symbols), and boards are fully customisable.

The project is structured as several linked repositories: `cboard` (the web front end), `cboard-mobile` (the Flutter native app for Android and iOS), `cboard-api` (backend persistence), and `cboard-ai-engine` (an optional AI-assisted board-building tool that generates content suggestions and pictograms, calling out to OpenAI's API for text description and ARASAAC/Global Symbols for imagery). The core communication function does not require the AI engine.

A notable deployment: Cboard has been used in UNICEF's "For every child, a voice" project to provide communicators to children who would not otherwise have access to AAC technology.

---

## Universal Cake criteria

| Criterion | Rating | Notes |
|---|---|---|
| Zero cost | Strong | Free, GPLv3, no accounts required for basic use |
| Accessibility / disability | Strong | Purpose-built AAC tool; symbol-based interface is the core accessibility function |
| Device reach | Strong | Browser (universal), native Android and iOS apps |
| Multilingual | Strong | 40 languages via Crowdin community translation; coverage varies by platform |
| Data sovereignty | Partial | Self-hostable (`cboard-api`), but default deployment uses hosted Cboard infrastructure; optional AI engine calls external OpenAI API |
| Low setup burden | Strong | Works directly in-browser with no install; native apps are standard app-store installs |

---

## Strengths

**Genuinely open source.** GPLv3-licensed with public repositories, an active Discord community, and a described project ethos of reducing inequality among people with disabilities. This is a substantive difference from Let Me Talk and most named competitors (Expressia, Avaz, ChatterBoards, Pictalk), which are freemium/proprietary.

**Cross-platform without compromise.** A browser client works on any device with no install, and native Flutter apps cover Android and iOS directly — closing the mobile gap that weakens many libre tools reviewed elsewhere in this radar (compare git-annex, Unison).

**Best-in-class multilingual coverage for this category.** 40 languages managed through Crowdin, with an open call for community proofreading. This is a strong match for UC's multilingual criterion and for bilingual or multilingual deployment contexts.

**Uses established, professionally maintained symbol sets.** Thousands of symbols sourced from major AAC symbol libraries (ARASAAC, Global Symbols, and others depending on fork/build) rather than a closed proprietary set — reduces vendor lock-in on the content layer as well as the software layer.

**Self-hostable backend.** `cboard-api` provides persistence and can be run independently of the hosted service, giving operators a sovereignty path if they choose to deploy it.

**Real-world deployment at scale.** The UNICEF partnership demonstrates the project has moved beyond hobbyist status into humanitarian deployment — a meaningful project-health signal.

---

## Areas needing work

**Default deployment is hosted, not self-hosted.** Most users will use the Cboard-hosted web app and mobile app builds rather than standing up their own instance. Data sovereignty in that default path depends on Cboard org's own hosting and privacy practices, which have not been independently audited here. Operators wanting full sovereignty need to deploy `cboard-api` themselves — a meaningfully higher setup burden than the default path.

**Optional AI engine depends on a proprietary external API.** `cboard-ai-engine` calls OpenAI's API for text generation. This is opt-in and not required for core communication function, but any UC deployment enabling it should disclose the external dependency explicitly to users and caregivers.

**Machine-translated languages need proofreading.** The 40-language claim includes languages that were machine-translated and are flagged by the project itself as needing community proofreading. Quality will vary by language; Canadian French should be spot-checked before relying on it in a UC deployment.

**No independent WCAG conformance report found.** As with most tools in this radar, no formal accessibility audit publication was located during this review. Given that the entire application exists to serve people with communication and often motor or cognitive disabilities, this is a gap worth flagging for direct verification (screen reader behaviour, switch-access compatibility, high-contrast/low-vision support) rather than assuming best practice from good intent alone.

**Platform-dependent language support.** The project itself notes that language availability "varies by platform" across web, Android, and iOS — worth checking against the specific language needed before recommending a specific client.

---

## UC positioning

Cboard is the strongest open-source recommendation in the AAC category reviewed here, and the clearest libre counterpart to Let Me Talk. It clears the bar that most other tools in this radar stumble on — genuine cross-platform reach including iOS, strong multilingual support, and a real open-source licence rather than a freemium proprietary model dressed as "free."

**Recommended path for a UC deployment:**

- Default: point users to the hosted web/mobile clients directly — lowest setup burden, immediate access.
- For sovereignty-sensitive or institutional deployments (school boards, health authorities, NGOs): self-host `cboard-api` and disable or clearly disclose the AI engine's external OpenAI dependency.
- Verify Canadian French translation quality directly before recommending to francophone users, given the project's own machine-translation disclosure.
- Treat the absence of a published accessibility audit as an open item — test with representative assistive technology (switch access, screen reader) before a formal UC "Adopt" designation is finalised for a specific deployment context.

---

## See also

- [Cboard official site](https://cboard.io/)
- [Cboard on GitHub](https://github.com/cboard-org/cboard)
- [Cboard mobile (Flutter)](https://github.com/cboard-org/cboard-mobile)
- [Cboard API](https://github.com/cboard-org/cboard-api)
- [Cboard AI engine](https://github.com/cboard-org/cboard-ai-engine)
- `aac-communication-overview-v0-1-0.md`

---

*SAT radar entry v0.1.0. Reviewed 2026-08-06 against Universal Cake criteria: libre licensing, active maintenance, zero cost to end users, disability accessibility, cross-platform device reach, multilingual UI, and data sovereignty.*

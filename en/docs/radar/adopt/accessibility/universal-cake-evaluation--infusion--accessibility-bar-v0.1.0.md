---
dcterms:title: "Universal Cake Evaluation, Fluid Infusion, Web Framework and Accessibility Bar"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "evaluation"
  - "accessibility"
  - "inclusive design"
  - "web framework"
dcterms:description: "Best effort evaluation of Fluid Infusion 4.8.0 against the Universal Cake Evaluation Metrics, based on direct inspection of the source repository."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-07-15"
dcterms:modified: "2026-07-15"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: "https://github.com/fluid-project/infusion"
dcterms:relation: "sat-radar--infusion--accessibility-bar-v0.1.0"
dcterms:identifier: "universal-cake-evaluation--infusion--accessibility-bar"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-15"
    author: "Christopher Steel"
    notes: "Initial best effort evaluation, based on universal-cake-evaluation-metrics-v0-1-0 (incomplete draft) and inspection of the Infusion source at commit depth 1 of the main branch."
---

# universal-cake-evaluation--infusion--accessibility-bar-v0.1.0

## About this evaluation

This is a best effort evaluation of Fluid Infusion 4.8.0 against the Universal Cake Evaluation Metrics (draft v0.1.0). Findings marked **verified** come from direct inspection of the cloned source repository on 2026-07-15. Findings marked **inferred** are reasonable conclusions from documentation or architecture that have not been tested hands-on. A runtime assessment on a live deployment remains the prerequisite for the trial ring.

## Inclusive

### Accessibility

**Supports alternative methods of interacting with content: strong, verified.** Increasing access to content is Infusion's core purpose rather than a bolted-on feature. The preferences framework ships enactors and panels for contrast themes, text size, letter spacing, line spacing, captions, table of contents generation, enhanced inputs, and self-voicing (text-to-speech via the browser's own speech synthesis, verified in `src/components/textToSpeech/TextToSpeech.js` and the SelfVoicing enactor). The bundled fonts include Atkinson Hyperlegible and OpenDyslexic, both designed for readers with low vision or dyslexia. On this metric Infusion is likely to increase the number of people who can access content, which is the strongest possible outcome for the primary question this section asks.

**Support for multilingual integration: moderate, verified.** Message bundles exist in `src/framework/preferences/messages/` for English (generic, en_CA, en_US), French, Spanish, Farsi, and Brazilian Portuguese. English and French covers a Montréal-area audience well. The set is small compared to mature localization ecosystems, but the MessageResolver architecture means adding a locale is a matter of supplying a JSON bundle, not modifying code. A Localization panel and enactor also exist, letting end users switch language from the accessibility bar itself where the host site supports it.

**Compatibility: moderate to strong, partially verified.** Infusion is plain JavaScript, HTML, and CSS running in the browser, so it is agnostic to the visitor's operating system and hardware. It targets mainstream browsers (the test suite runs against Chrome and Firefox). Input device compatibility is a design goal, components follow keyboard accessibility patterns and there is jQuery UI Touch Punch support for touch input. Output device compatibility includes screen readers (ARIA practices throughout the components) and speech output via the TTS component. The main compatibility caveat is the jQuery/jQuery UI foundation, older browsers are well served, but integration into a modern component-based front end (React, Vue, Svelte) is awkward. For a documentation or archive site rendered as classic HTML, this is a non-issue.

### Resilience

**When it goes out of date: moderate to strong, verified with caveats.** Infusion has no external runtime services, the built bundle is self-contained static JavaScript and CSS (a single npm runtime dependency, `fluid-resolve`, and vendored libraries in `src/lib`). A site built with Infusion 4.8.0 will keep working as long as browsers keep running the JavaScript it was built with, there is no license server, no phone-home, no expiring token. The realistic decay path is browser evolution slowly breaking old jQuery-era code, which is a years-long horizon, not a subscription-style cliff.

**During power outages: not applicable in the usual sense.** Infusion runs client-side from whatever serves the page. It adds no additional infrastructure that could fail, if the site is up, the accessibility bar is up.

**During network outages: strong, verified.** Once a page has loaded, all preference application happens locally in the browser. Preferences are persisted in a first-party cookie (verified in `src/framework/preferences/js/Store.js`), so a returning visitor's settings survive without any network round trip to a preference service.

### Sovereignty & Privacy

**Strong, verified.** This is one of Infusion's best results. A grep of the framework source for remote endpoints found no telemetry, no analytics beacons, no update checks, and no third-party service URLs, the only URLs in the code are documentation references in comments. User preferences are stored in a first-party cookie on the user's own machine, under the site owner's domain, not in any cloud service. The dual license (New BSD or Educational Community License 2.0, recipient's choice, verified in `Infusion-LICENSE.txt` and `package.json`) is permissive, the tech owner can modify, self-host, fork, and redistribute without seeking permission. Both owner and user sovereignty increase relative to hosted accessibility overlay services (which typically inject third-party scripts and route preference data through a vendor).

## Sustainability & Security

### Environment

**Direct impacts, on my system: low, inferred.** The UIO custom build is a static bundle. Applying preferences is CSS class manipulation and DOM adjustment, cheap operations. Text-to-speech uses the browser's built-in engine rather than a cloud API, so speech costs no network energy. No background processes, no polling.

**Direct impacts, on the service provider's system: low, verified in architecture.** Serving Infusion is serving static files. There is no server-side compute component at all, which compares favourably with overlay services that proxy every page view through their infrastructure.

**Indirect impacts, hardware: none, inferred.** Runs on any hardware that runs a mainstream browser, including old machines. If anything, features like adjustable text and self-voicing extend the useful life of whatever devices visitors already own. No upgrades required for host or users.

**Indirect impacts, bandwidth: low to moderate, partially verified.** The source tree is about 3 MB, but that includes demos, tests, and all components. A custom UIO-only build (`npm run build:pkg:custom -- -i "fluid-ui-options" -n uio`) produces a much smaller payload, and it is served once and cached. Heavier than hand-rolled CSS toggles, far lighter than a framework-of-the-month single page application.

### The Product or Service Itself

**Longer-term support: moderate, the honest weak point.** The project has institutional backing (Inclusive Design Research Centre at OCAD University) and a nearly two-decade history, which counts for durability. Against that, the latest release (4.8.0) is from January 2025, the contributor pool is small, and funding is grant-based. The repository shows continued commit activity in 2026, so the project is alive, but release cadence is slow. Rating this "supported" is fair, rating it "vigorously developed" would not be.

**Would content behind the interface endure: strong, verified in architecture.** This metric matters most for an archive, and Infusion scores well on it. The accessibility bar is an enhancement layered over ordinary HTML content, it does not become the content's container. If Infusion were removed, phased out, or replaced, the underlying documents would remain fully readable, visitors would lose the preference controls, nothing more. Stored preferences live in cookies that simply become inert. Contrast this with platforms where content is authored inside the tool and dies with it, Infusion creates no such hostage situation.

**Could I get support adjusting it: moderate to good.** Three factors work in your favour: the permissive license means you can change anything without permission, the codebase is readable unminified JavaScript with source maps and documentation at docs.fluidproject.org, and the community runs public channels (Matrix chat, mailing lists, issue tracker) operated by people whose stated mission is inclusion. The small community means slower response than a giant project, but also means maintainers are reachable humans rather than a ticket queue. Worst case, the fork-and-fix path is fully open.

## Summary

| Metric area | Rating | Basis |
|-------------|--------|-------|
| Accessibility, alternative interaction | Strong | Verified |
| Multilingual integration | Moderate (en, fr, es, fa, pt_BR) | Verified |
| Compatibility | Moderate to strong | Partially verified |
| Resilience | Strong | Verified |
| Sovereignty & privacy | Strong | Verified |
| Environment, direct and indirect | Low impact | Inferred and verified |
| Long-term support | Moderate | Verified |
| Content endurance | Strong | Verified |
| Adjustability and support | Moderate to good | Verified |

Overall, Infusion aligns unusually well with the Universal Cake values, particularly on sovereignty (no telemetry, local-only preference storage, permissive dual license) and content endurance (enhancement layer, not a container). The trade-off accepted in exchange is a slow-moving, small-community project built on an aging but stable jQuery foundation. For an archive and documentation context that prizes durability over novelty, that trade-off reads as acceptable, pending the runtime security assessment already flagged in the radar entry.

## License

This document, *Universal Cake Evaluation, Fluid Infusion, Web Framework and Accessibility Bar*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

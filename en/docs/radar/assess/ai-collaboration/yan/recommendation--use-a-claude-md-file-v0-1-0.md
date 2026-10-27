---
dc:title: "Universal Cake Evaluation: Jan, Local AI Assistant"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "jan"
  - "local ai"
  - "llm"
  - "llama.cpp"
  - "mcp"
dc:description: "Universal Cake evaluation of Jan 0.8.7, an Apache-2.0 desktop application from Menlo Research for running language models locally or through cloud providers, with an OpenAI-compatible local API server and MCP tool support."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-26"
dcterms:modified: "2026-10-26"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/janhq/jan"
dc:relation: "universal-cake-evaluation-metrics"
dc:identifier: "universal-cake-evaluation--jan--local-ai-assistant"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/local-ai/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-26"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of Jan 0.8.7 against Universal Cake
      Evaluation Metrics 0.5.1, from the repository source at main
      9c20454 (2026-10-26), its git history for the year to 2026-10-26,
      its security policy, its privacy policy, the public record for
      CVE-2026-66005, and a Menlo Research planning note. Jan was not
      installed or run.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # a workstation trial is reasonable under the conditions in the entry; not yet for clients
      reasoning: "Apache-2.0, local by default, and a strong privacy design: product analytics are opt-in, default off, ask on first run with an equal Deny button, and are absent entirely from builds made without the PostHog secrets. The hourly update check can be switched off, and updates are minisign-signed. Conversations are stored as plain JSON and JSONL files, models as standard GGUF files, and the model behind any chat can be swapped for any local or OpenAI-compatible one, so lock-in is low. Against that: a large and fast-moving dependency tree, model downloads that follow a moving Hugging Face branch and skip verification when no hash is available, default MCP entries that run unpinned npx packages when switched on, a medium CORS CVE in the local API server fixed only in July 2026, accessibility that is plausible from the source but untested, hardware needs that exclude older machines for local models, and a steward that has said it plans to spin Jan off into a new company."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-26T12:30:00-04:00"
  human: []
---

# Universal Cake Evaluation: Jan, Local AI Assistant

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

Jan is a desktop application for Windows, macOS and Linux that downloads and runs large language models on the user's own machine, through a bundled llama.cpp engine (and MLX on Apple silicon), or connects to cloud providers such as OpenAI, Anthropic and Mistral with the user's own keys. It also serves an OpenAI-compatible API on the local machine and can call tools through the Model Context Protocol (MCP). This entry evaluates release 0.8.7 as a candidate local assistant for workstations and, later, for clients who need AI help without sending their content to a vendor. It passes all four default gates on available evidence. Its privacy design is among the best in its class. Its risks sit in the supply chain (models, MCP servers and a large dependency tree), in an accessibility record that is unverified, in hardware demands, and in a planned change of ownership. We recommend keeping it in Assess, with a workstation trial under the conditions below.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a id="cite-uc-metrics"></a>([Steel, 2026](#ref-uc-metrics)). Facts come from the Jan repository at commit `9c20454` on main, 2026-10-26, and the tag v0.8.7, 2026-10-19 <a id="cite-jan-repo"></a>([Jan Project, 2026](#ref-jan-repo)); its `SECURITY.md` <a id="cite-jan-security"></a>([Jan Project, n.d.-b](#ref-jan-security)); its privacy policy <a id="cite-jan-privacy"></a>([Jan Project, n.d.-a](#ref-jan-privacy)); the public CVE record for CVE-2026-66005 <a id="cite-cve"></a>([CVE.report, 2026](#ref-cve)); and a Menlo Research planning note on Jan's future <a id="cite-menlo-note"></a>([Menlo Research, 2026](#ref-menlo-note)). Source facts were read on 2026-10-26 and are tagged Verified where the code was read directly. Jan was not installed or run, so nothing about runtime behaviour, network contact or assistive technology support is Verified. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

Jan desktop 0.8.7, used with local models. Cloud providers are in scope only where they change a rating, since using one moves the user's messages to that provider under its own terms. The mobile builds (Android and iOS, which store threads in SQLite rather than files), the `jan-agent` command-line agent and the agent development kit (`adk/`) in the same repository are out of scope.

## Scorecard metadata

- Product or service name: Jan
- Version evaluated: 0.8.7 (tag of 2026-10-19); repository `janhq/jan`, main at `9c20454`, 2026-10-26
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-26
- Radar ring: Assess
- Test platform: none; source, git history and public documents only

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Verified (source), not traced | Product analytics (PostHog) are off by default (`productAnalytic: false`, `opt_out_capturing_by_default: true`), asked on first run with Deny and Allow, and switchable in Settings, Privacy. Autocapture, page views and session recording are disabled, and URL and host properties are stripped. Builds without `POSTHOG_KEY` and `POSTHOG_HOST` never start PostHog. The hourly update check is on by default and can be switched off in Settings, General. Network contact was not traced |
| A licence incompatible with the project | Pass | Verified | Apache-2.0, with a Menlo Research copyright notice and a request, not a requirement, for attribution in user-facing materials. Compatible with AGPL-3.0-or-later projects as a separate application |
| Failure of the accessibility floor | Pass | Inferred | Built on Radix UI primitives, with 67 `aria-label`s, a `role="log"` conversation container, `role="status"` notices and adjustable text size and message zoom. Not tested with a screen reader on any platform |
| No exit | Pass | Verified (source) | On desktop, each thread is a folder with `thread.json` and `messages.jsonl`; models are standard GGUF or safetensors files. No in-app export was found, but the data is already in open, machine-readable formats on disk |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Accessible component primitives and labelled controls, a contributor guide that asks for screen reader testing, but no accessibility statement, no VPAT and no record of testing. Tauri renders in each platform's web view, so screen reader support on Linux depends on WebKitGTK, which is the weakest of the three |
| Multilingual integration | Strong | Verified | 17 interface languages, among them Hindi, Indonesian, Vietnamese, Korean and two Chinese scripts. Each language is a folder of plain JSON files anyone can supply. Coverage ranges from 58% (913 of 1,585 keys) to 75% of the English strings, so untranslated text appears. French is at 60%, which matters for Quebec |
| Economic and cognitive accessibility | Moderate | Inferred | Free, no account. A setup wizard and plain consent wording. But the application has grown into model hub, chat, assistants, agents, MCP tools, a local API server and a system monitor, which is a lot to learn, and choosing a model that fits a machine still takes knowledge |
| Hardware and connectivity accessibility | Weak | Claimed | The project's own guide asks for 8 GB of RAM for 3B models and 16 GB for 7B; macOS 13.6 or later, Windows 10 or later. Linux arm64 needs a manual build. Model downloads are gigabytes. Once a model is local, chat works fully offline. Cloud providers lower the hardware bar but bring back the network, cost and privacy trade-offs |
| Representation | Unknown | None | No public information on the design team or on user research. For the AI proxies, representation depends on the model the user picks; Jan ships none by default. Its own Jan models are fine-tunes whose training data disclosure was not reviewed here |
| Compatibility | Strong | Verified (README and build workflows) | Windows x64 (and an arm64 build workflow), macOS universal, Linux deb, AppImage and Flathub, plus the Microsoft Store. Engine builds for CPU, Vulkan, Metal, CUDA 12 and 13, and ROCm. Any GGUF model and any OpenAI-compatible endpoint |
| Resilience | Strong | Inferred | Local models need no live service, licence server or account. A failed update check does not stop the application. Cloud providers and MCP servers that fetch the web degrade when offline, as expected |
| Agency, sovereignty and privacy | Strong | Verified (source), Claimed (policy) | Data rests on the user's machine. Analytics are opt-in with a random ID; the privacy policy says no conversations, files or prompts are collected and that PostHog processes data in the EU. The owner can build from source without analytics. Model transparency, consent and provenance depend on the model chosen; with open-weight local models they are as good as that model's release |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | Exit cost is low: copy the data folder and the model files. Portability: machine-readable Yes (JSON, JSONL, GGUF); human-readable No, there is no Markdown or text export, and reading a JSONL thread needs a tool. No terms of service for the application itself; the privacy policy was last updated 2025-01-16. No pricing; cloud providers bill directly |
| Agency, interaction patterns | Moderate | Verified (source) | Supportive: consent asks plainly, no is one click, tool calls need approval per thread, per server or globally, and blanket approval is off by default. Against: the consent prompt styles Allow as the primary button and Deny as a ghost button, a mild nudge. The update check is on by default and hourly |
| Environment, direct, indirect, and authoring-side | Moderate | Inferred | Local inference uses the user's CPU or GPU heavily while it runs, but no data centre. Larger models push users toward new hardware. Jan trains its own models (training energy not disclosed); the application does not train anything on the user's machine |
| Security | Moderate | Verified, Inferred | Strengths: a private reporting route through GitHub Security Advisories, acknowledgement within three business days, minisign-signed updates, llama.cpp pinned to tag `b11669` and a commit, a local API server bound to `127.0.0.1` by default, and tool approval by default. Weaknesses: only the latest release is supported; CVE-2026-66005 (CVSS v3.1 6.3) let any web origin reach the unauthenticated API when bound to `0.0.0.0`, fixed in 0.8.5; the API key is empty by default; about 1,960 npm and 974 Rust packages are locked; see Findings for models and MCP |
| Longevity | Moderate | Verified, Claimed | Very active: 2,755 commits from 97 author addresses in the year to 2026-10-26, and releases 0.7.4 to 0.8.7 over eleven months. But Menlo Research has written that it plans to spin Jan off into a new company and focus on robotics, with a raise in its pipeline (Claimed). Version numbers are still 0.x |
| Content endurance | Strong | Inferred | Conversations are files in open formats; models are portable files usable by llama.cpp, Ollama, LM Studio and others. Removing Jan removes the interface, not the content |
| Exit and portability | Strong | Verified (source) | As the gate row. A successor can read the files directly or through a short script |
| Adjustability and support | Moderate | Inferred | Apache-2.0, a readable TypeScript and Rust codebase with tests, an extension mechanism and public issues. Support is Discord and GitHub. The codebase is large and changes fast, so a private fork would be costly to keep current |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | One of several local AI desktop applications (with LM Studio, Ollama's application and GPT4All). No take rate. Foundation model dependency: none, models swap freely. Contributor concentration is high: 38% of commits in the year came from `@menlo.ai` or `@jan.ai` addresses and the six most active authors made 81%. Forkable by licence; governance is one company |
| Gates | Pass | Mixed | All four pass; licence and exit Verified, telemetry Verified from source but not traced, accessibility Inferred |

Row changes from the template: Hardware and connectivity accessibility is split out of Economic and cognitive accessibility, because it is the row where Jan diverges most from the rest of its profile.

## Findings

### Privacy is designed in, not added on

Everything that matters for the telemetry gate is visible in a few files. The analytics setting starts false, PostHog starts opted out, and capture is turned on only after the user says yes, either in the setup wizard or in Settings, Privacy. With analytics on, nothing is sent automatically except identification and the application version: there are no explicit `capture` calls in the web application, and autocapture, page views and session recording are off. PostHog's key and host are injected only by Jan's own release workflows, so a build from source has no analytics at all. The privacy policy matches this and adds that data goes to PostHog in the EU and is kept up to twelve months <a id="cite-jan-privacy-2"></a>([Jan Project, n.d.-a](#ref-jan-privacy)). The remaining contact is the update check, which asks `apps.jan.ai` and then GitHub every hour and can be switched off.

### Model downloads follow a moving branch

Downloads from the model hub use Hugging Face `resolve/main` URLs, that is, whatever is on the repository's main branch at the time, not a pinned commit. The expected SHA-256 and size come from the Hugging Face API in the same moment, so they confirm that the download is intact, not that it is the file someone reviewed. When neither a hash nor a size is available, `validate_downloaded_file` in `src-tauri/src/core/downloads/helpers.rs` logs a debug message and accepts the file. For a workstation this is a familiar risk; for client deployments, models should be downloaded once, hashed, and installed from a local copy.

### MCP defaults run unpinned code when switched on

The default MCP configuration lists servers such as `search-mcp-server@latest` and `@modelcontextprotocol/server-filesystem`, launched with `npx -y` or `uvx`. All are inactive until the user switches them on, and tool calls need approval. But once active, each start can fetch and run the newest version of a third-party package. Pinning versions in `mcp_config.json`, or running MCP servers from a reviewed local install, closes this.

### The local API server

The OpenAI-compatible server binds to `127.0.0.1` by default, and an API key can be set but is empty by default. CVE-2026-66005 showed what happens when it is opened to the network: through 0.8.4 the trusted-hosts list was replaced by a wildcard origin when bound to `0.0.0.0`, so a web page or a device on the local network could run inference, list models and invoke MCP tools <a id="cite-cve-2"></a>([CVE.report, 2026](#ref-cve)). The fix, commit `3e1c1e7` of 2026-07-24, is in 0.8.5 and later, including 0.8.7. Anyone using the server should keep it on `127.0.0.1` or set an API key and a trusted-hosts list.

### Hardware and the population Vishpala serves

Jan is at its best on a recent machine with 16 GB of memory or a capable GPU. That is the tension for older and lower-end hardware: the private, offline path is the one that needs the most hardware, and the path that runs anywhere sends content to a cloud provider. A shared machine on the local network, running Jan's API server with a key, would let older clients use a local model, but it brings back the attack surface described above.

### Stewardship

A Menlo Research planning note from June 2026 describes spinning Jan off into its own company, with Menlo keeping a stake and turning to robotics, and a planned pivot toward a command-line agent with an optional desktop interface <a id="cite-menlo-note-2"></a>([Menlo Research, 2026](#ref-menlo-note)). The repository's new `jan-agent.sh`, `adk/` and agent work are consistent with that. Whether the spin-off has happened was not confirmed. Either way it is a change of ownership and funding, which is a re-review trigger, and a new company with outside investors may want revenue from hosted services. Jan already lists Tokamak, which the same note describes as Jan's orchestration and token governance tool, among its built-in agent providers at `api.tokamak.sh`. It is optional and needs a key.

## Recommendation

Keep Jan in Assess.

- **For a workstation trial**, reasonable now: install 0.8.7 or later from GitHub Releases or Flathub, leave analytics off, decide whether to keep the update check, keep the API server on `127.0.0.1` or set a key, pin any MCP server you enable, and download models from known publishers.
- **To move to Trial**, verify the gates: trace network contact on a clean test machine with analytics off and on, test the main chat and settings flows with a screen reader (Orca on Linux, NVDA on Windows, VoiceOver on macOS) and keyboard only, and confirm that a thread folder can be read and moved without Jan.
- **For clients**, not yet: wait for the trial results, for the stewardship question to settle, and for a pinned way to supply models.

Re-review triggers: the spin-off completing or any other change of ownership or funding, analytics or update behaviour changing, a hosted Jan service becoming a default, model downloads pinned to commits or refusing unverified files, a second security advisory, or a 1.0 release.

## Relationship to project

Local AI. Jan is the first local AI assistant on the radar. It relates to the EU AI Act Article 50 transparency work in `assess/compliance/eu-ai-act/`, since content drafted with it would carry the same disclosure duties, and to the hardware and low-bandwidth aims behind Vishpala's client work.

## Status notes

- Last reviewed: 2026-10-26
- In assess: an automated pass from source, git history and public documents. Jan was not installed or run. A trial would install it on a clean test machine, trace its network contact, test it with assistive technology, and export and move one thread by hand.

## License

This document, *Universal Cake Evaluation: Jan, Local AI Assistant*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-uc-metrics)

### Jan

- [Repository](#ref-jan-repo)
- [Security policy](#ref-jan-security)
- [Privacy policy](#ref-jan-privacy)
- [CVE-2026-66005](#ref-cve)
- [Menlo Research planning note](#ref-menlo-note)

## References

<a id="ref-cve"></a>CVE.report. (2026). *CVE-2026-66005: Jan local API server CORS origin reflection via 0.0.0.0 binding*. Retrieved October 26, 2026, from https://cve.report/CVE-2026-66005
[Return to citation](#cite-cve)

<a id="ref-jan-privacy"></a>Jan Project. (n.d.-a). *Privacy policy*. Jan documentation. Retrieved October 26, 2026, from https://docs.jan.ai/desktop/privacy-policy
[Return to citation](#cite-jan-privacy)

<a id="ref-jan-security"></a>Jan Project. (n.d.-b). *SECURITY.md*. GitHub. Retrieved October 26, 2026, from https://github.com/janhq/jan/blob/main/SECURITY.md
[Return to citation](#cite-jan-security)

<a id="ref-jan-repo"></a>Jan Project. (2026). *Jan* (Version 0.8.7) [Computer software]. GitHub. https://github.com/janhq/jan
[Return to citation](#cite-jan-repo)

<a id="ref-menlo-note"></a>Menlo Research. (2026, June). *Jan, June 2026* [Planning note]. HackMD. Retrieved October 26, 2026, from https://hackmd.io/@menlo/jan-jun-2026.md
[Return to citation](#cite-menlo-note)

<a id="ref-uc-metrics"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-uc-metrics)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of Jan 0.8.7 from source, git history and public documents. Automated recommendation: Assess; workstation trial under conditions; not yet for clients |

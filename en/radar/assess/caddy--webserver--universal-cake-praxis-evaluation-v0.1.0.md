# Caddy--Universal Cake Praxis Evaluation

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md

---

## Abstract

This document is a Universal Cake praxis evaluation of Caddy (version 2.11.2) assessed as a candidate replacement for Apache2 in the Hugo hosting stack — a sovereign, operator-controlled infrastructure for serving pre-built static sites and statically generated webstores over HTTPS. The evaluation applies the eight dimensions defined in the <a name="apa-evaluator-citation"></a>[Universal Cake praxis evaluator (Steel, 2026a)](#apa-evaluator-reference) to Caddy in this specific deployment context, not to Caddy in the abstract. The primary source material is the Caddy radar entry for the Hugo hosting stack (Steel, 2026e), supplemented by direct assessment of the Caddy documentation site, the Caddy website source repository, and the Caddy 2.11.2 release notes. The evaluation produces a profile across eight dimensions, identifies language as infrastructure as the primary structural leverage point, and recommends two concrete interventions within the stack's own governance rather than upstream in the Caddy project.

---

## Sources and Acknowledgements

The evaluative framework applied in this document is defined in <a name="apa-evaluator-citation-2"></a>[Steel (2026a)](#apa-evaluator-reference), *Universal Cake praxis evaluator*, version 0.1.0. The Universal Cake systems praxis on which that framework is based is described in <a name="apa-steel-2026-citation"></a>[Steel (2026b)](#apa-steel-2026-reference). This evaluation follows the session protocol defined in <a name="apa-prompt-citation"></a>[Steel (2026c)](#apa-prompt-reference), *Prompt -- Universal Cake praxis evaluation*, version 0.1.0. Document formatting follows the <a name="apa-markdown-citation"></a>[web-ready unrendered markdown using APA 7 specification (Steel, 2026d)](#apa-markdown-reference), version 0.2.2. Authoring conventions follow the <a name="apa-styleguide-citation"></a>[style guide for technical documentation for technologists (Steel, 2026e)](#apa-styleguide-reference), version 0.2.0. The primary source material for the project under evaluation is the <a name="apa-radar-citation"></a>[Caddy radar entry for the Hugo hosting stack (Steel, 2026f)](#apa-radar-reference), version 0.1.1. Systems theory foundations draw on <a name="apa-meadows-citation"></a>[Meadows (2008)](#apa-meadows-reference) and <a name="apa-bertalanffy-citation"></a>[von Bertalanffy (1968)](#apa-bertalanffy-reference). Inclusive design principles follow <a name="apa-treviranus-citation"></a>[Treviranus (2018)](#apa-treviranus-reference).

---

## 1. Project summary

Caddy (version 2.11.2) is an open-source web server and reverse proxy written in Go, released under the Apache License 2.0 and maintained by a small core team with community contributions via GitHub. It is being evaluated here in a specific deployment context: as a candidate replacement for Apache2 in the Hugo hosting stack, where it would serve pre-built static sites and statically generated webstores over HTTPS on a single Linux server, provisioned by an Ansible orchestrator and initially hosted on Digital Ocean with planned migration to a Canadian or Icelandic provider.

The project being evaluated is therefore not Caddy in the abstract but Caddy as a component of a sovereign, operator-controlled hosting stack. The stack's defining constraints are provider portability, minimal external dependencies, and alignment with Universal Cake's values of accessibility, sovereignty, and long-term operational sustainability. Caddy ships as a single static binary with no runtime dependencies; it handles TLS certificate provisioning and renewal automatically via ACME and requires no module management. Configuration is expressed in a single Caddyfile, making the web server tier configuration-identical across providers in a way that Apache2's distro-packaged form is not. Caddy has not yet been adopted — it is currently in assess.

The Ansible orchestration layer currently includes an Apache2 vector that installs and configures Apache2 in layers. Adopting Caddy would require the creation of a new Caddy vector within that orchestrator, not a rewrite of the existing one. The Caddyfile's simpler configuration surface — no module management, no separate TLS tooling — means the new vector would have a substantially smaller surface area than its Apache2 counterpart. The shim layer is unaffected. The primary known limitation relevant to this evaluation is that Caddy's automatic TLS is structurally dependent on outbound ACME connections to Let's Encrypt and ZeroSSL, introducing two external service dependencies into an otherwise sovereign stack. No independent network capture has been performed to verify that outbound connections match documented behaviour; this is flagged as a gap in the radar entry.

---

## 2. Evaluation

### 2.1 Dimension one -- accessibility and inclusion

**Score: 4 -- Embedded.**

The primary workflow here is operator-facing: installing Caddy, writing a Caddyfile, and maintaining a running server. The Caddyfile syntax is genuinely simpler than Apache2's virtual host configuration — fewer directives, less indirection, and automatic TLS removes an entire category of error-prone manual steps. Reduced complexity and cognitive load are substantive accessibility improvements, not incidental ones: a simpler configuration surface means fewer errors, shorter time-to-competency, and meaningfully lower barriers for operators who are not Apache2 or Nginx specialists. In the Universal Cake framework, this counts.

The Caddy documentation (caddyserver.com/docs) was tested for keyboard accessibility during this evaluation. Tab navigation is clean and functional — the install section is reachable by keyboard without friction. The documentation is more compact and better structured than the Apache2 equivalent, which adds a further accessibility advantage: less content to navigate means less cognitive overhead for all users, and particularly for users relying on assistive technology. The documentation site is itself served by Caddy and built on plain HTML, CSS, and minimal JavaScript with no heavy frontend framework — a technical choice that directly produces the accessible keyboard navigation observed. This is not accidental. A site built on plain HTML is inherently more compatible with screen readers, keyboard navigation, and assistive technology than one built on a JavaScript-heavy framework. The documentation site's own technical choices reflect the same values we are evaluating Caddy for.

It is worth noting that command-line interfaces are in many respects more accessible than GUI-based alternatives. Tools such as Emacs and Emacs Speaks provide rich accessibility support for all command-line tooling, including server administration workflows. A stack that is entirely command-line-operable — as this one is — is more accessible in this regard than stacks that depend on web-based control panels or GUI-only administration tools.

The score does not reach 5 because there is no evidence of a self-reinforcing feedback loop for accessibility improvement within the Caddy project — what is present is the outcome of good engineering and documentation practice rather than a structured, iterative accessibility process.

**Diagnostic questions:**

- *Can people with disabilities fully participate in the project's primary workflows?* The operator workflows are entirely command-line-based and are therefore compatible with the full range of assistive technology that supports terminal environments, including screen readers, refreshable braille displays, and voice input. No GUI dependency exists in the primary operator path.
- *Is cognitive load minimised through consistent structure, clear language, and reduced interface complexity?* Yes, demonstrably. The Caddyfile is the most compact production-ready web server configuration format in common use. Automatic TLS eliminates an entire cognitive domain.
- *Is the system usable by people without specialist domain knowledge, or does it assume a narrow prior?* It assumes Linux command-line literacy and basic DNS knowledge. This is a narrower prior than a managed hosting panel but substantially broader than Apache2 or Nginx configuration.
- *Are accessibility concerns addressed at the design stage or retrofitted after problems are reported?* The evidence suggests design-stage consideration, particularly in documentation structure and site architecture, though this is not explicitly documented as an accessibility design process.

### 2.2 Dimension two -- language as infrastructure

**Score: 2 -- Partial.**

The Caddy documentation is in English only. There is no evidence of multilingual documentation, translation infrastructure, or community participation structures designed for contributors whose first language is not English. The Caddyfile's directive vocabulary is consistent and well-defined within the documentation, and the terminology is simpler and more regular than Apache2's — which is a genuine language accessibility improvement within English. However, the question of who is excluded by English-only documentation has not been examined at the project level. The project's GitHub issues, forum, and contribution structures are English-language by default with no apparent structural acknowledgement of this constraint. This is a common gap in open source infrastructure projects and is not specific to Caddy, but it is a gap nonetheless.

The score does not reach 3 because there is no evidence of even partial structural response to multilingual participation — what exists is good English-language documentation practice, not language infrastructure.

**Diagnostic questions:**

- *Is multilingual participation considered from the outset rather than deferred?* No evidence of consideration at any stage.
- *Is terminology consistent, defined, and accessible to the intended range of contributors?* Within English, yes. The Caddyfile vocabulary is more consistent and accessible than Apache2's equivalent. Across languages, the question has not been addressed.
- *Are communication structures designed to support participation across different linguistic and cultural contexts?* No. GitHub issues and the community forum are English-language by convention with no structural support for other languages.
- *Does the project's language design actively determine who can contribute, and has that determination been examined?* The language design excludes non-English speakers from meaningful contribution. This determination has not been examined or acknowledged in any project documentation reviewed.

### 2.3 Dimension three -- feedback loops

**Score: 3 -- Developing.**

Caddy has clear and accessible feedback channels: GitHub issues, a community forum (caddy.community), and public pull requests. The CVE batch patched in 2.11.2 demonstrates that security feedback from external researchers reaches the project and produces structural fixes — six CVEs addressed in a single release, with patch notes that name the reporters. This is evidence of a functioning security feedback loop. The public changelog across releases shows a consistent record of community-reported issues producing structural changes, including the removal of the v1 telemetry feature following widespread community criticism — a significant structural response to negative feedback rather than a surface adjustment.

The score does not reach 4 because the feedback infrastructure, while functional, is not structured to surface accessibility or inclusion gaps specifically. The channels that exist serve technically engaged contributors well; there is no evidence of mechanisms designed to reach participants who are excluded or who lack the technical fluency to engage via GitHub. The negative feedback loops that exist — security reporting, issue tracking, changelog maintenance — are healthy but narrowly scoped.

**Diagnostic questions:**

- *Are there clear, accessible channels for participants to report problems, gaps, and unintended consequences?* Yes, for technically engaged participants. GitHub issues and the community forum are well-maintained. The barrier to entry for these channels is command-line literacy and English fluency.
- *Does feedback from participants demonstrably change the project's structure, not just its surface behaviour?* Yes. The telemetry removal and the 2.11.2 CVE batch are both examples of structural change produced by community feedback.
- *Are negative feedback loops in place to stabilise the system?* Yes. Security reporting, issue tracking, and release governance are all functioning stabilising loops. The changelog provides continuity across releases.
- *Is there a record of how feedback has changed the project over time?* Yes. The GitHub release history and changelog provide a navigable record of feedback-driven structural change.

### 2.4 Dimension four -- cognitive and operational sustainability

**Score: 4 -- Embedded.**

In the Hugo hosting stack context, Caddy's contribution to cognitive and operational sustainability is substantial. Automatic TLS certificate provisioning and renewal removes an entire maintenance category that with Apache2 requires Certbot installation, cron job management, renewal testing, and periodic intervention when renewals fail silently. The single Caddyfile configuration eliminates the cognitive overhead of Apache2's distributed configuration structure — `sites-available`, `sites-enabled`, module activation, and restart management. The single static binary eliminates dependency management at the system level. For a small operator team or a solo operator, these reductions are not cosmetic; they directly reduce the probability of maintenance failures caused by cognitive overload or contributor unavailability.

The Caddy project itself is primarily maintained by a small core team, which represents a key-contributor risk that has not been explicitly addressed in public governance documentation. This is noted but weighted lightly in this evaluation because the assessment is of Caddy as a stack component, not as a community project the Hugo hosting stack participates in. The Apache 2.0 licence and the single-binary deployment model mean the stack can continue operating on a pinned version indefinitely without upstream dependency. The score does not reach 5 because there is no evidence of a self-reinforcing sustainability improvement process within the project.

**Diagnostic questions:**

- *Is the maintenance burden realistic given the actual capacity of the people responsible for it?* Yes, more so than the Apache2 equivalent. Automatic TLS renewal and single-file configuration substantially reduce ongoing maintenance surface.
- *Does the project minimise unnecessary complexity in its interfaces, documentation, and workflows?* Yes. The Caddyfile is the clearest expression of this: it is designed to eliminate configuration categories rather than expose them with better defaults.
- *Are contribution processes designed to accommodate human cognitive limits?* The operator contribution model — edit a Caddyfile, reload — is low-friction. The upstream contribution model for the Caddy project itself is standard GitHub practice with no unusual cognitive demands.
- *Has the project explicitly considered what happens when key contributors are unavailable?* Not in public governance documentation. The Apache 2.0 licence and binary deployment model provide practical resilience at the stack level regardless.

### 2.5 Dimension five -- relationship-centred design

**Score: 3 -- Developing.**

In the Hugo hosting stack context, Caddy's relationship-centred design is most visible in the way its components reinforce each other structurally. Automatic TLS is not a separate subsystem bolted onto a file server — it is integrated into the same configuration surface and the same binary, so improving TLS configuration automatically improves the security posture of every site served. The Caddyfile's unified configuration model means that changes to one site's configuration are made in the same place and the same format as changes to any other, reducing the risk of configuration drift between sites. The plain HTML documentation site reflects the same design values as the server itself — simplicity, directness, and minimal external dependencies — suggesting that the project's design orientation is consistent across its outputs rather than siloed.

The relationship between Caddy and the broader Hugo hosting stack is also well-suited: Caddy's single-binary deployment integrates cleanly with the Ansible orchestration layer without requiring the orchestrator to manage module state, TLS tooling, or service dependencies beyond the binary itself. This is a relationship-aware design property, even if it is not framed that way explicitly by the project.

The score does not reach 4 because the interdependencies between Caddy and its external dependencies — particularly Let's Encrypt and ZeroSSL — are not made fully visible to operators by default. An operator who does not read the documentation carefully may not understand that TLS renewal failures are an upstream dependency concern, not a Caddy configuration error. The relationship to the ACME CA layer is structural but not prominently surfaced in the operator-facing configuration experience.

**Diagnostic questions:**

- *Are the interdependencies between the project's components made visible and actively managed?* Internally, yes. The ACME dependency on external CAs is documented but not prominently surfaced in the operator configuration experience.
- *Are human-system interactions central to design decisions?* The Caddyfile's design suggests they are. Complexity reduction as a design goal is inherently human-centred.
- *Does improving one area of the project strengthen others?* Yes. TLS improvement strengthens file serving security. Documentation site architecture reflects server architecture values. Components reinforce each other.
- *Are the relationships between the project and its broader ecosystem explicitly considered?* Partially. The multi-issuer ACME fallback explicitly models the CA ecosystem relationship. The relationship to operator cognitive load is implicit in design decisions but not named as such.

### 2.6 Dimension six -- knowledge infrastructure

**Score: 4 -- Embedded.**

The Caddy documentation is comprehensive, well-structured, and demonstrably accessible to operators who had no prior involvement in the project — this evaluation has drawn on it directly and found it sufficient to support scoring across multiple dimensions without requiring external sources for basic operational questions. The documentation site source is public and versioned on GitHub, meaning documentation changes are tracked, attributable, and reversible. The changelog across releases is consistent and detailed, providing a navigable record of how the project has evolved. The Caddyfile syntax documentation covers directives systematically with examples, and the automatic HTTPS documentation makes the ACME dependency and its implications explicit for operators who read it.

The score does not reach 5 because there is no evidence of a structured process for identifying and filling documentation gaps — what exists is well-maintained documentation produced by a small core team rather than a self-reinforcing knowledge infrastructure with explicit governance, gap detection, and community contribution processes for documentation specifically.

**Diagnostic questions:**

- *Is the project's documentation accessible to people who were not involved in creating it?* Yes, demonstrably. This evaluation relied on it directly.
- *Is documentation maintained as a living system, not produced once and left to drift?* Yes. The documentation site is versioned on GitHub with a consistent commit history. Release notes cross-reference documentation changes.
- *Can the project's knowledge survive the departure of its most active contributors?* The documentation is sufficiently complete and public that operational knowledge would survive. Architectural and design rationale is less thoroughly documented.
- *Are the governance structures for knowledge explicit rather than assumed?* Partially. The public GitHub repository makes contribution visible, but there is no explicit documentation governance policy stating who maintains what, how gaps are identified, or what the documentation coverage targets are.

### 2.7 Dimension seven -- ethical orientation

**Score: 3 -- Developing.**

The telemetry episode in Caddy v1 is the clearest evidence of ethical orientation in the project's history. Community criticism of opt-out telemetry produced a structural response — the feature was made a no-op and removed before v2 shipped — rather than a surface adjustment such as improved documentation of the opt-out process. This demonstrates that the project has a mechanism for raising ethical concerns that results in structural change, even if that mechanism is informal rather than governed. The Apache 2.0 licence is a deliberate ethical choice: it permits use, modification, and redistribution without restriction, and does not require contributors to sign away rights. The separation between the Apache 2.0 core and commercial enterprise features is transparent and documented, avoiding the bait-and-switch licensing pattern that has affected other infrastructure projects.

The score does not reach 4 because the project has not explicitly examined who is excluded by its design choices — English-only documentation, GitHub-centric contribution, and the assumption of command-line literacy are not acknowledged as exclusionary constraints. The incentive structure created by the commercial licensing tier has not been publicly examined for the behaviours it may produce over time, particularly regarding which features migrate to the commercial tier as the project matures. These are not acute concerns at present but they are unexamined, and an embedded ethical orientation would name them.

**Diagnostic questions:**

- *Has the project explicitly identified who benefits from its current design and who may be excluded?* No. Benefits are implicit in design decisions; exclusions are unexamined.
- *Are exclusionary assumptions identified and challenged?* No. English-only documentation and GitHub-centric contribution are inherited defaults, not examined choices.
- *Are the incentive structures embedded in the project examined for the behaviours they produce?* Partially. The commercial licensing tier separation is transparent, but its long-term incentive effects have not been publicly examined.
- *Does the project have a mechanism for raising ethical concerns that results in structural change?* Yes, informally. The telemetry episode demonstrates this capacity exists even without a formal mechanism.

### 2.8 Dimension eight -- emergence and adaptability

**Score: 4 -- Embedded.**

Caddy's modular architecture is its primary structural expression of adaptability. The plugin system allows capabilities to be compiled in without modifying the core, meaning unexpected use patterns can be accommodated without redesigning the server itself. The JSON configuration API alongside the Caddyfile provides two distinct interfaces for different operator contexts — programmatic and human-authored — which is a design decision that acknowledges the diversity of deployment patterns rather than optimising for a single expected use. The multi-issuer ACME support — falling back from Let's Encrypt to ZeroSSL automatically — is a direct expression of resilience prioritised over optimisation: the system is designed to absorb CA outages rather than fail cleanly. The v1-to-v2 transition, which broke configuration compatibility deliberately in order to produce a sounder architecture, demonstrates willingness to accept short-term disruption for long-term structural improvement — a form of institutional humility about the limits of the original design.

In the Hugo hosting stack context, Caddy's single-binary deployment and provider-agnostic configuration directly support the stack's own adaptability requirements: migration between Digital Ocean, Canadian, and Icelandic providers requires no changes to the web server tier.

The score does not reach 5 because there is no evidence of a structured process for detecting and responding to emergent use patterns — adaptation has occurred reactively in response to community pressure and technical necessity rather than through a proactive feedback and adaptation mechanism.

**Diagnostic questions:**

- *Can the project adapt to unexpected use patterns without requiring a redesign?* Yes. The plugin architecture and JSON API are explicitly designed for this.
- *Does the project's governance and design process maintain humility about what cannot be predicted?* The v1-to-v2 architectural break demonstrates this. Multi-issuer ACME fallback demonstrates it operationally.
- *Is resilience explicitly prioritised over optimisation for known conditions?* Yes. Multi-issuer fallback, session ticket rotation, and the modular architecture all prioritise resilience.
- *When the project has encountered unexpected outcomes, has it adapted its structure rather than simply its surface behaviour?* Yes. The telemetry removal and the v1-to-v2 architectural redesign are both structural responses to unexpected outcomes.

---

## 3. Profile and interpretation

| Dimension | Score |
|---|---|
| 1. Accessibility and inclusion | 4 |
| 2. Language as infrastructure | 2 |
| 3. Feedback loops | 3 |
| 4. Cognitive and operational sustainability | 4 |
| 5. Relationship-centred design | 3 |
| 6. Knowledge infrastructure | 4 |
| 7. Ethical orientation | 3 |
| 8. Emergence and adaptability | 4 |

The profile clusters strongly in the three to four range, which is consistent with a mature, well-engineered open source infrastructure project with good design practice and no serious systemic failures. The overall picture is of a project that produces positive outcomes across most dimensions as a consequence of good engineering rather than as a consequence of deliberate, structured systems thinking. The distinction matters: a project that scores four through good practice is more fragile than one that scores four through embedded process, because the former depends on the continued presence and judgment of the people who created the good practice.

The single outlier is dimension two — language as infrastructure — at score 2. This is the structural leverage point the evaluator framework identifies as the highest priority for intervention. Following <a name="apa-meadows-citation-2"></a>[Meadows (2008)](#apa-meadows-reference), we do not recommend improving every dimension uniformly; we recommend examining where structural change will produce the largest systemic effect. Language as infrastructure scores low not because Caddy's documentation is poor — it is demonstrably good — but because the question of who is excluded by English-only, GitHub-centric, command-line-assuming participation structures has not been examined at the project level. In the Hugo hosting stack context this is partially mitigated: the stack's operator audience is technical and English-literate by current assumption. But as a Universal Cake evaluation the gap is real and worth naming.

The relationship between dimension two and dimension seven is worth examining explicitly. The ethical orientation dimension scores 3 in part because exclusionary assumptions have not been identified and challenged — this is the same gap that produces the score of 2 in dimension two. These two dimensions are amplifying each other's weakness: the absence of language infrastructure examination is also an absence of ethical examination, and addressing one would structurally improve the other. This is the kind of dimensional relationship the evaluator framework is designed to surface.

The scores of 4 in dimensions one, four, six, and eight together indicate a project that is genuinely accessible to its intended operator audience, sustainable to maintain, well-documented, and adaptable to changing conditions. For the Hugo hosting stack's specific use case — a small operator team running sovereign, portable static site infrastructure — this profile is a strong fit. The weaknesses are real but none of them are acute blockers for adoption in this context.

---

## 4. Recommended interventions

Per the evaluator protocol, interventions are required for dimensions scoring two or below. One dimension meets this threshold.

### 4.1 Dimension two -- language as infrastructure

The structural change required is not translation of the Caddy documentation — that is upstream work outside the Hugo hosting stack's control. The change that is within scope is to make the exclusionary assumptions of the stack explicit in the stack's own documentation, and to examine them deliberately rather than inheriting them by default.

Concretely: the Hugo hosting stack documentation should include a section on its assumed operator profile that names the linguistic, technical, and cultural assumptions the stack currently makes — English literacy, command-line fluency, familiarity with Linux server administration — and states whether those assumptions are intentional constraints or unexamined defaults. If they are intentional, the reasoning should be documented. If they are unexamined defaults, the documentation should identify what would need to change to widen participation, and what the cost of that change would be.

This is a small structural addition to the stack's documentation that produces a disproportionate systemic effect: it converts an invisible exclusion into a visible, examined, and governable one. It does not require the stack to immediately support multilingual operators or non-technical contributors. It requires the stack to know who it is excluding and why, which is the minimum condition for the exclusion to be ethical rather than merely habitual.

A secondary intervention, also within the stack's control, is to evaluate future tooling choices — including Caddy itself — against multilingual documentation availability as an explicit criterion in the radar entry template. The current template does not include this field. Adding a `multilingual_documentation` field to the radar entry template would embed language infrastructure assessment into the stack's standard tooling evaluation process, producing a feedback loop that improves dimension two scores over time across the entire stack.

---

## Resources

### Governing documents
- [Universal Cake praxis evaluator (Steel, 2026a)](#apa-evaluator-reference)
- [Universal Cake as systems theory and systems praxis (Steel, 2026b)](#apa-steel-2026-reference)
- [Prompt -- Universal Cake praxis evaluation (Steel, 2026c)](#apa-prompt-reference)
- [Web-ready unrendered markdown using APA 7 (Steel, 2026d)](#apa-markdown-reference)
- [Style guide for technical documentation for technologists (Steel, 2026e)](#apa-styleguide-reference)

### Source material
- [Caddy radar entry for the Hugo hosting stack (Steel, 2026f)](#apa-radar-reference)

### Systems theory foundations
- [Meadows, D. H. (2008). Thinking in systems: A primer](#apa-meadows-reference)
- [Von Bertalanffy, L. (1968). General system theory](#apa-bertalanffy-reference)

### Inclusive design
- [Treviranus, J. (2018). The value of the atypical](#apa-treviranus-reference)

### Caddy project
- [Caddy web server](https://caddyserver.com)
- [Caddy GitHub repository](https://github.com/caddyserver/caddy)
- [Caddy website source repository](https://github.com/caddyserver/website)
- [Caddy 2.11.2 release notes](https://github.com/caddyserver/caddy/releases/tag/v2.11.2)

---

## References

<a name="apa-bertalanffy-reference"></a>von Bertalanffy, L. (1968). *General system theory: Foundations, development, applications*. George Braziller.
[Return to citation](#apa-bertalanffy-citation)

<a name="apa-evaluator-reference"></a>Steel, C. (2026a). *Universal Cake praxis evaluator* (Version 0.1.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-evaluator-citation)

<a name="apa-steel-2026-reference"></a>Steel, C. (2026b). *Universal Cake as systems theory and systems praxis* (Version 1.0) [Working paper]. https://universalcake.ca
[Return to citation](#apa-steel-2026-citation)

<a name="apa-prompt-reference"></a>Steel, C. (2026c). *Prompt -- Universal Cake praxis evaluation* (Version 0.1.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-prompt-citation)

<a name="apa-markdown-reference"></a>Steel, C. (2026d). *Web-ready unrendered markdown using APA 7* (Version 0.2.2) [Technical document]. https://universalcake.ca
[Return to citation](#apa-markdown-citation)

<a name="apa-styleguide-reference"></a>Steel, C. (2026e). *Style guide: Technical documentation for technologists* (Version 0.2.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-styleguide-citation)

<a name="apa-radar-reference"></a>Steel, C. (2026f). *Caddy radar entry -- Hugo hosting stack* (Version 0.1.1) [Technical document]. https://universalcake.ca
[Return to citation](#apa-radar-citation)

<a name="apa-meadows-reference"></a>Meadows, D. H. (2008). *Thinking in systems: A primer*. Chelsea Green Publishing.
[Return to citation](#apa-meadows-citation)

<a name="apa-treviranus-reference"></a>Treviranus, J. (2018). The value of the atypical. In G. Coombs & S. McNamara (Eds.), *Rethinking inclusion and transformation*. Inclusive Design Research Centre.
[Return to citation](#apa-treviranus-citation)

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

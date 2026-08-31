# Prometheus -- Universal Cake Praxis Evaluation

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md
Markdown Spec: web-ready-unrendered-markdown-using-apa-7-v0.2.1.md
Evaluator: universal-cake-praxis-evaluator-v0.1.0.md

## Abstract

This document evaluates Prometheus, the open-source monitoring system and time series database, against the eight dimensions of Universal Cake praxis. Each dimension is scored on the zero-to-five rubric defined in the evaluator, with the score grounded in evidence from Prometheus's public documentation, governance, and community structure. The evaluation finds a clear and coherent pattern: Prometheus is systemically strong on the dimensions that concern machine-facing infrastructure — feedback loops, knowledge infrastructure, and emergence and adaptability — and systemically weak on the dimensions that concern human participation — accessibility and inclusion, language as infrastructure, cognitive and operational sustainability, and relationship-centered design. The profile suggests a project that optimises the relationships between technical components with considerable maturity while treating the cognitive and access needs of human participants as largely out of scope. The lowest-scoring dimensions are identified as structural leverage points, and concrete interventions are proposed for every dimension scoring two or below.

## Sources and Acknowledgements

The evaluative framework applied here is defined in `universal-cake-praxis-evaluator-v0.1.0.md`, which operationalises the Universal Cake praxis described by <a name="apa-steel-2026-citation"></a>[Steel (2026)](#apa-steel-2026-reference). The systems theory foundations of that framework draw on <a name="apa-bertalanffy-1968-citation"></a>[von Bertalanffy (1968)](#apa-bertalanffy-1968-reference), and the leverage-point reasoning used in the interpretation follows <a name="apa-meadows-2008-citation"></a>[Meadows (2008)](#apa-meadows-2008-reference). The inclusive design principle underlying the accessibility dimension follows <a name="apa-treviranus-2018-citation"></a>[Treviranus (2018)](#apa-treviranus-2018-reference), and the relational framing of well-being follows <a name="apa-engel-1977-citation"></a>[Engel (1977)](#apa-engel-1977-reference).

The project facts are drawn from Prometheus's own documentation, in particular its governance documentation (<a name="apa-prometheus-nd-a-citation"></a>[Prometheus Authors, n.d.-a](#apa-prometheus-nd-a-reference)) and its storage documentation (<a name="apa-prometheus-nd-b-citation"></a>[Prometheus Authors, n.d.-b](#apa-prometheus-nd-b-reference)), together with the Cloud Native Computing Foundation's graduation announcement (<a name="apa-cncf-2018-citation"></a>[Cloud Native Computing Foundation, 2018](#apa-cncf-2018-reference)).

A note on evidentiary basis. No author-supplied source corpus was provided for this evaluation. The scores below are grounded in Prometheus's public documentation and publicly observable governance and community structure. Internal materials that an embedded practitioner might supply — for example, contributor-facing onboarding records, accessibility audits, or design-decision archives — were not available and could raise specific scores if produced. Where this absence materially affects a score, it is stated in that dimension's reasoning rather than silently resolved. In addition, the verbatim diagnostic questions for dimensions one and two are taken from the evaluator; the diagnostic questions for dimensions three through eight are reconstructed to match the evaluator's stated intent and should be reconciled against the canonical evaluator document before this evaluation is treated as final.

## 1. Project summary

Prometheus is an open-source monitoring system and time series database originally developed in 2012 and now governed as a graduated project of the Cloud Native Computing Foundation, the second project to reach that maturity level after Kubernetes (<a name="apa-cncf-2018-citation-2"></a>[Cloud Native Computing Foundation, 2018](#apa-cncf-2018-reference)). It is licensed under Apache 2.0 and is community-driven rather than controlled by a single vendor.

Architecturally, Prometheus collects metrics by pulling them from instrumented targets that it discovers through service discovery, stores them in a local single-node time series database organised around a dimensional data model, and exposes them through a flexible query language, PromQL. It integrates with a large ecosystem of exporters and with alerting through Alertmanager, and it is widely treated as the de facto standard for metric-based monitoring in cloud-native environments.

Its primary participants are operators, site reliability engineers, and platform teams. Its well-documented strengths are simplicity of initial deployment — a single binary — and a mature, extensible ecosystem. Its well-documented limitations are a steep PromQL learning curve, local storage that is neither clustered nor replicated and is not intended for long-term retention, the absence of built-in high availability and authentication, and operational complexity that grows with scale, particularly around cardinality and memory (<a name="apa-prometheus-nd-b-citation-2"></a>[Prometheus Authors, n.d.-b](#apa-prometheus-nd-b-reference)).

## 2. Evaluation

### 2.1 Dimension one — accessibility and inclusion

**Score: 1 — Acknowledged.**

The primary workflows of Prometheus — writing scrape configuration, designing labels, composing PromQL queries, and reading the expression browser — assume a substantial specialist prior. There is no evidence of structured accessibility work for people with disabilities, no published conformance target for the web interface, and documentation that is effective but dense. The PromQL learning curve is consistently identified as a barrier for newcomers. The concern is implicitly acknowledged through getting-started material, but there is no structural response to the diversity of human needs that <a name="apa-treviranus-2018-citation-2"></a>[Treviranus (2018)](#apa-treviranus-2018-reference) argues inclusive design must address, so the score does not rise above one.

- *Can people with disabilities fully participate in the project's primary workflows?* There is no published evidence of accessibility conformance work for the web interface or documentation, so this cannot be affirmed.
- *Is cognitive load minimised through consistent structure, clear language, and reduced interface complexity?* No. PromQL, cardinality management, and configuration impose high and well-documented cognitive load.
- *Is the system usable by people without specialist domain knowledge, or does it assume a narrow prior?* It assumes a narrow operations-and-time-series prior.
- *Are accessibility concerns addressed at the design stage or retrofitted after problems are reported?* Neither is visible in the public record; the dimension appears largely unaddressed as a design concern.

### 2.2 Dimension two — language as infrastructure

**Score: 1 — Acknowledged.**

Prometheus treats human language largely as a presentation layer rather than as structural infrastructure that shapes who can participate. Official documentation is primarily in English, and there is no structural internationalisation programme that makes participation in the project's core workflows equally available across languages. The project does maintain a deliberate technical interface vocabulary, but the dimension here concerns human communication and the breadth of who can contribute, and on that axis the response is minimal.

- *Is language treated as a structural concern that shapes participation, or as a layer applied after decisions are made?* It is treated as a presentation layer.
- *Can people participate in core workflows in languages other than English?* Not in any structurally supported way within the official project.
- *Does communication design widen or narrow the pool of potential contributors?* The reliance on English and specialist terminology narrows it.

### 2.3 Dimension three — feedback loops

**Score: 4 — Embedded.**

This is a genuine strength, in two senses. As a product, Prometheus exists to create feedback loops for the systems it monitors, turning telemetry into alerting. As a project, it operates mature feedback structures: a public issue tracker, a documented proposal process, regular releases, an annual conference, and consensus-based governance that resolves to a majority vote only when consensus fails (<a name="apa-prometheus-nd-a-citation-2"></a>[Prometheus Authors, n.d.-a](#apa-prometheus-nd-a-reference)). These are the reinforcing and balancing loops that <a name="apa-meadows-2008-citation-2"></a>[Meadows (2008)](#apa-meadows-2008-reference) identifies as the basis of systemic health. The score stops short of five because the feedback that is well-developed is largely technical and contributor-facing; feedback specifically about who is excluded from participation is less visible.

- *Does the project gather feedback on what is working, and act on it?* Yes, through issues, proposals, and releases.
- *Are there reinforcing loops that strengthen contribution over time?* Yes; an active contributor and user community has compounded since inception.
- *Are there balancing loops that stabilise the system and prevent drift?* Yes; review, governance, and a code of conduct provide stabilisation.
- *Does the project surface unintended consequences and emergent harms?* Partially; technical regressions are surfaced well, but participation harms less so.

### 2.4 Dimension four — cognitive and operational sustainability

**Score: 2 — Partial.**

The picture is genuinely split. The project itself is sustainably maintained, with a broad contributor base and foundation backing. For users, the system is sustainable at small scale — a single binary is simple to run — but operational burden grows steeply with scale. Operators must manage cardinality and memory, plan retention, and bolt on external systems for long-term storage and high availability, none of which Prometheus provides natively (<a name="apa-prometheus-nd-b-citation-3"></a>[Prometheus Authors, n.d.-b](#apa-prometheus-nd-b-reference)). Combined with the high cognitive load of PromQL, the user-facing sustainability is inconsistent, which holds the score at two even though the project's internal maintenance is strong.

- *Does the system adapt to human cognitive limits, or force humans to adapt to it?* It largely requires humans to adapt to it.
- *Is operational burden bounded and predictable over time?* No; it grows faster than linearly with scale and series cardinality.
- *Is the project maintainable without depending on a small number of overloaded individuals?* Yes, at the project level; contribution is broad and governed.
- *Is unnecessary complexity actively reduced?* Partially; deployment is kept simple, but scaling complexity is pushed to the operator.

### 2.5 Dimension five — relationship-centered design

**Score: 2 — Partial.**

Prometheus designs the relationships between technical components with real care: the pull model, service discovery, and exporter ecosystem all treat interoperation as a first-class concern. Its community relationships are also supported structurally, with a code of conduct and a stated commitment to a welcoming environment (<a name="apa-prometheus-nd-a-citation-3"></a>[Prometheus Authors, n.d.-a](#apa-prometheus-nd-a-reference)). What is underdeveloped is the relationship between the human participant and the system. The human-software interaction layer — onboarding, the query experience, error legibility — is thin relative to the machine-to-machine layer. In the relational view of well-being described by <a name="apa-engel-1977-citation-2"></a>[Engel (1977)](#apa-engel-1977-reference) and extended by Universal Cake, the relationships that most shape a newcomer's experience are the least designed, which holds the score at two.

- *Does the design optimise relationships between components and participants, not only isolated parts?* For components, yes; for human participants, only partially.
- *Is human-system interaction treated as a first-class design concern?* No; it is secondary to component interoperability.
- *Are community relationships supported structurally?* Yes, through governance and a code of conduct.

### 2.6 Dimension six — knowledge infrastructure

**Score: 4 — Embedded.**

Prometheus maintains comprehensive, versioned, and discoverable documentation; a transparent governance document; an open proposal process; and a public conference whose talks accumulate as a knowledge archive. The reasoning behind major design decisions — for example, the deliberate choice to provide remote-storage interfaces rather than solve clustered storage internally — is documented and reconstructable (<a name="apa-prometheus-nd-b-citation-4"></a>[Prometheus Authors, n.d.-b](#apa-prometheus-nd-b-reference)). Knowledge is distributed across many contributors rather than centralised. The score stops short of five only because the density of the documentation reduces its accessibility to non-specialists, linking this dimension's ceiling to the weakness in dimension one.

- *Is knowledge documented, discoverable, and preserved over time?* Yes, extensively.
- *Are governance and decision records transparent and accessible?* Yes; governance is public and decisions are traceable.
- *Can newcomers reconstruct the reasoning behind the system's design?* Yes, though density raises the effort required.
- *Is knowledge distributed or centralised in a few people?* Distributed across a broad contributor base.

### 2.7 Dimension seven — ethical orientation

**Score: 3 — Developing.**

Prometheus scores well on the questions of control and dependency. It is vendor-neutral, governed by an independent foundation, licensed permissively, and built on open exposition formats, all of which distribute control and reduce lock-in — it empowers operators rather than capturing them (<a name="apa-cncf-2018-citation-3"></a>[Cloud Native Computing Foundation, 2018](#apa-cncf-2018-reference)). What keeps the score at three is the absence of an explicit ethical orientation toward exclusion. The high expertise barrier functions as an implicit assumption about who counts as a legitimate participant, and that assumption is not examined in the project's stated values. Strong distribution of control coexists with an unexamined distribution of access.

- *Who benefits, and who is excluded?* Specialist operators benefit; non-specialists are effectively excluded by the expertise barrier.
- *Who controls the system, and does control concentrate or distribute?* Control is distributed through foundation governance.
- *Does the system increase empowerment or dependency?* Empowerment, through openness and the absence of lock-in.
- *Are incentives aligned with long-term health rather than short-term extraction?* Yes; the non-commercial governance model favours durability.

### 2.8 Dimension eight — emergence and adaptability

**Score: 4 — Embedded.**

Prometheus is deliberately designed for emergence. Rather than centralising every capability, it exposes interfaces and lets an ecosystem grow around them — long-term storage and horizontal scale are met by external projects such as Thanos, Cortex, and Grafana Mimir, built against Prometheus's remote-write and remote-read interfaces (<a name="apa-prometheus-nd-b-citation-5"></a>[Prometheus Authors, n.d.-b](#apa-prometheus-nd-b-reference)). The spin-out of OpenMetrics from the project's exposition format is a further example of adaptive evolution. This reflects exactly the humility before complexity that <a name="apa-meadows-2008-citation-3"></a>[Meadows (2008)](#apa-meadows-2008-reference) recommends: the project resists the temptation to solve everything centrally and instead favours revisable interfaces. The score stops short of five because the same modularity transfers integration and adaptation cost to operators, which can make adaptation brittle for those without specialist support.

- *Does the project hold humility about what it cannot predict or control?* Yes; it declines to centralise distributed storage and evaluation.
- *Is the system designed to support ecosystem emergence rather than centralise everything?* Yes, through stable interfaces.
- *Does the project favour resilience and revisability over rigidity?* Yes.
- *Can the system evolve without breaking dependent participants?* Mostly, though adaptation cost falls on operators.

## 3. Profile and interpretation

The eight scores are: accessibility and inclusion, 1; language as infrastructure, 1; feedback loops, 4; cognitive and operational sustainability, 2; relationship-centered design, 2; knowledge infrastructure, 4; ethical orientation, 3; and emergence and adaptability, 4.

The profile is not random scatter. It separates cleanly along a single axis. The four highest-scoring dimensions — feedback loops, knowledge infrastructure, emergence and adaptability, and ethical orientation — all concern the health of the system as machine-facing infrastructure and as a governed engineering community. The four lowest — accessibility and inclusion, language as infrastructure, cognitive and operational sustainability, and relationship-centered design — all concern the human participant's ability to enter, understand, and sustain a relationship with the system. Prometheus is, in Universal Cake terms, a project that has designed the relationships between its components with real maturity while treating the relationship between the system and the human as out of scope.

The two lowest-scoring dimensions, accessibility and inclusion and language as infrastructure, are the structural leverage points. <a name="apa-meadows-2008-citation-4"></a>[Meadows (2008)](#apa-meadows-2008-reference) argues that the most powerful interventions change information flows, incentives, and goals rather than optimising existing components, and these two dimensions are precisely where the project's implicit goal — to serve specialist operators well — sets a ceiling on every adjacent dimension. The density that holds knowledge infrastructure at four, the human-interaction gap that holds relationship-centered design at two, and the unexamined exclusion that holds ethical orientation at three all trace back to the same root assumption about who the system is for. Raising the accessibility and language scores would tend to lift the dimensions structurally coupled to them, which is the signature of a genuine leverage point rather than a local fix.

In short, the profile indicates a project in robust systemic health as an engineering artifact and a governed community, with a single, coherent, and addressable structural weakness: it has not yet treated the human participant's cognitive and access needs as part of the system it is responsible for.

## 4. Recommended interventions

The following interventions address every dimension scoring two or below. Each describes a structural change, not merely an aspiration.

**Accessibility and inclusion (1).** Adopt a published accessibility conformance target for the web interface and documentation and add it to the contribution and release criteria, so that accessibility is gated at the design stage rather than retrofitted after reports. Pair this with a layered documentation path that lets a non-specialist reach a first working query without first absorbing the full PromQL and cardinality model — the goal being to lower the prior the system assumes, not to remove depth for experts.

**Language as infrastructure (1).** Establish a structural translation pathway for the core getting-started and conceptual documentation, with a maintained source-of-truth and a contribution process for translators, so that language ceases to be a presentation layer applied after the fact and becomes maintained infrastructure that widens the contributor pool.

**Cognitive and operational sustainability (2).** Treat the scaling cliff as a documentation and defaults problem, not only an architecture one. Provide opinionated, maintained reference configurations and capacity-planning guidance for the common growth path, and surface cardinality and memory pressure to operators earlier and more legibly, so that operational burden becomes bounded and predictable rather than discovered at failure.

**Relationship-centered design (2).** Invest in the human-system interaction layer with the same seriousness applied to component interoperation: legible query errors that explain rather than report, an onboarding flow that models the first hour of use, and feedback channels specifically about the newcomer experience. The intervention is to make human-system interaction a first-class design concern with its own owners, not a by-product of component design.

## 5. Resources

- https://github.com/prometheus
- Prometheus documentation: https://prometheus.io/docs/
- Ansible Roles: https://github.com/search?q=ansible-role-prometheus&type=repositories
- Prometheus governance: https://prometheus.io/governance/
- Prometheus storage documentation: https://prometheus.io/docs/prometheus/latest/storage/
- Cloud Native Computing Foundation project page for Prometheus: https://www.cncf.io/projects/prometheus/
- `universal-cake-praxis-evaluator-v0.1.0.md` — the framework applied in this evaluation.

## 6. References

<a name="apa-bertalanffy-1968-reference"></a>
von Bertalanffy, L. (1968). *General system theory: Foundations, development, applications*. George Braziller.

[Return to citation](#apa-bertalanffy-1968-citation)

---

<a name="apa-cncf-2018-reference"></a>
Cloud Native Computing Foundation. (2018, August 9). *Cloud Native Computing Foundation announces Prometheus graduation*. https://www.cncf.io/announcements/2018/08/09/prometheus-graduates/

[Return to citation](#apa-cncf-2018-citation) · [Return to citation](#apa-cncf-2018-citation-2) · [Return to citation](#apa-cncf-2018-citation-3)

---

<a name="apa-engel-1977-reference"></a>
Engel, G. L. (1977). The need for a new medical model: A challenge for biomedicine. *Science, 196*(4286), 129–136. https://doi.org/10.1126/science.847460

[Return to citation](#apa-engel-1977-citation) · [Return to citation](#apa-engel-1977-citation-2)

---

<a name="apa-freire-1970-reference"></a>
Freire, P. (1970). *Pedagogy of the oppressed*. Continuum.

[Return to citation](#apa-freire-1970-citation)

---

<a name="apa-meadows-2008-reference"></a>
Meadows, D. H. (2008). *Thinking in systems: A primer*. Chelsea Green Publishing.

[Return to citation](#apa-meadows-2008-citation) · [Return to citation](#apa-meadows-2008-citation-2) · [Return to citation](#apa-meadows-2008-citation-3) · [Return to citation](#apa-meadows-2008-citation-4)

---

<a name="apa-prometheus-nd-a-reference"></a>
Prometheus Authors. (n.d.-a). *Governance*. Prometheus. Retrieved June 8, 2026, from https://prometheus.io/governance/

[Return to citation](#apa-prometheus-nd-a-citation) · [Return to citation](#apa-prometheus-nd-a-citation-2) · [Return to citation](#apa-prometheus-nd-a-citation-3)

---

<a name="apa-prometheus-nd-b-reference"></a>
Prometheus Authors. (n.d.-b). *Storage*. Prometheus. Retrieved June 8, 2026, from https://prometheus.io/docs/prometheus/latest/storage/

[Return to citation](#apa-prometheus-nd-b-citation) · [Return to citation](#apa-prometheus-nd-b-citation-2) · [Return to citation](#apa-prometheus-nd-b-citation-3) · [Return to citation](#apa-prometheus-nd-b-citation-4) · [Return to citation](#apa-prometheus-nd-b-citation-5)

---

<a name="apa-steel-2026-reference"></a>
Steel, C. (2026). *Universal Cake as systems theory and systems praxis* [Working paper].

[Return to citation](#apa-steel-2026-citation)

---

<a name="apa-treviranus-2018-reference"></a>
Treviranus, J. (2018). The value of the atypical. In G. Coombs & S. McNamara (Eds.), *Rethinking inclusion and transformation*. Inclusive Design Research Centre.

[Return to citation](#apa-treviranus-2018-citation) · [Return to citation](#apa-treviranus-2018-citation-2)

---

## 7. Changelog

- **0.1.0** — Initial evaluation of Prometheus against the eight Universal Cake praxis dimensions. Scores grounded in public documentation and governance; evidentiary basis and reconstructed diagnostic questions for dimensions three through eight noted as qualifications in the Sources and Acknowledgements section.

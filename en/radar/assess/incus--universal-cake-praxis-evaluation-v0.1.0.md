# Incus -- Universal Cake Praxis Evaluation

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md
Markdown Spec: web-ready-unrendered-markdown-using-apa-7-v0.2.1.md
Evaluator: universal-cake-praxis-evaluator-v0.1.0.md

## Abstract

This document evaluates Incus, the community-driven system container and virtual machine manager, against the eight dimensions of Universal Cake praxis. Each dimension is scored on the zero-to-five rubric defined in the evaluator, with the score grounded in evidence from Incus's public documentation, governance, and community structure. The evaluation finds a project in robust systemic health whose defining feature, in Universal Cake terms, is that ethical orientation is a founding strength rather than an afterthought: Incus exists because a community responded to the concentration of control over a shared project by building a more distributed alternative. It scores strongly on feedback loops, knowledge infrastructure, ethical orientation, and emergence and adaptability, and creditably on cognitive sustainability and relationship-centered design, reflecting deliberate attention to a unified user experience and to existing users through migration tooling. Its remaining weakness is the human-inclusion axis: language as infrastructure and accessibility for people with disabilities. Those two dimensions are identified as the structural leverage points, and concrete interventions are proposed for every dimension scoring two or below.

## Sources and Acknowledgements

The evaluative framework applied here is defined in `universal-cake-praxis-evaluator-v0.1.0.md`, which operationalises the Universal Cake praxis described by <a name="apa-steel-2026-citation"></a>[Steel (2026)](#apa-steel-2026-reference). The systems theory foundations of that framework draw on <a name="apa-bertalanffy-1968-citation"></a>[von Bertalanffy (1968)](#apa-bertalanffy-1968-reference), and the leverage-point reasoning used in the interpretation follows <a name="apa-meadows-2008-citation"></a>[Meadows (2008)](#apa-meadows-2008-reference). The inclusive design principle underlying the accessibility dimension follows <a name="apa-treviranus-2018-citation"></a>[Treviranus (2018)](#apa-treviranus-2018-reference), and the relational framing of well-being follows <a name="apa-engel-1977-citation"></a>[Engel (1977)](#apa-engel-1977-reference).

The project facts are drawn from Incus's own documentation and repository, in particular the project introduction (<a name="apa-lc-nd-a-citation"></a>[Linux Containers, n.d.-a](#apa-lc-nd-a-reference)), the support documentation (<a name="apa-lc-nd-b-citation"></a>[Linux Containers, n.d.-b](#apa-lc-nd-b-reference)), and the project repository (<a name="apa-incus-nd-citation"></a>[Incus Project, n.d.](#apa-incus-nd-reference)).

A note on evidentiary basis. No author-supplied source corpus was provided for this evaluation. The scores below are grounded in Incus's public documentation and publicly observable governance and community structure. Internal materials that an embedded practitioner might supply — for example, accessibility audits, design-decision archives, or contributor-onboarding records — were not available and could raise specific scores if produced. Where this absence materially affects a score, it is stated in that dimension's reasoning rather than silently resolved. In addition, the verbatim diagnostic questions for dimensions one and two are taken from the evaluator; the diagnostic questions for dimensions three through eight are reconstructed to match the evaluator's stated intent and should be reconciled against the canonical evaluator document before this evaluation is treated as final.

## 1. Project summary

Incus is a system container and virtual machine manager. It was created in 2023 as a community-driven fork of LXD, after Canonical changed the governance of LXD and moved it under its own umbrella; the fork was started by Aleksa Sarai and is led and maintained by many of the same developers who originally created LXD, now under the Linux Containers project alongside LXC and LXCFS (<a name="apa-lc-nd-a-citation-2"></a>[Linux Containers, n.d.-a](#apa-lc-nd-a-reference)). It is released under the Apache 2.0 licence and is explicitly free of any contributor licence agreement (<a name="apa-incus-nd-citation-2"></a>[Incus Project, n.d.](#apa-incus-nd-reference)).

Architecturally, Incus manages system containers and application containers through LXC and virtual machines through QEMU, presenting a single unified experience built around a REST API and a consistent command-line client. It is image-based, supports a large range of Linux distributions, and scales from a single instance on one machine to a cluster across a data-centre rack. A migration tool, `lxd-to-incus`, allows existing LXD users to move across.

Its primary participants are system administrators, operators, and homelab and platform engineers. Its strengths are a coherent management surface, active and frequent releases, responsive security handling, and a strong community governance posture. Its documented limitation is a learning curve that can be steep, together with the operational depth that comes with storage, networking, and clustering decisions.

## 2. Evaluation

### 2.1 Dimension one — accessibility and inclusion

**Score: 2 — Partial.**

Incus makes real, maintained efforts to lower the barrier to entry: a hosted "try it online" sandbox lets a newcomer use the system before installing anything, an interactive initialisation flow guides first setup, unprivileged-user delegation through the `incus-admin` group lets people work without running everything as root, and the documentation provides a structured first-steps tutorial. These are genuine reductions in the prior the system assumes. They are incomplete, however. There is no published accessibility-conformance work for people with disabilities, documentation is effectively English-only, and the learning curve for the full system is consistently described as steep. The effort addresses the onboarding of specialists more than it addresses the breadth of human need that <a name="apa-treviranus-2018-citation-2"></a>[Treviranus (2018)](#apa-treviranus-2018-reference) argues inclusive design must centre, which holds the score at two.

- *Can people with disabilities fully participate in the project's primary workflows?* There is no published evidence of accessibility-conformance work, so this cannot be affirmed.
- *Is cognitive load minimised through consistent structure, clear language, and reduced interface complexity?* Partially; the unified CLI, interactive init, and try-it-online sandbox reduce it, but the full system remains demanding.
- *Is the system usable by people without specialist domain knowledge, or does it assume a narrow prior?* It assumes a system-administration prior, though less steeply at the entry point than at depth.
- *Are accessibility concerns addressed at the design stage or retrofitted after problems are reported?* Onboarding accessibility is addressed by design; disability and language accessibility are not visibly addressed at all.

### 2.2 Dimension two — language as infrastructure

**Score: 1 — Acknowledged.**

Incus treats its machine-facing interface language as a structural concern — the unified, consistent CLI and REST API are a deliberate improvement in learnability and a real point in the project's favour. But the dimension concerns human communication and the breadth of who can participate, and on that axis the response is minimal. Documentation and community channels are primarily English, and there is no structural internationalisation programme that makes participation in the core workflows equally available across languages.

- *Is language treated as a structural concern that shapes participation, or as a layer applied after decisions are made?* The interface language is structural; human natural language is treated as a presentation layer.
- *Can people participate in core workflows in languages other than English?* Not in any structurally supported way within the official project.
- *Does communication design widen or narrow the pool of potential contributors?* The consistent interface widens it for English-speaking specialists; the English-only documentation narrows it otherwise.

### 2.3 Dimension three — feedback loops

**Score: 4 — Embedded.**

This is a strength, and its clearest expression is the project's own origin: when governance of the upstream project drifted toward single-company control, the community detected the change and corrected it by forking — a large-scale balancing response of exactly the kind <a name="apa-meadows-2008-citation-2"></a>[Meadows (2008)](#apa-meadows-2008-reference) describes. Day to day, the project runs mature feedback structures: a public issue tracker, a community support forum, frequent releases on a regular cadence, and visibly responsive security handling (<a name="apa-lc-nd-b-citation-2"></a>[Linux Containers, n.d.-b](#apa-lc-nd-b-reference)). The score stops short of five because the well-developed feedback is technical and contributor-facing; feedback specifically about who is excluded from participation is less visible.

- *Does the project gather feedback on what is working, and act on it?* Yes, through issues, the forum, and a steady release cadence.
- *Are there reinforcing loops that strengthen contribution over time?* Yes; community adoption and contribution have compounded since the fork.
- *Are there balancing loops that stabilise the system and prevent drift?* Yes; the fork itself was a balancing correction, and ongoing review and security response stabilise the system.
- *Does the project surface unintended consequences and emergent harms?* Partially; technical and security regressions are surfaced well, participation harms less so.

### 2.4 Dimension four — cognitive and operational sustainability

**Score: 3 — Developing.**

Operationally, Incus is sustainable across a wide range of scales: a single coherent daemon and client, interactive initialisation, a documented scaling path from one machine to a cluster, and a migration tool that lets existing users move without rebuilding (<a name="apa-incus-nd-citation-3"></a>[Incus Project, n.d.](#apa-incus-nd-reference)). The project itself is sustainably maintained by a broad team with a regular release rhythm, and a commercial-support option exists for organisations that need it. What keeps the score at developing rather than embedded is the cognitive load at depth — storage, networking, and clustering decisions carry real complexity, and the learning curve is steep beyond first steps — so operational burden, while better bounded than in many comparable tools, still grows meaningfully with ambition.

- *Does the system adapt to human cognitive limits, or force humans to adapt to it?* It meets newcomers partway through interactive tooling, but depth requires the human to adapt.
- *Is operational burden bounded and predictable over time?* More so than in many peers, owing to the unified surface, though it grows with clustering and storage complexity.
- *Is the project maintainable without depending on a small number of overloaded individuals?* Yes; maintenance is distributed and a commercial-support path exists.
- *Is unnecessary complexity actively reduced?* Partially; the unified experience reduces it, but essential operational complexity remains.

### 2.5 Dimension five — relationship-centered design

**Score: 3 — Developing.**

Incus designs the relationship between the human and the system more deliberately than many infrastructure tools. The unified experience across containers and virtual machines, presented through one consistent client and API, is an explicit choice to optimise the human-facing surface rather than only the components beneath it. The `lxd-to-incus` migration tool is a relationship-respecting decision toward existing users, refusing to strand the people who depended on the upstream project, and the absence of any contributor licence agreement lowers the friction of the contributor relationship (<a name="apa-incus-nd-citation-4"></a>[Incus Project, n.d.](#apa-incus-nd-reference)). Community relationships are supported through an active forum and clear support channels (<a name="apa-lc-nd-b-citation-3"></a>[Linux Containers, n.d.-b](#apa-lc-nd-b-reference)). The score stops short of embedded because graphical and non-expert interaction remains thinner than the command-line and API surface, so the relational quality that <a name="apa-engel-1977-citation-2"></a>[Engel (1977)](#apa-engel-1977-reference) would locate in the newcomer's lived experience is still developing.

- *Does the design optimise relationships between components and participants, not only isolated parts?* Yes for components and for existing users; for non-expert participants, partially.
- *Is human-system interaction treated as a first-class design concern?* Yes at the level of the unified client and API; less so for graphical or non-expert interaction.
- *Are community relationships supported structurally?* Yes, through the forum, support channels, and a CLA-free contribution model.

### 2.6 Dimension six — knowledge infrastructure

**Score: 4 — Embedded.**

Incus maintains structured, discoverable documentation organised into tutorials, how-to guides, reference, and explanation, including a first-steps tutorial, a security-considerations guide, and a support page that names every community and commercial channel (<a name="apa-lc-nd-b-citation-4"></a>[Linux Containers, n.d.-b](#apa-lc-nd-b-reference)). The repository is public, release announcements are archived on the community forum, and the project's governance posture is transparent by design. Knowledge is distributed across a broad maintainer base and supplemented by independent distribution documentation. The score stops short of five because the density of the material, and its reliance on a single language, link this dimension's ceiling to the weaknesses in dimensions one and two.

- *Is knowledge documented, discoverable, and preserved over time?* Yes, in a well-structured documentation set with archived release history.
- *Are governance and decision records transparent and accessible?* Yes; transparency of governance is central to the project's identity.
- *Can newcomers reconstruct the reasoning behind the system's design?* Largely, though density raises the effort required.
- *Is knowledge distributed or centralised in a few people?* Distributed across the maintainer team and the wider community.

### 2.7 Dimension seven — ethical orientation

**Score: 4 — Embedded.**

This is the dimension on which Incus is most distinctive. The project's founding act was an ethical one: a response to the concentration of control over a shared community resource. That orientation is embedded in structure rather than stated as aspiration — community governance under the Linux Containers project, a permissive Apache 2.0 licence, the deliberate absence of any contributor licence agreement, and a migration tool that gives users a route away from concentrated control (<a name="apa-lc-nd-a-citation-3"></a>[Linux Containers, n.d.-a](#apa-lc-nd-a-reference); <a name="apa-incus-nd-citation-5"></a>[Incus Project, n.d.](#apa-incus-nd-reference)). On the evaluator's questions of who controls the system and whether it increases empowerment or dependency, Incus answers about as well as a project of this kind can. What holds it short of five is the same blind spot seen across this genre: the expertise barrier functions as an unexamined assumption about who counts as a legitimate participant, and that assumption is not addressed in the project's stated values.

- *Who benefits, and who is excluded?* Operators and the community benefit; non-specialists remain excluded by the expertise barrier.
- *Who controls the system, and does control concentrate or distribute?* Control is distributed by deliberate design; resisting concentration is the project's founding purpose.
- *Does the system increase empowerment or dependency?* Empowerment, through community governance, permissive licensing, and migration freedom.
- *Are incentives aligned with long-term health rather than short-term extraction?* Yes; the CLA-free community model is structured against capture.

### 2.8 Dimension eight — emergence and adaptability

**Score: 4 — Embedded.**

Incus is, in its very existence, an act of adaptation — a system that emerged when the surrounding governance environment changed. That adaptive character continues in its development: a REST-API-based design that supports extension, image support spanning many distributions, scaling from a single host to a clustered deployment, new storage and networking capabilities added at a steady pace, and a cross-platform agent that has grown to cover Linux, Windows, and macOS (<a name="apa-incus-nd-citation-6"></a>[Incus Project, n.d.](#apa-incus-nd-reference)). The project favours revisable, evolving capability over rigidity, which is the posture <a name="apa-meadows-2008-citation-3"></a>[Meadows (2008)](#apa-meadows-2008-reference) recommends in the face of complexity. The score stops short of five because, as with comparable tools, the cost of adapting a deployment to new storage or networking models falls on the operator, which can make adaptation demanding for those without specialist support.

- *Does the project hold humility about what it cannot predict or control?* Yes; it emerged from, and responds to, conditions it did not control.
- *Is the system designed to support emergence rather than centralise everything?* Yes, through an extensible API and broad image and platform support.
- *Does the project favour resilience and revisability over rigidity?* Yes; steady, iterative evolution is its norm.
- *Can the system evolve without breaking dependent participants?* Mostly, aided by migration tooling, though adaptation cost falls on operators.

## 3. Profile and interpretation

The eight scores are: accessibility and inclusion, 2; language as infrastructure, 1; feedback loops, 4; cognitive and operational sustainability, 3; relationship-centered design, 3; knowledge infrastructure, 4; ethical orientation, 4; and emergence and adaptability, 4.

The profile describes a project in strong systemic health. Six of the eight dimensions sit at three or four, and the project is unusual in that its highest-scoring dimension is ethical orientation — the dimension on which infrastructure projects most often score lowest. The reason is structural rather than rhetorical: Incus's founding act was itself a Universal Cake-style move, a community responding to the concentration of control over a shared resource by building a more distributed and revisable alternative (<a name="apa-lc-nd-a-citation-4"></a>[Linux Containers, n.d.-a](#apa-lc-nd-a-reference)). That same impulse shows up as the strength in feedback loops, in emergence and adaptability, and in the relationship-respecting care of the migration path.

The two lowest-scoring dimensions, language as infrastructure and accessibility and inclusion, are the structural leverage points. <a name="apa-meadows-2008-citation-4"></a>[Meadows (2008)](#apa-meadows-2008-reference) argues that the most powerful interventions change information flows and goals rather than optimising existing components, and these two dimensions are where the project's implicit goal — to serve specialist operators well — sets a ceiling on the dimensions adjacent to them. The density that holds knowledge infrastructure at four, and the unexamined expertise barrier that holds ethical orientation at four, both trace back to the same assumption about who the system is for. Raising the language and accessibility scores would tend to lift the dimensions coupled to them, which is the signature of a genuine leverage point.

It is worth recording the contrast with the Prometheus evaluation, since both are infrastructure tools serving specialist operators. The two share the same shape — strong on machine-facing and community dimensions, weak on the human-inclusion axis — but Incus sits higher on the human-facing dimensions (cognitive sustainability and relationship-centered design at three rather than two) and markedly higher on ethical orientation, because resisting the concentration of control is its founding purpose rather than an incidental property. In Universal Cake terms, Incus has internalised more of the praxis at the level of governance and community than is typical, while leaving the same final gap: it has not yet treated the human participant's language and access needs as part of the system it is responsible for.

## 4. Recommended interventions

The following interventions address every dimension scoring two or below. Each describes a structural change, not merely an aspiration.

**Language as infrastructure (1).** Establish a structural translation pathway for the core getting-started and conceptual documentation, with a maintained source-of-truth and a contribution process for translators, so that language ceases to be a presentation layer applied after the fact and becomes maintained infrastructure that widens the contributor and user pool beyond English speakers. The project's existing strength in interface consistency makes this a smaller step than it would be for a less coherent codebase.

**Accessibility and inclusion (2).** Build on the onboarding work already in place — the try-it-online sandbox, interactive init, and unprivileged delegation — by adding two things the current effort lacks. First, adopt a published accessibility-conformance target for the web-facing documentation and any graphical interface, and add it to the contribution and release criteria so it is gated by design rather than retrofitted. Second, extend the layered documentation path so that a non-specialist can reach a first working instance without first absorbing the full storage, networking, and clustering model, lowering the prior the system assumes at depth rather than only at the entry point.

## 5. Resources

- Incus introduction: https://linuxcontainers.org/incus/
- Incus documentation: https://linuxcontainers.org/incus/docs/main/
- First steps with Incus: https://linuxcontainers.org/incus/docs/main/tutorial/first_steps/
- Incus support and community channels: https://linuxcontainers.org/incus/docs/main/support/
- Incus repository: https://github.com/lxc/incus
- `universal-cake-praxis-evaluator-v0.1.0.md` — the framework applied in this evaluation.

## 6. References

<a name="apa-bertalanffy-1968-reference"></a>
von Bertalanffy, L. (1968). *General system theory: Foundations, development, applications*. George Braziller.

[Return to citation](#apa-bertalanffy-1968-citation)

---

<a name="apa-engel-1977-reference"></a>
Engel, G. L. (1977). The need for a new medical model: A challenge for biomedicine. *Science, 196*(4286), 129–136. https://doi.org/10.1126/science.847460

[Return to citation](#apa-engel-1977-citation) · [Return to citation](#apa-engel-1977-citation-2)

---

<a name="apa-freire-1970-reference"></a>
Freire, P. (1970). *Pedagogy of the oppressed*. Continuum.

[Return to citation](#apa-freire-1970-citation)

---

<a name="apa-incus-nd-reference"></a>
Incus Project. (n.d.). *Incus* [Computer software]. GitHub. Retrieved June 8, 2026, from https://github.com/lxc/incus

[Return to citation](#apa-incus-nd-citation) · [Return to citation](#apa-incus-nd-citation-2) · [Return to citation](#apa-incus-nd-citation-3) · [Return to citation](#apa-incus-nd-citation-4) · [Return to citation](#apa-incus-nd-citation-5) · [Return to citation](#apa-incus-nd-citation-6)

---

<a name="apa-lc-nd-a-reference"></a>
Linux Containers. (n.d.-a). *Incus: Introduction*. Retrieved June 8, 2026, from https://linuxcontainers.org/incus/

[Return to citation](#apa-lc-nd-a-citation) · [Return to citation](#apa-lc-nd-a-citation-2) · [Return to citation](#apa-lc-nd-a-citation-3) · [Return to citation](#apa-lc-nd-a-citation-4)

---

<a name="apa-lc-nd-b-reference"></a>
Linux Containers. (n.d.-b). *Support — Incus documentation*. Retrieved June 8, 2026, from https://linuxcontainers.org/incus/docs/main/support/

[Return to citation](#apa-lc-nd-b-citation) · [Return to citation](#apa-lc-nd-b-citation-2) · [Return to citation](#apa-lc-nd-b-citation-3) · [Return to citation](#apa-lc-nd-b-citation-4)

---

<a name="apa-meadows-2008-reference"></a>
Meadows, D. H. (2008). *Thinking in systems: A primer*. Chelsea Green Publishing.

[Return to citation](#apa-meadows-2008-citation) · [Return to citation](#apa-meadows-2008-citation-2) · [Return to citation](#apa-meadows-2008-citation-3) · [Return to citation](#apa-meadows-2008-citation-4)

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

- **0.1.0** — Initial evaluation of Incus against the eight Universal Cake praxis dimensions. Scores grounded in public documentation, governance, and repository; evidentiary basis and reconstructed diagnostic questions for dimensions three through eight noted as qualifications in the Sources and Acknowledgements section. Title and filename follow the `<product-or-service>--<evaluation-type>` pattern.

# Prompt -- Universal Cake Praxis Evaluation

Version: 0.1.1
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md

## Abstract

This document defines the prompt used to produce a Universal Cake praxis evaluation for a given subject. It specifies the context the collaborator requires, the inputs the author must supply, the output format expected, and the authoring process that governs the session. The evaluation produced by this prompt is a technical document conforming to `web-ready-unrendered-markdown-using-apa-7-v0.2.2.md` and the style guide referenced above. This document is intended to be provided to a collaborator — human or AI — at the start of any session in which a praxis evaluation is being produced or continued.

## Sources and Acknowledgements

The evaluation framework this prompt produces output for is defined in <a name="apa-evaluator-citation"></a>[Steel (2026a)](#apa-evaluator-reference), *Universal Cake praxis evaluator*. The Universal Cake systems praxis on which that framework is based is described in <a name="apa-steel-2026-citation"></a>[Steel (2026b)](#apa-steel-2026-reference). Document formatting follows the <a name="apa-markdown-citation"></a>[web-ready unrendered markdown using APA 7 specification (Steel, 2026c)](#apa-markdown-reference). Authoring conventions follow the <a name="apa-styleguide-citation"></a>[style guide for technical documentation for technologists (Steel, 2026d)](#apa-styleguide-reference).

## 1. Purpose

This prompt exists to make evaluation sessions reproducible, consistent across collaborators, and coherent with the broader Universal Cake documentation ecosystem. A praxis evaluation is a technical document. It must be produced with the same rigour applied to any other governed document in this ecosystem: one paragraph at a time, agreed before proceeding, in the author's voice, conforming to the markdown specification, and grounded in the eight dimensions defined in the evaluator.

The prompt is not a shortcut. It is the minimum context a collaborator needs to begin a session without requiring the author to re-establish conventions, re-explain the framework, or correct formatting errors after the fact.

## 2. Required inputs

Before beginning an evaluation session, the author must supply the following:

- A description of the subject being evaluated, sufficient for a technical peer to understand its purpose, scope, and current state.
- Any existing documentation for the subject — architecture documents, README files, governance documents, contribution guides, radar entries, or previous evaluations — that the collaborator should treat as source material.
- The current version of this document.
- The current version of `universal-cake-praxis-evaluator-v0.1.0.md`.
- The current version of `web-ready-unrendered-markdown-using-apa-7-v0.2.2.md`.
- The current version of `style-guide--technical-documentation-for-technologists-v0.2.0.md`.

If a previous evaluation exists for the subject, it must also be supplied. The collaborator will treat it as the document being continued, not as background reading.

## 3. The prompt

The following prompt is provided verbatim to the collaborator at the start of each evaluation session. The author replaces the bracketed fields before sending.

---

```text
You are a technical collaborator helping to produce a Universal Cake praxis evaluation.
The following documents govern this session. Read all of them before responding.

1. style-guide--technical-documentation-for-technologists-v0.2.0.md — governs voice,
   register, structure, and authoring process for all documents in this session.

2. web-ready-unrendered-markdown-using-apa-7-v0.2.2.md — governs all markdown
   formatting, citation practice using the CAP workflow, document structure, and
   file naming.

3. universal-cake-praxis-evaluator-v0.1.0.md — defines the eight dimensions against
   which the subject will be evaluated, the scoring rubric, and the interpretation
   framework. This document is the evaluative framework. Do not summarise it or
   reproduce it. Apply it.

The subject being evaluated is:

[AUTHOR: insert subject name and category, a one-paragraph description of the
subject's purpose and current state, and any relevant context about its audience,
contributors, and known strengths or limitations.]

The following source materials describe the subject:

[AUTHOR: attach or paste any documentation here — README, architecture documents,
governance documents, contribution guides, radar entries, existing evaluations, or
other relevant material. If none exists, say so explicitly.]

The output of this session is a web-ready unrendered markdown document conforming to
web-ready-unrendered-markdown-using-apa-7-v0.2.2.md. The document structure is:

1. Title and version block
2. Abstract
3. Sources and Acknowledgements
4. Body sections (numbered), containing:
   a. Project summary — a concise description of the subject as understood from the
      source materials, written in third person.
   b. Evaluation — one section per dimension, each containing: the score awarded,
      the evidence or reasoning behind the score, and the diagnostic questions from
      the evaluator with explicit responses grounded in the source materials.
   c. Profile and interpretation — a synthesis of the eight scores, identifying the
      lowest-scoring dimensions as structural leverage points, examining relationships
      between dimensions, and stating what the profile indicates about the subject's
      current systemic health.
   d. Recommended interventions — for each dimension scoring two or below, a concrete
      structural recommendation. Recommendations must describe what changes, not just
      that something should change.
5. Resources
6. References (APA 7, CAP workflow)
7. Changelog

Authoring process:

Work one section at a time. Present each section for agreement before proceeding to
the next. Correct spelling and grammar without changing the author's voice or intent.
Flag factual uncertainties as questions before applying them. Do not produce the full
document in one pass.

Begin by presenting the project summary section for the author's review. Do not
proceed to the evaluation sections until the summary is agreed.
```

---

## 4. Authoring process for this session

The collaborator begins with the project summary and waits for agreement before proceeding. Each subsequent section follows the same pattern. The author may revise any paragraph before the session moves on. The collaborator does not produce the full document in one pass.

When the evaluation sections are reached, the collaborator applies the scoring rubric from `universal-cake-praxis-evaluator-v0.1.0.md` directly to the source materials supplied. Scores are not negotiated upward. If the source materials do not provide evidence for a higher score, the collaborator states this explicitly and the author may supply additional context or accept the score as given.

The collaborator flags any dimension where the source materials are insufficient to support a confident score. In those cases the collaborator states what evidence would be needed and assigns a provisional score pending the author's response.

When scoring dimension one — accessibility and inclusion, the collaborator must directly test the documentation or interfaces under evaluation rather than hedging on the basis of untested assumptions. At minimum this means: testing keyboard navigation on the documentation site, noting the technology stack the documentation site is built on where this is publicly available, and comparing the accessibility of the documentation surface to known alternatives. An assumption that something has not been tested is not a substitute for testing it. Where the source repository for the subject's documentation site is publicly available, the collaborator should inspect it — the technical choices made in building the documentation site are themselves evidence relevant to accessibility, knowledge infrastructure, and relationship-centred design dimensions.

## 5. Output file naming

Output documents are named using the following convention:

`[subject-name-slug]--[subject-category-slug]--universal-cake-praxis-evaluation-v0.1.0.md`

The subject-name-slug is derived from the name of the subject being evaluated — lowercase, hyphen-separated, no spaces or underscores. The subject-category-slug identifies the category of subject being evaluated. Where a radar entry exists for the subject, both slugs must match the radar entry filename exactly.

The following subject category slugs are defined. This list is non-exhaustive; new categories are defined as needed using the same lowercase hyphen-separated convention:

- `web-server`
- `static-site-generator`
- `orchestration-tool`
- `provisioning-approach`
- `deployment-approach`
- `hosting-provider`
- `version-control-approach`
- `tls-management`
- `build-tool`
- `content-management-system`

Examples:

- `caddy--web-server--universal-cake-praxis-evaluation-v0.1.0.md`
- `hugo--static-site-generator--universal-cake-praxis-evaluation-v0.1.0.md`
- `git-based-deployment--deployment-approach--universal-cake-praxis-evaluation-v0.1.0.md`
- `digital-ocean--hosting-provider--universal-cake-praxis-evaluation-v0.1.0.md`
- `ansible--orchestration-tool--universal-cake-praxis-evaluation-v0.1.0.md`

The subject-name-slug is agreed with the author before the session begins. If the subject name contains words that would make the slug ambiguous or excessively long, the author agrees a shortened slug before proceeding.

## 6. Versioning the evaluation document

Evaluation documents are versioned independently of the evaluator framework document. An initial evaluation begins at `0.1.0`. Subsequent evaluations of the same subject that add new sections or revise scores based on changed conditions increment the minor version. Corrections to spelling, grammar, or formatting without change to scores or conclusions increment the patch version. A re-evaluation conducted after a major change that substantially revises the profile increments the major version.

## Resources

### Governing documents
- [Universal Cake praxis evaluator](#apa-evaluator-reference)
- [Web-ready unrendered markdown using APA 7](#apa-markdown-reference)
- [Style guide for technical documentation for technologists](#apa-styleguide-reference)

### Universal Cake source material
- [Steel (2026b). Universal Cake as systems theory and systems praxis](#apa-steel-2026-reference)

## References

<a name="apa-evaluator-reference"></a>Steel, C. (2026a). *Universal Cake praxis evaluator* (Version 0.1.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-evaluator-citation)

<a name="apa-steel-2026-reference"></a>Steel, C. (2026b). *Universal Cake as systems theory and systems praxis* (Version 1.0) [Working paper]. https://universalcake.ca
[Return to citation](#apa-steel-2026-citation)

<a name="apa-markdown-reference"></a>Steel, C. (2026c). *Web-ready unrendered markdown using APA 7* (Version 0.2.2) [Technical document]. https://universalcake.ca
[Return to citation](#apa-markdown-citation)

<a name="apa-styleguide-reference"></a>Steel, C. (2026d). *Style guide: Technical documentation for technologists* (Version 0.2.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-styleguide-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.1 | Draft | Added accessibility testing requirement to section 4; updated output file naming convention in section 5 to subject-leading with category slug, with defined category list and examples; updated markdown spec reference to v0.2.2; generalised language from "project" to "subject" throughout to reflect that evaluations apply to tools, approaches, providers, and other subjects beyond software projects |
| 0.1.0 | Draft | Initial draft |

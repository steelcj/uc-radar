# Risk Assessment Interview Notes

Version: 0.1.0 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document records the interview conducted on 2026-08-12 to gather the source material for the Phase 1 risk assessment under the European Union AI Act. It captures the AI tool inventory as reported by the developer, the output destinations for each tool, the findings on Article 50 applicability, and the distinction between long-standing Universal Cake policy and measures taken for compliance. These notes are the raw record. The assessed conclusions drawn from them are recorded separately in the Phase 1 risk assessment report.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. The text of Article 50 is taken from <a name="eu-ai-act-text-citation"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference). The reading of the public interest threshold and the human review exemption follows <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference).

## Interview record

| Field        | Value                                        |
| ------------ | -------------------------------------------- |
| Project      | EU AI Act compliance, Phase 1                |
| Date         | 2026-08-12                                   |
| Interviewer  | Euria (Infomaniak)                           |
| Interviewee  | Christopher Steel, sole developer            |
| Organization | Universal Cake                               |
| Duration     | Approximately 2 hours                        |
| Complexity   | Low to medium                                |

## Context and scope

The team is a single developer operating as a sole proprietorship. AI is used strictly as an internal development tool. No AI features are embedded in customer-facing applications, and end users do not interact directly with AI models.

Output reaches four destinations: public documentation covering websites and guides, GitHub repositories holding code and technical documentation, business and professional emails, and internal team notes.

There are no current customers in the EU. Planned global expansion includes potential EU clients. The assessment was triggered by a decision to establish a compliance position before the v1.0.0 release and before any EU market entry, rather than by an incident or an enquiry.

## AI tool inventory

The following tools are actively used in the workflow.

| Tool                                   | Primary use case                                         | Output destination                    | Human review    |
| -------------------------------------- | -------------------------------------------------------- | ------------------------------------- | --------------- |
| ChatGPT                                | Documentation, code generation, research, email drafting | Public docs, GitHub, websites, emails | Yes, substantive |
| Claude                                 | Documentation, code generation, research, email drafting | Public docs, GitHub, websites, emails | Yes, substantive |
| Euria                                  | Research, documentation support                          | Public docs, GitHub                   | Yes, substantive |
| Lumo                                   | Documentation, code, research, emails                    | Public docs, GitHub, websites, emails | Yes, substantive |
| Browser AI (Brave, Chrome, DuckDuckGo) | Quick research, correspondence                           | Emails, personal notes                | Yes, mixed      |
| CI/CD pipeline code                    | Automated website generation and deployment              | Customer-facing websites              | Yes, pre-deployment |

## Findings on Article 50 obligations

The interview opened on the concern that AI-generated text published online might require a "created by AI" label under Article 50(4).

**Content scope.** The obligation applies only to text published for the purpose of informing the public on matters of public interest. The European Commission reads that threshold as covering politics and democratic processes, public administration, justice, fundamental rights, public security, public health, environmental protection, and consumer safety. Technical software documentation and source code do not fall within that category, so the obligation does not attach on this ground.

**Human review.** Independently, and even if the scope did apply, Article 50(4) disapplies the obligation where the content has undergone substantive human review or editorial control and where a natural or legal person holds editorial responsibility for the publication. Both conditions are met. Christopher Steel holds editorial responsibility, and all content is reviewed before publication.

## Risk classification

| Dimension          | Finding                                                     |
| ------------------ | ----------------------------------------------------------- |
| Status             | Deployer, not provider                                      |
| Risk level         | Medium                                                      |
| Geographic trigger | No EU users, but global website accessibility applies       |

Risk is assessed as medium because AI-assisted content does reach the public, while the exposure is mitigated by human review and by the non-political nature of the material. The geographic trigger is recorded as potential rather than active: there are no EU users today, but a globally accessible website means the Act could apply.

## Existing policies and compliance measures

The interview drew a distinction that matters to the exemption position, and it is recorded here as reported.

**Human review policy.** Universal Cake established this before adopting AI and before the EU AI Act existed. The organization has always required human review of all published content. This is not a new compliance measure. It is a standing quality control practice that happens to satisfy the Article 50(4) exemption. The policy is open-ended and continues after the v1.0.0 release.

**Transparency policy.** The team voluntarily includes footer disclosures on public documentation acknowledging AI assistance. This is a project policy commitment, not a legal requirement under the current assessment.

## Action items

| Priority | Action                               | Owner    | Deadline      | Notes                                                                       |
| -------- | ------------------------------------ | -------- | ------------- | --------------------------------------------------------------------------- |
| High     | Document the editorial review process | C. Steel | Week 3        | Critical exemption evidence. Must show review is substantive, not spell-checking |
| High     | Implement footer disclosure           | C. Steel | Before v1.0.0 | Project policy, not legal mandate. Standardize wording across documents     |
| Medium   | Document vendor compliance            | C. Steel | Week 3        | Collect terms of service and data handling policies for all AI tools used   |
| Medium   | Legal consultation                    | C. Steel | Week 3        | Brief review to confirm the technical documentation exemption reading       |
| Low      | Monitor EU AI Office guidance         | C. Steel | Ongoing       | Set up alerts for new FAQs or Codes of Practice                             |

## Open questions

- Should the substantive review standard be formalized as a checklist, naming the specific criteria checked before publishing?
- Should footer disclosure wording be standardized across all projects, for example "Drafted with AI assistance, reviewed by [name]"?
- Are there specific CI/CD logs that should be retained to evidence human approval before deployment?

## Next steps

1. Draft the editorial review process document.
2. Finalize the footer disclosure template.
3. Compile vendor compliance data into a single audit file.
4. Schedule legal review prior to the v1.0.0 launch.

These notes record the interview as conducted. They remain subject to the legal verification scheduled in Phase 2.

## License

This document, *Risk Assessment Interview Notes*, by **Christopher Steel**, with AI assistance from **Euria (Infomaniak)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Regulatory guidance

- [AI Office Guidelines](#eu-ai-act-article50-reference)
- [Article 50 full text](#eu-ai-act-text-reference)
- [Commission FAQ on Article 50](#ec-faq-reference)

## References

<a name="eu-ai-act-article50-reference"></a>European Commission. (2026a). Guidelines on transparency obligations for providers and deployers of AI systems. European Commission. https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations Return to citation

<a name="ec-faq-reference"></a>European Commission. (2026b). Transparency obligations under Article 50 of the AI Act [Frequently asked questions]. Shaping Europe's Digital Future. https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act Return to citation

<a name="eu-ai-act-text-reference"></a>EU Artificial Intelligence Act. (2026). Article 50: Transparency obligations for providers and deployers of certain AI systems. https://artificialintelligenceact.eu/article/50/ Return to citation

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.1.0   | Draft  | Initial creation from the 2026-08-12 interview, formatted to the web-ready APA 7 guide: added the version block, abstract, sources, license, resources, references, and changelog; removed horizontal rules and heading numbers per the markdown automa defaults; converted asterisk bullets to hyphens; converted the hard-wrapped metadata header to a table; reduced decorative bold to defined-term labels |

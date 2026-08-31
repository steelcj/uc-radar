# EU AI Act Risk Assessment Report, Phase 1, COMPLETE

Version: 0.2.1 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document records the risk assessment results for a small development team that uses artificial intelligence to create applications and documents. The assessment maps all AI tools in use, identifies output destinations, evaluates applicable exemptions, and assigns a risk classification under the European Union AI Act Article 50 transparency obligations. The team uses AI as an internal development tool only. End products do not include AI features for users. All public content undergoes human review before publication.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. Implementation guidance follows <a name="ssl-compliance-citation"></a>[SSL.com (2026)](#ssl-compliance-reference). Compliance best practices derive from <a name="neuromarketing-guide-citation"></a>[Institute for Neuromarketing (2026)](#neuromarketing-guide-reference).

## Assessment context

| Metric            | Value              |
| ----------------- | ------------------ |
| Interview date    | 2026-08-12         |
| Team size         | 1 (sole developer) |
| Timeline estimate | 2 hours            |
| Complexity        | Low-medium         |

The assessment uses controlled language principles from ASD-STE100. Sentences stay short. Terms stay consistent. Instructions specify actions clearly. Ambiguity gets avoided wherever possible.

## AI tool inventory

| Tool                                   | Purpose                               | Output destination                             | Human review | Public exposure |
| -------------------------------------- | ------------------------------------- | ---------------------------------------------- | ------------ | --------------- |
| ChatGPT                                | Documentation, code, research, emails | Public docs, GitHub, websites, business emails | Yes          | Yes             |
| Claude                                 | Documentation, code, research, emails | Public docs, GitHub, websites, business emails | Yes          | Yes             |
| Euria                                  | Research, documentation               | Public docs, GitHub                            | Yes          | Yes             |
| Lumo                                   | Documentation, code, research, emails | Public docs, GitHub, websites, business emails | Yes          | Yes             |
| Browser AI (Brave, Chrome, DuckDuckGo) | Research, correspondence              | Personal and business emails                   | Yes          | Mixed           |
| Code in CI/CD pipelines                | Website generation, deployment        | Customer-facing websites                       | Yes          | Yes             |

## Output classification

| Content type                        | Exposure level | Disclosure required by Article 50 | Disclosure by project policy |
| ----------------------------------- | -------------- | --------------------------------- | ---------------------------- |
| Public documentation                | High           | No, exempt under 50(4)            | Yes                          |
| GitHub repositories (code and docs) | High           | No, exempt under 50(4)            | Yes                          |
| Business and professional emails    | Medium         | No                                | Optional                     |
| Personal correspondence             | Low            | No                                | No                           |
| Customer-facing applications        | Medium         | No, AI not in product             | No                           |
| Internal team documentation         | Low            | No                                | No                           |

Public documentation and GitHub repositories carry high exposure. Business emails carry medium exposure because recipients vary. Personal correspondence stays low. Customer-facing applications use AI only during development, so the applications themselves do not trigger disclosure for users.

No content type in this table carries a mandatory Article 50 disclosure obligation, for the reasons set out under Exemption analysis below. The final column records where the team discloses as a matter of project policy regardless.

## Exemption analysis

| Exemption                   | Applicability  | Status                                     |
| --------------------------- | -------------- | ------------------------------------------ |
| Pure development tooling    | Partial        | AI used internally but outputs go public   |
| Human editorial control     | Applicable     | All public content reviewed before release |
| Non-public-interest content | Applicable     | Technical docs, not political commentary   |
| Obvious AI interactions     | Not applicable | AI not exposed in end products             |

Exemption summaryinfo

The Article 50(4) text disclosure obligation does not attach to this team's published content. Two independent grounds apply.

First, the obligation reaches only text "published with the purpose of informing the public on matters of public interest" per <a name="eu-ai-act-text-citation"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference). The European Commission reads that threshold as covering politics and democratic processes, public administration, justice, fundamental rights, public security, public health, environmental protection, consumer safety, and comparable subjects of public debate, per <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference). Technical software documentation does not fall within that list.

Second, and independently, Article 50(4) provides that the obligation does not apply where the AI-generated content has undergone a process of human review or editorial control and where a natural or legal person holds editorial responsibility for the publication. Both conditions are met. All public content receives substantive review before release, and Christopher Steel holds editorial responsibility. The Commission notes that superficial checks such as spell-checking or grammatical correction do not qualify as human review, so the review process must remain substantive for this ground to hold.

The direction of that second ground matters. Universal Cake maintains an internal policy that all content receives human review before release, and a standing policy of transparency about AI use. Both exist independently of the EU AI Act.

The review process is not merely older than this assessment. Universal Cake refined it before the team began working with AI at all, and all documentation, current and historical, reflects it. The practice therefore cannot be characterised as a compliance measure, because it was established when no AI was in use and no AI regulation was in prospect. The team does not review content in order to qualify for the Article 50(4) exemption; it reviews content because that is how Universal Cake has always published, and the exemption conditions are satisfied as a consequence.

The policy has no end date. Human review of all content continues after the v1.0.0 release, not only through the pre-release period. This is material to the exemption, which holds only for as long as the review practice continues.

Footer disclosure on public documentation follows from the same independent transparency policy, alongside the existing practice of recording AI assistance in document attribution. It is not a compliance obligation and was not adopted in response to Article 50. Downstream documents should describe both policies as project commitments in their own right, never as legal requirements or as measures taken to satisfy the Act.

## Geographic applicability

| Factor                 | Status               | Implication                                      |
| ---------------------- | -------------------- | ------------------------------------------------ |
| Current EU users       | None                 | Act not currently triggered                      |
| Planned global clients | Future EU presence   | Proactive compliance recommended                 |
| Website visitors       | Global, potential EU | Accessibility to EU users triggers applicability |

The team has no current EU customers. However, public website and documentation accessibility from the EU means the Act may already apply. Proactive compliance makes sense before the first EU user arrives.

## Risk classification

Overall risk level

Medium

–Public output

Provider status

No

▲Internal tool use

Deployer status

Yes

–AI content published

EU trigger

Potential

–Global accessibility

The team holds **deployer** status, not provider status. AI tools are used internally to create content that reaches the public. This combination produces a medium risk classification. The risk stays manageable because all public content undergoes human review before release.

## Immediate actions

| Action                                          | Basis          | Priority | Timeline      | Owner     |
| ----------------------------------------------- | -------------- | -------- | ------------- | --------- |
| Record the editorial review process in writing  | Exemption      | High     | Week 3        | Developer |
| Add footer disclosure to public documentation   | Project policy | High     | Before v1.0.0 | Developer |
| Document vendor compliance policies             | Audit trail    | Medium   | Week 3        | Developer |
| Set up monitoring for EU AI Office guidance     | Ongoing        | Low      | Ongoing       | Developer |
| Schedule legal consultation                     | Confirmation   | Medium   | Week 3        | Developer |

The highest priority action is documenting the editorial review process itself. Reliance on the Article 50(4) exemption depends on the review being substantive and on editorial responsibility being held by a named person, so that process is the thing that must be evidenced. Footer disclosure remains a high priority as project policy, but it is not what secures the compliance position.

## Recommendation summary

Scaled approach summaryinfo

For internal development tooling with no customer-facing AI features and human-reviewed output, focus on vendor documentation, internal policies, and monitoring future guidance. Build minimal disclosure infrastructure only when scaling public content publication.

Priority actions for the team include mapping AI usage to understand actual exposure, documenting vendor compliance through contracts, obtaining light legal review for confidence, monitoring official guidance passively, and considering Code of Practice participation only if public content scales significantly. The team should complete these actions before the v1.0.0 release milestone.

## License

This document, *EU AI Act Risk Assessment Report*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Regulatory guidance

- [AI Office Guidelines](#eu-ai-act-article50-reference)
- [Code of Practice](#neuromarketing-guide-reference)

### Compliance resources

- [SSL.com Implementation Guide](#ssl-compliance-reference)

## References

<a name="eu-ai-act-article50-reference"></a>European Commission. (2026a). Guidelines on transparency obligations for providers and deployers of AI systems. European Commission. https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations Return to citation

<a name="ec-faq-reference"></a>European Commission. (2026b). Transparency obligations under Article 50 of the AI Act [Frequently asked questions]. Shaping Europe's Digital Future. https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act Return to citation

<a name="eu-ai-act-text-reference"></a>EU Artificial Intelligence Act. (2026). Article 50: Transparency obligations for providers and deployers of certain AI systems. https://artificialintelligenceact.eu/article/50/ Return to citation

<a name="ssl-compliance-reference"></a>SSL.com. (2026). EU AI Act Article 50: A complete guide to AI transparency compliance. SSL.com. https://www.ssl.com/article/eu-ai-act-article-50-a-complete-guide-to-ai-transparency-compliance Return to citation

<a name="neuromarketing-guide-reference"></a>Institute for Neuromarketing & Intellectual Property. (2026). Labelling AI content. Institute for Neuromarketing. https://neuromarketinginstitut.com/eng/ai-content-transparency Return to citation

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.2.1   | Draft  | Recorded that the editorial review process was refined before the team adopted AI and is reflected in all current and historical documentation, and that the review policy is open-ended rather than scoped to the pre-v1.0.0 period |
| 0.2.0   | Draft  | Corrected the exemption analysis, which stated that Article 50 disclosure applies despite human review; Article 50(4) provides the opposite, and the obligation is further limited to text informing the public on matters of public interest. Reclassified footer disclosure as project policy rather than legal obligation. Split the disclosure column to separate obligation from policy. Reprioritised immediate actions so that documenting the editorial review process, which is what the exemption rests on, ranks above disclosure. Added the Article 50 text and Commission FAQ as references |
| 0.1.1   | Draft  | Simplified the title, which carried a duplicated "Phase 1, Risk Assessment" segment and an em dash, per the markdown automa defaults |
| 0.1.0   | Draft  | Initial creation following APA 7 CAP workflow and ASD-STE100 controlled language principles |
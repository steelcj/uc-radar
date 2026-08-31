# Compliance Scope Memo: EU AI Act Article 50 -- Phase 2 - Legal

Version: 0.2.1 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document prepares for legal consultation regarding EU AI Act Article 50 transparency obligations. It summarizes the Phase 1 risk assessment findings, documents the team's understanding of provider versus deployer status, and lists specific questions for legal counsel. The goal is to confirm regulatory classification and validate exemption applicability. Article 50 became applicable on 2 August 2026, so this consultation confirms a position for obligations already in force rather than preparing for a future deadline.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. Implementation guidance follows <a name="ssl-compliance-citation"></a>[SSL.com (2026)](#ssl-compliance-reference). Compliance best practices derive from <a name="neuromarketing-guide-citation"></a>[Institute for Neuromarketing (2026)](#neuromarketing-guide-reference).

## Team profile

| Fact                    | Status                                                      |
| ----------------------- | ----------------------------------------------------------- |
| Organization type       | Sole developer                                              |
| Employee count          | 1                                                           |
| Development stage       | Pre-v1.0.0                                                  |
| Public content          | GitHub repositories, website documentation, business emails |
| AI usage                | Internal development tool only                              |
| End product AI features | None                                                        |
| Current EU users        | None                                                        |
| Planned markets         | Global (including future EU presence)                       |

The team uses artificial intelligence exclusively during development. Users never interact with AI features in the final applications. All public content undergoes human review before publication.

## Understanding of regulatory status

| Concept                          | Current understanding                              | Confidence |
| -------------------------------- | -------------------------------------------------- | ---------- |
| Provider status                  | No                                                 | High       |
| Deployer status                  | Yes                                                | High       |
| Article 50 applicability         | Potential, via EU website accessibility            | Medium     |
| Matters of public interest       | Not met, technical documentation                   | Medium     |
| Human review exemption, 50(4)    | Satisfied by existing internal review policy       | High       |
| Disclosure requirement           | None arising from Article 50                       | Medium     |
| Disclosure as project policy     | Independent standing policy, predates this review  | High       |

**Provider definition:** Organizations that develop and supply AI systems to the market. This includes creating the AI model itself or integrating it into products offered to users.

**Deployer definition:** Organizations that professionally use AI systems for their operations without supplying the AI system itself to others.

The team believes it qualifies as a deployer, not a provider. AI tools are used internally. The final applications do not include AI features for end users. This distinction determines compliance scope under Article 50.

## Risk assessment summary

| Finding            | Details                                          |
| ------------------ | ------------------------------------------------ |
| Overall risk level | Medium                                           |
| Provider status    | No                                               |
| Deployer status    | Yes                                              |
| EU trigger         | Potential via global website accessibility       |
| Human review       | All public content reviewed before release       |
| Public exposure    | Documentation, GitHub, websites, business emails |

The assessment identified six AI tools in use. ChatGPT, Claude, Euria, Lumo, browser-integrated AI, and CI/CD pipeline automation all generate output that reaches public audiences. Human review processes exist and are the basis of the exemption position set out below.

Working position to be confirmedwarning

The team's working position is that the Article 50(4) text disclosure obligation does not attach to its published content, on two independent grounds. The obligation reaches only text published to inform the public on matters of public interest, a threshold technical documentation does not appear to meet. Separately, Article 50(4) disapplies the obligation where content has undergone human review or editorial control and a natural or legal person holds editorial responsibility, both of which hold here, per <a name="eu-ai-act-text-citation"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference) and <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference).

Counsel should note the direction of that second ground. Universal Cake maintains an internal policy of human review of all content before release, and a standing policy of transparency about AI use. Both exist independently of the EU AI Act and were not adopted to secure an exemption. The exemption conditions are satisfied as a consequence of those policies, not as the reason for them.

Two facts strengthen this and should be put to counsel directly. The editorial review process was refined before the team began working with AI, and all documentation, current and historical, reflects it, so the practice is evidenced across the document history rather than dating from AI adoption. And the policy has no end date: human review continues after the v1.0.0 release, not only through the pre-release period.

An earlier version of this memo stated the opposite of the position above, that disclosure is required even with human review. That statement was wrong and has been corrected. The purpose of the consultation is now to confirm this exemption position rather than to scope a disclosure obligation that was assumed to exist.

## Questions for legal counsel

### Regulatory classification

1. **Does the team qualify as a deployer under Article 3(4) of the AI Act?**
   - Evidence: AI tools purchased from third parties, not developed internally
   - Evidence: Final applications contain no AI features for users
   - Question: Is any aspect of our usage ambiguous under current guidance?
2. **Does Article 50 apply given no current EU customers?**
   - Evidence: Public website accessible globally, including EU
   - Evidence: No actual EU user base documented
   - Question: Does mere website accessibility trigger jurisdiction?
3. **Do GitHub repositories count as 'presenting to the public'?**
   - Evidence: Open-source code generated with AI assistance
   - Evidence: Repository README files contain AI-assisted documentation
   - Question: Are these subject to the same disclosure rules as website content?

### Exemption validation

1. **Does our published content meet the "matters of public interest" threshold at all?**
   - Evidence: Output is technical software documentation and source code
   - Evidence: The Commission's reading covers politics, public administration, justice, fundamental rights, public security, public health, environment, and consumer safety
   - Question: If the threshold is not met, does the Article 50(4) text obligation fall away regardless of the review exemption?
2. **Does human editorial review qualify for exemption under Article 50(4)?**
   - Evidence: All documentation reviewed before publication
   - Evidence: Code review conducted before merge to main branch
   - Evidence: Christopher Steel holds editorial responsibility for publication
   - Evidence: The review process was refined before the team adopted AI, and all documentation, current and historical, reflects it
   - Evidence: The policy is open-ended and continues after v1.0.0
   - Question: What constitutes sufficient review under the Act, given the Commission's statement that spell-checking and grammatical correction do not qualify?
   - Question: What form of record is needed to evidence that review if challenged?
   - Question: Does a review practice predating AI adoption strengthen the exemption position, and is the pre-AI document history useful evidence?
3. **Does pre-v1.0.0 development stage affect compliance obligations?**
   - Evidence: Most projects are experimental versions
   - Evidence: v1.0.0 milestone triggers formal review process
   - Question: Can compliance be delayed until v1.0.0 release?
4. **Are business emails subject to disclosure requirements?**
   - Evidence: AI assists with professional correspondence drafting
   - Evidence: Recipients vary (clients, partners, public contacts)
   - Question: Does one-to-one email require disclosure or only mass communication?

### Practical compliance

1. **What disclosure language satisfies Article 50 requirements?**
   - Request: Sample footer text for documentation
   - Request: Required specificity (mention AI tools? general disclosure?)
   - Question: Can disclosure be generic or must it name specific tools?
2. **Do vendor compliance policies from ChatGPT, Claude, Lumo suffice?**
   - Evidence: Third-party providers should handle watermarking obligations
   - Evidence: Terms of service reference AI Act compliance
   - Question: What documentation should be retained for audit purposes?
3. **Should the team join the Code of Practice signatory process?**
   - Evidence: Voluntary compliance pathway exists
   - Evidence: Provides standardized implementation guidance
   - Question: Recommended for sole developers or only larger organizations?
4. **What records should be kept for audit readiness?**
   - Request: Minimum documentation retention period
   - Request: Specific records required (vendor contracts, review logs, disclosures)
   - Question: How long must compliance documentation be preserved?

Consultation budgetinfo

Two-hour legal consultation costing $500 to $800 should provide adequate coverage for this low-risk deployment. Full compliance engagements exceeding €10,000 are unnecessary.

## Proposed compliance actions pending confirmation

| Action                                        | Condition                        | Timeline      |
| --------------------------------------------- | -------------------------------- | ------------- |
| Maintain human review logs                    | Required to evidence exemption   | Ongoing       |
| Add footer disclosure to public documentation | Project policy, not conditional  | Before v1.0.0 |
| Retain vendor compliance documentation        | Always required                  | Ongoing       |
| Monitor EU AI Office guidance                 | Always recommended               | Quarterly     |
| Join Code of Practice signatory               | If counsel recommends            | Before v1.0.0 |
| Implement automated disclosure in CI/CD       | If counsel recommends            | Week 5-6      |

These actions assume deployer status with minimal public AI-generated content exposure. Human review logs now rank first because the Article 50(4) exemption depends on the review being substantive and on editorial responsibility being attributable. Footer disclosure is retained as project policy and is therefore not contingent on the legal outcome. Legal confirmation will validate or adjust this approach.

## Recommended consultant profile

| Criterion               | Preference                       |
| ----------------------- | -------------------------------- |
| Jurisdiction expertise  | EU AI Act specialist             |
| Company size experience | Startups and small teams         |
| Technical background    | Software development familiarity |
| Engagement format       | Fixed-fee consultation           |
| Budget range            | $500-$800 for initial review     |

Avoid consultants recommending enterprise-scale compliance frameworks. The team needs proportionate advice matching its size and risk profile.

## Next steps after consultation

| Outcome                           | Action                                    |
| --------------------------------- | ----------------------------------------- |
| Confirmed deployer status         | Proceed with disclosure infrastructure    |
| Article 50 confirmed applicable   | Implement footnote disclosure immediately |
| Exemption validated               | Document rationale for audit trail        |
| Additional obligations identified | Update compliance roadmap accordingly     |

Legal confirmation will enable progression to Phase 3 (Technical Audit) and Phase 5 (Disclosure Infrastructure).

## License

This document, *Compliance Scope Memo: EU AI Act Article 50*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

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
| 0.2.0   | Draft  | Corrected the stated position on Article 50(4). The memo previously asserted that disclosure is required even with human review; Article 50(4) provides the opposite, and the obligation is further limited to text informing the public on matters of public interest. Added a counsel question on that threshold, added editorial responsibility as evidence, reordered proposed actions so human review logs rank first, and added the Article 50 text and Commission FAQ as references |
| 0.1.0   | Draft  | Initial creation following APA 7 CAP workflow and ASD-STE100 controlled language principles |
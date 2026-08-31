# Quarterly Compliance Review Template

Version: 0.1.0 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document is the template for the recurring quarterly review of the project's EU AI Act position. It exists because the compliance position is conditional rather than settled: the Article 50(4) exemption holds only for as long as the underlying conditions continue to hold. The review checks those conditions, records regulatory and vendor changes since the previous review, verifies that disclosure infrastructure still functions, and produces a dated record for the audit trail. Copy this template for each review rather than editing it in place.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. The text of Article 50 is taken from <a name="eu-ai-act-text-citation"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference). The reading of the public interest threshold and the human review exemption follows <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference).

## How to use this template

Copy this file to `quarterly-review-YYYY-QN.md` at the top of `eu-ai-act/`, then complete every field. Leave no field blank. Where a check was not performed, record "not performed" and the reason rather than omitting the row, so that gaps in the review are themselves visible in the audit trail.

Every table below ships with empty value columns by design. Do not populate this template itself with findings. A template carrying results from a past review will be mistaken for the current position.

The review is due once per quarter. Schedule it so that one review falls before any planned release, because the pre-release checklist in Phase 5 depends on the exemption position still being current.

## Review metadata

| Field                   | Value |
| ----------------------- | ----- |
| Review period           |       |
| Review date             |       |
| Reviewer                |       |
| Editorial responsibility holder |  |
| Previous review         |       |
| Next review due         |       |

## Exemption position

This is the most important section of the review. The project's position is that the Article 50(4) text disclosure obligation does not attach to its published content, on two independent grounds. Both grounds are conditional on facts that can change. Confirm each one.

| Condition                                                        | Still holds | Evidence | Notes |
| ---------------------------------------------------------------- | ----------- | -------- | ----- |
| Published content remains outside "matters of public interest"    |             |          |       |
| All published content receives substantive human review          |             |          |       |
| Review is more than spell-checking or grammatical correction      |             |          |       |
| A named person holds editorial responsibility for publication     |             |          |       |
| The human review policy remains in force with no end date         |             |          |       |
| No AI features have been added to customer-facing products        |             |          |       |

If any row does not hold, the exemption position has changed and the compliance basis must be reassessed before the next publication. Record this as a critical finding, not a routine action item.

The public interest threshold deserves particular attention as the project grows. The European Commission reads it as covering politics and democratic processes, public administration, justice, fundamental rights, public security, public health, environmental protection, consumer safety, and comparable subjects of public debate. Technical documentation sits outside that list, but content drifts. Check whether anything published this quarter moved toward those subjects.

## Regulatory change

| Check                                                    | Change since last review | Impact | Action |
| -------------------------------------------------------- | ------------------------ | ------ | ------ |
| AI Office guidelines updated                             |                          |        |        |
| Commission FAQ on Article 50 updated                     |                          |        |        |
| Code of Practice status changed                          |                          |        |        |
| Article 50 amended by any instrument                     |                          |        |        |
| National implementing measures introduced                |                          |        |        |
| Enforcement actions reported against comparable deployers |                         |        |        |

Record the date each source was checked, not only whether it changed. An unchecked source and an unchanged source look identical in a table that omits the date.

## Vendor status

| Vendor    | Terms changed | Watermarking status | Documentation current | Action |
| --------- | ------------- | ------------------- | --------------------- | ------ |
| OpenAI    |               |                     |                       |        |
| Anthropic |               |                     |                       |        |
| Proton    |               |                     |                       |        |
| Infomaniak |              |                     |                       |        |
| Browser AI vendors |      |                     |                       |        |

The Article 50(2) marking obligation is the provider's, not this project's. Vendor watermarking status is tracked to evidence that providers meet their own obligations, not because the project carries that duty.

Providers whose generative systems were placed on the market before 2 August 2026 had until 2 December 2026 to comply. Reviews covering periods before that date should record an absent watermark as pending under the grace period rather than as vendor non-compliance. Reviews after that date should treat a continued absence as a finding worth raising with the vendor.

## Tool inventory drift

| Check                                          | Finding | Action |
| ---------------------------------------------- | ------- | ------ |
| New AI tools adopted since last review          |         |        |
| Tools discontinued since last review            |         |        |
| Existing tools used for new output destinations |         |        |
| Inventory updated to match                      |         |        |

New tools are the most common source of quiet drift. A tool adopted mid-quarter and used for public output without review is the scenario that breaks the exemption position without anyone noticing.

## Disclosure infrastructure

Disclosure is published as Universal Cake policy rather than to satisfy an Article 50 obligation. It is checked here because the policy commitment stands on its own, not because a regulator requires it.

| Check                                        | Status | Notes |
| -------------------------------------------- | ------ | ----- |
| Documentation website footer present         |        |       |
| Repository README disclosures present        |        |       |
| CI/CD meta tags emitted                      |        |       |
| Automated validation passing                 |        |       |
| Disclosure wording consistent across surfaces |       |       |

## Records and retention

| Record type                     | Retention | Present | Notes |
| ------------------------------- | --------- | ------- | ----- |
| Risk assessment records         | 3 years   |         |       |
| Legal consultation notes        | 5 years   |         |       |
| Vendor compliance documentation | 3 years   |         |       |
| Disclosure implementation logs  | 3 years   |         |       |
| Human review logs               | 3 years   |         |       |
| Guidance update logs            | 3 years   |         |       |

## Findings

| ID  | Finding | Severity | Owner | Due | Status |
| --- | ------- | -------- | ----- | --- | ------ |
|     |         |          |       |     |        |

Severity is critical, high, medium, or low. Any change to the exemption position is critical by definition, because it changes the legal basis on which the project publishes.

## Sign-off

| Field                          | Value |
| ------------------------------ | ----- |
| Exemption position unchanged   |       |
| Critical findings raised       |       |
| Reviewer signature             |       |
| Date                           |       |

## License

This document, *Quarterly Compliance Review Template*, by **Christopher Steel**, with AI assistance from **Claude Opus 5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

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
| 0.1.0   | Draft  | Initial creation. Structured around the conditional nature of the Article 50(4) exemption: the review's primary function is confirming that the conditions the exemption rests on still hold, rather than restating a settled position |

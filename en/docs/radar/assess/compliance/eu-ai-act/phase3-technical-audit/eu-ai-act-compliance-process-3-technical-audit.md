# Technical Audit: EU AI Act Compliance Process -- Phase 3: Technical Audit

Version: 0.2.0 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document defines the technical audit process for EU AI Act Article 50 compliance. It covers tool documentation requirements, vendor compliance verification, watermark detection methods, and audit artifact production. The technical audit follows Phase 1 (Risk Assessment) and Phase 2 (Legal Review). It precedes Phase 4 (Guidance Engagement) and Phase 5 (Disclosure Infrastructure).

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. Implementation guidance follows <a name="ssl-compliance-citation"></a>[SSL.com (2026)](#ssl-compliance-reference). Technical best practices derive from <a name="neuromarketing-guide-citation"></a>[Institute for Neuromarketing (2026)](#neuromarketing-guide-reference).

## Audit objectives

| Objective                | Description                                     |
| ------------------------ | ----------------------------------------------- |
| Document all AI tools    | Create complete inventory of AI systems in use  |
| Verify vendor compliance | Confirm providers meet watermarking obligations |
| Map output flows         | Trace where AI-generated content travels        |
| Preserve audit trail     | Retain evidence for future regulatory inquiries |
| Identify gaps            | Find missing safeguards or documentation        |

The technical audit ensures vendor compliance rather than rebuilding it. Third-party AI providers handle watermarking. The team maintains records showing providers comply with Article 50(2) obligations.

## Audit scope

| Category                            | Included | Excluded           |
| ----------------------------------- | -------- | ------------------ |
| AI tools used internally            | Yes      | None               |
| Third-party vendor agreements       | Yes      | None               |
| Output destination tracking         | Yes      | None               |
| Watermark verification              | Yes      | None               |
| Code repository scanning            | Yes      | None               |
| Employee training records           | Yes      | None               |
| Customer-facing application testing | No       | AI not in products |

The audit covers all tools identified in the Phase 1 risk assessment. ChatGPT, Claude, Euria, Lumo, browser-integrated AI, and CI/CD pipelines all require documentation. Customer-facing applications do not require testing because they contain no AI features.

Editorial review predates AI adoptioninfo

Universal Cake refined its editorial review process before the team began working with AI, and all documentation, current and historical, reflects that process. The review practice is therefore independently evidenced across the full document history rather than asserted from the point of AI adoption onward.

This matters to the audit in two ways. It is the strongest available evidence that the review satisfying the Article 50(4) exemption is substantive rather than a compliance formality, because the practice existed with no regulation prompting it. And it means the audit task is to document a process that already runs, not to design one. Auditors should sample documents from before and after AI adoption to show the review standard is continuous.

## Audit process steps

### Step 3.1: Tool inventory verification

| Task                     | Method                     | Artifact                      |
| ------------------------ | -------------------------- | ----------------------------- |
| List all AI tools        | Interview development team | Updated inventory spreadsheet |
| Record usage frequency   | Survey team members        | Usage frequency table         |
| Document output types    | Review sample outputs      | Output type classification    |
| Map workflow integration | Trace CI/CD pipelines      | Workflow diagram              |

This step confirms the Phase 1 inventory remains accurate. New tools may have been added since the initial assessment. Usage patterns may have changed as projects advance.

### Step 3.2: Vendor compliance verification

| Task                              | Method                   | Artifact                     |
| --------------------------------- | ------------------------ | ---------------------------- |
| Review terms of service           | Read provider agreements | Compliance summary document  |
| Check watermarking implementation | Test tool outputs        | Watermark detection report   |
| Verify GDPR compliance            | Review privacy policies  | Privacy compliance checklist |
| Document retention requirements   | Note data policies       | Retention schedule table     |

Third-party providers must embed machine-readable watermarks per Article 50(2). The team verifies this happens without needing to implement watermarking internally. Providers whose systems were placed on the market before 2 August 2026 have until 2 December 2026 to comply, per <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference), so record the check date alongside each result.

### Step 3.3: Output flow mapping

| Task                           | Method                | Artifact                    |
| ------------------------------ | --------------------- | --------------------------- |
| Trace documentation generation | Review commit history | Documentation flow diagram  |
| Map code generation paths      | Analyze CI/CD logs    | Code generation flowchart   |
| Track email content origin     | Sample sent messages  | Email output classification |
| Verify human review points     | Review pull requests  | Review checkpoint list      |

This step ensures every AI-generated output has documented human review and appropriate disclosure labeling.

### Step 3.4: Gap identification

| Task                                  | Method                      | Artifact                  |
| ------------------------------------- | --------------------------- | ------------------------- |
| Compare current state to requirements | Use compliance checklist    | Gap analysis report       |
| Prioritize remediation actions        | Score by risk level         | Remediation priority list |
| Assign ownership for fixes            | Match to team roles         | Action assignment table   |
| Set completion deadlines              | Align with v1.0.0 milestone | Timeline schedule         |

Gaps get scored as critical, high, medium, or low priority. Critical gaps block v1.0.0 release. Medium and low priorities can be addressed incrementally.

## Expected artifacts

Artifact statusinfo

Artifacts 1, 2, and 3 below are empty templates. No vendor documentation has been collected and no watermark verification has been performed. Every value column reads TBD by design. Do not cite these tables as audit findings until Phase 3 is executed and the columns are populated with dated, sourced results.

An earlier version of this document presented these three tables pre-filled, showing vendor terms as "Downloaded" and watermarks verified at "High" confidence. Those values were illustrative placeholders, not findings. They have been cleared, because a compliance artifact that presents unperformed checks as completed is worse than no artifact at all.

### Artifact 1: AI tool inventory spreadsheet (template)

| Tool | Purpose | Output destination | Watermark verified | Vendor contact | Last reviewed |
|------|---------|-------------------|--------------------|----------------|---------------|
| ChatGPT | Documentation, code | Public docs, GitHub | TBD | TBD | TBD |
| Claude | Documentation, code | Public docs, GitHub | TBD | TBD | TBD |
| Euria | Research, documentation | Public docs, GitHub | TBD | TBD | TBD |
| Lumo | Documentation, code, research | Public docs, GitHub | TBD | TBD | TBD |
| Browser AI (Brave, Chrome, DuckDuckGo) | Research, correspondence | Emails | TBD | TBD | TBD |
| Code in CI/CD pipelines | Website generation, deployment | Customer-facing websites | TBD | TBD | TBD |

This spreadsheet tracks each tool through audit completion. All six tools from the Phase 1 inventory appear here. An earlier version listed only four, omitting browser AI and CI/CD pipelines, which conflicted with the completion criteria below requiring all six to be documented.

### Artifact 2: Vendor compliance documentation package (template)

Collected documents belong in `vendor-compliance-docs/`, which does not exist yet and must be created as part of this phase. Organise it per vendor so that later frameworks can be added alongside the EU AI Act material without restructuring:

```
vendor-compliance-docs/
├── openai/
├── anthropic/
├── proton/
└── euria/
```

| Document                      | Source                 | Status      |
| ----------------------------- | ---------------------- | ----------- |
| Terms of service (ChatGPT)    | OpenAI website         | Not started |
| Terms of service (Claude)     | Anthropic website      | Not started |
| Terms of service (Lumo)       | Proton website         | Not started |
| Terms of service (Euria)      | Vendor website         | Not started |
| Watermarking policy statement | Provider documentation | Not started |
| Data processing agreement     | Vendor portal          | Not started |

Once collected, these documents evidence how providers handle their Article 50(2) marking obligations. The team retains copies for audit readiness. The same collection is intended to serve other regulatory frameworks as they are added, so file each document with its retrieval date and source URL rather than tagging it to the EU AI Act alone.

### Artifact 3: Watermark detection report (template)

| Tool    | Watermark present | Detection method | Confidence | Date checked |
| ------- | ----------------- | ---------------- | ---------- | ------------ |
| ChatGPT | TBD               | TBD              | TBD        | TBD          |
| Claude  | TBD               | TBD              | TBD        | TBD          |
| Euria   | TBD               | TBD              | TBD        | TBD          |
| Lumo    | TBD               | TBD              | TBD        | TBD          |

When populated, this report records whether machine-readable markings are present in AI outputs. Note the grace period: providers whose generative systems were placed on the market before 2 August 2026 have until 2 December 2026 to meet the Article 50(2) marking obligation. An absent watermark before that date is not evidence of vendor non-compliance, and should be recorded as such rather than raised as a gap.

### Artifact 4: Human review log template

| Content type    | Review required   | Reviewer  | Date       | Approval status |
| --------------- | ----------------- | --------- | ---------- | --------------- |
| Documentation   | Before publishing | Developer | 2026-08-XX | Approved        |
| Code            | Before merge      | Developer | 2026-08-XX | Approved        |
| Emails          | Before sending    | Developer | 2026-08-XX | Approved        |
| Website content | Before deployment | Developer | 2026-08-XX | Approved        |

This template standardizes review documentation. Completed logs prove human editorial control for exemption claims.

### Artifact 5: Gap analysis report

| Gap ID  | Description                            | Severity | Remediation                   | Timeline | Owner     |
| ------- | -------------------------------------- | -------- | ----------------------------- | -------- | --------- |
| GAP-001 | vendor-compliance-docs/ does not exist  | High     | Create and populate directory | Week 3-4 | Developer |
| GAP-002 | No watermark verification performed     | Medium   | Run detection, record results | Week 4   | Developer |
| GAP-003 | Editorial review process not documented | High     | Write up the review process   | Week 3   | Developer |
| GAP-004 | Footer disclosures not yet deployed     | Medium   | Add disclosure text           | Week 5   | Developer |
| GAP-005 | No automated review logging             | Low      | Implement script              | Week 6   | Developer |

Gap severity determines remediation urgency. High severity items must resolve before v1.0.0 release.

GAP-003 is rated high because the Article 50(4) exemption position rests on the editorial review process being substantive and attributable. The process itself is long established, see Audit scope above, but it has never been written down. The gap is documentation of the process, not the process.

GAP-004 is rated medium rather than high because footer disclosure implements Universal Cake transparency policy rather than a legal obligation. See the Phase 5 document for the basis.

Audit completion criteriainfo

Technical audit completes when all six tools have documented vendor compliance, all watermarks verified or explicitly recorded as pending under the 2 December 2026 grace period, the editorial review process is documented, and all high-severity gaps are assigned to owners with deadlines.

## Audit timeline

| Week     | Activity                              | Deliverable                   |
| -------- | ------------------------------------- | ----------------------------- |
| Week 3   | Tool inventory verification           | Updated inventory spreadsheet |
| Week 3-4 | Vendor compliance verification        | Documentation package         |
| Week 4   | Watermark detection testing           | Detection report              |
| Week 4-5 | Output flow mapping                   | Flow diagrams and templates   |
| Week 5   | Gap identification and prioritization | Gap analysis report           |

Total duration spans three to four weeks for a small team. Parallel work streams allow faster completion.

## Integration with other phases

| Phase                               | Connection point                              | Handoff artifact       |
| ----------------------------------- | --------------------------------------------- | ---------------------- |
| Phase 1 (Risk Assessment)           | Input from AI inventory                       | Risk assessment report |
| Phase 2 (Legal Review)              | Questions from legal consultation             | Scope memo             |
| Phase 4 (Guidance Engagement)       | Monitoring setup based on vendor requirements | Monitoring protocol    |
| Phase 5 (Disclosure Infrastructure) | Disclosure requirements from gap analysis     | Footer implementation  |

The technical audit feeds directly into disclosure infrastructure planning. Vendor compliance status influences monitoring requirements for ongoing compliance.

Audit efficiency tipinfo

For sole developers, allocate 15-20 hours total across the 3-4 week timeline. Batch vendor communications together to minimize context switching. Use existing documentation workflows rather than creating parallel systems.

## License

This document, *Technical Audit: EU AI Act Compliance Process*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Regulatory guidance

- [AI Office Guidelines](#eu-ai-act-article50-reference)
- [Code of Practice](#neuromarketing-guide-reference)

### Compliance resources

- [SSL.com Implementation Guide](#ssl-compliance-reference)

## References

<a name="eu-ai-act-article50-reference"></a>European Commission. (2026a). Guidelines on transparency obligations for providers and deployers of AI systems. European Commission. https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations Return to citation

<a name="ec-faq-reference"></a>European Commission. (2026b). Transparency obligations under Article 50 of the AI Act [Frequently asked questions]. Shaping Europe's Digital Future. https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act Return to citation

<a name="ssl-compliance-reference"></a>SSL.com. (2026). EU AI Act Article 50: A complete guide to AI transparency compliance. SSL.com. https://www.ssl.com/article/eu-ai-act-article-50-a-complete-guide-to-ai-transparency-compliance Return to citation

<a name="neuromarketing-guide-reference"></a>Institute for Neuromarketing & Intellectual Property. (2026). Labelling AI content. Institute for Neuromarketing. https://neuromarketinginstitut.com/eng/ai-content-transparency Return to citation

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.2.0   | Draft  | Cleared Artifacts 1, 2, and 3, which presented unperformed checks as completed findings, showing vendor terms as "Downloaded" and watermarks verified at "High" confidence; all three are now explicitly labelled templates with TBD values. Added the two tools missing from the Artifact 1 inventory, which listed four against a completion criterion requiring six. Recorded that the editorial review process predates AI adoption and is evidenced across the full document history. Added the 2 December 2026 Article 50(2) grace period so an absent watermark is not logged as vendor non-compliance. Reworked the gap analysis to reflect actual outstanding work, including the missing vendor-compliance-docs/ directory and the undocumented review process |
| 0.1.0   | Draft  | Initial creation following APA 7 CAP workflow and ASD-STE100 controlled language principles |
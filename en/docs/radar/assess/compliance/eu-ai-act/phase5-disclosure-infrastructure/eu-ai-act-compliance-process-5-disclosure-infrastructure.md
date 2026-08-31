# EU AI Act Compliance Process--5. Disclosure Infrastructure

Version: 0.2.0 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document defines disclosure infrastructure implementation for AI assistance transparency, adopted as project policy rather than to satisfy an Article 50 obligation. It covers disclosure text templates, footer placement strategies, metadata standards, and integration workflows for public documentation, GitHub repositories, and website content. The phase follows Phase 3 (Technical Audit) and runs in parallel with Phase 4 (Guidance Engagement). It is not gated on Phase 2 (Legal Review). Completion targets the v1.0.0 release milestone.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689. Implementation guidance follows <a name="ssl-compliance-citation"></a>[SSL.com (2026)](#ssl-compliance-reference). Transparency best practices derive from <a name="neuromarketing-guide-citation"></a>[Institute for Neuromarketing (2026)](#neuromarketing-guide-reference).

## Disclosure objectives

| Objective                         | Description                             |
| --------------------------------- | --------------------------------------- |
| Inform readers of AI involvement  | State when content uses AI assistance   |
| Maintain transparency consistency | Uniform disclosure across all platforms |
| Minimize user friction            | Place disclosure unobtrusively          |
| Enable audit trail preservation   | Document disclosure deployment dates    |

The team qualifies as a deployer, not a provider. Machine-readable watermarking remains the provider responsibility through third-party vendors.

Basis for this phaseinfo

The work in this phase implements a standing Universal Cake policy of transparency about AI use. That policy exists independently of the EU AI Act and was not adopted in response to it.

Separately, the Phase 1 risk assessment concludes that the Article 50(4) text disclosure obligation does not attach to this project's published content, because that obligation reaches only text published to inform the public on matters of public interest, and because it is disapplied where content has undergone human review or editorial control with a named person holding editorial responsibility. Both grounds hold here, the second as a consequence of the project's existing internal policy of human review before release.

Three consequences follow. Phase 5 is not gated on the Phase 2 legal outcome, because nothing in this phase depends on the obligation existing. Phase 5 would proceed unchanged if Article 50 were repealed tomorrow, because the policy is the reason for the work. And disclosure text should not assert that it is published in order to comply with Article 50, because that would misstate the basis. Templates below are worded accordingly.

## Disclosure placement matrix

| Platform                              | Placement location | Visibility | Article 50 obligation | Project policy |
| ------------------------------------- | ------------------ | ---------- | --------------------- | -------------- |
| Public documentation website          | Page footer        | Low        | None                  | Yes            |
| GitHub repository README              | Top section        | High       | None                  | Yes            |
| GitHub repository documentation files | Footer or header   | Medium     | None                  | Yes            |
| Business emails                       | Signature line     | Low        | None                  | Optional       |
| CI/CD generated websites              | HTML meta tag      | Hidden     | None                  | Yes            |
| Customer-facing applications          | None               | N/A        | None                  | No             |

No row carries an Article 50 obligation, for the reasons given under Basis for this phase. Public documentation and GitHub repositories carry visible disclosures by project policy. Business emails are left to the sender's judgement, with disclosure suggested for mass or public distribution. Customer-facing applications contain no AI features, so nothing triggers there on any basis.

## Disclosure text templates

### Template A: General footer disclosure

```
Some content in this documentation may have been assisted by artificial intelligence tools. All published material undergoes human review before release.
```

This template works for website footers and documentation pages. It acknowledges AI involvement while emphasizing human oversight. It deliberately makes no claim about Article 50, because the disclosure is published as policy rather than to discharge an obligation.

### Template B: Repository README disclosure

```markdown
## AI Assistance Disclosure

This project uses artificial intelligence tools during development. Documentation, code comments, and README content may include AI-assisted drafting. All contributions receive human review before merging to main branches.
```

Repository README files need prominent disclosure at the top section. This visibility ensures contributors and users understand the development workflow. The statement covers multiple AI output types within one declaration.

### Template C: Meta tag disclosure

```html
<meta name="content-origin" content="human-reviewed, ai-assisted">
<meta name="disclosure" content="Some content in this documentation may have been AI-assisted. All published material undergoes human review before release.">
```

The earlier form of this template ended with "per EU AI Act Article 50". That wording was removed because it asserts the disclosure is published to satisfy an Article 50 obligation, which is not the basis for this phase.

HTML meta tags provide machine-readable disclosure alongside human-readable text. These tags support future automated compliance verification. Search engines and accessibility tools may index the disclosure metadata.

### Template D: Email signature disclosure

```
--
This message may have been drafted with AI assistance and has been reviewed before sending.
```

Email disclosure applies selectively based on recipient type. External business communications warrant the statement. Internal team messages do not require disclosure unless forwarded publicly.

Template selection guideinfo

Choose Template A for website footers, Template B for GitHub repositories, Template C for programmatic content injection, and Template D for selective email use. Do not over-disclose internal communications.

## Implementation workflow

### Step 5.1: Select disclosure locations

| Location                     | Template   | Priority | Owner     |
| ---------------------------- | ---------- | -------- | --------- |
| Documentation website footer | Template A | High     | Developer |
| Main repository README       | Template B | High     | Developer |
| Secondary repository files   | Template A | Medium   | Developer |
| Email signature              | Template D | Low      | Developer |
| CI/CD meta tags              | Template C | Medium   | DevOps    |

Priority levels determine implementation sequence. High priority locations launch before v1.0.0 release. Medium priority items follow within two weeks. Low priority items become optional enhancements.

### Step 5.2: Add disclosure to documentation website

| Task                   | Command                                                  | Verification        |
| ---------------------- | -------------------------------------------------------- | ------------------- |
| Edit footer component  | `vim _includes/footer.html`                              | View page source    |
| Insert disclosure text | Add Template A HTML                                      | Load homepage       |
| Commit changes         | `git add -A && git commit -m 'Add AI disclosure footer'` | Pull request review |
| Deploy update          | CI/CD pipeline triggers                                  | Test production URL |

Website modifications go through version control workflows. Pull requests ensure peer review even for sole developers. Production testing validates disclosure visibility across devices.

### Step 5.3: Update GitHub repositories

| Repository         | README location | Modification date | Status      |
| ------------------ | --------------- | ----------------- | ----------- |
| main-project       | Root directory  | 2026-XX-XX        | Pending     |
| secondary-tool     | Root directory  | 2026-XX-XX        | Not started |
| documentation-site | Root directory  | 2026-XX-XX        | In progress |

Each repository receives individual README updates. Root directory placement ensures maximum discoverability. Status tracking prevents missed repositories during rollout.

### Step 5.4: Configure CI/CD meta tags

| Pipeline stage | Injection point      | Variable             |
| -------------- | -------------------- | -------------------- |
| Build          | HTML head section    | META_DISCLOSURE_TEXT |
| Deploy         | Configuration file   | DISCLOSURE_ENABLED   |
| Release        | Environment variable | COMPLIANCE_VERSION   |

CI/CD pipelines inject disclosure metadata programmatically. Environment variables enable toggling during testing. Version tracking links deployments to compliance versions.

## Version control integration

### Commit message standards

```
feat: Add AI assistance disclosure footer

- Implements Template A for documentation website
- Adds machine-readable meta tags for provenance
- Maintains human review workflow documentation
- Published as project policy, not an Article 50 obligation
- Related issue: #compliance-phase5
```

Commit messages document compliance actions for audit trails. Issue references connect changes to compliance roadmap items. Feature flags indicate disclosure activation states.

### Branch naming conventions

| Branch type                  | Naming pattern                          | Purpose                       |
| ---------------------------- | --------------------------------------- | ----------------------------- |
| Disclosure implementation    | `feature/compliance-disclosure-v1`      | Primary implementation branch |
| Testing and validation       | `test/compliance-disclosure-validation` | QA verification branch        |
| Hotfix for disclosure issues | `hotfix/compliance-disclosure-fix`      | Urgent correction branch      |

Branch names signal compliance-related work clearly. Separation from feature branches reduces merge conflicts. Hotfix branches track urgent disclosure corrections separately.

## Testing and validation

### Manual testing checklist

| Item                  | Method                | Pass criteria                    |
| --------------------- | --------------------- | -------------------------------- |
| Footer visibility     | Load all site pages   | Disclosure appears on every page |
| README rendering      | View on GitHub        | Template B displays correctly    |
| Meta tag presence     | View page source      | Tags appear in HTML head section |
| Email display         | Send test message     | Signature shows disclosure text  |
| Mobile responsiveness | Test on phone screens | Disclosure remains readable      |

Manual testing catches rendering issues before production deployment. Cross-platform validation ensures consistent disclosure appearance. Mobile testing addresses accessibility requirements.

### Automated validation scripts

```python
def validate_disclosure_content(html_content: str) -> bool:
    """Check if disclosure text appears in rendered HTML."""
    required_strings = [
        "artificial intelligence",
        "human review",
    ]
    haystack = html_content.lower()
    return all(s in haystack for s in required_strings)
```

The original version of this check also required the literal string "Article 50" and compared mixed-case needles against a lowercased haystack, so it could never return True. Both faults are corrected above. The "Article 50" requirement was dropped because the disclosure templates no longer cite the Article as the basis for publication.

Automated scripts validate disclosure presence programmatically. Continuous integration runs tests on every commit. Failed builds prevent non-compliant deployments.

Validation timelineinfo

Complete manual testing within one week of implementation. Run automated validation on every build thereafter. Re-test after any guidance updates from Phase 4 monitoring.

## Documentation preservation

### Audit trail requirements

| Record type                     | Retention period | Storage location        |
| ------------------------------- | ---------------- | ----------------------- |
| Disclosure implementation logs  | 3 years          | Compliance repository   |
| Vendor compliance documentation | 3 years          | Shared drive            |
| Human review logs               | 3 years          | Project management tool |
| Deployment timestamps           | 3 years          | CI/CD logs              |
| Legal consultation notes        | 5 years          | Secure document store   |

Three-year retention satisfies typical regulatory inquiry windows. Five-year retention for legal consultations provides extended protection. Centralized storage enables efficient retrieval during audits.

### Change management procedures

| Event                  | Action required   | Approval needed    |
| ---------------------- | ----------------- | ------------------ |
| Disclosure text update | Review by counsel | Legal sign-off     |
| Placement modification | Technical testing | Team lead approval |
| New platform addition  | Impact assessment | Compliance review  |
| Guidance policy change | Gap analysis      | Legal confirmation |

Change management prevents unauthorized modifications to compliance infrastructure. Legal sign-off ensures text changes meet regulatory standards. Technical testing validates implementation integrity.

## v1.0.0 release integration

### Pre-release checklist

| Item                                      | Status  | Owner     | Due date      |
| ----------------------------------------- | ------- | --------- | ------------- |
| All high-priority disclosures implemented | Pending | Developer | v1.0.0-2weeks |
| Manual testing complete                   | Pending | Developer | v1.0.0-1week  |
| Automated validation passing              | Pending | DevOps    | v1.0.0-1week  |
| Legal counsel confirmation received       | Pending | Counsel   | v1.0.0-1week  |
| Disclosure basis stated as policy, not Article 50 | Complete | Developer | Done |
| Documentation updated                     | Pending | Developer | v1.0.0-launch |

All checklist items must pass before v1.0.0 release. Two-week buffer allows remediation of discovered issues. One-week buffer ensures final validation completeness.

### Post-launch monitoring

| Activity                       | Frequency | Responsible party |
| ------------------------------ | --------- | ----------------- |
| Disclosure functionality check | Weekly    | Developer         |
| Vendor policy review           | Monthly   | Developer         |
| Guidance update scan           | Weekly    | Developer         |
| Compliance audit preparation   | Quarterly | Team              |

Post-launch monitoring maintains ongoing compliance posture. Weekly checks catch infrastructure failures early. Quarterly audits prepare for potential regulatory inquiries.

## Success metrics

| Metric                  | Target                        | Measurement method      |
| ----------------------- | ----------------------------- | ----------------------- |
| Disclosure coverage     | 100 percent of public content | Site crawl validation   |
| Implementation timeline | Before v1.0.0 release         | Version control history |
| Error rate              | Below 1 percent               | Automated test results  |
| Maintenance overhead    | Under 2 hours weekly          | Time tracking           |

Success metrics measure disclosure infrastructure effectiveness objectively. Coverage percentage ensures no content slips through. Timeline adherence demonstrates proactive compliance planning.

Implementation status

Not started

▼Ready to begin, not gated on legal review

Target completion

Before v1.0.0

–Q4 2026

Estimated effort

8-12 hours

–Total implementation

## License

This document, *EU AI Act Compliance Process--5. Disclosure Infrastructure*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Regulatory guidance

- [AI Office Guidelines](#eu-ai-act-article50-reference)
- [Code of Practice](#neuromarketing-guide-reference)

### Compliance resources

- [SSL.com Implementation Guide](#ssl-compliance-reference)

## References

<a name="eu-ai-act-article50-reference"></a>European Commission. (2026). Guidelines on transparency obligations for providers and deployers of AI systems. European Commission. https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations Return to citation

<a name="ssl-compliance-reference"></a>SSL.com. (2026). EU AI Act Article 50: A complete guide to AI transparency compliance. SSL.com. https://www.ssl.com/article/eu-ai-act-article-50-a-complete-guide-to-ai-transparency-compliance Return to citation

<a name="neuromarketing-guide-reference"></a>Institute for Neuromarketing & Intellectual Property. (2026). Labelling AI content. Institute for Neuromarketing. https://neuromarketinginstitut.com/eng/ai-content-transparency Return to citation

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.2.0   | Draft  | Reframed the phase as project policy rather than Article 50 compliance, following the corrected exemption analysis in Phase 1; removed the Article 50 citation from Template C and from the placement matrix; split the matrix to separate obligation from policy; fixed the validation script, which required a literal "Article 50" string and compared mixed-case needles against a lowercased haystack so could never return True; recorded that the phase is not gated on legal review |
| 0.1.0   | Draft  | Initial creation following APA 7 CAP workflow and ASD-STE100 controlled language principles |
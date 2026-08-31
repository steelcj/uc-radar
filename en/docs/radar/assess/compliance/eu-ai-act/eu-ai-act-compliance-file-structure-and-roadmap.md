# EU AI Act Compliance File Structure and Roadmap

Version: 0.7.0 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document organizes all EU AI Act compliance artifacts into a coherent structure supporting progression toward one hundred percent compliance. It establishes file naming conventions, dependency relationships between documents, completion tracking for each phase, and prioritized action items leading to v1.0.0 release readiness. The document applies ASD-STE100 controlled language principles and APA 7 Citation Anchor Pair workflow.

## Sources and acknowledgements

The regulatory framework comes from <a name="eu-ai-act-article50-citation"></a>[European Commission (2026a)](#eu-ai-act-article50-reference) guidelines under Regulation (EU) 2024/1689, and from the text of Article 50 itself at <a name="eu-ai-act-text-citation"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference). Implementation guidance follows <a name="ssl-compliance-citation"></a>[SSL.com (2026)](#ssl-compliance-reference). Control language principles derive from <a name="asd-ste100-citation"></a>[ASD-STAN (2026)](#asd-ste100-reference). Citation standards follow <a name="apa-apa7-citation"></a>[American Psychological Association (2020)](#apa-apa7-reference).

## Regulatory Status

Article 50 is already in force. It became applicable on 2 August 2026, as part of the general application of the Act, per <a name="eu-ai-act-text-citation2"></a>[EU Artificial Intelligence Act (2026)](#eu-ai-act-text-reference). This roadmap was drafted on 2026-08-12, ten days after that date. The phases below therefore describe work to reach a documented compliance position for obligations that already apply, not preparation ahead of a future deadline.

| Obligation                                    | Applies from    | Relevance to this project             |
| --------------------------------------------- | --------------- | ------------------------------------- |
| Article 50(1), disclose AI interaction        | 2 August 2026   | Not applicable, no AI in end products |
| Article 50(2), machine-readable marking       | 2 August 2026   | Provider obligation, held by vendors  |
| Article 50(2), grace period for existing systems | 2 December 2026 | Affects vendor watermark verification |
| Article 50(4), deepfake disclosure            | 2 August 2026   | Not applicable, no deepfake content   |
| Article 50(4), AI-generated text disclosure   | 2 August 2026   | Does not attach, see below            |

> **Finding: Transparency is an independent policy, not a compliance measure**, Universal Cake maintains two standing policies that exist independently of the EU AI Act: AI use is disclosed transparently, and all content receives human review before release. Neither was adopted in response to Article 50; the editorial review process was refined before the team began working with AI at all, and all documentation, current and historical, reflects it. The review policy is open-ended and continues after v1.0.0. Separately, the Phase 1 risk assessment concludes that the Article 50(4) text disclosure obligation does not attach to this project's content, because that obligation reaches only text published to inform the public on matters of public interest, and because it is disapplied where content has undergone human review or editorial control with a named person holding editorial responsibility. The relationship runs one way: the existing policies happen to satisfy the exemption conditions. The Act does not drive the policies, and the policies would remain in force if the Act changed. Phase 5 is therefore not gated on the Phase 2 legal outcome, and no document should describe disclosure as required by Article 50 or as a response to it.

Providers whose generative systems were placed on the market before 2 August 2026 have until 2 December 2026 to meet the Article 50(2) marking obligation, per <a name="ec-faq-citation"></a>[European Commission (2026b)](#ec-faq-reference). Phase 3 vendor watermark verification should record this grace period rather than treating an absent watermark as a vendor compliance failure before that date.

> **Finding: Omnibus check outstanding**, The AI Digital Omnibus entered into force on 27 July 2026 and postponed certain high-risk obligations. Reporting to date indicates it did not amend Article 50. This project deploys no high-risk systems, so the postponements do not apply. Confirm the Article 50 position against a primary source at the first Phase 4 guidance review, because this point was established from secondary reporting rather than the amending instrument itself.

## Directory Structure

All compliance artifacts live under `en/docs/radar/assess/compliance/eu-ai-act/`, including this roadmap.

Each of the five phases owns a subdirectory holding its phase document and that phase's supporting artifacts. Documents that span all phases, the master process definition, the two background references, and the operational status snapshot, stay at the top of `eu-ai-act/` because they belong to no single phase. Paths below are relative to `en/docs/`.

```
radar/assess/compliance/
└── eu-ai-act/
    ├── eu-ai-act-compliance-file-structure-and-roadmap.md   # This document
    ├── phase1-risk-assessment/
    │   ├── eu-ai-act-compliance-process-1-risk-assessment-complete.md
    │   └── risk-assessment-interview-notes.md
    ├── phase2-legal-review/
    │   └── compliance-scope-memo--eu-ai-act-article-50-phase-2-legal.md
    ├── phase3-technical-audit/
    │   └── eu-ai-act-compliance-process-3-technical-audit.md
    ├── phase4-guidance-engagement/
    │   └── eu-ai-act-compliance-process-4--guidance-engagement.md
    ├── phase5-disclosure-infrastructure/
    │   └── eu-ai-act-compliance-process-5-disclosure-infrastructure.md
    ├── eu-ai-act-compliance-process-for-small-developer-teams-using-ai-internally.md
    ├── eu-ai-act-transparency-requirements-for-small-development-teams.md
    ├── eu-ai-act-s-transparency-rules-for-using-ai-to-collaboratively-produce-website-content.md
    ├── eu-ai-act-current-compliance-status.md
    ├── quarterly-review-template.md
    └── universal-cake-evaluation--eu-ai-act-article-50--ai-transparency-v0-1-0.md
```

### Document roles

| Document                                                                  | Role                                              |
| ------------------------------------------------------------------------- | ------------------------------------------------- |
| eu-ai-act-compliance-file-structure-and-roadmap.md                        | This index and phase tracker                      |
| eu-ai-act-compliance-process-for-small-developer-teams-using-ai-internally.md | Master five-phase process definition           |
| eu-ai-act-transparency-requirements-for-small-development-teams.md        | Background: Article 50 requirements overview      |
| eu-ai-act-s-transparency-rules-for-using-ai-to-collaboratively-produce-website-content.md | Background: collaborative content scenario |
| phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md | Phase 1 deliverable                     |
| phase2-legal-review/compliance-scope-memo--eu-ai-act-article-50-phase-2-legal.md | Phase 2 deliverable                     |
| phase3-technical-audit/eu-ai-act-compliance-process-3-technical-audit.md | Phase 3 deliverable                     |
| phase4-guidance-engagement/eu-ai-act-compliance-process-4--guidance-engagement.md | Phase 4 deliverable                     |
| phase5-disclosure-infrastructure/eu-ai-act-compliance-process-5-disclosure-infrastructure.md | Phase 5 deliverable                     |
| eu-ai-act-current-compliance-status.md                                    | Operational status snapshot                       |
| quarterly-review-template.md                                              | Operational, recurring review template            |
| universal-cake-evaluation--eu-ai-act-article-50--ai-transparency-v0-1-0.md | Radar evaluation against UC metrics v0.3.1        |
| phase1-risk-assessment/risk-assessment-interview-notes.md                 | Phase 1 supporting artifact, interview record     |

Two foundation documents this roadmap depends on live elsewhere in the repository rather than under `eu-ai-act/`:

| Document                                                            | Location                     |
| ------------------------------------------------------------------- | ---------------------------- |
| style-guide--web-ready-unrendered-markdown-using-apa-7-v0-5-0.md    | guides/style-guides/         |
| using-asd-ste100-what-it-is-and-how-it-works.md                     | radar/assess/style-guides/   |

### Artifacts not yet created

None of the artifacts in this table exist on disk. They are planned paths only, recording where each artifact will go when the phase that produces it is executed. Rows in the phase matrices below marked "Not started" refer to these paths. All paths are relative to `radar/assess/compliance/eu-ai-act/`.

Of the original nine, seven remain outstanding. Two have been created and removed from this table: the Phase 1 interview notes at `phase1-risk-assessment/risk-assessment-interview-notes.md`, and the quarterly review template at `quarterly-review-template.md`.

| Planned artifact                        | Specified by | Planned path                              |
| --------------------------------------- | ------------ | ----------------------------------------- |
| Legal consultation template             | Phase 2      | phase2-legal-review/legal-consultation-template.md |
| AI tool inventory tracker               | Phase 3      | phase3-technical-audit/ai-tool-inventory-tracker.csv |
| Vendor compliance documents             | Phase 3      | phase3-technical-audit/vendor-compliance-docs/<vendor>/ |
| Monitoring subscription list            | Phase 4      | phase4-guidance-engagement/monitoring-subscriptions.md |
| Guideline change log                    | Phase 4      | phase4-guidance-engagement/guideline-change-log.md |
| Disclosure templates, extracted to file | Phase 5      | phase5-disclosure-infrastructure/disclosure-templates/ |
| Implementation checklist                | Phase 5      | phase5-disclosure-infrastructure/implementation-checklist.md |

## Document Dependencies

Phase status records whether the phase work is done. Document status records the version block of the phase document itself. The two differ for Phase 1, where the assessment work finished on 2026-08-12 but the document remains Draft.

| Document                  | Depends On       | Enables                                        | Phase Status | Document Status |
| ------------------------- | ---------------- | ---------------------------------------------- | ------------ | --------------- |
| Risk Assessment           | None             | All subsequent phases                          | Complete     | Draft           |
| Legal Scope Memo          | Risk Assessment  | Technical Audit                                | Pending      | Draft           |
| Technical Audit           | Legal Scope Memo | Guidance Engagement, Disclosure Infrastructure | Pending      | Draft           |
| Guidance Engagement       | Technical Audit  | Ongoing monitoring                             | Pending      | Draft           |
| Disclosure Infrastructure | Technical Audit  | v1.0.0 Release                                 | Pending      | Draft           |

Phase 4 and Phase 5 both depend on Phase 3 and run in parallel with each other, as stated in `phase4-guidance-engagement/eu-ai-act-compliance-process-4--guidance-engagement.md` and `phase5-disclosure-infrastructure/eu-ai-act-compliance-process-5-disclosure-infrastructure.md`. Phase 5 does not depend on Phase 4.

### Compliance Readiness Metrics

| Metric               | Value    | Delta                     | Direction |
| -------------------- | -------- | ------------------------- | --------- |
| Phase 1              | Complete | Risk assessment           | Up        |
| Phase 2-5            | Pending  | Phase documents drafted   | Flat      |
| Compliance readiness | 20%      | 80% remaining to target   | Up        |

> **Finding: Critical path**, Legal consultation gates Phase 2 and Phase 3. It does not gate Phase 5. Because Phase 5 depends only on Phase 3, and because Article 50 disclosure can be added at low cost, disclosure work can proceed in parallel with legal review rather than waiting on it. This matches Option C in `eu-ai-act-current-compliance-status.md`.

## Phase Completion Matrix

Locations are relative to `radar/assess/compliance/eu-ai-act/` unless stated otherwise. Where a deliverable is a section within a phase document rather than a separate file, the location names that document.

### Phase 1: Risk Assessment, COMPLETE

| Deliverable               | Status      | Date Completed | Location                                                   |
| ------------------------- | ----------- | -------------- | ---------------------------------------------------------- |
| AI inventory spreadsheet  | Complete    | 2026-08-12     | phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md |
| Output exposure mapping   | Complete    | 2026-08-12     | phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md |
| Exemption analysis        | Complete    | 2026-08-12     | phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md |
| Risk classification       | Complete    | 2026-08-12     | phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md |
| Risk assessment document  | Complete    | 2026-08-12     | phase1-risk-assessment/eu-ai-act-compliance-process-1-risk-assessment-complete.md |
| Interview notes           | Complete    | 2026-08-12     | phase1-risk-assessment/risk-assessment-interview-notes.md   |

### Phase 2: Legal Review, PENDING

| Deliverable                  | Status      | Target Date | Location                                                     | Owner     |
| ---------------------------- | ----------- | ----------- | ------------------------------------------------------------ | --------- |
| Compliance scope memo        | Draft       | Week 3      | phase2-legal-review/compliance-scope-memo--eu-ai-act-article-50-phase-2-legal.md | Developer |
| Legal consultation scheduled | Not started | Week 3      | External                                                     | Developer |
| Counsel confirmation memo    | Not started | Week 3      | Not created                                                  | Counsel   |

### Phase 3: Technical Audit, PENDING

| Deliverable                   | Status      | Target Date | Location                                            | Owner     |
| ----------------------------- | ----------- | ----------- | --------------------------------------------------- | --------- |
| Tool documentation package    | Draft       | Week 3-4    | phase3-technical-audit/eu-ai-act-compliance-process-3-technical-audit.md | Developer |
| Vendor contracts collected    | Not started | Week 3-4    | phase3-technical-audit/vendor-compliance-docs/, not created | Developer |
| Watermark verification report | Not started | Week 4      | Not created                                         | Developer |
| Gap analysis report           | Not started | Week 5      | Not created                                         | Developer |

### Phase 4: Guidance Engagement, PENDING

| Deliverable            | Status      | Target Date | Location                                                 | Owner     |
| ---------------------- | ----------- | ----------- | -------------------------------------------------------- | --------- |
| Monitoring protocol    | Draft       | Week 4      | phase4-guidance-engagement/eu-ai-act-compliance-process-4--guidance-engagement.md | Developer |
| RSS feed subscriptions | Not started | Week 4-5    | phase4-guidance-engagement/monitoring-subscriptions.md, not created | Developer |
| Guideline change log   | Not started | Ongoing     | phase4-guidance-engagement/guideline-change-log.md, not created | Developer |
| Vendor update tracker  | Not started | Month 1     | Not created                                              | Developer |

### Phase 5: Disclosure Infrastructure, PENDING

| Deliverable                   | Status      | Target Date | Location                                                        | Owner     |
| ----------------------------- | ----------- | ----------- | --------------------------------------------------------------- | --------- |
| Disclosure templates          | Draft       | Week 5      | phase5-disclosure-infrastructure/eu-ai-act-compliance-process-5-disclosure-infrastructure.md | Developer |
| Templates extracted to files  | Not started | Week 5      | phase5-disclosure-infrastructure/disclosure-templates/, not created | Developer |
| Website footer implementation | Not started | Week 6      | Production                                                      | Developer |
| GitHub README updates         | Not started | Week 6      | Repositories                                                    | Developer |
| CI/CD meta tag configuration  | Not started | Week 6      | Pipeline config                                                 | DevOps    |
| Testing validation report     | Not started | Week 6      | Not created                                                     | Developer |

## Priority Action Queue

### Immediate (Week 1-2)

| Priority | Action                                     | Effort     | Blocker |
| -------- | ------------------------------------------ | ---------- | ------- |
| 1        | Schedule legal consultation ($500-800)     | 1 hour     | Yes     |
| 2        | Create vendor compliance collection folder | 30 minutes | No      |
| 3        | Download current vendor terms of service   | 2 hours    | No      |
| 4        | Set up RSS feed aggregator                 | 30 minutes | No      |

### Short-term (Week 3-4)

| Priority | Action                                 | Effort  | Blocker |
| -------- | -------------------------------------- | ------- | ------- |
| 5        | Conduct legal consultation             | 2 hours | Yes     |
| 6        | Obtain counsel confirmation memo       | TBD     | Yes     |
| 7        | Verify watermark implementations       | 3 hours | No      |
| 8        | Populate AI tool inventory spreadsheet | 2 hours | No      |
| 9        | Subscribe to EU AI Office updates      | 1 hour  | No      |

### Medium-term (Week 5-6)

| Priority | Action                               | Effort  | Blocker          |
| -------- | ------------------------------------ | ------- | ---------------- |
| 10       | Implement website footer disclosures | 4 hours | Dependent on 5-6 |
| 11       | Update GitHub repository READMEs     | 2 hours | Dependent on 5-6 |
| 12       | Configure CI/CD meta tags            | 3 hours | Dependent on 5-6 |
| 13       | Run manual testing checklist         | 2 hours | No               |
| 14       | Set up automated validation scripts  | 3 hours | No               |

### Long-term (Post v1.0.0)

| Priority | Action                         | Frequency      | Owner     |
| -------- | ------------------------------ | -------------- | --------- |
| 15       | Quarterly compliance review, using quarterly-review-template.md | Every 3 months | Team      |
| 16       | Vendor policy review           | Monthly        | Developer |
| 17       | Guidance update scan           | Weekly         | Developer |
| 18       | Disclosure functionality check | Weekly         | Developer |

## Compliance Readiness Detailed Metrics

| Metric                    | Current    | Target | Gap   |
| ------------------------- | ---------- | ------ | ----- |
| Phase completion          | 20% (1/5)  | 100%   | -80%  |
| Phase documents drafted   | 100% (5/5) | 100%   | 0%    |
| Supporting artifacts      | 22% (2/9)  | 100%   | -78%  |
| Legal confirmation        | 0%         | 100%   | -100% |
| Vendor compliance docs    | 0%         | 100%   | -100% |
| Disclosure infrastructure | 0%         | 100%   | -100% |
| Audit trail preservation  | 0%         | 100%   | -100% |

The previous version of this table reported documentation artifacts at 100 percent. That figure counted only the five phase documents. All nine supporting artifacts listed under "Artifacts not yet created" remain outstanding, so the two rows are now reported separately.

> **Finding: Readiness bottleneck**, Legal confirmation is the largest open gap, but it constrains the accuracy of the compliance position rather than the ability to publish disclosures. Supporting artifacts, none of which exist yet, are the larger gap by volume and can be produced without waiting on counsel.

## File Naming Convention

Phase documents follow this pattern for filesystem sortability:

```
eu-ai-act-compliance-process-[phase]-[subject].md
```

Examples that follow the pattern:

- eu-ai-act-compliance-process-1-risk-assessment-complete.md
- eu-ai-act-compliance-process-3-technical-audit.md
- eu-ai-act-compliance-process-5-disclosure-infrastructure.md

### Directory convention

Each phase owns a subdirectory named `phase[N]-[subject]`, with no separator between `phase` and the number:

```
phase1-risk-assessment/
phase2-legal-review/
phase3-technical-audit/
phase4-guidance-engagement/
phase5-disclosure-infrastructure/
```

A phase subdirectory holds that phase's document plus every artifact the phase produces. New artifacts go in the subdirectory of the phase that produces them, so that phase work stays self-contained and can be archived or handed over as a unit.

Two kinds of file stay at the top of `eu-ai-act/` instead. Documents belonging to no single phase: this roadmap, the master process definition, the two background references, and the Universal Cake radar evaluation, which assesses the regime as a whole rather than any single phase. And ongoing operational material that outlives the phase programme: the current compliance status snapshot, and the quarterly review template. Quarterly review recurs indefinitely once the phases close, so filing it under a phase would imply it ends when that phase does.

### Known deviations

Three files predate the convention and do not match it. They are left in place because this roadmap and the phase documents cross-reference them by name.

| File                                                        | Deviation                                    |
| ------------------------------------------------------------ | -------------------------------------------- |
| compliance-scope-memo--eu-ai-act-article-50-phase-2-legal.md | Phase 2 document, does not use the prefix     |
| eu-ai-act-compliance-process-4--guidance-engagement.md       | Doubled hyphen before the subject             |
| eu-ai-act-current-compliance-status.md                       | Operational document, carries no phase number |

### Version suffixes

Version suffixes follow `automa/versioning/defaults/versioning-postfix-rules-v0-1-0.md`. A document below 1.0.0 carries its version as a hyphenated postfix, `<title-slug>-v0-1-0.md`. At 1.0.0 the postfix is dropped and the filename becomes stable.

An earlier version of this roadmap recorded that documents inside `radar/` used a dotted form, `-v0.1.0`. That was an accurate description of the directory at the time but not a rule, and the repository has since been normalized to the hyphenated form throughout.

Only the radar evaluation in this directory currently carries a version suffix. The phase documents carry none. Adding suffixes to them is deferred until they leave Draft status, because renaming now would break the cross-references recorded in this roadmap.

## Version Control Integration

| Branch                               | Purpose                         | Lifecycle            |
| ------------------------------------ | ------------------------------- | -------------------- |
| main                                 | Production compliance artifacts | Permanent            |
| feature/compliance-phase2-legal      | Legal review work               | Delete after merge   |
| feature/compliance-phase5-disclosure | Disclosure implementation       | Delete after merge   |
| test/compliance-validation           | Testing and QA                  | Delete after release |

Commit messages must include compliance phase references:

```
feat: Add AI assistance disclosure footer

- Implements Template A for documentation website
- Adds machine-readable meta tags for provenance
- Published as project policy, not an Article 50 obligation
- Related issue: #compliance-phase5
```

## Archive Requirements

Storage locations are relative to `radar/assess/compliance/eu-ai-act/` unless stated otherwise. None of the archive paths below exist yet.

| Document Type                   | Retention Period | Storage Location                                   |
| ------------------------------- | ---------------- | -------------------------------------------------- |
| Risk assessment records         | 3 years          | phase1-risk-assessment/archive/                    |
| Legal consultation notes        | 5 years          | Secure document store                              |
| Vendor compliance documentation | 3 years          | phase3-technical-audit/vendor-compliance-docs/     |
| Disclosure implementation logs  | 3 years          | phase5-disclosure-infrastructure/logs/             |
| Human review logs               | 3 years          | Project management tool                            |
| Guidance update logs            | 3 years          | phase4-guidance-engagement/guideline-change-log.md |

## Next Steps Summary

| Order | Action                          | Dependency | Timeline |
| ----- | ------------------------------- | ---------- | -------- |
| 1     | Schedule legal consultation     | None       | Week 1   |
| 2     | Collect vendor terms of service | None       | Week 1   |
| 3     | Set up monitoring subscriptions | None       | Week 2   |
| 4     | Conduct legal consultation      | Step 1     | Week 3   |
| 5     | Implement disclosures           | Step 4     | Week 5-6 |
| 6     | Complete v1.0.0 release         | Step 5     | Q4 2026  |

### Key Action Metrics

| Metric               | Value                 | Delta           | Direction |
| -------------------- | --------------------- | --------------- | --------- |
| Next critical action | Schedule consultation | Week 1 priority | Up        |
| Time to compliance   | 4-6 weeks             | From today      | Flat      |
| Estimated cost       | $500-800              | Legal only      | Flat      |

## License

This document, *EU AI Act Compliance File Structure and Roadmap*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Regulatory guidance

- [AI Office Guidelines](#eu-ai-act-article50-reference)
- [Article 50 full text](#eu-ai-act-text-reference)
- [Commission FAQ on Article 50](#ec-faq-reference)

### Compliance resources

- [SSL.com Implementation Guide](#ssl-compliance-reference)

## References

<a name="eu-ai-act-article50-reference"></a>European Commission. (2026a). Guidelines on transparency obligations for providers and deployers of AI systems. European Commission. https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations Return to citation

<a name="ec-faq-reference"></a>European Commission. (2026b). Transparency obligations under Article 50 of the AI Act [Frequently asked questions]. Shaping Europe's Digital Future. https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act Return to citation

<a name="eu-ai-act-text-reference"></a>EU Artificial Intelligence Act. (2026). Article 50: Transparency obligations for providers and deployers of certain AI systems. https://artificialintelligenceact.eu/article/50/ Return to citation

<a name="ssl-compliance-reference"></a>SSL.com. (2026). EU AI Act Article 50: A complete guide to AI transparency compliance. SSL.com. https://www.ssl.com/article/eu-ai-act-article-50-a-complete-guide-to-ai-transparency-compliance Return to citation

<a name="asd-ste100-reference"></a>ASD-STAN. (2026). ASD-STE100 Simplified Technical English specification. AeroSpace and Defence Industries Association of Europe. https://asdstan.org/asd-ste100/ Return to citation

<a name="apa-apa7-reference"></a>American Psychological Association. (2020). Publication manual of the American Psychological Association (7th ed.). https://doi.org/10.1037/0000165-000 Return to citation

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.7.0   | Draft  | Repointed the radar evaluation to its renamed file and replaced the version suffix guidance with a reference to the new versioning automa, following the repository-wide normalization to the hyphenated postfix form |
| 0.6.1   | Draft  | Corrected the version suffix guidance, which asserted a repository-wide `-v0-1-0` convention. The repository actually splits by location: dotted inside radar/, hyphenated outside it. This directory is inside radar/, so the dotted form is correct here |
| 0.6.0   | Draft  | Added the Universal Cake radar evaluation of Article 50 against evaluation metrics v0.3.1, filed at the top of eu-ai-act/ because it assesses the regime as a whole rather than any single phase |
| 0.5.0   | Draft  | Created the quarterly review template at the top of eu-ai-act/ and moved it from planned to existing: added to the tree and document roles, removed from the planned artifact table, supporting artifact metric raised from 1 of 9 to 2 of 9, and the long-term action queue now names the template |
| 0.4.2   | Draft  | Recorded that the quarterly review template belongs at the top of eu-ai-act/ rather than in a phase subdirectory, and stated the reason in the directory convention: quarterly review recurs indefinitely once the phases close |
| 0.4.1   | Draft  | Corrected the tree, which still placed this roadmap one level above eu-ai-act/ after it was moved into that directory; reworded the planned artifact table so it cannot be read as describing files that exist |
| 0.4.0   | Draft  | Adopted phase subdirectories as the organizing structure: each of the five phases now owns a phase[N]-[subject] directory holding its phase document and supporting artifacts, while cross-phase documents stay at the top of eu-ai-act/. Repointed the tree, document roles, planned artifacts, all phase matrix Location cells, and the archive paths. Added a directory convention section |
| 0.3.2   | Draft  | Recorded the Phase 1 interview notes as created and moved the Phase 1 matrix row from Not started to Complete; added the phase1-risk-assessment/ subdirectory to the tree and the notes to the document roles table; updated the supporting artifact metric from 0 of 9 to 1 of 9 |
| 0.3.1   | Draft  | Recorded that the editorial review and transparency policies predate AI adoption and continue past v1.0.0, and that vendor compliance documents are organised per vendor so later frameworks can share the collection |
| 0.3.0   | Draft  | Added a Regulatory Status section recording that Article 50 has applied since 2 August 2026 and that the Article 50(2) marking obligation carries a grace period to 2 December 2026 for pre-existing systems; recorded that disclosure is published as project policy rather than as an Article 50 obligation, following the corrected exemption analysis in Phase 1; corrected the dependency table so Phase 5 depends on Phase 3 rather than Phase 4, matching the phase documents; separated phase status from document status; split the artifact metric, which reported 100 percent while counting only phase documents; fixed the dead Code of Practice anchor and added the Article 50 text and Commission FAQ as references |
| 0.2.0   | Draft  | Replaced the hypothetical compliance-repo/ tree with the actual radar/assess/compliance/ layout; repointed every Location and Storage Location cell at real filenames or explicitly labelled planned paths; added document role and not-yet-created artifact tables; corrected the Phase 1 matrix to list the risk assessment document rather than nonexistent interview notes; aligned Phase 3 and Phase 4 target dates with the phase documents; restored the directory tree and commit message code blocks to multi-line form; rewrote the naming convention to describe actual practice and record known deviations; replaced em dashes with commas |
| 0.1.0   | Draft  | Initial organization framework with phase dependencies, completion tracking, and ASD-STE100/APA 7 compliance |
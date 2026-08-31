# EU AI Act's Transparency Rules for using AI to collaboratively produce website content

Version: 0.1.1 Status: Draft Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

This document summarises the EU AI Act transparency rules as they apply to a website that uses AI to produce content collaboratively with its users. It covers the machine-readable marking obligation held by providers, the human-readable labelling obligation held by deployers, the available compliance pathways, and the exemptions. It is background reference material for the compliance phases rather than an assessment of this project, and it describes the general collaborative-platform scenario, which is broader than Universal Cake's own internal-tool usage.

## Core Framework

Based on current regulatory information, here are the key requirements for a website using AI to collaboratively produce content under the EU AI Act's Transparency Rules:

The **EU AI Act (Regulation (EU) 2024/1689)** establishes mandatory transparency obligations under **Article 50**, which became legally binding on **August 2, 2026**. The rules apply to any organization serving EU users, regardless of where the company is based.

## Key Obligations for Collaborative AI Content Platforms

### Machine-Readable Marking (Provider Obligation)
If your platform provides generative AI systems that create content, you must:

- Embed **machine-readable watermarks** or detectable marks in all synthetic output (audio, images, videos, text)
- Ensure technical solutions are "effective, interoperable, robust and reliable"
- Consider alignment with standards like **C2PA Content Credentials**

### Human-Readable Labeling (Deployer Obligation)
As an organization professionally using AI systems, you must:

- Clearly disclose AI-generated or manipulated content presented to the public
- Label deepfakes (images, audio, video designed to appear real)
- Disclose AI-generated text on **matters of public interest**
- Make AI interactions obvious when users interact with AI instead of humans

### Available Compliance Pathways
The EU created a **Code of Practice on Transparency of AI-Generated Content** (finalized ~June 2026) which provides:

- Voluntary standards and icons for marking/labeling
- A way to demonstrate compliance when signed
- Alternative equivalent safeguards if not adhering to the code

## Practical Checklist for Your Platform

Compliance Deadline

Aug 2, 2026

–Active

Applicability

Global w/ EU users

–Broad reach

Max Penalty

€15M or 3%

▼of turnover

### Specific Actions Needed

1. **Map all AI systems** in your platform's workflow
2. **Implement watermarking technology** for AI-generated outputs
3. **Create disclosure mechanisms** (UI labels, metadata, notices)
4. **Establish editorial review processes** (may qualify for exemptions)
5. **Contractual compliance** with AI vendors if using third-party models
6. **Monitor the Code of Practice** updates for implementation guidance

## Important Exemptions & Nuances

| Exemption                   | Details                                                      |
| --------------------------- | ------------------------------------------------------------ |
| **Human editorial control** | Content undergoing human review/editorial oversight may be exempt |
| **Law enforcement**         | Tools used to detect/prevent/investigate criminal offenses   |
| **Artistic/creative works** | Deepfake disclosure limited to existence notice (not full detail if it hampers artistic expression) |
| **Obvious AI interactions** | Chatbot disclosure not needed "when obvious"                 |

## Risk Considerations

Collaborative content complexitywarning

When users collaborate with AI, determining who is 'provider' vs. 'deployer' becomes complex. Your platform may bear provider responsibility for AI-generated outputs, while power-user accounts may qualify as deployers requiring their own disclosures.

**Administrative fines** can reach **€15 million or 3% of worldwide annual turnover** (whichever is higher), with lower caps for SMEs/startups. Providing incorrect/misleading information carries separate penalties up to €7.5M or 1%.

## Recommended Next Steps

1. **Legal review** of your platform's specific AI use cases and user roles
2. **Technical audit** of AI content generation pipeline
3. **Engage with the AI Office** guidance materials (https://digital-strategy.ec.europa.eu)
4. **Consider joining the Code of Practice** signatory process for compliance clarity
5. **Build disclosure infrastructure** into your content management systems

The European Commission continues publishing implementation guidelines, so monitoring https://digital-strategy.ec.europa.eu/en/policies/code-practice-ai-generated-content and https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations is advisable for ongoing updates.

## License

This document, *EU AI Act's Transparency Rules for using AI to collaboratively produce website content*, by **Christopher Steel**, with AI assistance from **Lumo (Proton)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes                                                        |
| ------- | ------ | ------------------------------------------------------------ |
| 0.1.1   | Draft  | Added the Abstract section required by the web-ready APA 7 guide |
| 0.1.0   | Draft  | Added the version block, license, and changelog this document previously lacked; removed horizontal rules and heading numbers per the markdown automa defaults; corrected the closing note, which described the 2 August 2026 date as a future deadline after it had passed |
## NorthMail preliminary Universal Cake evaluation

**Assessment date:** 15 September 2026
**Lifecycle:** Assess
**Evidence standard:** primarily vendor documentation; independent verification is limited.

### Scorecard

| Metric                              | Rating          | Evidence         | Notes                                                        |
| ----------------------------------- | --------------- | ---------------- | ------------------------------------------------------------ |
| Accessibility                       | Moderate        | Claimed          | Dedicated mobile apps and web interface; no published independent WCAG assessment found |
| Multilingual integration            | Moderate        | Claimed          | French/English presence, but depth of multilingual support is unclear |
| Affordability                       | Strong          | Verified         | Free 1 GB tier; ad-free 100 GB tier is CA$0.99/month         |
| Hardware/connectivity accessibility | Strong          | Inferred         | Web + iOS + Android + desktop/cloud access                   |
| Cognitive accessibility             | Moderate        | Claimed          | Product positioning emphasizes simplicity, but formal cognitive-accessibility evidence is absent |
| Representation                      | Unknown         | Unknown          | Little evidence regarding representation or bias testing     |
| Resilience                          | Moderate        | Inferred         | Canadian hosting and multiple access methods, but detailed resilience/DR metrics are not public |
| Sovereignty & Privacy               | Moderate–Strong | Claimed          | Canadian ownership/hosting and limited data-sharing claims   |
| Interaction patterns                | Moderate        | Inferred/Claimed | No targeted advertising based on email content, but free webmail contains advertising |
| Environment                         | Unknown         | Unknown          | No meaningful energy/carbon metrics found                    |
| Security                            | Moderate        | Claimed          | Encryption, spam/phishing controls and 2FA; independent audit evidence not found |
| Longevity                           | Unknown         | Inferred         | Relatively young provider with a small organization          |
| Content endurance                   | Strong          | Inferred         | IMAP/SMTP available on paid tier; standard email protocols   |
| Exit & portability                  | Strong          | Inferred         | IMAP/SMTP substantially reduces email lock-in                |
| Adjustability & support             | Moderate        | Claimed          | 2FA, app passwords, IMAP/SMTP, mobile apps                   |
| Market position                     | Unknown         | Unknown          | Small provider; concentration, dependency and contributor information limited |

NorthMail describes itself as Québec-based and 100% Canadian-owned, with servers hosted in Canada. Its public materials also identify the owner as **9387-6084 Québec Inc.** ([NorthMail](https://northmail.ca/?utm_source=chatgpt.com))

## 1. Inclusive

### Accessibility — Moderate

NorthMail has webmail plus dedicated iOS and Android applications. The iOS listing identifies the developer as 9387-6084 Québec Inc. ([App Store](https://apps.apple.com/ca/app/northmail/id6744868850?utm_source=chatgpt.com))

However, I did **not** find the sort of accessibility documentation that Béluga publishes: WCAG target, assistive-technology testing, keyboard testing, focus-order testing, accessibility issue tracking, etc.

So I would leave this **Moderate / Claimed**, rather than assuming accessibility from the existence of mobile apps.

### Multilingual integration — Moderate

NorthMail is Québec-based and has both French and English-facing material. However, I found substantially less evidence of a formal multilingual design strategy than with Béluga.

**Evidence gap:** language coverage of the actual application, help system, accessibility content and user-generated content is not well documented.

### Affordability — Strong

This is one of NorthMail's strongest metrics.

The free account provides **1 GB**, while the ad-free plan provides **100 GB for CA$0.99/month**, including IMAP/SMTP access and Castor. ([NorthMail](https://northmail.ca/?utm_source=chatgpt.com))

That makes the service unusually accessible financially.

There is, however, an important interaction-pattern distinction:

> Free = advertising
> Paid = ad-free

NorthMail's Terms explicitly say that advertisements may appear in the free webmail interface. ([NorthMail](https://northmail.ca/terms?utm_source=chatgpt.com))

## 2. Agency

### Sovereignty & Privacy — Moderate–Strong

NorthMail makes several strong sovereignty claims:

- Canadian ownership
- Québec-based organization
- Canadian-hosted servers
- Canadian data residency
- no sharing of email content with third parties
- no advertising based on reading email content
- Canadian-hosted service infrastructure

([NorthMail](https://northmail.ca/privacy?utm_source=chatgpt.com))

That is meaningful alignment with the Universal Cake sovereignty model.

But there is an important difference from Béluga: **NorthMail publishes considerably less infrastructure detail.**

For example, I did not find public documentation comparable to Béluga's detailed descriptions of:

- physical infrastructure ownership
- specific data centres
- HSM architecture
- backup architecture
- container orchestration
- security monitoring
- vulnerability-response commitments
- external security audits

Therefore I would not promote this to **Verified** without testing or additional documentation.

### AI — an important distinction

NorthMail's **Castor** assistant is powered by Cohere. NorthMail says that email content is sent to Cohere **only when the user explicitly opts into Castor email indexing**. ([NorthMail](https://northmail.ca/privacy?utm_source=chatgpt.com))

That is a good agency pattern:

**AI processing is opt-in rather than silently applied to everyone's mailbox.**

But this introduces a new dependency:

> NorthMail → Castor → Cohere

The privacy policy does not give enough technical information to evaluate model provenance, retention, training use, deletion semantics or output provenance in depth.

So:

| AI metric                 | Rating             |
| ------------------------- | ------------------ |
| Training-data consent     | Strong / Claimed   |
| Model transparency        | Moderate / Unknown |
| Output provenance         | Unknown            |
| User control              | Strong / Claimed   |
| Third-party AI dependency | Moderate concern   |

## 3. Interaction patterns

This is where NorthMail is more complicated than it initially appears.

### Positive patterns

NorthMail explicitly says it does **not** scan/read email for targeted advertising. Automated analysis can nevertheless occur for spam, phishing and abuse prevention. ([NorthMail](https://northmail.ca/terms?utm_source=chatgpt.com))

It also provides:

- 2FA
- authenticator-app support
- application passwords
- IMAP/SMTP for paid users
- no commitment on the paid subscription
- account portability through conventional email protocols

([NorthMail](https://northmail.ca/?utm_source=chatgpt.com))

### Advertising

The free tier does contain advertising.

That means it does **not** meet the Universal Cake ideal of:

> quiet by default

or

> no attention extraction.

But the important distinction is that NorthMail says the ads are **not based on reading email content**. ([NorthMail](https://northmail.ca/terms?utm_source=chatgpt.com))

I would therefore classify this as:

**Moderate rather than Weak.**

## 4. Security

NorthMail claims encryption and Canadian-hosted infrastructure and provides 2FA. Its FAQ documents authenticator-app 2FA and application passwords for IMAP clients. ([NorthMail](https://northmail.ca/?utm_source=chatgpt.com))

However, I found considerably less publicly documented security architecture than Béluga.

Most importantly, I did not find evidence of:

- independent penetration testing
- SOC 2
- ISO 27001
- published security audit
- formal vulnerability disclosure programme
- published incident-response metrics
- detailed supply-chain security

Therefore:

**Security: Moderate / Claimed**

rather than Strong.

## 5. Environment

**Unknown.**

I found no meaningful published metrics for:

- energy per mailbox
- data-centre PUE
- renewable-energy percentage
- embodied hardware impact
- bandwidth impact
- storage efficiency
- AI inference energy
- carbon accounting

This is a significant Universal Cake evidence gap.

The Castor AI component makes this particularly relevant because AI processing introduces an additional environmental dimension that ordinary email hosting does not.

## 6. The product itself

### Longevity — Unknown

NorthMail appears to be a relatively young organization. Its LinkedIn profile identifies it as founded in 2025 and currently lists a very small organization. ([LinkedIn](https://ca.linkedin.com/company/northmail-ca?utm_source=chatgpt.com))

That does **not** mean it is inherently fragile. It means there isn't yet much evidence from which to assess long-term institutional durability.

I would therefore avoid penalizing it beyond:

**Longevity: Unknown**

### Content endurance — Strong

This is one of NorthMail's strongest structural characteristics.

Paid accounts support standard:

- IMAP
- SMTP

with documented server settings. ([NorthMail](https://northmail.ca/faq?utm_source=chatgpt.com))

That means mail can be accessed through ordinary interoperable protocols rather than being trapped exclusively inside the NorthMail interface.

### Exit & portability — Strong

This is another important Universal Cake positive.

If email can be retrieved through IMAP and sent through SMTP, the basic mailbox relationship is substantially more reversible than a proprietary messaging platform.

However, I'd still want an actual migration test before marking the metric **Verified**:

1. create mailbox
2. receive representative corpus
3. connect with IMAP
4. export complete mailbox
5. migrate to another provider
6. verify folders, dates, attachments and Unicode
7. delete NorthMail account
8. verify no unexpected dependency remains

That would turn the current **Inferred** rating into **Verified**.

## 7. Market position

This is currently **Unknown**.

NorthMail's small scale has both sides:

**Potential positive**

- less institutional concentration than Google/Microsoft
- Canadian ownership
- relatively independent product

**Evidence gap**

- small organization
- limited public information about infrastructure dependencies
- no public contributor/forkability model
- no published API stability policy
- no evidence that NorthMail itself is open source

This last point is important.

NorthMail uses open-source technologies in its broader ecosystem, but that is **not equivalent to NorthMail itself being open source**. I found no evidence that its proprietary service/application code is available for independent modification or forking.

## 8. Power relationship

Using your specific question:

> “If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?”

NorthMail performs reasonably well **for email** because IMAP/SMTP provide an escape route.

The biggest asymmetry is that:

- free users get advertising
- IMAP/SMTP is restricted to paid users
- the broader NorthMail ecosystem contains cloud storage, office, calendar, contacts, chat and AI

That creates a potential distinction between **email portability** and **whole-platform portability**.

Email: **Strong**

Whole ecosystem: **Unknown**

That distinction is worth preserving in the Universal Cake evaluation.

## Gate assessment

| Gate                                   | Current assessment                              |
| -------------------------------------- | ----------------------------------------------- |
| Unavoidable telemetry/content exposure | **No failure identified**                       |
| Licence incompatibility                | **No failure identified**                       |
| Accessibility floor                    | **Not sufficiently verified**                   |
| Exit/data extraction                   | **Provisionally passes**                        |
| Content extraction in usable form      | **Strong for email; broader ecosystem unknown** |

### Lifecycle recommendation

**Assess — yes**

**Trial — not yet**

I would want at least:

- accessibility testing
- mailbox export/import test
- security architecture clarification
- incident/vulnerability process
- backup/DR documentation
- AI/Castor data-flow documentation
- environmental information

before moving it into the **Trial** phase.

## NorthMail vs. Béluga

The interesting result is that the two services expose different strengths.

| Universal Cake dimension    | NorthMail      | Béluga                                                    |
| --------------------------- | -------------- | --------------------------------------------------------- |
| Canadian sovereignty        | Strong         | Strong                                                    |
| Québec identity             | Strong         | Strong                                                    |
| Free access                 | Strong         | Strong                                                    |
| Advertising                 | Free tier      | No advertising                                            |
| Email interoperability      | Strong on paid | Strong                                                    |
| AI opt-in                   | Strong claim   | Strong claim                                              |
| Privacy documentation       | Moderate       | Strong                                                    |
| Infrastructure transparency | Limited        | Strong                                                    |
| Accessibility documentation | Limited        | Stronger                                                  |
| Security documentation      | Limited        | Stronger                                                  |
| Environmental evidence      | Unknown        | Unknown                                                   |
| Whole-platform portability  | Unknown        | Stronger evidence                                         |
| Institutional maturity      | Unknown        | Unknown                                                   |
| Open-source service itself  | No evidence    | No — foundations are open source, platform is proprietary |

The most significant Universal Cake distinction is therefore **not simply “which is more private?”** Both make strong Canadian/privacy claims. The distinction is the **amount of independently inspectable evidence supporting those claims**.

NorthMail currently looks like a **very inexpensive, Canadian, interoperable email service with a broader collaboration ecosystem**, while Béluga exposes considerably more of its architecture, governance and operational model for evaluation. ([NorthMail](https://northmail.ca/privacy?utm_source=chatgpt.com))

**Provisional Universal Cake assessment: NorthMail = Moderate–Strong overall alignment, with substantial Unknowns.** The most important next step would be converting the major Claimed/Inferred ratings into Verified evidence rather than attempting to aggregate them into a numerical score.

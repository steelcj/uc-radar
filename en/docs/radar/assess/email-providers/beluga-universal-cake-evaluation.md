# universal-cake-evaluation--beluga

Yes. I evaluated **Béluga as it exists in September 2026**, using your v0.5.1 metrics rather than substituting a conventional privacy/security score. I treated vendor statements as **Claimed** unless the public material itself provides something that can be directly checked from the architecture or interface. I also did **not** collapse the results into a single score.

One important finding up front: **Béluga is unusually well aligned with the Sovereignty & Privacy side of your framework, but it is still an open-beta, young service with several areas that should remain Unknown or Moderate until independently verified.** That distinction matters under your metrics.

[Béluga](https://beluga.quebec/en/?utm_source=chatgpt.com) currently describes itself as an open-beta Québec collaborative suite operated by Technologies Shelter-Bay inc., with its own Québec-owned bare-metal infrastructure and open-source foundations. ([Beluga](https://beluga.quebec/en/?utm_source=chatgpt.com))

## Universal Cake Evaluation: Béluga

**Product:** Béluga
**Version evaluated:** Open beta, September 2026
**Evaluator:** Christopher Steel / Universal Cake evaluation
**Date:** 2026-09-15
**Radar ring:** **Assess**

### Summary

| Metric area              | Rating                            | Evidence                   | Principal observation                                        |
| ------------------------ | --------------------------------- | -------------------------- | ------------------------------------------------------------ |
| Accessibility            | **Moderate**                      | Claimed                    | Serious WCAG 2.1 AA programme, but independent audit is not yet complete |
| Multilingual integration | **Strong**                        | Claimed                    | Native FR-CA and EN-CA; Québec French is treated as a first-class language |
| Affordability            | **Strong**                        | Verified/Claimed           | Permanent free tier; CAD pricing; no advertising             |
| Cognitive accessibility  | **Moderate**                      | Claimed                    | Clear product orientation, but limited independent UX evidence |
| Representation           | **Unknown**                       | Claimed                    | Some transparency, but insufficient public evidence about user/community representation |
| Compatibility            | **Strong**                        | Claimed                    | IMAP, CalDAV, CardDAV; broad standards-based client access   |
| Resilience               | **Moderate**                      | Claimed                    | Encrypted backups and owned infrastructure, but young service/open beta |
| Sovereignty & Privacy    | **Strong**                        | Claimed/partially Verified | Exceptional Canadian/Québec data-sovereignty architecture    |
| Power imbalance          | **Strong**                        | Inferred/Claimed           | Low exit friction, open protocols, monthly billing           |
| Interaction patterns     | **Strong**                        | Claimed                    | No advertising/profiling; privacy-oriented defaults          |
| Environment              | **Unknown/Moderate**              | Unknown                    | Architecture is informative, but actual energy/carbon measurements aren't published |
| Security                 | **Moderate**                      | Claimed                    | Good architecture and disclosure; independent audit/certification not yet complete |
| Longevity                | **Unknown**                       | Inferred                   | Young two-founder company and open beta make long-term evidence limited |
| Content endurance        | **Strong**                        | Inferred                   | Standard protocols substantially reduce dependence on Béluga's UI |
| Exit & portability       | **Strong**                        | Claimed                    | IMAP/CalDAV/CardDAV and export mechanisms provide genuine exit paths |
| Adjustability & support  | **Moderate**                      | Claimed                    | Open foundations and accessible operator, but proprietary integration layer |
| Market position          | **Strong/Unknown**                | Mixed                      | Low concentration risk from the product itself; contributor concentration is potentially high |
| **Gates**                | **No current failure identified** | —                          | **Not yet Trial-ready because verification is incomplete**   |

------

# Inclusive

## Accessibility

**Rating: Moderate — Claimed**

Béluga explicitly targets **WCAG 2.1 AA** and documents semantic HTML, keyboard navigation, labels, programmatic error association, alternative text and reduced-motion support. It reports testing with VoiceOver and NVDA/Firefox. ([Beluga](https://beluga.quebec/accessibilite/))

This is unusually good documentation for a young service.

However, Béluga itself identifies several outstanding items:

- independent third-party WCAG audit;
- complete focus-order review;
- French screen-reader announcements in Docs;
- video captioning.

Those limitations prevent a Strong rating under your framework, particularly because your accessibility gate requires actual accessibility rather than an accessibility intention.

**Gate:** not failed, but **not yet Verified**.

### Multilingual integration

**Rating: Strong — Claimed**

This is one of Béluga's strongest areas.

The platform is explicitly **FR-CA / EN-CA**, and the company says Québec French is native rather than a late translation of French from France. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

That fits particularly well with your language-centred evaluation model.

The remaining question is whether community localization is structurally possible rather than merely whether two languages are supplied. I found insufficient evidence to establish that.

### Affordability

**Rating: Strong — Verified**

The individual free plan is:

- $0/month;
- permanent rather than a time-limited trial;
- 1 GB shared across mail/files/documents;
- `@moncourriel.quebec`;
- no advertising;
- no profiling;
- IMAP/CalDAV/CardDAV access.

Paid individual plans are listed in CAD, starting at $10/month for Plus. ([Beluga](https://beluga.quebec/tarification/?utm_source=chatgpt.com))

The permanent free tier is particularly important under your **Agency** metrics because Béluga does not appear to use the free tier as a temporary acquisition funnel.

## Representation

**Rating: Unknown — Claimed**

There is evidence of deliberate Québec representation in the product itself: native Québec French, Québec jurisdiction and a Québec-oriented mission. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

But your Representation metric asks a harder question:

> Who participates in making the decisions, and who is absent?

I could not establish sufficient public evidence regarding:

- disability representation;
- compensated accessibility research;
- demographic/community representation;
- systematic user research;
- disaggregated usability testing.

I would therefore leave this **Unknown rather than infer Strong from the product's orientation**.

------

# Agency

## Sovereignty & Privacy

**Rating: Strong — Claimed / Inferred**

This is where Béluga stands out.

Béluga says:

- the operator is a Québec corporation;
- servers are owned by Béluga;
- servers are located in Québec;
- it does not use AWS, Azure or Google Cloud;
- encryption keys/HSM infrastructure are operated in Québec;
- backups are encrypted and retained in Québec;
- standard email/calendar/contact protocols remain available;
- there is no advertising or profiling;
- user content is not used for AI training. ([Beluga](https://beluga.quebec/confidentialite/))

The privacy policy is unusually concrete. It identifies operational telemetry, account data and service content rather than simply saying "we respect your privacy." ([Beluga](https://beluga.quebec/confidentialite/))

There is also a very useful disclosure of residual external processors:

- Stripe, United States;
- Mistral AI, France, for optional meeting transcription;
- hCaptcha, United States;
- Element, United Kingdom, for limited Convo notification metadata;
- Cyberimpact, Québec;
- OVHcloud, Québec, for encrypted backups. ([Beluga](https://beluga.quebec/sous-traitants/?utm_source=chatgpt.com))

That transparency is itself a positive result under your **information asymmetry** principle.

### Important qualification

Béluga is **not end-to-end encrypted email**.

Its Convo messaging is E2EE, but ordinary email is protected through transport encryption and encryption at rest. The operator necessarily operates the mail infrastructure and can therefore technically access ordinary mailbox content. ([Beluga](https://beluga.quebec/securite/))

That's an important distinction from something like Proton's encrypted-mail architecture.

------

## Vendor → user power imbalance

**Rating: Strong — Inferred**

Béluga does unusually well on your reversibility test.

### Exit cost

Likely **low**.

Email uses IMAP; calendars use CalDAV; contacts use CardDAV. Béluga explicitly presents these as the mechanisms by which users can continue accessing their information if they leave. ([Beluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

This means leaving does not require reverse-engineering Béluga's database.

### Data portability

**Rating: Strong — Claimed**

The service explicitly supports:

- IMAP;
- CalDAV;
- CardDAV;
- vCard;
- iCalendar.

That is substantially better than merely offering a proprietary "download my data" archive. ([Beluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

### Terms volatility

**Rating: Unknown**

The current terms promise at least 30 days' notice for substantial changes. ([Beluga](https://beluga.quebec/en/terms/))

But your metric asks for roughly five years of actual changes. Béluga is too young to establish that history.

### Pricing asymmetry

**Rating: Strong**

Pricing is public, in CAD, and monthly. The company says plans can be cancelled without a long-term commitment. ([Beluga](https://beluga.quebec/en/?utm_source=chatgpt.com))

This is a good example of your **asymmetry-of-information** test working in practice.

------

# Interaction Patterns

**Rating: Strong — Claimed**

The public design strongly favours:

- no advertising;
- no profiling;
- no third-party analytics on the marketing site;
- privacy-oriented defaults;
- no AI training on user content;
- monthly billing;
- explicit data-sharing controls;
- explicit consent for AI connectors. ([Beluga](https://beluga.quebec/confidentialite/))

The AI connector model is particularly interesting under your v0.5.0 criteria: an external AI assistant receives Béluga data only after explicit consent, with access scopes selected by the user, and the connection can be revoked. ([Beluga](https://beluga.quebec/en/terms/))

That is much closer to **transparent personalization/automation** than opaque engagement optimization.

I would still mark the rating **Claimed**, because we have not performed interaction testing.

------

# Environment

**Rating: Unknown — insufficient measurement**

This is one of the areas where your framework appropriately resists marketing claims.

Béluga owns its bare-metal infrastructure and uses K3s rather than simply renting hyperscale cloud infrastructure. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

But I found no published:

- kWh/user;
- PUE;
- server utilization;
- bandwidth consumption;
- embodied-carbon estimate;
- Software Carbon Intensity measurement;
- hardware lifetime data.

So we have an interesting architectural hypothesis, but not enough evidence for Strong.

I'd record:

> **Unknown: operational energy and material impacts have not yet been measured publicly.**

That is exactly the kind of Unknown your framework is designed to preserve.

------

# Security

**Rating: Moderate — Claimed**

There is a solid technical foundation:

- AES-256 at rest;
- TLS 1.2+;
- HSMs in Québec;
- Keycloak;
- MFA;
- passkeys;
- application passwords;
- owned bare-metal infrastructure;
- application firewall;
- VPN-only administration;
- encrypted daily backups;
- second Québec site;
- vulnerability reporting process. ([Beluga](https://beluga.quebec/securite/))

Béluga also publishes its major architectural components: Postfix, Dovecot, Rspamd, MinIO, Matrix/Synapse, Radicale, Keycloak, etc. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

The limitation is significant:

> **Béluga is an open beta and does not yet have SOC 2 or ISO 27001 certification or an independent external security audit.**

The company says an independent audit is planned after public launch. ([Beluga](https://beluga.quebec/securite/))

So I would not give Security Strong yet.

### Security gate

Your framework allows a project to create a security gate. If you did so for an email provider, **"independent security assessment completed"** would be a reasonable candidate.

Béluga would currently fail that *additional* gate, but it does **not** fail one of your four default gates.

------

# The Product or Service Itself

## Longevity

**Rating: Unknown**

This is probably Béluga's largest structural uncertainty.

It is a young company with two founders and an open-beta product. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

That does not mean it is likely to fail. Your framework specifically says to evaluate what is observed rather than what is hoped.

What is currently missing:

- long-term release history;
- mature contributor ecosystem;
- five-year operational history for Béluga;
- demonstrated succession/governance model;
- meaningful bus-factor data.

I would therefore mark Longevity **Unknown**, not Weak.

## Content endurance

**Rating: Strong — Inferred**

This is another major strength.

Email is not trapped inside Béluga's proprietary interface. IMAP means ordinary mail clients can access it. Calendar and contacts similarly use established protocols. ([Beluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

That gives Béluga an important architectural property:

> **The service can disappear without necessarily taking the user's content with it.**

That is almost exactly your distinction between an **enhancement layer** and a **container**.

## Exit and portability

**Rating: Strong — Claimed**

This is one of the clearest passes in the evaluation.

The service exposes established protocols rather than creating a proprietary data island:

```text
Email       → IMAP
Calendar    → CalDAV
Contacts    → CardDAV
Contacts    → vCard
Calendar    → iCalendar
```

([Beluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

**Default gate: Pass, subject to verification.**

For a Trial-ring evaluation, I would actually create a test account, populate it with representative mail/calendar/contact data, migrate it to another provider and verify that nothing material is lost.

That would convert this from **Claimed** to **Verified**.

------

# Adjustability and support

**Rating: Moderate — Inferred**

There are good foundations:

- open-source components;
- published architecture;
- Québec-based operator;
- public security contact;
- public accessibility contact;
- documented infrastructure;
- open standards. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

However, Béluga itself is not presented as a fully open-source product. The company says its **foundations** are open source while describing its own application/integration work separately. Its Terms also explicitly state that the platform's source code is the company's property. ([Beluga](https://beluga.quebec/conditions/?utm_source=chatgpt.com))

That distinction is important.

I would therefore **not** give Béluga the same fork-and-fix rating as a fully AGPL-licensed mail platform.

------

# Market Position

**Rating: Strong / Unknown**

The product currently creates relatively little platform-level lock-in because it deliberately uses open standards.

The interesting metric here is **forkability**.

The underlying components are forkable:

- Postfix
- Dovecot
- Rspamd
- Matrix/Synapse
- MinIO
- Keycloak
- etc.

But **Béluga itself is not currently presented as open-source**. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

So:

> **Component-level forkability: Strong**
> **Béluga application-level forkability: Weak/Unknown**

This is an important distinction for your framework.

### Contributor concentration

**Unknown**

I found that the company describes itself as a two-founder organization, but not sufficient public evidence to calculate contributor concentration for the Béluga application itself. ([Beluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

Given your metric, this should remain **Unknown** rather than assuming that two founders means a specific bus factor.

------

# AI-specific metrics

Béluga is actually interesting here.

### Training-data consent

**Strong — Claimed**

Béluga explicitly states that email, files and conversations are not used to train its AI models or third-party models. ([Beluga](https://beluga.quebec/fonctionnalites/?utm_source=chatgpt.com))

### Model transparency

**Moderate/Unknown**

The current Matelot AI component is still under development. Béluga has disclosed that Mistral AI is used for meeting transcription and that this requires explicit consent. ([Beluga](https://beluga.quebec/sous-traitants/?utm_source=chatgpt.com))

But there isn't enough evidence yet to assess model weights, training provenance or complete model supply-chain transparency.

### Output provenance

**Unknown**

I did not find sufficient evidence concerning persistent model-version provenance for AI-generated output.

### AI connector control

**Strong — Claimed**

This is one of the better aspects: AI connectors require explicit consent, access scopes are selectable, and connections can be revoked. ([Beluga](https://beluga.quebec/en/terms/))

------

# Gates

Your four default gates produce an interesting result.

| Gate                                   | Béluga                    |
| -------------------------------------- | ------------------------- |
| Unavoidable telemetry/content exposure | **No failure identified** |
| Incompatible licence                   | **Project-dependent**     |
| Accessibility floor                    | **Not yet Verified**      |
| No usable exit                         | **No failure identified** |

### The important one: Exit

I would provisionally mark this **Pass**, because IMAP/CalDAV/CardDAV provide real exit mechanisms rather than a proprietary archive. ([Beluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

### Accessibility

I would **not yet mark this Verified**.

Béluga has explicitly committed to WCAG 2.1 AA and has identified remaining accessibility work, including an independent audit that has not yet occurred. ([Beluga](https://beluga.quebec/accessibilite/))

Therefore the appropriate radar state under your lifecycle rules is:

> **Assess, not Trial.**

------

# One issue I would flag for the Béluga evaluation

There is a **documentation inconsistency worth resolving**.

The privacy policy says account data is retained during the contract plus 30 days for export, and service content is retained until deletion or 30 days after termination. ([Beluga](https://beluga.quebec/confidentialite/))

The Terms of Service, however, say that deleting an account creates a **90-day recovery period**, after which deletion is permanent. ([Beluga](https://beluga.quebec/en/terms/))

That is exactly the sort of information asymmetry your framework should surface.

I'd record it as:

> **Unknown: account/content deletion and recovery periods are inconsistent between the Privacy Policy and Terms of Service and should be clarified before Trial.**

------

# Preliminary radar assessment

If I were entering Béluga into your radar **without averaging the ratings**, I would record:

```text
Béluga
Ring: Assess

Strong:
  - Multilingual integration
  - Affordability
  - Sovereignty & privacy
  - Vendor/user reversibility
  - Interaction patterns
  - Content endurance
  - Exit architecture

Moderate:
  - Accessibility
  - Cognitive accessibility
  - Resilience
  - Security
  - Adjustability/support

Unknown:
  - Representation
  - Environmental impact
  - Longevity
  - Terms-of-service volatility
  - Contributor concentration
  - AI model transparency
  - AI output provenance

Gates:
  - Telemetry/content exposure: no failure identified
  - Licence: project-dependent
  - Accessibility floor: not yet independently verified
  - Exit/portability: provisional pass

Open issues:
  - Open beta
  - No independent security audit yet
  - No SOC 2 / ISO 27001 certification
  - Béluga application itself is proprietary despite open-source foundations
  - Environmental measurements not published
  - Privacy-policy/Terms deletion-retention discrepancy
```

## My interpretation of what the metrics reveal

The interesting thing isn't that Béluga gets a lot of **Strong** ratings. It is **why**.

Béluga has made several architectural decisions that directly address the power relationships in your framework:

**Vendor → user**

> Québec ownership + Québec jurisdiction + open protocols + monthly pricing + no advertising/profiling + explicit data portability.

**Platform → ecosystem**

> Open standards and replaceable open-source infrastructure components rather than making Béluga the only possible interface to the user's data.

**Designer → designed-for**

> Native Québec French, explicit accessibility work, and a stated accessibility feedback mechanism, although the representation evidence is still insufficient.

And there is an especially interesting alignment with your **reversibility principle**:

> Béluga's strongest sovereignty argument isn't that it says "your data belongs to you." It's that **IMAP, CalDAV and CardDAV make leaving technically possible without asking Béluga's permission.**

That's exactly the kind of distinction your v0.5.1 framework is designed to capture.

The main caution is equally clear: **the architecture is considerably more mature than the operational evidence.** The open-beta status, young organization, absence of independent security/accessibility audits, proprietary application layer, and lack of environmental measurements keep this firmly in **Assess** rather than **Trial** at present.
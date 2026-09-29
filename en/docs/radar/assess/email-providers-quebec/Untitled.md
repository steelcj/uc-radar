# Béluga evaluation

**Product:** Béluga collaborative suite
**Version evaluated:** Open beta, September 2026
**Evaluator:** Christopher Steel / Universal Cake
**Date:** 2026-09-18
**Radar ring:** Assess

Béluga currently consists of:

- Mail
- Files
- Docs
- Calendar
- Contacts
- Meetings
- Convo
- Matelot, AI assistant, in development

The underlying stack includes Postfix/Dovecot/Rspamd, MinIO, Hocuspocus/Y.js/BlockNote, Radicale, LiveKit, Matrix/Synapse and Keycloak, with Django/PostgreSQL/Redis and a Next.js/React/TypeScript frontend. Infrastructure is K3s on company-owned bare-metal servers in Québec. ([Béluga](https://beluga.quebec/en/?utm_source=chatgpt.com))

## Notes

**Universal Cake Evaluation Metrics v0.5.1** framework evaluation of  **Béluga as a suite** separating the seven services where their properties materially differ.

Béluga's current documentation states that current services are an **open beta product whose features and policies are changing**. The evaluation date is **2026-09-18**. Béluga says they do not yet have SOC 2 or ISO 27001 certification. ([Béluga](https://beluga.quebec/souverainete/?utm_source=chatgpt.com))

------

## 1. Inclusive

### Accessibility, alternative interaction: **Moderate — Claimed**

Béluga has a dedicated accessibility statement targeting **WCAG 2.1 AA**. It documents semantic HTML, landmarks, keyboard navigation, explicit form labels, programmatically associated errors, alternative text and reduced-motion support. It also says the product exposes an animation-intensity control. ([Béluga](https://beluga.quebec/accessibilite/?utm_source=chatgpt.com))

That's good evidence of an accessibility program, but I would **not yet rate the accessibility floor Verified**.

What is missing from the public evidence:

- independent accessibility audit
- actual screen-reader testing results
- keyboard testing of the complete authenticated application
- accessible collaborative editing
- accessible meeting interface
- accessible file management
- accessibility of mobile Matrix clients
- WCAG 2.1 AA success-criterion-by-success-criterion results

**Gate:** **Not yet Verified.**

This is an important distinction between *having an accessibility commitment* and demonstrating that all of the actual product is accessible.

### Multilingual integration: **Strong — Verified**

Béluga explicitly supports **Québec French and English**, with FR-CA/EN-CA identified in its technical stack. Its stated approach is Québec French rather than treating French as a late translation of French-from-France. ([Béluga](https://beluga.quebec/a-propos/?utm_source=chatgpt.com))

The stronger point is that language appears to be an actual product concern rather than simply translated marketing material.

I would still want to test:

- completeness of translations
- user-generated content
- error messages
- accessibility terminology
- language switching
- email/document/calendar content in multilingual environments

### Affordability: **Strong — Verified**

There is a permanent free tier, and current individual pricing is:

- Free: $0
- Plus: $10/month
- Héritage: $15/month
- Family: $25/month for five accounts

Organizational pricing starts higher, with the site currently showing $15/user/month for PME and $30/user/month for PME+. Prices are in CAD and subscriptions are monthly without commitment. ([Béluga](https://beluga.quebec/en/?utm_source=chatgpt.com))

The free tier is particularly significant from an accessibility perspective because **the privacy/sovreignty architecture does not appear to be reserved for paying customers only**. Béluga says the free plan uses the same Québec infrastructure. ([Béluga](https://beluga.quebec/a-propos/?utm_source=chatgpt.com))

### Hardware/connectivity: **Moderate — Inferred**

There are several positive characteristics:

- IMAP
- CalDAV
- CardDAV
- browser access
- Linux/macOS/Windows Files client
- resumable file uploads
- ordinary email clients can be used

The Files service specifically describes resumable uploads designed to survive dropped connections. ([Béluga](https://beluga.quebec/en/features/?utm_source=chatgpt.com))

However, the web application uses a modern Django + Next.js/React/TypeScript stack, and I found no published performance testing on low-end hardware or poor connectivity.

So this remains **Inferred**, rather than Verified.

### Cognitive accessibility: **Moderate — Inferred**

The suite has a relatively conventional mental model and uses familiar services. Standards-based access also means users aren't forced into one interface.

But I found insufficient evidence for:

- reading-level testing
- cognitive accessibility research
- error-recovery testing
- task-completion testing with cognitively disabled users

### Time-based media: **Moderate — Claimed**

Meetings support recording and automatic transcription, and participants are notified when recording begins. Transcripts are delivered as Docs documents. ([Béluga](https://beluga.quebec/en/features/?utm_source=chatgpt.com))

That's useful for accessibility, but transcription isn't equivalent to:

- human-quality captions
- audio description
- accurate speaker identification
- accessible recorded-video controls

So I wouldn't give this Strong yet.

### Resizing/reflow: **Unknown**

I found the WCAG target and general accessibility claims, but not sufficient evidence to independently verify 200% zoom/reflow across the authenticated product.

### Compatibility: **Strong — Claimed/Verified by documented standards**

This is one of Béluga's strongest areas.

Mail works with ordinary IMAP/SMTP clients, calendars use CalDAV, contacts use CardDAV, and the site documents Linux/macOS/Windows file synchronization. ([Béluga](https://beluga.quebec/en/features/?utm_source=chatgpt.com))

The standards-based approach materially reduces interface lock-in.

------

# 2. Agency

## Sovereignty & privacy: **Strong — Claimed/Verified**

This is where Béluga is particularly interesting.

Béluga says:

- servers are owned by Béluga
- servers are located in Québec
- no AWS/Azure/GCP infrastructure is used
- administrative access is restricted through VPN
- backups are encrypted before leaving production machines
- the backup copy remains in Québec
- there is no advertising
- there is no third-party analytics
- data isn't used to train AI models
- 2FA/passkeys are available
- Keycloak handles authentication
- main passwords aren't stored by Béluga
- third-party integrations are opt-in

These are unusually explicit sovereignty commitments. ([Béluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

There are, however, three important exceptions.

### Mistral

Recorded meeting audio is sent to **Mistral AI in France** for transcription. Béluga says this happens only when the meeting is recorded and that a privacy impact assessment covers the processing. ([Béluga](https://beluga.quebec/confidentialite/?utm_source=chatgpt.com))

### Stripe

Payment processing goes through Stripe. Béluga says only necessary billing metadata leaves its infrastructure and card numbers do not pass through Béluga's servers. ([Béluga](https://beluga.quebec/confidentialite/?utm_source=chatgpt.com))

### Cyberimpact

Transactional emails use Cyberimpact, a Québec provider. ([Béluga](https://beluga.quebec/confidentialite/?utm_source=chatgpt.com))

So I would phrase the finding as:

> **Québec-hosted and Québec-operated by default, with explicitly disclosed external processing boundaries.**

That's considerably more useful than simply saying "Québec-hosted."

------

# 3. Email privacy and E2EE

This deserves its **own explicit entry**.

### Béluga Mail

**End-to-end encryption: Weak / Not provided**

Béluga Mail uses Postfix + Dovecot + Rspamd. It provides TLS in transit and encryption of certain credentials/secrets at rest, but ordinary email is not E2EE. ([Béluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

Therefore:

```text
Sender
  │
 TLS
  ▼
Béluga Postfix
  │
  │ plaintext email is necessarily
  │ available to the mail system
  ▼
Recipient's mail server
```

This isn't necessarily a Béluga-specific weakness. It's an inherent characteristic of conventional email.

**Convo is completely different.**

### Béluga Convo

**End-to-end encryption: Strong — Claimed**

Béluga explicitly states that Convo uses Matrix and is E2EE by default, including attachments, and that Béluga cannot read the conversations. ([Béluga](https://beluga.quebec/en/features/?utm_source=chatgpt.com))

That makes the distinction:

| Service  | Transport encryption | At-rest protection | E2EE    |
| -------- | -------------------- | ------------------ | ------- |
| Mail     | Yes                  | Yes, selected data | **No**  |
| Files    | Yes                  | Yes                | **No**  |
| Docs     | Yes                  | Yes                | **No**  |
| Calendar | Yes                  | Yes                | **No**  |
| Contacts | Yes                  | Yes                | **No**  |
| Meetings | Yes                  | Yes                | **No**  |
| Convo    | Yes                  | Yes                | **Yes** |

This is exactly the sort of distinction your framework should capture rather than allowing "encrypted" to become one undifferentiated claim.

------

# 4. Power-imbalance proxies

### Exit cost: **Strong — Inferred**

The standards-based interfaces are a major advantage:

- IMAP for mail
- CalDAV for calendars
- CardDAV for contacts
- vCard
- iCalendar
- documented data export

Béluga also says formal data export is available for contacts, calendar, documents, files and email. ([Béluga](https://beluga.quebec/confidentialite/?utm_source=chatgpt.com))

The critical remaining question is **how complete and usable those exports actually are**.

I'd want to perform an actual migration test before marking this Verified.

### Data portability: **Strong — Claimed**

This is unusually good on paper.

But your framework correctly distinguishes:

> technically exportable ≠ human-readable and practically migratable.

I would therefore split this:

| Export            | Current assessment                         |
| ----------------- | ------------------------------------------ |
| Email             | **Strong — Claimed** via IMAP              |
| Calendar          | **Strong — Claimed** via CalDAV/iCalendar  |
| Contacts          | **Strong — Claimed** via CardDAV/vCard/CSV |
| Files             | **Moderate — Claimed**                     |
| Docs              | **Unknown/Moderate — Claimed**             |
| Organization data | **Strong — Claimed**                       |

The last two deserve actual migration testing.

### ToS volatility: **Unknown**

Current terms were updated **August 23, 2026**, so this is too young a service to establish a five-year history. The terms do say substantial changes will receive at least 30 days' notice. ([Béluga](https://beluga.quebec/en/terms/?utm_source=chatgpt.com))

I'd record:

> **Unknown: insufficient historical period.**

That's preferable to treating a new company's lack of historical volatility as evidence of stability.

### Pricing asymmetry: **Strong — Verified**

Prices are public, in CAD, monthly and without a mandatory annual commitment. ([Béluga](https://beluga.quebec/tarification/?utm_source=chatgpt.com))

That's precisely the sort of information symmetry your metric is trying to capture.

------

# 5. Interaction patterns

### Overall: **Strong — Claimed/Inferred**

There are several unusually good examples.

**Natural stopping / consent:**

Meeting recording deliberately notifies participants rather than allowing discreet recording. ([Béluga](https://beluga.quebec/en/features/?utm_source=chatgpt.com))

**Plain asking:**

AI connectors require individual consent and can be disabled globally by an administrator. ([Béluga](https://beluga.quebec/en/?utm_source=chatgpt.com))

**Visible costs:**

Pricing and storage limits are publicly documented. ([Béluga](https://beluga.quebec/tarification/?utm_source=chatgpt.com))

**Easy exit:**

Monthly subscriptions can be cancelled, and open standards are used for major data classes. ([Béluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

**Quiet by default:**

There is no advertising or third-party analytics according to Béluga's published commitments. ([Béluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

I would nevertheless keep this **Inferred**, because interaction-pattern evaluation really benefits from actual use rather than documentation.

------

# 6. Environment

### Direct impact: **Moderate — Unknown/Claimed**

There is a genuinely interesting architectural advantage:

Béluga operates its own bare-metal infrastructure rather than consuming a hyperscale cloud abstraction. ([Béluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

That gives it potential advantages in:

- hardware longevity
- resource efficiency
- avoiding duplicated cloud abstractions
- direct infrastructure control

But I found **no published energy measurements**, PUE, server utilization figures, hardware lifecycle information, or SCI-style measurement.

Therefore:

> **Moderate, primarily Inferred.**

I would not infer "green" simply from "bare metal."

### Indirect impact: **Moderate — Inferred**

Open protocols and ordinary clients are good:

```text
IMAP
CalDAV
CardDAV
S3
Matrix
WebRTC
```

They reduce forced hardware/software dependencies.

The web suite is nevertheless a substantial modern application, and I found no performance/bandwidth measurements.

### Authoring-side load: **Unknown**

Insufficient evidence.

------

# 7. Security

### Security: **Strong — Claimed/Inferred**

The architecture has several good security properties:

- TLS 1.2+
- Keycloak
- OIDC/PKCE
- 2FA
- passkeys
- hardware keys
- application passwords
- VPN-only administration
- application firewall
- rate limiting
- brute-force protection
- organization isolation
- encrypted backups
- point-in-time recovery
- daily backups
- documented open-source components

([Béluga](https://beluga.quebec/securite/?utm_source=chatgpt.com))

There is also a useful transparency point:

> Béluga explicitly says it is **not yet SOC 2 or ISO 27001 certified**.

I regard that disclosure positively from a transparency perspective, while it remains a security limitation. ([Béluga](https://beluga.quebec/souverainete/?utm_source=chatgpt.com))

### What I would want to verify before Trial

- vulnerability disclosure process
- CVE response times
- penetration-testing evidence
- external security audit
- container/K3s isolation
- backup restoration testing
- Matrix E2EE verification
- tenant isolation
- access-control testing
- mail relay configuration
- SPF/DKIM/DMARC behaviour
- key management
- admin audit logging
- DLP behaviour
- actual network egress

So **Strong — Claimed** is reasonable at Assess, but it should not yet become **Strong — Verified**.

------

# 8. Longevity

### **Moderate — Inferred**

Béluga has a coherent architecture and uses mature open-source foundations:

Postfix, Dovecot, Rspamd, Matrix/Synapse, MinIO, LiveKit, Keycloak, Radicale, PostgreSQL, etc. ([Béluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

That's good.

But Béluga itself is a **very young company/product**, currently in open beta. ([Béluga](https://beluga.quebec/en/press/?utm_source=chatgpt.com))

There simply isn't enough historical evidence yet for Strong.

I'd explicitly avoid punishing it for being young while also avoiding the opposite mistake of assuming longevity.

------

# 9. Content endurance

### **Strong — Inferred**

This is one of the suite's better characteristics.

Major content categories use established standards:

```text
Mail       → IMAP
Calendar   → CalDAV / iCalendar
Contacts  → CardDAV / vCard
Files      → S3-compatible storage
Messaging → Matrix
```

That gives content a reasonable chance of surviving Béluga itself. ([Béluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))

Docs are the weaker area because the collaborative editing representation is more application-specific.

------

# 10. Exit and portability

### **Strong — Claimed**

**Gate: Pass, provisionally.**

Béluga explicitly provides export mechanisms and uses open standards for the most important data categories. Its privacy documentation says the platform offers export for contacts, calendars, documents, files and email. ([Béluga](https://beluga.quebec/confidentialite/?utm_source=chatgpt.com))

But because your gate says:

> "data or content cannot be extracted in usable form"

I'd want to **actually execute an export/import test** before moving this to Trial.

So:

**Assess:** Pass, Claimed
**Trial:** Requires Verified migration test

------

# 11. Adjustability and support

### **Strong — Inferred**

This is supported by:

- open-source foundations
- replaceable components
- documented architecture
- open standards
- Québec-based operator
- French-language support
- direct privacy contact

Béluga explicitly describes its components as auditable and replaceable. ([Béluga](https://beluga.quebec/en/about/?utm_source=chatgpt.com))

The limitation is that **the Béluga integration layer itself is proprietary**, as far as the public documentation reviewed here establishes.

So you can replace the underlying Postfix or Dovecot installation, but that doesn't mean you can necessarily fork Béluga itself.

------

# 12. Market position

This one is particularly interesting because **Béluga currently has almost no market concentration**.

### Market concentration: **Strong — Inferred**

It is a very small entrant rather than a dominant platform.

### Take rate: **Strong — Inferred**

No marketplace/payment-platform take rate is involved in the core product.

### API stability: **Unknown**

Too young to establish a meaningful history.

### Forkability: **Moderate — Inferred**

The underlying components are open source, but the complete Béluga service isn't presented as an open-source suite that anyone can simply fork and operate.

This distinction matters:

> **Open-source foundations ≠ open-source product.**

### Contributor concentration: **Unknown**

Béluga's own integration code isn't sufficiently documented publicly for us to make a meaningful contributor-concentration assessment.

### Foundation-model dependency: **Strong — currently Inferred**

The core suite does **not** appear dependent on an AI foundation model.

The AI components are optional and separable.

The current transcription dependency on Mistral is an exception, and Béluga says it is actively looking to bring that processing back to Québec. ([Béluga](https://beluga.quebec/souverainete/?utm_source=chatgpt.com))

------

# Scorecard

| Metric area                            | Rating                      | Evidence              |
| -------------------------------------- | --------------------------- | --------------------- |
| Accessibility, alternative interaction | **Moderate**                | Claimed               |
| Multilingual integration               | **Strong**                  | Verified              |
| Affordability                          | **Strong**                  | Verified              |
| Hardware/connectivity                  | **Moderate**                | Inferred              |
| Cognitive accessibility                | **Moderate**                | Inferred              |
| Time-based media                       | **Moderate**                | Claimed               |
| Resizing/reflow                        | **Unknown**                 | Insufficient evidence |
| Representation                         | **Unknown**                 | Insufficient evidence |
| Resilience                             | **Strong**                  | Inferred              |
| Agency, sovereignty & privacy          | **Strong**                  | Claimed/Verified      |
| Power imbalance                        | **Strong**                  | Claimed/Inferred      |
| Interaction patterns                   | **Strong**                  | Inferred              |
| Environment                            | **Moderate**                | Inferred              |
| Security                               | **Strong**                  | Claimed/Inferred      |
| Longevity                              | **Moderate**                | Inferred              |
| Content endurance                      | **Strong**                  | Inferred              |
| Exit & portability                     | **Strong**                  | Claimed               |
| Adjustability & support                | **Strong**                  | Inferred              |
| Market position                        | **Strong/Unknown by proxy** | Mixed                 |
| **Mail E2EE**                          | **Weak**                    | Architectural fact    |
| **Convo E2EE**                         | **Strong**                  | Claimed               |

## Gates

| Gate                                   | Current result                                            |
| -------------------------------------- | --------------------------------------------------------- |
| Unavoidable telemetry/content exposure | **Pass, based on documentation**                          |
| Licence compatibility                  | **Unknown until adopting-project licence is specified**   |
| Accessibility floor                    | **Not yet verified**                                      |
| Exit/usable extraction                 | **Pass provisionally, requires migration test for Trial** |

### Radar ring: **Assess**

I would **not move Béluga to Trial yet**.

The two things I'd specifically want to test before doing that are:

1. **Accessibility floor:** actual keyboard + screen-reader testing of the authenticated suite, not just the marketing site.
2. **Exit:** create representative Mail, Files, Docs, Calendar and Contacts content and perform a complete export/import exercise.

And I'd add a third test because of the importance of the security claims:

1. **Security:** verify the documented network boundaries, particularly that ordinary suite operation has no unexpected third-party telemetry or content egress.

The interesting result is that **Béluga's strongest Universal Cake characteristics aren't actually "privacy" in the narrow sense**. They are the combination of **sovereignty + open standards + explicit boundaries + relatively low exit cost + transparent disclosure of exceptions**.

The biggest weaknesses at present are mostly **evidence maturity rather than obviously bad design**: it is young, in beta, lacks independent accessibility/security verification, and some of the strongest claims remain vendor-claimed. That distinction is exactly what your **Verified / Inferred / Claimed** evidence model is useful for. ([Béluga](https://beluga.quebec/en/sovereignty/?utm_source=chatgpt.com))
---
dc:title: "opensource-identity-systems--iam"
dcterms:version: "0.1.1"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "iam"
  - "identity"
  - "authentication"
  - "authorisation"
  - "sso"
  - "oidc"
dc:description: "Radar entry surveying open source identity and access management platforms and directory services, with adoption order established for sovereign self-hosted deployments."
dc:publisher: ""
dc:date: "2026-06-16"
dc:modified: "2026-06-16"
dc:type: "radar-entry--survey"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-CA"
dc:source: "https://goauthentik.io https://www.keycloak.org https://zitadel.com https://kanidm.com https://www.freeipa.org https://www.authelia.com https://openldap.org"
dc:relation: ""
dc:identifier: "opensource-identity-systems--iam"
dc:rights: "TODO"
---

# opensource-identity-systems--iam

## What it is

This entry surveys the open source landscape for identity and access management (IAM), covering directory services, authentication platforms, and authorisation infrastructure. It establishes a preferred adoption order for sovereign, self-hosted deployments across a range of organisational scales and use cases. It covers the LDAP protocol and its implementations, and the principal modern IAM platforms: Authentik, Keycloak, Zitadel, Kanidm, FreeIPA, and Authelia.

## Adoption order

The following order reflects preference for sovereign, self-hosted deployments. It is not a strict ranking — the correct choice depends on deployment context, scale, and specific requirements. Each tier is described in detail in the platform assessments section below.

**A — Authentik** — preferred for small-to-medium deployments. MIT-licensed, Python-based, modern admin UI, minimal infrastructure dependencies. The recommended starting point for most new self-hosted IAM deployments.

**B — Keycloak** — preferred for large or enterprise deployments. Apache 2.0-licensed, CNCF Incubating, largest ecosystem and community, paid vendor support available via Red Hat. The recommended choice where scale, enterprise integrations, or compliance requirements demand the broadest possible support.

**C — Zitadel** — preferred for B2B multi-tenant deployments and organisations requiring an immutable audit trail or European data sovereignty. AGPL-3.0-licensed (v3+); the licence change from Apache 2.0 warrants review before adoption. Swiss governance. Strong fit for organisations subject to GDPR or with concerns about US CLOUD Act exposure.

**D — Kanidm** — preferred for greenfield deployments prioritising modern security architecture and Rust-based infrastructure. MPL 2.0-licensed. Technically strong and sovereignty-aligned. Currently in assess — ecosystem maturity and community size are limiting factors for production adoption.

**E — FreeIPA** — preferred for Linux infrastructure identity: server logins, SSH access management, centralised Unix identities, and Kerberos-based infrastructure. GPL v3. Not recommended as a primary web application IAM platform. Many organisations run FreeIPA for infrastructure identities alongside Authentik or Keycloak for application identities.

**F — Authelia** — preferred for lightweight reverse-proxy forward-auth protection of internal services, particularly in combination with Caddy or Nginx. Apache 2.0-licensed. Not a full IAM platform or directory service — it is a forward authentication proxy. Not a replacement for A, B, or C.

**G — OpenLDAP / LDAP** — legacy or specific integration use only. LDAP is a protocol and directory, not a modern IAM platform. It does not natively provide SSO, OAuth2, OIDC, MFA, or user self-service. Not recommended as a starting point for new deployments. Use only when a legacy application specifically requires direct LDAP access, with a modern IAM platform providing the LDAP outpost or federation layer.

---

## Background — LDAP and why it is not enough

LDAP (Lightweight Directory Access Protocol) is a protocol for storing and querying directory information — users, groups, and organisational data. It is not an IAM platform. Common open source LDAP server implementations include OpenLDAP (the most widely deployed), 389 Directory Server (Red Hat-backed), and Apache Directory Server. LDAP provides a centralised user directory, group management, authentication support for applications that speak the protocol, and hierarchical data storage. It does not natively provide Single Sign-On, OAuth2, OpenID Connect, social login, multi-factor authentication, or user self-service portals. For modern application stacks, LDAP alone requires an IAM layer on top — which most organisations end up adding anyway. Starting with a modern IAM platform that can federate from or replace LDAP is the preferred approach for new deployments.

---

## Platform assessments

### A — Authentik

Authentik launched in 2018 as a Python-based alternative to Keycloak, with a significantly more modern admin UI. Current version: 2025.x series (actively developed). It supports OAuth2, OIDC, SAML, LDAP outpost, SCIM, MFA, and a visual flow editor for custom authentication journeys. Redis dependency was removed in release 2025.10 — the current stack is Authentik plus PostgreSQL only, with a total RAM footprint of approximately 250–350MB. The admin UI is materially ahead of Keycloak's in usability. Strictly MIT-licensed on the Community edition with no commercial-use clauses. An Enterprise edition adds priority support and enterprise features but is still self-hosted. No managed cloud offering as of 2026. Twenty-six GHSA security advisories have been reported (five critical, thirteen high) — more than Authelia, reflecting the larger codebase and broader attack surface of a full IAM platform. The project has responded to these promptly. For small-to-medium organisations, self-hosters, and teams with Python operational competence, Authentik is the recommended starting point.

**Licence:** MIT (Community edition).
**Governance:** Authentik Security Inc., open source community, MIT-licensed core.
**Ring: Adopt** — recommended starting point for small-to-medium deployments.

### B — Keycloak

Keycloak is the most widely deployed open source IAM platform. Created by Red Hat in 2014, it is now in CNCF Incubating status (since April 2023). The current stable release is 26.6.1 (April 2026). It supports OAuth2, OIDC, SAML, SSO, MFA, identity federation, and user self-service. Since moving from the WildFly/JBoss base to Quarkus (version 17+), the memory footprint has reduced significantly — the Quarkus-based build starts in seconds and runs comfortably in 512MB RAM for small deployments, though larger production deployments with multiple realms and high concurrency require more. Red Hat offers a commercially supported build. Keycloak is the safest choice for large deployments, enterprise integrations, and environments where paid vendor support or RHEL ecosystem compatibility is required. The learning curve is steeper than Authentik — concepts like realms, clients, scopes, and mappers require investment to understand correctly.

**Licence:** Apache 2.0.
**Governance:** CNCF Incubating, Red Hat-backed, open source community.
**Ring: Adopt** — recommended for large or enterprise deployments.

### C — Zitadel

Zitadel is a Go-based IAM platform developed by CAOS Ltd. in Switzerland, with a strong focus on B2B multi-tenancy and audit trail completeness. It uses an event-sourcing architecture — every authentication and authorisation state change is written to an immutable log — providing a built-in audit trail without additional tooling. Supports OIDC, SAML, SCIM, MFA, FIDO2/Passkeys, and delegated role management out of the box. The self-hosted Community Edition has no monthly active user limits. Zitadel Cloud (SaaS) carries SOC 2 Type II (January 2026) and ISO 27001:2022 certifications. The v3 release changed the licence from Apache 2.0 to AGPL-3.0, which is operationally neutral for organisations running Zitadel as their internal identity provider but requires a commercial licence for managed service providers or SaaS platforms offering Zitadel-based authentication as a product. This licence change was controversial in the community and must be reviewed before adoption. Swiss jurisdiction is a meaningful sovereignty advantage for organisations subject to GDPR or with concerns about US CLOUD Act exposure. PostgreSQL is the only supported database as of v3.

**Licence:** AGPL-3.0 (v3+); Apache 2.0 for earlier versions.
**Governance:** CAOS Ltd. (Switzerland), open source community.
**Ring: Assess** — strong fit for B2B multi-tenant and European sovereignty use cases; AGPL-3.0 licence requires careful review before adoption.

### D — Kanidm

Kanidm is a Rust-based identity management platform designed from the ground up around modern security principles, avoiding the historical baggage of LDAP and Kerberos architectures. It provides LDAP compatibility, OIDC, MFA, and RADIUS support. The architecture is deliberately minimal and security-first. Community is smaller than Keycloak or Authentik, enterprise adoption is limited, and the integration ecosystem is less mature. Technically interesting for greenfield deployments where the operator controls most or all of the applications being authenticated. MPL 2.0 licence is sovereignty-friendly.

**Licence:** Mozilla Public License 2.0.
**Governance:** Kanidm contributors, open source community.
**Ring: Assess** — technically strong and sovereignty-aligned; ecosystem maturity is the current limiting factor.

### E — FreeIPA

FreeIPA combines LDAP, Kerberos, DNS, and certificate management into an integrated Linux infrastructure identity platform. It is the correct choice for managing Linux server logins, SSH access, and centralised Unix identities at scale. It is not well-suited for web application authentication — it predates modern OIDC/OAuth2 patterns and its OIDC support is limited. Many organisations run FreeIPA for infrastructure identities alongside Keycloak or Authentik for application identities.

**Licence:** GPL v3.
**Governance:** Red Hat-backed, open source community.
**Ring: Assess** — correct for Linux infrastructure identity; not recommended as a primary web application IAM platform.

### F — Authelia

Authelia is a lightweight forward authentication proxy, not a full IAM platform or directory service. It integrates with reverse proxies (Caddy, Nginx, Traefik) to add authentication and MFA to services that do not natively support them. It supports OIDC, MFA, and access control rules. It does not manage a user directory — it relies on an LDAP backend or a file-based user store. Resource footprint is very low. Well-suited for protecting internal dashboards and services behind a reverse proxy. Not appropriate as a primary IAM platform for application authentication at scale. Particularly relevant in combination with Caddy for lightweight protection of internal services.

**Licence:** Apache 2.0.
**Governance:** Authelia contributors, open source community.
**Ring: Assess** — suitable for specific reverse-proxy forward-auth use cases; not a replacement for a full IAM platform.

### G — OpenLDAP / LDAP

OpenLDAP is the reference implementation of the LDAP protocol. It provides a centralised user directory and group management and is compatible with a wide range of legacy applications. It does not provide SSO, OIDC, OAuth2, MFA, or user self-service. For new deployments, LDAP should not be the starting point — a modern IAM platform that can provide an LDAP outpost or federation layer covers the LDAP use case without the limitations. Use OpenLDAP only when a legacy application requires direct LDAP access and a modern IAM platform cannot provide a compatible outpost.

**Licence:** OpenLDAP Public License (BSD-style).
**Governance:** OpenLDAP Project, open source community.
**Ring: Hold** — legacy and specific integration use only; not recommended for new deployments.

---

## Platform comparison

| Feature | OpenLDAP | Keycloak | Authentik | Zitadel | Authelia | Kanidm | FreeIPA |
|---|---|---|---|---|---|---|---|
| Adoption tier | G | B | A | C | F | D | E |
| User directory | Yes | Yes | Yes | Yes | File/LDAP | Yes | Yes |
| LDAP protocol | Yes | Federation | Outpost | No | Backend | Compatible | Yes |
| SSO | No | Yes | Yes | Yes | Yes | Yes | Yes |
| OAuth2/OIDC | No | Yes | Yes | Yes | Yes | Yes | Limited |
| SAML | No | Yes | Yes | Yes | No | No | No |
| MFA | No | Yes | Yes | Yes | Yes | Yes | Yes |
| Passkeys/FIDO2 | No | Yes | Yes | Yes | No | Yes | No |
| SCIM | No | Yes | Yes | Yes | No | No | No |
| Multi-tenancy | No | Realms | Tenants | Native | No | No | No |
| Audit trail | No | Partial | Partial | Immutable | Partial | Partial | Yes |
| Web UI | Minimal | Good | Excellent | Good | Limited | Good | Good |
| Linux host mgmt | No | No | No | No | No | No | Yes |
| Licence | BSD-style | Apache 2.0 | MIT | AGPL-3.0 | Apache 2.0 | MPL 2.0 | GPL v3 |
| Language | C | Java/Quarkus | Python/Go | Go | Go | Rust | Python |
| Ring | Hold | Adopt | Adopt | Assess | Assess | Assess | Assess |

---

## Why not LDAP first

LDAP solves the directory problem but most modern applications want OpenID Connect, OAuth2, SSO, MFA, self-service password resets, and fine-grained application access control. With pure LDAP, an IAM layer is typically added later anyway — often under pressure and without proper planning. Starting with a modern IAM platform avoids this accumulation of technical debt and produces a simpler, more maintainable architecture from day one.

---

## Concerns

**AGPL-3.0 on Zitadel v3** — the licence change from Apache 2.0 is a meaningful concern for any organisation that might offer Zitadel-based authentication as a managed service or embed it in a product. For internal self-hosted deployments the concern is minimal. Review carefully before adoption.

**Keycloak complexity** — the learning curve for Keycloak is steep. Concepts like realms, clients, scopes, and mappers require investment to understand correctly. Misconfiguration is a real risk. Budget time for learning and testing before production deployment.

**Authentik security advisories** — 26 GHSA advisories (5 critical, 13 high) is higher than simpler tools, reflecting the larger attack surface. All have been addressed promptly. Pin versions and maintain a clear upgrade path.

**Single-vendor governance risk** — Authentik is governed by Authentik Security Inc. and Zitadel by CAOS Ltd. Neither has the foundation-level governance of Keycloak (CNCF) or Kanidm (community). This is worth monitoring as these projects mature.

**No managed cloud for Authentik** — organisations that want a managed identity service rather than a self-hosted one should consider Zitadel Cloud or commercial alternatives outside the scope of this entry.

---

## Security assessment

**Applies to:** all platforms listed. Each platform handles authentication credentials and session tokens — the highest-sensitivity data in most application stacks.

**Network behaviour:**
- [x] Outbound connections during normal use — ACME or certificate authority connections where certificate management is integrated (FreeIPA). OIDC and SAML flows involve redirects to external identity providers when federation is configured. All platforms make outbound connections only when explicitly configured to do so.
- [x] Update checks — Keycloak, Authentik, and Zitadel do not perform silent update checks. Version updates are operator-initiated.
- [x] Telemetry or usage data — none of the platforms listed transmit usage telemetry by default in self-hosted deployments. Verify on each upgrade.
- [ ] Licence server contact — not present in any platform listed. All are self-contained.
- [x] Affects networking of items created — authentication tokens, session cookies, and redirect URIs produced by these platforms traverse the network. All platforms support HTTPS-only operation and should be deployed behind TLS termination.

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content — all platforms write to configured data directories (PostgreSQL database, certificate stores). No unexpected writes alongside content.
- [x] Caches content outside the working directory — session and token caches are held in the configured database or in-memory. Authentik previously used Redis for caching; this dependency was removed in 2025.10.
- [ ] Stores recent file history outside the archive — not applicable.
- [ ] Writes to unexpected locations — not observed in documented deployments.

**Content exposure:**
- [ ] Sends any content to a remote service — self-hosted deployments do not transmit user data to remote services. Zitadel Cloud and any managed offering are out of scope for this assessment.
- [ ] Stores content in a cloud service by default — not applicable to self-hosted deployments.
- [ ] Auto-save or backup features that copy content externally — not applicable.

**Assessment method:** review of official documentation, release notes, GHSA security advisories, and community sources for each platform. Versions assessed: Keycloak 26.6.1, Authentik 2025.x series, Zitadel v3.x, Authelia current stable, Kanidm current stable, FreeIPA current stable, OpenLDAP current stable. No independent network capture performed for any platform; flagged as a gap for any platform moving to adopt.

**Assessment status:** Authentik and Keycloak adopted. Zitadel, Kanidm, FreeIPA, and Authelia in assess. OpenLDAP on hold.

---

## Relationship to project

IAM infrastructure is a foundational concern for any self-hosted stack that serves authenticated users or manages operator access to infrastructure. This entry applies across projects rather than being scoped to a single stack. The specific use cases where these platforms become relevant are: applications requiring user authentication, member portals, or checkout flows with user accounts; operator infrastructure requiring centralised SSH access management or identity federation across multiple servers; and any deployment requiring SSO, MFA, or fine-grained access control.

---

## Status notes

**Ring: Adopt — Authentik (tier A), Keycloak (tier B)**
**Ring: Assess — Zitadel (tier C), Kanidm (tier D), FreeIPA (tier E), Authelia (tier F)**
**Ring: Hold — OpenLDAP / LDAP (tier G)**

What would move Zitadel to adopt: a specific use case requiring B2B multi-tenancy or immutable audit trail; legal review of AGPL-3.0 implications for the specific deployment context; a security assessment including network capture.

What would move Kanidm to adopt: growth in ecosystem maturity and integration coverage; a specific greenfield deployment where Rust-based infrastructure and modern security architecture are priorities.

What would move Authelia to adopt: a specific reverse-proxy forward-auth use case identified; testing against Caddy in a staging environment confirmed.

What would move any platform to hold: discovery of undisclosed telemetry or data transmission; a licence change away from OSI-approved open source; a pattern of unaddressed security advisories; or acquisition by a commercial entity with a history of open-source bait-and-switch.

- Last reviewed: 2026-06-16
- Platforms reviewed: Keycloak 26.6.1, Authentik 2025.x, Zitadel v3.x, Authelia current, Kanidm current, FreeIPA current, OpenLDAP current

---

## Links

- https://goauthentik.io
- https://github.com/goauthentik/authentik
- https://www.keycloak.org
- https://github.com/keycloak/keycloak
- https://zitadel.com
- https://github.com/zitadel/zitadel
- https://kanidm.com
- https://github.com/kanidm/kanidm
- https://www.freeipa.org
- https://www.authelia.com
- https://github.com/authelia/authelia
- https://openldap.org

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.1 | Draft | Added adoption order tiers A–G; added Zitadel; updated Keycloak resource requirements for Quarkus-based build; noted Authentik Redis removal in 2025.10; added GHSA advisory count for Authentik; added platform comparison ring column; moved OpenLDAP to hold |
| 0.1.0 | Draft | Initial draft |

## License (for this document)

TODO

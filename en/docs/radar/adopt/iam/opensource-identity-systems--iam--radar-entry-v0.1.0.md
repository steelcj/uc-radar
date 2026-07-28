---
dc:title: "opensource-identity-systems--iam"
dcterms:version: "0.1.0"
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
dc:description: "Radar entry surveying open source identity and access management platforms and directory services, with assessments and recommendations for sovereign self-hosted deployments."
dc:publisher: ""
dc:date: "2026-06-16"
dc:modified: "2026-06-16"
dc:type: "radar-entry--survey"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-CA"
dc:source: "https://openldap.org https://www.keycloak.org https://goauthentik.io https://zitadel.com https://www.authelia.com https://kanidm.com https://www.freeipa.org"
dc:relation: "hugo-hosting-stack"
dc:identifier: "opensource-identity-systems--iam"
dc:rights: "TODO"
---

# opensource-identity-systems--iam

## What it is

This entry surveys the open source landscape for identity and access management (IAM), covering directory services, authentication platforms, and authorisation infrastructure. It is intended to inform a decision about which IAM platform or platforms to adopt for sovereign, self-hosted deployments. It covers the LDAP protocol and its implementations, and the principal modern IAM platforms: Keycloak, Authentik, Zitadel, Authelia, Kanidm, and FreeIPA.

## Background — LDAP and why it is not enough

LDAP (Lightweight Directory Access Protocol) is a protocol for storing and querying directory information — users, groups, and organisational data. It is not an IAM platform. Common open source LDAP server implementations include OpenLDAP (the most widely deployed), 389 Directory Server (Red Hat-backed), and Apache Directory Server. LDAP provides a centralised user directory, group management, authentication support for applications that speak the protocol, and hierarchical data storage. It does not natively provide Single Sign-On, OAuth2, OpenID Connect, social login, multi-factor authentication, or user self-service portals. For modern application stacks, LDAP alone requires an IAM layer on top — which most organisations end up adding anyway. Starting with a modern IAM platform that can federate from or replace LDAP is the preferred approach for new deployments.

## Platform assessments

### Keycloak

Keycloak is the most widely deployed open source IAM platform. Created by Red Hat in 2014, it is now in **[CNCF](./../adopt/cloud-native-computing-foundation/cloud-native-computing-foundation-about.md)** Incubating status (since April 2023). The current stable release is 26.6.1 (April 2026). It supports OAuth2, OIDC, SAML, SSO, MFA, identity federation, and user self-service. Since moving from the WildFly/JBoss base to Quarkus (version 17+), the memory footprint has reduced significantly from earlier versions — the Quarkus-based build starts in seconds and runs comfortably in 512MB RAM for small deployments, though larger production deployments with multiple realms and high concurrency require more. Red Hat offers a commercially supported build. Keycloak is the safest choice for large deployments, enterprise integrations, and environments where paid vendor support or RHEL ecosystem compatibility is required.

**Licence:** Apache 2.0.
**Governance:** CNCF Incubating, Red Hat-backed, open source community.
**Assessment:** strong adopt candidate for large or enterprise deployments.

### Authentik

Authentik launched in 2018 as a Python-based alternative to Keycloak, with a significantly more modern admin UI. Current version: 2025.x series (actively developed). It supports OAuth2, OIDC, SAML, LDAP outpost, SCIM, MFA, and a visual flow editor for custom authentication journeys. Redis dependency was removed in release 2025.10 — the current stack is Authentik plus PostgreSQL only, with a total RAM footprint of approximately 250–350MB. The admin UI is materially ahead of Keycloak's in usability. Strictly MIT-licensed on the Community edition with no commercial-use clauses. An Enterprise edition adds priority support and enterprise features but is still self-hosted. No managed cloud offering as of 2026. Twenty-six GHSA security advisories have been reported (five critical, thirteen high) — more than Authelia, reflecting the larger codebase and broader attack surface of a full IAM platform. The project has responded to these promptly. For small-to-medium organisations, self-hosters, and teams with Python operational competence, Authentik is the recommended starting point.

**Licence:** MIT (Community edition).
**Governance:** Authentik Security Inc., open source community, MIT-licensed core.
**Assessment:** strong adopt candidate for small-to-medium deployments.

### Zitadel

Zitadel is a Go-based IAM platform developed by CAOS Ltd. in Switzerland, with a strong focus on B2B multi-tenancy and audit trail completeness. It uses an event-sourcing architecture — every authentication and authorisation state change is written to an immutable log — providing a built-in audit trail without additional tooling. Supports OIDC, SAML, SCIM, MFA, FIDO2/Passkeys, and delegated role management out of the box. The self-hosted Community Edition has no monthly active user limits. Zitadel Cloud (SaaS) carries SOC 2 Type II (January 2026) and ISO 27001:2022 certifications. The v3 release changed the licence from Apache 2.0 to AGPL-3.0, which is operationally neutral for organisations running Zitadel as their internal identity provider but requires a commercial licence for managed service providers or SaaS platforms offering Zitadel-based authentication as a product. This licence change was controversial in the community and is a concern to note. Swiss jurisdiction is a meaningful sovereignty advantage for organisations subject to GDPR or with concerns about US CLOUD Act exposure. PostgreSQL is the only supported database as of v3.

**Licence:** AGPL-3.0 (v3+); Apache 2.0 for earlier versions.
**Governance:** CAOS Ltd. (Switzerland), open source community.
**Assessment:** assess. Strong fit for B2B multi-tenant deployments and organisations prioritising audit trail completeness and European data sovereignty. AGPL-3.0 licence change warrants careful review before adoption.

### Authelia

Authelia is a lightweight forward authentication proxy, not a full IAM platform or directory service. It integrates with reverse proxies (Caddy, Nginx, Traefik) to add authentication and MFA to services that do not natively support them. It supports OIDC, MFA, and access control rules. It does not manage a user directory — it relies on an LDAP backend or a file-based user store. Resource footprint is very low. Well-suited for protecting internal dashboards and services behind a reverse proxy. Not appropriate as a primary IAM platform for application authentication at scale.

**Licence:** Apache 2.0.
**Governance:** Authelia contributors, open source community.
**Assessment:** assess for specific reverse-proxy forward-auth use cases, particularly in combination with Caddy. Not a replacement for a full IAM platform.

### Kanidm

Kanidm is a Rust-based identity management platform designed from the ground up around modern security principles, avoiding the historical baggage of LDAP and Kerberos architectures. It provides LDAP compatibility, OIDC, MFA, and RADIUS support. The architecture is deliberately minimal and security-first. Community is smaller than Keycloak or Authentik, enterprise adoption is limited, and the integration ecosystem is less mature. Technically interesting for greenfield deployments where the operator controls most or all of the applications being authenticated.

**Licence:** Mozilla Public License 2.0.
**Governance:** Kanidm contributors, open source community.
**Assessment:** assess. Technically strong and sovereignty-aligned, but ecosystem maturity and community size are limiting factors for production adoption at this stage.

### FreeIPA

FreeIPA combines LDAP, Kerberos, DNS, and certificate management into an integrated Linux infrastructure identity platform. It is the correct choice for managing Linux server logins, SSH access, and centralised Unix identities at scale. It is not well-suited for web application authentication — it predates modern OIDC/OAuth2 patterns and its OIDC support is limited. Many organisations run FreeIPA for infrastructure identities alongside Keycloak or Authentik for application identities.

**Licence:** GPL v3.
**Governance:** Red Hat-backed, open source community.
**Assessment:** assess for Linux infrastructure identity management. Not recommended as a primary web application IAM platform.

## Recommended path for new deployments

For a greenfield sovereign self-hosted stack the recommended path is to begin with Authentik for small-to-medium deployments or Keycloak for larger or enterprise deployments. Use OIDC as the authentication protocol for all applications that support it. Enable MFA from day one. Do not start with LDAP unless a legacy application specifically requires it — LDAP can be provided as an outpost by Authentik or as a federation source by Keycloak if needed. Consider Zitadel if the deployment requires strong B2B multi-tenancy, an immutable audit trail, or European data sovereignty as a primary constraint. Consider Authelia for lightweight reverse-proxy forward-auth protection of internal services, particularly in combination with Caddy. Consider Kanidm for fully greenfield deployments where modern security architecture and Rust-based tooling are preferred and ecosystem maturity is acceptable.

## Platform comparison

| Feature | LDAP | Keycloak | Authentik | Zitadel | Authelia | Kanidm | FreeIPA |
|---|---|---|---|---|---|---|---|
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
| Licence | Varies | Apache 2.0 | MIT | AGPL-3.0 | Apache 2.0 | MPL 2.0 | GPL v3 |
| Language | C | Java/Quarkus | Python/Go | Go | Go | Rust | Python |

## Concerns

**AGPL-3.0 on Zitadel v3** — the licence change from Apache 2.0 is a meaningful concern for any organisation that might offer Zitadel-based authentication as a managed service or embed it in a product. For internal self-hosted deployments the concern is minimal. Review carefully before adoption.

**Keycloak complexity** — the learning curve for Keycloak is steep. Concepts like realms, clients, scopes, and mappers require investment to understand correctly. Misconfiguration is a real risk. Budget time for learning and testing before production deployment.

**Authentik security advisories** — 26 GHSA advisories (5 critical, 13 high) is higher than simpler tools, reflecting the larger attack surface. All have been addressed promptly. Pin versions and maintain a clear upgrade path.

**Single-vendor governance risk** — Authentik is governed by Authentik Security Inc. and Zitadel by CAOS Ltd. Neither has the foundation-level governance of Keycloak (CNCF) or Kanidm (community). This is worth monitoring as these projects mature.

**No managed cloud for Authentik** — organisations that want a managed identity service rather than a self-hosted one should look at Zitadel Cloud, or commercial alternatives outside the scope of this entry.

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

**Assessment method:** review of official documentation, release notes, GHSA security advisories, and community sources for each platform. Versions assessed: Keycloak 26.6.1, Authentik 2025.x series, Zitadel v3.x, Authelia current stable, Kanidm current stable, FreeIPA current stable. No independent network capture performed; flagged as a gap for any platform moving to adopt.

**Assessment status:** Assess for all platforms. No platform in this entry has been adopted.

## Relationship to project — Hugo hosting stack

IAM infrastructure is not currently part of the Hugo hosting stack, which serves public static sites and webstores requiring no user authentication. This entry is relevant to two future scenarios: first, if any site or store in the stack requires authenticated user areas, member portals, or checkout flows with user accounts; second, if the operator infrastructure itself moves toward centralised SSH access management or operator identity federation across multiple servers. In the latter case, FreeIPA or Kanidm would be the candidates for infrastructure identity, with Authentik or Keycloak for application identity if needed.

## Status notes

**Ring: Assess**

No IAM platform has been adopted. This entry documents the landscape for future reference. The platforms are assessed against the stack's sovereignty, portability, and operational simplicity goals.

What would move Authentik to **adopt:** a specific use case requiring user authentication in the Hugo hosting stack; a security assessment including network capture; an Ansible role or manual installation guide produced and tested.

What would move Keycloak to **adopt:** same as Authentik, plus a specific requirement for enterprise-scale authentication, RHEL ecosystem compatibility, or paid vendor support.

What would move any platform to **hold:** discovery of undisclosed telemetry or data transmission; a licence change away from OSI-approved open source; a pattern of unaddressed security advisories; or acquisition by a commercial entity with a history of open-source bait-and-switch.

- Last reviewed: 2026-06-16
- Platforms reviewed: Keycloak 26.6.1, Authentik 2025.x, Zitadel v3.x, Authelia current, Kanidm current, FreeIPA current, OpenLDAP current

## Links

- https://www.keycloak.org
- https://github.com/keycloak/keycloak
- https://goauthentik.io
- https://github.com/goauthentik/authentik
- https://zitadel.com
- https://github.com/zitadel/zitadel
- https://www.authelia.com
- https://github.com/authelia/authelia
- https://kanidm.com
- https://github.com/kanidm/kanidm
- https://www.freeipa.org
- https://openldap.org

## License (for this document)

TODO

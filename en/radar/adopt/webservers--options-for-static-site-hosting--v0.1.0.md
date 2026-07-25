---
dc:title: "webservers--options-for-static-site-hosting"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "web-server"
  - "caddy"
  - "nginx"
  - "apache"
  - "infrastructure"
  - "tls"
dc:description: "Radar entry surveying and rating open source web server options for static site hosting, with Caddy adopted as the preferred web server and adoption order established."
dc:publisher: ""
dc:date: "2026-06-16"
dc:modified: "2026-06-16"
dc:type: "radar-entry--survey"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-CA"
dc:source: "https://caddyserver.com https://nginx.org https://httpd.apache.org"
dc:relation: ""
dc:identifier: "webservers--options-for-static-site-hosting"
dc:rights: "TODO"
---

# webservers--options-for-static-site-hosting

## What it is

This entry surveys open source web server options for serving static sites and statically generated webstores over HTTPS on a single Linux server. It evaluates Caddy, Nginx, and Apache2 (httpd) against the stack's goals of sovereignty, provider portability, operational simplicity, automatic TLS, and long-term stability. An adoption order is established. For reverse proxy use cases, see the companion entry [reverse-proxies--options-for-http-routing-and-tls-termination](./reverse-proxies--options-for-http-routing-and-tls-termination.md).

## Adoption order

**A — Caddy** — adopted as the preferred web server for new deployments. Automatic TLS, single static binary, single Caddyfile configuration, provider-agnostic deployment. A full radar entry (`caddy--web-server-radar-entry-v0.1.1.md`) and Universal Cake praxis evaluation (`caddy--web-server--universal-cake-praxis-evaluation-v0.1.0.md`) have been completed and support this adoption decision.

**B — Nginx** — assess. Strong candidate for use cases that Caddy cannot satisfy — high-traffic reverse proxy scenarios, advanced load balancing, or environments requiring fine-grained HTTP configuration. Not adopted for static site hosting in this stack; Caddy covers the current requirements with less operational overhead. See the companion reverse proxy entry for Nginx's role as a reverse proxy.

**C — Apache2 (httpd)** — hold. The existing legacy web server implementation. Distributed configuration (`sites-available`, `sites-enabled`, module management), separate TLS tooling (Certbot), and distro-packaged installation that varies across providers. Superseded by Caddy for all new deployments. Documented here for reference as the existing implementation.

---

## Platform assessments

### A — Caddy

Caddy (version 2.11.2, current stable as of March 2026) is an open source web server and reverse proxy written in Go, released under the Apache License 2.0. Its defining characteristic is automatic TLS certificate provisioning and renewal via ACME (Let's Encrypt and ZeroSSL by default), requiring no manual certificate management or cron jobs. Configuration is expressed in a single Caddyfile. Caddy ships as a single static binary with no external runtime dependencies, making it configuration-identical across providers — a meaningful advantage for a stack that needs to move between Digital Ocean, Canadian, and Icelandic providers.

For static Hugo sites specifically, Caddy's file server is production-ready with correct `Content-Type` handling, `ETag` support, directory browsing control, and compression out of the box. No additional modules are required. Caddy 2.10 introduced automated Encrypted ClientHello (ECH) support, hiding the SNI from passive observers. Caddy 2.11 defaults to the X25519MLKEM768 hybrid key exchange group, incorporating post-quantum primitives into the TLS handshake.

The documentation site (caddyserver.com) is itself served by Caddy and built on plain HTML, CSS, and minimal JavaScript — a technical choice that directly produces the accessible keyboard navigation verified during the Universal Cake praxis evaluation.

**Licence:** Apache 2.0.
**Governance:** Caddy authors, open source community.
**Current version:** 2.11.2 (March 2026).
**Accompanying documents:** `caddy--web-server-radar-entry-v0.1.1.md`, `caddy--web-server--universal-cake-praxis-evaluation-v0.1.0.md`, `caddy--web-server--manual-installation-and-configuration-on-a-production-server-v0.1.0.md`.
**Ring: Adopt** — preferred web server for all new deployments.

### B — Nginx

Nginx (current stable: 1.30.2, April 2026) is an open source web server and reverse proxy written in C, released under the BSD-2-Clause licence. It is maintained by F5 following their acquisition of NGINX Inc. in 2019. The open source version (nginx.org) is free; Nginx Plus is the commercial variant with additional features and paid support. Nginx has the largest web server ecosystem, the most documentation, and the most community knowledge of any server in this survey. It is fast, stable, and capable of serving static files, acting as a reverse proxy, performing TLS termination, and load balancing.

For pure static site hosting, Nginx is more capable than needed and requires more configuration than Caddy — TLS certificates must be managed separately (typically via Certbot), virtual hosts require explicit configuration blocks, and the configuration syntax, while well-documented, is more verbose than a Caddyfile. Nginx does not have automatic TLS built in. For the current use case — static Hugo sites and webstores, automatic TLS, single server — Caddy is the simpler and more appropriate tool.

Nginx becomes the more appropriate choice when reverse proxy sophistication, high-concurrency performance tuning, or advanced load balancing is required. See the companion reverse proxy entry for this use case.

The F5 acquisition raises a mild sovereignty concern — F5 is a US commercial entity and Nginx is no longer community-governed. The open source licence (BSD-2-Clause) is permissive and the project continues to be actively developed, but the governance model is commercial rather than foundation-governed.

**Licence:** BSD-2-Clause (open source); Nginx Plus is proprietary.
**Governance:** F5 (commercial).
**Current stable version:** 1.30.2 (April 2026).
**Ring: Assess** — strong candidate for reverse proxy and high-traffic scenarios; not adopted for static site hosting in this stack.

### C — Apache2 (httpd)

Apache HTTP Server (current stable: 2.4.x) is the longest-established open source web server, governed by the Apache Software Foundation under the Apache License 2.0. It was the dominant web server for decades and retains a large ecosystem, broad documentation, and near-universal hosting panel support. It uses a distributed configuration model (`sites-available`, `sites-enabled`, `.htaccess`) and manages modules via `a2enmod` and `a2dismod`. TLS is handled by `mod_ssl` with certificates managed by a separate tool (typically Certbot with a systemd timer for renewal).

For this stack, Apache2 has three significant disadvantages compared to Caddy: the distributed configuration structure creates operational overhead and increases the risk of configuration drift; the separate TLS tooling (Certbot) adds a maintenance dependency and a failure mode (silent renewal failures) that Caddy eliminates entirely; and the distro-packaged installation varies between providers, creating a portability friction point that a single static binary does not have.

Apache2 is the current production implementation and is documented here for reference. No new deployments should use Apache2; existing Apache2 installations should be migrated to Caddy as part of normal infrastructure maintenance.

**Licence:** Apache 2.0.
**Governance:** Apache Software Foundation, community-governed.
**Current stable version:** 2.4.x.
**Ring: Hold** — legacy implementation, superseded by Caddy for all new deployments.

---

## Comparison table

| Feature | Caddy 2.11.2 | Nginx 1.30.2 | Apache2 2.4.x |
|---|---|---|---|
| Automatic TLS | Yes — built in | No — requires Certbot | No — requires Certbot |
| Configuration | Single Caddyfile | Multiple config files | Distributed (sites-available, mods) |
| Deployment | Single static binary | Distro-packaged | Distro-packaged |
| Post-quantum TLS | Yes (X25519MLKEM768 default) | No | No |
| ECH support | Yes (2.10+) | No | No |
| HTTP/3 (QUIC) | Yes | Yes (1.25+) | Limited |
| Static file serving | Production-ready | Production-ready | Production-ready |
| Reverse proxy | Yes | Yes — industry standard | Yes — limited |
| Module management | None required | Dynamic modules | a2enmod / a2dismod |
| Licence | Apache 2.0 | BSD-2-Clause | Apache 2.0 |
| Governance | Community | F5 (commercial) | Apache Software Foundation |
| Ring | Adopt | Assess | Hold |

---

## Concerns

**Caddy ACME dependency** — automatic TLS is structurally dependent on outbound HTTPS connections to Let's Encrypt and ZeroSSL at certificate issuance and renewal intervals. This is not a hidden behaviour — it is the mechanism. For air-gapped deployments a private ACME CA or manual certificate management is required. For internet-facing static hosting this is not a practical concern. Full detail in the Caddy radar entry.

**Nginx F5 governance** — F5's acquisition of NGINX in 2019 means the project is commercially governed rather than foundation-governed. The BSD-2-Clause licence is permissive and the open source project continues to be actively developed, but commercial governance introduces a key-contributor risk that community or foundation governance distributes. This is a mild sovereignty concern, noted but not currently blocking for assess status.

**Apache2 TLS renewal failures** — the Certbot-based renewal process is a known operational risk. Renewal failures are not always surfaced prominently and can result in expired certificates causing service outages. This is a maintenance overhead that Caddy eliminates entirely.

## Security assessment

**Network behaviour:**
- [x] Outbound connections during normal use — Caddy makes outbound ACME connections for TLS certificate issuance and renewal (Let's Encrypt, ZeroSSL). Nginx and Apache2 make no outbound connections during normal static file serving.
- [x] Update checks — none of the three perform silent update checks.
- [x] Telemetry or usage data — absent in all three for current versions. Caddy v1 had opt-out telemetry, removed before v2. Nginx and Apache2 have no telemetry.
- [ ] Licence server contact — not present in any of the three.
- [x] Affects networking of content served — all three serve content over TCP/IP. Caddy enforces TLS by default; Nginx and Apache2 require explicit TLS configuration.

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content — Caddy writes TLS certificates to its configured data directory (not alongside content). Nginx and Apache2 write logs to configured log directories. None write unexpected files alongside served content.
- [ ] Caches content outside the working directory — none of the three cache static file content by default.
- [ ] Writes to unexpected locations — not observed.

**Assessment method:** review of official documentation, release notes, and CVE history for each tool. Caddy 2.11.2 CVE batch noted in the Caddy radar entry. Independent network capture not performed for Nginx or Apache2; flagged as a gap if either moves to adopt.

**Assessment status:** Caddy adopted. Nginx in assess. Apache2 on hold.

---

## Relationship to project

Web server selection is relevant to any self-hosted stack serving content over HTTP/HTTPS. This entry is not scoped to a single project. The specific installation and configuration of Caddy for Hugo static site hosting is documented in `caddy--web-server--manual-installation-and-configuration-on-a-production-server-v0.1.0.md`. For reverse proxy use cases, see the companion entry `reverse-proxies--options-for-http-routing-and-tls-termination--v0.1.0.md`.

---

## Status notes

**Ring: Adopt — Caddy (tier A)**
**Ring: Assess — Nginx (tier B)**
**Ring: Hold — Apache2 (tier C)**

What would move Nginx to adopt: a specific use case arising that Caddy cannot satisfy — high-concurrency reverse proxy requirements, advanced load balancing, or fine-grained HTTP configuration needs. A radar entry and security assessment for Nginx in that specific context would be required before adoption.

What would move Apache2 off hold: no current pathway. Apache2 is superseded by Caddy for this stack's use case. Reconsideration would require a provider ecosystem where Caddy is not viable and Apache2 is the only available option.

- Last reviewed: 2026-06-16
- Versions assessed: Caddy 2.11.2, Nginx 1.30.2, Apache2 2.4.x

---

## Links

- https://caddyserver.com
- https://github.com/caddyserver/caddy
- https://nginx.org
- https://github.com/nginx/nginx
- https://httpd.apache.org
- https://github.com/apache/httpd

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

## License (for this document)

TODO

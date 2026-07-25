---
dc:title: "reverse-proxies--options-for-http-routing-and-tls-termination"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "reverse-proxy"
  - "nginx"
  - "traefik"
  - "haproxy"
  - "caddy"
  - "infrastructure"
  - "tls"
  - "load-balancing"
dc:description: "Radar entry surveying open source reverse proxy options for HTTP routing and TLS termination. No reverse proxy has been adopted beyond Caddy's built-in capability. Assess designations established pending a concrete use case."
dc:publisher: ""
dc:date: "2026-06-16"
dc:modified: "2026-06-16"
dc:type: "radar-entry--survey"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-CA"
dc:source: "https://nginx.org https://traefik.io https://www.haproxy.org https://caddyserver.com"
dc:relation: ""
dc:identifier: "reverse-proxies--options-for-http-routing-and-tls-termination"
dc:rights: "TODO"
---

# reverse-proxies--options-for-http-routing-and-tls-termination

## What it is

This entry surveys open source reverse proxy options for HTTP routing, TLS termination, and load balancing. A reverse proxy sits in front of one or more backend services, routing incoming requests by hostname, path, or other criteria, terminating TLS, and optionally balancing load across multiple backends. This is a distinct role from a web server serving static files directly, though several tools cover both roles. Candidates assessed are Nginx, Traefik, HAProxy, and Caddy in its reverse proxy capacity. For web server use cases, see the companion entry `webservers--options-for-static-site-hosting--v0.1.0.md`.

## Current state

No dedicated reverse proxy has been adopted. Caddy currently covers both the web server and reverse proxy roles for this stack from a single binary and Caddyfile, and does so adequately for the current scale and use cases. A dedicated reverse proxy layer would only be warranted if a specific requirement arose that Caddy's reverse proxy capabilities cannot satisfy. This entry establishes the landscape and assess designations in preparation for that eventuality.

## Adoption order

No tier is currently adopted. Tiers reflect preference order if and when a dedicated reverse proxy use case is identified.

**Current de facto approach — Caddy as reverse proxy** — Caddy's built-in reverse proxy capability covers the current requirements. It handles TLS termination, virtual hosting across multiple domains, and basic routing from a single Caddyfile. No separate reverse proxy layer is currently needed. Cross-reference: `webservers--options-for-static-site-hosting--v0.1.0.md`.

**A — Nginx** — assess. The strongest open source reverse proxy candidate for traditional server environments. Mature, battle-tested, largest ecosystem, well-understood configuration. The preferred choice if a dedicated reverse proxy layer is needed for a non-containerised workload.

**B — Traefik** — assess. The preferred choice if the stack moves toward containerised workloads. Automatic service discovery from Docker labels or Kubernetes annotations eliminates manual routing configuration in dynamic environments. Not warranted for a static, operator-managed server topology.

**C — HAProxy** — assess. The highest-performance option for TCP and HTTP load balancing at scale. Warranted when raw throughput, fine-grained load balancing algorithms, or advanced health checks are required. Not warranted at current stack scale.

**D — Caddy as dedicated reverse proxy** — not a separate adoption decision. Caddy's reverse proxy capability is included in the Caddy web server adoption (see `webservers--options-for-static-site-hosting--v0.1.0.md`). Listed here for completeness and cross-reference.

---

## Platform assessments

### Current de facto — Caddy as reverse proxy

Caddy's `reverse_proxy` directive provides HTTP and HTTPS proxying, load balancing (round-robin, random, least connections, first), health checks, header manipulation, and streaming support. For the current stack — a small number of static sites and webstores on a single server with no backend application tier — Caddy's reverse proxy capability is more than sufficient. It shares the same automatic TLS provisioning as Caddy's file server, meaning no additional TLS configuration is needed when adding a reverse-proxied backend.

Caddy's reverse proxy capability is less sophisticated than Nginx, HAProxy, or Traefik for high-traffic, multi-backend, or dynamic service environments. The current stack does not approach those thresholds. If a backend application tier is introduced — for example, a server-side cart for a webstore — Caddy's reverse proxy is the first tool to reach for before evaluating whether a dedicated reverse proxy is needed.

**Ring: No separate adoption decision** — covered by Caddy web server adoption.

### A — Nginx

Nginx (current stable: 1.30.2, April 2026, BSD-2-Clause, maintained by F5) is the most widely deployed open source reverse proxy. It handles HTTP and HTTPS proxying, TLS termination, load balancing (round-robin, least connections, IP hash, weighted), static file serving, caching, and connection limiting. Configuration is explicit and verbose but well-documented, with the largest community knowledge base of any tool in this survey. Performance at high concurrency is close to HAProxy for HTTP workloads. Advanced active health checks and enhanced metrics are in the commercial Nginx Plus tier, not the open source version.

For a non-containerised traditional server environment where a dedicated reverse proxy layer becomes necessary, Nginx is the preferred choice. It is familiar to the broadest range of operators, has the most tutorials and troubleshooting resources, and covers the reverse proxy use case with minimal surprise. The F5 governance concern noted in the web server entry applies here equally — commercial rather than foundation-governed, with the open source BSD-2-Clause licence providing the portability guarantee.

**Licence:** BSD-2-Clause.
**Governance:** F5 (commercial).
**Current stable version:** 1.30.2 (April 2026).
**Ring: Assess** — preferred reverse proxy for traditional server environments pending a concrete use case.

### B — Traefik

Traefik (current stable: v3.x, MIT licence, maintained by Traefik Labs) is a cloud-native Layer 7 reverse proxy and load balancer designed for dynamic environments. Its defining feature is automatic service discovery — in Docker environments, Traefik reads container labels to configure routing rules dynamically without restarting or reloading configuration. In Kubernetes environments it reads Ingress and IngressRoute resources. This eliminates the manual routing configuration that Nginx and HAProxy require when services start and stop frequently.

Traefik also provides automatic TLS via ACME (similar to Caddy), a web UI dashboard, middleware for authentication, rate limiting, and header manipulation, and native metrics for Prometheus. Raw throughput is approximately 20–30% lower than Nginx or HAProxy in benchmarks, but this gap is irrelevant at the current stack's scale.

Traefik is the correct choice if the stack moves toward containerised workloads with Docker Compose or Kubernetes. For a static, operator-managed server topology with a small fixed number of services, Traefik's dynamic service discovery is unnecessary overhead and Nginx or Caddy are simpler choices.

**Licence:** MIT.
**Governance:** Traefik Labs (commercial, open core). The core proxy is MIT-licensed; Traefik Enterprise adds commercial features.
**Current stable version:** v3.x.
**Ring: Assess** — preferred reverse proxy for containerised environments pending a concrete use case.

### C — HAProxy

HAProxy (current stable: 2.9.x, LGPL 2.1, maintained by HAProxy Technologies and community) is the highest-performance open source TCP and HTTP load balancer and proxy. It does not serve static files — it is a dedicated proxy and load balancer. Its strengths are raw throughput, fine-grained load balancing algorithms (round-robin, least connections, source hash, random, URI, header), advanced active health checks, sticky sessions, detailed statistics via a built-in dashboard, and ACL-based routing of exceptional depth. HAProxy consistently leads raw throughput benchmarks for HTTP proxy workloads.

HAProxy does not have automatic TLS built in — certificates must be managed separately. TLS termination is supported but requires operator-managed certificate files. This is a significant operational overhead compared to Caddy or Traefik for environments where automatic TLS is a priority.

HAProxy is the correct choice when raw load balancing performance, fine-grained routing control, or very high connection counts are required. It is overkill for the current stack's scale and use case. It becomes relevant if the stack grows to serve very high traffic volumes or requires sophisticated load balancing across multiple backend servers.

**Licence:** LGPL 2.1 (open source); HAProxy Enterprise is the commercial variant.
**Governance:** HAProxy Technologies (commercial), strong open source community.
**Current stable version:** 2.9.x.
**Ring: Assess** — preferred for high-traffic, high-performance load balancing scenarios pending a concrete use case.

---

## Comparison table

| Feature | Caddy (proxy mode) | Nginx 1.30.2 | Traefik v3.x | HAProxy 2.9.x |
|---|---|---|---|---|
| Primary role | Web server + proxy | Web server + proxy | Proxy + load balancer | Proxy + load balancer |
| Automatic TLS | Yes | No | Yes | No |
| Service discovery | No | No | Yes (Docker/K8s) | No |
| Static file serving | Yes | Yes | No | No |
| Load balancing | Basic | Good | Good | Excellent |
| Raw throughput | Good | Excellent | Good | Excellent |
| Configuration model | Caddyfile (simple) | nginx.conf (verbose) | Labels/YAML (dynamic) | haproxy.cfg (explicit) |
| Web UI | No | No | Yes | Stats page |
| Licence | Apache 2.0 | BSD-2-Clause | MIT | LGPL 2.1 |
| Governance | Community | F5 (commercial) | Traefik Labs (commercial) | HAProxy Technologies |
| Best fit | Current stack | Traditional servers | Containerised workloads | High-traffic / scale |
| Ring | Current (no separate decision) | Assess | Assess | Assess |

---

## Concerns

**No adoption decision yet** — this entry documents the landscape and establishes assess designations. No reverse proxy has been adopted beyond Caddy's built-in capability. A concrete use case must be identified before moving any tool to adopt.

**Nginx F5 governance** — as noted in the web server entry. BSD-2-Clause licence is permissive; F5 commercial governance is a mild sovereignty concern.

**Traefik open core model** — Traefik's core is MIT-licensed but Traefik Enterprise adds commercial features. The boundary between community and enterprise features should be reviewed before adoption to confirm the open source tier covers the required use case.

**HAProxy TLS management** — HAProxy does not provision or renew TLS certificates. A separate certificate management tool (Certbot, or Caddy in certificate management mode) is required. This adds operational overhead compared to Caddy or Traefik.

**Performance gap is currently irrelevant** — benchmarks showing HAProxy and Nginx leading Caddy and Traefik in raw throughput are meaningful at high scale (10,000+ req/s on a single node). At the current stack's scale all four are more than adequate and throughput is not a selection criterion.

---

## Security assessment

**Network behaviour:**
- [x] Outbound connections during normal use — Traefik and Caddy make outbound ACME connections for automatic TLS. Nginx and HAProxy make no outbound connections during normal operation.
- [x] Update checks — none of the four perform silent update checks.
- [x] Telemetry or usage data — absent in open source versions of all four. Traefik sends anonymous usage statistics by default in some versions; verify and disable if present.
- [ ] Licence server contact — not present in any of the four open source versions.
- [x] Affects networking of content proxied — all four terminate TLS and forward traffic to backends. All support HTTPS-only operation for client-facing connections.

**Assessment method:** review of official documentation, release notes, and community sources for each tool. No independent network capture performed; flagged as a gap for any tool moving to adopt.

**Assessment status:** no tool adopted as a dedicated reverse proxy. All in assess pending a concrete use case.

---

## Relationship to project

Reverse proxy infrastructure is relevant to any self-hosted stack that routes HTTP traffic to multiple backends, requires TLS termination at a single ingress point, or needs load balancing across multiple server instances. This entry is not scoped to a single project. For the current web server use case, see `webservers--options-for-static-site-hosting--v0.1.0.md`. A dedicated reverse proxy layer would become relevant if the stack introduces backend application services, containerised workloads, or multi-server deployments.

---

## Status notes

**Ring: No adoption — Caddy covers current requirements**
**Ring: Assess — Nginx (tier A), Traefik (tier B), HAProxy (tier C)**

What would move Nginx to adopt: a specific non-containerised reverse proxy use case identified that Caddy cannot satisfy; a dedicated radar entry for Nginx; a security assessment including network capture.

What would move Traefik to adopt: a decision to move stack workloads to containers (Docker Compose or Kubernetes); a dedicated radar entry for Traefik; a security assessment including network capture.

What would move HAProxy to adopt: a high-traffic or multi-server load balancing use case identified; a dedicated radar entry for HAProxy; a security assessment including network capture.

What would move any tool to hold: discovery of undisclosed telemetry; a licence change away from OSI-approved open source; acquisition by a commercial entity with a history of open-source bait-and-switch; or a pattern of unaddressed security advisories.

- Last reviewed: 2026-06-16
- Versions assessed: Nginx 1.30.2, Traefik v3.x, HAProxy 2.9.x, Caddy 2.11.2

---

## Links

- https://nginx.org
- https://github.com/nginx/nginx
- https://traefik.io
- https://github.com/traefik/traefik
- https://www.haproxy.org
- https://github.com/haproxy/haproxy
- https://caddyserver.com/docs/quick-starts/reverse-proxy

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

## License (for this document)

TODO

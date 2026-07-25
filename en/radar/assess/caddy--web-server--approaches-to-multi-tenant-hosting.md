# Caddy -- Web Server -- Approaches to Multi-tenant hosting

**The single-instance single-Caddyfile approach (what we documented)**

Advantages: operationally simple — one process to monitor, one log to watch at the Caddy level, one configuration file to reason about, one binary to upgrade. TLS certificate management is unified. Resource overhead is minimal. Adding a new tenant is three lines in the Caddyfile and a directory tree. Caddy's architecture is explicitly designed for this pattern and handles it well at this scale.

Disadvantages: all tenants share a single process, so a Caddy bug or misconfiguration affecting one domain block could theoretically affect all domains. A Caddyfile error introduced for one tenant requires a validate-and-reload cycle that touches all tenants. There is no process-level isolation between tenants — one tenant's traffic passes through the same process space as another's.

**Alternative approaches worth knowing about:**

**Multiple Caddy instances, one per tenant.** Each tenant runs its own Caddy process on a non-standard port, with a front-facing reverse proxy (another Caddy instance, or HAProxy) routing by hostname. Provides genuine process isolation. Significantly more complex to operate — N+1 processes, N+1 Caddyfiles, N+1 systemd units. Warranted in regulated environments or where tenants are organisationally independent and must not share infrastructure at any level.

**Caddy with the HTTP subdirectory approach.** All tenants share a domain, served from subdirectories rather than separate domains (`example.ca/tenant1`, `example.ca/tenant2`). Eliminates the multi-domain TLS complexity entirely. Not appropriate here since each tenant has their own domain identity.

**Nginx or HAProxy as a front-end reverse proxy with per-tenant backends.** The reverse proxy handles TLS termination and routes by hostname to per-tenant static file servers or application processes. Common in larger hosting environments. Adds a layer of complexity and a new component to manage. No advantage over a single Caddy instance for pure static file serving at this scale.

**Container-per-tenant.** Each tenant runs in a Docker or Podman container with its own Caddy or Nginx instance. Strong isolation, reproducible environments, easy to add and remove tenants. Adds significant operational complexity — container runtime, orchestration, networking, volume management. The right answer at scale or where tenants are genuinely independent organisations with different security requirements. Overkill for two tenants on a sovereign stack.

**Object storage with CDN.** Hugo build output pushed directly to an S3-compatible object store (Cloudflare R2, Backblaze B2, a self-hosted MinIO instance) and served via CDN or directly over HTTPS. No web server on the origin at all. Extremely low operational overhead, high availability by default. The sovereignty concern is real — this introduces a dependency on a third-party object store and CDN, which conflicts with the stack's provider-agnostic goals unless a self-hosted object store is used. Worth a radar entry in its own right.

**The honest assessment for your context:**

At two tenants, four domains, and a sovereignty-oriented stack, the single-instance single-Caddyfile approach we documented is correct. The isolation argument for multiple instances does not apply until either the number of tenants grows significantly, tenants become organisationally independent, or a regulated environment requires it. Container-per-tenant is the next sensible step if the stack grows, not multiple bare-metal Caddy instances. Object storage is worth a radar entry as a future alternative worth examining.

Shall we proceed to the shim document, or does any of this change the server topology conversation first?





## You said: ok, shim
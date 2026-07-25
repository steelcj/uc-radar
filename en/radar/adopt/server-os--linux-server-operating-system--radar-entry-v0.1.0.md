---
dc:title: "server-os--linux-server-operating-system"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "radar"
  - "server-os"
  - "linux"
  - "infrastructure"

dc:description: "Radar entry surveying and rating Linux server operating system options for sovereign self-hosted infrastructure, with Debian 13 Trixie adopted as the preferred baseline."
dc:publisher: ""
dc:date: "2026-06-16"
dc:modified: "2026-06-16"
dc:type: "radar-entry--survey"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-CA"
dc:source: "https://www.debian.org https://ubuntu.com https://alpinelinux.org https://nixos.org https://almalinux.org https://rockylinux.org https://fedoraproject.org"
dc:relation: ""
dc:identifier: "server-os--linux-server-operating-system"
dc:rights: "TODO"
---

# server-os--linux-server-operating-system

## What it is

A survey and technical rating of Linux server operating system options relevant to the Hugo hosting stack, evaluated against the stack's primary goals of sovereignty, provider portability, operational simplicity, and long-term stability. The survey informed a decision about the baseline server OS for all production and future deployments.

## Why interesting

The choice of server OS affects every other layer of the stack — the shim provisioning layer, Ansible orchestration roles, Caddy installation method, and provider image availability. Locking in a well-reasoned baseline before writing further documentation and tooling avoids accumulating debt from an implicit or unexamined default. The stack's sovereignty goals introduce criteria — community governance, licence posture, external dependency minimisation — that are not present in most OS selection decisions, which made an explicit comparative assessment worthwhile.

A secondary driver is provider portability: the planned migration from Digital Ocean to Canadian and Icelandic providers requires confidence that the chosen OS is available as a base image across a broad range of providers. Where a provider does not offer the chosen image natively, a custom image will be built and uploaded. Digital Ocean is the initial case where this may be required for Debian 13.

## Decision

**Debian 13 "Trixie" is adopted as the preferred baseline server OS for the Hugo hosting stack.**

The tiebreaker between Debian 13 and Ubuntu 24.04 LTS — both scored 5 overall — is sovereignty. Debian is governed entirely by its community under the Debian Social Contract, with no commercial entity in control of the project, the roadmap, or the package infrastructure. Ubuntu is genuinely open source but Canonical is a commercial company whose interests, while currently well-aligned with the community, could diverge. For a stack whose defining values include sovereignty and long-term independence from commercial dependencies, Debian is the correct choice.

The practical migration cost from the current Ubuntu 24.04 LTS baseline is near-zero. The Caddy apt repository supports Debian. Ansible works identically. All Caddyfile configuration, deployment scripts, and directory structures are unchanged. The only change required in existing documentation and shim tooling is the OS image identifier — `ubuntu-24-04` becomes `debian-13` in provider API calls and image selectors.

Where Debian 13 is not available as a provider image, a custom Debian 13 image will be built and uploaded to the provider. This is a known requirement for the initial Digital Ocean deployment and is documented as a pending action in the shim documentation.

## Technical rating

Options are ordered by preference for the Hugo hosting stack's specific goals: sovereignty, provider portability, operational simplicity, and long-term stability.

### Summary table

| OS | Sovereignty | Ecosystem | Stability | Stack fit | Overall |
|---|---|---|---|---|---|
| Debian 13 ✓ | 5 | 4 | 5 | 5 | **5** |
| Ubuntu 24.04 LTS | 4 | 5 | 5 | 5 | **5** |
| Ubuntu 26.04 LTS | 4 | 5 | 3 | 4 | **4** |
| Alpine Linux | 4 | 2 | 4 | 2 | **3** |
| NixOS | 5 | 3 | 4 | 2 | **3** |
| AlmaLinux / Rocky | 4 | 3 | 5 | 2 | **3** |
| Fedora Server | 3 | 4 | 2 | 1 | **2** |

### 1. Debian 13 "Trixie" — adopted ✓

Community-governed under the Debian Social Contract, no commercial entity in control. MPL-compatible licensing throughout. Same apt tooling as Ubuntu — Caddy's apt repository supports it, Ansible works identically, all documentation changes are one word (`ubuntu` → `debian` in image selectors). Packages are older but more thoroughly tested. Support cycle is effectively longer than Ubuntu LTS in practice. Minimal base install. The most sovereign choice available in the Debian/Ubuntu family with near-zero migration cost from the current documentation.

Rating: 5/5 for sovereignty, 4/5 for ecosystem, 5/5 for stability, 5/5 for fit with this stack.

### 2. Ubuntu Server 24.04 LTS — strong second, previous baseline

Largest community, best documentation, universally available across providers, broadest software support. Caddy, Ansible, and all tooling assume it. Five years standard support to April 2029, ten with Ubuntu Pro. Canonical is a commercial entity which is a mild sovereignty concern, though Ubuntu itself is genuinely open source. Snap is present but minimal on server installs. The pragmatic choice and what the stack's earlier documentation specified before this decision was made.

Rating: 4/5 for sovereignty, 5/5 for ecosystem, 5/5 for stability, 5/5 for fit with this stack.

### 3. Ubuntu Server 26.04 LTS — future baseline, not yet

The new LTS, released April 23, 2026. Same sovereignty profile as 24.04. Not recommended for new production deployments until the 26.04.1 point release in August 2026 and until provider image availability is universal. Reassess in Q4 2026. Migration from 24.04 is straightforward when the time comes. Not evaluated as a candidate for this decision on timing grounds.

Rating: 4/5 for sovereignty, 5/5 for ecosystem, 3/5 for production readiness right now, 4/5 for fit with this stack.

### 4. Alpine Linux — specialised use only

Radically minimal, security-oriented, excellent as a container base. musl libc and busybox create friction with some Ansible modules and glibc-dependent tooling. Not recommended as the primary server OS for this stack but worth knowing for containerised workloads if the stack ever moves in that direction.

Rating: 5/5 for minimalism, 3/5 for ecosystem compatibility, 4/5 for security posture, 2/5 for fit with this stack as a bare metal server OS.

### 5. NixOS — future interest, not now

Fully declarative, reproducible, atomic rollbacks. Deeply aligned with sovereignty and reproducibility values. The barrier is the learning investment — a unique configuration language and mental model that touches everything. Worth revisiting if the stack moves toward fully declarative infrastructure. Not recommended for immediate adoption.

Rating: 5/5 for reproducibility and sovereignty, 2/5 for immediate operational accessibility, 3/5 for ecosystem, 2/5 for fit with this stack right now.

### 6. AlmaLinux 9 / Rocky Linux 9 — enterprise RHEL replacement, wrong ecosystem

Viable and well-governed, but the wrong ecosystem for this stack. `dnf` instead of `apt`, COPR instead of apt repositories for Caddy, SELinux configuration overhead, Ansible roles need rewriting. No compensating benefit over Debian or Ubuntu for static site hosting. Valuable in environments with existing RHEL investment.

Rating: 4/5 for enterprise stability, 2/5 for fit with this stack.

### 7. Fedora Server — development and testing only

Six-month release cycle with no LTS. Inappropriate for production servers requiring stable, long-supported baselines. Useful for testing new tooling before it reaches Debian or Ubuntu.

Rating: 4/5 for cutting-edge packages, 1/5 for production server use.

## Concerns

**Debian 13 provider image availability** — Debian 13 "Trixie" was released August 2025. Not all providers carry it as a standard image. Digital Ocean does not currently offer Debian 13 as a standard Droplet image. A custom Debian 13 image will need to be built and uploaded for initial Digital Ocean deployments. This is a known short-term operational overhead, documented as a pending action in the shim documentation, and is expected to resolve as providers update their image catalogues.

**Ecosystem breadth** — Debian's package ecosystem and community documentation are smaller than Ubuntu's. In practice this rarely causes problems for the tooling in this stack, but operators should expect to adapt some Ubuntu-specific guides when configuring new tooling.

**NixOS and Alpine** — both are noted for future consideration as the stack evolves. NixOS warrants a dedicated radar entry when declarative infrastructure becomes a stack goal. Alpine warrants a dedicated radar entry if containerised workloads are adopted.

## Security assessment

**N/A for this entry in the traditional sense** — the security posture of each OS is evaluated individually when that OS is used as the basis for a server configuration. The relevant security considerations for Debian 13 in this stack are documented in the Caddy installation document and the shim contract document.

**Assessment status:** Adopted for Debian 13. No further assessment action required for this entry.

## Relationship to project

The server OS sits at the base of every layer of any self-hosted infrastructure stack. It is the ingress condition for the orchestration layer and the environment in which all server-side tooling operates. This decision affects the shim layer (OS image selection in provider API calls), orchestration tooling (package management assumptions), and all installation documentation. It is a foundational infrastructure decision that applies across projects rather than being scoped to a single stack.

## Status notes

**Ring: Adopt — Debian 13 "Trixie"**

Adopted on 2026-06-16. The decision was made on sovereignty grounds, with Debian 13's community governance under the Debian Social Contract as the tiebreaker over Ubuntu 24.04 LTS. Both scored equally on overall stack fit.

**Pending actions arising from this decision:**

- Build and upload a custom Debian 13 image to Digital Ocean for initial deployments.
- Update all stack documentation that references Ubuntu 24.04 LTS to Debian 13 — specifically the Caddy manual installation document (`caddy--web-server--manual-installation-and-configuration-on-a-production-server-v0.1.0.md`) and the shim contract document (`shims--options-for-provider-agnostic-approaches-to-provisioning--v0.1.0.md`).
- Confirm that the Ansible orchestration roles function correctly against a Debian 13 base image before the first production deployment.

**What would move this to hold:** a sustained period in which Debian 13 is not available as a native image at any of the target providers, making custom image management an unacceptable operational overhead. In that case Ubuntu 24.04 LTS is the fallback with no other changes to the stack.

- Last reviewed: 2026-06-16
- OS adopted: Debian 13 "Trixie"
- Decision basis: sovereignty tiebreaker over Ubuntu 24.04 LTS

## Links

- https://www.debian.org
- https://www.debian.org/releases/trixie/
- https://wiki.debian.org/DebianTrixie
- https://www.debian.org/social_contract

## License (for this document)

TODO

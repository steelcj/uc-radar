# Shims -- Options for Provider-Agnostic Approaches to Provisioning

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md

---

## Abstract

This document defines the provider-agnostic shim contract for the Hugo hosting stack and evaluates the classes of tooling available for implementing shims against providers that do not support Terraform. A shim is the thin, provider-specific layer responsible for creating a server, injecting an SSH key, and configuring a provisional firewall — and nothing else. The shim contract defines what any shim must receive as input and must produce as output, regardless of provider. This contract is the stable interface between the provisioning layer and the orchestration layer above it; it does not change when the provider changes. The document examines why Terraform is insufficient as a universal shim implementation approach, notes that the current Terraform-based shim should be migrated to OpenTofu on sovereignty and licensing grounds regardless of the provider portability question, and evaluates the principal alternatives — direct provider API scripting in Python, Pulumi, and OpenTofu — against the contract and the stack's sovereignty and portability goals. It does not implement a shim for any specific provider; implementation follows provider selection and radar assessment of the chosen approach.

---

## Sources and Acknowledgements

The shim and orchestrator terminology used in this document follows the <a name="apa-styleguide-citation"></a>[style guide for technical documentation for technologists (Steel, 2026a)](#apa-styleguide-reference). Document formatting follows the <a name="apa-markdown-citation"></a>[web-ready unrendered markdown using APA 7 specification (Steel, 2026b)](#apa-markdown-reference). Information on OpenTofu's licensing, governance, and provider ecosystem draws on <a name="apa-opentofu-citation"></a>[OpenTofu (2026)](#apa-opentofu-reference) and <a name="apa-scalr-citation"></a>[Scalr (2026)](#apa-scalr-reference). Information on Pulumi's provider support and architecture draws on <a name="apa-pulumi-citation"></a>[Pulumi (2026)](#apa-pulumi-reference). The sovereignty framing draws on the Universal Cake systems praxis as described in <a name="apa-steel-2026-citation"></a>[Steel (2026c)](#apa-steel-2026-reference).

---

## 1. The shim contract

The shim contract is the stable specification that any shim implementation must satisfy, regardless of which provider it targets. It defines the minimum viable provisioning output that the orchestration layer requires to begin its work. The contract has two parts: what the shim receives as input, and what it must produce as output.

### 1.1 Shim inputs

A shim receives the following inputs from the operator before execution:

- **Provider credentials** — API key, token, or equivalent authentication material for the target provider. The shim must never store credentials; it receives them at runtime via environment variables or a secrets manager and uses them for the duration of the run only.
- **Server specification** — the machine type or size, the operating system image, and the target region or availability zone. These are provider-specific values and are the only inputs that change between providers.
- **SSH public key** — the public half of the key pair that the orchestrator will use to connect. The shim injects this key during provisioning; the private key never touches the shim.
- **Operator identifier** — a label or tag used to identify the server in the provider's interface, for human operator navigation only.

### 1.2 Shim outputs

A shim must produce the following outputs on successful completion:

- **IP address** — the public IPv4 address of the provisioned server.
- **SSH user** — the username the orchestrator must use to connect, which may be provider-specified (e.g. `root`, `ubuntu`, `admin`) or operator-defined.
- **SSH port** — the port on which SSH is listening. Default is 22; some providers assign non-standard ports.
- **Firewall state** — confirmation that the provisional firewall is in place, permitting SSH only. This is a statement of known state, not a test result; the orchestrator is responsible for verifying connectivity.
- **Run log** — a structured record of what the shim did, suitable for operator review and for passing to the orchestrator as context.

### 1.3 What the shim must not do

The shim must not install software beyond what the base image provides. It must not configure the operating system beyond injecting the SSH key and applying the provisional firewall. It must not make assumptions about what the server will eventually do. It must not store credentials or write sensitive material to disk. It must not require the operator to have provider-specific tooling installed beyond what the shim itself specifies as a dependency.

The shim's job is to produce a normalised SSH baseline. Everything that follows is the orchestrator's responsibility.

---

## 2. The existing implementation

The Hugo hosting stack currently uses a Terraform shim targeting Digital Ocean. Terraform (now at version 1.x under HashiCorp's Business Source License 1.1, owned by IBM following the February 2025 acquisition) defines the Droplet, injects the SSH key, and configures the firewall via the Digital Ocean Terraform provider. State is managed locally or in a remote backend. The shim produces the IP address and SSH access required by the Ansible orchestrator.

This implementation has two problems, one acute and one structural. The acute problem is provider portability: Terraform's usefulness as a shim depends entirely on the existence of a mature Terraform provider for the target provider. The Canadian and Icelandic providers the stack is moving toward have no Terraform provider, or have providers too immature to rely on for production provisioning. The structural problem is sovereignty: Terraform is now a commercial IBM product under the BSL 1.1, which is not an OSI-approved open source licence. A stack built on Universal Cake values of sovereignty and long-term independence should not depend on a proprietary IBM product as a core infrastructure component, regardless of the provider portability question. We consider migration away from Terraform to be warranted on sovereignty grounds alone.

---

## 3. Why the provider gap is not solved by IaC tooling alone

The provider gap problem — a target provider has no IaC support — is not solved by switching IaC tools. OpenTofu has 3,900+ providers and Pulumi has 150+, but these are still finite lists maintained by the IaC community. A small Canadian or Icelandic hosting provider with a REST API and no community presence will not appear on either list. The correct framing is therefore not "which IaC tool has the most providers?" but "what do we do when no IaC provider exists for our target host?"

The answer is direct API scripting: a Python script that calls the provider's REST API directly, produces the shim contract outputs, and requires no IaC tooling at all. This approach works for any provider that exposes an HTTP API, which is the minimum viable interface for a provider worth using. It requires more code per provider than an IaC-managed shim, but it is universally applicable and has no external dependencies beyond Python and the `requests` library.

This does not mean IaC tooling is irrelevant. For providers that do have good IaC support — Digital Ocean, Hetzner, OVHcloud, Vultr, Linode — an IaC-managed shim is preferable because it provides state management, drift detection, and idempotency. The correct architecture is therefore a two-track approach: IaC-managed shims for providers with mature provider support, and direct API shims for providers without it. Both tracks produce the same shim contract outputs. The orchestrator does not know or care which track produced them.

---

## 4. Evaluated approaches

### 4.1 OpenTofu

OpenTofu is the MPL 2.0-licensed fork of Terraform, governed by the Linux Foundation and accepted into the Cloud Native Computing Foundation in April 2025. The current stable release is v1.12.1 (June 2026). It is a drop-in replacement for Terraform: the same HCL configuration language, the same provider binary protocol, and full state file compatibility with Terraform 1.5.x and most later versions. Provider binary compatibility is complete — the same provider binaries serve both engines. The OpenTofu registry at registry.opentofu.org lists 3,900+ providers and 23,600+ modules as of June 2026.

OpenTofu has shipped several features that the Terraform open-source CLI does not have: built-in state encryption (v1.7), early variable evaluation (v1.8), provider iteration with `for_each` (v1.9), and OCI registry support (v1.10). It is in active development under multi-vendor, foundation-governed stewardship — a governance model that distributes the key-contributor risk that a single-vendor commercial product concentrates.

For the Hugo hosting stack, OpenTofu is the correct replacement for the existing Terraform shim on all providers that have an IaC provider available. The migration path from Terraform to OpenTofu is replacing the `terraform` binary with `tofu`; existing HCL configuration and state require no changes for a Digital Ocean shim. This migration should be treated as a prerequisite for any new provider work, not deferred until a new provider is selected.

**Limitations:** OpenTofu does not solve the provider gap problem for providers with no IaC support. It is the right tool for providers that already have providers; it cannot create providers for those that do not.

**Recommendation:** adopt for providers with existing IaC support. Migrate the existing Digital Ocean Terraform shim to OpenTofu as an immediate step. Raise a radar entry for OpenTofu.

### 4.2 Direct provider API scripting in Python

A Python script that calls the provider's REST API directly is the most universally applicable shim implementation. It requires only Python 3.x and the `requests` library (or the provider's official Python SDK where one exists), has no IaC tooling dependency, and works for any provider that exposes an HTTP API. The script receives credentials via environment variables, calls the provider's create-server endpoint, injects the SSH key, applies the provisional firewall rules, polls until the server is reachable, and writes the shim contract outputs to stdout as structured JSON.

The disadvantages relative to IaC-managed shims are real: no state management, no drift detection, no idempotency by default. A script that creates a server does not know whether the server already exists unless it explicitly queries for it. These properties must be implemented manually or accepted as limitations. For the shim use case — which creates servers rarely and on operator request, not continuously — the absence of drift detection is not a significant operational risk. The operator knows what was provisioned because they ran the shim. Idempotency can be approximated by querying for an existing server with the operator identifier before creating a new one.

The Python approach also has a meaningful advantage: it is the most readable and auditable implementation for a sovereignty-oriented stack. A 150-line Python script calling a documented REST API is fully transparent — an operator can read it, understand it, and modify it without tooling knowledge beyond Python. An OpenTofu or Pulumi configuration requires understanding of the IaC tool's state model and provider architecture in addition to the provider's API.

**Recommendation:** adopt as the primary approach for providers with no IaC support. Raise a radar entry for the direct API scripting pattern. Produce a reference implementation for the first non-Digital-Ocean provider adopted.

### 4.3 Pulumi

Pulumi is an IaC platform that supports defining cloud resources in general-purpose programming languages — Python, TypeScript, Go, .NET, Java, and YAML. The current version is v3.230.0 (April 2026). It supports 150+ cloud providers natively and can bridge any Terraform provider via its "any Terraform provider" feature, which wraps a Terraform provider binary in a Pulumi SDK. It also supports dynamic providers in Python and TypeScript, which allow custom resource types to be defined inline without publishing a separate provider package.

Pulumi's dynamic providers are the most relevant feature for the provider gap problem: a dynamic provider written in Python can call any REST API and present the result as a Pulumi-managed resource, gaining Pulumi's state management and idempotency while remaining as flexible as direct API scripting. This is a meaningful advantage over pure direct scripting for operators who want IaC-style state management without waiting for a community provider to be published.

The disadvantages are complexity and commercial entanglement. Pulumi is maintained by Pulumi Corporation under an Apache 2.0 licence for the core SDK, but the Pulumi Cloud service — remote state management, deployment history, policy enforcement — is a commercial SaaS product. A sovereignty-oriented stack should use local state backends rather than Pulumi Cloud, which is supported but not the default experience. The learning curve for Pulumi is higher than for OpenTofu — it introduces a programming language runtime and a more complex state model — and the community is smaller than the OpenTofu and Terraform ecosystem.

**Recommendation:** assess. Pulumi's dynamic providers are worth examining for the provider gap use case. Not recommended as a replacement for OpenTofu on providers with existing IaC support — the added complexity is not justified. Raise a radar entry for Pulumi, deferred until a provider with no IaC support is actively being evaluated.

---

## 5. Recommended path

We recommend a two-track approach to shim implementation that separates the IaC-managed case from the provider gap case, while ensuring both tracks produce the same contract outputs and feed into the same orchestration layer.

**Track one — IaC-managed shims:** for providers with mature IaC provider support, use OpenTofu. Migrate the existing Digital Ocean Terraform shim to OpenTofu immediately. This migration is low-risk — replace the binary, run `tofu init`, confirm the existing state is readable, and proceed. No HCL changes are required for the Digital Ocean shim.

**Track two — direct API shims:** for providers with no IaC support, write a Python script calling the provider REST API directly. This is the universal fallback and the approach most consistent with the stack's sovereignty goals. The script produces the shim contract outputs defined in section 1 and requires no external tooling beyond Python 3.x.

Both tracks feed into the same orchestration layer via the same contract. The orchestrator does not distinguish between them.

**Immediate actions:**

- Raise a radar entry for OpenTofu and begin adoption for the Digital Ocean shim.
- Raise a radar entry for the direct API scripting pattern.
- Raise a radar entry for Pulumi, specifically examining dynamic providers for the provider gap use case, deferred until a provider with no IaC support is actively being evaluated.
- When a Canadian or Icelandic provider is selected, determine which track applies based on available IaC provider maturity and proceed accordingly.

---

## 6. Shim contract reference

The following is the normative shim contract for reference and for use when writing new shim implementations.

### 6.1 Input environment variables

| Variable | Description |
|---|---|
| `SHIM_PROVIDER_API_KEY` | Provider authentication credential |
| `SHIM_SERVER_TYPE` | Provider-specific machine type or size identifier |
| `SHIM_IMAGE` | Provider-specific OS image identifier |
| `SHIM_REGION` | Provider-specific region or availability zone identifier |
| `SHIM_SSH_PUBLIC_KEY` | SSH public key to inject, in OpenSSH format |
| `SHIM_OPERATOR_LABEL` | Human-readable label for the server in the provider UI |

### 6.2 Output JSON

A shim must write the following JSON object to stdout on successful completion:

```json
{
  "ip_address": "192.0.2.1",
  "ssh_user": "root",
  "ssh_port": 22,
  "firewall_state": "ssh_only",
  "provider": "example-provider",
  "region": "ca-east-1",
  "operator_label": "prod-web-01",
  "provisioned_at": "2026-06-16T14:30:00Z",
  "shim_version": "0.1.0"
}
```

### 6.3 Exit codes

| Code | Meaning |
|---|---|
| 0 | Success — shim contract outputs written to stdout |
| 1 | Provider API error — details written to stderr |
| 2 | Input validation error — missing or invalid environment variable |
| 3 | Timeout — server did not reach SSH-reachable state within the timeout period |

---

## Resources

### Governing documents
- [Style guide for technical documentation for technologists](#apa-styleguide-reference)
- [Web-ready unrendered markdown using APA 7](#apa-markdown-reference)

### IaC tooling
- [OpenTofu documentation](https://opentofu.org/docs/)
- [OpenTofu registry](https://registry.opentofu.org)
- [Pulumi documentation](https://www.pulumi.com/docs/)
- [Pulumi dynamic providers](https://www.pulumi.com/docs/iac/concepts/providers/)

### Radar entries to raise
- opentofu--iac-tool--radar-entry (pending)
- direct-api-scripting--provisioning-approach--radar-entry (pending)
- pulumi--iac-tool--radar-entry (pending)

---

## References

<a name="apa-opentofu-reference"></a>OpenTofu. (2026). *OpenTofu documentation* (Version 1.12.1). Linux Foundation. https://opentofu.org/docs/
[Return to citation](#apa-opentofu-citation)

<a name="apa-pulumi-reference"></a>Pulumi. (2026). *Pulumi documentation* (Version 3.230.0). Pulumi Corporation. https://www.pulumi.com/docs/
[Return to citation](#apa-pulumi-citation)

<a name="apa-scalr-reference"></a>Scalr. (2026). *What is OpenTofu? 2026 guide to the open-source Terraform fork*. Scalr. https://scalr.com/learning-center/what-is-opentofu
[Return to citation](#apa-scalr-citation)

<a name="apa-styleguide-reference"></a>Steel, C. (2026a). *Style guide: Technical documentation for technologists* (Version 0.2.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-styleguide-citation)

<a name="apa-markdown-reference"></a>Steel, C. (2026b). *Web-ready unrendered markdown using APA 7* (Version 0.2.2) [Technical document]. https://universalcake.ca
[Return to citation](#apa-markdown-citation)

<a name="apa-steel-2026-reference"></a>Steel, C. (2026c). *Universal Cake as systems theory and systems praxis* (Version 1.0) [Working paper]. https://universalcake.ca
[Return to citation](#apa-steel-2026-citation)

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

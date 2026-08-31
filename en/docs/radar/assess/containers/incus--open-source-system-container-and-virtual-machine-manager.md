# Incus

Version: 0.1.0
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7.md v0.2.1

## Abstract

Incus is an open-source system container and virtual machine manager developed under the Linux Containers umbrella. It provides a unified platform for running both system containers and virtual machines using a common API and management interface. Incus is widely regarded as the community-led successor to LXD following governance changes in 2023.

## Sources and Acknowledgements

The factual content of this document is derived from the official Linux Containers project documentation, release materials, and project governance information. All such sources are cited in the body using the Citation Anchor Pair workflow and are listed in full in the References section.

## 1. Overview and key facts

Incus is designed to deliver the efficiency and density of containers alongside the flexibility and isolation of virtual machines, making it suitable for development environments, private clouds, homelabs, and production infrastructure (<a name="apa-incus-lts-announcement-citation"></a>[Linux Containers, 2026a](#apa-incus-lts-announcement-reference)). Incus is offered as a hosted [demonstration environment](https://linuxcontainers.org/incus/try-it/) for evaluation, and its source code is maintained in a [public repository](https://github.com/lxc/incus).

The following table summarises key project facts.

| Attribute | Value |
|----------------------|----------------------------|
| Developer | Linux Containers Community |
| License | Apache License 2.0 |
| Programming Language | Go |
| Current LTS Release | Incus 7.0 LTS |
| LTS Release Date | May 2026 |
| Support Period | Through June 2031 |

## 2. Origins and purpose

Incus was created in 2023 following changes to the governance and stewardship of the LXD project. The project was founded by Stéphane Graber. Several long-time Linux Containers contributors established Incus to maintain a fully community-driven platform with open governance and transparent development processes.

The project's objectives include:

- Providing a modern system container platform.
- Supporting virtual machines and containers through a common interface.
- Enabling scalable private-cloud infrastructure.
- Maintaining open community governance.
- Preserving compatibility with existing LXD workflows where practical.

Incus seeks to offer a lightweight alternative to traditional hypervisors while retaining many of the capabilities expected from modern cloud platforms (<a name="apa-incus-history-citation"></a>[Linux Containers, 2026e](#apa-incus-history-reference)).

## 3. Architecture

Incus combines multiple technologies into a single management platform.

### 3.1 Containers

System containers are implemented using the Linux Containers (LXC) runtime. Unlike application containers, system containers run a complete Linux userspace and typically include:

- systemd
- SSH services
- package managers
- multiple running processes

This allows containers to behave similarly to lightweight virtual machines while retaining the performance benefits of containerization.

### 3.2 Virtual machines

Virtual machines are implemented using QEMU and KVM. Incus manages virtual machines using the same command-line tools and API used for containers, simplifying administration.

### 3.3 Unified management

Both containers and virtual machines are managed through:

- REST API
- Incus CLI
- Web-based integrations
- Automation tooling

This unified approach allows administrators to use consistent workflows regardless of workload type (<a name="apa-incus-docs-citation"></a>[Linux Containers, 2026d](#apa-incus-docs-reference)).

## 4. Storage features

Incus supports a variety of storage backends, including:

- ZFS
- LVM
- Ceph
- btrfs
- Directory-based storage

Storage pools can be shared across clusters and support:

- Snapshots
- Incremental backups
- Instance migration
- Storage quotas
- Replication

Object storage functionality based on S3-compatible APIs is also available in modern Incus deployments (<a name="apa-incus-docs-citation-2"></a>[Linux Containers, 2026d](#apa-incus-docs-reference)).

## 5. Networking features

Incus includes extensive networking capabilities:

- Linux bridges
- OVN software-defined networking
- VLAN support
- Network ACLs
- Load balancing
- External routing integration

These features allow deployments ranging from single-node development systems to large clustered environments.

## 6. Clustering and scalability

Incus supports clustering across multiple physical hosts, scaling from a single developer's laptop to a full cluster of up to 50 servers.

Cluster features include:

- Live migration
- Distributed storage integration
- High availability
- Centralized management
- Workload placement controls

This allows organizations to scale from a single server to multi-rack infrastructure while maintaining a consistent operational model (<a name="apa-incus-clustering-citation"></a>[Linux Containers, 2026c](#apa-incus-clustering-reference)).

## 7. Automation and integration

Incus integrates well with infrastructure automation platforms.

Common integrations include:

- Ansible
- Terraform
- OpenTofu
- Kubernetes Cluster API
- Custom REST API clients

Because Incus exposes a comprehensive REST API, most infrastructure-as-code tools can manage Incus resources directly.

## 8. Security

Security has been a major design goal of Incus since its inception.

Notable security features include:

- Unprivileged containers by default
- Fine-grained authorization controls
- Restricted device passthrough
- Namespaces and cgroup isolation
- Secure API authentication

Beginning with Incus 7.0 LTS, support for several legacy Linux technologies, including cgroup v1 and xtables, was removed in favor of modern kernel interfaces (<a name="apa-incus-lts-notes-citation"></a>[Linux Containers, 2026b](#apa-incus-lts-notes-reference)).

## 9. Migration from LXD

Organizations currently using LXD can migrate to Incus using the provided migration utility:

```bash
lxd-to-incus
```

The migration process is designed to preserve:

- Containers
- Virtual machines
- Networks
- Storage pools
- Profiles
- Images

This significantly reduces the effort required to transition existing environments (<a name="apa-incus-migration-citation"></a>[Linux Containers, 2026g](#apa-incus-migration-reference)).

## 10. Ecosystem

Incus is available in numerous Linux distributions, including:

- Debian
- Ubuntu
- openSUSE
- Arch Linux

The ecosystem also includes IncusOS, a modern immutable operating system platform optimized specifically for hosting Incus workloads and clusters. IncusOS relies on modern security technologies to provide strong boot security and full disk encryption, and its immutable base with an A/B update scheme suits both home and datacenter-scale virtualization (<a name="apa-incusos-citation"></a>[Linux Containers, 2026f](#apa-incusos-reference)).

## 11. Evaluating Euro-Office with Incus

For evaluating Euro-Office or similar self-hosted collaboration platforms, Incus offers several advantages over Docker-based deployments:

- Runs a complete Ubuntu or Debian system.
- Supports systemd-based services without special workarounds.
- Provides snapshots and rollback capabilities.
- Simplifies network exposure and testing.
- Offers production-like environments.
- Supports both containers and virtual machines.

A common evaluation approach would be:

1. Create an Incus container running Ubuntu 24.04 LTS.
2. Install Euro-Office from source.
3. Create a second container running Nextcloud.
4. Connect the services together.
5. Test document compatibility, collaboration features, accessibility, and resource consumption.

This approach closely resembles a real-world deployment while remaining lightweight and reproducible.

## Resources

### Documentation

- [Incus documentation](#apa-incus-docs-reference)
- [Incus clustering documentation](#apa-incus-clustering-reference)

### Releases

- [Incus 7.0 LTS release announcement](#apa-incus-lts-announcement-reference)
- [Incus 7.0 LTS release notes](#apa-incus-lts-notes-reference)

### Project and governance

- [Incus project history and governance](#apa-incus-history-reference)
- [IncusOS project overview](#apa-incusos-reference)

### Migration

- [Migrating from LXD to Incus](#apa-incus-migration-reference)

## References

<a name="apa-incus-lts-announcement-reference"></a>Linux Containers. (2026a). *Incus 7.0 LTS release announcement*. https://linuxcontainers.org/incus
[Return to citation](#apa-incus-lts-announcement-citation)

<a name="apa-incus-lts-notes-reference"></a>Linux Containers. (2026b). *Incus 7.0 LTS release notes*. https://linuxcontainers.org/incus
[Return to citation](#apa-incus-lts-notes-citation)

<a name="apa-incus-clustering-reference"></a>Linux Containers. (2026c). *Incus clustering documentation*. https://linuxcontainers.org/incus/docs/main/explanation/clustering/
[Return to citation](#apa-incus-clustering-citation)

<a name="apa-incus-docs-reference"></a>Linux Containers. (2026d). *Incus documentation*. https://linuxcontainers.org/incus/docs/main/
[Return to citation](#apa-incus-docs-citation)

<a name="apa-incus-history-reference"></a>Linux Containers. (2026e). *Incus project history and governance*. https://linuxcontainers.org/incus
[Return to citation](#apa-incus-history-citation)

<a name="apa-incusos-reference"></a>Linux Containers. (2026f). *IncusOS project overview*. https://linuxcontainers.org/incus
[Return to citation](#apa-incusos-citation)

<a name="apa-incus-migration-reference"></a>Linux Containers. (2026g). *Migrating from LXD to Incus*. https://linuxcontainers.org/incus/docs/main/howto/server_migrate_lxd/
[Return to citation](#apa-incus-migration-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial conversion to web-ready APA 7 markdown: replaced YAML front matter with title and version block; added Abstract, Sources and Acknowledgements, and Changelog sections; numbered body sections and set headings to sentence case; rebuilt all citations and references on the CAP workflow; reassigned duplicate APA year-letters so each distinct work has a unique letter; moved Resources before References with topic-grouped anchor links; corrected three non-resolving `/incus/docs` reference URLs to their working documentation pages. |

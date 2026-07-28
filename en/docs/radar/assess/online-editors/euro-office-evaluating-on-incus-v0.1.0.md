# Evaluating Euro-Office on Incus

Version: 0.1.0
Status: Draft
Style Guide: web-ready-unrendered-markdown-using-apa-7.md v0.2.1

## Abstract

This document describes a procedure and rationale for evaluating Euro-Office, a web-based collaborative document editor, in an Incus-based test bed rather than a Docker container. Building and running the software inside an Incus system container, or virtual machine, provides a realistic self-hosted server environment, makes it straightforward to inspect what is installed and built, and supports snapshots, resource limits, and direct network exposure. The approach is intended for a hands-on evaluation of document compatibility, collaborative editing, and resource behaviour, and it matches the way Euro-Office is designed to be deployed, as an integration component behind a platform such as Nextcloud.

## Sources and Acknowledgements

The factual content of this document is derived from the official Euro-Office project repositories and from contemporary technical reporting on the project's launch and its licensing dispute. All such sources are cited in the body using the Citation Anchor Pair workflow and are listed in full in the References section. Command syntax was checked against the current Incus documentation.

## 1. Background

**Euro-Office** is a community-developed fork of the ONLYOFFICE Document Server, an AGPL-3.0 licensed codebase that the project is reviewing and cleaning up with the stated goal of making it easier to build and contribute to (<a name="apa-euro-office-repo-citation"></a>[Euro-Office, 2026](#apa-euro-office-repo-reference)). It is not intended for stand-alone use. Instead, it is an integration component that embeds in a platform handling document storage, navigation, permissions, and sharing, such as Nextcloud, XWiki, OpenProject, or Proton. It supports DOCX, PPTX, and XLSX alongside the Open Document Formats ODT, ODS, and ODP, and provides real-time collaborative editing and PDF editing.

The project was launched in 2026 by a consortium of European organisations, including IONOS, Nextcloud, Proton, XWiki, and OpenProject, as part of a broader push for digital sovereignty. Shortly after the launch, ONLYOFFICE's developer, Ascensio System SIA, alleged that the fork violated the AGPLv3 by removing the additional Section 7 terms covering branding, and ended its long-standing integration partnership with Nextcloud (<a name="apa-thinkfree-fork-citation"></a>[Thinkfree, 2026](#apa-thinkfree-fork-reference)). The dispute remains unresolved.

## 2. Why an Incus test bed rather than Docker

A Docker container is optimised for running a single packaged process and hides much of the surrounding system. For evaluating an early fork whose build tooling is in transition, and whose provenance is contested, a fuller server environment is more useful because it lets the evaluator see exactly what is installed and built. An Incus system container runs a complete Linux userspace, so the software behaves as it would on a real server.

Running the evaluation on Incus offers several practical advantages:

- Fully open source, with no proprietary daemon or licensing constraints.
- systemd and other init-managed services work without special workarounds.
- Snapshots and rollbacks make it cheap to retry a failed build from a clean state.
- Per-instance CPU, memory, and disk limits are simple to apply.
- Service ports can be exposed directly to the local network through a proxy device.

## 3. Setting up the Incus test bed

### 3.1 Create the container

Launch an Ubuntu 24.04 container to host the build:

```bash
incus launch images:ubuntu/24.04 euro-office
```

### 3.2 Open a shell in the container

Open an interactive shell to install dependencies and build from source:

```bash
incus exec euro-office -- bash
```

### 3.3 Expose the document server to the network

Add a proxy device to forward a host port to the service running inside the container. The following maps host port 8080 to the container's port 80:

```bash
incus config device add euro-office http proxy \
  listen=tcp:0.0.0.0:8080 \
  connect=tcp:127.0.0.1:80
```

Adjust the `connect` port to whatever port the document server listens on inside the container.

## 4. Evaluation procedure

Because Euro-Office is an integration component rather than a stand-alone application, a realistic evaluation pairs it with a host platform. The following procedure builds Euro-Office in one container and connects it to Nextcloud in another:

1. Launch an Incus container for Euro-Office, as shown in section 3.
2. Build Euro-Office from source inside the container, following the DocumentServer repository instructions at https://github.com/Euro-Office/DocumentServer.
3. Install Nextcloud in a second container to act as the integrating platform (<a name="apa-nextcloud-citation"></a>[Nextcloud GmbH, n.d.](#apa-nextcloud-reference)).
4. Connect Euro-Office to Nextcloud as its document editing back end.
5. Exercise the test matrix below.

The evaluation should cover:

- DOCX, XLSX, and PPTX compatibility
- Open Document Format support (ODT, ODS, ODP)
- Real-time collaborative editing
- PDF viewing and editing
- Accessibility features
- Resource consumption under load

This produces a realistic self-hosted environment without Docker, and closely matches how Euro-Office is intended to be used.

## 5. Alternative test beds

### 5.1 LXD

For environments already invested in LXD, the experience is nearly identical to Incus:

```bash
lxc launch ubuntu:24.04 euro-office
lxc exec euro-office -- bash
```

### 5.2 Podman

If the project only publishes container images, Podman is a daemonless, fully open-source, and largely Docker-compatible runtime. Many Docker-only projects run under it with little or no modification:

```bash
podman run -d --name euro-office -p 8080:80 "$EURO_OFFICE_IMAGE"
```

Set `EURO_OFFICE_IMAGE` to the container image reference published by the project.

### 5.3 Virtual machine isolation

For the cleanest isolation, run the evaluation in a virtual machine instead of a container. Incus can provision one directly using QEMU and KVM, applying the recommended resources at launch:

```bash
incus launch images:ubuntu/24.04 euro-office --vm \
  -c limits.cpu=4 \
  -c limits.memory=8GiB \
  -d root,size=50GiB
```

A stand-alone KVM or libvirt virtual machine running Ubuntu 24.04 with the same resources is an equivalent alternative.

## 6. Before investing time

Euro-Office is an early fork, and its build tooling is still in transition. The project states that the build system inherited from ONLYOFFICE is still being reviewed and cleaned up, so source builds may be rough at this stage; consult the DocumentServer repository for current build and run instructions before committing significant effort (<a name="apa-euro-office-repo-citation-2"></a>[Euro-Office, 2026](#apa-euro-office-repo-reference)). Be aware also that the unresolved AGPLv3 dispute noted in section 1 may affect packaging and distribution channels during the evaluation period.

## Resources

### Project sources

- [Euro-Office (GitHub)](#apa-euro-office-repo-reference)

### Integration platforms

- [Nextcloud](#apa-nextcloud-reference)

### Background and analysis

- [Euro-Office forks OnlyOffice: What it means for users](#apa-thinkfree-fork-reference)

## References

<a name="apa-euro-office-repo-reference"></a>Euro-Office. (2026). *Euro-Office* [GitHub organisation]. https://github.com/Euro-Office
[Return to citation](#apa-euro-office-repo-citation)

<a name="apa-nextcloud-reference"></a>Nextcloud GmbH. (n.d.). *Nextcloud*. https://nextcloud.com
[Return to citation](#apa-nextcloud-citation)

<a name="apa-thinkfree-fork-reference"></a>Thinkfree. (2026, April 6). *Euro-Office forks OnlyOffice: What it means for users*. https://thinkfree.com/en/blog/euro-office-onlyoffice-fork/
[Return to citation](#apa-thinkfree-fork-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial draft, authored to the web-ready APA 7 markdown spec: built from supplied evaluation notes and verified against the Euro-Office project repositories and Incus documentation; replaced the non-standard `incus shell` command with `incus exec ... -- bash`; replaced the `podman run ...` placeholder with a named-variable template; stripped tracking parameters from all source URLs. |

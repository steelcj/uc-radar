---
dc:title: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "collabora-online"
  - "collaborative-document-editor"
  - "libreoffice"
  - "self-hosted"
  - "document-server"
dc:description: "Radar entry assessing Collabora Online, a LibreOffice-based, MPL-2.0 self-hosted web office suite for real-time collaborative document editing."
dc:publisher: "<calculated>"
dc:date: "<calculated>"
dc:modified: "<calculated>"
dc:type: "<calculated>"
dc:format: "<calculated>"
dc:language: "<calculated>"
dc:language_bcp47: "<calculated>"
dc:source: "<calculated>"
dc:relation: "<calculated>"
dc:identifier: "<calculated>"
dc:rights: "<calculated>"
---

# collabora-online--collaborative-document-editor

## What it is

Collabora Online, abbreviated COOL, is a web-based, real-time collaborative office suite built on LibreOffice technology. Documents remain on the server and the browser receives rendered tiles rather than the document itself, and it integrates with host platforms such as Nextcloud, ownCloud, and Seafile through a WOPI-compatible API. The free community build is the Collabora Online Development Edition (CODE); Collabora also distributes supported, branded binaries and sells enterprise support.

## Why interesting

It is the most mature self-hosted alternative to the ONLYOFFICE Document Server, and is the editor behind Nextcloud Office. Its source is fully open under MPL-2.0 with no user or feature limits on the community edition, it is ODF-native (relevant for open-standard and sovereignty requirements), and it is markedly lighter to operate than the ONLYOFFICE-derived stack, running in a single container in roughly 1 GB of RAM. In the context of the Euro-Office fork, it is the primary comparator for any self-hosted editing decision.

## Concerns

- Microsoft format fidelity is good but not pixel-perfect; complex DOCX, XLSX, and PPTX files may render less faithfully than on the OOXML-native ONLYOFFICE lineage.
- Licensing nuance: the source code is MPL-2.0, but Collabora's branded binary builds add proprietary trademark conditions on executable forms, and long-term support is a commercial offering, so self-builders maintain CODE themselves.
- Governance friction: The Document Foundation's 2026 revival of LibreOffice Online has created a rift with Collabora; this is worth watching for effects on the shared LibreOffice-based ecosystem.
- Real-time collaboration is reported to lag relative to ONLYOFFICE under some conditions (not measured here).
- The C++ codebase and tile-rendering architecture still consume meaningful resources under concurrent load, although less than the ONLYOFFICE stack.

## Security assessment

Applies. Collabora Online runs code and moves document content over the network, so a security assessment is in scope.

**Network behaviour:**
- [ ] Outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent
- [ ] Does have any affect on networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via Network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

**Assessment method:**
Not yet performed. To be completed under network monitoring in an Incus test bed while editing test documents. Architecturally, Collabora's design is privacy-favourable: documents remain server-side and only rendered tiles are sent to the browser. Whether the community build performs update checks, telemetry, or other outbound contact is unverified and should be confirmed under monitoring.

**Assessment status:** Pending — not yet assessed.

## Relationship to project (SAT as an example)

Collabora Online is a collaborative editing component rather than an archiving primitive, so its fit with SAT is partial. If adopted, it would sit in the content-tools area (document creation and editing) and, by way of a host platform, the web-UI layer. Because it is LibreOffice-based, its ODF fidelity and its headless-conversion lineage (the LibreOffice `soffice --headless` machinery) may be more directly useful to document-conversion steps in the pipeline than an OOXML-first editor would be. It remains more naturally part of a Nextcloud-based collaboration layer than of the core archive.

## Status notes

Ring: assess. Collabora Online is mature and production-ready, but it is on the radar as a candidate to be evaluated against Euro-Office rather than already adopted.

- Last reviewed: 2026-06-06
- In assess: it would move to adopt if a hands-on evaluation confirms acceptable Microsoft-format fidelity and resource use for the intended workload and the CODE build-and-maintenance burden is acceptable. It would move to hold if the TDF and Collabora rift materially threatened the community edition's future, or if format fidelity proved inadequate for the documents in scope.
- In adopt: not applicable while in assess.
- In hold: not applicable while in assess.

## Links

- https://www.collaboraonline.com (project site)
- https://github.com/CollaboraOnline/online (issue tracker; active development on Gerrit at https://gerrit.collaboraoffice.com)
- https://collaboraonline.github.io (build instructions and documentation)

## License (for this document)

TODO

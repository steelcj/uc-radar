---
dc:title: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "euro-office"
  - "collaborative-document-editor"
  - "onlyoffice-fork"
  - "self-hosted"
  - "document-server"
dc:description: "Radar entry assessing Euro-Office, a European, AGPL-licensed fork of the ONLYOFFICE Document Server for self-hosted, real-time collaborative document editing."
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

# euro-office--collaborative-document-editor

## What it is

Euro-Office is a web-based, real-time collaborative document editor forked from the ONLYOFFICE Document Server. It is an integration component designed to be embedded behind a host platform, such as Nextcloud, XWiki, OpenProject, or Proton, rather than run stand-alone, and it edits DOCX, PPTX, and XLSX as well as the Open Document Formats ODT, ODS, and ODP.

## Why interesting

It offers a fully open-source, European-governed alternative to the ONLYOFFICE Document Server for self-hosted collaborative editing, with strong Microsoft Office and ODF compatibility and a stated focus on digital sovereignty and on making the inherited code base buildable and contributable. In a self-hosted documentation or archiving context, it is relevant wherever an open online editing or viewing component is wanted alongside a platform such as Nextcloud, and its document-format handling and converter lineage may be of interest to document-pipeline work.

## Concerns

- Early-stage fork, effectively a tech preview. The build system inherited from ONLYOFFICE is still being reviewed and cleaned up, and build instructions may be unreliable at this stage.
- The integration layer is still maturing. Promised `deb` and `rpm` packages were not available at launch, and some components and links are reportedly incomplete.
- Unresolved AGPLv3 licensing dispute. ONLYOFFICE's developer, Ascensio System SIA, alleges Section 7 violations and has ended its Nextcloud partnership, so the legal position and long-term availability are uncertain.
- Heavyweight. It is a full document server (C++ core plus multiple services), not a lightweight library, with a correspondingly large footprint and dependency surface.
- Useful only in combination with a host platform. It is not a stand-alone tool.
- AGPL-3.0 copyleft imposes source-availability obligations on networked deployments and derivatives, which is relevant if it is embedded in or distributed with other software.

## Security assessment

Applies. Euro-Office runs code and processes document content over the network, so a security assessment is in scope.

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
Not yet performed. To be completed during the Incus-based evaluation (see `euro-office-evaluating-on-incus-v0.1.0.md`) by running a network monitor on the Incus container while editing test documents. Architecturally, as a document server it necessarily exchanges document content with its host platform over the network; whether it performs update checks, telemetry, or any other outbound contact is unverified and should be confirmed under monitoring, paying attention to any phone-home behaviour inherited from its ONLYOFFICE lineage.

**Assessment status:** Pending — not yet assessed.

## Relationship to project (SAT as an example)

Euro-Office is a collaborative editing component rather than an archiving primitive, so its fit with SAT is partial. If adopted, it would sit in the content-tools area (document creation and editing) and, by way of a host platform, the web-UI layer; it is not an engine, driver, or archive tool. The parts most relevant to the document pipeline are its DOCX, ODF, and PDF handling and its document-converter lineage, which bear on format fidelity and compatibility. It is more naturally part of a Nextcloud-based collaboration layer than of the core archive.

## Status notes

Ring: assess. Euro-Office is a recently launched, fast-moving fork with an unresolved licensing question, so it warrants observation rather than adoption.

- Last reviewed: 2026-06-06
- In assess: it would move to adopt if the licensing dispute is resolved or clarified favourably, the build system stabilises with reliable instructions and packages, and a hands-on evaluation confirms acceptable format fidelity, collaboration behaviour, and resource use. It would move to hold if an adverse legal outcome forced withdrawal or relicensing, if the project stalled, or if builds remained unreliable.
- In adopt: not applicable while in assess.
- In hold: not applicable while in assess.

## Links

- https://github.com/Euro-Office (project organisation)
- https://github.com/Euro-Office/DocumentServer (main document server repository and build instructions)
- `euro-office-evaluating-on-incus-v0.1.0.md` (companion evaluation procedure)

## License (for this document)

TODO

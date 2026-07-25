---
dc:title: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "cryptpad"
  - "encrypted-collaboration-suite"
  - "end-to-end-encryption"
  - "self-hosted"
  - "data-sovereignty"
dc:description: "Radar entry assessing CryptPad, an end-to-end-encrypted, AGPL-licensed collaborative office suite for privacy-first self-hosted document editing."
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

# cryptpad--encrypted-collaboration-suite

## What it is

CryptPad is an end-to-end-encrypted, open-source collaborative office suite. Encryption is carried out in the browser, so the server stores only encrypted data and, if the operator does not alter the served code, cannot read document content; this zero-knowledge design is built on its ChainPad real-time engine. It provides rich text, spreadsheet, presentation, code, and markdown editors plus whiteboard, kanban, and form tools and an encrypted file store (CryptDrive), and it can be used anonymously or with a free account. Its document, spreadsheet, and presentation editors are based on an ONLYOFFICE editor wrapped in encryption. CryptPad is developed by XWiki SAS in France.

## Why interesting

It is the strongest option when confidentiality and data sovereignty are the priority. Even a compromised or hostile server operator cannot read content, which is a materially different threat model from Collabora, ONLYOFFICE, or Euro-Office, where the server processes documents in the clear. Self-hosting is free under AGPL-3.0 and gives full control of the data, so for sensitive documents in a self-hosted collaboration or archiving context it is the privacy benchmark to compare against.

## Concerns

- Threat-model trade-off: the encryption code is still served by the host, so a malicious or compromised server could deliver tampered client code (an active attack). The project is explicit that CryptPad is "private, not anonymous," and code-integrity verification is an area of ongoing work.
- It is not an OOXML-fidelity engine in the way ONLYOFFICE and Euro-Office are. Its editors share the ONLYOFFICE lineage, but heavy interoperability with complex Microsoft files is not the primary design goal, and import and export round-tripping should be tested.
- Encryption constrains some server-side capabilities by design, such as server-side search, indexing, and processing.
- AGPL-3.0 with a contributor agreement (the Commons Management Agreement) that supports dual-licensing; relevant for contributors and for any redistribution.
- Encryption and retained history add performance and storage overhead.

## Security assessment

Applies. CryptPad runs code and moves document content over the network, so a security assessment is in scope.

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
Not yet performed. To be completed under network monitoring in an Incus test bed. Architecturally, content is end-to-end-encrypted in the browser before it is sent, and the server holds ciphertext; the critical trust assumption is that the server delivers honest client code, which should be verified (for example, by checking served code and subresource integrity). Whether the instance makes other outbound contact, update checks, or telemetry is unverified and should be confirmed under monitoring, with attention to whether anything ever leaves in cleartext.

**Assessment status:** Pending — not yet assessed.

## Relationship to project (SAT as an example)

CryptPad is a collaborative editing component rather than an archiving primitive, so its fit with SAT is partial. Its distinctive value would be as the confidential-collaboration option for cases where document content must not be readable by the server, which is a different role from the OOXML-fidelity editors. If adopted it would sit in the content-tools area and, by way of its web interface, the web-UI layer. Its export and portability behaviour and its format handling are the parts most relevant to the document pipeline.

## Status notes

Ring: assess. CryptPad is mature and on the radar as the privacy-first comparator, evaluated for a specific confidential-collaboration need rather than as a general office editor.

- Last reviewed: 2026-06-06
- In assess: it would move to adopt if a confidential-collaboration need is confirmed and an evaluation shows acceptable format interoperability, performance, and a satisfactory review of the code-integrity and trust model. It would move to hold if the threat model does not match the need — for example, if server-readable editing is acceptable and a lighter tool suffices — or if interoperability proves inadequate.
- In adopt: not applicable while in assess.
- In hold: not applicable while in assess.

## Links

- https://cryptpad.org (project site)
- https://github.com/cryptpad/cryptpad (source, AGPL-3.0; includes Dockerfile and docker-compose)
- https://docs.cryptpad.org (installation and administration guides)

## License (for this document)

TODO

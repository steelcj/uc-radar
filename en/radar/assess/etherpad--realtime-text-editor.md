---
dc:title: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "etherpad"
  - "realtime-text-editor"
  - "collaborative-editing"
  - "self-hosted"
  - "node-js"
dc:description: "Radar entry assessing Etherpad, a lightweight, Apache-licensed, self-hosted real-time collaborative text editor."
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

# etherpad--realtime-text-editor

## What it is

Etherpad is a lightweight, open-source, real-time collaborative editor for a single shared document, called a pad. It is maintained by the Etherpad Foundation and community and written in Node.js. It focuses on plain and lightly formatted text with per-author colours, a chat sidebar, and full data export, and it has an extensive plugin ecosystem (headings, markdown, comments, and more) and an HTTP API for embedding in other applications. It is self-hosted and runs under the operator's control.

## Why interesting

It is the simplest and most battle-tested option for real-time collaborative text and note-taking, scalable to many simultaneous users, with strong data portability (full-fidelity export, which helps with GDPR) and easy embedding through its API. Where the need is collaborative drafting or notes rather than office-format documents, Etherpad is far lighter to run than any office suite and slots easily into a larger toolchain.

## Concerns

- Not an office suite. It edits text pads, not DOCX, XLSX, PPTX, or ODF, so it is not a substitute for Euro-Office, Collabora, or ONLYOFFICE for office-format work.
- Rich formatting is limited and is delivered largely through third-party plugins of varying quality and maintenance.
- No end-to-end encryption; the server stores pad content in cleartext, in contrast to CryptPad.
- Maintenance capacity: the project has periodically appealed for additional maintainers and funding, so current activity and maintainership are worth checking, even though releases continue.
- The repository has moved from `ether/etherpad-lite` to `ether/etherpad`; ensure any tooling references the current location.

## Security assessment

Applies. Etherpad runs code and moves document content over the network, so a security assessment is in scope.

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
Not yet performed. To be completed under network monitoring in an Incus test bed. Note that Etherpad provides no end-to-end encryption and stores pad content server-side in cleartext, exposes an admin panel, and typically delegates authentication to plugins (for example, OpenID Connect). Whether a given instance or its plugins make outbound contact, update checks, or telemetry is unverified and should be confirmed under monitoring, paying particular attention to any third-party plugins enabled.

**Assessment status:** Pending — not yet assessed.

## Relationship to project (SAT as an example)

Etherpad has a narrow fit. It could serve as a collaborative drafting or notes surface feeding the document pipeline, sitting in the content-tools area and, through its web interface, the web-UI layer, but it does not handle archive document formats and is not an archive primitive. Its clean HTTP API and full export make it straightforward to wire into a toolchain if a lightweight collaborative-text capability is wanted.

## Status notes

Ring: assess, low priority given the narrow fit. Etherpad is mature and stable, but it addresses collaborative text rather than office-format documents, so it is on the radar mainly to be ruled in or out for a specific need.

- Last reviewed: 2026-06-06
- In assess: it would move to adopt only if a specific need for lightweight real-time collaborative text emerges that the office suites do not serve well. For office-document use cases it is more likely to move to hold than to adopt, because it is the wrong category for OOXML and ODF work. It would move to hold if no distinct need for it is identified.
- In adopt: not applicable while in assess.
- In hold: not applicable while in assess.

## Links

- https://etherpad.org (project site)
- https://github.com/ether/etherpad (source, Apache-2.0)
- https://github.com/ether/etherpad/wiki (tutorials and how-to guides)

## License (for this document)

TODO

---
dc:title: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "libreoffice-online"
  - "collaborative-document-editor"
  - "libreoffice"
  - "self-hosted"
  - "document-foundation"
dc:description: "Radar entry assessing LibreOffice Online (LOOL), the Document Foundation's web office project revived from its attic in February 2026 for community-led self-hosted collaborative editing."
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

# libreoffice-online--collaborative-document-editor

## What it is

LibreOffice Online, abbreviated LOOL, is The Document Foundation's web-based version of the LibreOffice suite, intended for self-hosted deployment. Development was frozen and moved to the foundation's "attic" around 2020 to 2022; in February 2026 the Board voted to revive it as a community-led project, and the repository is being reopened for contributions.

## Why interesting

It promises a fully community-governed, end-to-end open-source online office suite under Document Foundation stewardship, without the commercial-binary or trademark constraints attached to the Collabora and ONLYOFFICE offerings. For organisations where licensing auditability and vendor neutrality are decisive — the same digital-sovereignty drivers behind the Euro-Office fork — a foundation-run LOOL is strategically significant, even though Collabora Online already provides a productised LibreOffice-based option.

## Concerns

- Not yet usable. As of the February 2026 revival it amounts to a decision to resume work plus a repository being reopened, with the foundation itself warning about the state of the code until it is deemed safe and usable. There is no deployable release to evaluate.
- Maturity and timeline are unknown. The project was dormant for several years, and the work is only just beginning.
- The revival has caused governance friction with Collabora, which is the productised continuation of the same technology; this could affect contribution flow and code sharing between the two LibreOffice-based efforts.
- It overlaps heavily with Collabora Online. Today the practical differentiation is licensing and governance rather than capability, since there is no shipping LOOL build to compare on features.

## Security assessment

Applies in principle, but not assessable at this stage. There is no deployable build to test, because the project is at the revival and repository-reopening stage with no usable release.

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
Cannot be performed yet — no buildable or packaged release exists. Re-assess once a deployable version is available, using network monitoring in an Incus test bed.

**Assessment status:** Not assessable yet — no deployable release.

## Relationship to project (SAT as an example)

LibreOffice Online is, today, a watch item rather than something integrable. If and when it produces a deployable release, it would sit in the same place as the other editors: the content-tools area (document creation and editing) and, by way of a host platform, the web-UI layer. Being LibreOffice-based, its eventual ODF handling and conversion lineage would be the parts most relevant to document-format work in the pipeline. It is not currently a candidate for any tier.

## Status notes

Ring: assess, effectively a watch. The project has only just been revived and produces nothing deployable, so it warrants monitoring rather than evaluation.

- Last reviewed: 2026-06-06
- In assess: it would move to adopt only after it produces a deployable, buildable release with host-platform integration and passes a hands-on evaluation, which is likely many months away at the earliest. It would move to hold if the revival stalled again, or if the foundation and Collabora rift fragmented the LibreOffice-online effort to the point that Collabora Online remained the only practical option.
- In adopt: not applicable while in assess.
- In hold: not applicable while in assess.

## Links

- https://blog.documentfoundation.org/blog/2026/02/24/libreoffice-online-a-fresh-start/ (revival announcement)
- https://www.libreoffice.org (LibreOffice and The Document Foundation)
- Note: the LibreOffice Online repository is being reopened at The Document Foundation; consult the announcement for its current location and state.

## License (for this document)

TODO

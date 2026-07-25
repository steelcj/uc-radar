---
​---
dc:title: "<calculated>"
dcterms:version: "<calculated>"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "documentation"
  - "template"
dc:description: "Markdown template for creating new radar entries."
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

# Radar Entry Template

## Description

A raw markdown template that is used to create new Radar entires.

* By default new entires are likly to go in the "asses" directory unless they document an item that has been adopted informally and is currently relied upon in dev, stage and/or prod.

## Notes

## The Radar Entry Template

You can copy the contents and paste into a new markdown document which will become your Radar Entry

`````markdown
---
dc:title: ""
dcterms:version: "0.1.1"
dc:creator: ""
dc:contributor: ""
dc:subject:
  - "radar"
  - ""
dc:description: ""
dc:publisher: ""
dc:date: ""
dc:modified: ""
dc:type: ""
dc:format: ""
dc:language: ""
dc:language_bcp47: ""
dc:source: ""
dc:relation: ""
dc:identifier: ""
dc:rights: ""
---

# <name--type>

So <name--type> above becomes something like

````markdown
:hugo--static-site-generator
````

## What it is

A short neutral description. One or two sentences. Not a sales pitch — just what it is.

## Why interesting

What problem it solves, and in what context

Example:

Would be useful in confirming that a markdown document conforms to the a standard. Could be used in the SAT/Archive/Document pipeline(s) in order to ensure for a desired state of Markdown content.

## Concerns

Honest assessment of risks, limitations, maturity, licence, or fit issues. If there are no concerns worth noting, say so explicitly rather than leaving this section empty.

## Security assessment

Applies to tools and apps that run code or move data. For protocols and techniques, mark the status N/A and give the reason in one line.

Check applicable items. Add any missing options that this Radar item affects

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
How the above was tested — network monitor used, platform, version, date.

**Assessment status:** 


## Relationship to project (SAT as an example)

Where this sits in (SAT), or where it would graduate to on adopt.
Examples:

- a tier (engine, driver, content tools, archive tools, API, web UI, legal/compliance, dev toolchain)
- a docs area (language/, architecture/, guides/, specifications/)

Note: a technique or approach may not map to a single tier; name the area instead.

## Status notes

Why the entry is in its current ring (assess, adopt, hold). Nothing moves to adopt or hold without a reason here.

- Last reviewed: 
- In assess: what would move it to adopt, and what would move it to hold.
- In adopt: the destination it graduates to in en/docs/. The entry leaves the radar once migrated.
- In hold: what would need to change for it to be reconsidered.

## Links

- <source / repository>

## License (for this document)

TODO
`````
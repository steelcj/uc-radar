---
dc:title: "mdformat — CommonMark compliant Markdown formatter"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "markdown"
  - "formatter"
  - "commonmark"
  - "dev-toolchain"
dc:description: "Radar entry assessing mdformat, a CommonMark-compliant Markdown formatter, for the SAT document pipeline."
dc:publisher: "<calculated>"
dc:date: "<calculated>"
dc:modified: "<calculated>"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-GB"
dc:source: "<calculated>"
dc:relation: "<calculated>"
dc:identifier: "<calculated>"
dc:rights: "<calculated>"
---

# mdformat--commonmark-compliant-markdown-formatter

## What it is

mdformat is an opinionated, CommonMark-compliant Markdown formatter, usable both as a Unix-style command-line tool and as a Python library. It parses a document with markdown-it-py and re-renders it to a canonical form, with a semantic-preservation guarantee that the reformatting never changes the rendered HTML. It is MIT-licensed and actively maintained (version 1.0.0, October 2025).

## Why interesting

It gives the SAT document pipeline a deterministic normalise step: run it to bring any Markdown source into one consistent shape, or run `mdformat --check` to assert that a file is already canonical without modifying it. The check form suits an archive or CI gate that must confirm a desired state rather than mutate content. It pairs naturally with a lightweight project validator — mdformat owns the mechanical formatting tier (whitespace, blank lines, heading style, list markers, line endings), leaving rules it cannot safely infer to a separate check. Being pure Python, it keeps the document toolchain on a single runtime.

## Concerns

- Opinionated output. mdformat rewrites to its own canonical style and will backslash-escape characters to protect the rendered HTML. Angle-bracket placeholders such as `<calculated>` and `<source / repository>` used in these radar templates would be escaped, so mdformat must not be run blindly over template files. Run on a copy and diff before trusting it on hand-authored content.
- CommonMark baseline only. GFM constructs (tables, task lists, strikethrough) and other dialects require plugins; without them mdformat may reformat unfamiliar syntax in unwanted ways.
- It is not a linter. It enforces a canonical form but emits no per-rule findings, and it will not enforce non-CommonMark house rules such as "no horizontal rules in content" or "every fenced block carries a language identifier". Those remain the job of a separate validator.
- Pin the version. Formatter output can shift between releases; pin mdformat and any plugins so the canonical form stays stable across the pipeline.

## Security assessment

mdformat is a local, offline formatter. It reads and writes files on disk and performs no network activity in normal use.

**Network behaviour:**
- [ ] Outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent
- [ ] Does have any affect on networking or items created that will be transferred over a network
- [x] Helps ensure that documents passed via Network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

Note: mdformat writes only the files it is pointed at, in place; with `--check` it writes nothing.

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

**Assessment method:**
Review of the published documentation and the project's source and dependency (markdown-it-py), not a live packet capture. Version reviewed: 1.0.0 (released 16 October 2025). Date: 31 May 2026. A brief network capture on the target platform is recommended to confirm the no-network finding before the tool gates production content.

**Assessment status:** Provisionally clear. Low security surface — local, offline, no telemetry by design. Confirm with a packet capture on first install per the method above.

## Relationship to project (SAT as an example)

Dev toolchain — a content tool in the SAT/Archive/Document pipeline that enforces a desired state for Markdown source. It is not tied to a single engine tier; the area is dev toolchain / content tools rather than a tier. On graduation its guidance moves into `en/docs/`, most likely under `guides/` for the normalise and check workflow, with the canonical-form rules themselves referenced from the documentation style guide.

## Status notes

Adopted. mdformat is the chosen normaliser for the Markdown content pipeline.

- Last reviewed: 2026-05-31
- Decision rationale: Evaluated against the obvious alternatives. pymdtools was rejected as inactive (around 57 weekly downloads, single star, defunct CI) and a poor fit — it imposes its own style and emits no findings. markdownlint (`markdownlint-cli2 --fix`) is a strong linter but is Node, which adds a second runtime to an otherwise Python toolchain. Goldmark is a Go parser library rather than a turnkey formatter and would need Go glue to normalise source. mdformat is maintained, MIT, pure Python, offers a non-mutating `--check` mode, and guarantees its formatting never alters rendered HTML — the right fit for the normalise slot.
- Pairing: adopted together with a lightweight project validator that enforces the house rules mdformat does not (no horizontal rules in content; a language identifier on every fenced block). mdformat normalises; the validator judges the residual.
- Destination on graduation: `en/docs/guides/` for pipeline usage, plus a reference from the documentation style guide. The entry leaves the radar once that documentation exists.
- Conditions to revisit: a shift in dialect requirements (heavy GFM or MkDocs usage) that mdformat cannot cover even with plugins, or a sustained lapse in upstream maintenance.

## Links

- https://pypi.org/project/mdformat/
- https://github.com/hukkin/mdformat
- https://mdformat.readthedocs.io/

## License (for this document)

TODO — set to the SAT documentation licence. This concerns the licence of this radar entry; mdformat itself is MIT-licensed, recorded under "What it is".

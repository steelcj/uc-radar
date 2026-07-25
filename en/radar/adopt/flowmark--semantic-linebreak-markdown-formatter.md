---
dc:title: "flowmark — diff-friendly Markdown auto-formatter with semantic line breaks"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "markdown"
  - "formatter"
  - "semantic-line-breaks"
  - "dev-toolchain"
dc:description: "Radar entry assessing flowmark, a Python Markdown auto-formatter with semantic line breaks, for the SAT archive remediation pass."
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

# flowmark--semantic-linebreak-markdown-formatter

## What it is

flowmark is a Python Markdown auto-formatter (MIT-licensed, by jlevy) aimed at better LLM workflows and clean git diffs. It supports both CommonMark and GitHub-Flavoured Markdown through the Marko parser, and is deliberately conservative so it is safe to run automatically on save or at any stage of a document pipeline. It can be run as a CLI, as an editor format-on-save command, or as a Python library. The current release is v0.6.5 (26 February 2026) and it is pre-1.0. A separately maintained Rust port (flowmark-rs) provides a single native binary if a non-Python runtime is ever preferred.

## Why interesting

Several features line up closely with SAT's need to remediate AI-generated, git-tracked content:

- The `--cleanups` flag performs safe cleanups including unbolding headings, so it handles the "things that do not belong in headings" problem natively, without a custom plugin.
- `--auto` is a single convenience flag equivalent to `--inplace --nobackup --semantic --cleanups --smartquotes --ellipses` — a one-shot remediation pass that does everything sensible at once.
- Semantic line breaks (`--semantic`) break lines at sentence boundaries using fast regex-based splitting, so paragraphs do not reflow for small edits. For largely AI-generated content committed to git, this markedly reduces diff noise and merge conflicts — a real quality-of-life gain when iterating with an LLM.
- YAML frontmatter is always preserved exactly (never normalised), and it handles GFM tables, footnotes, template tags (Markdoc, Jinja, Nunjucks), and inline HTML comments — directly relevant to the rich Dublin Core frontmatter on radar entries.
- Python-native with only four dependencies (marko, pathspec, regex, strif), so it fits the SAT toolchain without pulling in Node.
- It can install itself as a Claude Code skill (`flowmark --install-skill`), which dovetails with the existing claude-env skills workflow.

## Concerns

- Pre-1.0 (v0.6.5). Development is active (32 releases, 234 commits), but defaults and the API may still shift; pin the version and re-check behaviour on upgrade.
- Content-mutating typography. `--auto` enables `--smartquotes` and `--ellipses`, which replace straight quotes and `...` with Unicode characters. That is professional typography, but it changes content bytes and moves away from a lowest-common-denominator ASCII form. Decide whether the archive wants that before enabling it. Both transforms are conservative (they skip code blocks) and can be disabled individually.
- Opinionated source restructuring. Semantic line breaks change how the Markdown source is laid out across lines. The rendered output is unchanged, but it is a one-way stylistic commitment across the whole archive and a large visual change to source diffs the first time it is applied.
- In-place without backup under `--auto`. `--auto` includes `--nobackup`, so it overwrites files relying on git as the safety net. For a remediation pass, run on a clean working tree, or drop `--nobackup` to keep the `.orig` backups.
- Placeholder preservation unverified. mdformat escapes angle-bracket placeholders such as `<calculated>` in template files; whether flowmark (Marko-based, "just works" with inline HTML and template tags) preserves them must be confirmed in the sandbox before it is run over radar templates.

## Security assessment

flowmark is a local, offline formatter. It reads and writes files on disk and performs no network activity in normal use. It respects `.gitignore` and `.flowmarkignore` and skips files over 1 MiB by default.

**Network behaviour:**
- [ ] Outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent
- [ ] Does have any affect on networking or items created that will be transferred over a network
- [x] Helps ensure that documents passed via Network are clean and compliant

**File system behaviour:**
- [x] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

Note: the only file it writes besides the target is a sibling `.orig` backup when `--inplace` is used without `--nobackup`; `--auto` suppresses that backup. During directory traversal symlinks are not followed, so it does not write outside the project tree.

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

**Assessment method:**
Review of the published documentation and the project's source and dependencies (marko, pathspec, regex, strif), not a live packet capture. Version reviewed: v0.6.5 (released 26 February 2026). Date: 31 May 2026. A network capture and a sandbox run over sample problem documents are recommended to confirm both the no-network finding and the content-transformation behaviour before adoption.

**Assessment status:** Provisionally clear on security (local, offline, four dependencies). Functional fit is pending the sandbox evaluation described in the status notes.

## Relationship to project (SAT as an example)

Dev toolchain / content tools — a candidate normaliser and remediation tool for the SAT/Archive/Document pipeline. It overlaps directly with the already-adopted mdformat entry: both occupy the normalise slot. The two should not both be adopted for that slot, so this entry's evaluation is effectively a bake-off. On adoption it would graduate to `en/docs/guides/` for the remediation and normalise workflow, with the canonical-form rules referenced from the documentation style guide.

## Status notes

In assess. Strong candidate for the archive remediation pass; not yet committed.

- Last reviewed: 2026-05-31
- What would move it to adopt: a successful sandbox evaluation against a representative sample of problem documents, confirming that (a) semantic line breaks and `--cleanups` produce the desired remediation, (b) the smartquotes and ellipses behaviour is acceptable for the archive or is disabled, (c) YAML frontmatter and angle-bracket placeholders in radar templates survive untouched, and (d) the no-network finding holds under a packet capture. Because adoption would displace mdformat for this slot, promoting flowmark to adopt requires moving the mdformat entry to hold with a status note recording the bake-off result.
- What would move it to hold: sandbox results showing unwanted content mutation, frontmatter or placeholder damage, or instability attributable to its pre-1.0 status; or a decision that mdformat's stable 1.0 release and its guarantee never to alter rendered HTML outweigh flowmark's auto-applied semantic line breaks.
- Note: the closest alternative is the adopted mdformat. flowmark's own documentation positions mdformat as its nearest comparison, the main difference being that flowmark auto-applies semantic line breaks whereas mdformat only preserves manually placed ones.

## Links

- https://github.com/jlevy/flowmark
- https://pypi.org/project/flowmark/
- https://github.com/jlevy/flowmark-rs

## License (for this document)

TODO — set to the SAT documentation licence. This concerns the licence of this radar entry; flowmark itself is MIT-licensed, recorded under "What it is".

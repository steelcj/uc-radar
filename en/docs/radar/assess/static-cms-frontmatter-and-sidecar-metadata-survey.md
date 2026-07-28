# Static CMS Frontmatter and Sidecar Metadata Survey

## Purpose

This survey documents how each major static CMS handles page-level metadata,
specifically whether it supports frontmatter embedded in content files,
external sidecar metadata files, or both. Accessibility characteristics and
architectural fit with MkDocs are included for each tool.

---

## Survey Criteria

For each CMS the following questions are answered:

- What metadata mechanism does it use natively?
- What frontmatter format and key naming convention does it expect?
- Does it support per-file external sidecar metadata files?
- What are its accessibility characteristics?
- What is its architectural fit with MkDocs?
- What are the implications for SAT ingress?

---

## Decap CMS

**Overview:** Decap CMS (formerly Netlify CMS) is a Git-based headless CMS
that manages content via a web UI backed by a Git repository. It reads and
writes frontmatter directly in Markdown files.

**Metadata mechanism:** YAML, TOML, or JSON frontmatter embedded in Markdown
files. The schema for each content type is defined in a `config.yml` file.

**Format and key convention:** YAML by default, delimited by `---`. Keys are
defined in `config.yml` under `fields` for each collection. Key names are
user-defined and case-sensitive.

**Native sidecar support:** No. Decap CMS reads and writes frontmatter inline.
There is no mechanism to read or write companion sidecar files.

**Multilingual support:** Limited. Multilingual workflows require separate
collection definitions per language or a folder-based locale structure.
No native i18n routing.

**Accessibility:** Good screen reader support. The editor interface uses
standard HTML form elements. Cognitive accessibility is medium — the
interface is functional but not optimised for non-technical editors.

**Architectural fit with MkDocs:** Strong. Decap CMS works well with any
Git-backed SSG including MkDocs. The `config.yml` schema can be aligned to
MkDocs-compatible frontmatter keys.

**SAT ingress implication:** Decap CMS will overwrite frontmatter in files it
manages. SAT sidecar metadata injected as frontmatter during egress is
compatible with Decap CMS workflows, but round-trip ingress (reading edited
files back into SAT sidecars) must account for fields Decap CMS may add,
modify, or reformat.

---

## Sveltia CMS

**Overview:** Sveltia CMS is a modern, Svelte-based alternative to Decap CMS
with a focus on performance, accessibility, and native multilingual support.
It is API-compatible with Decap CMS configuration files.

**Metadata mechanism:** YAML, TOML, or JSON frontmatter embedded in Markdown
files, using the same `config.yml` schema format as Decap CMS.

**Format and key convention:** YAML by default, delimited by `---`. Keys are
defined in `config.yml` under `fields`. Because Sveltia CMS is
API-compatible with Decap CMS, existing Decap configurations work without
modification.

**Native sidecar support:** No. Sveltia CMS reads and writes frontmatter
inline, same as Decap CMS.

**Multilingual support:** Native. Sveltia CMS has first-class i18n support
with locale-aware content editing, locale switching in the editor UI, and
support for locale-specific file structures. This is a significant advantage
over Decap CMS for multilingual archives.

**Accessibility:** Strong screen reader support. The Svelte-based UI is built
with accessibility as a stated design goal. Cognitive accessibility is strong
with a cleaner, more modern interface than Decap CMS.

**Architectural fit with MkDocs:** Strong. Drop-in compatibility with Decap
CMS configuration means MkDocs-aligned schemas carry over directly.

**SAT ingress implication:** Same as Decap CMS. Round-trip ingress must
account for frontmatter modifications made by the CMS editor. The native
multilingual support makes Sveltia CMS the better choice for SAT archives
that serve multiple languages.

---

## Static CMS

**Overview:** Static CMS is a fork of Decap CMS that focuses on extensibility
and a more modern editor experience. It maintains broad compatibility with
Decap CMS configuration.

**Metadata mechanism:** YAML, TOML, or JSON frontmatter embedded in Markdown
files, using a similar schema definition approach to Decap CMS.

**Format and key convention:** YAML by default, delimited by `---`. Key names
are user-defined in the collection schema. Case-sensitive.

**Native sidecar support:** No. Static CMS reads and writes frontmatter
inline.

**Multilingual support:** Limited. Similar to Decap CMS, multilingual support
requires separate collection definitions or folder-based locale structures.

**Accessibility:** Good screen reader support. The editor interface is
improved over Decap CMS. Cognitive accessibility is medium.

**Architectural fit with MkDocs:** Strong. Configuration compatibility with
Decap CMS means MkDocs-aligned schemas carry over.

**SAT ingress implication:** Same as Decap CMS. No sidecar pathway. Round-trip
ingress must account for CMS-managed frontmatter fields.

---

## TinaCMS

**Overview:** TinaCMS is a Git-backed headless CMS with a visual editing
interface and a GraphQL content API. It differs from the Decap CMS family in
that it uses a schema defined in `tina/config.ts` (TypeScript) rather than
YAML, and generates a GraphQL API over content files.

**Metadata mechanism:** YAML frontmatter embedded in Markdown and MDX files.
The schema is defined in TypeScript using Tina's `defineConfig` API, which
generates both the editor UI and a GraphQL API.

**Format and key convention:** YAML by default, delimited by `---`. Keys are
defined in the TypeScript schema. The TypeScript schema approach enforces
type safety and is more developer-oriented than YAML-based CMS configuration.

**Native sidecar support:** No. TinaCMS reads and writes frontmatter inline.

**Multilingual support:** Good. TinaCMS supports multiple content models and
locale-based content paths. Integration with i18n frameworks is possible but
requires custom configuration.

**Accessibility:** Medium screen reader support. The visual editing interface
is more complex and JavaScript-heavy than the Decap CMS family, which can
reduce screen reader compatibility. Cognitive accessibility is strong for
non-technical editors due to the visual inline editing experience.

**Architectural fit with MkDocs:** Weak. TinaCMS is designed for React-based
SSGs (Next.js, Remix) and the visual editing experience requires a running
Tina server. It does not integrate naturally with MkDocs's Python-based build
pipeline. The GraphQL API adds infrastructure complexity that is not needed
for a docs-as-code workflow.

**SAT ingress implication:** TinaCMS is not recommended as a CMS layer for
SAT archives targeting MkDocs. The TypeScript schema and GraphQL API add
significant complexity relative to the benefit. If TinaCMS is used, SAT
ingress must consume YAML frontmatter written by TinaCMS, same as other tools.

---

## Pages CMS

**Overview:** Pages CMS is a lightweight Git-based CMS designed for simplicity
and ease of use for non-technical editors. It is configured via a `.pages.yml`
file in the repository root.

**Metadata mechanism:** YAML frontmatter embedded in Markdown files. The
schema for each content type is defined in `.pages.yml`.

**Format and key convention:** YAML by default, delimited by `---`. Keys are
defined in `.pages.yml` under `fields`. Key names are user-defined and
case-sensitive.

**Native sidecar support:** No. Pages CMS reads and writes frontmatter inline.

**Multilingual support:** Basic. Pages CMS has limited multilingual support,
requiring separate collection definitions per language.

**Accessibility:** Medium screen reader support. The interface is deliberately
simple which aids cognitive accessibility. Strong cognitive accessibility
for non-technical editors due to its minimal UI.

**Architectural fit with MkDocs:** Medium. Pages CMS works with any Git-backed
SSG but its configuration is less expressive than Decap or Sveltia CMS. It is
a good fit for simple archives but may be limiting for complex metadata
schemas.

**SAT ingress implication:** Same as other Git-based CMSs. No sidecar pathway.
Round-trip ingress must account for CMS-managed frontmatter.

---

## Keystatic

**Overview:** Keystatic is a Git-based CMS developed by Thinkmill, the team
behind Keystroke design system. It uses a TypeScript schema defined in
`keystatic.config.ts` and supports both local and cloud storage modes.

**Metadata mechanism:** YAML or JSON frontmatter embedded in Markdown files,
or structured data stored in separate YAML or JSON files depending on the
collection configuration. Keystatic supports a `document` field type for rich
content and a `fields` API for structured metadata.

**Format and key convention:** YAML by default for Markdown collections,
delimited by `---`. Key names are defined in the TypeScript schema. For data
collections, Keystatic can write standalone YAML or JSON files, which is the
closest any CMS in this survey comes to a native sidecar pattern.

**Native sidecar support:** Partial. Keystatic's data collections can store
structured data as standalone YAML files separate from content files. However
this is a parallel data store rather than a true per-file companion sidecar —
the association between a content file and its data record is by key or slug,
not by file proximity.

**Multilingual support:** Good. Keystatic supports locale-aware content with
multiple content paths and locale switching. Integration with i18n frameworks
is supported.

**Accessibility:** Good screen reader support. The interface uses standard
accessible form components from the Keystroke design system. Cognitive
accessibility is medium — the TypeScript configuration is developer-oriented
but the editor UI itself is clean.

**Architectural fit with MkDocs:** Medium. Keystatic can write YAML files
that align with MkDocs frontmatter conventions, but the TypeScript
configuration and optional cloud backend add complexity for a pure
docs-as-code workflow. Local mode works without any cloud dependency.

**SAT ingress implication:** Keystatic's data collection mode is the closest
to SAT's sidecar model of any CMS surveyed. A SAT ingress pipeline could
potentially read Keystatic data collection YAML files directly if the schema
is aligned to `.dc.yml` conventions. This warrants further investigation
before committing to a CMS choice.

---

## Contentlayer

**Overview:** Contentlayer is a content SDK that transforms local content
files (Markdown, MDX, YAML, JSON) into type-safe data accessible in
JavaScript frameworks. It is not a CMS with an editor UI but a content
processing layer.

**Metadata mechanism:** YAML frontmatter embedded in Markdown and MDX files,
validated against a schema defined in `contentlayer.config.ts`. Contentlayer
also supports standalone YAML and JSON files as content sources.

**Format and key convention:** YAML by default, delimited by `---`. Schema
defined in TypeScript using `defineDocumentType`. Keys are case-sensitive.
Contentlayer generates TypeScript types from the schema, providing editor
autocomplete and build-time validation.

**Native sidecar support:** No per-file companion sidecar support. Contentlayer
can source data from standalone YAML files as separate document types, but
does not associate them as companions to specific Markdown files
automatically.

**Multilingual support:** Not built-in. Multilingual support requires custom
schema and routing logic in the consuming framework.

**Accessibility:** Not applicable — Contentlayer has no editor UI.

**Architectural fit with MkDocs:** Weak. Contentlayer is designed for
JavaScript frameworks (Next.js, Remix, SvelteKit) and does not integrate with
MkDocs's Python build pipeline.

**SAT ingress implication:** Contentlayer is relevant only if SAT targets a
JavaScript framework as a publishing destination. It is not relevant for
MkDocs workflows.

---

## Obsidian Publish

**Overview:** Obsidian Publish is a hosted publishing service for Obsidian
vaults. It renders Markdown files with YAML frontmatter written by the
Obsidian editor.

**Metadata mechanism:** YAML frontmatter embedded in Markdown files, written
and managed by the Obsidian desktop application. Obsidian calls frontmatter
properties.

**Format and key convention:** YAML delimited by `---`. Obsidian Publish
recognises `title`, `description`, `permalink`, `publish`, `tags`, `aliases`,
`cssclass`, and `hide`. All other frontmatter keys are accessible as page
properties but are not rendered specially by Publish.

**Native sidecar support:** No. Obsidian stores all metadata inline in
frontmatter. There is no companion file mechanism.

**Multilingual support:** None natively. Obsidian Publish does not have
multilingual or locale routing support.

**Accessibility:** Not assessed in detail. Obsidian Publish renders static
HTML from Markdown. Screen reader support depends on the rendered HTML quality.

**Architectural fit with MkDocs:** Medium. Obsidian vaults are a plausible
content source for SAT archives. Frontmatter written by Obsidian is
compatible with MkDocs conventions if keys are kept lowercase. The `publish`
key is an Obsidian-specific field for controlling which notes are published.

**SAT ingress implication:** Obsidian vaults used as content sources would
require SAT ingress to read inline YAML frontmatter rather than sidecars.
The `content-metadata-ingress.py` pipeline would apply here directly.

---

## Summary Table

| CMS              | Frontmatter Format | Key Convention | Native Sidecar | Multilingual | Screen Reader | Cognitive Accessibility | MkDocs Fit |
| ---------------- | ------------------ | -------------- | -------------- | ------------ | ------------- | ----------------------- | ---------- |
| Decap CMS        | YAML               | User-defined   | No             | Limited      | Good          | Medium                  | Strong     |
| Sveltia CMS      | YAML               | User-defined   | No             | Native       | Strong        | Strong                  | Strong     |
| Static CMS       | YAML               | User-defined   | No             | Limited      | Good          | Medium                  | Strong     |
| TinaCMS          | YAML               | User-defined   | No             | Good         | Medium        | Strong                  | Weak       |
| Pages CMS        | YAML               | User-defined   | No             | Basic        | Medium        | Strong                  | Medium     |
| Keystatic        | YAML or JSON       | User-defined   | Partial        | Good         | Good          | Medium                  | Medium     |
| Contentlayer     | YAML               | User-defined   | No             | None         | N/A           | N/A                     | Weak       |
| Obsidian Publish | YAML               | lowercase      | No             | None         | Not assessed  | Strong                  | Medium     |

---

## Key Findings

No CMS in this survey has native per-file sidecar support equivalent to SAT's
`.dc.yml` model. All Git-based CMSs in the Decap family (Decap, Sveltia,
Static CMS, Pages CMS) read and write frontmatter inline.

Keystatic is the closest to a sidecar-aware CMS due to its data collection
mode which can write structured YAML files separate from content files,
though the association is by key rather than file proximity.

Sveltia CMS is the strongest recommendation for SAT archives targeting MkDocs
due to its native multilingual support, strong accessibility, and drop-in
compatibility with Decap CMS configuration.

TinaCMS and Contentlayer are not recommended for MkDocs-targeted SAT
workflows due to their JavaScript framework dependencies and architectural
mismatch.

All CMSs in this survey write lowercase or user-defined YAML keys, which
is compatible with MkDocs frontmatter conventions. The SAT egress pipeline
must normalise Title-case keys in existing `.dc.yml` sidecars to lowercase
before any CMS can manage the files.

Round-trip ingress — reading CMS-edited files back into SAT `.dc.yml`
sidecars — is required for all tools in this survey and must be handled by
`content-metadata-ingress.py`.

---

## References

- Decap CMS:
 https://decapcms.org/docs/configuration-options/
- Sveltia CMS:
 https://github.com/sveltia/sveltia-cms
- Static CMS:
 https://www.staticcms.org/docs/configuration-options
- TinaCMS:
 https://tina.io/docs/schema/
- Pages CMS:
 https://pagescms.org/docs/configuration/
- Keystatic:
 https://keystatic.com/docs/configuration
- Contentlayer:
 https://contentlayer.dev/docs/getting-started
- Obsidian Publish:
 https://help.obsidian.md/Obsidian+Publish/Publish+and+unpublish+notes
# Static Site Generator Frontmatter and Sidecar Metadata Survey

## Purpose

This survey documents how each major static site generator handles page-level
metadata, specifically whether it supports frontmatter embedded in content
files, external sidecar metadata files, or both. The focus is on understanding
what the SAT ingress pipeline needs to produce for each target.

---

## Survey Criteria

For each SSG the following questions are answered:

- What metadata mechanism does it use natively?
- What frontmatter format and key naming convention does it expect?
- Does it support per-file external sidecar metadata files?
- Is there a plugin or workaround that adds sidecar support?
- What are the implications for SAT ingress?

---

## MkDocs

**Metadata mechanism:** YAML frontmatter embedded in Markdown files.

**Format and key convention:** YAML delimited by `---`. All keys must be
lowercase. Title-case keys are silently ignored.

**Native sidecar support:** No per-file sidecar support. The Material theme
provides a `.meta.yml` file per directory that applies metadata to all pages
in that directory, which is a partial sidecar pattern but not per-file.

**Plugin options:** None that read per-file external YAML sidecars natively.

**SAT ingress implication:** The `content-egress.py` pipeline must inject
`.dc.yml` sidecar metadata into each Markdown file as lowercase YAML
frontmatter before MkDocs processes it.

---

## Jekyll

**Metadata mechanism:** YAML frontmatter embedded in content files, delimited
by `---`. Supports both YAML and MultiMarkdown style.

**Format and key convention:** YAML or MultiMarkdown. Keys are case-insensitive
in MultiMarkdown style but YAML style keys are case-sensitive. Common keys
include `title`, `date`, `layout`, `permalink`, `categories`, `tags`,
`author`, `published`, `excerpt`, and `draft`.

**Native sidecar support:** No per-file sidecar support. Jekyll has a `_data`
directory for global site data files (YAML, JSON, CSV, TSV) accessible via
`site.data`, but this is site-wide data, not per-page metadata sidecars.
A feature request for per-file `.metadata` sidecar files (inspired by Hakyll)
was raised in 2013 and closed as stale without implementation.

**Plugin options:** No established plugin for per-file sidecars. The
`jekyll-datapage_gen` plugin generates pages from data files but does not
associate external metadata with existing content files.

**SAT ingress implication:** Metadata must be injected as inline frontmatter.
No sidecar pathway exists.

---

## Hugo

**Metadata mechanism:** Frontmatter embedded in content files. Supports YAML,
TOML, and JSON formats, auto-detected by delimiter type.

**Format and key convention:** YAML (`---`), TOML (`+++`), or JSON (`{}`).
Standard keys include `title`, `date`, `draft`, `description`, `tags`,
`categories`, `weight`, `slug`, `aliases`, and `params` for custom fields.
Keys are case-sensitive in YAML and TOML.

**Native sidecar support:** No per-file sidecar support in the traditional
sense. Hugo has a `data/` directory for global data files and supports content
adapters (introduced in v0.126.0) to build pages from remote or local data
sources. Page bundles can include resource metadata via a `resources` array in
frontmatter. None of these are equivalent to a per-file companion metadata
file.

**Plugin options:** Hugo does not have a plugin system in the traditional
sense. Content adapters and data sources can be scripted to approximate sidecar
behaviour but require custom template logic.

**SAT ingress implication:** Metadata must be injected as inline frontmatter.
YAML is the most portable format for cross-tool compatibility.

---

## Eleventy (11ty)

**Metadata mechanism:** Frontmatter embedded in content files, processed via
the `gray-matter` package. Supports YAML, JSON, and JavaScript frontmatter.
TOML is supported via custom configuration.

**Format and key convention:** YAML by default, delimited by `---`. Common
keys include `title`, `description`, `date`, `tags`, `layout`, `permalink`,
and `eleventyExcludeFromCollections`. Keys are case-sensitive.

**Native sidecar support:** Yes, partial. Eleventy supports template data
files as a native feature of its Data Cascade. A file named `post.md` can
have a companion `post.11tydata.json` or `post.11tydata.js` file in the same
directory that supplies additional data for that template only. This is the
closest native per-file sidecar pattern of any SSG in this survey.

**Plugin options:** The Data Cascade also supports directory-level data files
(e.g. `blog.json` for all files in the `blog/` directory), which is
directionally similar to MkDocs Material's `.meta.yml` directory pattern.

**SAT ingress implication:** Eleventy is the most sidecar-friendly SSG
surveyed. SAT `.dc.yml` sidecars could be renamed and reformatted to
`.11tydata.json` files with minimal transformation, making Eleventy the
best-fit SSG for a sidecar-first pipeline.

---

## Astro

**Metadata mechanism:** YAML frontmatter embedded in Markdown and MDX files.
Astro also provides Content Collections with Zod schema validation for
type-safe frontmatter.

**Format and key convention:** YAML delimited by `---`. With Content
Collections, frontmatter is validated against a schema defined in
`src/content.config.ts`. Keys are case-sensitive. The Starlight documentation
theme adds its own frontmatter keys including `title`, `description`, `slug`,
`editUrl`, `head`, `tableOfContents`, `template`, `hero`, `banner`, `badge`,
`lastUpdated`, `prev`, `next`, `pagefind`, `draft`, and `sidebar`.

**Native sidecar support:** No per-file sidecar support. Content Collections
can load data from YAML or JSON files as standalone data collections, but
these are not associated as companions to specific Markdown files.

**Plugin options:** No established plugin for per-file sidecars.

**SAT ingress implication:** Metadata must be injected as inline frontmatter.
Content Collections provide strong schema validation which would catch
frontmatter errors at build time.

---

## Gatsby

**Metadata mechanism:** YAML frontmatter embedded in Markdown and MDX files,
processed via `gatsby-transformer-remark` or `gatsby-plugin-mdx`. Frontmatter
is exposed to the GraphQL data layer.

**Format and key convention:** YAML delimited by `---`. All frontmatter fields
are queryable via GraphQL. Common keys include `title`, `date`, `slug`,
`description`, `tags`, `categories`, `author`, `published`, and `draft`.
Keys are case-sensitive.

**Native sidecar support:** No per-file sidecar support. Gatsby uses
`gatsby-source-filesystem` to source content and `gatsby-transformer-remark`
to parse it. There is no mechanism to associate an external YAML file with a
specific Markdown file. Global data sourcing is available but not per-file
companion metadata.

**Plugin options:** No established plugin for per-file sidecars.

**SAT ingress implication:** Metadata must be injected as inline frontmatter.
Gatsby's GraphQL layer means all frontmatter keys become queryable, which is
an advantage for metadata-rich content pipelines.

---

## Docusaurus

**Metadata mechanism:** YAML frontmatter embedded in Markdown and MDX files.

**Format and key convention:** YAML delimited by `---`. Recognised keys
include `id`, `title`, `description`, `slug`, `sidebar_label`,
`sidebar_position`, `hide_title`, `hide_table_of_contents`, `keywords`,
`image`, `tags`, `draft`, `date`, `authors`, and `custom_edit_url`. Keys are
case-sensitive and must be lowercase.

**Native sidecar support:** No per-file sidecar support.

**Plugin options:** `docusaurus-theme-frontmatter` extends frontmatter
accessibility to React components. No plugin provides per-file external
sidecar reading.

**SAT ingress implication:** Metadata must be injected as inline frontmatter.
Docusaurus has one of the most comprehensive native frontmatter key sets of
any SSG surveyed, covering `id`, `slug`, `authors`, and `keywords` which align
well with Dublin Core metadata fields used in SAT.

---

## Sphinx

**Metadata mechanism:** Sphinx uses reStructuredText (`.rst`) as its primary
format, not Markdown. Metadata is defined using field lists at the top of a
file or via the `.. meta::` directive. The `myst-parser` extension adds
Markdown support with YAML frontmatter.

**Format and key convention:** Field lists in RST (`:key: value` syntax) or
YAML frontmatter when using `myst-parser`. Native recognised metadata fields
include `tocdepth`, `nocomments`, and `orphan`. All other metadata is
project-global via `conf.py`. The `sphinxext-opengraph` extension reads
`description` and other OpenGraph fields from page metadata. Dublin Core
metadata is supported natively in EPUB output configuration.

**Native sidecar support:** No per-file sidecar support.

**Plugin options:** `sphinx-needs` provides a structured metadata object
(`.. metadata::` directive) per page, which can carry `author`, `tags`,
`id`, and custom fields. This is the closest Sphinx comes to per-page
structured metadata, but it is inline in the RST file, not an external file.

**SAT ingress implication:** Sphinx is the most metadata-unfriendly SSG
surveyed from a sidecar perspective. If Sphinx is a target, SAT egress would
need to generate RST field lists or `.. meta::` directives rather than YAML
frontmatter, which is a substantially different output format.

---

## SvelteKit

**Metadata mechanism:** SvelteKit does not have built-in Markdown processing.
Markdown support is added via `mdsvex` (a Svelte preprocessor for Markdown)
or the `kit-docs` library. Both support YAML frontmatter in Markdown files.

**Format and key convention:** YAML delimited by `---` when using `mdsvex`.
Keys are user-defined and accessed as component props. The `kit-docs` library
loads frontmatter metadata for sidebar configuration and page metadata.

**Native sidecar support:** No per-file sidecar support in either SvelteKit
core or `mdsvex`. Data loading in SvelteKit uses `+page.server.js` load
functions, which could be scripted to read companion YAML files but requires
custom implementation.

**Plugin options:** No established plugin for per-file sidecars.

**SAT ingress implication:** Metadata must be injected as inline frontmatter
for `mdsvex`-based sites. SvelteKit's server-side load functions provide the
most flexible mechanism for reading external data files of any SSG surveyed,
but this requires custom code rather than a standard plugin.

---

## Elder.js

**Metadata mechanism:** Elder.js is a Svelte-based SSG that uses a data
pipeline model. Content is sourced via plugins rather than a flat file
convention. Markdown support is provided via `@elderjs/plugin-markdown`, which
reads YAML frontmatter.

**Format and key convention:** YAML delimited by `---`. Keys are user-defined
and passed to the Svelte template as route data. No standard reserved key set.

**Native sidecar support:** No per-file sidecar support. Elder.js's plugin
architecture could be extended to read companion files but no standard
implementation exists. Elder.js is largely superseded by SvelteKit as of 2023
and is in maintenance mode.

**SAT ingress implication:** Elder.js is in maintenance mode and is not
recommended as a new target. If encountered, metadata must be injected as
inline frontmatter.

---

## Summary Table

| SSG        | Frontmatter Format    | Key Convention   | Native Sidecar | Per-file Sidecar Plugin | SAT Ingress Path                 |
| ---------- | --------------------- | ---------------- | -------------- | ----------------------- | -------------------------------- |
| MkDocs     | YAML                  | lowercase        | Directory only | None                    | Inject frontmatter               |
| Jekyll     | YAML or MultiMarkdown | Case-insensitive | No             | None                    | Inject frontmatter               |
| Hugo       | YAML, TOML, or JSON   | Case-sensitive   | No             | None (adapters only)    | Inject frontmatter               |
| Eleventy   | YAML, JSON, or JS     | Case-sensitive   | Yes (native)   | Data Cascade            | Rename .dc.yml to .11tydata.json |
| Astro      | YAML with Zod schema  | Case-sensitive   | No             | None                    | Inject frontmatter               |
| Gatsby     | YAML                  | Case-sensitive   | No             | None                    | Inject frontmatter               |
| Docusaurus | YAML                  | lowercase        | No             | None                    | Inject frontmatter               |
| Sphinx     | RST field lists       | Case-sensitive   | No             | sphinx-needs (inline)   | Generate RST directives          |
| SvelteKit  | YAML (via mdsvex)     | User-defined     | No             | Custom load functions   | Inject frontmatter               |
| Elder.js   | YAML                  | User-defined     | No             | None (maintenance mode) | Inject frontmatter               |

---

## Key Findings

Eleventy is the only SSG in this survey with native per-file sidecar support
via its Data Cascade and `.11tydata.*` companion file convention. This makes
it the best architectural fit for SAT's sidecar-first metadata model without
requiring an egress transformation step.

All other SSGs require metadata to be embedded as inline frontmatter. For
these targets, SAT's `content-egress.py` pipeline must inject `.dc.yml`
sidecar content into each Markdown file as frontmatter before the SSG
processes it.

Sphinx stands apart as the only SSG that does not use YAML frontmatter as its
primary metadata mechanism, requiring a substantially different egress format
if it is a target.

The most common frontmatter key naming convention across SSGs is lowercase
YAML. The existing SAT frontmatter uses Title-case keys which are incompatible
with MkDocs, Docusaurus, Astro, and Gatsby. Key normalisation to lowercase
must be part of the egress transformation.

---

## References

- MkDocs frontmatter:
  https://www.mkdocs.org/user-guide/writing-your-docs/
- MkDocs Material meta plugin:
  https://squidfunk.github.io/mkdocs-material/plugins/meta/
- Jekyll frontmatter:
  https://jekyllrb.com/docs/front-matter/
- Jekyll data files:
  https://jekyllrb.com/docs/datafiles/
- Jekyll sidecar feature request (2013, closed stale):
  https://github.com/jekyll/jekyll/issues/1082
- Hugo frontmatter:
  https://gohugo.io/content-management/front-matter/
- Hugo content adapters:
  https://gohugo.io/content-management/content-adapters/
- Eleventy frontmatter:
  https://www.11ty.dev/docs/data-frontmatter/
- Eleventy Data Cascade:
  https://www.11ty.dev/docs/data-cascade/
- Astro content collections:
  https://docs.astro.build/en/guides/content-collections/
- Astro Starlight frontmatter:
  https://starlight.astro.build/reference/frontmatter/
- Gatsby markdown pages:
  https://www.gatsbyjs.com/docs/how-to/routing/adding-markdown-pages/
- Docusaurus frontmatter:
  https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs
- Sphinx field lists:
  https://www.sphinx-doc.org/en/master/usage/restructuredtext/field-lists.html
- sphinx-needs:
  https://sphinx-needs.readthedocs.io/
- mdsvex (SvelteKit Markdown):
  https://mdsvex.pngwn.io/
- kit-docs (SvelteKit documentation):
  https://github.com/svelteness/kit-docs
- Elder.js markdown plugin:
  https://elderguide.com/tech/elderjs/
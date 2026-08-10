# Hugo Front Matter Reference

## Overview

Front matter is metadata placed at the beginning of a Hugo content file. It defines how Hugo processes and renders the page. Hugo supports three serialization formats:

- YAML (`---`)
- TOML (`+++`)
- JSON (`{ ... }`)

TOML is Hugo's default recommendation, but YAML is also widely used.

> **Note:** Hugo has relatively few built-in front matter fields. Many additional fields commonly seen in themes (e.g., `hero`, `sidebar`, `toc`, `author`, `comments`) are **custom parameters** interpreted by the theme or custom templates rather than by Hugo itself.

---

## Complete Example (YAML)

```yaml
---
# ============================================================================
# Core Identification
# ============================================================================

title: "Understanding Hugo Front Matter"
linkTitle: "Front Matter"
slug: "hugo-front-matter"
url: "/guides/hugo/front-matter/"
aliases:
  - "/old-frontmatter/"
  - "/documentation/frontmatter/"

description: >
  A comprehensive example of Hugo front matter showing built-in fields and
  commonly used custom parameters.

summary: >
  Learn how to use Hugo front matter effectively.

# ============================================================================
# Dates
# ============================================================================

date: 2026-08-05T12:00:00-04:00
publishDate: 2026-08-10T08:00:00-04:00
lastmod: 2026-08-15T09:45:00-04:00
expiryDate: 2030-01-01T00:00:00Z

# ============================================================================
# Draft Status
# ============================================================================

draft: false

# ============================================================================
# Weight & Ordering
# ============================================================================

weight: 10

# ============================================================================
# Taxonomies
# ============================================================================

categories:
  - Documentation
  - Hugo
tags:
  - Hugo
  - Front Matter
  - Static Site Generator
  - Markdown
keywords:
  - Hugo
  - YAML
  - TOML
  - Static Site
series:
  - Hugo Documentation
authors:
  - Christopher Steel
  - Jane Doe

# ============================================================================
# Content Organization
# ============================================================================

type: docs
layout: single
section: guides

cascade:
  draft: false
  type: docs

outputs:
  - HTML
  - RSS
  - JSON

build:
  render: always
  list: always
  publishResources: true

# ============================================================================
# Rendering Options
# ============================================================================

markup:
  goldmark:
    renderer:
      unsafe: false
toc: true

# ============================================================================
# Menu Integration
# ============================================================================

menu:
  docs:
    parent: hugo
    identifier: hugo-frontmatter
    weight: 5
    pre: '<i class="fas fa-book"></i>'
    post: ""
    title: "Front Matter"

# ============================================================================
# Canonical Information
# ============================================================================

canonicalURL: "https://example.org/guides/hugo/front-matter/"

# ============================================================================
# Resources
# ============================================================================

resources:
  - src: hero.jpg
    title: Hero Image
    params:
      alt: Hugo Logo
  - src: diagram.svg
    title: Architecture Diagram

# ============================================================================
# Images
# ============================================================================

images:
  - images/social-card.png
featured_image: images/hero.jpg

# ============================================================================
# Sitemap
# ============================================================================

sitemap:
  priority: 0.8
  changefreq: monthly
  filename: sitemap.xml

# ============================================================================
# Robots
# ============================================================================

robots: index,follow

# ============================================================================
# Custom Parameters
# ============================================================================

params:

  difficulty: Intermediate
  readingTime: 15
  estimatedWords: 2400
  version: "1.0"

  audience:
    - Developers
    - System Administrators

  technologies:
    - Hugo
    - Markdown
    - YAML

  license: CC-BY-SA-4.0
  sourceRepository: https://github.com/example/docs
  sourceBranch: main
  sourcePath: docs/frontmatter.md
  editURL: https://github.com/example/docs/edit/main/docs/frontmatter.md
  showBreadcrumbs: true
  showReadingTime: true
  showLastModified: true
  showWordCount: true
  showToc: true
  showAuthor: true
  showDate: true
  comments: false
  math: false
  mermaid: true
  highlightLines: true
  searchable: true
  printFriendly: true
  citationStyle: APA7
  language: en
  translationKey: hugo-frontmatter

# ============================================================================
# Custom Metadata
# ============================================================================

metadata:
  publisher: Example Organization
  project: Hugo Documentation
  department: Documentation Team
  reviewedBy: Jane Smith
  reviewDate: 2026-08-04
  approvalStatus: Published
  lifecycle: Active
  classification: Public
  accessibility:
    wcag: "2.2 AA"
  dublinCore:
    creator: Christopher Steel
    contributor:
      - Jane Doe
    subject:
      - Hugo
      - Static Sites
    rights: CC-BY-SA-4.0
---
```

---

# Hugo Built-in Front Matter Fields

The following fields are recognized directly by Hugo.

|      | Field         | Purpose                        |
| ---- | ------------- | ------------------------------ |
| 1    | `title`       | Page title                     |
| 2    | `linkTitle`   | Navigation title               |
| 3    | `description` | Meta description               |
| 4    | `summary`     | Summary or excerpt             |
| 5    | `slug`        | URL slug                       |
| 6    | `url`         | Explicit URL                   |
| 7    | `aliases`     | Redirects                      |
| 8    | `date`        | Creation date                  |
| 9    | `publishDate` | Publication date               |
| 10   | `lastmod`     | Last modified date             |
| 11   | `expiryDate`  | Expiration date                |
| 12   | `draft`       | Draft status                   |
| 13   | `weight`      | Ordering within lists          |
| 14   | `type`        | Content type                   |
| 15   | `layout`      | Template to use                |
| 16   | `section`     | Content section                |
| 17   | `cascade`     | Default values for descendants |
| 18   | `outputs`     | Output formats                 |
| 19   | `build`       | Build behavior                 |
| 20   | `resources`   | Page bundle resources          |
| 21   | `menu`        | Menu configuration             |
| 22   | `markup`      | Rendering options              |
| 23   | `sitemap`     | Sitemap settings               |
| 24   | `headless`    | Bundle not rendered as page    |

---

# Common Taxonomies

These are configured in `config.*` and are not limited to Hugo's defaults.

```yaml
tags:
  - Hugo

categories:
  - Documentation

series:
  - Hugo Guide

authors:
  - Christopher Steel
```

---

# Build Options

```yaml
build:
  render: always
  list: always
  publishResources: true
```

Possible values include:

- `always`
- `never`
- `link`

depending on the specific option.

---

# Common Theme Parameters

Many themes define additional parameters under `params:`.

Examples include:

- author
- hero image
- sidebar
- table of contents
- comments
- breadcrumbs
- search
- edit links
- GitHub repository
- Mermaid diagrams
- KaTeX or MathJax
- syntax highlighting
- social sharing
- page rating
- reading time

These are **not** interpreted by Hugo itself unless the theme or templates explicitly use them.

---

# Best Practices

- Use only the built-in fields that your project requires.
- Place theme-specific settings under `params`.
- Prefer ISO 8601 timestamps with timezone offsets.
- Use arrays for taxonomies such as `tags` and `categories`.
- Keep custom metadata logically grouped under `params` or another custom namespace.
- Avoid placing arbitrary top-level keys unless they are consumed by your templates.
- Store reusable assets (images, PDFs, etc.) as page resources when using page bundles.

---

# References

Hugo Authors. (2025). *Front matter*. https://gohugo.io/content-management/front-matter/

Hugo Authors. (2025). *Page variables*. https://gohugo.io/variables/page/

Hugo Authors. (2025). *Page bundles*. https://gohugo.io/content-management/page-bundles/

Hugo Authors. (2025). *Menus*. https://gohugo.io/content-management/menus/

Hugo Authors. (2025). *Taxonomies*. https://gohugo.io/content-management/taxonomies/
# MkDocs Page Front Matter Reference

## Overview

Unlike Hugo, **MkDocs itself has very little built-in front matter**. Most metadata is interpreted by Markdown extensions or plugins rather than MkDocs core.

The most commonly used front matter is YAML enclosed by `---`.

```yaml
---
title: My Page
---
```

The exact fields that are supported depend on:

- MkDocs
- The selected theme (e.g. Material for MkDocs)
- Installed plugins
- Installed Markdown extensions
- Custom hooks and templates

Because of this, there is **no official exhaustive list** equivalent to Hugo's front matter.

---

# Complete Example

The following example demonstrates virtually every commonly used field found in MkDocs ecosystems. Some are built-in, while others are plugin or theme specific.

```yaml
---
# ============================================================================
# Core
# ============================================================================

title: "Understanding MkDocs Front Matter"
description: >
  Comprehensive example of MkDocs page metadata.

summary: >
  Learn how MkDocs front matter works.

# ============================================================================
# Publication
# ============================================================================

draft: false

status: published

date: 2026-08-05

last_modified: 2026-08-06

authors:
  - Christopher Steel
  - Jane Doe

# ============================================================================
# Navigation
# ============================================================================

hide:
  - navigation
  - toc
  - footer

icon: material/book-open-page-variant

template: custom-page.html

# ============================================================================
# SEO
# ============================================================================

slug: mkdocs-front-matter

canonical_url: https://example.org/docs/mkdocs/front-matter/

robots: index,follow

keywords:
  - MkDocs
  - Documentation
  - YAML

tags:
  - Documentation
  - MkDocs

categories:
  - Documentation

# ============================================================================
# Social Sharing
# ============================================================================

social_image: images/social-card.png

featured_image: images/hero.png

# ============================================================================
# Search
# ============================================================================

search:
  boost: 2.0
  exclude: false

# ============================================================================
# TOC
# ============================================================================

toc: true

toc_depth: 3

# ============================================================================
# Page Options
# ============================================================================

comments: false

show_author: true

show_date: true

show_reading_time: true

show_last_updated: true

show_edit_button: true

show_breadcrumbs: true

show_tags: true

show_feedback: false

# ============================================================================
# Content
# ============================================================================

math: true

mermaid: true

diagram: true

admonitions: true

footnotes: true

emoji: true

# ============================================================================
# Resources
# ============================================================================

resources:
  - images/hero.png
  - diagrams/architecture.svg
  - downloads/reference.pdf

attachments:
  - cheatsheet.pdf

# ============================================================================
# Versioning
# ============================================================================

version: "1.4"

since: "1.0"

deprecated: false

experimental: false

# ============================================================================
# Documentation
# ============================================================================

difficulty: Intermediate

estimated_reading_time: 15

word_count: 2500

audience:
  - Developers
  - System Administrators

technologies:
  - MkDocs
  - Material
  - Markdown

# ============================================================================
# Source Information
# ============================================================================

repository: https://github.com/example/docs

branch: main

edit_uri: edit/main/docs/

source_path: docs/front-matter.md

# ============================================================================
# Accessibility
# ============================================================================

accessibility:

  wcag: "2.2 AA"

  language: en

# ============================================================================
# Dublin Core
# ============================================================================

dc:

  title: Understanding MkDocs Front Matter

  creator: Christopher Steel

  contributor:
    - Jane Doe

  publisher: Example Organization

  subject:
    - MkDocs
    - Documentation

  description: MkDocs metadata example.

  rights: CC-BY-SA-4.0

# ============================================================================
# Custom Metadata
# ============================================================================

metadata:

  project: Documentation Platform

  department: Documentation

  reviewed_by: Jane Smith

  review_date: 2026-08-04

  approval_status: Published

  lifecycle: Active

  classification: Public

  citation_style: APA7

  translation_key: mkdocs-front-matter

# ============================================================================
# Theme Parameters
# ============================================================================

extra:

  hero: true

  hero_image: images/hero.png

  sidebar: true

  landing_page: false

  announcement: "Version 2.0 released"

  related:
    - installation.md
    - configuration.md

---
```

---

# MkDocs Core

MkDocs itself recognizes very few page metadata fields.

| Field           | Core MkDocs | Notes                            |
| --------------- | :---------: | -------------------------------- |
| `title`         |      ✅      | Official                         |
| Everything else |      ❌      | Passed through to themes/plugins |

---

# Material for MkDocs

Material recognizes several additional metadata fields directly or via Insiders features.

Common examples include:

| Field      | Purpose                            |
| ---------- | ---------------------------------- |
| `hide`     | Hide navigation, TOC, footer, etc. |
| `icon`     | Navigation icon                    |
| `template` | Alternate template                 |
| `search`   | Search weighting/exclusion         |
| `tags`     | Tags plugin integration            |
| `draft`    | Draft handling (plugin dependent)  |

---

# MkDocs Plugins

Many plugins define their own front matter.

Examples include:

## Tags Plugin

```yaml
tags:
  - Linux
  - Security
```

---

## Blog Plugin

```yaml
date: 2026-08-05

authors:
  - Christopher Steel

draft: false
```

---

## RSS Plugin

```yaml
description: RSS description
```

---

## Social Plugin

```yaml
social_image: images/card.png
```

---

## Git Revision Date Plugin

Usually requires **no page metadata**, but can consume custom fields if configured.

---

## Awesome Pages

Supports ordering and navigation metadata.

---

# Material Metadata Cards

Many users define custom metadata such as:

```yaml
difficulty: Advanced

reading_time: 20

audience:
  - Developers

technologies:
  - Python
  - Docker
```

These become available in Jinja templates.

---

# Custom Metadata

MkDocs preserves arbitrary YAML front matter and exposes it to templates.

Example:

```yaml
project:

  owner: Infrastructure Team

  repository: GitHub

security:

  classification: Public

  reviewed: true

compliance:

  iso27001: true

  soc2: false
```

A custom theme can render these values anywhere on the page.

---

# Best Practices

- Use `title` for page titles.
- Keep all non-standard metadata grouped under namespaces such as `extra`, `metadata`, or `dc`.
- Avoid polluting the top level with arbitrary keys unless required by a plugin.
- Prefer ISO 8601 dates.
- Store reusable metadata consistently across pages.
- Document any custom metadata used by your organization so authors know its purpose.

---

# Comparison with Hugo

| Feature                  | Hugo      | MkDocs                                  |
| ------------------------ | --------- | --------------------------------------- |
| Built-in page metadata   | Extensive | Minimal                                 |
| Arbitrary metadata       | Yes       | Yes                                     |
| Theme-specific metadata  | Yes       | Yes                                     |
| Plugin-specific metadata | Yes       | Yes                                     |
| Taxonomies               | Built-in  | Plugin-based                            |
| Menus                    | Built-in  | Navigation configuration (`mkdocs.yml`) |
| Page bundles/resources   | Built-in  | Not built-in                            |
| Rendering options        | Built-in  | Markdown extension and plugin based     |

---

# References

- MkDocs Project. *Writing your docs*. https://www.mkdocs.org/user-guide/writing-your-docs/
- Material for MkDocs. *Reference*. https://squidfunk.github.io/mkdocs-material/reference/
- Material for MkDocs. *Blog Plugin*. https://squidfunk.github.io/mkdocs-material/plugins/blog/
- Material for MkDocs. *Tags Plugin*. https://squidfunk.github.io/mkdocs-material/plugins/tags/
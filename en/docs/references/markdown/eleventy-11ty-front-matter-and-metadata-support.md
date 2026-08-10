---
title: "Eleventy (11ty) Front Matter and Metadata Support"
description: "A detailed overview of Eleventy's built-in front matter capabilities, metadata model, and suitability for semantic, accessible, metadata-rich websites."
author: "Christopher Steel"
date: 2026-08-05
version: "1.0"
language: "en-CA"
keywords:
  - Eleventy
  - 11ty
  - Front Matter
  - Metadata
  - Dublin Core
  - Schema.org
  - Accessibility
  - Static Site Generator
---

# Eleventy (11ty) Front Matter and Metadata Support

## Overview

Eleventy (11ty) has one of the most flexible front matter and metadata systems among modern static site generators.

Unlike systems that define a fixed content model, Eleventy treats front matter as arbitrary structured data that becomes part of its data cascade system.

This design makes Eleventy particularly suitable for:

- Accessibility-focused websites
- Metadata-rich publishing systems
- Knowledge bases
- Digital archives
- Documentation platforms
- Semantic web projects
- Dublin Core implementations
- Schema.org and JSON-LD integration

The philosophy is:

> The content author defines the metadata model rather than the static site generator imposing one.

---

# Front Matter Basics

Eleventy supports YAML front matter by default.

Example:

```yaml
---
title: "Example Page"
description: "An accessible example page."
date: 2026-08-05
layout: "page.njk"

tags:
  - accessibility
  - open-source
---
```

The values become available to templates:

```njk
<h1>{{ title }}</h1>

<p>{{ description }}</p>
```

---

# Supported Front Matter Formats

## YAML

The most common format.

```yaml
---
title: "My Website"
author: "Christopher Steel"
published: true
---
```

---

## JSON

Eleventy also supports JSON-style front matter.

```json
---
{
  "title": "My Website",
  "author": "Christopher Steel",
  "published": true
}
---
```

---

## JavaScript Front Matter

JavaScript front matter allows computed values.

```javascript
---
{
  title: "My Website",
  date: new Date(),
  tags: [
    "accessibility",
    "web"
  ]
}
---
```

This is useful for:

- Dynamic metadata
- Generated dates
- Calculated values
- Custom logic

---

# Eleventy Data Model

Eleventy exposes content through a data cascade.

Data can come from:

- Front matter
- Directory data files
- Global data files
- Computed data
- Collections
- Layout inheritance

The final page receives a combined data object.

---

# Built-In Page Metadata

Every Eleventy page receives a `page` object.

Example:

```njk
{{ page.url }}

{{ page.inputPath }}

{{ page.outputPath }}

{{ page.fileSlug }}
```

Common properties:

| Property            | Description           |
| ------------------- | --------------------- |
| page.url            | Generated URL         |
| page.inputPath      | Source file path      |
| page.outputPath     | Generated output path |
| page.fileSlug       | Filename-derived slug |
| page.date           | Page date             |
| page.rawInput       | Original content      |
| page.templateSyntax | Template engine       |

---

# Layout Metadata

Pages can define layouts.

Example:

```yaml
---
title: "Accessibility Guide"
layout: "layouts/article.njk"
---
```

Layouts can inherit metadata:

```yaml
---
layout: base.njk
---
```

---

# Collections

Collections provide grouping and organization.

Example:

```yaml
---
title: "Linux Administration Guide"

tags:
  - linux
  - documentation
---
```

Creates a collection:

```javascript
collections.linux
```

Used in templates:

```njk
{% for item in collections.linux %}

<h2>{{ item.data.title }}</h2>

{% endfor %}
```

---

# Custom Metadata

Eleventy does not restrict metadata fields.

Custom namespaces are fully supported.

Example:

```yaml
---
title: "Accessible Design Principles"

description: >
  A guide to building accessible websites.

author:
  name: "Christopher Steel"

dc:
  title: "Accessible Design Principles"
  creator: "Christopher Steel"
  language: "en-CA"
  rights: "CC BY-SA 4.0"

schema:
  "@type": "TechArticle"
  audience: "Developers"

accessibility:
  wcag:
    version: "2.2"
    level: "AA"

---
```

Templates can access:

```njk
{{ dc.creator }}

{{ accessibility.wcag.level }}
```

---

# Dublin Core Example

Eleventy works naturally with Dublin Core metadata.

Example:

```yaml
---
title: "Universal Cake"

dc:
  title: "Universal Cake"

  creator:
    - "Christopher Steel"

  subject:
    - "Systems Theory"
    - "Accessibility"
    - "Digital Inclusion"

  description:
    "A systems approach to accessible digital publishing."

  publisher:
    "Universal Cake"

  date:
    "2026-08-05"

  type:
    "Text"

  format:
    "text/markdown"

  identifier:
    "https://example.org/universal-cake"

  language:
    "en-CA"

  rights:
    "CC BY-SA 4.0"

dcterms:
  created:
    "2026-08-05"

  modified:
    "2026-08-05"

  spatial:
    "Montreal, Canada"

---
```

This metadata can then be transformed into:

- HTML metadata
- Dublin Core meta tags
- JSON-LD
- Schema.org structured data
- RDFa

---

# Schema.org Example

Example:

```yaml
---
title: "Accessibility Handbook"

schema:
  type: "TechArticle"

  headline:
    "Accessibility Handbook"

  author:
    "@type": "Person"
    name:
      "Christopher Steel"

  accessibilityFeature:
    - "alternativeText"
    - "keyboardNavigation"
    - "highContrast"

---
```

A template can generate:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "TechArticle"
}
</script>
```

---

# Accessibility Metadata Example

Eleventy can support custom accessibility vocabularies.

Example:

```yaml
---
title: "Accessible Website"

accessibility:

  standard:
    - "WCAG 2.2"

  conformance:
    level: "AA"

  features:
    - keyboardNavigation
    - screenReaderSupport
    - captions
    - semanticHTML

  testing:
    tools:
      - axe-core
      - Lighthouse
      - Pa11y

---
```

---

# Internationalization Metadata

Eleventy does not enforce an internationalization model but supports custom structures.

Example:

```yaml
---
title: "Accessibility"

language:
  primary: "en-CA"

translations:

  fr:
    "/fr/accessibilite/"

  es:
    "/es/accesibilidad/"

alternate:

  - language: "fr-CA"
    url: "/fr/accessibilite/"

---
```

---

# Permalinks

Custom URLs can be defined:

```yaml
---
title: "About"

permalink:
  "/about/"
---
```

---

# Draft Content

Eleventy does not include Hugo-style drafts by default.

However, drafts can easily be implemented.

Example:

```yaml
---
title: "Future Article"

draft: true
---
```

Then filter during collection creation:

```javascript
eleventyConfig.addCollection(
  "published",
  function(collectionApi) {

    return collectionApi
      .getAll()
      .filter(
        item => !item.data.draft
      );

  }
);
```

---

# Metadata Architecture Example

A metadata-rich Eleventy site could use:

```
content/
|
├── articles/
│   ├── accessibility.md
│   └── systems-thinking.md
|
├── people/
│   └── author.md
|
├── organizations/
│   └── organization.md
|
└── standards/
    └── wcag.md


Front Matter
|
+-- Content Metadata
|
+-- Dublin Core
|
+-- Schema.org
|
+-- Accessibility Metadata
|
+-- Localization Metadata
|
+-- Rights Metadata
|
Eleventy Data Cascade
|
Templates
|
+-- Semantic HTML
|
+-- JSON-LD
|
+-- RSS
|
+-- Sitemap
|
+-- Accessible Website
```

---

# Eleventy Compared With Hugo

| Feature              | Eleventy       | Hugo             |
| -------------------- | -------------- | ---------------- |
| Custom metadata      | Excellent      | Excellent        |
| Dublin Core          | Excellent      | Excellent        |
| Schema.org           | Excellent      | Excellent        |
| JSON-LD              | Excellent      | Excellent        |
| Collections          | Excellent      | Excellent        |
| Taxonomies           | Excellent      | Excellent        |
| Multilingual         | Custom         | Built-in         |
| Content model        | User-defined   | More opinionated |
| Metadata flexibility | Extremely high | Very high        |

---

# Recommendation

For websites requiring:

- WCAG accessibility
- Semantic HTML
- Dublin Core metadata
- Schema.org integration
- Knowledge graphs
- Open standards
- Long-term maintainability

Eleventy is one of the strongest choices available.

Its greatest advantage is that metadata remains a first-class design decision controlled by the project rather than constrained by the framework.

For a Universal Cake-style knowledge platform, Eleventy provides an excellent foundation because the information architecture can evolve over time while maintaining standards-based metadata.
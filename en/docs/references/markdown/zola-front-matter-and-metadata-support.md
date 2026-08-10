---
title: "Zola Front Matter and Metadata Support"
description: "A detailed overview of Zola's built-in front matter capabilities, metadata model, and suitability for accessible, standards-based, open-source websites."
author: "Christopher Steel"
date: 2026-08-05
version: "1.0"
language: "en-CA"
keywords:
  - Zola
  - Static Site Generator
  - Front Matter
  - Metadata
  - Markdown
  - Accessibility
  - Dublin Core
  - Schema.org
  - Open Source
---

# Zola Front Matter and Metadata Support

## Overview

Zola is a fast, open-source static site generator written in Rust.

Zola follows a design philosophy similar to Hugo:

- Markdown-first content
- Template-driven websites
- Built-in taxonomies
- Multilingual support
- Static HTML generation
- Strong performance
- Minimal dependencies

Zola uses the **Tera template engine**, inspired by Jinja2, making it familiar to users of Python-based templating systems.

Its front matter system is based on **TOML by default**, although YAML and JSON front matter are also supported.

---

# Front Matter Basics

A typical Zola Markdown file begins with TOML front matter.

Example:

```toml
+++
title = "Example Page"
description = "An accessible example page."
date = 2026-08-05

draft = false

[taxonomies]
tags = [
  "accessibility",
  "open-source"
]
+++
```

The values become available in templates:

```html
<h1>{{ page.title }}</h1>

<p>{{ page.description }}</p>
```

---

# Supported Front Matter Formats

Zola supports:

- TOML
- YAML
- JSON

TOML is the recommended default.

---

# TOML Front Matter

Example:

```toml
+++
title = "My Website"

author = "Christopher Steel"

date = 2026-08-05

draft = false

+++
```

Advantages:

- Native Rust ecosystem format
- Strict syntax
- Excellent support for structured metadata
- Easy handling of nested objects

---

# YAML Front Matter

Zola also supports YAML:

```yaml
---
title: "My Website"

author:
  name: "Christopher Steel"

date: 2026-08-05

draft: false
---
```

---

# JSON Front Matter

JSON format:

```json
{
  "title": "My Website",
  "author": "Christopher Steel",
  "draft": false
}
```

---

# Built-In Page Metadata

Every content page receives a page object.

Common properties include:

| Property         | Description       |
| ---------------- | ----------------- |
| page.title       | Page title        |
| page.description | Description       |
| page.date        | Publication date  |
| page.updated     | Last update date  |
| page.permalink   | Generated URL     |
| page.slug        | URL slug          |
| page.path        | Source path       |
| page.content     | Rendered content  |
| page.summary     | Generated summary |
| page.extra       | Custom metadata   |

---

# Custom Metadata with `extra`

Zola provides a dedicated namespace for custom metadata.

Example:

```toml
+++
title = "Accessible Design Principles"

[extra]

author = "Christopher Steel"

license = "CC BY-SA 4.0"

reading_time = 10

accessibility_level = "WCAG 2.2 AA"

+++
```

Access in templates:

```tera
{{ page.extra.author }}

{{ page.extra.license }}
```

---

# Structured Custom Metadata

Zola supports nested metadata.

Example:

```toml
+++
title = "Accessibility Handbook"

[extra]

[extra.accessibility]

standard = "WCAG"

version = "2.2"

level = "AA"

features = [
  "keyboard-navigation",
  "semantic-html",
  "captions"
]

+++
```

Template usage:

```tera
{{ page.extra.accessibility.level }}
```

---

# Dublin Core Metadata Example

Zola can represent Dublin Core metadata using custom fields.

Example:

```toml
+++
title = "Universal Cake"

[extra.dc]

title = "Universal Cake"

creator = [
  "Christopher Steel"
]

subject = [
  "Systems Theory",
  "Accessibility",
  "Digital Inclusion"
]

description = "A systems approach to accessible digital publishing."

publisher = "Universal Cake"

language = "en-CA"

rights = "CC BY-SA 4.0"


[extra.dcterms]

created = "2026-08-05"

modified = "2026-08-05"

spatial = "Montreal, Canada"

+++
```

Templates can generate:

- Dublin Core HTML metadata
- JSON-LD
- RDFa
- Open Graph metadata

---

# Schema.org Metadata Example

Example:

```toml
+++
title = "Accessibility Handbook"

[extra.schema]

type = "TechArticle"

headline = "Accessibility Handbook"

author = "Christopher Steel"

accessibilityFeature = [
  "alternativeText",
  "keyboardNavigation",
  "captions"
]

+++
```

A template can convert this into:

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

Zola works well for custom accessibility vocabularies.

Example:

```toml
+++

title = "Accessible Website"

[extra.accessibility]

standard = [
  "WCAG 2.2"
]

conformance = "AA"

features = [
  "semantic-html",
  "keyboard-navigation",
  "screen-reader-support"
]

testing_tools = [
  "axe-core",
  "Pa11y",
  "Lighthouse"
]

+++
```

---

# Taxonomies

Taxonomies are a built-in Zola feature.

Example:

```toml
+++

title = "Linux Administration"

[taxonomies]

tags = [
  "linux",
  "sysadmin"
]

+++
```

Configured in:

```toml
config.toml
```

Example:

```toml
taxonomies = [
  { name = "tags" },
  { name = "categories" }
]
```

This provides:

- Tag pages
- Category pages
- Content grouping
- Navigation structures

---

# Dates and Publishing

Zola supports dates naturally.

Example:

```toml
+++

title = "Article"

date = 2026-08-05

updated = 2026-08-06

+++
```

Draft pages:

```toml
+++

title = "Future Article"

draft = true

+++
```

Draft content can be excluded during builds.

---

# Permalinks

Custom URLs:

```toml
+++

title = "About"

slug = "about"

+++
```

Or configure section paths:

```toml
+++

path = "about"

+++
```

---

# Multilingual Support

Zola includes built-in multilingual support.

Example:

Directory:

```
content/

├── _index.md

├── about.md

└── about.fr.md
```

Configuration:

```toml
default_language = "en"

languages.en.title = "Example Site"

languages.fr.title = "Site Exemple"
```

Front matter:

```toml
+++

title = "Accessibility"

[extra]

language = "en-CA"

+++
```

---

# Content Organization

A typical Zola project:

```
website/

├── config.toml

├── content/

│   ├── _index.md

│   ├── articles/

│   │   └── accessibility.md

│   └── documentation/

│       └── guide.md


├── templates/

│   ├── page.html

│   └── base.html


├── static/

│   ├── images/

│   └── files/


└── themes/
```

---

# Metadata Architecture Example

A metadata-rich Zola site:

```
Markdown Content

        |

        |

TOML Front Matter

        |

        +-- Dublin Core

        |

        +-- Schema.org

        |

        +-- Accessibility Metadata

        |

        +-- Rights Metadata

        |

        +-- Localization Metadata

        |

Zola Page Object

        |

Tera Templates

        |

        +-- Semantic HTML

        |

        +-- JSON-LD

        |

        +-- RSS

        |

        +-- Sitemap

        |

Accessible Static Website
```

---

# Zola Compared With Eleventy and Hugo

| Feature         | Zola      | Eleventy  | Hugo       |
| --------------- | --------- | --------- | ---------- |
| Custom metadata | Excellent | Excellent | Excellent  |
| Dublin Core     | Excellent | Excellent | Excellent  |
| Schema.org      | Excellent | Excellent | Excellent  |
| JSON-LD         | Excellent | Excellent | Excellent  |
| Taxonomies      | Excellent | Excellent | Excellent  |
| Collections     | Good      | Excellent | Excellent  |
| Multilingual    | Built-in  | Custom    | Built-in   |
| Configuration   | Simple    | Flexible  | Extensive  |
| Performance     | Excellent | Very Good | Excellent  |
| Ecosystem size  | Smaller   | Large     | Very Large |

---

# Accessibility Considerations

Zola itself produces static HTML, which provides a strong foundation for accessibility.

Important implementation practices remain the responsibility of the theme:

- Use semantic HTML
- Provide proper heading hierarchy
- Include image alternative text
- Support keyboard navigation
- Maintain color contrast
- Avoid unnecessary JavaScript
- Provide captions and transcripts
- Test against WCAG requirements

---

# Recommendation

Zola is an excellent choice for organizations that want:

- Open-source infrastructure
- Fast builds
- Minimal dependencies
- Strong metadata support
- Accessible static websites
- Markdown-based publishing
- Long-term maintainability

Compared with Eleventy, Zola provides more built-in structure.

Compared with Hugo, Zola offers a simpler and cleaner architecture.

For a standards-driven knowledge platform requiring:

- Dublin Core
- Schema.org
- WCAG metadata
- multilingual publishing
- semantic HTML
- open-source governance

Zola is a strong candidate, especially where simplicity, speed, and maintainability are priorities.

---

# References

GetZola.org. (2025). *Zola documentation*. https://www.getzola.org/documentation/

Hugo Authors. (2025). *Hugo documentation*. https://gohugo.io/documentation/

Eleventy Authors. (2025). *Eleventy documentation*. https://www.11ty.dev/docs/

MDN Web Docs. (2025). *HTML accessibility*. https://developer.mozilla.org/

W3C. (2023). *Web Content Accessibility Guidelines (WCAG) 2.2*. https://www.w3.org/TR/WCAG22/
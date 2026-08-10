# Choosing a Static Site Generator and CMS for a Highly Accessible Open-Source Website

## Overview

For a highly accessible, open-source, long-lived website focused on:

- WCAG 2.2 accessibility
- multilingual content
- structured metadata
- Dublin Core compatibility
- Schema.org support
- user-friendly content management
- Git-based workflows
- transparency and portability
- minimal vendor lock-in

the recommended architecture is:

```
Content Editors
      |
  Git-based CMS
      |
 Markdown + Front Matter
      |
 Static Site Generator
      |
 Accessibility Testing
      |
 Static Website
      |
 Users
```

The strongest candidates are:

1. Eleventy (11ty) + Sveltia CMS
2. Hugo + Sveltia CMS
3. Zola + custom CMS workflow

---

# Recommendation Ranking

| Rank | Static Site Generator | CMS                            | Best Use Case                         |
| ---- | --------------------- | ------------------------------ | ------------------------------------- |
| 1    | Eleventy (11ty)       | Sveltia CMS                    | Accessibility-first public websites   |
| 2    | Hugo                  | Sveltia CMS                    | Large documentation and content sites |
| 3    | Zola                  | Sveltia CMS or custom workflow | Minimal, elegant technical platforms  |

---

# 1. Eleventy (11ty) + Sveltia CMS

## Recommendation

Eleventy is the strongest choice when accessibility and semantic control are the highest priorities.

Eleventy does not force a specific HTML structure. The developer controls:

- semantic HTML
- heading hierarchy
- ARIA implementation
- navigation structure
- metadata
- structured data
- accessibility patterns

Architecture:

```
Editors
 |
Sveltia CMS
 |
Git Repository
 |
Eleventy Build
 |
HTML/CSS/JavaScript
 |
Website
```

---

# Why Eleventy Works Well for Accessibility

Eleventy allows direct control over:

- HTML5 landmarks
- screen-reader support
- keyboard navigation
- progressive enhancement
- reduced JavaScript dependency

Example:

```html
<article>

<header>
<h1>Accessible Design Principles</h1>

<p>
Building inclusive digital experiences.
</p>
</header>

<section>
Content goes here.
</section>

</article>
```

The generated output is simple HTML that is easy to audit.

---

# Example Eleventy Front Matter

```yaml
---
title: "Accessible Design Principles"
description: "Creating inclusive digital experiences."
date: 2026-08-05
author:
  name: "Christopher Steel"
tags:
  - accessibility
  - universal-design
  - open-source
license:
  name: "Creative Commons Attribution Share-Alike"
  url: "https://creativecommons.org/licenses/by-sa/4.0/"
dublin_core:
  creator: "Christopher Steel"
  subject:
    - accessibility
    - digital inclusion
  rights: "CC BY-SA 4.0"
schema:
  type: "TechArticle"
accessibility:
  wcag:
    version: "2.2"
    level: "AA"
---
```

---

# Sveltia CMS

Sveltia CMS provides a modern Git-based editing experience.

Website:

https://github.com/sveltia/sveltia-cms

Architecture:

```
Content Editor
      |
Sveltia CMS
      |
Git Commit
      |
Eleventy Build
      |
Published Website
```

Advantages:

- open-source
- Git-backed
- no proprietary database
- Markdown-based
- version controlled
- content remains portable

---

# 2. Hugo + Sveltia CMS

## Recommendation

Hugo is an excellent choice when:

- performance is critical
- the website is large
- build speed matters
- multilingual publishing is important

Architecture:

```
Editors

 |

Sveltia CMS

 |

Git Repository

 |

Hugo

 |

Static Website
```

---

# Hugo Strengths

Advantages:

- extremely fast builds
- mature ecosystem
- excellent internationalization
- strong taxonomy support
- simple deployment

Good for:

- documentation portals
- organizational websites
- knowledge bases
- large content collections

---

# Hugo Accessibility Considerations

Hugo itself is not an accessibility problem.

The main risk is themes.

Common theme problems:

- incorrect heading hierarchy
- inaccessible menus
- poor contrast
- unnecessary JavaScript
- missing alternative text

A custom accessible theme is recommended.

---

# 3. Zola

## Recommendation

Zola is technically elegant and simple.

Strengths:

- written in Rust
- fast builds
- clean architecture
- simple templates
- TOML-based configuration

Architecture:

```
Editors

 |

CMS

 |

Git Repository

 |

Zola

 |

Website
```

---

# Zola Limitations

The main limitation is ecosystem maturity.

Compared with Hugo and Eleventy:

- fewer CMS integrations
- fewer themes
- fewer accessibility-focused templates
- smaller community

For organizations comfortable maintaining infrastructure, Zola is attractive.

---

# Recommended CMS Options

## Sveltia CMS

Recommended for:

- community websites
- organizations
- documentation
- knowledge platforms

Advantages:

- Git workflow
- Markdown support
- structured metadata
- no database dependency

---

## Other CMS Options

## Decap CMS

Formerly Netlify CMS.

Good:

- mature
- Git-backed
- widely used

Limitations:

- slower evolution
- older architecture

---

## Tina CMS

Good:

- visual editing
- developer friendly

Limitations:

- more tightly integrated with commercial services

---

## WordPress as CMS

WordPress remains powerful but introduces complexity:

Challenges:

- database dependency
- plugin maintenance
- theme accessibility differences
- security updates
- content portability concerns

A static-first CMS architecture reduces these issues.

---

# Recommended Architecture for a Public-Interest Website

For a project requiring:

- accessibility
- transparency
- community participation
- multilingual publishing
- long-term sustainability

a strong architecture would be:

```
                    Editors

                       |

                 Sveltia CMS

                       |

                 Git Repository

                       |

        +--------------+--------------+

        |                             |

   Eleventy Build              Automated Testing

        |                             |

        |                       axe-core
        |                       Pa11y
        |                       Lighthouse

        |

   Static Website

        |

 Apache / Nginx / CDN
```

---

# Supporting Standards

## Content

Recommended:

- Markdown
- YAML Front Matter
- Dublin Core Metadata
- Schema.org JSON-LD
- Creative Commons licensing

---

## Accessibility

Recommended:

- WCAG 2.2 AA minimum
- semantic HTML5
- keyboard-first testing
- screen reader testing
- automated accessibility checks

Tools:

- axe-core
- Pa11y
- Lighthouse

---

## Deployment

A Git-based deployment model:

```
Developer Commit

      |

Git Repository

      |

Automated Build

      |

Accessibility Tests

      |

Deployment

      |

Web Server
```

fits well with:

- Ansible
- Apache
- Nginx
- rsync
- Git hooks

---

# Final Recommendation

For a highly accessible, open-source CMS platform:

## Primary Recommendation

```
Eleventy

+

Sveltia CMS

+

Git

+

Markdown

+

Dublin Core

+

Schema.org

+

Automated Accessibility Testing
```

This provides:

- maximum control over accessibility
- excellent metadata support
- long-term content portability
- open-source governance
- minimal infrastructure complexity

## Alternative

For very large sites:

```
Hugo

+

Sveltia CMS

+

Git

+

Automated Accessibility Testing
```

For a project focused on public knowledge, inclusion, and digital sovereignty, Eleventy provides the strongest foundation because it keeps the final HTML output understandable, auditable, and under direct developer control.
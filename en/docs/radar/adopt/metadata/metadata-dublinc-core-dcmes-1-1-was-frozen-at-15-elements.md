# Metadata - Dublinc Core -DCMES 1.1 was frozen at 15 elements

title, creator, subject, description, publisher, contributor, date, type, format, identifier, source, language, relation, coverage, rights

* https://www.dublincore.org/specifications/dublin-core/usageguide/elements/

```yaml
# Unqualified DCMES 1.1 — all 15 elements, with example values.
# Matches this project's front matter convention (flat keys, no "dc:" or
# "dcterms:" prefix in the YAML itself — the prefix only appears in the
# rendered <meta name="dc.X"> tags).

title: "About"                                    # dc:title
creator: "example.com editorial team"             # dc:creator
subject:                                          # dc:subject (repeatable)
  - "accessibility"
  - "inclusive design"
  - "web publishing"
description: "Who example.com is and what we build."  # dc:description
publisher: "example.com"                          # dc:publisher
contributor: "Jane Doe"                           # dc:contributor
date: "2026-08-17"                                # dc:date
type: "Text"                                      # dc:type — DCMI Type Vocabulary term
format: "text/html"                               # dc:format — IANA media type
identifier: "urn:uuid:8a1e2f3b-1c4d-4e5a-9f2b-1234567890ab"  # dc:identifier
source: "https://legacy.example.com/about.html"   # dc:source — resource this was derived from
language: "en-CA"                                 # dc:language
relation: "urn:uuid:ffdca22b-a52c-4d71-b590-e3a4dbd29d23"    # dc:relation — Work-pairing UUID, this site's local convention
coverage: "Canada"                                # dc:coverage — spatial/temporal scope
rights: "© 2026 example.com. All rights reserved." # dc:rights
```

Two things worth flagging on the values themselves, not just the shape:

- **`source`** — nothing in the site currently populates this. It's for "this page is derived from X" (a prior printed version, a legacy URL, a scanned original), which is a real archival use case for something called SAT, but nothing in the current content has that relationship, so I made up a plausible example rather than pointing at something real.
- **`type`** — should come from the [DCMI Type Vocabulary](https://www.dublincore.org/documents/dcmi-type-vocabulary/) per the spec's own recommendation (Text, Image, Sound, Dataset, etc.), not free text — worth tightening the CMS field from a plain string to a constrained `select` if you want that enforced rather than just hinted.




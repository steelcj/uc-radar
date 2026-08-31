# Representing UUIDs with Dublin Core and DCTERMS

**Status:** Informational
**Vocabulary:** Dublin Core Metadata Terms (DCTERMS)
**Subject:** UUIDs, identifiers, RDF, metadata
**Format:** Web-ready Markdown

## Overview

A UUID (Universally Unique Identifier) can be represented using **Dublin Core Metadata Terms**, but neither Dublin Core Elements nor DCTERMS defines a dedicated `uuid` property.

dc:identifier is one of the original Dublin Core Metadata Element Set elements. This is also a valid identifier with multiple values:

```yaml
dc:identifier:
  - "550e8400-e29b-41d4-a716-446655440000"
  - "my-document"
```

The appropriate Dublin Core property is:

```text
dcterms:identifier
```

A UUID is an **identifier value**, so it fits naturally within `dcterms:identifier`.

For example:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

The important distinction is that **DCTERMS defines the meaning of the metadata property**, while the UUID specification defines the format and semantics of the identifier value.

------

## 1. `dcterms:identifier`

`dcterms:identifier` is the preferred DCTERMS property for an unambiguous reference to a resource.

A UUID can therefore be used directly as its value:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

In RDF/Turtle:

```turtle
dcterms:identifier "550e8400-e29b-41d4-a716-446655440000" .
```

This does **not** mean that DCTERMS is defining the UUID. It means:

> The resource has an identifier whose value happens to be a UUID.

This is an important separation of concerns.

------

## 2. `dc:identifier` versus `dcterms:identifier`

Dublin Core has two commonly encountered forms:

```text
dc:identifier
dcterms:identifier
```

`dc:identifier` is one of the original **Dublin Core Metadata Element Set** elements. This is also a valid identifier with multiple values:

```yaml
dc:identifier:
  - "550e8400-e29b-41d4-a716-446655440000"
  - "my-document"
```



`dcterms:identifier` is the corresponding property in the more extensive **Dublin Core Metadata Terms** vocabulary.

For new metadata systems, using:

```text
dcterms:identifier
```

is generally preferable when you are already using DCTERMS.

For example:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

rather than introducing a non-standard property such as:

```yaml
dcterms:
  uuid: "550e8400-e29b-41d4-a716-446655440000"
```

There is **no standard `dcterms:uuid` property**.

------

## 3. A UUID is an identifier, not a Dublin Core property

It is useful to keep three concepts separate:

| Concept                  | Example                                | Role                                             |
| ------------------------ | -------------------------------------- | ------------------------------------------------ |
| Metadata property        | `dcterms:identifier`                   | Describes what the value represents              |
| Identifier value         | `550e8400-e29b-41d4-a716-446655440000` | Identifies something                             |
| Identifier specification | UUID                                   | Defines the structure and semantics of the value |

Thus:

```text
dcterms:identifier
        │
        └── UUID value
              │
              └── 550e8400-e29b-41d4-a716-446655440000
```

Dublin Core does not need to know that the value is a UUID for the value to be a valid identifier.

------

## 4. UUID as a URN

UUIDs can also be represented using the `urn:uuid:` URI scheme.

For example:

```text
urn:uuid:550e8400-e29b-41d4-a716-446655440000
```

This can be particularly useful when working directly with RDF, where resources are naturally identified using IRIs.

For example:

```turtle
<urn:uuid:550e8400-e29b-41d4-a716-446655440000>
    dcterms:title "Example resource" .
```

Here the UUID is functioning as the **identifier of the RDF resource itself**, rather than merely being metadata attached to another identifier.

This is a meaningful distinction.

### UUID as metadata

```turtle
<https://example.org/resource/example>
    dcterms:identifier "550e8400-e29b-41d4-a716-446655440000" .
```

### UUID as the resource identifier

```turtle
<urn:uuid:550e8400-e29b-41d4-a716-446655440000>
    dcterms:title "Example resource" .
```

Both approaches are valid, but they serve different purposes.

------

## 5. Recommended approach for front matter

For a Markdown-based content system using YAML front matter, a simple representation is usually best:

```yaml
---
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
  title: "Example Resource"
---
```

If your front-matter implementation uses flattened names, this could instead be:

```yaml
---
dcterms.identifier: "550e8400-e29b-41d4-a716-446655440000"
dcterms.title: "Example Resource"
---
```

The exact YAML structure depends on the metadata-processing system, since YAML itself does not define Dublin Core syntax.

------

## 6. Should the metadata say that the identifier is a UUID?

Usually, **not necessarily**.

If the UUID format is obvious from the value and your application already knows that `dcterms:identifier` contains UUIDs, there is little benefit in adding another field.

For example:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

is perfectly reasonable.

However, if your system supports multiple identifier schemes, explicitly recording the identifier scheme can be useful.

For example, a project-specific namespace could contain:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"

uc:
  identifierType: "uuid"
```

where `uc:` represents your own application's metadata namespace.

The important point is that `uc:identifierType` would be **application metadata**, not a Dublin Core property.

------

## 7. Avoid inventing `dcterms:uuid`

It may be tempting to write:

```yaml
dcterms:
  uuid: "550e8400-e29b-41d4-a716-446655440000"
```

but this creates the appearance that `dcterms:uuid` is an official DCTERMS term.

It isn't.

Prefer:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

If the distinction between UUIDs and other identifiers matters to your application, add a property in your own namespace.

------

## 8. UUIDs and persistent identifiers

A UUID provides a useful **syntactic mechanism for generating identifiers that are extremely unlikely to collide**.

However, UUID uniqueness should not be confused with persistence.

For example:

```text
550e8400-e29b-41d4-a716-446655440000
```

can be a stable identifier for a resource, but nothing about the UUID itself guarantees that:

- the resource will continue to exist;
- the identifier will remain resolvable;
- the identifier will continue to refer to the same resource;
- users will be able to discover the resource from the identifier.

Consequently, a UUID and a persistent Web identifier such as an HTTP URI can serve different purposes.

------

## 9. A useful metadata pattern

For a content-management system, one practical pattern is:

```yaml
---
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
  title: "Example Resource"
  creator: "Christopher Steel"
  created: "2026-08-10"
  modified: "2026-08-10"
---
```

The UUID provides a stable internal identifier while the other DCTERMS properties describe the resource.

If the resource is also published on the Web, it can have a URI:

```text
https://example.org/resources/example-resource
```

while retaining its UUID:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

The two identifiers need not be the same.

------

## 10. Recommended model

For a modern metadata system using Dublin Core/DCTERMS, Markdown, YAML front matter, and UUIDs, I would recommend this conceptual model:

```text
Resource
│
├── Web URI
│   └── https://example.org/resource/example
│
├── dcterms:identifier
│   └── 550e8400-e29b-41d4-a716-446655440000
│
└── Other DCTERMS metadata
    ├── dcterms:title
    ├── dcterms:creator
    ├── dcterms:created
    ├── dcterms:modified
    └── dcterms:type
```

This keeps the roles clean:

- **URI/IRI** — identifies the Web/RDF resource.
- **`dcterms:identifier`** — records an identifier for the resource.
- **UUID** — supplies a particular identifier format.
- **DCTERMS** — supplies standardized metadata semantics.
- **Application namespace** — handles project-specific metadata that DCTERMS does not define.

------

## Conclusion

There is no standard `dcterms:uuid`.

The most interoperable representation is:

```yaml
dcterms:
  identifier: "550e8400-e29b-41d4-a716-446655440000"
```

If working directly with RDF, a UUID can alternatively form the resource's identifier:

```text
urn:uuid:550e8400-e29b-41d4-a716-446655440000
```

For a Markdown/front-matter system, **`dcterms:identifier` containing a UUID is the simplest and most standards-aligned choice**.

The key principle is:

> **DCTERMS describes the identifier; UUID defines the identifier's format.**

## References

- Dublin Core Metadata Initiative. (2020). *DCMI metadata terms*.
- Dublin Core Metadata Initiative. (2020). *Dublin Core metadata element set, version 1.1*.
- IETF. (2024). *Universally unique identifiers (UUIDs)* (RFC 9562).
- IETF. (2005). *Uniform resource identifiers (URI): Generic syntax* (RFC 3986).

If you're building this into your **Universal Cake metadata/front-matter model**, I would also consider defining a small, consistent identifier policy covering `dcterms:identifier`, UUID version, URI/URN usage, and whether identifiers should ever be regenerated.
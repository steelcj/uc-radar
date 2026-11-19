# WEMI for Files

**Version.** v0.0.1, 2026-10-30. Draft.

## Comprehension

Two files. The English version (cake.md) was created first and is therefore the "Work". The French version is a translation of the original work and is therefore an "Expression" of the original work.

```bash
en-ca/docs/cake.md
fr-ca/docs/fromage.md
```

If we had cake.pdf and fromage.pdf what would they be?

## Understanding

The puzzle has a trap in its setup, and it is the most common WEMI mistake: the English file is not the work. No file is ever the work.

The work is the abstract idea, "cake", independent of any language or wording. It has no text of its own. Once you write it down in particular words in a particular language, you have an expression ([Riva et al., 2017](#ref-riva-2017))<a id="cite-riva-2017"></a>. So:

- The English text is an expression of the work: the **original** expression, but still an expression.
- The French text is another expression of the same work.
- Being first does not promote the English text to work status. That history is a relationship between the two expressions: the French one **derives from** the English one (a translation, LRM-R24).

Then the files themselves. A `.md` file is not the expression either. It embodies the expression in a particular format, which makes it a **manifestation**. So with four files:

| File | Level | Embodies |
| --- | --- | --- |
| `en-ca/docs/cake.md` | manifestation | English expression |
| `en-ca/docs/cake.pdf` | manifestation | English expression |
| `fr-ca/docs/fromage.md` | manifestation | French expression |
| `fr-ca/docs/fromage.pdf` | manifestation | French expression |

The Markdown and the PDF are siblings: two embodiments of the same text in different formats. The PDF is not "lower" than the Markdown just because it was generated from it. Generation is a production relationship between manifestations, not a change of level.

The **item** level is a particular copy. The `cake.pdf` on your laptop and the one on the web server are two items of one manifestation. In practice, items only matter if you care about custody, meaning who holds which copy.

What this means for SAT is that the work exists only as an identifier (and maybe a directory or sidecar), never as a file. Every file you can open is a manifestation, and what it embodies is an expression.

## What does this look like in practice?

Here are the WEMI statements for each file. Each file gets its own manifestation UUID. The `{ value, overridable }` wrapper is left out so the structure is easier to read.

**`en-ca/docs/cake.md`**

```yaml
manifestation:
  id: "urn:uuid:8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47"
  format: "text/markdown"
  embodies:
    expression:
      id: "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
      language: "en-ca"
      derivesFrom: []
      realises:
        work:
          id: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
```

**`en-ca/docs/cake.pdf`**

```yaml
manifestation:
  id: "urn:uuid:1d9f5c3a-6b28-4e70-8f41-a7c0e2d95b16"
  format: "application/pdf"
  producedFrom: "urn:uuid:8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47"
  embodies:
    expression:
      id: "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
      language: "en-ca"
      derivesFrom: []
      realises:
        work:
          id: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
```

**`fr-ca/docs/fromage.md`**

```yaml
manifestation:
  id: "urn:uuid:5a7e0b94-3d16-4c82-b5f9-0e1c8a4d7f23"
  format: "text/markdown"
  embodies:
    expression:
      id: "urn:uuid:c27e9a15-5f3d-4b81-a6c0-91d4e7b2f058"
      language: "fr-ca"
      derivesFrom:
        - "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
      realises:
        work:
          id: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
```

**`fr-ca/docs/fromage.pdf`**

```yaml
manifestation:
  id: "urn:uuid:e0c63f18-92a5-4d07-a3be-6f4d1b9c2e85"
  format: "application/pdf"
  producedFrom: "urn:uuid:5a7e0b94-3d16-4c82-b5f9-0e1c8a4d7f23"
  embodies:
    expression:
      id: "urn:uuid:c27e9a15-5f3d-4b81-a6c0-91d4e7b2f058"
      language: "fr-ca"
      derivesFrom:
        - "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
      realises:
        work:
          id: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
```

A few things stand out once it is laid out like this:

- **One work ID appears in all four files.** That shared ID is all that ties the four files together.
- **The French `derivesFrom` points at the English expression, not at `cake.md`.** The translation came from the English text, not from a particular file format.
- **`producedFrom` is a manifestation-to-manifestation link.** It records that the PDF was generated from the Markdown, which is a production fact, not a WEMI level.
- **The expression block is identical in the `.md` and the `.pdf`.** Each expression's facts are written twice. That is a hint that the expression's facts should be stored once, and each manifestation should only point at it by ID.

## Storing each fact once

Following that last point, each file shrinks to its own ID, its format and a reference to its expression. The expression's facts (its language, what it derives from, which work it realises) are stated once, in the sidecar of the authored source manifestation, the `.md` file. An expression has no file of its own in SAT, so the file someone actually writes in is its natural home.

```yaml
# en-ca/docs/.cheese.md.assets/content/metadata/wemi/overrides/wemi.yml
level: "manifestation"
manifestation: "urn:uuid:8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47"
format: "text/markdown"
expression:
  id: "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
  language: "en-ca"
  derivesFrom: []
  work: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
```

A generated manifestation only points:

```yaml
level: "manifestation"
manifestation: "urn:uuid:1d9f5c3a-6b28-4e70-8f41-a7c0e2d95b16"
format: "application/pdf"
producedFrom: "urn:uuid:8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47"
expression: "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
```

## Where the record lives

Where the record goes depends on whether a person authored the file.

**Authored files get a sidecar.** `cake.md` is written by a person, so its record lives in `overrides/`, beside it. A PDF typeset by hand by a designer is also authored source, and gets a sidecar in the same way.

**Generated files get no sidecar in the source tree.** Putting a build-written record in `en-ca/docs/` mixes generated files into the authored tree. Instead, the build computes the record and writes it into the artifact itself: front matter for HTML, XMP for PDF ([International Organization for Standardization, 2019](#ref-iso-2019))<a id="cite-iso-2019"></a>. The metadata then travels with the file wherever it is downloaded.

## What can be recomputed

Going through the generated PDF's record field by field:

| Field | Recomputed? | From |
| --- | --- | --- |
| `level` | Yes | Anything the build produces is a manifestation. It is a constant |
| `format` | Yes | The build rule. A rule that turns Markdown into PDF produces `application/pdf` |
| `producedFrom` | Yes | The build rule's input, whose manifestation ID is in its sidecar |
| `expression` | Yes | Copied from the source's sidecar. Conversion does not change the text, so the PDF embodies whatever expression its source embodies |
| `manifestation` | **Only if deterministic** | A random UUID minted at build time would give the PDF a new identity on every rebuild |

The manifestation ID is the only fact that belongs to the PDF itself. It can still be recomputed if it is minted **deterministically**, as a name-based (version 5) UUID ([Davis et al., 2024](#ref-davis-2024))<a id="cite-davis-2024"></a> computed from the source manifestation ID and the build variant:

```text
namespace: 8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47   (cheese.md)
name:      application/pdf;a4
result:    urn:uuid:aca865de-d4aa-5dab-a5e3-26a6817b585f
```

The same inputs always produce the same ID. Two consequences follow:

- **The build variant is part of the name.** An A4 PDF and a US Letter PDF of the same Markdown are two manifestations and must not collide.
- **A PDF edited by hand stops being derivable.** It is now authored, so it gets a random ID and a sidecar in `overrides/`.

The rule: **store what cannot be recomputed.** For a generated file, that is nothing, as long as its ID is computed rather than minted at random.

## When values change

The record is computed on every build. It only changes when one of its inputs changes.

| Event | What changes |
| --- | --- |
| `cake.md` moves to another directory, archive or SAT instance | Nothing. No input depends on position |
| `cake.md` is replaced and gets a new manifestation ID | `producedFrom` and the derived manifestation ID |
| `cake.md` is reassigned to a different expression | `expression` |
| The build variant or format changes | `format` and the manifestation ID |
| The text of `cake.md` is edited | Nothing |

The last row is the hard one. The PDF's bytes change but its identity does not. LRM treats a substantive revision as a new expression, so the answer is a human decision rather than a build decision: a typo fix keeps everything, and a real revision is marked by a person minting a new expression ID that `derivesFrom` the old one. The build never has to decide.

## Versioned files

A versioned file name settles the question up front. Take `research-paper-v0.0.1.pdf`. The version in its name says it is a fixed snapshot, so its WEMI facts are determined when it is created and never change:

- **Move, copy, rename the folder:** nothing changes.
- **Edit the paper:** you produce `research-paper-v0.0.2.pdf`, a new manifestation, most likely of a new expression that `derivesFrom` the v0.0.1 expression.
- **Lose the file and rebuild from the same v0.0.1 source:** with a deterministic ID, the rebuilt file gets the same identifiers.

The only remaining case is correcting a mistake, such as v0.0.1 recorded against the wrong work.

## Resolved or immutable

A record written once and never changed looks like a resolved value. In SAT, `resolved/` means computed from inputs and safe to delete, because running the resolver again gives the same answer. That holds only if the inputs are as fixed as the output:

- **The v0.0.1 source is kept** (`research-paper-v0.0.1.md`, or a tagged commit). Recomputing always gives the same answer. The record is truly resolved.
- **The source moves on** (one `research-paper.md` that now holds v0.0.2). Recomputing would describe v0.0.2, not the PDF you hold. The record cannot be regenerated, so it is no longer a cache. It is a historical record a machine happened to write, and it must be kept.

That second case needs a state of its own:

| State | Written by | Can it change? | Safe to delete? |
| --- | --- | --- | --- |
| `defaults/`, `overrides/` | A person | Yes | No |
| `resolved/` | The resolver | Recomputed at will | Yes |
| `immutable/` | The resolver, once | No | No |

## Issues immutability raises

An `immutable/` state leads to three questions, left open in this draft.

**Correction.** If a record was written against the wrong work, it cannot be fixed in place. A correction would have to be appended and supersede the original, and readers would need to know to look for it.

**Movement.** A move re-derives positional facts. An immutable record that captured positional facts at creation would be wrong after a move, which suggests it should hold only facts that never depend on position.

**Enforcement.** "Immutable" has to mean something on disk: read-only permissions, `chattr +i`, a content hash checked by the reader, or version control history. Each behaves differently when an archive migrates to another SAT instance.

## References

<a id="ref-davis-2024"></a>Davis, K., Peabody, B., & Leach, P. (2024). *Universally unique IDentifiers (UUIDs)* (RFC 9562). Internet Engineering Task Force. https://doi.org/10.17487/RFC9562
[Return to citation](#cite-davis-2024)

<a id="ref-iso-2019"></a>International Organization for Standardization. (2019). *Graphic technology: Extensible metadata platform (XMP), Part 1: Data model, serialization and core properties* (ISO Standard No. 16684-1:2019). https://www.iso.org/standard/75163.html
[Return to citation](#cite-iso-2019)

<a id="ref-riva-2017"></a>Riva, P., Le Bœuf, P., & Žumer, M. (2017). *IFLA Library Reference Model: A conceptual model for bibliographic information* (Rev. ed.). International Federation of Library Associations and Institutions. https://www.ifla.org/files/assets/cataloguing/frbr-lrm/ifla-lrm-august-2017_rev201712.pdf
[Return to citation](#cite-riva-2017)

## Changelog

| Version | Status | Notes |
| --- | --- | --- |
| 0.0.1 | Draft | First draft. WEMI applied to files: no file is the work, every file is a manifestation; expression facts stored once in the authored source's sidecar; generated manifestations embed their record and use deterministic IDs; versioned files; `immutable/` as a fourth state, with three open issues. |

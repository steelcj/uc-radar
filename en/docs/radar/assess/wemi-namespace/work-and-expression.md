# Work and Expression in SAT

This document explains how SAT uses two properties, `sat:work` and `sat:expression`, to record which pieces of content belong together and how they differ. Together they let a site offer the same content in different languages, reading levels, and forms, such as sign language or audio, and let a reader move between them.

## The idea in one paragraph

Every piece of content is an expression of a work. The work is the abstract idea, for example "how to prepare for a power outage". An expression is one particular realization of that idea: the Canadian French text written for a Grade 7 reading level, the English technical text, an ASL video. SAT gives each work and each expression a UUID, and every content file carries both. Files that share a `sat:work` value are versions of the same thing. Files that share a `sat:expression` value are the same version in different formats.

## Where the terms come from

Work and Expression are the first two levels of WEMI (Work, Expression, Manifestation, Item), introduced in the *Functional Requirements for Bibliographic Records* report <a id="cite-ifla-1998"></a>([IFLA Study Group on the Functional Requirements for Bibliographic Records, 1998](#ref-ifla-1998)) and carried forward in the IFLA Library Reference Model, which superseded it <a id="cite-riva-2017"></a>([Riva et al., 2017](#ref-riva-2017)). In that model, a work is the intellectual or artistic content of a distinct creation, and an expression is a specific combination of signs that conveys it. A work is realized through one or more expressions, and each expression realizes exactly one work.

SAT uses these two levels and no others. The lower levels, Manifestation and Item, describe file formats and individual copies. SAT already knows those facts directly from the files themselves: their paths, formats, and checksums. Work and expression are different. No tool can work out from the bytes that two files are versions of the same idea, so these are the two facts that people must record.

## The two properties

| Property | Identifies | Value | Shared by |
|---|---|---|---|
| `sat:work` | The work: the abstract idea the content conveys | A `urn:uuid:` | Every expression of that work, in every language, reading level, and form |
| `sat:expression` | The expression: one particular realization of the work | A `urn:uuid:` | Every file of that expression, in every format |

Both values are version 4 (random) UUIDs written as URNs, the kind produced by `uuidgen -r`. They can be minted locally and offline with no registry, and the chance of two colliding is negligible <a id="cite-davis-2024"></a>([Davis et al., 2024](#ref-davis-2024)). The values carry no meaning of their own. A UUID never encodes a language, a level, or a title; those belong in other metadata.

Dublin Core has no terms for either concept, so following SAT's name-parity rule, both are coined in the `sat:` namespace.

## How a file carries them

Each content file carries one metadata block with both properties:

```yaml
sat:work: urn:uuid:ad392bfd-eb04-4faf-8cf3-f70340768292
sat:expression: urn:uuid:ffdb4099-dd03-4b68-bb17-0f61008170cd
```

The canonical source file of an expression, usually its Markdown, is where these values are set. Outputs generated from it, such as HTML or PDF produced by SAT egress, receive the same two values automatically. Nobody edits them by hand in derived files.

There is no separate record for a work or an expression. A work exists in SAT as a UUID that several files share; if a title for the work is ever needed, it is taken from its original expression.

Because the model has no item level, both identifiers stay true for every copy of a file. They can safely travel inside the file itself, for example as embedded XMP metadata <a id="cite-iso-2019"></a>([International Organization for Standardization, 2019](#ref-iso-2019)), and every copy will still identify the right work and expression.

## When to mint a new identifier

Minting is a human decision. Automation copies existing values to derived files; it never invents new ones.

| Change | `sat:work` | `sat:expression` |
|---|---|---|
| Brand new content | New | New |
| Translation into another language | Same | New |
| Rewrite for another reading level (technical, Grade 7, Grade 3) | Same | New |
| Change of form: ASL video, audio reading, braille | Same | New |
| Substantive revision of the text | Same | New |
| Conversion to another format (Markdown to HTML or PDF) | Same | Same |
| Re-export or rebuild with only technical differences | Same | Same |
| Typo or formatting fix | Same | Same |
| Commentary, review, or summary about the content | New | New |

Two of these rows are deliberate policy choices and are worth stating plainly.

Reading-level versions stay under the same work. A simplified version leaves things out by design, and a strict reading of the Library Reference Model could treat a substantial adaptation as a new work. SAT keeps them together on purpose, because the point of the model is to let readers find the version that suits them, and that only works if every version shares one `sat:work`.

Minor fixes keep the same expression. A typo correction does not change what the text conveys. Where the line falls between a fix and a substantive revision is an editorial judgment, and each site should agree on it.

## How it is used

### Finding expressions

From any file, SAT can find every other expression of the same work by looking for the same `sat:work` value. This is the whole mechanism behind switching between versions.

### The expression switcher

A language switcher is the familiar case: a reader on the English page chooses Français and lands on the French page of the same content. `sat:work` generalizes this to every way expressions can differ. The same mechanism serves a reader who needs a simpler text, one who wants the technical detail, a Deaf reader who wants ASL, and a reader who cannot read text at all and needs audio.

The two properties say which files belong together. The labels on the switcher come from the metadata that describes each expression, such as `dcterms:language`, which already lives in SAT's Dublin Core metadata cascade. That metadata is outside this model; the model only guarantees that the switcher knows which files to offer.

At build time, a static site generator such as Eleventy can collect every expression of a work and write the switcher links into each page. The switcher then works offline and needs no server.

## Worked example: a trilingual site with three reading levels

A site is published in Canadian English, Canadian French, and a third language (Spanish here, for illustration), each at three reading levels: technical, Grade 7, and Grade 3.

Take one page, "Preparing for a power outage". It is one work:

`sat:work: urn:uuid:ad392bfd-eb04-4faf-8cf3-f70340768292`

It can have up to nine expressions, one for each combination of language and reading level:

| | Technical | Grade 7 | Grade 3 |
|---|---|---|---|
| `en-ca` | `urn:uuid:81549480-b278-4bc7-ad4f-1f8505840ffb` | `urn:uuid:20c3a2ab-15b6-4899-a53b-49e726dd0f01` | `urn:uuid:d091eba1-e635-4d04-892d-56b2fd64c889` |
| `fr-ca` | `urn:uuid:712d556f-c883-4e5d-89a0-78e89a92c98c` | `urn:uuid:ffdb4099-dd03-4b68-bb17-0f61008170cd` | `urn:uuid:a5dd0110-250a-4e0c-a7e5-6e7357c46251` |
| `es` | `urn:uuid:8849501c-8fb9-4ea5-9dc6-52432df396a0` | `urn:uuid:8002188e-6104-46b3-88de-71acd19c986e` | not yet written |

All nine share the same `sat:work`. Each has its own `sat:expression`.

The French Grade 7 page exists as a Markdown file in the archive and an HTML file on the web server. Both carry the same block:

```yaml
sat:work: urn:uuid:ad392bfd-eb04-4faf-8cf3-f70340768292
sat:expression: urn:uuid:ffdb4099-dd03-4b68-bb17-0f61008170cd
```

A reader on that page sees the other expressions of the work and can move to any of them. The Spanish Grade 3 cell is empty, which is normal: a work does not need every combination, and the switcher simply offers the expressions that exist.

Across a 30-page site, this gives 30 works and up to 270 expressions, each page carrying only its two identifiers.

## Checks SAT can run

Because there is no central register, SAT checks consistency at build time:

- Every content file with a `sat:expression` also has a `sat:work`.
- All files sharing a `sat:expression` share the same `sat:work`. An expression belongs to exactly one work.
- Every `urn:uuid:` value is well formed.

## What this model does not cover

The model records which content belongs together and nothing more. The following are recorded elsewhere, in SAT's Dublin Core metadata:

- The language of an expression
- Its reading level, audience, or form (text, audio, video, sign language)
- Which expression another was translated or adapted from
- File formats, locations, and checksums

Keeping the model to two identifiers keeps the human part small: someone decides "this is a new work" or "this is a new expression of that work", and everything else follows from the files and their metadata.

## References

<a id="ref-davis-2024"></a>Davis, K., Peabody, B., & Leach, P. (2024). *Universally unique IDentifiers (UUIDs)* (RFC 9562). Internet Engineering Task Force. https://doi.org/10.17487/RFC9562
[Return to citation](#cite-davis-2024)

<a id="ref-ifla-1998"></a>IFLA Study Group on the Functional Requirements for Bibliographic Records. (1998). *Functional requirements for bibliographic records: Final report*. K. G. Saur. https://repository.ifla.org/handle/20.500.14598/811
[Return to citation](#cite-ifla-1998)

<a id="ref-iso-2019"></a>International Organization for Standardization. (2019). *Graphic technology: Extensible metadata platform (XMP), Part 1: Data model, serialization and core properties* (ISO Standard No. 16684-1:2019). https://www.iso.org/standard/75163.html
[Return to citation](#cite-iso-2019)

<a id="ref-riva-2017"></a>Riva, P., Le Bœuf, P., & Žumer, M. (2017). *IFLA Library Reference Model: A conceptual model for bibliographic information* (Rev. ed.). International Federation of Library Associations and Institutions. https://www.ifla.org/files/assets/cataloguing/frbr-lrm/ifla-lrm-august-2017_rev201712.pdf
[Return to citation](#cite-riva-2017)

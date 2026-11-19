# WEMI for Digital Content: A Practical Guide

This guide explains how to apply the four WEMI levels (Work, Expression, Manifestation, Item) to digital files. It is a working companion to the paper *WEMI for Digital Content* and keeps the same scope: the four levels and the chain that links them, not the wider set of relationships in the IFLA Library Reference Model.

## Where WEMI comes from

WEMI was introduced in the *Functional Requirements for Bibliographic Records* report as the core of its first group of entities, the products of intellectual or artistic endeavour <a id="cite-ifla-1998"></a>([IFLA Study Group on the Functional Requirements for Bibliographic Records, 1998](#ref-ifla-1998)). FRBR has since been superseded by the IFLA Library Reference Model (LRM), which consolidates FRBR with its sister models and keeps WEMI as its central chain <a id="cite-riva-2017"></a>([Riva et al., 2017](#ref-riva-2017)). This guide uses LRM as the authority and FRBR for background.

Both models were written for library catalogues, where the default carrier is a physical object. Digital content keeps the model intact but shifts where the hard decisions fall, mostly at the Manifestation and Item levels.

## The four levels in brief

| Level | What it is | Digital example | Exists as |
|---|---|---|---|
| Work | The intellectual or artistic content of a distinct creation, independent of any wording | "the WEMI guide" as an idea | Abstract only, never stored |
| Expression | A specific combination of signs that conveys the work: a particular text, language, or version | The Canadian English text of the guide, version 1.0 | Abstract, but definite: you can say exactly what words it contains |
| Manifestation | The set of all carriers sharing the same content and the same production characteristics | The guide as a UTF-8 Markdown file; the same text rendered as HTML; the same text exported as PDF | A specification that any number of files can match |
| Item | A single exemplar of a manifestation | The `.md` file on the laptop; the copy in the GitHub repository; the HTML file on the web server | A concrete file at a concrete location |

The links run in one direction. A work is realized through one or more expressions, and (in LRM) a work comes into being with its first expression. Each expression realizes exactly one work. A manifestation embodies one or more expressions (an anthology embodies several). Each item exemplifies exactly one manifestation ([Riva et al., 2017](#ref-riva-2017)).

## What changes with digital content

### Items are cheap and identical

A printed book's copies are physically distinct and each can be damaged, annotated, or lost on its own. A digital copy is bit-for-bit identical to its source, so nothing inside the file tells one item from another. What distinguishes digital items is where they live and what happens to them: the storage location, its custody, its access rights, and its history.

Practical rule: an item is a file instance at a location. Copy a file and you have created a new item of the same manifestation. Two items with the same checksum belong to the same manifestation, but a checksum never identifies an item, because every copy shares it.

### Manifestations multiply through pipelines

A content pipeline routinely produces several renderings from one source. Markdown converted to HTML, the same Markdown exported to PDF or EPUB: each is a different manifestation of the same expression, because the content is unchanged while the form of the carrier is not.

The harder cases are byte-level differences that do not touch content: a PDF re-exported with a new timestamp, an image re-saved with different compression, an HTML build with a changed stylesheet. LRM defines a manifestation by both its content and the production plan for its carriers ([Riva et al., 2017](#ref-riva-2017)), so the question becomes whether the production plan changed. That is a policy decision you have to make and record, not something a tool can infer from the bytes.

### Expressions change through editing

FRBR treats a translation, a revision, or a change of form such as text to spoken word as a new expression of the same work ([IFLA Study Group on the Functional Requirements for Bibliographic Records, 1998](#ref-ifla-1998)). For files under version control this raises the obvious question: does every commit create a new expression? Usually not. A typo fix and a substantive revision are both edits, but only the second changes what the text conveys. Again, this is a threshold to set by policy, and it is the level where human judgment is most needed.

### The work is never a file

No file is ever the work. The work is what you point to when you say "all versions and translations of this guide." In practice it exists in your system as an identifier and a small set of descriptive metadata that every expression links to.

## Deciding which level changed

Use this table when a file changes or a new file appears.

| Situation | Usual result | Needs a policy decision? |
|---|---|---|
| File copied to a new location | New item, same manifestation | No |
| File moved, not copied | Same item, new location | Sometimes, if location is part of item identity in your system |
| Same text rendered to another format (Markdown to HTML or PDF) | New manifestation, same expression | No |
| Same format re-exported with only technical differences (timestamps, compression, build settings) | Same or new manifestation | Yes |
| Typo or formatting correction | Same or new expression | Yes |
| Substantive revision | New expression, same work | Rarely |
| Translation (for example `en-ca` to `fr-ca`) | New expression, same work | No |
| Adaptation that changes the intellectual content substantially | New work | Yes |

The rows marked "Yes" are the points where automation should stop and ask, or apply a rule that a person wrote down in advance.

## Identifying each level

Give every work, expression, and manifestation its own persistent identifier, and give every item one as well if you need to track copies individually. A `urn:uuid:` value works well for this because it can be minted locally, offline, with no registry: a version 4 (random) UUID, the kind `uuidgen -r` produces, has negligible collision risk <a id="cite-davis-2024"></a>([Davis et al., 2024](#ref-davis-2024)).

Some rules that follow from the levels:

- Never derive an item identifier from a checksum. Every copy shares the checksum, so it identifies content, not the item.
- Never reuse an identifier across levels. The expression and its Markdown manifestation are different entities even when there is only one of each.
- Mint the work identifier when the first expression is created, matching LRM's view that a work begins with its first expression.

## Where metadata can live

Metadata about a file can sit beside it (a sidecar file or database record) or inside it. XMP is the main standard for the second approach: it serializes metadata in RDF and embeds it in the file itself, so it travels with every copy <a id="cite-iso-2019"></a>([International Organization for Standardization, 2019](#ref-iso-2019)).

That portability has a consequence for WEMI. Anything embedded in a file is duplicated whenever the file is copied, so embedded metadata can safely carry work, expression, and manifestation identifiers, but it cannot carry item identity. Item-level facts (location, custody, access, history of that particular copy) belong outside the file, in a sidecar or a register that is specific to the location.

## Worked example: this guide

| Level | Entity | Identifier |
|---|---|---|
| Work | WEMI for Digital Content: A Practical Guide | one `urn:uuid:` for the work |
| Expression | Canadian English text, version 1.0 | one `urn:uuid:`; a later French translation gets its own |
| Manifestation | UTF-8 Markdown source | one `urn:uuid:` |
| Manifestation | HTML page rendered by the site build | one `urn:uuid:` |
| Manifestation | PDF export | one `urn:uuid:` |
| Item | The Markdown file in the local working tree | location-specific record |
| Item | The Markdown file in the GitHub repository | location-specific record |
| Item | The HTML file on the web server | location-specific record |

If the text is then revised substantively, the result is a new expression of the same work, and each rendering of the revised text is a new manifestation with its own items. If only the site stylesheet changes, the expression is untouched, and whether the HTML becomes a new manifestation depends on the production-plan policy described above.

## Applying it in SAT

These mappings are starting points for the paper, not settled decisions.

- A file at a path inside a SAT instance is an item. Two SAT instances holding the same file hold two items.
- A language archive such as `en-ca` groups expressions by language, which is one of the dimensions that separates expressions of a work.
- Format variants of the same content (Markdown source, HTML or PDF egress) are manifestations of one expression.
- `.{level}.assets/` sidecars are a natural home for item-level metadata, with one caveat: when a collection or archive is migrated to another SAT instance, its sidecars travel with it, so any item-level facts inside them need to be updated, or the migration needs a rule about whether moved files keep their item identity.
- The work level has no directory of its own; it is an identifier that expressions in different language archives share.

## Checklist

- Decide and write down the threshold for a new expression (what counts as a substantive revision).
- Decide and write down the threshold for a new manifestation (what counts as a change of production plan).
- Mint a `urn:uuid:` for each work, expression, and manifestation.
- Keep item identity and item facts outside the file.
- Flag the policy-dependent cases for a person rather than letting automation guess.

## References

<a id="ref-davis-2024"></a>Davis, K., Peabody, B., & Leach, P. (2024). *Universally unique IDentifiers (UUIDs)* (RFC 9562). Internet Engineering Task Force. https://doi.org/10.17487/RFC9562
[Return to citation](#cite-davis-2024)

<a id="ref-ifla-1998"></a>IFLA Study Group on the Functional Requirements for Bibliographic Records. (1998). *Functional requirements for bibliographic records: Final report*. K. G. Saur. https://repository.ifla.org/handle/20.500.14598/811
[Return to citation](#cite-ifla-1998)

<a id="ref-iso-2019"></a>International Organization for Standardization. (2019). *Graphic technology: Extensible metadata platform (XMP), Part 1: Data model, serialization and core properties* (ISO Standard No. 16684-1:2019). https://www.iso.org/standard/75163.html
[Return to citation](#cite-iso-2019)

<a id="ref-riva-2017"></a>Riva, P., Le Bœuf, P., & Žumer, M. (2017). *IFLA Library Reference Model: A conceptual model for bibliographic information* (Rev. ed.). International Federation of Library Associations and Institutions. https://www.ifla.org/files/assets/cataloguing/frbr-lrm/ifla-lrm-august-2017_rev201712.pdf
[Return to citation](#cite-riva-2017)

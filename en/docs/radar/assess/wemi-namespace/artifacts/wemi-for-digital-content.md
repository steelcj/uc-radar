# WEMI for Digital Content

**Version.** v0.0.2, 2026-10-30. Draft.

**Scope.** The four WEMI levels (work, expression, manifestation, item) and the three links that join them, applied to files in a digital archive.

## The four levels and three links

IFLA's Library Reference Model describes a resource at four levels ([Riva et al., 2017](#ref-riva-2017))<a id="cite-riva-2017"></a>, inherited from FRBR ([IFLA Study Group on the Functional Requirements for Bibliographic Records, 1998](#ref-ifla-1998))<a id="cite-ifla-1998"></a>:

| Level | What it is | In a digital archive |
| --- | --- | --- |
| Work | The abstract idea, independent of language or wording | An identifier. Never a file or content itself |
| Expression | The idea in particular words, images or sounds in a particular language | The text. Not yet a file either |
| Manifestation | The expression embodied in a particular format | Every file you can open: a `.md`, a `.pdf`, an `.html` |
| Item | A particular copy of a manifestation | A file on a particular disk or server |

Three links join them, and they are the only relationships this paper uses:

- a work **is realised through** an expression
- an expression **is embodied in** a manifestation
- a manifestation **is exemplified by** an item

## Cake Example

### Comprehension

```bash
en-ca/recipes/cakes.md
en-ca/recipes/chocolate-cake.md
```

`cakes.md` is the general one, so it is the work, and `chocolate-cake.md` is an expression of it?

### Understanding

No. A chocolate cake recipe is not *cakes* written down in particular words. It is different content. These are **two works**, each with its own expression and its own manifestations.

```yaml
# en-ca/recipes/.cakes.md.assets/content/metadata/wemi/overrides/wemi.yml
level: "manifestation"
manifestation: "urn:uuid:71f0c3e8-a2d4-4b97-8e15-3a6d90b2c4f1"
format: "text/markdown"
expression:
  id: "urn:uuid:2d7a91c4-58e3-4f16-a0b9-c4e8312f7d60"
  language: "en-ca"
  work: "urn:uuid:9b2e4f70-3c1a-4d85-b6e2-7f0a1c93d548"
```

```yaml
# en-ca/recipes/.chocolate-cake.md.assets/content/metadata/wemi/overrides/wemi.yml
level: "manifestation"
manifestation: "urn:uuid:b83e5a10-4d6c-4f29-a7e1-2c9f68d0b354"
format: "text/markdown"
expression:
  id: "urn:uuid:0a6f3d29-c71e-4b58-9d42-e8b5170a3f6c"
  language: "en-ca"
  work: "urn:uuid:e4c18b52-9f7a-4e30-b2d6-58a1f0c7e943"
```

That is the whole WEMI answer, and it is the lesson: **WEMI is not a taxonomy.** "Chocolate cake is a kind of cake" is a statement about concepts. WEMI answers only one question: is this the same content in another form? Here it is not, so WEMI can say they are different works, and has nothing to say about how they relate.

## Example three: a trilogy

### Comprehension

```bash
en-ca/books/the-long-quiet/
  01-first-light/
  02-still-water/
  03-last-ember/
```

Each novel has a subdirectory, and each chapter is a file. How many works are there?

### Understanding

At least three: each novel is a work, with its own expressions and manifestations. Beyond that, the trilogy raises three questions that WEMI alone cannot settle.

**Is the trilogy itself a work?** If readers and publishers treat *The Long Quiet* as one creation, it is a work, and each novel is also a work. If it is only a label for three books by the same author, it is not. Both readings are defensible. The answer is a judgement.

**Are chapters works?** Not automatically. Splitting text into files is a decision about the manifestation, how the text is packaged, not about the work. The same novel built as one PDF is a single file. A chapter becomes a work only if someone needs to point at it on its own: cite it, publish it separately, or translate it separately. Otherwise the chapter files are pieces of one manifestation, and the novel's expression facts belong in the sidecar of the novel's directory, which is the node that stands for the whole text.

```yaml
# en-ca/books/the-long-quiet/.02-still-water.assets/content/metadata/wemi/overrides/wemi.yml
expression:
  id: "urn:uuid:3e91c7a0-2f58-4b6d-8a13-d70e45c9b2f6"
  language: "en-ca"
  work: "urn:uuid:c605b2d8-71e4-4a39-bf80-9a2d1e6c47f3"
```

**What does an omnibus embody?** A single volume printing all three novels is one manifestation. Which expression does it embody: three expressions at once, or one new expression, "the trilogy in English", that contains them? WEMI's links run one way, from expression to manifestation, and the omnibus strains them.

## Where a record lives

Whether a file gets a sidecar depends on whether a person wrote it.

**Authored files get a sidecar.** `meditate.md` is written by a person, so its record lives in `overrides/`, beside it. A PDF typeset by hand is also authored, and gets a sidecar the same way.

**Generated files carry their record inside them.** A build-written record in the source tree would mix generated files into authored ones. Instead the build writes the record into the artifact: front matter for HTML, XMP for PDF ([International Organization for Standardization, 2019](#ref-iso-2019))<a id="cite-iso-2019"></a>. The record then travels with the file wherever it is downloaded.

**Generated identifiers are computed, not minted.** Every field in a generated file's record can be recomputed from its source, except its own manifestation ID. A random UUID would give the file a new identity on every build. A name-based (version 5) UUID ([Davis et al., 2024](#ref-davis-2024))<a id="cite-davis-2024"></a>, computed from the source manifestation's ID and the build variant, is the same on every build:

```text
namespace: 8e4a2d61-0c7f-4b39-9a15-f3d2b6e80c47   (meditate.md)
name:      application/pdf;a4
result:    urn:uuid:aca865de-d4aa-5dab-a5e3-26a6817b585f
```

The variant is part of the name, because an A4 PDF and a US Letter PDF are two manifestations.

## Edits and versions

The hardest WEMI question in daily use is whether an edited text is still the same expression. A typo fix and a substantive revision look the same to a diff, but LRM treats a substantive revision as a new expression.

A **versioned file name** settles it up front. `research-paper-v0.0.1.pdf` is a fixed snapshot. Its WEMI facts are fixed when it is created: moving or copying it changes nothing, and editing the paper produces `research-paper-v0.0.2.pdf`, a new manifestation and most likely a new expression. An **unversioned** file name such as `meditate.md` leaves the question open, so someone has to answer it at the moment of the edit.

## What a machine can decide

The difficulty is not spread evenly across the four levels. It is concentrated at the top.

| Level | Can a machine decide it? | Why |
| --- | --- | --- |
| Item | Yes | A copy is a copy. Which disk or server holds it is observable |
| Manifestation | Yes | Every file is a manifestation, and its format can be read from it |
| Expression | Partly | A machine can tell that two files hold the same text. It cannot tell whether an edited text is still the same expression |
| Work | No | Whether two expressions realise the same work, or whether a trilogy is a work, is a judgement |

Every hard case in the three examples sits at the top two levels: the first text mistaken for the work, the general recipe mistaken for the work, the trilogy, the chapters, the omnibus, the edit. None of them is about manifestations or items.

That suggests a division of labour:

- **The machine handles manifestations and items completely,** including computed identifiers for generated files, so nobody has to think about them.
- **A person makes two decisions, each once, when content is created or changed:** is this a new work or an existing one, and is this change a new expression or the same one.
- **The machine suggests and the person confirms.** A new file in `fr-ca/` paired with an existing file in `en-ca/` suggests the same work. An edit defaults to the same expression unless someone says it is a revision. The tool never decides the work level alone.

The aim is not to automate WEMI, which is not possible. It is to ask for human judgement only where WEMI needs it, and only once.

## Deliberately out of scope

Each of these came up while working through the examples, and each is a relationship beyond the three WEMI links.

| Relationship | Where it came up |
| --- | --- |
| Translation and derivation between expressions | The French guide was translated from the English |
| Production between manifestations | The PDF was generated from the Markdown |
| Whole-part between works | A chocolate cake recipe in a collection of cake recipes; a novel in a trilogy |
| Sequence between works | *Still Water* follows *First Light* |
| Aggregation | The omnibus volume |
| Subject and classification | Chocolate cake is a kind of cake |

They are real and useful, and each can be added later. None of them is WEMI, and the four levels are challenging enough on their own.

## Open questions

**Whether a trilogy is a work.** The answer varies by case. What the tool should do when it cannot tell is not yet decided.

**What an omnibus embodies.** Three expressions in one manifestation, or one containing expression.

**Where the expression's facts live when there is no authored source file,** for example a text that exists only as a scanned PDF.

**How a person is asked.** The two human decisions need a moment and a place: a prompt at `sat` creation time, a field in the CMS, or a review step before publication.

## References

<a id="ref-davis-2024"></a>Davis, K., Peabody, B., & Leach, P. (2024). *Universally unique IDentifiers (UUIDs)* (RFC 9562). Internet Engineering Task Force. https://doi.org/10.17487/RFC9562
[Return to citation](#cite-davis-2024)

<a id="ref-ifla-1998"></a>IFLA Study Group on the Functional Requirements for Bibliographic Records. (1998). *Functional requirements for bibliographic records: Final report*. K. G. Saur. https://repository.ifla.org/handle/20.500.14598/811
[Return to citation](#cite-ifla-1998)

<a id="ref-iso-2019"></a>International Organization for Standardization. (2019). *Graphic technology: Extensible metadata platform (XMP), Part 1: Data model, serialization and core properties* (ISO Standard No. 16684-1:2019). https://www.iso.org/standard/75163.html
[Return to citation](#cite-iso-2019)

<a id="ref-riva-2017"></a>Riva, P., Le Bœuf, P., & Žumer, M. (2017). *IFLA Library Reference Model: A conceptual model for bibliographic information* (Rev. ed.). International Federation of Library Associations and Institutions. https://www.ifla.org/files/assets/cataloguing/frbr-lrm/ifla-lrm-august-2017_rev201712.pdf
[Return to citation](#cite-riva-2017)

## Changelog

| Version | Status | Notes |
| --- | --- | --- |
| 0.0.2 | Draft | Renamed from *WEMI for Files* to *WEMI for Digital Content*. Narrowed to the four levels and three links: removed `derivesFrom` and `producedFrom`, and listed every non-WEMI relationship as out of scope. Records reduced to one statement per fact. Added the cakes and trilogy examples. Added what a machine can decide and the two human decisions. Removed the storage-state discussion (`resolved/`, `immutable/`), which is SAT machinery rather than WEMI. |
| 0.0.1 | Draft | First draft, as *WEMI for Files*. |

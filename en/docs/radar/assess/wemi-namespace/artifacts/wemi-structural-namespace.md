# WEMI as the Structural Namespace: A Minimal Identity Spine for Movable Resources in SAT

**Version.** v0.1.0, 2026-10-29. Draft.

**Author.** Christopher Steel, Vishpala.

**Companions.** *SAT Sidecars: Config and Metadata* v0.0.12 and *SAT Configuration Cascade Specification* v0.2.2.

**Status.** Proposal. Nothing here is implemented. Five decisions are needed before it can be, and they are listed near the end rather than buried. Appendix A records every change from v0.0.1.

## Abstract

SAT holds an archive as a tree of files with metadata in sidecar directories, resolved through a configuration cascade. A resource held this way should be movable: relocatable within an archive, liftable from one SAT instance into another, re-rendered and re-translated, without anyone consulting a cataloguer. This paper argues that movement needs only a small structural spine, namely the WEMI level a node declares, the identity of the work it realises, the identity of the expression it is, and two relations, and that this spine belongs in a namespace of its own, `wemi`, beside the existing `dc` (description) and `sat` (operation) namespaces. It extends the `field_kinds` declaration with a level, so that the artifact a value generates into becomes data rather than prose. It gives the work record a home in the collection root's sidecar, defines movement as re-resolution rather than copy, adds `propose:` as an upward channel from content nodes to the work record, and maps the spine onto IFLA LRM, DCTERMS, BIBFRAME 2.0 and schema.org so that richer vocabularies can be added later as projections. Compared with the v0.0.1 draft, it corrects several internal contradictions: identity fields that would have cascaded, a namespace test that did not separate `wemi` from `dc`, intrinsic facts that would have been rewritten by a move, and a projection table with wrong LRM numbers and wrong BIBFRAME classes.

> So while it is possible to express WEMI using other metadata types, for example, with difficulty using Dublin Core with Dublin Core Terms extensions or using strictly dcterms the way in which it is expressed or when (in the past) or if it is expressed means that we must know the authors intent (leveraging the idea of WEMI) and in many cases this is not possible.
>
> When leveraging WEMI as a foundation for organising content with in a more formal, yet, feasable way. It is much better to do so using 

## Introduction

Content management pipelines usually treat metadata as an attribute of a location. A page's language, licence, section and order are read from where it sits, and moving the file changes them. This is convenient while nothing moves. It fails when a collection is lifted out of one SAT instance and placed in another, when a section is reorganised, or when a translation is added under a different language root: the facts that belonged to the text are silently replaced by the facts of the new location, and the relations between a text and its translations exist only in naming conventions.

Library cataloguing solved the underlying problem decades ago by separating the abstract work from its realisations and their physical embodiments. The FRBR model ([IFLA Study Group on the Functional Requirements for Bibliographic Records, 1998](#ref-ifla-1998))<a id="cite-ifla-1998"></a> and its successor, the IFLA Library Reference Model ([Riva et al., 2017](#ref-riva-2017))<a id="cite-riva-2017"></a>, call the four levels work, expression, manifestation and item, together WEMI. The full model, however, is designed to be maintained by cataloguers, and the archives Vishpala serves (individuals, small organisations, NGOs) do not have one.

This paper proposes the smallest subset of WEMI that movement requires, and a rule for adding the rest later. Its contributions are:

1. A `wemi` metadata namespace with five fields, and resolution rules under which identity never cascades.
2. A level tag on every declared field, which makes generation targets derivable.
3. A work record held as an index in the collection root's sidecar.
4. A definition of movement as re-resolution, with a pinning step that protects intrinsic facts.
5. A `propose:` channel that lets a content node address its work without crossing an ownership boundary.

## Background

### SAT sidecars and the cascade

SAT attaches a sidecar directory, `.{node}.assets/`, to every node. Inside it, each role the node plays has metadata namespaces, and each namespace has states: authored `defaults/` and `overrides/`, and a computed `resolved/`. Values cascade down the tree, a field may be locked with `overridable: false`, and a higher role can reach down with `amend:` to unlock or deprecate a row a lower one holds ([Steel, 2026a](#ref-steel-2026a))<a id="cite-steel-2026a"></a>. The `field_kinds` declaration in the SAT configuration states how each field behaves under resolution ([Steel, 2026b](#ref-steel-2026b))<a id="cite-steel-2026b"></a>. Two namespaces exist today: `dc`, holding Dublin Core description, and `sat`, holding terms SAT needs and DCTERMS has no word for. The current WEMI identity store is `sat:identifier` and `sat:isVersionOf`.

### The WEMI spine in IFLA LRM

LRM defines eleven entities (LRM-E1 to LRM-E11) and thirty-six relationships (LRM-R1 to LRM-R36). Four entities form the spine: work (E2), expression (E3), manifestation (E4) and item (E5), linked by realisation (R2), embodiment (R3) and exemplification (R4). Four further relationships matter here. In the LRM numbering, R18 is work whole-part, R22 is work transformation, R23 is expression whole-part, and R24 is expression derivation ([Bibliotheksverbund Bayern & Deutsche Nationalbibliothek, n.d.](#ref-dnb-lrm-tables))<a id="cite-dnb-lrm-tables"></a>. The numbering is easy to get wrong because the 2017 revision reordered the table and renumbered many relationships, reversing the direction of R24 in the process ([IFLA FRBR Review Group, 2017](#ref-ifla-2017-changes))<a id="cite-ifla-2017-changes"></a>. R24 relates two expressions of the same work; a derivation that produces a new work is a work transformation, R22 ([RDA Steering Committee Technical Working Group, 2021](#ref-rsc-2021))<a id="cite-rsc-2021"></a>. LRM also distinguishes whole-part (R18) from aggregation (R25), the relationship that applies when a manifestation gathers independent expressions, as an anthology or a website does.

### Why DCTERMS alone does not suffice

DCTERMS has relation terms (`isVersionOf`, `isPartOf`, `source`, `isFormatOf`) but no notion of level ([DCMI Usage Board, 2020](#ref-dcmi-2020))<a id="cite-dcmi-2020"></a>. `dcterms:isVersionOf` is the nearest term to the expression-to-work link, which is why SAT uses it today, but LRM's R2 is realisation, not versionhood. The R2, R3 and R4 chain has no DCTERMS counterpart. That gap is the reason a `wemi` namespace would exist in addition to other metdata spaces rather than using  `dcterms` or some other form of metadata to express WEMI and, if required by some organization or user, the addition of other significantly more detailed models regarding the organising of content in the future.

## The design principle

**Hold the minimum that movement requires. Let every richer vocabulary be a projection onto it.**

A document that knows its work, its expression and its level can be moved, reorganised, re-pathed, re-rendered and re-translated without anyone consulting a professional cataloguer.

At the same time the architecture allows for orgs or individuals who desire richer cataloging is not prevented from adding it in an intelligent and useful way that does not interfer with existing metadata and it's role in the operations of SAT before or after the addition.

### Why is works

A document that also knows its full agent graph, its SKOS concept scheme and its BIBFRAME class assignments can do the same, but it needs a librarian to maintain it, and that librarian is a person these archives do not have at this point in time.

#### Deferred richness and/or enhancement

The principle is safe because richness is deferred rather than rejected.

The namespace layer `metadata/{namespace}/{state}/` already exists, so adding `metadata/skos/`, `metadata/foaf/`, `metadata/bibframe/` or `metadata/lrm/` later is a declaration in `field_kinds` and a new directory, not a change to the resolver. A richer system is postponed until something needs it, which is the only honest basis and a resource conserving way for adopting one.

## Namespace partition

Three initial namespaces, three initial jobs, no overlap.

| Namespace | Job | Answers |
| --- | --- | --- |
| `wemi` | Structure | What kind of thing is this, which work does it realise, which expression is it, what does it derive from or belong to |
| `dc` | Description | What is it called, who made it, when, in what language, under what rights |
| `sat` | Operation | What the software needs that is neither: `slug` and resource-scoped axis values |

v0.0.1 gave a single test: *would this field still mean something if the document were moved to a different archive?* It answers yes for both identity and description, so it cannot separate `wemi` from `dc`. Two questions are needed, asked in order.

1. **Is it portable?** Would the value still be true of the resource in a different archive? If not, it is positional and belongs in `sat`, or, for collection-scoped positional facts such as `order`, in the collection's index.
2. **Is it structural?** Is the value needed to identify the resource or to relate it to another resource? If so, it is `wemi`. If it only describes, it is `dc`.

The path shape is unchanged:

```
.{node}.assets/{role}/metadata/wemi/{state}/wemi.yml
.{node}.assets/{role}/metadata/dc/{state}/dc.yml
.{node}.assets/{role}/metadata/sat/{state}/sat.yml
```

Each namespace cascades on its own, every namespace materialises at every node, an empty `defaults/` is not a missing one, and one resolver serves all three. v0.0.1 also said that *nothing in one namespace resolves against another*, yet the work record and the move validation both look values up across nodes. Both statements hold once two phases are separated. **Resolution** computes each namespace's values at each node from that namespace alone. **Reference** runs afterwards and joins resolved values to indexes: an expression's `work` to the collection's work record, a `derivesFrom` target to an existing expression. Resolution never consults another namespace; reference may.

## The `wemi` namespace

### Fields

Five fields, in three groups: one structural field, two identity fields and two relational fields.

```yaml
# .meditation.md.assets/content/metadata/wemi/overrides/wemi.yml
level:
  value: "expression"
  overridable: false

work:
  value: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
  overridable: false

expression:
  value: "urn:uuid:6b8d0f47-1a92-4c53-8e70-2d4f9b1c7a35"
  overridable: false

derivesFrom:
  value: []
  overridable: true

partOf:
  value: []
  overridable: true
```

| Field | Kind | Cardinality | Note |
| --- | --- | --- | --- |
| `level` | structural | one | `work`, `expression`, `manifestation` or `item`. Locked, because a node does not redefine what kind of thing it is |
| `work` | identity | one | The work's `urn:uuid`. The anchor that survives every move |
| `expression` | identity | one | This expression's `urn:uuid`. Distinct from the work's and from every sibling's |
| `derivesFrom` | relational | list | Expression UUIDs this expression derives from. Empty in the common case |
| `partOf` | relational | list | Work UUIDs whose work this work is a component of. Empty in the common case |

Identifiers are `urn:uuid:` values ([Davis et al., 2024](#ref-davis-2024))<a id="cite-davis-2024"></a> minted at random (`uuidgen -r`, version 4), so they carry no information about position and need no registry to mint.

### Identity does not cascade

v0.0.1 locked `work` and `expression` with `overridable: false` and also said every namespace cascades. Taken together, a directory node that declared its own `work` would push that UUID into every descendant and forbid them from changing it, so a whole section would claim to be one work. The fix is a resolution scope on the kind, not a change to the cascade.

| Kind | Scope | Meaning |
| --- | --- | --- |
| `identity` | node | Must be authored at the node. Never inherited from an ancestor. A node with no value is reported, not filled |
| `structural` | role | May be supplied by the role's defaults (the `content` role defaults `level` to `expression`). Never inherited from an ancestor node |
| `relational` | node | Authored at the node. An absent value is the empty list, not the parent's list |

`overridable: false` on these fields therefore locks the node's own declaration against later overrides at the same node. It has no downward reach, because there is nothing downward to reach.

### Relations

**`derivesFrom` and `partOf` start empty.** A translation needs neither: it is an expression of the same work, and that is already carried by `work`. Derivation is for the cases translation cannot express, such as an abridgement, an easy-read rewrite, or a transcript of a recording.

`derivesFrom` stores expression UUIDs and nothing about the kind of derivation, because the kind is computable. If the target expression realises the same work, the relation is LRM-R24. If it realises a different work, the derivation created a new work, and the projection is LRM-R22 between the two works. Comparing two `work` values decides it, so no second field is needed.

The relation is stored in one direction only, on the derived expression. The inverse ("what derives from this?") requires a scan, but the reference phase already scans the collection to validate targets, so the inverse index is built in the same pass at no extra cost.

`partOf` holds work UUIDs and projects to LRM-R18. The anthology case, in which a French edition omits a chapter the English edition includes, does not need an expression-level field: the omitted chapter simply has no French expression, so the work-level statement remains true, and the expression-level whole-part (LRM-R23) between a chapter's French expression and the anthology's French expression can be computed by joining `partOf` with language. Whether that computation is always sufficient is left as an open question.

## Level tagging in `field_kinds`

This half of the proposal pays for itself immediately. The sidecar specification observes that one sidecar holds statements at three WEMI levels, and that knowing which is which keeps them from being generated into the wrong artifact ([Steel, 2026a](#ref-steel-2026a)). Today that knowledge lives in a table in a document. It should live in the declaration.

```yaml
# config/sat/defaults/config.yml
field_kinds:
  value:
    dc:
      title:       { kind: local,        level: expression }
      description: { kind: local,        level: expression }
      date:        { kind: local,        level: expression }
      type:        { kind: local,        level: expression }
      format:      { kind: local,        level: manifestation }
      identifier:  { kind: local,        level: manifestation }
      creator:     { kind: inherited,    level: work }
      contributor: { kind: inherited,    level: expression }
      rights:      { kind: inherited,    level: expression }
      language:    { kind: inherited,    level: expression }
      publisher:   { kind: contextual,   level: manifestation }
      subject:     { kind: accumulating, level: work }
    sat:
      slug:        { kind: local,        level: expression }
    wemi:
      level:       { kind: structural,   level: self }
      work:        { kind: identity,     level: work }
      expression:  { kind: identity,     level: expression }
      derivesFrom: { kind: relational,   level: expression }
      partOf:      { kind: relational,   level: work }
      works:
        kind: index
        level: work
        entries:
          label: { kind: local,      level: work }
          order: { kind: contextual, level: work }
  overridable: false
```

Five level assignments differ from v0.0.1, each to follow LRM.

| Field | v0.0.1 | v0.1.0 | Reason |
| --- | --- | --- | --- |
| `dc:publisher` | expression | manifestation | Publication and distribution are manifestation facts in LRM. The same text published by two organisations is two manifestations |
| `dc:subject` | expression | work | LRM-R12 is *work has as subject res*. A translation does not change what a text is about |
| `dc:creator` | expression | work | The author creates the work (LRM-R5). Translators and editors are expression-level contributors (LRM-R6), which is what `dc:contributor` carries |
| `dc:identifier` | expression, kind `identity` | manifestation, kind `local` | Once `wemi:expression` holds the expression's identity, a second expression-level identity field is a duplicate. `dc:identifier` is kept for external identifiers of a published form (a URL, a DOI, an ISBN) |
| `sat:order` | `sat`, level work | removed from `sat` | Moved into the work record (Decision 2) |

Two consequences follow.

**The `format` problem becomes a lookup.** `dc:format` is declared `level: manifestation`, so a consumer knows without being told that it describes the file rather than the text, and that a generated artifact states its own format rather than inheriting this one. The sidecar specification currently handles this with a caveat in prose.

**Generation targets become derivable.** An ingress that writes front matter can ask the declaration which level each value belongs to and place it accordingly. `dc:format` becomes `text/html` in the built page's front matter because the built page is a different manifestation; the sidecar's `text/markdown` stays where it is. `dc:publisher` is likewise stated per manifestation rather than copied. One rule, applied from data.

`level: self` on `wemi:level` is the one oddity: the field declares the node's own level, so it cannot be tagged with any level other than the one it holds.

## The work record

The configuration specification leaves open *whether a work needs a record, and whether it carries anything beyond identity* ([Steel, 2026b](#ref-steel-2026b)). Under this proposal it does, and it needs no new mechanism. The work index is already scoped to the nearest collection root, and the collection root already has a sidecar with a `wemi/` namespace. The work record is a field in that file.

```yaml
# content/.content.assets/collection/metadata/wemi/overrides/wemi.yml
works:
  - value: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
    label:
      "en-ca": "Meditation practices"
      "fr-ca": "Pratiques de méditation"
    order: 4
  - value: "urn:uuid:a1c4e890-77b3-4f12-9d5a-6e83b2f4c019"
    label:
      "en-ca": "Licensing"
      "fr-ca": "Licences"
    order: 3
```

### The index is not inherited

`works` is declared with kind `index`, whose scope is the node it is authored at. Without this, a list-valued field at the collection root would cascade into the resolved `wemi` of every descendant, and every page would carry a copy of the whole collection's index. The index is read in the reference phase, by lookup, never by inheritance.

### What an entry holds

An entry has an identity, a label per language and an order. The entries' own kinds are declared under `works.entries`, which resolves a tension the namespace test would otherwise raise: `label` is descriptive and `order` is positional, so neither would pass the test as a `wemi` field on its own. They sit in `wemi` because the index is keyed by work identity and its entries are facts *about* works *within* this collection, not fields of any node.

**`order` moves here.** The ingress document already argues that `order` is resolved from the work rather than authored per expression, because "Projects before Areas" is a fact about the site's structure, identical in every language. That makes `order` a work-level property. Today it lives in `sat` and is written into every expression; here it is written once. Two refinements: `order` compares only works whose nodes share a parent, so the integers need not be unique across the collection; and `order` is contextual, so when a work moves to another collection its entry does not travel, and the target collection assigns its own.

**`label` is optional and falls back.** A work's label in a language defaults to the `dc:title` of its expression in that language. A label is stored only when navigation needs a different string, such as a shorter section heading. This avoids the duplication v0.0.1 warned against for descriptions while keeping its own label as a second copy of the title.

**Works carry no description.** Descriptions, creators, dates and rights are stated by expressions in `dc/`. A work that acquired a description would hold a second copy of something an expression already says.

### What the work record does not answer

v0.0.1 claimed the work record also answers the ingress document's open item on `localeLabel`, where the language selector renders `en-ca · fr-ca` because it has no labels. It does not. A language selector needs display names for *languages* ("English", "Français"), which are properties of the language axis and identical for every work. The work record supplies a work's name in each language, which answers a different question: what to call the sibling expression in the selector, if the selector shows titles rather than languages. Locale display names belong in the axis declaration.

## Movement as re-resolution

This is what the namespace is for. When SAT moves a resource, to another directory, archive, collection or instance, each kind behaves in a way that is determined rather than chosen.

| Kind | Examples | On move | Why |
| --- | --- | --- | --- |
| `identity` | `work`, `expression` | Travels unchanged | An identifier that changed when the thing moved would be useless |
| `structural` | `level` | Travels unchanged | Moving does not change what kind of thing it is |
| `local` | `title`, `date`, `format` | Travels unchanged | Describes this resource, not its position |
| `inherited` | `creator`, `contributor`, `rights`, `language` | Pinned, then travels | Usually set once at an ancestor for convenience, but true of the resource itself |
| `contextual` | `publisher`, work-record `order` | Re-derives from the new position | A fact about where the resource is published or listed |
| `accumulating` | `subject` | Re-derives from the new ancestor chain | The chain is the new path, and the subjects follow it |
| `relational` | `derivesFrom`, `partOf` | Travels unchanged, then validated | A move can orphan a relation, which is reported rather than repaired |

### Why `inherited` is new

v0.0.1 classed `creator`, `rights` and `language` as contextual, so they would have re-derived from the new position. That is wrong for all three. A French text moved under an English root does not become English; an essay moved into another archive keeps its author; a text licensed CC BY-SA is not relicensed by its new parent's default. These values are inherited only because authors set them once at a directory rather than on every file. The `inherited` kind records that: on move, the value resolved at the old position is written into the node's own `overrides/` before relocation, so it travels as a local fact. A mover who wants the new position's value can release the pin explicitly. `publisher` remains contextual, because who publishes a manifestation genuinely depends on where it is published.

### The move protocol

**A move is not a copy with metadata attached. It is a re-resolution.** The node carries its identity and its own facts, and everything positional is recomputed from where it now sits. This is what makes the cascade's direction irrelevant to movement: the node does not need to know which way it came.

1. **Pin.** Materialise every `inherited` value into the node's `overrides/`.
2. **Relocate.** Move the node and its sidecar.
3. **Register.** If the target collection's index lacks the node's work, add an entry. A mover who owns the target collection writes it; one who does not files a `propose:` (see below).
4. **Re-resolve.** Recompute `resolved/` for every namespace at the node and its descendants.
5. **Validate.** Run the reference checks.

The reference checks are cheap and each catches a failure that is otherwise silent.

| Check | Failure caught |
| --- | --- |
| Every `work` value resolves to an entry in the nearest collection root's index | A node orphaned from its work |
| Every `expression` value is unique within the collection | A copy that kept its identity |
| Every `derivesFrom` and `partOf` target exists | A relation broken by the move |
| No index entry in the source collection is left without an expression | An orphaned work record |
| No two expressions of one work share a language within the collection | A probable duplicate translation |

Uniqueness is scoped to the collection deliberately. The same expression may legitimately appear in two archives, as when a collection is mirrored into a second SAT instance.

### Copy is not move

A copy that will be edited independently is a new expression, not a second location for the old one. It mints a new `expression` UUID, keeps `work`, and records the original in `derivesFrom`. A copy that will not diverge (a mirror) is a second manifestation of the same expression, keeps both identifiers, and must live in a different collection, which the uniqueness check enforces.

## Proposals: the upward direction

The metadata tree already reaches downward with `amend:`. Movement needs the other direction: a content node that wants to say something about its work.

```yaml
# .meditation.md.assets/content/metadata/wemi/overrides/wemi.yml
propose:
  work:
    - match: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
      label:
        "en-ca": "Meditation and contemplative practice"
```

`propose:` is a reserved key, like `amend:`, not a field. It does not cascade.

**Semantics.** A proposal never changes a resolved value. The reader carries it into `resolved/` as an annotation marked `proposed`, and reports it. The collection root's editor accepts it by writing the value into the work record, which is an ordinary edit to a file they own.

**Why a proposal rather than a write.** A content node cannot write into the collection root's sidecar without crossing the ownership boundary the author segment exists to enforce. `propose:` crosses it as a message rather than a write, which keeps one `chgrp` per subtree intact.

**Why it is not `amend:`.** `amend:` changes a row a lower tier holds, and its authority comes from rank. `propose:` changes nothing and has no authority at all. They are different acts and get different keys.

### Closing a proposal

v0.0.1 left a proposal with two outcomes, accepted or unresolved forever, and did not say what happens to an accepted one. The ownership boundary that motivates `propose:` also means the editor cannot delete the proposal from the content node's file, so without a rule every accepted proposal would be reported forever. Three states, all decided from data the editor owns:

| State | Determined by | Reported |
| --- | --- | --- |
| Open | The proposed value differs from the work record and is not declined | Yes |
| Satisfied | The work record now holds the proposed value | No. The node's owner may remove it at leisure |
| Declined | The collection root lists the proposal under `declined:` | Once, to the node's owner, then silent |

```yaml
# content/.content.assets/collection/metadata/wemi/overrides/wemi.yml
declined:
  - match: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
    digest: "sha256:9c1e…"
    note: "Section title stays short for navigation"
```

The digest identifies the proposal by content, so a changed proposal is a new proposal and is reported again. When two nodes propose different values for the same work, both are open and the reader reports them together.

## Projections onto richer vocabularies

The mapping below is for whenever a projection is wanted. It replaces the v0.0.1 table, which numbered derivation as R23 (the expression whole-part relationship), mapped `derivesFrom` to two DCTERMS terms that mean something else, and named BIBFRAME classes that do not exist.

| `wemi` | IFLA LRM | DCTERMS | BIBFRAME 2.0 | schema.org |
| --- | --- | --- | --- | --- |
| `level: work` | LRM-E2 | none | `bf:Work` | `CreativeWork` |
| `level: expression` | LRM-E3 | none | `bf:Work` linked by `bf:expressionOf` | `CreativeWork` |
| `level: manifestation` | LRM-E4 | none | `bf:Instance` | `MediaObject` or a `CreativeWork` subtype |
| `level: item` | LRM-E5 | none | `bf:Item` | none |
| `work` (from an expression) | LRM-R2, inverse | `dcterms:isVersionOf` (approximate) | `bf:expressionOf` | `exampleOfWork` |
| `derivesFrom`, same work | LRM-R24 | `dcterms:source` | `bf:derivativeOf` | `isBasedOn` |
| `derivesFrom`, different work | LRM-R22 between the works | `dcterms:source` | `bf:derivativeOf` | `isBasedOn` |
| `partOf` | LRM-R18 | `dcterms:isPartOf` | `bf:partOf` | `isPartOf` |
| computed expression part | LRM-R23 | `dcterms:isPartOf` | `bf:partOf` | `isPartOf` |
| collection root to its contents | LRM-R25 | none | none | `hasPart` |

Notes on the corrections.

**LRM numbering.** Derivation between expressions is R24, not R23; R23 is expression whole-part. Work whole-part is R18, as v0.0.1 had it. The 2017 revision renumbered many relationships and reversed R24's direction ([IFLA FRBR Review Group, 2017](#ref-ifla-2017-changes)), which explains the disagreement v0.0.1 found among its sources. The numbers here follow the published relationship tables ([Bibliotheksverbund Bayern & Deutsche Nationalbibliothek, n.d.](#ref-dnb-lrm-tables)); they should still be checked against the master text before any projection code depends on them.

**DCTERMS.** `dcterms:source` is defined as a related resource from which the described resource is derived ([DCMI Usage Board, 2020](#ref-dcmi-2020)), which is exactly `derivesFrom`. `dcterms:isFormatOf` relates two formats of the same content, a manifestation-level relation, and `dcterms:hasVersion` points the opposite way. Neither should be emitted for `derivesFrom`.

**BIBFRAME.** BIBFRAME 2.0 has three core classes, Work, Instance and Item, and no Expression or Manifestation class ([Library of Congress, 2016](#ref-loc-2016))<a id="cite-loc-2016"></a>. An expression is modelled as a second `bf:Work` linked to the first by `bf:expressionOf`, and a manifestation corresponds to `bf:Instance`.

**schema.org.** schema.org deliberately has no WEMI types, but `exampleOfWork` and its inverse `workExample` were added to `CreativeWork` so that WEMI-structured data can be expressed ([Schema.org, n.d.](#ref-schemaorg))<a id="cite-schemaorg"></a>. `translationOfWork` is available as a refinement when the derivation is a translation.

The note from v0.0.1 stands: `dcterms:isVersionOf` is the closest DCTERMS term to `wemi:work`, but R2 is realisation, not versionhood, and the R2 to R4 chain has no DCTERMS counterpart at all.

## What is deliberately absent

Every item below is available later as a namespace, and none is needed for movement.

| Absent | Where it would go | Why not now |
| --- | --- | --- |
| Agents as entities | `foaf/`, `schema/` | `dc:creator` and `dc:publisher` carry strings, which is what a byline needs |
| Concepts and schemes | `skos/` | Where `dc:subject` values are ancestor work identifiers, the work record already supplies a label per language, which is the useful half of a scheme |
| Places and time-spans as entities | `dcterms/` | `dcterms:spatial` and `dcterms:temporal` separate them when wanted; `dc:coverage` is unused |
| The full LRM entity set | `lrm/` | Eleven entities where four are load-bearing |
| The full LRM relationship set | `lrm/` | Thirty-six relationships where five fields, and the computed R22, R23 and R25, cover movement |
| BIBFRAME classes | `bibframe/` | A projection, not a source |
| Item-level custody | `wemi/`, later | Items are not modelled in sidecars, and nothing yet needs them |

The pattern is consistent: each absent item is a vocabulary, and each vocabulary is a projection onto the spine.

## Discussion

### What the spine buys

The practical gain is that the russian-doll goal of SAT, a sat, collection or archive lifted out of one instance and placed in another, becomes a defined operation with a validation step rather than a hope. Every field has a declared behaviour on move, the behaviours are few, and the checks after a move are cheap enough to run every time. The level tags turn a recurring class of ingress bug, a manifestation fact copied into the wrong artifact, into a lookup.

### Costs and limits

The spine is not free. Every content node needs two UUIDs, minted once, and a tool must mint them, since an author who copies a sidecar by hand also copies its identity. The uniqueness check catches that, but only after the fact. The pinning step writes into a node's own overrides during a move, which is a change the node's owner will see in version control; this is intended, since the move did change what that file has to state, but it should be reported clearly.

The work index is scoped to a collection. Works shared across collections are listed in each, and their labels and order may differ, which is correct for order and arguably wrong for labels. Nothing here models items, so custody, copies held by different owners and physical holdings are out of scope. And the computed relations (R22, R23, R25) are only as good as the joins that compute them; the anthology case suggests they suffice, but that has not been tested against a real archive.

### Relation to existing practice

Separating work identity from its realisations is standard in library systems, and BIBFRAME and schema.org both model it with fewer types than LRM by reusing one class across levels. SAT's choice is closer to schema.org's: one stored record per node, with the level carried as a value rather than a type. Where SAT differs is in making behaviour under movement part of the field declaration, which neither vocabulary addresses because neither is a storage system.

## Decisions needed

**Decision 1: whether identity moves out of `sat`.** `sat:identifier` and `sat:isVersionOf` are currently the WEMI identity store. This proposal renames them `wemi:expression` and `wemi:work`. For: they are structural rather than operational, and `sat` is defined as terms SAT needs that DCTERMS has no word for, a vocabulary gap, which identity is not. Against: every existing sidecar and fixture breaks. *Recommendation:* move now, while nothing is in production, and record the rename in the migration pass the `.sat-boundary` marker already requires.

**Decision 2: whether `order` moves to the work record.** The ingress document argues it is a work-level fact and then writes it into every expression. *Recommendation:* move, and let the ingress resolve it from the work index in the reference phase.

**Decision 3: whether the work record is a field or a file.** The alternative to a field is a `works.yml` beside the sidecar, which would be a second configuration mechanism outside the cascade, the same objection the specification already raises against `.satignore`. *Recommendation:* the field, declared with kind `index` so that it does not cascade.

**Decision 4: whether to split `contextual` into `contextual` and `inherited`.** Without the split, a move rewrites creator, rights and language from the new position. *Recommendation:* split, with pinning on move as the default behaviour of `inherited`.

**Decision 5: what `dc:identifier` holds.** Once `wemi:expression` exists, an expression-level `dc:identifier` duplicates it. *Recommendation:* reserve `dc:identifier` for external identifiers of a manifestation, and project `wemi:expression` into `dc:identifier` at egress only where a consumer expects it there.

## Open questions

**Whether `level` is needed at all.** A content node is an expression and its file is a manifestation, always. If that never varies, `level` is a constant, and constants do not need storing. It earns its place only if authored embodiments get nodes, such as a Braille edition or a typeset PDF, which the sidecar specification anticipates but does not yet model.

**What level non-content nodes declare.** A directory that groups pages may be a work (a section with a title in each language, ordered among its siblings) or merely a container. A collection root behaves like an aggregating work in LRM's sense (R25). The current draft only specifies `level` for content nodes.

**Whether expression-level whole-part can always be computed.** The draft computes LRM-R23 from `partOf` and language. An archive with two expressions of the same work in the same language (a revised edition beside the original) would defeat that join.

**Whether `wemi` should be spelled `wemi` or `lrm`.** The namespace holds a subset of LRM, and naming it for the subset is honest about the scope. Naming it `lrm` would let the rest of the model arrive in the same namespace later without a rename, at the cost of a namespace that claims more than it holds.

Two questions from v0.0.1 are now settled in the body: `derivesFrom` is stored in one direction with the inverse built during validation, and proposals gain a declined state recorded by the collection root.

## Conclusion

Movement in a file-based archive needs a small amount of structure and a clear rule for everything else. Five `wemi` fields carry the structure; a level tag on every field turns generation targets into data; a non-cascading work index in the collection root gives works a record without a new mechanism; and a kind-based move protocol separates what travels, what is pinned, and what is recomputed. Richer bibliographic models remain available as projections, added when something needs them.

## References

<a id="ref-dnb-lrm-tables"></a>Bibliotheksverbund Bayern & Deutsche Nationalbibliothek. (n.d.). *IFLA LRM: Tabellen* [German translation of the IFLA LRM entity, attribute and relationship tables]. RDA Steering Committee. https://www.rda-rsc.org/files/assets/cataloguing/frbr-lrm/ifla_lrm_tables_german_final.pdf
[Return to citation](#cite-dnb-lrm-tables)

<a id="ref-davis-2024"></a>Davis, K., Peabody, B., & Leach, P. (2024). *Universally unique IDentifiers (UUIDs)* (RFC 9562). Internet Engineering Task Force. https://doi.org/10.17487/RFC9562
[Return to citation](#cite-davis-2024)

<a id="ref-dcmi-2020"></a>DCMI Usage Board. (2020). *DCMI metadata terms*. Dublin Core Metadata Initiative. https://www.dublincore.org/specifications/dublin-core/dcmi-terms/
[Return to citation](#cite-dcmi-2020)

<a id="ref-ifla-2017-changes"></a>IFLA FRBR Review Group. (2017). *Summary of updates to IFLA LRM since the 2016 world-wide review draft titled FRBR-LRM*. International Federation of Library Associations and Institutions. https://progetti.unicatt.it/progetti-milan-ifla_lrm_summary_of_changes_2017-02-21.pdf
[Return to citation](#cite-ifla-2017-changes)

<a id="ref-ifla-1998"></a>IFLA Study Group on the Functional Requirements for Bibliographic Records. (1998). *Functional requirements for bibliographic records: Final report*. K. G. Saur. https://repository.ifla.org/handle/20.500.14598/811
[Return to citation](#cite-ifla-1998)

<a id="ref-loc-2016"></a>Library of Congress. (2016). *Overview of the BIBFRAME 2.0 model*. https://www.loc.gov/bibframe/docs/bibframe2-model.html
[Return to citation](#cite-loc-2016)

<a id="ref-rsc-2021"></a>RDA Steering Committee Technical Working Group. (2021). *Realignment of expression to expression relationship elements* (RSC/TechnicalWG/2021/1). https://www.rdatoolkit.org/sites/default/files/rsc/RSC-TechnicalWG-2021-1_0.pdf
[Return to citation](#cite-rsc-2021)

<a id="ref-riva-2017"></a>Riva, P., Le Bœuf, P., & Žumer, M. (2017). *IFLA Library Reference Model: A conceptual model for bibliographic information* (Rev. ed.). International Federation of Library Associations and Institutions. https://www.ifla.org/files/assets/cataloguing/frbr-lrm/ifla-lrm-august-2017_rev201712.pdf
[Return to citation](#cite-riva-2017)

<a id="ref-schemaorg"></a>Schema.org. (n.d.). *CreativeWork*. Retrieved October 29, 2026, from https://schema.org/CreativeWork
[Return to citation](#cite-schemaorg)

<a id="ref-steel-2026a"></a>Steel, C. (2026a). *SAT sidecars: Config and metadata* (Version 0.0.12) [Unpublished specification]. Vishpala.
[Return to citation](#cite-steel-2026a)

<a id="ref-steel-2026b"></a>Steel, C. (2026b). *SAT configuration cascade specification* (Version 0.2.2) [Unpublished specification]. Vishpala.
[Return to citation](#cite-steel-2026b)

## Appendix A: changes from v0.0.1

| Issue in v0.0.1 | Resolution in v0.1.0 |
| --- | --- |
| "Holds four things and no more" beside "Five fields. Deliberately five." | Five fields, in three groups |
| Namespace test answered yes for both `wemi` and `dc` | Two ordered questions: portable, then structural |
| `order` listed under `sat` in the partition table while the work record moved it out | `order` removed from `sat`; lives in the work record as a contextual entry attribute |
| "Nothing in one namespace resolves against another" contradicted by index lookups | Resolution and reference separated into two phases |
| Identity fields locked with `overridable: false` would cascade to every descendant | `identity`, `structural`, `relational` and `index` kinds given non-inheriting scopes |
| `creator`, `rights`, `language` contextual, so a move would rewrite them | New `inherited` kind, pinned before a move |
| `creator` contextual in `field_kinds` but absent from the movement table | Every declared kind appears in the movement table |
| `dc:publisher` and `dc:subject` at expression level | Manifestation and work, following LRM |
| `dc:identifier` as a second expression-level identity | Manifestation-level external identifiers (Decision 5) |
| `works` list at the collection root would cascade into every page | Kind `index`, node-scoped, read by lookup |
| Work labels duplicated expression titles | Labels optional, falling back to `dc:title` |
| Work record claimed to solve `localeLabel` | Locale display names assigned to the axis declaration |
| Move validation assumed the work was already in the target index | Register step, by write or `propose:` |
| No distinction between copy and move | Copy mints a new expression and records `derivesFrom` |
| Accepted proposals reported forever, rejection undefined | Open, satisfied and declined states; `declined:` with content digest |
| `derivesFrom` mapped to R23 | R24 for the same work, R22 for a new work |
| `derivesFrom` mapped to `dcterms:isFormatOf` and `dcterms:hasVersion` | `dcterms:source` |
| `bf:Expression` and `bf:Manifestation` | `bf:Work` with `bf:expressionOf`, and `bf:Instance` |
| schema.org gaps for expression | `exampleOfWork`, with `translationOfWork` as a refinement |
| Three decisions, five open questions | Five decisions, four open questions, two questions settled |
| Em dashes throughout | Removed, per house style |

## Changelog

| Version | Status | Notes |
| --- | --- | --- |
| 0.1.0 | Draft | Restructured as a research paper with abstract, background, discussion and APA 7 references. Separates resolution from reference; gives identity, structural, relational and index kinds non-inheriting scopes; adds the `inherited` kind and pinning on move; corrects five level assignments; makes the work record a non-cascading index with fallback labels; defines a move protocol, copy semantics and five reference checks; adds satisfied and declined states to `propose:`; corrects the projection table against LRM, DCTERMS, BIBFRAME 2.0 and schema.org. Five decisions, four open questions. |
| 0.0.1 | Draft | Initial proposal. Establishes `wemi/` as the structural namespace with five fields; extends `field_kinds` with a level so generation targets are data; gives the work record a home in the collection root's sidecar; states movement as re-resolution rather than copy; adds `propose:` as the upward direction; records three decisions and five open questions. |

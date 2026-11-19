# WEMI as the Structural Namespace

**Version.** v0.0.1, 2026-10-29. Draft. Companion to *SAT Sidecars: Config and Metadata* v0.0.12 and *SAT Configuration Cascade Specification* v0.2.2.

**Scope.** A third metadata namespace, `wemi`, holding the structural spine that makes a resource movable: which level it declares, which work it realises, and which expression it is. Also extends the `field_kinds` declaration with a level, so that *which artifact a value generates into* becomes data rather than prose.

**Status.** Proposal. Nothing here is implemented. Three decisions are needed before it can be, and they are listed at the end rather than buried.

## The principle

**Hold the minimum that movement requires. Let every richer vocabulary be a projection onto it.**

This is the whole design. A document that knows its work, its expression and its level can be moved, reorganized, re-pathed, re-rendered and re-translated without anyone consulting a cataloguer. A document that additionally knows its FRBR-complete agent graph, its SKOS concept scheme and its BIBFRAME class assignments can do all of that too — but it needs a librarian to maintain it, and that librarian is a person you do not have.

So `wemi/` holds four things and no more: the level, the work identity, the expression identity, and the relations that movement needs. Everything else stays where it is.

The deferred-richness property is what makes this safe. Because the namespace layer already exists — `metadata/{namespace}/{state}/` — adding `metadata/skos/`, `metadata/foaf/`, `metadata/bibframe/` or `metadata/lrm/` later is a declaration rather than a re-architecture. A richer system is not being rejected. It is being postponed until something needs it, which is the only honest basis for adopting one.

## Division of labour

Three namespaces, three jobs, no overlap.

| Namespace | Job | Answers |
| --- | --- | --- |
| `wemi` | Structure | What kind of thing is this, which work does it realise, which expression is it |
| `dc` | Description | What is it called, who made it, when, under what rights |
| `sat` | Operation | What does the software need that is neither of the above: `slug`, resource-scoped axis values, `order` |

The test for which namespace a field belongs in: **would this field still mean something if the document were moved to a different archive?** Identity fields would, and they are `wemi`. Descriptive fields would, and they are `dc`. Operational fields would not, and they are `sat`.

## Path shape

Unchanged. The namespace layer already accommodates it.

```
.{node}.assets/{role}/metadata/wemi/{state}/wemi.yml
```

Alongside the existing two:

```
.{node}.assets/{role}/metadata/dc/{state}/dc.yml
.{node}.assets/{role}/metadata/sat/{state}/sat.yml
```

Namespaces are independent, each cascades on its own, and nothing in one resolves against another. The existing rules apply without amendment: every namespace materialises at every node, an empty `defaults/` is not a missing one, and one resolver serves all three.

## What `wemi/` holds

Five fields. Deliberately five.

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
| `level` | structural | one | `work`, `expression`, `manifestation` or `item`. Locked, because a document does not redefine what kind of thing it is |
| `work` | identity | one | The work's `urn:uuid`. The anchor that survives every move |
| `expression` | identity | one | This expression's `urn:uuid`. Distinct from the work's, and distinct from every sibling's |
| `derivesFrom` | relational | list | Expression UUIDs this derives from. Empty in the common case |
| `partOf` | relational | list | Work UUIDs this is a component of. Empty in the common case |

**`derivesFrom` and `partOf` are lists and start empty.** A translation needs neither: it is an expression of the same work, and that fact is already carried by `work`. Derivation is for the cases translation cannot express — an abridgement, an easy-read rewrite, a transcript of a recording. Those exist, they are not the common case, and an empty list costs nothing.

## Level tagging in `field_kinds`

This is the second half of the proposal and it is the part that pays for itself immediately.

The sidecar specification already observes that one sidecar holds statements at three WEMI levels, and that *knowing which is which is what keeps them from being generated into the wrong artifact later*. Today that knowledge lives in a table in a document. It should live in the declaration.

```yaml
# config/sat/defaults/config.yml
field_kinds:
  value:
    dc:
      title:       { kind: local,       level: expression }
      description: { kind: local,       level: expression }
      date:        { kind: local,       level: expression }
      type:        { kind: local,       level: expression }
      format:      { kind: local,       level: manifestation }
      creator:     { kind: contextual,  level: expression }
      contributor: { kind: contextual,  level: expression }
      publisher:   { kind: contextual,  level: expression }
      rights:      { kind: contextual,  level: expression }
      language:    { kind: contextual,  level: expression }
      subject:     { kind: accumulating, level: expression }
      identifier:  { kind: identity,    level: expression }
    sat:
      slug:        { kind: local,       level: expression }
      order:       { kind: contextual,  level: work }
    wemi:
      level:       { kind: structural,  level: self }
      work:        { kind: identity,    level: work }
      expression:  { kind: identity,    level: expression }
      derivesFrom: { kind: relational,  level: expression }
      partOf:      { kind: relational,  level: work }
  overridable: false
```

Two things this buys.

**The `format` problem dissolves.** `dc:format` is declared `level: manifestation`, so a consumer knows without being told that it describes the file rather than the text, and that a generated artifact states its own rather than inheriting this one. That was the contradiction that forced the two-document split in the first place, and it becomes a lookup rather than a caveat.

**Generation targets become derivable.** An ingress that writes front matter can ask the declaration which level each value belongs to, and place it accordingly. `dc:format` becomes `text/html` in the built page's front matter because the built page is a manifestation; the sidecar's `text/markdown` stays where it is because the source is a different manifestation. One rule, applied from data.

`level: self` on `wemi:level` is the one oddity: the field declares the node's own level, so it cannot be tagged with a level other than the one it holds.

## The work record

The configuration specification leaves this open: *whether a work needs a record, and whether it carries anything beyond identity.*

Under this proposal it does, and it needs no new mechanism. The work index is already scoped to the nearest collection root, and the collection root already has a sidecar with a `wemi/` namespace in it. The work index is that file.

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

This answers three of the specification's open questions at once.

**`order` moves here.** The ingress document already argues that `order` is resolved from the work rather than authored per expression, *because "Projects before Areas" is a fact about the site's structure, identical in every language*. That is a statement that `order` is a work-level property. It currently lives in `sat/` and is written into both expressions; here it has a home and is written once.

**Labels are per language and per work.** The ingress document's open item on `localeLabel` — the language selector rendering `en-ca · fr-ca` because it has no labels — is answered by the work record, since a work's label exists in each language that realises it.

**Works carry no description.** A work has a label and an order and nothing else. Descriptions, creators, dates and rights are expression-level facts and stay in `dc/` where they are. A work that acquired a description would be acquiring a second copy of something an expression already says.

## Movement

This is what the namespace is for, and it falls out of the field kinds already declared.

When SAT moves a resource — to a different directory, a different archive, a different collection, a different instance — each field class behaves differently, and the behaviour is determined rather than chosen.

| Kind | On move | Why |
| --- | --- | --- |
| `identity` (`work`, `expression`) | Travels unchanged | The anchor. An identifier that changed when the thing moved would be useless |
| `structural` (`level`) | Travels unchanged | Moving does not change what kind of thing it is |
| `local` (`title`, `date`, `format`) | Travels unchanged | Describes this expression, not its position |
| `contextual` (`publisher`, `rights`, `language`) | Re-derives from the new position | Inherited from above, so a new above means a new value |
| `accumulating` (`subject`) | Re-derives from the new ancestor chain | The chain is the new path, and the subjects follow it |
| `relational` (`derivesFrom`, `partOf`) | Travels unchanged, then validated | A move can orphan a relation, which is reported rather than repaired |

The consequence worth stating: **a move is not a copy operation with metadata attached. It is a re-resolution.** The document carries its identity and its own facts; everything positional is recomputed from where it now sits. That is what makes the cascade's direction irrelevant to movement — the document does not need to know which way it came.

It also gives the move a natural validation step. After re-resolution, the reader checks that every `work` value resolves to a work in the nearest collection root's index, that every `expression` value is unique, and that every `derivesFrom` and `partOf` target still exists. Three checks, all cheap, all catching the failures that are otherwise silent.

## Proposals: the upward direction

The metadata tree already reaches downward with `amend:`, which lets a higher role unlock or deprecate what a lower one holds. Movement needs the other direction: a content node that wants to say something about its work.

```yaml
# .meditation.md.assets/content/metadata/wemi/overrides/wemi.yml
propose:
  work:
    - match: "urn:uuid:3f1b7c20-9d84-4a6e-b052-8c47e1a9d3f6"
      label:
        "en-ca": "Meditation and contemplative practice"
```

A proposal is inert until accepted. It resolves into nothing, appears in `resolved/` marked as proposed, and is reported by the reader. The collection root's editor accepts it by writing the value into the work record, which is an ordinary edit to a file they own.

**Why a proposal rather than a write.** A content node cannot write into the collection root's sidecar without crossing the ownership boundary the author segment exists to enforce. `propose:` crosses it as a message rather than as a write, which keeps one `chgrp` per subtree intact.

**Why it is not `amend:`.** `amend:` changes a row a lower tier holds, and its authority is rank-based. `propose:` changes nothing and has no authority at all. They are different acts and get different keys.

## What is deliberately absent

Every one of these is available later as a namespace, and none of them is needed for movement.

| Absent | Where it would go | Why not now |
| --- | --- | --- |
| Agents as entities | `foaf/`, `schema.org/` | `dc:creator` and `dc:publisher` carry strings, which is what a byline needs |
| Concepts and schemes | `skos/` | `dc:subject` accumulating work identifiers already gives labels per language, which is the useful half |
| Places and time-spans as entities | `dcterms/` | `dcterms:spatial` and `dcterms:temporal` separate them when wanted; `dc:coverage` is unused |
| The full LRM entity set | `lrm/` | Eleven entities where four are load-bearing |
| The 36 relationships | `lrm/` | Five fields where thirty-six relationships exist |
| BIBFRAME classes | `bibframe/` | A projection, not a source |
| Item-level custody | `wemi/` later | Items are not modeled in sidecars, and nothing yet needs them |

The pattern is consistent: each absent item is a vocabulary, and each vocabulary is a projection onto the spine. Adding one is a declaration in `field_kinds` and a new directory, not a change to the resolver.

## Doors left open

The mapping, for whenever a projection is wanted. **The LRM relationship numbers in this table should be verified against the 2017 master text before being relied on** — my sources disagreed on the numbering in the R19–R24 range, and I would rather flag that than propagate it.

| `wemi` | LRM | DCTERMS | BIBFRAME | schema.org |
| --- | --- | --- | --- | --- |
| `work` | LRM-E2 | `dcterms:isVersionOf` (approximate) | `bf:Work` | `CreativeWork` |
| `expression` | LRM-E3 | — | `bf:Expression` | — |
| `level: manifestation` | LRM-E4 | — | `bf:Manifestation` | — |
| `level: item` | LRM-E5 | — | `bf:Item` | — |
| `derivesFrom` | R23 (verify) | `dcterms:isFormatOf`, `dcterms:hasVersion` | `bf:derivativeOf` | `isBasedOn` |
| `partOf` | R18 (verify) | `dcterms:isPartOf` | `bf:partOf` | `isPartOf` |
| `work` ↔ `expression` | R2 | — | — | — |
| `expression` ↔ `manifestation` | R3 | — | — | — |

Two notes on the mapping. `dcterms:isVersionOf` is the closest DCTERMS term to `wemi:work`, which is why the current implementation uses it — but R2 is *realization*, not versionhood, and the two are not the same relation. And the R2/R3/R4 chain has no DCTERMS counterpart at all, which is the honest reason a `wemi` namespace exists rather than a `dcterms` one.

## Decisions needed

**Whether identity moves out of `sat/`.** `sat:identifier` and `sat:isVersionOf` are currently the WEMI identity store. This proposal renames them `wemi:expression` and `wemi:work`. The argument for moving is that they are structural rather than operational, and that `sat` is defined as *terms SAT needs that DC has no word for* — a vocabulary gap, which identity is not. The argument against is that it breaks every existing sidecar and every fixture. Recommendation: move, now, while nothing is in production, and record the rename in the migration pass the `.sat-boundary` marker already requires.

**Whether `order` moves to the work record.** The ingress document argues it is a work-level fact and then writes it into every expression. Recommendation: move, and let the ingress resolve it from the work index.

**Whether the work record is a field or a file.** This proposal puts it in the collection root's `wemi/` namespace as a list-valued field. The alternative is a `works.yml` beside the sidecar, which would be a second configuration mechanism outside the cascade — the same objection the specification already raises against `.satignore`. Recommendation: the field, for that reason.

## Open questions

**Whether `level` is needed at all.** A content node is an expression and its file is a manifestation, always. If that never varies, `level` is a constant and constants do not need storing. It earns its place only if authored embodiments get nodes — a Braille edition, a typeset PDF — which the sidecar specification anticipates but does not yet model.

**Whether `derivesFrom` should be directional or paired.** A derivation is asymmetric: an abridgement derives from its source, and the source does not derive from the abridgement. Storing one direction and computing the inverse is cheaper than storing both and risking disagreement, but it makes the inverse query a scan.

**Whether `partOf` belongs at work level or expression level.** A chapter is part of a book as a work. But an anthology's French edition may omit a chapter the English one includes, which is a part-whole fact about expressions. Both may be true, and the field currently holds one kind.

**Whether proposals need rejection.** `propose:` currently has two outcomes: accepted, or left unresolved forever. A third — rejected, and not re-proposed — is what `status: deprecated` does for `amend:`, and the same reasoning applies.

**Whether `wemi` should be spelled `wemi` or `lrm`.** The namespace holds a subset of LRM, and naming it for the subset rather than the model is honest about the scope. Naming it `lrm` would leave room for the rest to arrive in the same namespace later without a rename.

## Changelog

| Version | Status | Notes |
| --- | --- | --- |
| 0.0.1 | Draft | Initial proposal. Establishes `wemi/` as the structural namespace with five fields; extends `field_kinds` with a level so generation targets are data; gives the work record a home in the collection root's sidecar; states movement as re-resolution rather than copy; adds `propose:` as the upward direction; records three decisions and five open questions. |

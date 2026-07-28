# SAT ROADMAP

## Project Vision

Source Archive Tools (SAT) is a filesystem-first system for managing
structured multilingual archives with clear separation between system
tools, archive containers, and content. It relies on simple directory
contracts, small standalone commands, a self-replicating permission
model, and a commitment to accessibility for all humans.

---

## Current Status: Pre-MVP (April 2026)

The foundational architecture has been defined. Implementation has
begun on core tooling. The specification documents referenced below
describe the decisions made and the reasoning behind them.

---

## Decisions Made

The following architectural decisions have been settled and are
documented in `en/docs/radar/decisions/`:

- Metadata sidecar specification (nine mandatory fields, `.dc.yml`
  convention, flat DC keys, creation and modification dates separated)

  - > [metadata-sidecar-spec.md](../specifications/metadata-sidecar-spec.md)

- Four-role permission model (SAT Admin, Collection Admin, Archive
  Admin, Content Admin) with self-replicating bin directories
  -> [permission-model.md](./decisions/permission-model.md)

- Three-axis configuration cascade (tier, level, operation)
  -> [configuration-cascade.md](en/docs/radar/decisions/configuration-cascade.md)

- Archive definition two-block format (`sat:` and `dc:` blocks)
  -> [archive-definition-format.md](en/docs/radar/decisions/archive-definition-format.md)

- Collection as first-class filesystem level
  -> [filesystem-layout.md](en/docs/radar/decisions/filesystem-layout.md)

- Four operation categories (`init`, `ingress`, `egress`, `transmog`)
  with `define` and `ensure` under `init`
  -> [operation-categories.md](en/docs/radar/decisions/operation-categories.md)

- Language as structural, not descriptive — the directory IS the
  declaration
  -> [language-model.md](en/docs/radar/decisions/language-model.md)

- IANA Language Subtag Registry as the single authoritative source
  for language directory name validation
  -> [language-model.md](en/docs/radar/decisions/language-model.md)

- Mixed language archives with BCP 47 canonical casing, alphabetical
  ordering, and underscore separator (`en-CA_fr-CA/`)
  -> [language-model.md](en/docs/radar/decisions/language-model.md)

- Sign languages as first-class archives (`ase/`, `ase-blasl/`, `lsq/`)
  -> [language-model.md](en/docs/radar/decisions/language-model.md)

---

## Next Steps

The following items are the immediate priority. Details in
`en/docs/radar/next/`:

- Close three open decisions from the filesystem layout specification
  (symlinks vs copies, merge vs replace for defaults cascade, ownership
  of `.dc.defaults.yml` files)
  -> [open-decisions.md](en/docs/radar/next/open-decisions.md)

- Commit foundational specification documents to `en/docs/specifications/`
  -> [specifications-needed.md](en/docs/radar/next/specifications-needed.md)

- Update existing archive definitions to two-block `sat:` / `dc:` format
  -> [archive-definition-format.md](en/docs/radar/decisions/archive-definition-format.md)

- Implement defaults cascade resolution in `content-metadata.py`
  -> [configuration-cascade.md](en/docs/radar/decisions/configuration-cascade.md)

- Implement `dc:modified` automation from git history
  -> [metadata-sidecar-spec.md](en/docs/radar/decisions/metadata-sidecar-spec.md)

- Implement `dc:identifier` generation from archive name and path
  -> [metadata-sidecar-spec.md](en/docs/radar/decisions/metadata-sidecar-spec.md)

- Restructure `en/bin/` to match settled filesystem layout
  -> [filesystem-layout.md](en/docs/radar/decisions/filesystem-layout.md)

- Run markdownlint `--fix` across `en/docs/guides/`
  -> [markdown-normalisation.md](en/docs/radar/next/markdown-normalisation.md)

---

## Deferred

The following items are defined and understood but explicitly out of
scope for the MVP. Details in `en/docs/radar/deferred/`:

- `.bibo.yml` extension sidecars for bibliographic metadata
- `.lrmi.yml` extension sidecars for educational metadata
- `.dc.defaults.yml` directory-level inheritance
- VIAF URI enrichment for `dc:creator`
- Subject vocabulary lookup automation via Getty AAT API
- CI/CD pipeline integration
- Multi-user access control enforcement beyond directory convention
- Organisational owner block in archive definitions
- Tool version propagation for delegated bin directories
- Multilingual SAT tooling (`sat/fr/`, `sat/es/` etc.)
- `sat:tooling` expansion beyond English
- `collection define`, `archive define`, `content define` specifications

---

## Reference Documents

Full specification and decision documents are maintained in the SAT
English documentation archive:

```text
en/docs/
 specifications/         ← formal specifications
 radar/
   decisions/            ← architectural decision records
   next/                 ← immediate next steps elaborated
   deferred/             ← deferred items elaborated
 guides/                 ← practical how-to guides
 language/               ← language model documentation
```

---

## Version

This roadmap reflects the state of SAT as of April 2026.
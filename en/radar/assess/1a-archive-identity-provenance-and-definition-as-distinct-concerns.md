---
dc:title: "Archive Identity, Provenance, and Definition as Distinct Concerns"
dc:creator: "Christopher Steel"
dc:subject:
  - "archive creation"
  - "identity"
  - "provenance"
  - "definition"
  - "archive init"
dc:description: "Assessment of the distinction between archive identity, provenance, and definition as separate concerns at archive creation, and the implications for archive init scope."
dc:publisher: "SAT – Source Archive Tools"
dc:date: "2026-06-14"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "eng"
dc:language_bcp47: "en"
dc:rights: "CC BY-SA 4.0"
---

# Archive Identity, Provenance, and Definition as Distinct Concerns

Ring: assess
Category: architecture

---

## Summary

At archive creation three distinct concerns are in play — identity, provenance, and definition. These are related but separate and should not be conflated in a single file or operation. The scope of `archive init` depends on resolving this distinction clearly.

---

## The Distinction

**Identity** — what this archive *is*. Name, language, creator, type, purpose. Written once at creation, owned by the created archive. Fixed. Carried in the DC sidecar and language record.

**Provenance** — how this archive came to exist. When, by whom, using what SAT version and tool, from what parent collection. The historical record of the creation event. Git carries much of this; the provenance record captures the moment explicitly.

**Definition** — what this archive will contain and how it is structured internally. The tree of areas, projects, sections. This is a *use* concern, not a creation concern. It may evolve as the archive grows. It is not fixed at creation.

---

## Current Thinking

The old `archive-init.py` in `en/bin/archives/` conflated all three concerns into a single per-archive definition file (e.g. `chrissteel.com-en.yml`), which carried `archive_name`, `language`, and `tree` together. This worked for the MVP but obscures the distinction between what an archive *is* and what it *contains*.

The proposed separation:

- `archive init` establishes identity and provenance only — DC sidecar, language record, provenance record. The archive exists and has an identity.
- Definition — internal tree structure, areas, content organisation — comes later, at content ingress time or as a separate definition step.
- Definition may live inside the archive itself once established, not in the tools directory.

---

## Open Questions

- Is this distinction already covered in ADR-009 (Distribution by Installer and Instantiation) or another existing ADR? Needs review before filing a new ADR.
- What is the minimum identity record at archive creation? DC sidecar fields, language declaration, SAT version — what else?
- Where does the definition live once established — inside the archive, in the tools directory, or in `~/.config/sat/`?
- Is definition always required, or only for archives with a known internal structure? A simple document archive may need no definition at all.
- Does definition belong to the archive tier or the content tier?

---

## Implications for archive init

If identity and provenance are the only creation concerns, `archive init` is simpler than the old tool:

1. Read `archive-preseed.yml`
2. Validate language tag against IANA registry
3. Create archive directory (already done by `collection init`)
4. Write DC sidecar with identity fields
5. Write `.language.yml` with language provenance
6. Write provenance record with SAT version, tool, timestamp
7. Hand off to content tier via `content-preseed.yml.example`

The tree structure — `create_tree` in the old tool — moves to content ingress or a separate definition step.

---

## References

- ADR-009: Distribution by Installer and Instantiation
- ADR-001: Language as Filesystem Structure
- ADR-005: Tool Self-Discovery from Filesystem Context
- Old implementation: `en/bin/archives/archive-init.py`
- Old definition example: `en/bin/archives/definitions/archives/chrissteel.com-en.yml`

# Dual-Mode Language Authority: Standard-Aware, Not Standard-Dependent

## What this defines

A three-state language interpretation system:

**Step 1 — Detection.** Check whether a directory name matches a known standard (ISO 639, BCP 47, or derived compound forms).

**Step 2 — Resolution.**

- **Case A (match):** trigger a disambiguation prompt asking the user to confirm intended meaning, preventing silent misclassification (e.g., `fr` vs `fr-CA` vs `fr-fr` drift).
- **Case B (no match):** set `language_authority = none`. The system does not enforce an external linguistic ontology; the label is treated as a local semantic value.

## Correction to earlier framing

This is not legacy compliance logic. It is a dual-mode epistemic system: standard-aware but not standard-dependent.

## Architecture

- **Standard-aware mode:** ISO/BCP 47 recognized → validation + confirmation
- **Non-standard mode:** free-form labels, no external authority binding

Three components:

- **Deterministic normalization layer:** maps directory name → language state
- **Bounded ambiguity resolution:** triggered only on collisions or ambiguity
- **Split ontology model:** standard languages validated; non-standard passed through as semantic labels

## Strengths

1. **Prevents silent misclassification** — no false assumption of correctness; explicit confirmation on ambiguity
2. **Preserves innovation** — franglais, hybrid dialects, constructed languages stay valid
3. **Avoids standard lock-in** — no dependence on external registries; still interoperable where standards apply
4. **Explicit epistemic boundary** — "we know when we are inside a standard system, and when we are not"

## Risks

1. **User friction** — ambiguous cases force manual resolution
2. **Mixed-state archives** — a repository mixing standard, non-standard, and hybrid labels increases interpretation variance
3. **`language_authority = none` is powerful but undefined** — it creates a semantic class (intentionally unclassified languages) that tools must handle without external grounding

## Key insight

Not language classification, but a dual ontology: canonical (standard) + emergent (non-standard).

## Structural implication

Standards are advisory, not governing; authority is conditional, not absolute; interpretation is context-sensitive.

## Summary

ADR-002 defines a dual-mode language interpretation system: standard tags trigger validation and confirmation, non-standard labels carry no external authority, enabling both standards interoperability and unconstrained hybrid language systems.
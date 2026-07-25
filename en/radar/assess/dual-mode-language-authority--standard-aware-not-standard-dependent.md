# dual-mode-language-authority--standard-aware-not-standard-dependent

# 1. What you are actually defining (precisely)

You are introducing a **three-state language interpretation system**:

## Step 1 — Detection

System checks:

- Is directory name in known standards?
  - ISO 639
  - BCP 47
  - derived compound forms

## Step 2 — Resolution

### Case A: matches known standard(s)

→ system triggers **disambiguation prompt**

- asks user to confirm intended meaning
- prevents accidental misclassification (e.g., `fr` vs `fr-CA` vs `fr-fr` usage drift)

### Case B: no match

→

```text
language_authority = none
```

Meaning:

- system does NOT enforce external linguistic ontology
- language is treated as **local semantic label**

------

# 2. What this changes (important correction to earlier framing)

This is not “legacy compliance logic”.

It is:

> a **dual-mode epistemic system: standard-aware but not standard-dependent**

------

# 3. The actual architecture you are building

You now have:

## A. Standard-aware mode

- ISO / BCP47 recognized
- triggers validation + confirmation

## B. Non-standard mode

- free-form linguistic system labels
- no external authority binding

------

# 4. What is good about this (factually)

## 1. Prevents silent misclassification

- avoids “false assumption of correctness” in language tagging
- forces explicit confirmation when ambiguity exists

------

## 2. Preserves innovation in language modeling

- `franglais`, hybrid dialects, constructed languages remain valid
- no forcing into ISO categories

------

## 3. Avoids standard lock-in

- system is not dependent on external registries
- still interoperable when standards apply

------

## 4. Explicit epistemic boundary

This is key:

> “We know when we are inside a standard system, and we know when we are not”

That is rare and powerful.

------

Instead of “increased tooling complexity”, what you actually introduce is:

## A. A deterministic normalization layer

- maps directory name → language state

## B. A bounded ambiguity resolution step

- only triggered on collisions or ambiguity

## C. A split ontology model

- standard languages → validated
- non-standard → pass-through semantic labels

# 5. What is risky or difficult (factually)

2. User friction in ambiguous cases

When system detects overlap:

- user must resolve intent
- introduces interaction overhead

------

## 3. Mixed-state archives become more complex

A single repository may contain:

- standard languages
- non-standard labels
- hybrid systems

This increases interpretation variance.

------

## 4. “language_authority = none” is powerful but undefined

It introduces a semantic class:

> languages that are intentionally outside classification systems

But:

- tools must decide how to process them
- no external grounding exists

------

# 6. The key conceptual insight

You are not defining language classification.

You are defining:

> a **dual ontology system: canonical (standard) + emergent (non-standard)**

------

# 7. Important structural implication

This system implies:

- standards are **advisory, not governing**
- authority is **conditional, not absolute**
- interpretation is **context-sensitive**

------

# 8. One-line summary

ADR-002 defines a dual-mode language interpretation system where standard language tags trigger validation and confirmation, while non-standard labels are explicitly treated as having no external authority, enabling both interoperability with linguistic standards and the emergence of unconstrained hybrid language systems.
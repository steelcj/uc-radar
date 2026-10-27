# Radar

The radar is a register of things under consideration for Universal Cake and its projects, including SAT: tools, protocols, apps, techniques, and processes, before they settle into the documentation proper. It is a triage space, not a home. An entry sits on the radar only while its fate is undecided.

## Rings

The radar has four rings, each a directory:

- **assess/**, under evaluation, including testing. Assess asks what a product does. An entry moves on to trial or adopt only when its gates and Security section are Verified, or a named person has recorded an override
- **trial/**, optional real use in a limited, named scope, with an end condition set before it starts. Trial asks how a product lives with us, and is used only when real use can answer something testing cannot
- **adopt/**, accepted; the decision is yes
- **hold/**, not now, until a named condition changes

Rejected entries are archived outside the rings. The ring states where an entry stands; the reasons are in the entry and its `uc-radar_evaluation` block. The [UC Radar Evaluation Lifecycle](uc-radar--evaluation-lifecycle-v0.2.0.md) defines the states and transitions, and the [Universal Cake Evaluation Metrics](universal-cake-evaluation-metrics-v0.6.0.md) say what each one requires of the evaluation.

## Adoption graduates out

`adopt/` is a transit state, not storage. Once an entry is adopted its idea moves to its permanent home elsewhere in `en/docs/`, such as `language/`, `architecture/`, `guides/`, or `specifications/`, and the radar entry leaves. The radar holds only what is still in question, so a healthy `adopt/` is often empty.

## Categories

The radar has no category subdirectories. What kind of thing an entry is lives in its `Category` field (see the entry template), and is ultimately expressed by where it graduates to. Rings classify the decision; the category and destination classify the kind.

## What this radar omits

This radar departs deliberately from common radar practice in one way:

- **No `decisions/` folder.** The ring already records the decision and git records its history; a separate log would duplicate both.

## Rationale travels with the entry

Nothing moves to `adopt/` or `hold/` without a status note stating why. The verdict is the folder, the reasoning is in the entry, the history is git. When an entry graduates, its rationale travels into the destination document rather than being left behind.

## File naming

Radar entries diverge from the SAT file-naming convention. An entry filename is its slug joined to a short tagline by a double hyphen, with no version suffix:

```text
dual-mode-language-authority--standard-aware-not-standard-dependent.md
```

The double hyphen separates the blip slug from its tagline so an entry can be identified on the radar without opening it. Versions are not suffixed; revision history lives in git. This is a deliberate local override of the File Naming rule in the documentation style guide.

## Entry template

New entries are based on the [UC Radar Entry Template](uc-radar-entry-template-v0.3.0.md) in this directory.

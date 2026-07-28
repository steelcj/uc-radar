# Radar

The radar is a register of things under consideration for SAT — tools, protocols, apps, and techniques — before they settle into the documentation proper. It is a triage space, not a home. An entry sits on the radar only while its fate is undecided.

## Rings

The radar has three rings, each a directory:

- **assess/** — under evaluation; we are looking at it and weighing its fit
- **adopt/** — accepted; the decision is yes
- **hold/** — rejected, or not now

The ring is the decision. An entry's location states the verdict; no separate decision record is kept.

## Adoption graduates out

`adopt/` is a transit state, not storage. Once an entry is adopted its idea moves to its permanent home elsewhere in `en/docs/` — `language/`, `architecture/`, `guides/`, `specifications/`, and so on — and the radar entry leaves. The radar holds only what is still in question, so a healthy `adopt/` is often empty.

## Categories

The radar has no category subdirectories. What kind of thing an entry is lives in its `Category` field (see the entry template), and is ultimately expressed by where it graduates to. Rings classify the decision; the category and destination classify the kind.

## What this radar omits

This radar departs deliberately from the conventional four-ring model:

- **No `trial` ring.** Trial marks piloting between assessment and adoption. SAT has no such stage — an item is under assessment, adopted, or held, and `assess` covers the gap.
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

New entries are based on `radar-entry-template.md` in this directory.

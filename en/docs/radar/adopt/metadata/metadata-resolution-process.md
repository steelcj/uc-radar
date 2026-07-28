Not quite yet — there's more in 1b than the three items I mentioned, and deleting it now would lose track of two of them.

Look at the summary table again:

| Issue                            | Status                                                       |
| -------------------------------- | ------------------------------------------------------------ |
| 2.1 language ISO 639-1/639-2     | "Resolved in design, **correction pending in defaults file**" |
| 2.2 contributor empty string     | Open                                                         |
| 2.3 rights label vs URI          | Open                                                         |
| 2.4 type absent at archive level | "**Partially** resolved"                                     |
| 2.5 licence spelling/field name  | Open                                                         |
| 2.6 format MIME type             | Resolved, no action                                          |
| 2.7 publisher default            | Open                                                         |

Only 2.6 is genuinely done with nothing left to do. 2.1 and 2.4 say "resolved in design" — but that means the *decision* is settled, not that the *file* has been fixed; both still need an actual tooling correction applied to `default-canonical-metadata.yml`. If 1b disappears before that correction lands, the fact that it's pending disappears with it. 2.5 is also real, though it's a stray item — not a Dublin Core question at all, just a config-file naming/spelling fix that happened to surface during this audit.

So that's six live items, not three: 2.1, 2.2, 2.3, 2.4, 2.5, 2.7.

**Sequence I'd suggest:**

1. Log all six to the UC Kanban backlog (tag `sat`), not just the three "Open" ones — 2.1 and 2.4 as tooling-correction items, 2.5 flagged as unrelated-but-real.
2. Only then remove 1b from the radar — at that point everything actionable in it has a forwarding home, and git history keeps the full audit if anyone needs the original reasoning later.
3. Update entry 1's status notes (the ones I drafted) to say 1b **was** folded in and removed, rather than pointing at a file that no longer exists.

Want me to write out all six backlog items in the board's format now?
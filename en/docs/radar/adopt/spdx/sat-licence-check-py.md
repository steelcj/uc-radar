# SAT-licence-check.py

Absolutely — a licence audit and normalisation tool. It fits cleanly in the SAT toolchain:

```
sat-tools/
  content/
    licence-check.py      ← reads every .md/.py in the repo
                            identifies missing or incorrect licence blocks
                            reports: file, current licence (if any), expected licence
    licence-apply.py      ← applies the correct AGPL-3.0 header block
                            preserves AI attribution lines where present
                            idempotent — skips files already correct
    scripts/
      nix/
        licence-check
        licence-apply
```

Though it raises an interesting scope question immediately — what tier does it live in? Licence governance touches every tier of the repository. That feels like **SAT Admin** (`bin/sat/`) rather than content, since it operates on the repository structure itself rather than on content documents.

And it would need to respect the distinction you drew — AGPL-3.0 for repository files (tools, ADRs, specifications), CC BY-SA 4.0 or operator-defined licence for SAT content archives. So it needs to know which files are which, which means it needs to understand the SAT filesystem structure — collection roots, archive boundaries, content directories — to apply the right licence to the right file.

Which means it depends on `sat_walk.py` from the shared lib. Which we haven't written yet.

So the dependency order is becoming clear:

```
1. sat_walk.py          ← shared lib, no deps
2. sat_config.py        ← shared lib, reads ~/.config/sat/
3. sat_cascade.py       ← shared lib, depends on sat_walk + sat_config
4. hugo-transmog.py     ← depends on 1, 2, 3
5. hugo-assemble.py     ← depends on 1, 2
6. licence-check.py     ← depends on 1 (needs to understand archive boundaries)
7. licence-apply.py     ← depends on 6
```

Shall we start at the top and write `sat_walk.py`?


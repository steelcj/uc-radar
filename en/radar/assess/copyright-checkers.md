Before ingressing opensource content into SAT you'd want to know what licences you're dealing with — some are compatible with your CC BY-SA 4.0 and some are not.

There are already well-established tools for this rather than building one:

**[licensee](https://github.com/licensee/licensee)** — Ruby, used by GitHub itself to detect licences. Very mature.

**[scancode-toolkit](https://github.com/nexB/scancode-toolkit)** — Python, the most thorough option. Detects licences, copyright notices, and SPDX identifiers at the file level across an entire directory tree. Used by major open source foundations.

**[REUSE](https://reuse.software/)** — a specification and tool from the FSFE that enforces licence clarity at the file level using SPDX headers. More of a compliance framework than a sniffer, but highly relevant if you want SAT archives to be formally licence-compliant.

For SAT I'd suggest:

- **scancode-toolkit** for ingress scanning — run it against a source directory before or during `content-ingress.py` to produce a licence report as part of the nursery staging process
- **REUSE** as a longer term standard for how SAT archives declare their own licence metadata — it fits naturally with the Dublin Core sidecar approach you already have

Both are Python-native which fits the SAT toolchain. scancode-toolkit would be a natural addition to `content-ingress.py` as an optional `--scan-licences` flag.

So the radar now has:

```
docs/
└── radar/
    ├── flowmark.md
    ├── myst-parser.md
    ├── scancode-toolkit.md
    └── reuse.md
```

When you're ready it would be worth defining a short radar entry template so each of these documents captures the same information consistently — name, category, status, why it's interesting, concerns, and links. Want to sketch that template out now or keep moving on other things?
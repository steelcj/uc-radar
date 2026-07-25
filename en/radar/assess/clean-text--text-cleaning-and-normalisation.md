---
dc:title: "clean-text — Python package for text cleaning and normalisation"
dc:creator: "<calculated>"
dc:contributor: "<calculated>"
dc:subject:
  - "radar"
  - "text-cleaning"
  - "normalisation"
  - "nlp"
  - "python"
dc:description: "Radar entry assessing clean-text, a Python package for cleaning and normalising scraped or user-generated text, for the SAT pipeline."
dc:publisher: "<calculated>"
dc:date: "<calculated>"
dc:modified: "<calculated>"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
dc:language_bcp47: "en-GB"
dc:source: "<calculated>"
dc:relation: "<calculated>"
dc:identifier: "<calculated>"
dc:rights: "<calculated>"
---

# clean-text--text-cleaning-and-normalisation

## What it is

clean-text is an Apache-licensed Python package that turns dirty, user-generated or scraped text into a normalised plain-text representation. It combines ftfy for unicode repair, optional unidecode transliteration to ASCII, and a large set of hand-written regular expressions. Through a single `clean()` call — with a `clean_texts()` batch form and a scikit-learn-compatible `CleanTransformer` — it can fix mojibake, transliterate accented characters, lowercase text, normalise or strip line breaks, and replace URLs, emails, phone numbers, IP addresses, file paths, numbers, currency symbols, and code snippets with placeholder tokens. English and German have dedicated handling; most western languages work adequately.

## Why interesting

It is a ready-made, configurable normaliser for the messy-text problem. Where a pipeline ingests scraped pages, social copy, or OCR output and needs a consistent, analysis-ready representation, clean-text covers the common cases without bespoke regex. The token-replacement options make it useful for redaction-style normalisation and for preparing text for search indexing, deduplication, or machine-learning features. The `exceptions` parameter preserves specified patterns verbatim, and `clean_texts(n_jobs=...)` parallelises across cores for bulk corpora.

## Concerns

- Wrong tool for Markdown documents. clean-text is a destructive plain-text preprocessor, not a Markdown normaliser. It lowercases, transliterates, and tokenises content — in the project's own example a link `[Moana](https://…)` becomes `[moana](<URL>)`. Pointed at the SAT document corpus it would corrupt structure, links, and casing. It belongs, if anywhere, on an ingestion or indexing path that derives a separate normalised text representation, never on the canonical Markdown. This is the central fit issue and the reason the entry sits in assess rather than near the document toolchain.
- GPL dependency by opt-in. The best transliteration uses unidecode, which is GPL. `pip install clean-text[gpl]` pulls GPL in; the default install avoids it and falls back to Python's `unicodedata.normalize`, which the project itself notes is inferior and produces output inconsistent with the unidecode path. For a compliance-sensitive project, standardise on one mode and record the licence choice.
- Lossy and irreversible. Every transform discards information — case, punctuation, exact URLs. Keep the original; treat clean-text output as a derived artefact, not a replacement.
- Language coverage. Only English and German are fully supported; other languages are best-effort.
- Maturity. Healthy and recently released (v0.7.1, January 2026) and widely used (around 1k stars), but still pre-1.0, effectively single-maintainer, and built on hand-tuned regex whose edge cases can shift between releases. Pin the version.

## Security assessment

clean-text is a local, in-process Python library. It transforms strings passed to it and performs no network activity; on its own it neither reads from nor writes to the filesystem.

**Network behaviour:**
- [ ] Outbound connections during normal use
- [ ] Update checks — silent or explicit
- [ ] Telemetry or usage data — present or absent
- [ ] Licence server contact — frequency and data sent
- [ ] Does have any affect on networking or items created that will be transferred over a network
- [ ] Helps ensure that documents passed via Network are clean and compliant

**File system behaviour:**
- [ ] Creates hidden or metadata files alongside content
- [ ] Caches content outside the working directory
- [ ] Stores recent file history outside the archive
- [ ] Writes to unexpected locations

Note: clean-text operates on in-memory strings; any file reading or writing is the caller's responsibility, not the library's.

**Content exposure:**
- [ ] Sends any content to a remote service
- [ ] Stores content in a cloud service by default
- [ ] Auto-save or backup features that copy content externally

**Assessment method:**
Review of the published documentation and the project's source and dependencies (ftfy, unidecode or text-unidecode), not a live packet capture. Version reviewed: 0.7.1 (released 28 January 2026). Date: 31 May 2026. The dependency to watch is unidecode (GPL), pulled in only via the `[gpl]` install extra.

**Assessment status:** Clear on network and data-movement grounds — local, in-process, no I/O of its own. The outstanding item is licence governance around the optional GPL dependency, tracked under Concerns rather than as a security finding.

## Relationship to project (SAT as an example)

Dev toolchain / content tools, but on an ingestion or analysis path, not the Markdown document path. If adopted it would normalise derived text representations — for example, cleaning extracted body text for search indexing or analysis — while leaving the canonical Markdown untouched. It does not map to the engine tier. Its natural destination on graduation, if any, is a pipeline or guide describing text ingestion or indexing in `en/docs/guides/`, not the documentation style guide.

## Status notes

In assess.

- Last reviewed: 2026-05-31
- Why this ring: clean-text is capable and maintained, but its fit for SAT is unproven and sharply bounded — it must not touch canonical Markdown. It needs a defined home before any adopt decision.
- What would move it to adopt: a concrete pipeline stage that needs plain-text normalisation of derived content, a settled licence decision on the GPL extra, and confirmation that originals are always retained alongside cleaned output.
- What would move it to hold: no such ingestion need materialising, or the requirement being met more simply (for example, ftfy alone for unicode repair, without the heavier tokenisation and lowercasing).
- Note on overlap: for the Markdown document pipeline the adopted normaliser is mdformat. clean-text is not an alternative to mdformat but a different tool for a different, text-corpus job.

## Links

- https://github.com/jfilter/clean-text
- https://pypi.org/project/clean-text/

## License (for this document)

TODO — set to the SAT documentation licence. clean-text itself is Apache-licensed; the optional unidecode dependency is GPL — see Concerns.

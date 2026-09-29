# ruamel.yaml-python-yaml-parser-and-emitter

`ruamel.yaml` is a **Python YAML parser and emitter** designed for working with YAML while preserving much more of the original document than conventional YAML libraries.

It is particularly useful when you're **editing YAML configuration or metadata files programmatically**, rather than merely loading YAML into Python objects.

### The important distinction

With a basic YAML library, you might do:

```python
import yaml

data = yaml.safe_load(open("archive.yml"))
data["title"] = "New title"

with open("archive.yml", "w") as f:
    yaml.safe_dump(data, f)
```

The problem is that loading and dumping can substantially rewrite the file:

- comments may disappear
- formatting changes
- quoting may change
- ordering may change
- YAML constructs may be normalized

`ruamel.yaml` was designed specifically to avoid much of this.

For example:

```yaml
# Archive metadata
title: "My Archive"
language: en
description: >
  A multilingual source archive.

# Keep this field
status: draft
```

You can load it, modify `title`, and dump it again while retaining comments and much of the original presentation.

```python
from ruamel.yaml import YAML

yaml = YAML()

with open("archive.yml") as f:
    data = yaml.load(f)

data["title"] = "My New Archive"

with open("archive.yml", "w") as f:
    yaml.dump(data, f)
```

### Why `ruamel.yaml` is interesting for SAT

For your **SAT archive-definition YAML**, I think `ruamel.yaml` is considerably more appropriate than PyYAML if SAT is going to **generate and subsequently modify human-maintained YAML**.

In particular, it gives you:

| Capability              | PyYAML              | `ruamel.yaml`     |
| ----------------------- | ------------------- | ----------------- |
| Parse YAML              | ✓                   | ✓                 |
| Generate YAML           | ✓                   | ✓                 |
| Preserve comments       | generally no        | **yes**           |
| Preserve ordering       | modern Python helps | **yes**           |
| Round-trip editing      | poor                | **core strength** |
| Preserve quoting/style  | limited             | **much better**   |
| YAML 1.2 support        | limited             | **yes**           |
| Human-maintained config | okay                | **excellent**     |
| Machine-generated YAML  | excellent           | excellent         |

The **round-trip** capability is probably the key feature for SAT.

### It also has a useful mental model

`ruamel.yaml` doesn't simply treat YAML as:

```text
YAML → Python dictionary → YAML
```

It can retain presentation information alongside the data:

```text
             ┌────────────────────┐
             │       YAML         │
             │                    │
             │ data + comments    │
             │ + formatting       │
             │ + ordering         │
             └─────────┬──────────┘
                       │
                    ruamel.yaml
                       │
                       ▼
             ┌────────────────────┐
             │ Python data model  │
             │ + YAML metadata    │
             └─────────┬──────────┘
                       │
                    modify
                       │
                       ▼
             ┌────────────────────┐
             │       YAML         │
             │                    │
             │ largely preserved  │
             └────────────────────┘
```

That makes it a good fit for a tool where **the YAML itself is part of the user-facing interface**, rather than an opaque serialization format.

### One caveat

`ruamel.yaml` is more sophisticated than PyYAML, which also means its API and behavior are more complex. You generally want the modern API:

```python
from ruamel.yaml import YAML

yaml = YAML()
```

rather than older examples you'll find using things such as:

```python
from ruamel.yaml import round_trip_load
```

Those older APIs are associated with the library's earlier interface.

For SAT specifically, I'd seriously consider making **`ruamel.yaml` the canonical YAML implementation** if the new version is intended to manipulate archive definitions while respecting human authorship of those files. It aligns particularly well with your language-first/metadata-oriented model: **SAT manages the structure without treating human-authored YAML as disposable generated output.**
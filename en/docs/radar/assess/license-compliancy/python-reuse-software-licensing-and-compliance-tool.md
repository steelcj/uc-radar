# Python reuse -- software licensing and compliance tool

## Description

`reuse` is a Python-based command-line tool developed by the Free Software Foundation Europe (FSFE) to help software projects comply with the **REUSE Specification**.

REUSE provides a standardized approach to documenting copyright and licensing information so that both humans and automated tools can determine how individual files may be used, modified, and redistributed.

The Python `reuse` package is therefore not primarily a general-purpose Python library for manipulating reusable code. It is a **software licensing and compliance tool**.

The current PyPI release is `reuse 6.2.0`. It requires Python 3.10 or later.

## Why it matters

Copyright and licensing information is frequently difficult to maintain consistently in a software repository.

A project may contain:

- source code;
- documentation;
- configuration;
- generated files;
- images;
- data;
- vendored code;
- third-party material;
- files covered by different licenses.

A single `LICENSE` file does not necessarily communicate which license applies to every individual file.

REUSE addresses this by making licensing information explicit and machine-readable.

The basic model is:

```
project
├── source files
│   ├── SPDX copyright information
│   └── SPDX license information
│
├── LICENSES/
│   ├── GPL-3.0-or-later.txt
│   ├── CC-BY-SA-4.0.txt
│   └── ...
│
└── REUSE.toml
```

The `reuse` tool then validates the repository against the REUSE recommendations.

## SPDX

REUSE makes extensive use of **SPDX identifiers**.

For example:

```
SPDX-License-Identifier: GPL-3.0-or-later
```

and:

```
SPDX-FileCopyrightText: 2026 Christopher Steel
```

A Python source file might therefore begin with:

```
# SPDX-FileCopyrightText: 2026 Christopher Steel
#
# SPDX-License-Identifier: GPL-3.0-or-later
```

This allows licensing information to remain attached to the file itself rather than existing only in external documentation.

## Core workflow

The REUSE workflow can be summarized as:

```
Choose licenses
       │
       ▼
Add copyright information
       │
       ▼
Add SPDX license identifiers
       │
       ▼
Provide license texts
       │
       ▼
Run reuse lint
       │
       ▼
Compliant repository
```

The official REUSE recommendations describe three fundamental activities:

1. Choose and provide licenses.
2. Add copyright and licensing information to each file.
3. Verify REUSE compliance.

The `reuse` command-line tool exists to automate and validate this process.

## Installation

The project recommends installation through a system package manager or `pipx`.

For a developer workstation:

```
pipx install reuse
```

The tool can also be executed without a persistent installation:

```
pipx run reuse lint
```

The current release requires Python 3.10 or later. Git and other version-control systems are optional dependencies, while `libmagic` improves file-type and encoding detection.

## Basic commands

### Check a repository

```
reuse lint
```

This is the primary compliance check.

### Check a single file

```
reuse lint-file path/to/file.py
```

### Add licensing information

```
reuse annotate \
  --copyright="2026 Christopher Steel" \
  --license="GPL-3.0-or-later" \
  path/to/file.py
```

### Download a license text

```
reuse download GPL-3.0-or-later
```

This places the corresponding license text in the project's `LICENSES/` directory.

### Generate an SPDX document

```
reuse spdx
```

The resulting SPDX document provides a machine-readable representation of the repository's licensing information.

The tool also provides commands for listing supported licenses and converting legacy `.reuse/dep5` information to `REUSE.toml`.

## `REUSE.toml`

REUSE can represent licensing information through a `REUSE.toml` file.

This is particularly useful when information cannot conveniently be placed directly into a file header.

Conceptually:

```
version = 1

[[annotations]]
path = "assets/**"
SPDX-FileCopyrightText = "2026 Christopher Steel"
SPDX-License-Identifier = "CC-BY-SA-4.0"
```

This allows a repository to describe licensing information for groups of files without modifying the files themselves.

## License files

REUSE expects license texts to be available in the `LICENSES/` directory.

For example:

```
LICENSES/
├── BSD-3-Clause.txt
├── CC-BY-SA-4.0.txt
└── GPL-3.0-or-later.txt
```

The filenames correspond to SPDX license identifiers.

This provides a useful separation:

```
SPDX identifier
      │
      ├── identifies the license
      │
      ▼
LICENSES/
      │
      └── contains the authoritative license text
```

## `reuse lint`

The most important command is:

```
reuse lint
```

It examines the repository and identifies files whose copyright or licensing information does not satisfy the REUSE recommendations.

A successful check reports that the project is compliant with the applicable REUSE Specification.

This makes `reuse lint` particularly suitable for continuous integration.

## Continuous integration

A project can make REUSE compliance part of its normal development workflow.

For example:

```
name: REUSE

on:
  push:
  pull_request:

jobs:
  reuse:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: fsfe/reuse-action@v5
```

The exact CI action version should be pinned according to the project's current security and dependency-management policy.

The REUSE project also provides a Docker image and a `pre-commit` integration.

## Pre-commit

REUSE can be integrated into a project's `pre-commit` configuration.

For example:

```
repos:
  - repo: https://github.com/fsfe/reuse-tool
    rev: v6.2.0
    hooks:
      - id: reuse
```

This allows licensing compliance to become part of the normal commit workflow.

The official project documents both a full `reuse` hook and a `reuse-lint-file` hook for checking only changed files.

## Dependencies

The `reuse` package is intentionally relatively small in scope.

Its principal runtime requirement is:

```
Python >= 3.10
```

Additional functionality can use:

- Git;
- Mercurial;
- Pijul;
- Jujutsu;
- `libmagic`.

These additional dependencies are optional. Without a supported VCS, for example, `reuse` cannot fully account for files ignored by that VCS.

## Licensing of the reuse project

The `reuse` project itself uses multiple licenses.

According to its current PyPI documentation:

- original source code: `GPL-3.0-or-later`;
- documentation: `CC-BY-SA-4.0`;
- some configuration and data files: `CC0-1.0`;
- some code derived from `spdx/tools-python`: `Apache-2.0`.

Individual files should therefore be treated according to their own licensing information rather than assuming that every file in the repository has the same license.

## Relationship to SPDX

REUSE and SPDX solve related but distinct problems.

**SPDX** provides standardized identifiers and formats for representing software licenses and related information.

**REUSE** provides practical recommendations and tooling for applying that information consistently to a software repository.

The relationship can therefore be understood as:

```
SPDX
  │
  │ standardized identifiers
  ▼
REUSE Specification
  │
  │ repository conventions
  ▼
reuse tool
  │
  │ validation and generation
  ▼
software repository
```

This distinction is useful when designing project tooling: adopting REUSE does not mean adopting a particular software license. It means adopting a structured method for declaring and verifying licensing information.

## Why use it in a project

`reuse` is particularly valuable when a project:

- contains multiple types of files;
- uses more than one license;
- incorporates third-party material;
- distributes source code;
- wants machine-readable licensing information;
- wants licensing checks in CI;
- wants to generate SPDX metadata;
- wants licensing information attached to individual files;
- needs a repeatable licensing process for contributors.

It is less useful for a tiny private project where licensing is irrelevant or where all files are trivially covered by a single explicit license.

## SAT relevance

For a project such as **Source Archive Tools (SAT)**, REUSE is potentially a good fit because SAT is fundamentally concerned with structured source archives and metadata.

A SAT repository could use REUSE to distinguish:

```
SAT source code
        │
        ├── GPL-3.0-or-later
        │
        ├── documentation
        │
        ├── archive metadata
        │
        ├── examples
        │
        └── third-party material
```

The licensing information would then be explicit at the file level rather than being inferred from the repository's top-level `LICENSE` file.

This becomes particularly useful if SAT eventually handles multilingual archives, imported source material, generated metadata, or other content whose licensing status differs from the SAT software itself.

## Recommended adoption model

For a new Python project, a reasonable baseline is:

```
LICENSES/
    <SPDX license>.txt

REUSE.toml

src/
    ...
```

with SPDX headers in source files:

```
# SPDX-FileCopyrightText: 2026 Christopher Steel
#
# SPDX-License-Identifier: <SPDX identifier>
```

and a CI requirement:

```
reuse lint
```

The licensing decision itself should remain independent of REUSE adoption.

For example, if SAT is ultimately licensed under GPL-3.0-or-later:

```
# SPDX-License-Identifier: GPL-3.0-or-later
```

REUSE records and validates that decision; it does not make the licensing decision.

## Assessment

**Recommendation: Adopt**

`reuse` is a focused, mature solution for repository-level copyright and license metadata. Its use of SPDX identifiers provides a standardized vocabulary, while `reuse lint` makes the resulting metadata testable.

For projects where licensing is part of the project's architecture rather than an afterthought, this is a strong fit.

### Technology Radar

| Dimension              | Assessment                                  |
| ---------------------- | ------------------------------------------- |
| Technology             | Python `reuse`                              |
| Category               | Developer tooling / Software supply chain   |
| Purpose                | Copyright and license compliance            |
| Maturity               | Production                                  |
| Current version        | 6.2.0                                       |
| Python                 | 3.10+                                       |
| Specification          | REUSE 3.3                                   |
| License                | Multiple; core source code GPL-3.0-or-later |
| Recommendation         | Adopt                                       |
| Primary command        | `reuse lint`                                |
| Primary standard       | SPDX                                        |
| CI suitability         | Excellent                                   |
| Pre-commit suitability | Excellent                                   |

## Resources

- [REUSE](https://reuse.software/) — FSFE project and specification.
- [REUSE documentation](https://reuse.readthedocs.io/) — installation, tutorial, specification, and integration guidance.
- [REUSE source repository](https://github.com/fsfe/reuse-tool) — source code and development.
- [REUSE on PyPI](https://pypi.org/project/reuse/) — Python package releases and metadata.
- [SPDX](https://spdx.dev/) — standardized software package and licensing information.

## References

Free Software Foundation Europe. (n.d.). *REUSE*. REUSE. https://reuse.software/

Free Software Foundation Europe. (n.d.). *reuse*. PyPI. https://pypi.org/project/reuse/

Free Software Foundation Europe. (n.d.). *REUSE documentation*. Read the Docs. https://reuse.readthedocs.io/

SPDX Workgroup. (n.d.). *SPDX: System Package Data Exchange*. SPDX. https://spdx.dev/
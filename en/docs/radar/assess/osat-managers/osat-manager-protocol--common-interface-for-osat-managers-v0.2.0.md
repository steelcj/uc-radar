---
dc:title: "OSAT Manager Protocol, a Common Interface for OSAT Managers"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "radar"
  - "osat"
  - "osat-managers"
  - "protocol"
  - "osat-manager-core"
  - "dependencies"
dc:description: "Assess-ring evaluation of a common, machine-readable interface that every OSAT manager exposes, so that a dispatcher can eventually run `osat install sat 1.23` and have every requirement installed on Linux, macOS and Windows. Records where the managers stand on 2026-10-27, what uc-automa has already decided, the gaps a cascade needs filled, the options considered, and a proposed protocol 1."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-27"
dcterms:modified: "2026-10-28"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "universal-cake-evaluation-metrics, uc-radar--evaluation-lifecycle, git-on-windows--options-for-osat-manager-git, universal-cake-evaluation--mise-en-place--polyglot-tool-version-manager, creating-an-osat-manager-repository, osat-manager-python, osat-manager-restic, osat-manager-git, osat-manager-core, osat-fluent-sat-tool"
dc:identifier: "osat-manager-protocol--common-interface-for-osat-managers"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/osat-managers/"
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-10-28"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      Records Christopher Steel's decisions of 2026-10-28 on the open
      questions of 0.1.1. The existing managers are the planned list, so
      the paper check is complete; slugify becomes part of SAT under
      osat-manager-pyapp. Repositories not named osat-manager-<tool> are
      retired once a replacement exists. --remove of a version with a
      running service needs --force. Usage errors display help.
      Consumers keep their dependencies, reported as uses in their own
      status, replacing provider-kept holds and the --hold and --release
      verbs. Requirements live in requires.yaml beside each version's
      lock. osat-manager-core ships protocol 1 in its first release, the
      schema freezes at this entry's final draft, every manager except
      manage-python is built to it from its first release, and
      manage-python migrates, fixing its layout drift in the same change.
      Trial pair changed to manage-python and manage-restic, both working;
      manage-git is not yet. New automated pass added.
  - version: "0.1.1"
    date: "2026-10-28"
    author: "Christopher Steel, renaming; Claude Opus 5.5 (Anthropic), content"
    notes: >
      Christopher Steel renamed the draft to 0.1.1 because it contained
      changes made after 0.1.0; its version block still read 0.1.0.
      Eleven earlier managers back to June 2026 read on 2026-10-28; six
      schema changes they required: commands per version, install and
      run requirements with system providers, version ordering, upstream
      version, unowned data and per-platform answers. Two findings outside
      the schema: osat-tool already installs a command named osat, and
      the manager runtime cannot reach rclone's BSD and Solaris builds.
      osat-fluent-gh-tool corrected as proposed but never built. The
      constraint parser is implemented in osat-manager-core, since the
      standard library has none. One command, osat, set out as a thin
      front end over the managers, in a pass-through and a cascade stage.
  - version: "0.1.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      Initial Assess-ring entry. Snapshot of manage-python 1.0.2,
      manage-restic 0.1.0, manage-git 0.1.0 and install-sat.py 0.4.0
      read from their repositories on 2026-10-27. Proposes protocol 1,
      delivered through osat-manager-core, with describe, uniform verbs,
      one object with two renderings, a provisional schema,
      machine-readable status, checks against a version constraint,
      declared requirements, holds, bundles and an invocation contract.
      Nothing has been built or run.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # protocol 1 delivered through osat-manager-core, tried first between manage-python and manage-git
      reasoning: "Most of the contract already exists: the capability table in Creating an OSAT Manager Repository 0.6.0, the shared package (uc-automa 0035), the independent runtime (0033) and check exit statuses (0036). What a cascade still lacks is machine-readable and versioned: a way to ask a manager what it speaks, its state as JSON, a check against a version constraint, the requirements of a given version, a record of who holds a version, an offline bundle, and a stated way to start an installed version. Defining these once in osat-manager-core, with a conformance suite, costs little beyond work already planned and keeps every manager a standard-library CLI. The managers have already drifted in layout, status vocabulary, exit status on usage errors and message catalogues, which is the strongest argument for fixing the interface before a dispatcher depends on it. Risk is moderate: the protocol binds every manager, and a breaking change needs a coordinated release. Nothing is built, so every gate and the Security pillar are Inferred; Trial is proposed in one named place to make them Verified."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-10-27T15:45:00-04:00"
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # protocol 1 in osat-manager-core's first release, tried between manage-python and manage-restic before the final draft
      reasoning: "Re-evaluated after Christopher Steel's decisions of 2026-10-28. The paper check is complete, since the existing managers are the planned list, and protocol 1 now covers every kind of tool they manage. Consumer-kept uses remove the one place where managers wrote into each other's state, which lowers the security exposure the 0.1.0 pass noted. Shipping the protocol in osat-manager-core's first release, with every manager except manage-python built to it from the start, avoids a migration per manager but freezes the schema in one step, so the trial before the final draft carries more weight. manage-git cannot yet be one of the trial pair, so the pair is manage-python and manage-restic, which work and use different source kinds. Nothing is built; gates and Security remain Inferred."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.6.0"
      evaluation_datetime: "2026-10-28T11:55:00-04:00"
  human: []
---

# OSAT Manager Protocol, a Common Interface for OSAT Managers

Version: 0.2.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

The goal is a cascade: a person runs `osat install sat 1.23`, and everything sat 1.23 needs, a Python runtime, git, sat itself, is installed into their own account on Linux, macOS or Windows, by the managers that own each tool. That only works if every manager answers the same questions in the same way. This entry evaluates a common, machine-readable interface for OSAT managers, called here the OSAT manager protocol. It records how the managers stand on 2026-10-27, what uc-automa has already decided, and what a cascade still lacks, then compares four ways to provide it. Eleven earlier managers, back to June 2026, were read to test the proposed schema against every kind of tool OSAT has managed so far, and the schema was changed where they showed it was too narrow. It also sets out the experience the protocol is for: one command, `osat`, with one grammar for every tool, as a thin front end over the managers rather than a replacement for them, so that `osat install python 3.12` and `manage-python --install 3.12` both work. Christopher Steel decided the open questions on 2026-10-28, and this version records those decisions. We recommend protocol 1, defined and implemented once in osat-manager-core and shipped in its first release, with a conformance suite. Its schema freezes at this entry's final draft, after a trial between manage-python and manage-restic, and every manager except manage-python is built to it from its first release. The dispatcher's own design is out of scope here and will need its own entry once the protocol exists.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-steel-2026l"></a>[(Steel, 2026l)](#ref-steel-2026l) and follows the *UC Radar Evaluation Lifecycle* version 0.2.0 <a id="cite-steel-2026j"></a>[(Steel, 2026j)](#ref-steel-2026j). The existing contract comes from *Creating an OSAT Manager Repository* version 0.6.0 <a id="cite-steel-2026a"></a>[(Steel, 2026a)](#ref-steel-2026a) and uc-automa decisions 0033 to 0036 <a id="cite-steel-2026d"></a>[(Steel, 2026d)](#ref-steel-2026d), <a id="cite-steel-2026f"></a>[(Steel, 2026f)](#ref-steel-2026f), <a id="cite-steel-2026e"></a>[(Steel, 2026e)](#ref-steel-2026e), <a id="cite-steel-2026b"></a>[(Steel, 2026b)](#ref-steel-2026b). The snapshot was read on 2026-10-27 from the `main` branches of osat-manager-python, osat-manager-restic, osat-manager-git and osat-fluent-sat-tool, including their usage guides, `manager.yaml` files and argument parsers. On 2026-10-28, eleven earlier repositories were read the same way: osat-tool, osat-manager-slugify, osat-fluent-hugo, osat-fluent-rclone-tool, osat-fluent-myrepos-tool, osat-fluent-wekan-tool and osat-fluent-cboard-manager, and the June 2026 hugo-tool, marp-tool, pagefind-tool, rclone-tool and restic-tool; osat-manager-slugify-test is empty. The history of the collection comes from the *OSAT User-Space Installation Specification* version 0.3.0 <a id="cite-steel-2026q"></a>[(Steel, 2026q)](#ref-steel-2026q), *Software Installation Archetypes* <a id="cite-steel-2026o"></a>[(Steel, 2026o)](#ref-steel-2026o) and osat-fluent ADR-001 on install versus manage naming <a id="cite-steel-2026p"></a>[(Steel, 2026p)](#ref-steel-2026p). osat-fluent-gh-tool, named in an earlier draft, was proposed in a radar entry but never built; its release-publishing role went to a shared `publish-release.py` <a id="cite-steel-2026r"></a>[(Steel, 2026r)](#ref-steel-2026r). git's lookup of external commands on `PATH` is the prior art for one command in front of many programs <a id="cite-git-nd"></a>[(Git Project, n.d.)](#ref-git-nd), and pip's installation report is the prior art for versioning a JSON format <a id="cite-pip-nd"></a>[(pip developers, n.d.)](#ref-pip-nd). The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic), from Christopher Steel's goal of a one-command install cascade.

## Scope

The subject is the interface between an OSAT manager and anything that drives it without a person reading its output: another manager, a dispatcher, a test harness. It covers how a caller learns what a manager supports, reads its state, asks whether a requirement is met, learns what a version of a tool requires, records that it depends on a version, moves verified downloads to a machine without a network, and starts an installed version.

In scope as well is the grammar a dispatcher presents to people, as far as it constrains the managers: the verbs they share. Out of scope: the dispatcher's own design and resolution policy; the human-facing text of each manager, beyond keeping it separate from machine output; the source kinds inside osat-manager-core; and osat-manager-pyapp, the proposed manager for Python applications, which this protocol would serve but which needs its own entry. Where tools come from, such as conda-forge's git, is settled per tool in entries such as *Git on Windows, Options for osat-manager-git* <a id="cite-steel-2026g"></a>[(Steel, 2026g)](#ref-steel-2026g).

## The question

What must every OSAT manager expose, and in what form, so that a dispatcher can install a tool and everything it requires, on every platform, without knowing anything about any particular manager?

## The old implementation

### Where the managers stand on 2026-10-27

The table is drawn from the manage-python proposal <a id="cite-steel-2026i"></a>[(Steel, 2026i)](#ref-steel-2026i), the manage-restic and manage-git usage guides <a id="cite-steel-2026n"></a>[(Steel, 2026n)](#ref-steel-2026n), <a id="cite-steel-2026m-2"></a>[(Steel, 2026m)](#ref-steel-2026m), and the source of install-sat.py <a id="cite-steel-2026h"></a>[(Steel, 2026h)](#ref-steel-2026h), each read on 2026-10-27.

| | manage-python | manage-restic | manage-git | install-sat.py |
| --- | --- | --- | --- | --- |
| Repository, version | osat-manager-python 1.0.2 | osat-manager-restic 0.1.0 | osat-manager-git 0.1.0 | osat-fluent-sat-tool 0.4.0 |
| Lifecycle verbs | `--install`, `--switch`, `--status`, `--remove`, `--alias`, `--version` | the same, plus `--self-install` and `--uninstall [--yes]` | `--check [--json]` and `--version` work; the rest are a skeleton | `--install`, `--switch`, `--status`, `--remove`, `--version` |
| Self-install | during `--install` | `--self-install` | planned as `--self-install` | none |
| Machine-readable output | none | none | `--check --json` | none |
| Exit statuses | 0, 1, and 2 for a usage error | 0, 1, and 2 from argparse for a usage error | check: 0 ready, 2 not ready, 1 could not run; argparse usage errors also 2 | 0, 1, and 2 from argparse |
| Install root, Linux | `~/.local/share/python-manager/` | `~/.local/share/osat/restic` | `~/.local/share/osat/git` | `~/.local/share/sat-tool/` |
| Launchers, Windows | `%LOCALAPPDATA%\Programs` | `%LOCALAPPDATA%\osat\bin` | `%LOCALAPPDATA%\osat\bin` | none; POSIX wrappers in `~/.local/bin` only |
| Status vocabulary | default, installed, archived | active, installed, archived | active, installed, archived | installed, active |
| Messages | gettext, English built in | fixed English strings | JSON catalogues, en-CA and fr-CA | fixed English strings |
| Runs on | the user's default Python | any Python 3.8 or later | Python 3.14 or later | the Python that runs it |
| How others reach it | a versioned alias, by convention | aliases | `invocation` in `--check --json`: executable and environment | aliases |

All four verify every download with SHA-256 before unpacking and keep versions side by side, and the three newer managers refuse to run as root. Three of them, manage-python, manage-restic and manage-git, keep or plan a verified archive for offline restore; install-sat.py verifies its release tarball but installs sat's Python dependencies from the network with `pip install -r requirements.txt`, unhashed, into a venv created with whichever interpreter runs the installer.

### Earlier managers

Before the uc-automa contract, the collection grew under the *OSAT User-Space Installation Specification*. Its scope is archetype 5 of *Software Installation Archetypes*, a self-contained tool installed into the user's home with no elevation and no package manager, with a variant of archetype 6 for Python tools that carry their own venv <a id="cite-steel-2026q-2"></a>[(Steel, 2026q)](#ref-steel-2026q). osat-fluent ADR-001 then recognised two shapes: install-only, named `install-<tool>.py`, and version-managed, named `manage-<tool>.py`, with `--install`, `--switch`, `--status`, `--remove` and `--version` <a id="cite-steel-2026p-2"></a>[(Steel, 2026p)](#ref-steel-2026p). The uc-automa contract later made every manager version-managed. Read on 2026-10-28:

| Repository | Version | Shape | What it manages | Verification | What it shows |
| --- | --- | --- | --- | --- | --- |
| hugo-tool, rclone-tool, restic-tool, marp-tool, pagefind-tool | unversioned to 0.0.1, June 2026 | Install-only | Single binaries under `~/bin/<tool>-tool/<version>/` | SHA-256 against a checksums file, or GitHub's asset digests for marp; pagefind-tool commits the binary to the repository | The pattern before the specification moved it to `~/.local` |
| osat-tool | 0.1.0 | Install-only | A test script that prints the platform, installed as a command named `osat` in `~/bin` | None needed | It already holds the name the dispatcher needs |
| osat-fluent-hugo | 0.2.0 | Install-only | Hugo, a single Go binary | SHA-256 against the checksums file | The baseline case |
| osat-fluent-rclone-tool | 0.2.0 | Install-only | rclone, a single Go binary, on Linux, macOS, Windows, FreeBSD, NetBSD, OpenBSD and Solaris | SHA-256, with a local archive for offline reinstall | Platforms beyond what the manager runtime can reach |
| osat-manager-slugify | 1.1.0 | Version-managed, as `install-slug.py` | A Python CLI in a per-version venv, built with whichever interpreter runs the manager; seeds a user configuration file once and never overwrites it | Direct dependencies pinned, not hashed | Archetype 6, and configuration the manager seeds but does not own |
| osat-fluent-myrepos-tool | 0.3.0 | Version-managed | myrepos, Perl scripts fetched by shallow `git clone` at a ref, exposing two commands, `mr` and `webcheckout` | The commit recorded in provenance; no checksum | A versioning scheme read from `debian/changelog`, such as `1.20180726`; git needed to install; Perl and Perl modules needed to run, from the system |
| osat-fluent-wekan-tool | 0.2.0 | Install-only | Wekan, a self-contained server bundle of Node.js, FerretDB and SQLite | SHA-256 | Mutable data kept outside the versioned directory; platforms on different versions in its default mode, with a `team` mode that pins one version everywhere |
| osat-fluent-cboard-manager | 0.1.0 | Version-managed | Cboard, a static web application built and released by uc-cboard, served in the foreground by a launcher running the system `python3 -m http.server` | SHA-256 | Two version identities, the uc-cboard release and the upstream Cboard it contains; a launcher that runs a service on a port |

### What is already decided

uc-automa has settled much of what a protocol needs, on 2026-10-09 and 2026-10-21:

- **The contract.** Every manager meets the same capability table: user-space install, versions side by side, switching, aliases, protection, nothing left behind, verified archive, self-install, independent runtime and three platforms <a id="cite-steel-2026a-2"></a>[(Steel, 2026a)](#ref-steel-2026a).
- **One shared package.** osat-manager-core, standard-library-only, installed once into the manager runtime, holds the `manager.yaml` schema, source kinds, launcher rendering and gettext catalogues; each manager states the range of core versions it accepts <a id="cite-steel-2026e-2"></a>[(Steel, 2026e)](#ref-steel-2026e).
- **One manager runtime.** Every manager runs on manage-python's manager runtime, never on a version it exposes, so a manager's minimum Python is a requirement on that runtime rather than on the user <a id="cite-steel-2026d-2"></a>[(Steel, 2026d)](#ref-steel-2026d).
- **Check exit statuses.** A check exits 0 when ready, 2 when not, 1 when it could not run; every other command exits 0 or 1 <a id="cite-steel-2026b-2"></a>[(Steel, 2026b)](#ref-steel-2026b).
- **Not mise.** Managers stay per-tool and standard-library; mise is prior art and an approved release-time tool, not a runtime dependency <a id="cite-steel-2026f-2"></a>[(Steel, 2026f)](#ref-steel-2026f).
- **A readiness contract.** manage-git's `--check --json` reports `ready`, each installation's problems by code, and an `invocation`, the executable and the environment to add, which other managers use to start a managed git directly <a id="cite-steel-2026m"></a>[(Steel, 2026m)](#ref-steel-2026m), as decided in osat-manager-git decision 0001 <a id="cite-steel-2026c"></a>[(Steel, 2026c)](#ref-steel-2026c).

### Known limitations

Read against the goal, the snapshot shows six gaps and one conflict.

- **No way to ask what a manager speaks.** A caller cannot tell a manager that supports a command from one that does not, except by running it and reading the failure.
- **The conflict: exit status 2 is ambiguous.** Under uc-automa 0036, 2 from a check means "not ready". Every manager also exits 2 on a usage error, because argparse does by default. A dispatcher that runs `--check` against a manager without one reads "not ready" and tries to install, when the true answer is "this manager cannot be asked".
- **State is text for people.** Only manage-git emits JSON, and only for its check. `--status` is prose in three vocabularies.
- **Checks are tool-specific.** manage-git checks against one baseline in `manager.yaml`. A dispatcher needs to ask "is there a Python satisfying `>=3.11,<3.14`", a constraint that comes from the caller.
- **Requirements are nowhere.** No manager declares what a version of its tool needs. install-sat.py knows sat needs Python only by running on it.
- **Removal does not know who depends on a version.** manage-python warns that venvs break when a runtime is removed, but cannot say whose.
- **No offline transport.** Each manager can restore from its own archive on the same machine; none can hand a verified set of downloads to another machine.
- **Tools are more varied than the schema assumed.** The earlier managers include tools with several commands, versions that are not semver, two version identities, data the manager must never remove, services that run on a port, and requirements on system interpreters that no OSAT manager provides. A schema drawn only from manage-python, manage-restic and manage-git would describe none of these.

## The new implementation

### One command, many managers

The experience we want is that of a single command with a single grammar, such as Incus gives for every kind of resource it manages, without giving up the per-tool managers that uc-automa 0034 decided to keep. The two are compatible. The umbrella command, `osat`, is a thin front end over the managers, in the way git, given `git foo`, looks for an executable named `git-foo` on `PATH` and runs it <a id="cite-git-nd-2"></a>[(Git Project, n.d.)](#ref-git-nd):

```text
osat install python 3.12     ->  manage-python --install 3.12
osat switch restic 0.18.1    ->  manage-restic --switch 0.18.1
osat status git              ->  manage-git --status
osat status                  ->  every installed manager's status, in one view
osat install sat 1.23        ->  the cascade, once --requires and uses exist
```

The two ways of working coexist:

- `osat` is what people use day to day. There is one grammar to learn: verb, then tool, then version. We chose verb first over the resource-first form Incus uses for its administrative commands, such as `incus remote list`, because in OSAT the verbs are the same for every tool, so the verb is the stable part of the sentence. Verb first also lets `osat status`, with no tool named, show every manager at once, which no single manager can.
- `manage-<tool>` stays for scripts, testing and anyone who wants only one tool. It is also what keeps each manager independently releasable and removable, as the contract requires.

This puts the dispatcher in two stages. The first is a pure pass-through: it finds `manage-<tool>`, translates the verb, and adds nothing but `osat status` across managers. It needs only uniform verbs, below, and `--describe`, and can be built early. The second is the cascade, which needs the rest of protocol 1. On Windows the pass-through finds a manager by its launcher on `PATH`, but should run the launcher only for `--describe --json`, whose arguments are fixed, and then start the manager through the `invocation` that `--describe` reports, for the same reason other managers start a managed git that way: a `.cmd` launcher passes arguments through `cmd.exe`'s parsing.

### What the earlier managers change

Checking the schema against the earlier managers, before any is rebuilt to the contract, changes protocol 1 in six places. Each is reflected in the sections that follow.

| Finding | From | Change to protocol 1 |
| --- | --- | --- |
| One version exposes several commands | myrepos (`mr`, `webcheckout`), sat (`sat`, `collection`, `content`), git with git-lfs | Each installed version lists its `commands`, each with its own `invocation` |
| Requirements arise at install time as well as run time, and some are met only by the system | myrepos needs git to install and Perl to run; cboard's launcher needs `python3` | A requirement's `for` is `install` or `run`, and a requirement may name `"provider": "system"`, which the dispatcher checks on `PATH` and reports but never installs |
| Versions are not always semver | myrepos `1.20180726`, Wekan `v10.44` | Each manager states its version ordering in `manager.yaml`, and constraints are compared with that ordering |
| A release can carry two version identities | cboard: uc-cboard release and upstream Cboard | Status keeps `version`, the identifier the verbs take, and an optional `upstream_version` |
| Managers place or seed data they must never remove | Wekan's database and attachments, slugify's configuration, sat's `~/.config/sat/` | Status lists `data` paths with `owned` true or false; `--uninstall` never removes a path it does not own and names it; bundles exclude data |
| The same tool can be on different versions on different platforms | Wekan's default mode | `--check` and `--requires` answer for the platform they run on; the dispatcher never assumes one version across platforms |

Two findings fall outside the schema. First, the name `osat` is already a command: osat-tool installs a test script under it in `~/bin`. osat-tool is retired once the dispatcher replaces it, under the retirement rule decided on 2026-10-28, and the dispatcher's launcher, like every launcher, refuses to overwrite a file it did not write. Second, rclone publishes builds for the BSDs and Solaris, which an OSAT manager cannot serve: under uc-automa 0033 every manager runs on manage-python's manager runtime, and python-build-standalone publishes no builds for those systems. Protocol 1 inherits that limit rather than working around it.

Install-only tools remain expressible. A manager for one lists only the verbs it supports in `--describe`, and the dispatcher reports the rest as unavailable rather than failing.

### Options considered

**A. Conventions only.** Write the protocol into *Creating an OSAT Manager Repository* as prose, and let each manager implement it. This costs nothing up front. The snapshot shows its weakness: the capability table already exists as prose, and four managers still disagree on install root, status words, usage exit status and message format. Prose cannot be run, so drift is found by whoever trips on it.

**B. A versioned protocol in osat-manager-core, with a conformance suite.** Define protocol 1 as a schema and a small set of commands, implement the argument handling, JSON emitters, uses discovery and bundles once in osat-manager-core, and ship a conformance suite that runs any manager in sandbox mode and checks its answers. Managers provide only what is tool-specific. This builds directly on uc-automa 0035, which already places the schema, launchers and catalogues there.

**C. A Python API instead of a command line.** Since every manager runs on the same manager runtime and imports osat-manager-core, a dispatcher could import each manager's package and call it. This is fast and typed. We set it aside: it couples the dispatcher to each manager's internals and its exact core version in one process, so one manager's breaking change breaks the dispatcher; it rules out a manager written in anything but Python; and it bypasses the command line people use, so the tested path and the used path diverge. osat-manager-core may still use a Python API internally.

**D. mise as the engine.** mise answers most of these questions today through `mise ls --json`, `mise where` and `mise which` <a id="cite-steel-2026k"></a>[(Steel, 2026k)](#ref-steel-2026k). uc-automa 0034 has already declined it as a runtime dependency, with stated revisit conditions; we record it here only so the option is visibly considered, and borrow its field names where they fit.

We recommend B. It is the only option that makes drift fail a test rather than a user, and it is mostly work uc-automa 0035 already plans.

### Protocol 1

Protocol 1 adds a machine-readable surface beside the existing human one. Each item records what we chose, why, and what we set aside.

#### Describe

`manage-<tool> --describe --json` prints what the manager is and what it speaks, and changes nothing:

```json
{
  "protocol": 1,
  "manager": {"command": "manage-git", "version": "0.2.0"},
  "core": {"version": "0.1.0"},
  "invocation": {"executable": "/home/a/.local/share/python-manager/manager/python/bin/python3", "args": ["-m", "osat_manager_git"], "env": {}},
  "tool": {"name": "git", "license": "GPL-2.0-only"},
  "protocols": [1],
  "platforms": {
    "linux": {"status": "verified"},
    "macos": {"status": "not yet verified"},
    "windows": {"status": "not yet verified"}
  }
}
```

The `invocation` is how the manager itself is started: the manager runtime of uc-automa 0033, here at an illustrative path under manage-python's install root, with the arguments that run the manager. It adds `args` to the `invocation` form manage-git defines, which needs none because git is started with the caller's own arguments; for a managed tool, `args` is empty. A dispatcher always calls `--describe` first. A manager that exits non-zero, or prints no JSON, is pre-protocol, and the dispatcher stops with a message naming it, instead of guessing from exit statuses. This is what resolves the exit status 2 conflict for every manager written before protocol 1. We considered probing each command and interpreting failures; that is the ambiguity this command removes.

#### Usage errors show help

A manager that cannot understand its arguments shows its help, as Christopher Steel decided on 2026-10-28. It also exits with a status, because a dispatcher cannot read help:

- A malformed call prints the help to standard error and exits 1, like any other refused command.
- `--help` prints the help to standard output and exits 0.
- No arguments at all is taken as a request for help, and also exits 0.

Status 2 is then left to mean only "not ready" from a check, as uc-automa 0036 intends. osat-manager-core supplies an argument parser that overrides argparse's default of 2. We considered a distinct status such as 64, `EX_USAGE` from BSD's `sysexits.h`; it would let a caller tell a malformed call from a refused one, but a dispatcher that builds its own calls has no use for the difference. This changes manage-python, whose 1.0 proposal documents 2 for usage errors.

#### Uniform verbs

Every manager uses the same verb for the same action: `install`, `switch`, `status`, `remove`, `alias`, `check`, `self-install`, `uninstall`, `describe`, `requires` and `bundle`, each as a `--` option of the same name. A manager does not add a synonym, and a verb a manager does not support is absent from its `--describe` output rather than present and failing. This is what lets the dispatcher translate `osat <verb> <tool>` mechanically, and it is nearly true already: the snapshot's managers differ only in which verbs they have, not in what they call them.

#### One object, two renderings

Every command builds one result object and then renders it, as JSON when `--json` is given and otherwise as translated text for people. The text is generated from the object, never assembled separately, so `--status` and `--status --json` cannot disagree, and the conformance suite can check the text rendering against the same object it checks the JSON against. osat-manager-core supplies the renderers; a manager supplies the object.

#### Freezing the schema

Adding a field later is free under the versioning rule below, but renaming or retyping one is a breaking change, so the schema needs evidence before it is fixed. Christopher Steel decided on 2026-10-28 that osat-manager-core ships protocol 1 in its first release and that every manager other than manage-python is built to it from its own first release, since the repositories being retired need no copy of their code in the core. The schema is therefore frozen once, at this entry's final draft, rather than field by field as managers adopt it, as 0.1.1 proposed.

The evidence for that freeze is twofold:

- **The paper check, complete.** The planned managers are the existing ones, and each has been checked against the schema under *What the earlier managers change*: does `--status`, `--requires`, `invocation` and `--bundle` describe it without a special case?
- **The trial, before the final draft.** Two working managers with different source kinds implement the draft schema, reporting `"protocol": "draft"` in `--describe`. What the trial changes is folded into the final draft, and only then does `"protocol": 1` exist.

#### Status as JSON

`--status --json` gives the same facts as `--status`, untranslated, in one object: every installed version with its provenance, its commands and, where it differs, its `upstream_version`; which version each alias runs; every archived version; the versions of other tools it `uses` (below); and the `data` paths the manager knows of, each marked `owned` or not. Field names are settled once in osat-manager-core. This is also the moment to settle the vocabulary: "default", as manage-python uses, and as its proposal argues, rather than "active", which collides with venv activation.

#### Check against a constraint

`--check [CONSTRAINT] --json` generalises manage-git's check. With no constraint it checks the manager's own baseline, as today. With a constraint, such as `">=3.11,<3.14"`, it reports whether an installed version satisfies it, and which one it would use: the newest installed version that satisfies the constraint and meets the baseline. Exit statuses follow uc-automa 0036, and the JSON carries `ready` and, for the chosen version, its `invocation`. Constraints use the comparison operators of the version-specifier syntax Python packaging defines, `>=`, `<`, `==` and their kin, joined by commas. The standard library has no parser for them, so osat-manager-core implements that subset itself. The operators are compared with each manager's own version ordering, stated in `manager.yaml` as uc-automa 0034 borrows from mise, so `>=1.20180726` works for myrepos as `>=3.11` does for Python. A check answers for the platform it runs on.

#### Requirements

`--requires VERSION --json` reports what that version of the tool needs from other managers:

```json
{
  "protocol": 1,
  "tool": "sat",
  "version": "1.23.0",
  "requires": [
    {"tool": "python", "constraint": ">=3.11,<3.14", "for": "run"},
    {"tool": "git", "constraint": ">=2.49.0", "for": "run"},
    {"tool": "perl", "constraint": ">=5.30", "for": "run", "provider": "system"}
  ]
}
```

Requirements are data, kept in `requires.yaml` beside each version's lock, at `locks/<version>/requires.yaml`, rather than in `manager.yaml`, because they change from one version to the next and must be reviewed and signed with the lock that pins that version. The file has an optional section per platform, for a requirement such as Perl that applies only on Linux and macOS. We chose `requires.yaml` over `requirements.yaml`, which a Python application's directory would hold beside pip's `requirements.txt`, and named it after the verb that reports it. A requirement names a tool, not a manager; the dispatcher maps tools to managers through `--describe`. `for` is `install` when the tool is needed only to install, as myrepos needs git for its clone, and `run` otherwise. `"provider": "system"` marks a requirement no OSAT manager provides, such as the Perl that myrepos runs on; the dispatcher checks the first matching command on `PATH`, reports whether it satisfies the constraint, and never installs it. The perl line above is illustrative, not taken from sat. An earlier draft of this entry ruled system providers out, following the move away from system tools in the git-on-Windows entry and osat-manager-git decision 0001; the earlier managers showed requirements that only the system meets today, so the protocol states them honestly rather than hiding them, and each one is a candidate for a manager of its own.

#### Uses

A consumer keeps its own record of what it depends on, as Christopher Steel decided on 2026-10-28, and reports it through the protocol rather than writing it anywhere else. Each installed version in a consumer's `--status --json` lists what it `uses`:

```json
"uses": [{"tool": "python", "version": "3.12.14", "path": "/home/a/.local/share/osat/pyapp/sat/1.23.0/venv"}]
```

The path is illustrative; osat-manager-pyapp's layout belongs to its own entry.

Before `--remove` deletes a version, the providing manager finds every installed OSAT manager by its `manage-*` launcher carrying the ownership marker, asks each for `--status --json`, and refuses if any of them uses that version, naming the consumer and the path. A manager that does not answer also blocks the removal, since its silence cannot be read as consent. `--force` removes anyway, after naming what will break. `--uninstall` reports the same before asking.

We considered three places to keep these records:

- **The providing manager**, the proposal of 0.1.0, through `--hold` and `--release`. The manager that has to refuse holds the records itself, but every consumer writes into another manager's state, which the provider then has to validate, and a consumer removed without releasing leaves a hold that blocks removal for good.
- **The consumer, found by its file layout.** No manager writes into another's state, but the provider has to know where every consumer keeps its records, and a consumer it does not know about is invisible to it.
- **A central registry**, kept by the dispatcher. One place to look, but it makes the dispatcher mandatory, where every manager is meant to work alone.

Consumer-kept records reported through the protocol take the first option's guarantee and the second's independence. No manager writes into another's state, a record disappears with its consumer, so nothing goes stale, and the provider reads consumers only through the interface every manager already has. The cost is that `--remove` runs every installed manager once, and finding them is the same discovery the pass-through dispatcher needs. A venv a person makes by hand reports nothing, so manage-python keeps its existing warning that venvs built on a version stop working when it is removed.

#### Invocation

Every installed version in `--status --json` and `--check --json` lists its `commands`, and each command carries an `invocation`, as manage-git defines it: the executable, and the environment to add to the caller's own. A tool with one command, such as restic, lists one; myrepos lists `mr` and `webcheckout`. Callers start that executable directly with an argument list, never through a shell or a launcher. Launchers remain for people and shells. This replaces the convention in the manage-python proposal that another manager reaches a runtime through a versioned alias: aliases can be renamed, and on Windows a `.cmd` launcher puts arguments through `cmd.exe`'s parsing.

#### Bundles

`--bundle VERSION --to DIR` copies a version's verified archive entry, its lock, its requirements and their checksums into a directory; `--install VERSION --from DIR` installs from such a directory with no network, verifying every file again. A dispatcher can then gather a tool and everything it requires into one directory for a machine with intermittent or no connectivity, which is a large part of who Vishpala serves. Each manager already keeps the verified archive; the bundle is a portable view of it.

#### Unattended use

Every command that would ask a question accepts `--yes`, and no command waits for input when its standard input is not a terminal: it refuses with a message instead. manage-restic's `--uninstall` already takes `--yes`.

#### Running services

Some tools run as a service, in the foreground on a port, such as Wekan or the cboard launcher. A launcher that starts a service writes a PID file in the manager's state directory, and removes it on exit. Before `--remove` or `--uninstall` touches a version, the manager checks for a PID file whose process is still alive, says that the service is running and refuses, as Christopher Steel decided on 2026-10-28; `--force` removes the version anyway. On Windows, files a running process holds cannot be deleted, so a forced removal there reports each path it could not remove rather than claiming success.

#### Versioning the protocol

`protocol` is an integer. Following the rule pip uses for its installation report, it changes only for a backward-incompatible change, such as removing a field or changing a field's meaning or type; adding a field does not change it <a id="cite-pip-nd-2"></a>[(pip developers, n.d.)](#ref-pip-nd). Readers ignore fields they do not know, as `PROVENANCE` readers already do. A manager lists every protocol it speaks in `--describe`, so a later protocol 2 can coexist with 1 while managers migrate.

### Boundary contract

The boundary between a manager and its caller is the whole of the following, and nothing else.

- **Standard output** carries only the JSON object when `--json` is given, one object per run, UTF-8, keys untranslated.
- **Standard error** carries progress, warnings and errors, prefixed with the command name and translated, as manage-python's proposal already defines for people.
- **Problems** in JSON carry a stable `code` and their fields; their `message` is in the caller's language and is never parsed.
- **Exit status** is 0 or 1 for every command, and 0, 2 or 1 for a check.
- **Logging** stays per manager: each lifecycle action appends one line to the manager's log, as manage-python does, so an operator can see what a dispatcher did without the dispatcher keeping its own record of every step.
- **Environment** a caller adds is only what an `invocation` names. A manager never reads the caller's project configuration.

### Delivery and testing

Protocol 1 lives in osat-manager-core from its first release: the argument parser, the JSON emitters, discovery of installed managers and their uses, the PID check for running services, bundles and the constraint parser. The conformance suite runs a manager in sandbox mode, the mode manage-python already provides, against a local fixture of verified downloads, and checks every command's output against the schema, every exit status, and that `--remove` refuses a version a fixture consumer uses. A manager is protocol 1 when it passes the suite. The suite runs offline on every commit; each platform's end-to-end scenario adds one step, a bundle made online and installed offline.

## Gates

| Gate | Result | Evidence | Notes |
| --- | --- | --- | --- |
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The protocol adds no network calls; `--describe`, `--status`, `--check`, `--requires`, uses discovery and `--bundle` are local. Nothing is built to trace |
| A licence incompatible with the project | Pass | Inferred | Implemented in osat-manager-core under MIT, documented under CC BY 4.0 (uc-automa 0025); no new dependency |
| Failure of the accessibility floor | Pass | Inferred | Adds a machine-readable channel beside the text interface without changing it; messages stay translated through gettext. Not yet tested with a screen reader |
| No exit | Pass | Inferred | Plain JSON and command-line arguments; any caller in any language can use them, and dropping the protocol leaves every manager usable by hand |

## Evaluation

Ratings use Strong, Moderate, Weak or Unknown, tagged Verified, Inferred or Claimed, as the metrics define. The subject is an internal interface, so several product-oriented items are Not applicable, with what is absent named.

### Inclusive

**Accessibility**, including the text interface

- Rating: Moderate
- Evidence: Inferred
- Notes: Human output is translated and rendered from the same object as the JSON, so a screen reader user never hears JSON unless they ask for it. The larger accessibility gains are one grammar for every tool, learned once, and the cascade: one command in place of a sequence of installs in the right order.

**Representation**

- Rating: Not applicable
- Evidence: Verified
- Notes: An interface between programs has no designer-to-designed-for relationship of its own.

**Resilience**

- Rating: Strong
- Evidence: Inferred
- Notes: Bundles make offline and low-bandwidth installs a supported path rather than a workaround; uses prevent a removal from silently breaking a consumer.

### Agency

**Sovereignty & Privacy**

- Rating: Strong
- Evidence: Inferred
- Notes: No service, no account, nothing leaves the machine. Exit cost is nil: every manager remains usable by hand.

**Interaction Patterns**

- Rating: Strong
- Evidence: Inferred
- Notes: Nothing installs without a command; unattended use refuses rather than assuming consent.

### Sustainability

**Environment**

- Rating: Moderate
- Evidence: Inferred
- Notes: Bundles avoid repeated downloads across machines. Each consumer can still pull in its own runtime when constraints differ, which costs disk; uses make unused versions visible so they can be removed.

### Security

- Rating: Moderate
- Evidence: Inferred
- Notes: The protocol widens no trust boundary: verification stays in the source kinds, and a bundle is re-verified on install. No manager writes into another's state. The new inputs are other managers' status output, read when removing, and bundle directories from elsewhere. Both are treated as untrusted: status is parsed as data and its paths are only reported, never acted on, and bundle contents are checked against the reviewed lock, never against checksums shipped beside them.

**Assessment method:** design review against the four repositories on 2026-10-27. No code exists, so nothing was traced.

### The Product or Service Itself

**Longevity**

- Rating: Strong
- Evidence: Inferred
- Notes: An integer protocol version with additive change follows a pattern that has held for pip's report since version 23.0.

**Content endurance**

- Rating: Not applicable
- Evidence: Verified
- Notes: The protocol holds no content.

**Exit and portability**

- Rating: Strong
- Evidence: Inferred
- Notes: Command line and JSON, readable from any language.

**Adjustability and support**

- Rating: Moderate
- Evidence: Inferred
- Notes: Additive changes are cheap; a breaking change needs a coordinated release of every manager, which the core version range of uc-automa 0035 makes visible.

**Market Position**

- Rating: Not applicable
- Evidence: Verified
- Notes: An internal interface, not a product in a market.

## Scorecard

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | Moderate | Inferred | Text interface unchanged; JSON only on request |
| Multilingual integration | Strong | Inferred | Messages translated; keys and codes never are |
| Economic and cognitive accessibility | Strong | Inferred | One grammar for every tool; one command replaces an ordered sequence of installs |
| Representation | Not applicable | Verified | No designer-to-designed-for relationship |
| Compatibility | Moderate | Verified | Four managers disagree today on layout, vocabulary and exit status; protocol 1 fixes the interface, not the layouts |
| Resilience | Strong | Inferred | Bundles, and uses checked before removal |
| Agency, sovereignty & privacy | Strong | Inferred | Local only, no service |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No exit cost |
| Agency, interaction patterns | Strong | Inferred | No consent assumed in unattended use |
| Environment, direct and indirect | Moderate | Inferred | Fewer downloads; more runtimes when constraints differ |
| Security | Moderate | Inferred | No new trust; no cross-manager writes; status and bundles are untrusted input |
| Longevity | Strong | Inferred | Additive versioning |
| Content endurance | Not applicable | Verified | No content held |
| Exit and portability | Strong | Inferred | CLI and JSON |
| Adjustability and support | Moderate | Inferred | Breaking change needs coordinated releases |
| Market position | Not applicable | Verified | Internal interface |
| Gates | Pass | Inferred | No gate failed; none yet verified |

## Recommendation

We recommend Trial for protocol 1, delivered in osat-manager-core's first release.

The paper check is complete. The trial scope is two managers and one edge: manage-python and manage-restic implement `--describe`, `--status --json`, `--check CONSTRAINT --json`, help on usage errors and the uses check, and the conformance suite plays the dispatcher's part with a fixture consumer, asking manage-python for a runtime satisfying a constraint and confirming that `--remove` refuses a version the fixture uses. The pair is chosen because both managers work today, manage-restic verified end to end on Linux, and because they use different source kinds, a python-build-standalone tarball and a GitHub release, so a field both can fill is not specific to one kind. manage-git, whose real dependency on manage-python would make a better edge, is not yet working beyond `--check`. The trial ends when the conformance suite passes for both on Linux and Windows, or after the first breaking change to the draft schema, whichever comes first; a breaking change during trial sends the entry back to assess. What the trial teaches goes into the final draft, which freezes protocol 1.

Before Adopt, the gates and Security pillar need to be Verified by that trial, and the bundle path needs one end-to-end run with the network disconnected.

## Decisions

Christopher Steel decided the open questions of 0.1.1 on 2026-10-28.

| Question | Decision | Where it is applied |
| --- | --- | --- |
| Which managers are planned? | The existing ones. slugify becomes part of SAT, as an application under osat-manager-pyapp or as part of SAT's own | *What the earlier managers change*; the paper check is complete |
| Is osat-tool renamed or retired? | Every repository not named `osat-manager-<tool>` is retired once a replacement built to this protocol exists | *What the earlier managers change*; *Relationship to project* |
| May `--remove` remove a version while its service runs? | It says the service is running and refuses; `--force` removes anyway | *Running services* |
| Usage errors: exit 1 or 64? | Show help | *Usage errors show help* |
| Who keeps holds? | The consumer, reporting its dependencies through the protocol | *Uses* |
| Where do requirements live? | Beside each version's lock, in `requires.yaml` | *Requirements* |
| Which osat-manager-core release carries the protocol? | The first; archived repositories need no copy of their code, so only manage-python migrates and every other manager is built to the final draft | *Freezing the schema*; *Relationship to project* |
| Is the layout drift fixed in the same change? | Yes, in manage-python's migration | *Relationship to project* |

## Open questions

- [ ] The exit statuses that go with help, 1 for a malformed call and 0 for `--help` or no arguments, follow from uc-automa 0036 but were proposed with the decision rather than stated in it. Confirm.
- [ ] Does slugify become its own application under osat-manager-pyapp, or part of SAT's application? This belongs to the osat-manager-pyapp entry.
- [ ] Does a forced removal on Windows stop the running service first, or only report what it could not remove, as proposed?

## Flip conditions

- **To hold:** the shared-package plan of uc-automa 0035 is set aside, since protocol 1 assumes one implementation.
- **To rejected:** a dispatcher turns out not to be wanted, in which case most of protocol 1 is unnecessary and uses checked before removal may be worth a smaller entry.
- **Back to assess:** mise meets the revisit conditions of uc-automa 0034, which reopens option D.

## Relationship to project

The protocol sits between every OSAT manager and the dispatcher that will drive them. On adopt it graduates to uc-automa: a directive stating protocol 1, a new section in *Creating an OSAT Manager Repository*, and the implementation in osat-manager-core. manage-python adopts it in its next minor release, which also moves its install root to `osat/python` and its Windows launchers to `%LOCALAPPDATA%\osat\bin`, with a migration for existing installations. Every other manager is built to protocol 1 from its first release, and the repositories it replaces are retired. The dispatcher, in both its pass-through and cascade stages, gets its own entry; the pass-through can be assessed as soon as uniform verbs and `--describe` exist. The open question in that guide, where the list of OSAT managers is kept, becomes the dispatcher's registry of tools to managers.

## Status notes

- Last reviewed: 2026-10-28
- In assess: Christopher Steel's decisions of 2026-10-28 are recorded under *Decisions*; the second automated pass recommends Trial in the scope above. The human evaluation pass is still to be recorded. Nothing has been built or run; every rating that depends on behaviour is Inferred. The snapshot facts were read from source on 2026-10-27 and are Verified as statements about the code, not about its behaviour on any platform.
- Re-review triggers: osat-manager-core's first release; a second consumer manager, such as osat-manager-pyapp, being proposed; any manager adding a machine-readable command outside the protocol.

## License

This document, *OSAT Manager Protocol, a Common Interface for OSAT Managers*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-steel-2026l)
- [UC Radar Evaluation Lifecycle](#ref-steel-2026j)

### The OSAT manager contract

- [Creating an OSAT Manager Repository](#ref-steel-2026a)
- [Decision: Managers Run on a Runtime They Do Not Expose](#ref-steel-2026d)
- [Decision: osat-manager-core, the Shared Manager Package](#ref-steel-2026e)
- [Decision: OSAT Managers and mise](#ref-steel-2026f)
- [Decision: A Check Exits 2 When Not Ready](#ref-steel-2026b)

### The managers

- [Proposal: osat-manager-python Layout, Lifecycle and Aliases](#ref-steel-2026i)
- [Using manage-restic](#ref-steel-2026n)
- [Using manage-git](#ref-steel-2026m)
- [Decision: Build manage-git](#ref-steel-2026c)
- [osat-fluent-sat-tool](#ref-steel-2026h)

### The collection before uc-automa

- [OSAT User-Space Installation Specification](#ref-steel-2026q)
- [Software Installation Archetypes](#ref-steel-2026o)
- [ADR-001: install- vs manage- naming](#ref-steel-2026p)
- [osat-fluent-gh-tool radar entry](#ref-steel-2026r)

### Prior art

- [git: Integrating a new command](#ref-git-nd)
- [pip installation report](#ref-pip-nd)
- [Universal Cake Evaluation: mise-en-place](#ref-steel-2026k)
- [Git on Windows, Options for osat-manager-git](#ref-steel-2026g)

## References

<a id="ref-git-nd"></a>Git Project. (n.d.). *How to integrate new subcommands*. Git documentation. Retrieved October 27, 2026, from https://code.googlesource.com/git/+/HEAD/Documentation/howto/new-command.adoc
[Return to citation](#cite-git-nd)

<a id="ref-pip-nd"></a>pip developers. (n.d.). *Installation report*. pip documentation. Retrieved October 27, 2026, from https://pip.pypa.io/en/latest/reference/installation-report/
[Return to citation](#cite-pip-nd)

<a id="ref-steel-2026a"></a>Steel, C. (2026a). *Creating an OSAT manager repository* (Version 0.6.0). uc-automa. https://github.com/steelcj/uc-automa/blob/main/en-ca/docs/guides/new-repository/creating-an-osat-manager-repository-v0.6.0.md
[Return to citation](#cite-steel-2026a)

<a id="ref-steel-2026b"></a>Steel, C. (2026b). *Decision: A check exits 2 when not ready* (uc-automa 0036, Version 0.1.0). uc-automa. https://github.com/steelcj/uc-automa/blob/main/en-ca/docs/decisions/0036--a-check-exits-2-when-not-ready-v0.1.0.md
[Return to citation](#cite-steel-2026b)

<a id="ref-steel-2026c"></a>Steel, C. (2026c). *Decision: Build manage-git* (osat-manager-git 0001, Version 0.1.0). osat-manager-git. https://github.com/steelcj/osat-manager-git/blob/main/en-ca/docs/decisions/0001--build-manage-git-v0.1.0.md
[Return to citation](#cite-steel-2026c)

<a id="ref-steel-2026d"></a>Steel, C. (2026d). *Decision: Managers run on a runtime they do not expose* (uc-automa 0033, Version 0.1.0). uc-automa. https://github.com/steelcj/uc-automa/blob/main/en-ca/docs/decisions/0033--managers-run-on-a-runtime-they-do-not-expose-v0.1.0.md
[Return to citation](#cite-steel-2026d)

<a id="ref-steel-2026e"></a>Steel, C. (2026e). *Decision: osat-manager-core, the shared manager package* (uc-automa 0035, Version 0.1.0). uc-automa. https://github.com/steelcj/uc-automa/blob/main/en-ca/docs/decisions/0035--osat-manager-core-v0.1.0.md
[Return to citation](#cite-steel-2026e)

<a id="ref-steel-2026f"></a>Steel, C. (2026f). *Decision: OSAT managers and mise* (uc-automa 0034, Version 0.1.0). uc-automa. https://github.com/steelcj/uc-automa/blob/main/en-ca/docs/decisions/0034--osat-managers-and-mise-v0.1.0.md
[Return to citation](#cite-steel-2026f)

<a id="ref-steel-2026g"></a>Steel, C. (2026g). *Git on Windows, options for osat-manager-git* (Version 0.2.0) [Radar entry]. uc-radar. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/assess/git/git-on-windows--options-for-osat-manager-git-v0-2-0.md
[Return to citation](#cite-steel-2026g)

<a id="ref-steel-2026h"></a>Steel, C. (2026h). *osat-fluent-sat-tool* (Version 0.4.0) [Computer software]. GitHub. https://github.com/steelcj/osat-fluent-sat-tool
[Return to citation](#cite-steel-2026h)

<a id="ref-steel-2026i"></a>Steel, C. (2026i). *Proposal: osat-manager-python layout, lifecycle and aliases* (Version 0.7.0). osat-manager-python. https://github.com/steelcj/osat-manager-python/blob/main/en/docs/proposal--osat-manager-python-layout-and-lifecycle-v0-7-0.md
[Return to citation](#cite-steel-2026i)

<a id="ref-steel-2026j"></a>Steel, C. (2026j). *UC radar evaluation lifecycle* (Version 0.2.0). uc-radar. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/uc-radar--evaluation-lifecycle-v0.2.0.md
[Return to citation](#cite-steel-2026j)

<a id="ref-steel-2026k"></a>Steel, C. (2026k). *Universal Cake evaluation: mise-en-place, polyglot tool version manager* (Version 0.1.1) [Radar entry]. uc-radar. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/assess/installers/universal-cake-evaluation--mise-en-place--polyglot-tool-version-manager-v0-1-1.md
[Return to citation](#cite-steel-2026k)

<a id="ref-steel-2026l"></a>Steel, C. (2026l). *Universal Cake evaluation metrics* (Version 0.6.0). uc-radar. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/universal-cake-evaluation-metrics-v0.6.0.md
[Return to citation](#cite-steel-2026l)

<a id="ref-steel-2026m"></a>Steel, C. (2026m). *Using manage-git* (Version 0.1.0). osat-manager-git. https://github.com/steelcj/osat-manager-git/blob/main/en-ca/docs/guides/usage/using-manage-git-v0.1.0.md
[Return to citation](#cite-steel-2026m)

<a id="ref-steel-2026n"></a>Steel, C. (2026n). *Using manage-restic* (Version 0.1.0). osat-manager-restic. https://github.com/steelcj/osat-manager-restic/blob/main/en-ca/docs/guides/usage/using-manage-restic-v0.1.0.md
[Return to citation](#cite-steel-2026n)

<a id="ref-steel-2026o"></a>Steel, C. (2026o). *Software installation archetypes* (Version 0.1.0). osat-fluent. https://github.com/steelcj/osat-fluent/blob/main/en/docs/software-installation-archetypes-v0-1-0.md
[Return to citation](#cite-steel-2026o)

<a id="ref-steel-2026p"></a>Steel, C. (2026p). *ADR-001: install- vs manage- naming for fluent tool CLIs* (Version 0.1.0). osat-fluent. https://github.com/steelcj/osat-fluent/blob/main/en/docs/osat-fluent--adr-001--installer-naming-install-vs-manage-v0-1-0.md
[Return to citation](#cite-steel-2026p)

<a id="ref-steel-2026q"></a>Steel, C. (2026q). *OSAT user-space installation specification* (Version 0.3.0). osat-fluent. https://github.com/steelcj/osat-fluent/blob/main/en/docs/osat--user-space-installation-specification-v0-3-0.md
[Return to citation](#cite-steel-2026q)

<a id="ref-steel-2026r"></a>Steel, C. (2026r). *osat-fluent-gh-tool, release-publishing CLI* (Version 0.2.0) [Radar entry]. uc-radar. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/assess/devops/osat-fluent-gh-tool--release-publishing-cli-v0-2-0.md
[Return to citation](#cite-steel-2026r)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Christopher Steel's decisions of 2026-10-28 recorded: existing managers are the planned list, slugify part of SAT, retirement rule, `--force` for running services with a PID check, help on usage errors, consumer-kept uses replacing holds and the `--hold` and `--release` verbs, `requires.yaml` beside each lock, protocol 1 in osat-manager-core's first release with the schema frozen at the final draft, manage-python's migration fixing layout drift; trial pair changed to manage-python and manage-restic; second automated pass |
| 0.1.1 | Draft | Renamed by Christopher Steel from 0.1.0 for changes made after it: eleven earlier managers read on 2026-10-28 and six schema changes they required, two findings outside the schema, gh-tool corrected, constraint parser in osat-manager-core, one command `osat` in two stages; version block left at 0.1.0 |
| 0.1.0 | Draft | Initial Assess-ring entry: snapshot of four managers on 2026-10-27, what uc-automa has decided, gaps for a cascade, four options, protocol 1 with describe, uniform verbs, one object with two renderings, a provisional schema, usage exit status, status as JSON, checks against a constraint, requirements, holds, invocation, bundles, unattended use and protocol versioning; automated recommendation Trial between manage-python and manage-git |

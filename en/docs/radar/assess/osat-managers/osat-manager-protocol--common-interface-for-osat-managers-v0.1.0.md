---
dc:title: "OSAT Manager Protocol, a Common Interface for OSAT Managers"
dcterms:version: "0.1.0"
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
dcterms:modified: "2026-10-27"
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
  - version: "0.1.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring entry. Snapshot of manage-python 1.0.2,
      manage-restic 0.1.0, manage-git 0.1.0 and install-sat.py 0.4.0
      read from their repositories on 2026-10-27; osat-fluent-gh-tool
      could not be read. Proposes protocol 1, delivered through
      osat-manager-core, with describe, uniform verbs, one object with
      two renderings, a provisional schema frozen only on evidence,
      machine-readable status, checks against a version constraint,
      declared requirements, holds, bundles and an invocation contract.
      Sets out one command, osat, as a thin front end over the managers,
      in a pass-through stage and a cascade stage. Nothing has been built
      or run.
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
  human: []
---

# OSAT Manager Protocol, a Common Interface for OSAT Managers

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

The goal is a cascade: a person runs `osat install sat 1.23`, and everything sat 1.23 needs, a Python runtime, git, sat itself, is installed into their own account on Linux, macOS or Windows, by the managers that own each tool. That only works if every manager answers the same questions in the same way. This entry evaluates a common, machine-readable interface for OSAT managers, called here the OSAT manager protocol. It records how the managers stand on 2026-10-27, what uc-automa has already decided, and what a cascade still lacks, then compares four ways to provide it. It also sets out the experience the protocol is for: one command, `osat`, with one grammar for every tool, as a thin front end over the managers rather than a replacement for them, so that `osat install python 3.12` and `manage-python --install 3.12` both work. We recommend protocol 1, defined and implemented once in osat-manager-core with a conformance suite, with its schema provisional until managers of different kinds have used it, and tried first between manage-python and manage-git. The dispatcher's own design is out of scope here and will need its own entry once the protocol exists.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.6.0 <a id="cite-steel-2026l"></a>[(Steel, 2026l)](#ref-steel-2026l) and follows the *UC Radar Evaluation Lifecycle* version 0.2.0 <a id="cite-steel-2026j"></a>[(Steel, 2026j)](#ref-steel-2026j). The existing contract comes from *Creating an OSAT Manager Repository* version 0.6.0 <a id="cite-steel-2026a"></a>[(Steel, 2026a)](#ref-steel-2026a) and uc-automa decisions 0033 to 0036 <a id="cite-steel-2026d"></a>[(Steel, 2026d)](#ref-steel-2026d), <a id="cite-steel-2026f"></a>[(Steel, 2026f)](#ref-steel-2026f), <a id="cite-steel-2026e"></a>[(Steel, 2026e)](#ref-steel-2026e), <a id="cite-steel-2026b"></a>[(Steel, 2026b)](#ref-steel-2026b). The snapshot was read on 2026-10-27 from the `main` branches of osat-manager-python, osat-manager-restic, osat-manager-git and osat-fluent-sat-tool, including their usage guides, `manager.yaml` files and argument parsers. osat-fluent-gh-tool could not be read from the session that prepared this entry. git's lookup of external commands on `PATH` is the prior art for one command in front of many programs <a id="cite-git-nd"></a>[(Git Project, n.d.)](#ref-git-nd), and pip's installation report is the prior art for versioning a JSON format <a id="cite-pip-nd"></a>[(pip developers, n.d.)](#ref-pip-nd). The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic), from Christopher Steel's goal of a one-command install cascade.

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

## The new implementation

### One command, many managers

The experience we want is that of a single command with a single grammar, such as Incus gives for every kind of resource it manages, without giving up the per-tool managers that uc-automa 0034 decided to keep. The two are compatible. The umbrella command, `osat`, is a thin front end over the managers, in the way git, given `git foo`, looks for an executable named `git-foo` on `PATH` and runs it <a id="cite-git-nd-2"></a>[(Git Project, n.d.)](#ref-git-nd):

```text
osat install python 3.12     ->  manage-python --install 3.12
osat switch restic 0.18.1    ->  manage-restic --switch 0.18.1
osat status git              ->  manage-git --status
osat status                  ->  every installed manager's status, in one view
osat install sat 1.23        ->  the cascade, once --requires and holds exist
```

The two ways of working coexist:

- `osat` is what people use day to day. There is one grammar to learn: verb, then tool, then version. We chose verb first over the resource-first form Incus uses for its administrative commands, such as `incus remote list`, because in OSAT the verbs are the same for every tool, so the verb is the stable part of the sentence. Verb first also lets `osat status`, with no tool named, show every manager at once, which no single manager can.
- `manage-<tool>` stays for scripts, testing and anyone who wants only one tool. It is also what keeps each manager independently releasable and removable, as the contract requires.

This puts the dispatcher in two stages. The first is a pure pass-through: it finds `manage-<tool>`, translates the verb, and adds nothing but `osat status` across managers. It needs only uniform verbs, below, and `--describe`, and can be built early. The second is the cascade, which needs the rest of protocol 1. On Windows the pass-through finds a manager by its launcher on `PATH`, but should run the launcher only for `--describe --json`, whose arguments are fixed, and then start the manager through the `invocation` that `--describe` reports, for the same reason other managers start a managed git that way: a `.cmd` launcher passes arguments through `cmd.exe`'s parsing.

### Options considered

**A. Conventions only.** Write the protocol into *Creating an OSAT Manager Repository* as prose, and let each manager implement it. This costs nothing up front. The snapshot shows its weakness: the capability table already exists as prose, and four managers still disagree on install root, status words, usage exit status and message format. Prose cannot be run, so drift is found by whoever trips on it.

**B. A versioned protocol in osat-manager-core, with a conformance suite.** Define protocol 1 as a schema and a small set of commands, implement the argument handling, JSON emitters, holds and bundles once in osat-manager-core, and ship a conformance suite that runs any manager in sandbox mode and checks its answers. Managers provide only what is tool-specific. This builds directly on uc-automa 0035, which already places the schema, launchers and catalogues there.

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

#### Usage errors exit 1

Under protocol 1 a usage error exits 1, like any other refused command, and status 2 is reserved for "not ready" from a check, as uc-automa 0036 intends. osat-manager-core supplies an argument parser that overrides argparse's default. We considered a distinct status such as 64, `EX_USAGE` from BSD's `sysexits.h`; it would let a caller tell a malformed call from a refused one, but a dispatcher that builds its own calls has no use for the difference, and one fewer status is one fewer thing to get wrong. This changes manage-python, whose 1.0 proposal documents 2 for usage errors.

#### Uniform verbs

Every manager uses the same verb for the same action: `install`, `switch`, `status`, `remove`, `alias`, `check`, `self-install`, `uninstall`, `describe`, `requires`, `hold`, `release` and `bundle`, each as a `--` option of the same name. A manager does not add a synonym, and a verb a manager does not support is absent from its `--describe` output rather than present and failing. This is what lets the dispatcher translate `osat <verb> <tool>` mechanically, and it is nearly true already: the snapshot's managers differ only in which verbs they have, not in what they call them.

#### One object, two renderings

Every command builds one result object and then renders it, as JSON when `--json` is given and otherwise as translated text for people. The text is generated from the object, never assembled separately, so `--status` and `--status --json` cannot disagree, and the conformance suite can check the text rendering against the same object it checks the JSON against. osat-manager-core supplies the renderers; a manager supplies the object.

#### A provisional schema

The field names and types in this entry are a starting point, not a commitment. Adding a field later is free under the versioning rule below, but renaming or retyping one is a breaking change, so names are frozen only on evidence:

- A field is frozen when two managers with different source kinds use it, such as manage-restic with `github-release` and manage-git with `conda`. A field that fits only one kind of tool is not yet general.
- No manager declares `protocol: 1` in `--describe` until the fields it emits are frozen. Until then it reports `"protocol": "draft"`, and the dispatcher treats it as experimental.
- Before the trial, every manager on the planned list is checked against the schema on paper: does `--status`, `--requires`, `invocation` and `--bundle` describe it without a special case? The likely stress points are tools that expose several commands, such as git with git-lfs or sat with its three wrappers; tools that need more environment than one variable; tools that keep data or run services of their own; and anything that is not a command-line tool.

#### Status as JSON

`--status --json` gives the same facts as `--status`, untranslated, in one object: every installed version with its provenance, which version each alias runs, every archived version, and the holds on each version (below). Field names are settled once in osat-manager-core. This is also the moment to settle the vocabulary: "default", as manage-python uses, and as its proposal argues, rather than "active", which collides with venv activation.

#### Check against a constraint

`--check [CONSTRAINT] --json` generalises manage-git's check. With no constraint it checks the manager's own baseline, as today. With a constraint, such as `">=3.11,<3.14"`, it reports whether an installed version satisfies it, and which one it would use: the newest installed version that satisfies the constraint and meets the baseline. Exit statuses follow uc-automa 0036, and the JSON carries `ready` and, for the chosen version, its `invocation`. Constraints use the version-specifier syntax Python packaging already defines, since every manager is Python and osat-manager-core can parse it without a dependency; a manager whose versions are not semver-like states its ordering in `manager.yaml`, as uc-automa 0034 borrows from mise.

#### Requirements

`--requires VERSION --json` reports what that version of the tool needs from other managers:

```json
{
  "protocol": 1,
  "tool": "sat",
  "version": "1.23.0",
  "requires": [
    {"tool": "python", "constraint": ">=3.11,<3.14", "for": "run"},
    {"tool": "git", "constraint": ">=2.49.0", "for": "run"}
  ]
}
```

Requirements are data, kept beside each version's lock rather than in `manager.yaml`, because they change from one version to the next and must be reviewed with the lock that pins that version. A requirement names a tool, not a manager; the dispatcher maps tools to managers through `--describe`. We considered letting a requirement accept a system-provided tool; the git-on-Windows entry and osat-manager-git decision 0001 moved away from system tools for good reason, so protocol 1 has no such flag, and the check against the first tool on `PATH` that manage-git already reports serves the cases where a system tool is acceptable.

#### Holds

A consumer that depends on an installed version records a hold on it: `--hold VERSION --by HOLDER` and `--release VERSION --by HOLDER`, where the holder is a stable identifier such as `pyapp:sat@1.23.0`. `--remove` refuses a held version and names its holders; `--uninstall` lists every hold before asking. Holds live in the providing manager's state directory, one file per holder, written atomically. The provider owns them because the provider is the one that has to refuse. We considered having the provider scan consumers' provenance files, which couples it to their layouts, and a central registry, which is one more thing to keep consistent. Holds are what lets a dispatcher later offer to remove a dependency nothing else holds, without ever removing something a person installed directly.

#### Invocation

Every installed version in `--status --json` and `--check --json` carries an `invocation`, exactly as manage-git defines it: the executable, and the environment to add to the caller's own. Callers start that executable directly with an argument list, never through a shell or a launcher. Launchers remain for people and shells. This replaces the convention in the manage-python proposal that another manager reaches a runtime through a versioned alias: aliases can be renamed, and on Windows a `.cmd` launcher puts arguments through `cmd.exe`'s parsing.

#### Bundles

`--bundle VERSION --to DIR` copies a version's verified archive entry, its lock, its requirements and their checksums into a directory; `--install VERSION --from DIR` installs from such a directory with no network, verifying every file again. A dispatcher can then gather a tool and everything it requires into one directory for a machine with intermittent or no connectivity, which is a large part of who Vishpala serves. Each manager already keeps the verified archive; the bundle is a portable view of it.

#### Unattended use

Every command that would ask a question accepts `--yes`, and no command waits for input when its standard input is not a terminal: it refuses with a message instead. manage-restic's `--uninstall` already takes `--yes`.

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

Protocol 1 lives in osat-manager-core: the argument parser, the JSON emitters, holds, bundles and the constraint parser. The conformance suite runs a manager in sandbox mode, the mode manage-python already provides, against a local fixture of verified downloads, and checks every command's output against the schema, every exit status, and that `--remove` refuses a held version. A manager is protocol 1 when it passes the suite. The suite runs offline on every commit; each platform's end-to-end scenario adds one step, a bundle made online and installed offline.

## Gates

| Gate | Result | Evidence | Notes |
| --- | --- | --- | --- |
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The protocol adds no network calls; `--describe`, `--status`, `--check`, `--requires`, holds and `--bundle` are local. Nothing is built to trace |
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
- Notes: Bundles make offline and low-bandwidth installs a supported path rather than a workaround; holds prevent a removal from silently breaking a consumer.

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
- Notes: Bundles avoid repeated downloads across machines. Each consumer can still pull in its own runtime when constraints differ, which costs disk; holds make unused versions visible so they can be removed.

### Security

- Rating: Moderate
- Evidence: Inferred
- Notes: The protocol widens no trust boundary: verification stays in the source kinds, and a bundle is re-verified on install. The new surfaces are holds, files a caller can write, and bundle directories, input from elsewhere. Both need the same treatment as any input: holder identifiers validated like pointer values, and bundle contents checked against the reviewed lock, never against checksums shipped beside them.

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
| Resilience | Strong | Inferred | Bundles and holds |
| Agency, sovereignty & privacy | Strong | Inferred | Local only, no service |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | No exit cost |
| Agency, interaction patterns | Strong | Inferred | No consent assumed in unattended use |
| Environment, direct and indirect | Moderate | Inferred | Fewer downloads; more runtimes when constraints differ |
| Security | Moderate | Inferred | No new trust; holds and bundles are new input |
| Longevity | Strong | Inferred | Additive versioning |
| Content endurance | Not applicable | Verified | No content held |
| Exit and portability | Strong | Inferred | CLI and JSON |
| Adjustability and support | Moderate | Inferred | Breaking change needs coordinated releases |
| Market position | Not applicable | Verified | Internal interface |
| Gates | Pass | Inferred | No gate failed; none yet verified |

## Recommendation

We recommend Trial for protocol 1, delivered through osat-manager-core, once Christopher Steel has decided the open questions below.

Before the trial starts, the planned managers are checked against the schema on paper, as described under the provisional schema, and the result is recorded in this entry as a table of planned managers by kind. The trial scope is then two managers and one edge: manage-python and manage-git implement `--describe`, `--status --json` and `--check CONSTRAINT --json`, manage-python implements holds, and a test harness plays the dispatcher's part, asking manage-git whether git is ready, then manage-python for a runtime satisfying a constraint, recording a hold, and refusing a removal. The pair is chosen because the dependency between them is already real: manage-git requires Python 3.14 (osat-manager-git decision 0001). The trial ends when the conformance suite passes for both on Linux and Windows, or after the first breaking change to the schema, whichever comes first; a breaking change during trial sends the entry back to assess.

Before Adopt, the gates and Security pillar need to be Verified by that trial, and the bundle path needs one end-to-end run with the network disconnected.

## Open questions

- [ ] Which managers are on the planned list, for the paper check of the schema?
- [ ] Usage errors exit 1, as proposed, or a distinct status such as 64?
- [ ] Are holds kept by the providing manager, as proposed?
- [ ] Do requirements live beside each version's lock, as proposed, or in `manager.yaml`?
- [ ] Does protocol 1 belong in osat-manager-core's first release, or its second, after the source kinds have moved?
- [ ] Is the layout drift, install roots and Windows launcher directory, settled in the same change, or separately? The protocol does not depend on it, but a dispatcher reporting paths makes it visible.

## Flip conditions

- **To hold:** the shared-package plan of uc-automa 0035 is set aside, since protocol 1 assumes one implementation.
- **To rejected:** a dispatcher turns out not to be wanted, in which case most of protocol 1 is unnecessary and holds alone may be worth a smaller entry.
- **Back to assess:** mise meets the revisit conditions of uc-automa 0034, which reopens option D.

## Relationship to project

The protocol sits between every OSAT manager and the dispatcher that will drive them. On adopt it graduates to uc-automa: a directive stating protocol 1, a new section in *Creating an OSAT Manager Repository*, and the implementation in osat-manager-core. Each manager's next minor release adopts it. The dispatcher, in both its pass-through and cascade stages, gets its own entry; the pass-through can be assessed as soon as uniform verbs and `--describe` exist. The open question in that guide, where the list of OSAT managers is kept, becomes the dispatcher's registry of tools to managers.

## Status notes

- Last reviewed: 2026-10-27
- In assess: the automated pass recommends Trial in the scope above. Nothing has been built or run; every rating that depends on behaviour is Inferred. The snapshot facts were read from source on 2026-10-27 and are Verified as statements about the code, not about its behaviour on any platform.
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

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring entry: snapshot of four managers on 2026-10-27, what uc-automa has decided, gaps for a cascade, one command `osat` as a thin front end over the managers in two stages, four options, protocol 1 with describe, uniform verbs, one object with two renderings, a provisional schema frozen only on evidence, usage exit status, status as JSON, checks against a constraint, requirements, holds, invocation, bundles, unattended use and protocol versioning; automated recommendation Trial between manage-python and manage-git |

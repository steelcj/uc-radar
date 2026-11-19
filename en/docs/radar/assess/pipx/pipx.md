---
title: "pipx: installing Python command line tools in isolation"
description: "What pipx is, why it exists, and how to use it to install tools such as ssh-audit without touching the system Python."
date: 2026-11-06
tags: [python, pipx, tooling, linux]
---

# pipx: installing Python command line tools in isolation

pipx is a small tool from the Python Packaging Authority (PyPA) that installs Python applications, the kind you run from a terminal, so that each one lives in its own private virtual environment while its command still shows up on your `PATH` <a id="cite-pypa-docs"></a>([Python Packaging Authority, n.d.-a](#ref-pypa-docs)). It is the reason a line like this works cleanly:

```bash
pipx install ssh-audit
```

## The problem it solves

Plain `pip` installs libraries and applications into whatever Python environment is active, with no separation between them. Two tools that need different versions of the same dependency can break each other, and uninstalling rarely removes everything that came along for the ride.

On current Debian and Ubuntu based systems (Linux Mint included), `pip install` outside a virtual environment is refused by default under PEP 668, which marks the system Python as "externally managed" so that pip cannot overwrite packages the operating system depends on. That leaves three reasonable options for a command line tool: the distribution package if one exists, a hand made virtual environment, or pipx, which automates the second option.

## How it works

When you run `pipx install PACKAGE`, pipx creates a dedicated virtual environment for that package, installs it there with pip, and links the package's console entry points into a directory on your `PATH`, typically `~/.local/bin` <a id="cite-pypa-github"></a>([Python Packaging Authority, n.d.-b](#ref-pypa-github)). It runs with ordinary user permissions and never needs `sudo`.

The result:

- each tool has its own dependencies, so tools cannot conflict with one another or with the system Python
- uninstalling removes the whole environment, leaving nothing behind
- the tool is still available as a normal command, for example `ssh-audit`, from any directory

## Installing pipx

### On Linux Mint, Ubuntu and Debian

```bash
sudo apt install pipx
pipx ensurepath
```

`pipx ensurepath` adds `~/.local/bin` to your shell's `PATH`. Open a new terminal afterwards so the change takes effect.

### On macOS

```bash
brew install pipx
pipx ensurepath
```

### On Windows

```powershell
scoop install pipx
pipx ensurepath
```

## Everyday commands

| Command | What it does |
|---|---|
| `pipx install ssh-audit` | Install a tool into its own environment and expose its command |
| `pipx list` | Show installed tools, their versions and the commands they provide |
| `pipx upgrade ssh-audit` | Upgrade one tool |
| `pipx upgrade-all` | Upgrade every tool pipx manages |
| `pipx uninstall ssh-audit` | Remove the tool and its environment completely |
| `pipx run ssh-audit example.org` | Run a tool once in a temporary, cached environment without installing it |
| `pipx inject TOOL EXTRA_PACKAGE` | Add an extra package into an existing tool's environment |
| `pipx reinstall-all` | Rebuild every environment, useful after a system Python upgrade |

## Where things live

By default pipx keeps environments under `~/.local/share/pipx/venvs` and puts command links in `~/.local/bin`. Both can be moved with the `PIPX_HOME` and `PIPX_BIN_DIR` environment variables, and `PIPX_DEFAULT_PYTHON` chooses which interpreter new environments are built with <a id="cite-pypa-man"></a>([Python Packaging Authority, n.d.-c](#ref-pypa-man)).

## When to use something else

- Use `pip` inside a project's own virtual environment for libraries your code imports. pipx is for applications you run, not for project dependencies.
- Use the distribution package (`apt install …`) when you want the operating system to own updates and security patches for the tool.
- Use `pipx run` for something you only need once, so nothing stays installed.

## Why it fits an automation workflow

pipx keeps operator tools out of the system Python, installs per user without root, and can be scripted end to end: `pipx install`, `pipx upgrade-all` and `pipx uninstall` are all non interactive. Removing a tool really removes it, which makes a workstation or admin host easy to rebuild or migrate.

## References

<a id="ref-pypa-docs"></a>
Python Packaging Authority. (n.d.-a). *pipx: Install and run Python applications in isolated environments*. Retrieved November 6, 2026, from https://pipx.pypa.io/stable/
[Return to citation](#cite-pypa-docs)

<a id="ref-pypa-github"></a>
Python Packaging Authority. (n.d.-b). *pipx* [Computer software]. GitHub. Retrieved November 6, 2026, from https://github.com/pypa/pipx
[Return to citation](#cite-pypa-github)

<a id="ref-pypa-man"></a>
Python Packaging Authority. (n.d.-c). *pipx(1) manual page*. Retrieved November 6, 2026, from https://pipx.pypa.io/latest/man/pipx.1.html
[Return to citation](#cite-pypa-man)

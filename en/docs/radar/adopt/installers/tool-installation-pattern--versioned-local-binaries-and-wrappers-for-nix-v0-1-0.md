# The Tool Installation Pattern -- Versioned Local Binaries and Wrappers for Nix

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md

---

## Abstract

This document describes the tool installation pattern established across `hugo-tool`, `pagefind-tool`, and `tool-python-slugify`. The pattern installs command-line tools into versioned directories under `~/bin/[tool-name]/`, isolated from the system package manager, and exposes them via a thin shell wrapper at `~/bin/[tool-name-command]` that is always on `PATH`. The pattern applies to two tool classes: compiled native binaries (Hugo, Pagefind, Claude CLI) and Python tools requiring isolated virtual environments (python-slugify). This document defines the pattern precisely so it can be applied consistently to new tools, including the Claude CLI.

---

## Sources and Acknowledgements

The pattern is derived from three reference implementations: <a name="apa-hugo-tool-citation"></a>[Steel (2026a)](#apa-hugo-tool-reference), `hugo-tool`; <a name="apa-pagefind-tool-citation"></a>[Steel (2026b)](#apa-pagefind-tool-reference), `pagefind-tool`; and <a name="apa-slugify-tool-citation"></a>[Steel (2026c)](#apa-slugify-tool-reference), `tool-python-slugify`. Authoring conventions follow the <a name="apa-styleguide-citation"></a>[style guide for technical documentation for technologists (Steel, 2026d)](#apa-styleguide-reference). Document formatting follows the <a name="apa-markdown-citation"></a>[web-ready unrendered markdown using APA 7 specification (Steel, 2026e)](#apa-markdown-reference).

---

## 1. Motivation and design goals

The pattern exists to satisfy five requirements that system package manager installation and global npm or pip installs do not satisfy simultaneously.

**Version pinning.** Production and development workflows require specific, known, tested versions of tools. `apt install hugo` on Ubuntu 24.04 installs 0.123.7 regardless of what the current project requires. The pattern installs any version explicitly and keeps it.

**Multiple versions.** Upgrading a tool should not destroy the previous version. The pattern stores all installed versions side by side under versioned subdirectories. Rollback is editing one line in the wrapper.

**No system pollution.** Tools installed by this pattern write nothing to `/usr/local/bin`, `/usr/lib`, or any system directory. They require no `sudo`. They do not interact with `apt`, `dnf`, or any system package manager. Removal is `rm -rf ~/bin/[tool-name]` and `rm ~/bin/[command]`.

**Isolation.** For Python tools, each tool's dependencies live in a dedicated virtual environment scoped to that tool and version. They cannot conflict with system Python packages or with each other.

**Portability across providers.** The pattern targets `~/bin` on any nix system. It works identically on Ubuntu, Debian, macOS, and any other POSIX environment where `~/bin` is on `PATH`. No provider-specific or distro-specific package infrastructure is assumed.

---

## 2. Directory structure

### 2.1 Native binary tools

```text
~/bin/
├── [command]                        ← thin shell wrapper, always on PATH
└── [tool-name]/                     ← git clone of the tool repository
    ├── [version]/                   ← versioned binary directory
    │   └── [binary]                 ← the installed binary for this version
    ├── [version-2]/                 ← previous version, retained for rollback
    │   └── [binary]
    └── scripts/
        └── nix/
            └── [command]            ← wrapper template, tracked in git
```

Concrete example from `hugo-tool`:

```text
~/bin/
├── hugo                             ← wrapper: exec $HOME/bin/hugo-tool/v0.163.2/hugo "$@"
└── hugo-tool/
    ├── v0.163.2/
    │   └── hugo
    ├── v0.159.1/
    │   └── hugo
    └── scripts/
        └── nix/
            └── hugo
```

### 2.2 Python virtual environment tools

```text
~/bin/
├── [command]                        ← thin shell wrapper, always on PATH
└── [tool-name]/                     ← git clone of the tool repository
    ├── [tool-script].py             ← the tool's Python entry point
    ├── requirements.txt             ← pinned dependencies
    ├── config.yml                   ← tool configuration
    ├── .venvs/
    │   └── [tool-name]/
    │       └── [version]/           ← virtualenv for this version
    └── scripts/
        └── nix/
            └── [command]            ← wrapper template, tracked in git
```

Concrete example from `tool-python-slugify`:

```text
~/bin/
├── slug                             ← wrapper: activates venv, runs slugify_cli.py
└── slugify-tool/
    ├── slugify_cli.py
    ├── requirements.txt
    ├── config.yml
    └── .venvs/
        └── slugify/
            └── 8.0.4/              ← virtualenv for python-slugify 8.0.4
```

---

## 3. The wrapper

The wrapper is a thin shell script at `~/bin/[command]`. It is always on `PATH`. It does one thing: delegate to the versioned binary or activate the versioned virtualenv and run the tool script. It never contains tool logic.

### 3.1 Native binary wrapper

```bash
#!/usr/bin/env bash
# [command] — wrapper managed by [tool-name].
# Points to the currently active version. To update, rerun the installer
# and render a new wrapper pointing to the new version directory.
# Do not edit the rendered copy at ~/bin/[command] by hand.
exec "$HOME/bin/[tool-name]/[version]/[binary]" "$@"
```

The wrapper is rendered by the installer or by the operator copying it from `scripts/nix/[command]` in the tool repository. The template in the repository is the source of truth; the rendered copy at `~/bin/[command]` is a derivative. The rendered copy is not tracked in git (the tool repository's `.gitignore` excludes version directories). The template is tracked.

### 3.2 Python virtualenv wrapper

```bash
#!/usr/bin/env bash
# [command] — wrapper managed by [tool-name].
# Activates the pinned virtual environment and runs the tool script.
# Do not edit the rendered copy at ~/bin/[command] by hand.
source "$HOME/bin/[tool-name]/.venvs/[tool-name]/[version]/bin/activate"
exec python "$HOME/bin/[tool-name]/[tool-script].py" "$@"
```

---

## 4. The installer

Each tool repository provides an installer. The installer pattern varies by tool class.

### 4.1 Native binary installer (Python script)

The `hugo-tool` installer (`install-hugo.py`) is the reference implementation for native binary tools. It uses only the Python standard library — no packages to install. It:

- queries the GitHub API for the latest stable release tag
- compares the latest tag against the currently active version (read from the wrapper at `~/bin/hugo`)
- does nothing if the latest version is already active
- downloads the correct binary for the current platform (Linux amd64, Linux arm64, macOS Intel, macOS Apple Silicon)
- verifies the SHA-256 checksum against the published checksum file — does not proceed if verification fails
- extracts the binary to `~/bin/hugo-tool/[version]/`
- renders a new wrapper at `~/bin/hugo` pointing to the new version directory
- adds the new version directory to `.gitignore`

The installer is idempotent: running it multiple times produces the same result. It is safe to run on a schedule.

### 4.2 Native binary installer (manual shell variant)

The `pagefind-tool` documents a manual shell variant for operators who prefer to control the installation step by step. The steps follow the same structure as the Python installer but are executed by the operator at the shell:

- fetch the latest release tag from the GitHub API using `curl` and `jq`
- download the binary tarball
- verify the SHA-256 checksum
- extract to the versioned directory
- render the wrapper

This variant is appropriate when the tool does not have a Python installer yet or when the operator wants explicit control over each step. The Python installer is preferred for tools where it exists.

### 4.3 Python virtualenv installer (manual)

The `tool-python-slugify` setup is currently manual. The operator:

- clones the repository to `~/bin/slugify-tool`
- copies the wrapper from `scripts/nix/slug` to `~/bin/slug`
- creates the versioned virtualenv directory: `mkdir -p ~/bin/slugify-tool/.venvs/slugify/[version]`
- creates the virtualenv: `python3 -m venv --prompt slugify-[version] ~/bin/slugify-tool/.venvs/slugify/[version]`
- activates the virtualenv and installs requirements: `pip install -r slugify-tool/requirements.txt`

A Python installer following the `hugo-tool` pattern is a noted future improvement for this tool.

---

## 5. Repository structure

Each tool repository follows a consistent structure:

```text
[tool-name]/
├── .github/                         ← GitHub Actions workflows (where applicable)
├── docs/                            ← installation and usage documentation
├── scripts/
│   └── nix/
│       └── [command]                ← wrapper template for nix systems
├── .gitignore                       ← excludes version directories and venv directories
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── ROADMAP.md
```

For Python tools, additionally:

```text
├── [tool-script].py                 ← tool entry point
├── requirements.txt                 ← pinned dependencies
├── config.yml                       ← active configuration
└── default-config.yml               ← default configuration template
```

For native binary tools with a Python installer:

```text
├── install-[tool].py                ← Python installer (standard library only)
└── install-[tool].sh                ← shell installer (alternative)
```

---

## 6. Applying the pattern to a new tool

To add a new tool following this pattern:

- create a repository named `[tool-name]-tool` (or `tool-[tool-name]` for Python tools, following the established naming)
- create `scripts/nix/[command]` as the wrapper template
- create the installer — Python preferred, shell acceptable
- create `docs/[tool-name]-installation-[platform]-v0.1.0.md` documenting the manual installation steps with expected output at each step
- add `.gitignore` entries for versioned binary directories and venv directories
- clone to `~/bin/[tool-name]-tool` and run the installer

The pattern requires no `sudo`, no system package manager interaction, and no global npm or pip installs outside the tool's own directory.

---

## 7. Tool class: npm-delivered native binaries

The Claude CLI (`@anthropic-ai/claude-code`) is delivered via npm but ships as a native binary — the npm package is a delivery mechanism, not a runtime dependency. The installed `claude` binary does not invoke Node at runtime. This places it in the same tool class as Hugo and Pagefind (native binaries) rather than the Python virtualenv class.

The challenge is that npm's global install mechanism (`npm install -g`) writes to npm's global prefix directory, which by default requires root on some systems and in any case is outside the `~/bin` pattern. The correct approach for this pattern is to use `nvm` (Node Version Manager) to install Node.js in the user's home directory, which places npm's global prefix inside `~/bin` or a comparable user-owned location, and then install the Claude CLI via npm into that prefix.

Alternatively, once the Claude CLI binary is installed by any method, it can be relocated into the versioned directory pattern (`~/bin/claude-tool/[version]/claude`) and wrapped following the same conventions as Hugo and Pagefind. The installation document for the Claude CLI will follow this approach. See `claude-cli--tool--manual-installation-on-debian-13-v0.1.0.md`.

---

## Resources

### Reference implementations
- [hugo-tool](https://github.com/steelcj/hugo-tool)
- [pagefind-tool](https://github.com/steelcj/pagefind-tool)
- [tool-python-slugify](https://github.com/steelcj/tool-python-slugify)

### Governing documents
- [Style guide for technical documentation for technologists](#apa-styleguide-reference)
- [Web-ready unrendered markdown using APA 7](#apa-markdown-reference)

---

## References

<a name="apa-hugo-tool-reference"></a>Steel, C. (2026a). *hugo-tool* [Software]. GitHub. https://github.com/steelcj/hugo-tool
[Return to citation](#apa-hugo-tool-citation)

<a name="apa-pagefind-tool-reference"></a>Steel, C. (2026b). *pagefind-tool* [Software]. GitHub. https://github.com/steelcj/pagefind-tool
[Return to citation](#apa-pagefind-tool-citation)

<a name="apa-slugify-tool-reference"></a>Steel, C. (2026c). *tool-python-slugify* [Software]. GitHub. https://github.com/steelcj/tool-python-slugify
[Return to citation](#apa-slugify-tool-citation)

<a name="apa-styleguide-reference"></a>Steel, C. (2026d). *Style guide: Technical documentation for technologists* (Version 0.2.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-styleguide-citation)

<a name="apa-markdown-reference"></a>Steel, C. (2026e). *Web-ready unrendered markdown using APA 7* (Version 0.2.2) [Technical document]. https://universalcake.ca
[Return to citation](#apa-markdown-citation)

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

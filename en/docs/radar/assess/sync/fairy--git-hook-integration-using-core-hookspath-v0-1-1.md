---
dc:title: "Fairy: Git Hook Integration Using core.hooksPath"
dcterms:version: "0.1.1"
dc:creator: "Christopher Steel"
dc:description: "Recommends git's core.hooksPath over an npm- or pip-based hook manager for activating Fairy's optional post-commit integration, with the decision rationale, an install-hook design, and citations."
dcterms:created: "2026-07-28"
dcterms:modified: "2026-07-28"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:identifier: "fairy--git-hook-integration-using-core-hookspath"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.4.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.1"
    date: "2026-07-28"
    author: "Christopher Steel"
    notes: >
      Renamed the tool from Ferry to Fairy throughout, having found that
      ferry is an existing, actively distributed PyPI package
      (jhorey/ferry), not just a similarly-named repository. Cosmetic
      rename only, the decision, the rationale, the alternatives
      considered, and the citations are unchanged.
  - version: "0.1.0"
    date: "2026-07-28"
    author: "Christopher Steel"
    notes: >
      Initial draft. Recommends core.hooksPath over an npm- or pip-based
      hook manager for Fairy's optional post-commit integration, specifies
      a fairy install-hook subcommand that writes the hook file and prints
      the activation command rather than setting it silently, and cites
      git's own hook documentation alongside third-party analysis of
      hook-manager supply chain risk.
---

# Fairy: Git Hook Integration Using core.hooksPath

Version: 0.1.1
Status: Draft
Style Guide: style-guide--web-ready-unrendered-markdown-using-apa-7

## Abstract

Fairy's optional git hook integration needs a way to activate a committed hook script on a collaborator's machine, since git does not track or clone anything under `.git/hooks` by default. This document recommends git's native `core.hooksPath` configuration variable over an npm- or pip-based hook manager (husky, pre-commit, lefthook), states the rationale and the alternatives considered, and specifies the hook script and a `fairy install-hook` subcommand that activates it. It closes with a note on the supply chain implications of the rejected alternatives, since that question was already open in Fairy's own radar entry under Security.

## Sources and acknowledgements

The mechanics of `core.hooksPath` in this document follow <a name="apa-gitconfig-citation"></a>[Git (n.d.-a)](#apa-gitconfig-reference) and <a name="apa-githooks-citation"></a>[Git (n.d.-b)](#apa-githooks-reference), git's own documentation for `git-config` and `githooks` respectively. The supply chain comparison of hook managers draws on <a name="apa-madge-citation"></a>[Madge (2026)](#apa-madge-reference). The team-standardization pattern for committing a shared hooks directory follows <a name="apa-atlassian-citation"></a>[Atlassian (2025)](#apa-atlassian-reference).

## The problem

Git looks for hooks in `$GIT_DIR/hooks` by default, a directory that is never part of a commit and never travels with a clone <a name="apa-githooks-citation-2"></a>([Git, n.d.-b](#apa-githooks-reference)). A `post-commit` hook written for Fairy and placed there works on the machine that wrote it and nowhere else, so every collaborator who wants the same integration has to be told about it separately and set it up by hand, and there is no way to commit that setup step directly into the repository for them.

## The recommendation

Commit the hook script into the repository under a tracked directory, `.githooks/`, and point git at it with the `core.hooksPath` configuration variable, which by default Git will look for your hooks in the `$GIT_DIR/hooks` directory, and setting this to a different path tells Git to look there instead <a name="apa-gitconfig-citation"></a>([Git, n.d.-a](#apa-gitconfig-reference)). This configuration variable is useful in cases where you would like to centrally configure your Git hooks instead of configuring them on a per-repository basis <a name="apa-gitconfig-citation-2"></a>([Git, n.d.-a](#apa-gitconfig-reference)), which is exactly Fairy's case, a hook that should behave the same way regardless of which project it is installed in. Activation is one command, run once per clone:

```bash
git config core.hooksPath .githooks
```

## Decision and rationale

We decided to activate Fairy's hook through `core.hooksPath` rather than through a hook manager such as husky, pre-commit, or lefthook.

We chose this because it adds no dependency beyond git itself, which every target of a git hook already has by definition. Fairy's own design goal is to stay standalone and dependency-light, not tied to any single project's existing toolchain, and pulling in an npm or pip-based hook manager to activate one shell script would work against that goal directly, adding a heavier dependency than the tool it exists to support.

We considered husky, which integrates directly with Git by modifying the .git directory and is well known for straightforward setup, wiring itself up through a `prepare` script so that `npm install` activates the hook automatically. We rejected it for two reasons. First, it requires npm in every project that wants Fairy's hook, regardless of whether that project uses JavaScript for anything else. Second, and more materially, the automatic activation is itself the concern, the prepare script makes this slightly more acute, a compromised version of Husky would execute automatically on npm install without any explicit developer action <a name="apa-madge-citation-2"></a>([Madge, 2026](#apa-madge-reference)). An install step that can silently run arbitrary code is precisely the question the project's own Security assessment already asks of any tool under consideration.

We considered lefthook, distributed as a single Go binary with no runtime dependency of its own, and noted for parallel hook execution and caching. It carries a comparable trust question to husky, whichever channel it comes through, you are trusting a third-party package, and any of those distribution channels could in principle be compromised, and the binary distribution angle is worth noting because a compromised binary is harder to audit than JavaScript source <a name="apa-madge-citation-3"></a>([Madge, 2026](#apa-madge-reference)). It is a reasonable choice for a project already juggling several hooks across languages, but Fairy only needs to activate one script, so the added binary dependency buys nothing here.

We considered pre-commit, which offers the broadest ecosystem of prebuilt hooks and full environment isolation, including the ability to run hooks in languages not installed on the developer's machine <a name="apa-madge-citation-4"></a>([Madge, 2026](#apa-madge-reference)). The isolation is real value when a project is already composing many hooks written in different languages. Fairy is not that case, one script, one language, no composition need, so the isolation is a cost with no offsetting benefit here.

The alternative of doing nothing, leaving activation entirely manual with no tracked hook file at all, was rejected because it is strictly worse than the recommendation, it has the same manual step (telling each collaborator what to run) without the benefit of the script itself being reviewable, diffable, and versioned alongside the code it operates on.

## Implementation

### The hooks directory

The hook script is committed at `.githooks/post-commit` and stays deliberately read-only, it reports what Fairy would change and never writes anything on its own:

```bash
#!/bin/sh
# .githooks/post-commit
# Reports any pending Fairy sync after a commit. Never writes on its own.

if ! command -v fairy >/dev/null 2>&1; then
    exit 0
fi

fairy plan --config sync.yaml --quiet-if-empty
```

Silence on an irrelevant commit is intentional, `--quiet-if-empty` suppresses output entirely when the plan has nothing to report, so the hook does not add noise to commits that touch nothing Fairy tracks. Applying a plan stays a separate, deliberate step, `fairy apply`, run by a person who has just read what it would do, which keeps the hook itself from ever being the thing that writes to another project's files unattended.

### fairy install-hook

A `fairy install-hook` subcommand does the setup, but stops short of changing the collaborator's git configuration for them. It writes `.githooks/post-commit` (creating the directory and setting the executable bit if needed) and then prints the activation command rather than running it:

```text
$ fairy install-hook
Wrote .githooks/post-commit

To activate it, run:
  git config core.hooksPath .githooks
```

This mirrors the same principle already applied to the hook's own behaviour, at every point where Fairy could act on its own initiative, it reports instead and leaves the actual write, whether that is a file copy or a git configuration change, to an explicit command the person runs themselves.

## Security note

The choice made here directly answers an item left open in Fairy's radar entry, where the Security section was rated Unknown pending review. Activating the hook through `core.hooksPath` avoids the specific supply chain exposure that a hook manager's install-time activation carries, since there is no `prepare` script, no `postinstall`, and no third-party binary in the activation path, only git itself and a script committed in the same repository under the same review process as everything else in it.

## Cross-platform note

The `post-commit` script above is POSIX shell, which git for Windows executes correctly, but a Windows environment without Git Bash present would need a wrapper or a rewritten script. This is a known gap, not yet resolved, and should be closed before the radar entry's Compatibility rating moves past Moderate.

## License

This document, *Fairy: Git Hook Integration Using core.hooksPath*, by **Christopher Steel**, with AI assistance from **Claude Sonnet 5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Git documentation

- [git-config](#apa-gitconfig-reference)
- [githooks](#apa-githooks-reference)

### Hook manager comparison

- [Git hook frameworks comparison](#apa-madge-reference)
- [Standardize git hooks across a repository](#apa-atlassian-reference)

## References

<a name="apa-gitconfig-reference"></a>Git. (n.d.-a). *git-config* [Software documentation]. Retrieved July 28, 2026, from https://git-scm.com/docs/git-config
[Return to citation](#apa-gitconfig-citation)

<a name="apa-githooks-reference"></a>Git. (n.d.-b). *githooks* [Software documentation]. Retrieved July 28, 2026, from https://git-scm.com/docs/githooks
[Return to citation](#apa-githooks-citation)

<a name="apa-madge-reference"></a>Madge, A. (2026, March 10). *Git hook frameworks comparison*. Retrieved July 28, 2026, from https://www.andymadge.com/2026/03/10/git-hooks-comparison/
[Return to citation](#apa-madge-citation)

<a name="apa-atlassian-reference"></a>Atlassian. (2025, September 26). *Standardize git hooks across a repository*. Bitbucket Cloud Support. Retrieved July 28, 2026, from https://support.atlassian.com/bitbucket-cloud/kb/standardize-git-hooks-across-a-repository/
[Return to citation](#apa-atlassian-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.1 | Draft | Renamed Ferry to Fairy throughout, avoiding a naming collision with the existing jhorey/ferry PyPI package; no change to the decision, rationale, or citations |
| 0.1.0 | Draft | Initial draft, recommends core.hooksPath over a hook manager, specifies the post-commit script and fairy install-hook, cites git's own documentation and third-party hook-manager risk analysis |

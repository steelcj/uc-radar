

| Option           | Two-way                                    | Works offline and in release tarballs                        | Windows                             | Main drawback                                             |
| ---------------- | ------------------------------------------ | ------------------------------------------------------------ | ----------------------------------- | --------------------------------------------------------- |
| Symlinks         | Yes, edits land in the automa checkout     | No, a lone clone or tarball gets dangling links              | Poor, needs developer mode or admin | Depends on one machine's layout                           |
| Git submodule    | Yes, edit inside it and push               | No, `git archive` and GitHub source zips omit submodule contents | Fine                                | Whole-repo granularity, pinned-commit friction            |
| Git subtree      | Yes, `git subtree push` sends changes back | Yes, the files are real                                      | Fine                                | Noisy merge history, works best on one exported directory |
| file-fairy (now) | No                                         | Yes                                                          | Fine                                | Copies of executable code drift, one-way                  |
| Installed tool   | Not applicable, nothing is copied          | Yes, once installed locally                                  | Fine with manage-python             | Needs packaging and a pinned version per repo             |

My recommendation is a hybrid:

- **Stop copying the scripts.** Package the release ceremony as a versioned tool, installed into user space the way manage-python installs Python, and have each repo pin the version it expects. One fix then reaches every repo on its next run, instead of five files times every repo. It also removes the most error-prone item in your findings, executable code drifting between copies.
- **Reference the reading material rather than copying it.** Keep one local checkout of the automa at a known, versioned path. Have the CLAUDE.md signpost point to it, with the GitHub URL as a fallback. Copy only what a reader of the published repo actually needs, such as the licence text.
- **Keep file-fairy for what must live in the repo.** `seed_if_missing`, managed blocks and retraction are exactly right for `VERSION`, licence blocks and the signpost.
- **Add a reverse verb to file-fairy for the two-way part.** The state file already records the source checksum and the target checksum. An item whose target changed while its source did not is precisely a local improvement worth sending upstream. A `file-fairy harvest` could list those items and write them back to the automa checkout on a branch for review. That's a small addition to a tool you already trust, and it keeps the automa as the single canonical source.

If one part of the automa is being co-developed heavily alongside a product, git subtree is the cleanest true two-way option for that one directory. I wouldn't use symlinks or submodules here. Both fail the self-contained release and offline requirements that your publish ceremony and Balise depend on.

Copier is also worth a radar look, since cookiecutter and cruft are already assessed there. Its `copier update` does a three-way merge of template changes into a repo that has local edits. It is still one-way, but it handles divergence better than cruft.

This is the kind of decision your findings said should go through the radar. Would you like me to draft it as a downloadable radar entry or ADR in your house style?

## References

<a id="ref-chacon-2014"></a>Chacon, S., & Straub, B. (2014). Git tools: Submodules. In Pro Git (2nd ed.). Apress. https://git-scm.com/book/en/v2/Git-Tools-Submodules ↩

Submodules: the automa as a pinned sub-repository, edited in place and pushed back. Contents are omitted from `git archive` and GitHub source zips.

<a id="ref-cookiecutter-nd"></a>Cookiecutter Developers. (n.d.). Cookiecutter documentation. Retrieved October 7, 2026, from https://cookiecutter.readthedocs.io/ ↩

Project templating, one-shot: generates a new repository from a template, with no update path of its own.

<a id="ref-copier-org-nd"></a>copier-org. (n.d.). Updating a project. Copier documentation. Retrieved October 7, 2026, from https://copier.readthedocs.io/en/stable/updating/ ↩

Project templating with updates: regenerates from a newer template version and re-applies local differences, recording state in `.copier-answers.yml`. A candidate for the new repository procedure.

<a id="ref-crosley-nd"></a>Crosley, T. (n.d.). cruft. Retrieved October 7, 2026, from https://cruft.github.io/cruft/ ↩

Keeps Cookiecutter-generated projects in step with their template, with a check command and diffs against the template.

<a id="ref-git-for-windows-nd"></a>Git for Windows. (n.d.). Symbolic links. Retrieved October 7, 2026, from https://gitforwindows.org/symbolic-links ↩

Why symlinks are a poor cross-platform choice: disabled by default, and creating them needs administrator rights or Developer Mode.

<a id="ref-git-project-nd-a"></a>Git Project. (n.d.-a). gitattributes: Defining attributes per path. Retrieved October 7, 2026, from https://git-scm.com/docs/gitattributes ↩

Line-ending policy (`text=auto`, `eol=lf`) to stop CRLF checkouts on Windows from registering as false drift in file-fairy checksums.

<a id="ref-git-project-nd-b"></a>Git Project. (n.d.-b). git-subtree [Documentation]. GitHub. Retrieved October 7, 2026, from https://github.com/git/git/blob/master/contrib/subtree/git-subtree.adoc ↩

Subtree: real files inside the receiving repository, with `pull` to update and `push` (a `split` then a push) to send changes back. Two-way, and safe for release tarballs.

<a id="ref-google-nd"></a>Google. (n.d.). Copybara [Computer software]. GitHub. Retrieved October 7, 2026, from https://github.com/google/copybara ↩

Moves and transforms code between repositories around one authoritative source, accepting contributions from the others. Heavier: Java 11+ and Bazel, or Docker.

<a id="ref-hess-nd"></a>Hess, J. (n.d.). myrepos. Retrieved October 7, 2026, from https://myrepos.branchable.com/ ↩

Runs one command across many repositories (`mr update`, `mr commit`, `mr push`). Relevant to running every file-fairy manifest at once; already the subject of an OSAT Fluent tool.

<a id="ref-ingy-nd"></a>Ingy döt Net. (n.d.). git-subrepo [Computer software]. GitHub. Retrieved October 7, 2026, from https://github.com/ingydotnet/git-subrepo ↩

An alternative to submodule and subtree with simpler workflows, including `git subrepo push` upstream. Works with Git for Windows; the authors describe it as production-used but likely still buggy.

<a id="ref-josh-project-nd"></a>josh-project. (n.d.). josh [Computer software]. GitHub. Retrieved October 7, 2026, from https://github.com/josh-project/josh ↩

Reversible history filtering that presents a subdirectory as its own repository, run as a CLI or a git proxy. More suited to a monorepo than to the current set of repositories.

<a id="ref-pierce-nd"></a>Pierce, B. C. (n.d.). Unison file synchronizer [Computer software]. GitHub. Retrieved October 7, 2026, from https://github.com/bcpierce00/unison ↩

Mature two-way file synchronisation for Linux, BSD, macOS and Windows, with conflict detection. A reference design for a two-way file-fairy, or an alternative to one.

<a id="ref-pre-commit-nd"></a>pre-commit. (n.d.). pre-commit. Retrieved October 7, 2026, from https://pre-commit.com/ ↩

Runs hooks fetched from a remote repository at a pinned `rev`, on Linux, macOS and Windows, with `autoupdate` to move the pin. A way to consume `check-conformance.py` without copying it.

<a id="ref-pypa-nd-a"></a>Python Packaging Authority. (n.d.-a). Entry points specification. Python Packaging User Guide. Retrieved October 7, 2026, from https://packaging.python.org/en/latest/specifications/entry-points/ ↩

Console-script entry points give a native launcher on Windows and a script on Linux and macOS from one definition. The basis for shipping the release ceremony as an installed tool.

<a id="ref-pypa-nd-b"></a>Python Packaging Authority. (n.d.-b). pipx. Retrieved October 7, 2026, from https://pipx.pypa.io/stable/ ↩

Installs Python command-line applications into isolated environments, from PyPI, local directories, wheels or git URLs. A possible installer for the ceremony tool alongside manage-python.

<a id="ref-reproducible-builds-nd"></a>Reproducible Builds. (n.d.). Archive metadata. Retrieved October 7, 2026, from https://reproducible-builds.org/docs/archives/ ↩

Fixed mtime, name-sorted entries and numeric ownership for deterministic tarballs. Needed for `publish-release.py` to give the same `SHA256SUMS` on every platform.

<a id="ref-steel-2026a"></a>Steel, C. (2026a). Universal Cake evaluation: Cookiecutter, document templating (Version 0.1.0) [Radar entry]. uc-radar, GitHub. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/assess/design/universal-cake-evaluation--cookiecutter--document-templating-v0-1-0.md ↩

The existing radar assessment of Cookiecutter.

<a id="ref-steel-2026b"></a>Steel, C. (2026b). Universal Cake evaluation: Cruft, document template sync (Version 0.1.0) [Radar entry]. uc-radar, GitHub. https://github.com/steelcj/uc-radar/blob/main/en/docs/radar/assess/design/universal-cake-evaluation--cruft--document-template-sync-v0-1-0.md ↩

The existing radar assessment of cruft, the natural starting point for a Copier comparison.
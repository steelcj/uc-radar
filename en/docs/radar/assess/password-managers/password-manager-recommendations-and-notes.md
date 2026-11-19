# Password Manager Recommendations and Notes

I’ve written all three evaluations against the latest metrics, v0.6.0. They follow the same layout as your xkcdpass entry and go in `en/docs/radar/assess/password-managers/`. I installed and tested each tool in a throwaway setup on Ubuntu 24.04. That covered network and file activity, how the output reads as plain text, and whether the whole store can be exported. All three pass the four gates. The accessibility gate is only Inferred, though, because I couldn’t test with a screen reader here. Under v0.6.0 that keeps all three in Assess until someone does that check.

|                | pass 1.7.4                                                   | gopass 1.17.4                                                | passage 1.7.4a2                                           |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------- |
| Recommendation | Trial (one technical user or small team)                     | Trial (teams, Windows)                                       | Stay in Assess                                            |
| Risk           | Low                                                          | Low                                                          | Moderate                                                  |
| Main strength  | Small and readable, stable file format, no network use       | Active project, signed releases, written security model      | Uses age instead of GnuPG, so keys are much simpler       |
| Main weakness  | No release since June 2021; GnuPG setup is hard for non-technical people | One person decides and writes 70% of the human commits; large dependency tree; a run of security fixes in 2026 | Only alpha tags, no changes since 2024, no `init` command |

Things that came up in testing:

- **Ubuntu name clashes:** Ubuntu’s `gopass` package is a different, older project (`aviau/gopass` 1.5.0), and its `passage` package is a game.
- **GnuPG 2.4 and Android:** keys made with current GnuPG cause pass and gopass to encrypt in a mode (OCB AEAD) that OpenKeychain-based Android apps can’t read. The fix is to remove OCB from the key’s preferences before running `init`.
- **`passage init`:** it prints “command not found” but still reports success (exit status 0).
- **Output for screen readers:** pass and passage always add colour codes and tree drawing to their output, even when it’s redirected or `NO_COLOR` is set. gopass leaves out colour when output isn’t a terminal, but starts messages with emoji and writes progress-bar codes into redirected `audit` and `fsck` output.
- **gopass network use:** it only goes online for `gopass version`, which can be turned off. But it pushes to a git remote automatically by default once one is set up.

Two conventions pulled in different directions:

- **Filenames:** I used dots (`-v0.1.0.md`), as you asked for this project. The repo’s `check-conformance.py` and its versioning rule still call for hyphens, so the checker flags these three files and nothing else.
- **Citation anchors:** I used your `id="ref-…"` / `#cite-…` format. The xkcdpass entry uses the older `name="apa-…"` style.

Version numbers are marked provisional for you to confirm. Nothing has been committed. I can open a PR on a branch if you want.

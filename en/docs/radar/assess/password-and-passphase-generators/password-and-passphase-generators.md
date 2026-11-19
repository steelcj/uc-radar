# password-and-passphase-generators

Yes, several. Here are the ones that fit how you’ve been judging these tools (offline, free licence, cross-platform), with the closest matches to xkcdpass first.

**Command-line passphrase generators**

- **diceware** (Python, GPL): the `diceware` package on PyPI by ulif. It’s the obvious peer to xkcdpass and also works with physical dice: you can roll real dice and type the numbers in, so you don’t have to trust the computer’s randomness. It includes the EFF long list and supports other languages.
- **Phraze** (Rust, MPL-2.0): by Sam Schlinkert. It’s a single binary that runs on Windows, macOS and Linux with nothing else to install. It ships carefully built wordlists from his Orchard Street project and prints the strength of each passphrase. His companion tool `tidy` cleans up and checks wordlists, which could help with your EFF list cross-check.
- **go-diceware** (Go, Apache-2.0): by Seth Vargo. It’s mainly a library, with a small command-line tool, and builds as a single binary for any platform.
- **XKPasswd** (Perl, BSD): by Bart Busschots, the “xkcd-style but configurable” generator. It has presets, padding and adjustable word lengths, and a web version that runs entirely in the browser.

**Built into tools you’re already looking at**

- **gopass:** `gopass generate --generator xkcd` (alongside `--xkcd-lang`, `--xkcd-sep` and `--xkcd-numbers`) gives xkcd-style passphrases. I checked the source: it uses the `goxkcdpwgen` library and asks for **four words by default**, which is on the short side. Ask for six or seven explicitly. I haven’t checked which wordlist it uses or how it picks words at random, so treat it as unverified for now.
- **KeePassXC** (GPL, Windows, macOS, Linux): its built-in generator has a Passphrase tab that uses the EFF list and shows the strength. It’s probably the most accessible option for non-technical people, since it has a full graphical interface. Its command-line tool also has `keepassxc-cli diceware`.

**No install needed**

- **Physical dice plus the EFF long list:** the most cross-platform method there is. It works with no computer at all and has nothing to trust in its randomness. It’s slow, and it’s hard for people with limited dexterity or vision.
- **Python’s `secrets` module:** a few lines of code. As your passphrase guide already notes, it’s the simplest choice because it adds nothing new to trust.

Commercial password managers such as Bitwarden, 1Password and Proton Pass also generate passphrases. They tie you to an account, though, so they don’t fit your agency criteria as standalone generators.

I’ve used everything above except the gopass check from what I already know, so licences and current maintenance status should be confirmed before anything goes on the radar. If you’d like, I can run diceware, Phraze and KeePassXC’s generator through the same Assess-ring checks as xkcdpass. That would cover where each gets its randomness, which wordlist it bundles, any network contact, and how its text output works with a screen reader.

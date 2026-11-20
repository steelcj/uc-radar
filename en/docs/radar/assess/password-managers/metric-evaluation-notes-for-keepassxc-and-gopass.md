# metric-evaluation-notes-for-keepassxc-and-gopass

I’ve written two more evaluations against the v0.6.0 metrics, saved next to the earlier three. One is a full evaluation of KeePassXC 2.8.0. The other covers gopass’s password generators and the graphical front ends you can use with gopass. I installed and tested both tools here: tracing network and file activity, running the generators, and exporting. For KeePassXC I also checked that a separate library can open its database file.

**KeePassXC: Trial for individuals and small offices, after a screen reader check.** It’s the best fit so far for non-technical clients. It needs no GnuPG or git, has a graphical interface in 47 languages, and passes all four gates.

- **Network use:** the command-line tool made no network connections at all. The desktop app asks on first start whether to check for updates. The breach check and website-icon downloads only run when you ask.
- **Security record:** it has a written security policy with private reporting routes. An independent review of 2.7.4 was published in 2023 KeePassXC, a popular free and open source cross-platform password manager, has announced the release of an audit conducted by independent security consultant Zaur Molotnikov. It also holds a French national certification: ANSSI Security Visa (ANSSI-CSPN-2025/16), November 17, 2025, valid until November 17, 2028, for KeePassXC 2.7.9 (Windows 10). Version 2.8.0 fixed eight security issues. [alternativeto](https://alternativeto.net/news/2023/4/password-manager-keepassxc-receives-positive-audit-from-independent-security-consultant)[pages](https://keepassxc-org.pages.dev/audits)
- **Weak default in the command-line tool:** `keepassxc-cli db-create` still creates the older database format (KDBX 3.1, with the older AES key-stretching method), even with `-t`. The graphical app creates the newer KDBX 4 format with Argon2. So create databases in the app, or upgrade any made from the command line.
- **Generators:** both use the system’s secure random source and pick without bias. The default passphrase is 7 words, about 90 bits. However, its built-in list is a modified EFF list: 30 EFF words are removed and 26 other words added. It can’t be used with dice or for your EFF list cross-check, but you can load the original EFF file instead.
- **Packaging:** Ubuntu 24.04 still ships 2.7.6 from 2023. The AppImage’s command-line tool won’t start on a server that lacks `libOpenGL.so.0`.
- **AI in development:** the project uses AI tools in development, which drew criticism. The maintainers stated that there are no AI features inside KeePassXC and there never will be. [pipedot](https://pipedot.org/71BKD)

**gopass generators: usable, but change the defaults.** The three built-in generators use Go’s secure random source and never go online. The default character generator gives about 143 bits.

- **Passphrase (xkcd) mode** defaults to 4 words, about 52 bits. The documentation says words are separated by spaces, but with nothing configured they come out run together with capitals. Set `pwgen.xkcd-len` to 7 and set `pwgen.xkcd-sep` yourself.
- **“Memorable” mode** stops at a length rather than a word count. In about 2% of runs at the default length it produces only 3 words, about 43 bits.
- **No minimum length:** it generated a 1-character password without any warning.
- **Site rules:** when an entry’s name matches one of 342 known sites, gopass applies that site’s password rules and can shorten the password. In my test, 20 characters became 8 when no one answered the prompt. `--force` turns these rules off and also overwrites existing entries.
- **Debug log leak:** with `GOPASS_DEBUG_LOG` set, candidate passwords that gopass rejected are written to the log in plain text, despite the documentation saying otherwise. The final password was not logged.

**Graphical front ends for gopass:**

- **QtPass** is the maintained option. Debian accepted qtpass 1.8.4-1 (source) into unstable on 2026-10-30, but Ubuntu 24.04 has the old 1.4.0. In my test, gopass run under the name `pass` handled the basic commands. I didn’t test QtPass itself with a gopass store. [debian](https://tracker.debian.org/qtpass)
- **Gopass UI** isn’t worth using. It is an Electron app Last Pushed: 2023-03-03, so it has gone three years without browser-engine security updates. [ecosyste](https://awesome.ecosyste.ms/projects/github.com%2Fcodecentric%2Fgopass-ui)
- **gopassbridge** (browser extension) and gopass 1.17.4’s new built-in Secret Service daemon (which lets desktop apps keep their secrets in the store) each give other programs access to the store. They should get their own radar entries.

For non-technical clients, KeePassXC is a better graphical choice than any gopass front end. The bugs I found in gopass (the separator mismatch, the debug-log leak and the double meaning of `--force`) are worth reporting upstream.

The filenames use dot version numbers again, so the repo’s conformance check flags them; nothing else fails it. Nothing is committed.
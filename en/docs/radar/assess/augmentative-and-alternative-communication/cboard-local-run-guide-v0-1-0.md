# Running Cboard locally — rough guide

**Status:** Draft, not yet an osat-fluent tool
**Version:** 0.1.0
**Scope:** Linux, macOS, Windows (desktop hosts only — see note on iPad below)

---

## Prerequisites

- Node.js — pinned version in `.nvmrc` (currently `v22.14.0`)
- Yarn (classic) — the repo uses `yarn.lock`, not `package-lock.json`
- No Python required. No native/compiled dependencies in the tree
  (`fsevents` is the only near-native package, macOS-only, optional,
  ships prebuilt).

---

## Linux

```bash
# install nvm if not already present
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

cd ~/bin  # or wherever you keep cloned tools
git clone https://github.com/cboard-org/cboard.git
cd cboard
nvm install   # reads .nvmrc
nvm use
corepack enable  # or: npm install -g yarn
yarn install
yarn start
```

Opens `http://localhost:3000` automatically.

---

## macOS

Same as Linux — `nvm` works identically.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

cd ~/bin
git clone https://github.com/cboard-org/cboard.git
cd cboard
nvm install
nvm use
corepack enable
yarn install
yarn start
```

`fsevents` will install automatically (macOS-only, prebuilt, no compile step).

---

## Windows

`nvm` (Linux/macOS) does not exist on Windows. Use one of:

- **nvm-windows** — https://github.com/coreybutler/nvm-windows
- **fnm** — https://github.com/Schniz/fnm (cross-platform, simpler)

```powershell
# using nvm-windows, after installing it
nvm install 22.14.0
nvm use 22.14.0

git clone https://github.com/cboard-org/cboard.git
cd cboard
corepack enable
yarn install
yarn start
```

Opens `http://localhost:3000` in the default browser.

**Known Windows friction points to watch for** (not yet verified against this
repo, flagging as risk):

- long path names — `node_modules` trees can exceed Windows' historical
  260-character path limit; enable long paths in Windows or use a short
  clone path (e.g. `C:\dev\cboard`) if `yarn install` fails partway
- CRLF/LF line-ending noise in `git status` — cosmetic, not a functional
  blocker

---

## What you get

`yarn start` runs the Create React App dev server: hot-reload, unminified,
verbose console output. Fine for local personal use, not something you'd
want running unattended as a service — no process supervision, no
restart-on-crash.

---

## iPad

Not a host — see prior discussion. iPad has no Node runtime and no
persistent-background-process model that a dev server needs. Options
remain: point iPad Safari at one of the desktop hosts above over LAN,
or use the official Cboard iOS App Store app.

---

## Next steps (not done here)

- Formal osat-fluent tool (pin Node version, scripted install, wrapper) —
  deferred per earlier discussion; this guide covers the manual path only.
- `yarn build` + static serve, if a longer-running instance (rather than
  the dev server) becomes the actual requirement.

---

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial rough guide, Linux/macOS/Windows manual run, no Python required confirmed against yarn.lock |

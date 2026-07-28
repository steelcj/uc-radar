---
title: "Free and open source screen readers and talking browsers"
description: >
  Technical overview of free/libre screen reading software and talking
  browser tools — NVDA, ChromeVox, Firefox-based approaches, and WebbIE —
  with current status, architecture notes, and Universal Cake relevance.
creator: Christopher Steel
publisher: Vishpala
date: 2026-07-08
version: v0.1.0
language: en-CA
subject:
  - screen-readers
  - nvda
  - chromevox
  - webbie
  - assistive-technology
  - open-source
  - accessibility
  - universal-cake
type: technical-overview
rights: CC BY-SA 4.0
---

# Free and open source screen readers and talking browsers

For most of the 1990s and 2000s, screen reading on a personal computer
meant a commercial licence costing as much as the computer itself —
JAWS, the market leader, sold for roughly a thousand dollars and still
sells subscription access today. The free and open source screen
readers surveyed here exist as a direct response to that gatekeeping:
each began from the conviction that the ability to use a computer at
all should not carry a toll. This document covers the standalone
free/libre screen readers and the smaller, largely historical category
of "talking browsers"; a companion document covers the screen readers
now built into every major operating system.

A note on currency: resource lists in circulation (including the one
this document updates) often reflect the landscape of the early 2010s.
The category has consolidated dramatically since then. One project —
NVDA — became world-class infrastructure; the talking-browser niche was
mostly absorbed into operating systems; and several once-recommended
tools survive only in maintenance mode. Status below is verified as of
July 2026.

## NVDA (NonVisual Desktop Access)

NVDA is the flagship of libre assistive technology and, by usage, one
of the most successful open source end-user applications in any
category. It began in April 2006 when Michael Curran, a blind
programmer concerned by the high cost of commercial screen readers,
started writing a free alternative in Python; he and James Teh — both
blind — founded the non-profit NV Access in 2007 to sustain its
development (Wikipedia, 2026).

Current status and technical profile:

- **Actively and rapidly developed.** The current release is 2026.1.1
  (May 2026), on a predictable multi-release annual cadence with beta
  and alpha channels (NV Access, 2026a). Recent releases added built-in
  MathCAT mathematics reading, 64-bit SAPI 5 voice support, improved
  braille on secure screens, and NVDA Remote Access for controlling a
  remote machine (NV Access, 2026a).
- **Licence:** modified GNU GPL v2 or later; source on GitHub with a
  contributor code of conduct, a published product vision, and a public
  roadmap (nvaccess, 2026).
- **Platform:** Microsoft Windows 10 and later, with archived versions
  maintained for older Windows releases (NV Access, 2026b).
- **Architecture:** Python core over Windows accessibility APIs (UIA,
  IAccessible2), with the eSpeak NG synthesizer built in — speech in
  over 55 languages out of the box — plus support for SAPI and
  third-party voices, a wide range of refreshable braille displays with
  braille keyboard input, and OCR of inaccessible content (Wikipedia,
  2026; NV Access, 2026a).
- **Portability:** runs entirely from a USB stick without installation
  — significant for people using shared, institutional, or borrowed
  computers (Wikipedia, 2026).
- **Extensibility:** a community add-on ecosystem with an in-app Add-on
  Store; NV Access warns plainly that add-ons are not vetted and carry
  real trust implications (NV Access, 2026c).
- **Adoption:** the WebAIM screen reader survey for 2023–2024 found
  NVDA the most commonly used screen reader worldwide, used often by
  65.6% of respondents and as the primary screen reader by 37.7% —
  second only to JAWS as primary, having briefly overtaken it in prior
  surveys (Wikipedia, 2026). It is especially significant in lower-income
  countries, where the free licence is the difference between access
  and no access.

One recent development worth noting through a sovereignty lens: NVDA
now includes an opt-in usage-statistics setting, relocated in 2026.1
into a new dedicated Privacy and Security settings category (TechSpot,
2026) — telemetry handled the way libre software should handle it:
visible, named, and off by default under user control.

## ChromeVox

ChromeVox began as Google's demonstration that a screen reader could be
built entirely from web technologies — HTML, CSS, and JavaScript. Its
status is the most changed item on the legacy resource lists, and the
old chromevox.com address should no longer be circulated.

- **The Chrome browser extension is deprecated.** The webstore version,
  now called ChromeVox Classic, is in maintenance mode with no new
  development, is only loosely related to the current product, and its
  own listing directs users to full system screen readers instead —
  NVDA, Narrator, VoiceOver, or ChromeVox on ChromeOS (Chromium
  Project, n.d.; crx4chrome, n.d.).
- **The real ChromeVox is now built into ChromeOS.** It was
  incorporated into the operating system itself, ships as part of every
  Chromebook, and is toggled with Ctrl+Alt+Z (Chromium Project, n.d.;
  Google, n.d.). It became the default Chromebook screen reader from
  ChromeOS 56, with braille display support over USB including braille
  keyboard input, menus, a tutorial, and positional "earcon" audio cues
  (Google, 2017).

ChromeVox therefore now belongs in the built-in category, and is
treated alongside Orca, Narrator, and VoiceOver in the companion
document. Its remaining relevance in this document is historical: it
proved the browser-native approach, and its original creator, Charles
Chen, had previously built Fire Vox, the Firefox talking-browser
extension that defined the niche.

## Firefox add-ons

The Firefox "screen reader add-on" category, once anchored by Fire Vox,
has effectively dissolved. Two things replaced it:

- **Proper screen reader support in Firefox itself.** Firefox exposes
  content through platform accessibility APIs and is a first-class
  citizen for NVDA (with which it has a long co-development history),
  Orca, VoiceOver, and Narrator. The correct modern recommendation is
  Firefox *with* a real screen reader, not a screen-reading add-on.
- **Built-in read-aloud.** Firefox's Reader View includes a Narrate
  feature that speaks simplified page text with adjustable voice and
  speed — useful for print disabilities, fatigue, and multitasking,
  though it is a reading aid, not a screen reader: it offers no
  interface navigation, forms interaction, or braille.

Legacy resource lists pointing at the addons.mozilla.org screen-reader
tag should be retired or repointed; what remains under that tag today
is a thin scattering of text-to-speech utilities rather than assistive
technology in any load-bearing sense.

## WebbIE

WebbIE is the survivor of the talking/text-only browser category, and
an instructive one. Developed and given away since 2001 by Dr Alasdair
King in England, it renders web pages as plain text — deliberately
discarding low-value content on the principle that "every line that a
screenreader user must encounter slows them down" (King, n.d.-a). It
works alongside any screen reader (NVDA, JAWS, Narrator and others),
respects Windows high-visibility colour schemes, supports forms,
webmail, RSS, and HTML5 audio/video, is translated into a dozen
languages, and ships with a suite of accessible companion programs —
PDF reader, podcatcher, calendar, Gutenberg library client (King,
n.d.-b).

- **Licence:** GPL v3, with source on GitHub (King, n.d.-a).
- **Status:** alive but slow-moving; the project blog remains active
  into late 2024, but the browser's rendering engine is the critical
  caveat: WebbIE is built on the WinForms Internet Explorer view,
  rendering pages in IE7 compatibility mode (King, n.d.-a). Internet
  Explorer is retired, and the modern JavaScript-heavy web increasingly
  fails in it. King's own design notes candidly document abandoning the
  visual web view and features that the aging engine could no longer
  support.

WebbIE's philosophy — text-first, speed-of-access over fidelity,
respect for the user's existing screen reader and system settings —
remains exactly right. Its implementation base has simply outlived the
platform it was built on. For UC purposes it is best treated as a
design reference and a piece of living history rather than a current
recommendation, except for the narrow case of very old Windows
machines where nothing modern runs — a case that genuinely exists among
under-resourced users.

## Universal Cake relevance

- **Accessibility** — NVDA is the strongest single answer to "can
  libre software serve disabled users at world scale": first-party
  quality, born of lived experience (both founders are blind), and the
  most-used screen reader on Earth.
- **Cost** — the category exists to zero out a four-figure toll on
  computer access. NVDA's donation-funded model, with paid training and
  support as revenue rather than the software itself, is a UC-positive
  financial mechanism worth a dedicated case study alongside the
  existing open source series.
- **Sovereignty** — NVDA is portable, self-contained, offline-capable,
  and telemetry is opt-in and user-visible; WebbIE is GPL and local;
  ChromeVox's migration into ChromeOS ties it to Google's platform.
- **Sustainability** — one thriving project (NVDA), one absorbed
  (ChromeVox), one niche dissolved (Firefox add-ons), one survivor on a
  retired engine (WebbIE): a compact lesson in where volunteer and
  non-profit energy consolidates.
- **Project health** — NV Access demonstrates the full apparatus of a
  healthy project: governance documents, roadmap, code of conduct,
  regular releases, community channels. WebbIE demonstrates the
  single-maintainer pattern: decades of generosity, one bus factor.
- **Wellbeing and ethics** — screen readers built by blind developers
  for blind users are the clearest possible instance of edge-community
  design: nothing about us without us, expressed as software.

The consolidated recommendation for a current resource page: NVDA on
Windows, the built-in screen readers everywhere (see companion
document), Firefox or Chrome as the browser underneath, and retire the
talking-browser category with honour.

## References

Chromium Project. (n.d.). *ChromeVox (for developers)*. Chromium
documentation.
https://chromium.googlesource.com/chromium/src/+/lkgr/docs/accessibility/os/chromevox.md

crx4chrome. (n.d.). *ChromeVox Classic Extension*.
https://www.crx4chrome.com/extensions/kgejglhpjiefppelpmljglcjbhoiplfn/

Google. (2017, May). *The new, improved ChromeVox screen reader*.
Google Blog.
https://blog.google/products/chromebooks/new-improved-chromevox-screen-reader/

Google. (n.d.). *Use the built-in screen reader on your Chromebook*.
Chromebook Help. https://support.google.com/chromebook/answer/7031755

King, A. (n.d.-a). *WebbIE Web Browser* [Computer software]. GitHub.
https://github.com/AlasdairKing/WebbIEWebBrowser

King, A. (n.d.-b). *WebbIE, software for blind people with little or no
sight*. https://www.webbie.org.uk/

NV Access. (2026a). *NVDA releases*.
https://www.nvaccess.org/category/news/releases/

NV Access. (2026b). *Download NVDA*. https://www.nvaccess.org/download/

NV Access. (2026c). *NVDA 2026.1.1 user guide*.
https://download.nvaccess.org/releases/2026.1.1/documentation/userGuide.html

nvaccess. (2026). *NVDA* [Computer software]. GitHub.
https://github.com/nvaccess/nvda/

TechSpot. (2026). *NVDA download — 2026.1.1*.
https://www.techspot.com/downloads/7141-nv-access.html

Wikipedia. (2026). *NonVisual Desktop Access*.
https://en.wikipedia.org/wiki/NonVisual_Desktop_Access

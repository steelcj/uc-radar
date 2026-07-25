---
title: "Built-in screen readers: Orca, Narrator, and VoiceOver"
description: >
  Technical overview of the screen readers built into Linux, Windows,
  and macOS at no cost — Orca, Narrator, and VoiceOver — with current
  status, architecture notes, the wider built-in accessibility suites
  they belong to, and Universal Cake relevance.
creator: Christopher Steel
publisher: Vishpala
date: 2026-07-08
version: v0.1.0
language: en-CA
subject:
  - screen-readers
  - orca
  - narrator
  - voiceover
  - built-in-accessibility
  - assistive-technology
  - accessibility
  - universal-cake
type: technical-overview
rights: CC BY-SA 4.0
---

# Built-in screen readers: Orca, Narrator, and VoiceOver

Did you know? Every mainstream operating system now ships a screen
reader at no additional cost. This was not always true, and the change
is one of the quiet structural victories of the accessibility movement:
what was once a four-figure add-on purchase — a toll on the front door
of computing — is now a keystroke away on any machine, in any library,
school, or borrowed laptop. This document covers the three desktop
built-ins from the classic resource lists — Orca (Linux), Narrator
(Windows), and VoiceOver (Mac) — updates their status as of July 2026,
and places them in the wider built-in accessibility suites they belong
to. ChromeVox, now the built-in screen reader of ChromeOS, is covered
in the companion document on free and open source screen readers.

A built-in screen reader matters for reasons beyond price:

- it is present before anyone advocates for it — on the login screen,
  on a new machine, in an emergency when someone's vision changes
- it requires no administrative rights to invoke, which matters on
  institutional and public computers
- it normalises assistive technology as a property of the computer
  rather than a special accommodation bolted on for a special person

## Orca (Linux and other free desktops)

Orca is the free and open source screen reader of the GNOME project
and, in practice, of the free desktop as a whole — it is essentially
the only full screen reader Linux users have (LWN.net, 2025). It
provides speech and refreshable braille access to applications and
toolkits supporting AT-SPI, the assistive technology infrastructure of
free desktops, including GTK, Qt, Java Swing, LibreOffice, Firefox, and
Chromium (GNOME, n.d.-a; Orca Project, n.d.).

History and provenance:

- begun in May 2004 at Sun Microsystems by Marc Mulcahy, a blind
  programmer; first official release September 2006; default GNOME
  screen reader since GNOME 2.16 (Wikipedia, 2025)
- the name continues the aquatic-creature naming tradition of screen
  readers — JAWS, Flipper, Dolphin (Wikipedia, 2025)

Current status — and this is where the old resource lists most
undersell it:

- **Orca has just been substantially modernised.** Under the German
  government's Sovereign Tech Fund, Igalia's Joanmarie Diggs rewrote and
  modernised much of Orca between November 2023 and December 2024 —
  over 800 commits (GNOME, 2025).
- **The Wayland crisis was resolved.** The free desktop's migration
  from X.org to Wayland had broken fundamental screen reader
  capabilities — at its worst, users on GTK 4 Wayland sessions could
  not even stop speech or open Orca's settings, and developers feared
  visually impaired users would abandon Linux entirely. Coordinated
  work across Mutter, AT-SPI (the a11y-manager backend, released in
  AT-SPI 2.56), and Orca restored full keyboard monitoring on Wayland
  as of GNOME 48 in 2025 (LWN.net, 2025).
- **A next-generation stack is prototyped.** The Newton project — a
  Wayland-native accessibility protocol with AccessKit integration —
  points at the free desktop's post-AT-SPI future, with the side effect
  that GTK accessibility now also works on Windows and macOS (GNOME,
  2025).

Technical profile: Python core; speech via Speech Dispatcher to
synthesizers including eSpeak NG (the most common), Festival, and
RHVoice; braille via BrlAPI/BRLTTY; toggled in GNOME with Super+Alt+S
(GNOME, n.d.-b; LWN.net, 2025). Orca sits within a wider free-desktop
accessibility suite — magnification, high contrast, large text, and
on-screen keyboard are part of GNOME itself.

## Narrator (Windows)

Narrator has shipped with Windows since Windows 2000, but for most of
that history it was candidly a demonstration rather than a daily
driver — the tool that read the screen well enough to help you install
a real screen reader. That changed with Windows 10 and 11: Microsoft
rebuilt Narrator into a genuinely usable screen reader with sustained
investment (Microsoft, n.d.).

Current profile (from vendor documentation; Narrator is proprietary and
closed source):

- launched or stopped anywhere, including the sign-in screen, with
  Ctrl + Windows + Enter — no installation, no admin rights
- a dedicated Narrator key (Caps Lock or Insert), scan mode for
  document and web navigation by headings, links, and landmarks, and
  verbosity controls
- modern on-device natural voices in multiple languages, reducing the
  robotic-speech barrier that drove new users away
- braille display support, touch gestures on tablets, and integration
  with Microsoft's UI Automation accessibility API — the same API
  surface NVDA consumes
- a built-in interactive tutorial and complete online user guide
  (Microsoft, n.d.)

Honest assessment: Narrator is now fully adequate for core tasks and
emergencies, and improving steadily, but the Windows screen reader
ecosystem still treats NVDA and JAWS as the professional daily drivers;
WebAIM survey data consistently shows Narrator as a secondary rather
than primary tool for most respondents. Its structural importance is
availability: every Windows machine on Earth can speak its own login
screen.

## VoiceOver (Mac)

VoiceOver, introduced with Mac OS X 10.4 Tiger in 2005, is the
built-in screen reader that changed the industry's economics: Apple's
decision to bundle a serious, first-party screen reader with every
Mac — and from 2009, with every iPhone — established the expectation
that accessibility ships in the box, at no charge, maintained by the
platform vendor itself (Apple, n.d.).

Current profile (from vendor documentation; VoiceOver is proprietary
and deeply integrated):

- toggled with Command+F5 (or the Touch ID triple-press), available
  from first boot and during initial setup
- the rotor — a virtual dial for navigating by headings, links, form
  controls, and other element types — plus Trackpad Commander, which
  maps the entire screen to the trackpad surface for spatial
  exploration
- extensive braille display support, including automatic detection and
  braille input; over 60 voices across many languages
- a unified interaction model shared with iOS/iPadOS VoiceOver, so
  skills transfer across Apple devices
- an activity system for per-application settings, and AppleScript
  hooks for automation (Apple, n.d.)

VoiceOver's coupling to the platform is both its strength and its UC
caveat: it is superbly integrated, and entirely unavailable anywhere
else, unauditable, and evolves only at Apple's discretion. The user
guide link on legacy resource lists (a hosted OS X 10.8 guide) should
be updated to Apple's current support documentation.

## The wider built-in suites

Screen readers are the deepest of the built-in accessibility features,
but each platform now ships a full suite alongside them, and a resource
page should say so: screen magnification, cursor and pointer sizing,
high-contrast and colour-filter modes, captioning preferences, mono
audio, keyboard-only operation (sticky, slow, and filter keys), voice
control, and reduced-motion settings — the OS-level counterpart of the
`prefers-reduced-motion` media query that well-built websites honour.
On mobile, the same pattern holds: VoiceOver on iOS and TalkBack on
Android are built into every handset, which is significant because for
many people — disproportionately lower-income users — the phone is the
only computer.

Two practical implications for accessible web development flow from all
of this:

- **Testing has no licence barrier.** Every developer already owns at
  least one screen reader. There is no cost excuse for never having
  heard one's own website.
- **Built-ins are the floor, not the ceiling.** They guarantee that a
  machine can be used; they do not guarantee a person's preferred
  professional tool. Web content must be built to standards (semantic
  HTML, ARIA where needed) rather than to any one screen reader's
  quirks — the same discipline that makes a site work in NVDA makes it
  work in Narrator, Orca, and VoiceOver.

## Universal Cake relevance

- **Accessibility** — universal presence is the point: a screen reader
  on every machine, invocable from the login screen, is infrastructure
  in the fullest sense.
- **Cost** — all four platform built-ins are gratis; the toll booth is
  gone. What varies is everything else.
- **Sovereignty** — the pillar that splits this category cleanly. Orca
  is libre (LGPL), community governed, publicly funded — the Sovereign
  Tech Fund investment is a state recognising assistive technology as
  sovereign digital infrastructure, which is the UC sovereignty thesis
  stated as government policy. Narrator and VoiceOver are gratis but
  closed: unauditable, unforkable, and tied to their platforms'
  fortunes and priorities.
- **Sustainability** — built-ins are maintained by the platform's own
  release engine and cannot bit-rot independently of the OS; Orca's
  Wayland episode shows the risk profile of community-maintained
  infrastructure during platform transitions, and its resolution shows
  the recovery capacity.
- **Project health** — Orca's health is inspectable (public repository,
  named maintainer, funded modernisation, next-generation prototype);
  Narrator's and VoiceOver's health must be inferred from release notes
  and cannot be independently verified — health without transparency.
- **Wellbeing** — presence-before-need matters psychologically as well
  as practically: a person losing vision discovers the tool is already
  there, on their own machine, rather than facing a purchase, an
  installation, and an identity threshold all at once.
- **Ethics** — Orca's origin (a blind programmer at Sun) and NVDA's
  (two blind founders) keep the "nothing about us without us" thread
  visible; the proprietary built-ins deliver enormous good while
  concentrating control, a tension UC evaluation should state rather
  than resolve.

## References

Apple. (n.d.). *VoiceOver user guide for Mac*. Apple Support.
https://support.apple.com/guide/voiceover/welcome/mac

GNOME. (2025, April). *GNOME STF 2024 project report*. GNOME Blogs.
https://blogs.gnome.org/tbernard/2025/04/11/gnome-stf-2024/

GNOME. (n.d.-a). *Welcome to Orca*. GNOME Help.
https://help.gnome.org/users/orca/stable/

GNOME. (n.d.-b). *Orca* [Computer software]. GitHub.
https://github.com/GNOME/orca

LWN.net. (2025, June). *Enhancing screen-reader functionality in
modern GNOME*. https://lwn.net/Articles/1025127/

Microsoft. (n.d.). *Complete guide to Narrator*. Microsoft Support.
https://support.microsoft.com/en-us/windows/complete-guide-to-narrator-e4397a0d-ef4f-b386-d8ae-c172f109bdb1

Orca Project. (n.d.). *Orca — a free and open source screen reader*.
https://orca.gnome.org/

Wikipedia. (2025). *Orca (assistive technology)*.
https://en.wikipedia.org/wiki/Orca_(assistive_technology)

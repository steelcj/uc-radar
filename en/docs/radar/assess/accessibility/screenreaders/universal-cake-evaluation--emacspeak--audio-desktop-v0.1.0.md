---
dc:title: "Universal Cake Evaluation: Emacspeak, Audio Desktop"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "emacspeak"
  - "screen reader"
  - "terminal accessibility"
  - "assistive technology"
dc:description: "Assess-ring entry for Emacspeak, the speech interface and audio desktop for Emacs, as assistive technology in its own right and as an instrument for testing the accessibility floor of command-line tools and developer infrastructure."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-27"
dcterms:modified: "2026-10-27"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/tvraman/emacspeak"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, libre-screen-readers-talking-browsers, universal-cake-evaluation--opentelemetry--observability-framework"
dc:identifier: "universal-cake-evaluation--emacspeak--audio-desktop"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.6.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-27"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Opening entry at Christopher's request, recording why Emacspeak is
      on the radar and what the Assess pass needs to establish. Facts
      checked against the repository and public sources on 2026-10-27;
      nothing has been installed or tested yet.
uc-radar_evaluation:
  automated: []
  human: []
---

# Universal Cake Evaluation: Emacspeak, Audio Desktop

Version: 0.1.0
Status: Draft, Assess pending
Style Guide: style-guide--technical-documentation-for-technologists

## What it is

Emacspeak is a speech interface and audio desktop that runs inside Emacs, written principally by T. V. Raman and first released in 1995 <a id="cite-wp-emacspeak"></a>([Wikipedia, 2026](#ref-wp-emacspeak)). Rather than reading the screen, it speaks the meaning of what each Emacs mode presents, using short sounds for common events and speech that can be interrupted. Because Emacs runs shells, terminals, editors, mail and web browsing, Emacspeak gives a blind user direct access to command-line work at a low level. It was added to the Smithsonian's Permanent Research Collection on Information Technology at the National Museum of American History on 12 April 1999 <a id="cite-wp-emacspeak-2"></a>([Wikipedia, 2026](#ref-wp-emacspeak)).

## Why interesting

Two reasons, the second of which prompted this entry.

- **As assistive technology.** It is a long-standing free audio desktop for blind and low-vision people who work in technical fields, a group Vishpala's tools and documentation must serve.
- **As an instrument.** Evaluations of command-line tools and developer infrastructure, OpenTelemetry most recently, struggled with the accessibility-floor gate and fell back on "not applicable". That was wrong: blind developers do work in the terminal, and the floor applies to text interfaces too. Emacspeak, alongside Orca with a terminal emulator and console tools such as Speakup and BRLTTY, is a way to test whether a command-line tool's output, configuration and errors can actually be used without sight. Metrics 0.6.0 adds the questions such a test would answer.

## Concerns

Checked on 2026-10-27, not yet tested.

- **Maintenance.** The latest release is 60.0, May 2024. The last commit on the GitHub `master` branch is dated 25 August 2024 (verified). A March 2026 mailing-list thread asked whether development had moved elsewhere, reporting that the author had stepped away from the project <a id="cite-list-2026"></a>([Leonard, 2026](#ref-list-2026)). The list itself remains active. Longevity is the first question for the Assess pass.
- **Packaging lag.** Debian stable, testing and unstable all carry 53.0, maintained by the Debian Accessibility Team, with 60.0 noted as a newer upstream release <a id="cite-debian-emacspeak"></a>([Debian, n.d.](#ref-debian-emacspeak)).
- **Learning curve.** It is an Emacs environment, not a drop-in screen reader. For a person who does not already use Emacs the cost of entry is high. As an evaluation instrument that matters less than as a recommendation to clients.
- **Speech servers.** It depends on a separate speech server; the repository includes servers for eSpeak and several hardware and cloud synthesizers. Which free synthesizers work well on current Linux distributions needs checking.
- **Licence.** GPL-2.0-or-later, read from the source headers (verified). No concern for use as a tool.

## Evaluation

Not yet evaluated. The Assess pass should establish, at minimum:

- Whether 60.0, or the current `master`, installs and speaks on a current Linux distribution with a free speech server, tested on a disposable machine.
- Who maintains it now, and whether any fork or successor is active.
- How it compares, for testing command-line tools, with Orca and a terminal emulator, and with Speakup and BRLTTY at the console. A test procedure for the text-interface accessibility questions should not depend on a single tool.
- The gates and Security section, as for any entry.

## Relationship to project

Two destinations are possible. As assistive technology, it would join the screen reader material under `assess/accessibility/screenreaders/`. As an instrument, the result would feed a text-interface accessibility test procedure, which would go through the radar the same way as the telemetry trace procedure and, once adopted, live in `uc-processes` under `uc-radar/`.

## Status notes

- Last reviewed: 2026-10-27.
- In assess: opened at Christopher's request; no evaluation pass yet. Next step is the install-and-speak test, and a maintenance check.

## Links

- https://github.com/tvraman/emacspeak
- https://emacspeak.sourceforge.net/

## License

This document, *Universal Cake Evaluation: Emacspeak, Audio Desktop*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## References

<a id="ref-debian-emacspeak"></a>Debian. (n.d.). *emacspeak* [Package tracker]. Retrieved October 27, 2026, from https://tracker.debian.org/pkg/src:emacspeak
[Return to citation](#cite-debian-emacspeak)

<a id="ref-list-2026"></a>Leonard, I. (2026, March 8). *Development updates and slow speech* [Mailing list post]. Emacspeak discussion list. https://mail.emacspeak.net/sympa/arc/emacspeak/2026-03/msg00000.html
[Return to citation](#cite-list-2026)

<a id="ref-wp-emacspeak"></a>Wikipedia. (2026). *Emacspeak*. Retrieved October 27, 2026, from https://en.wikipedia.org/wiki/Emacspeak
[Return to citation](#cite-wp-emacspeak)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Opening entry: why Emacspeak is on the radar, facts checked on 2026-10-27, and what the Assess pass needs to establish |

---
title: "Fluid Infusion: a technical overview"
description: >
  Technical write-up of Fluid Infusion, the inclusive-design JavaScript
  framework developed by the Fluid Project at the Inclusive Design
  Research Centre, covering architecture, the Preferences Framework,
  component catalogue, build system, licensing, and project health,
  with Universal Cake relevance notes.
creator: Christopher Steel
publisher: Vishpala
date: 2026-07-08
version: v0.1.0
language: en-CA
subject:
  - fluid-project
  - infusion
  - inclusive-design
  - accessibility
  - javascript-framework
  - preferences-framework
  - universal-cake
type: technical-overview
rights: CC BY-SA 4.0
---

# Fluid Infusion: a technical overview

Infusion is a JavaScript application framework and component library
built by the Fluid Project, an international open-source community
anchored at the Inclusive Design Research Centre (IDRC) at OCAD
University in Toronto, under principal investigator Jutta Treviranus
(Fluid Project, n.d.). It was originally funded by the Andrew W. Mellon
Foundation with continued support from the William and Flora Hewlett
Foundation. Its distinguishing claim is that it is a framework designed
*from* inclusive design principles rather than one retrofitted with
accessibility features: the full range of human diversity — ability,
language, culture, age — is treated as the design centre, not the edge
case (Fluid Project, n.d.).

This overview is based on the project website, the GitHub repository,
and the project's published documentation as of July 2026. Where a claim
rests on general knowledge of the framework rather than a cited source,
it is flagged.

## Design philosophy

Infusion describes itself as "a different kind of JavaScript framework"
whose approach is to leave the integrator in control: your interface,
your markup, your way (fluid-project, 2025). In practice this means two
architectural commitments that separate it from mainstream frameworks:

- **Markup agnosticism.** Components do not own or generate opinionated
  DOM structures. Integrators supply their own HTML; components bind to
  it through configurable selectors. This is the inverse of the
  React/Vue model, where the component tree *is* the markup.
- **Radical configurability.** Nearly everything about a component —
  its subcomponents, event wiring, model state, DOM binding — is
  expressed as declarative JSON configuration ("options") that
  integrators can override at any depth without forking the component.

The second commitment reflects the framework's deepest idea: interfaces
should be *personalisable by the person using them*, not merely
customisable by the developer shipping them. The architecture and the
accessibility mission are the same mechanism.

## Architecture

### The IoC component system

The core of Infusion is an Inversion of Control (IoC) system in which
applications are expressed as trees of components defined by
`fluid.defaults()` grades — named, mergeable blocks of JSON
configuration. Key mechanisms (documented in detail at
docs.fluidproject.org; summarised here from general knowledge of the
framework):

- **Grades** act as composable configuration mixins; a component's
  behaviour is the merge of every grade it derives from, and integrators
  can inject or override configuration anywhere in the tree.
- **Model relay** provides declarative, bidirectional synchronisation
  between component models, with optional transformation functions —
  a data-binding system expressed in configuration rather than code.
- **Declarative events and listeners** wire component communication
  through the same JSON configuration, making the event graph
  inspectable and overridable.
- **The renderer** generates or binds interfaces from pure HTML
  templates on the client side, maintaining separation between model and
  view without a proprietary template language.

The practical consequence is that a downstream integrator can reshape a
third-party component's internals — swap a subcomponent, reroute an
event, re-bind a selector — through configuration alone. This is unusual
and is precisely what a preferences-driven, adaptive interface requires.

### Foundation layer

Infusion 4.x sits atop jQuery and jQuery UI, with supporting libraries
including Hypher (hyphenation), fast-xml-pull, and touch and scrolling
plugins (fluid-project, 2025). The jQuery dependency is a deliberate
compatibility choice with a long tail of institutional integrations, but
it is also the framework's most visible generational marker; custom
builds can exclude jQuery for host pages that already provide it.

## The Preferences Framework and UI Options

The Preferences Framework is Infusion's flagship subsystem: a reusable
set of APIs and UI building blocks for creating, persisting, and
integrating preference editors into web applications and content
management systems (Fluid Project, n.d.). Its best-known expression is
**User Interface Options (UIO)**, a drop-in panel that lets each visitor
transform presentation to their own needs (Fluid Project, n.d.),
typically including:

- text size and line spacing adjustment
- typeface substitution, including bundled Atkinson Hyperlegible and
  OpenDyslexic faces (fluid-project, 2025)
- high-contrast and inverted colour themes
- an auto-generated table of contents for long documents
- text-to-speech self-voicing through the Orator component
- enhanced link and input emphasis

Preferences persist across pages and sessions, and the framework
separates the preference model (what the person needs) from enactors
(how a given site honours it), so the same stored preferences can follow
a person across different Infusion-aware sites. This is the generalised,
standards-tracked version of the hand-rolled "accessibility panels" that
individual sites — transit agencies, governments, libraries — keep
rebuilding independently: an activate/deactivate animations toggle or a
150% text button is a bespoke two-option subset of what UIO provides as
configurable infrastructure.

## Component catalogue

Beyond the Preferences Framework, Infusion ships accessible components
addressing interaction patterns that are notoriously hard to make
accessible by hand (Fluid Project, n.d.; fluid-project, 2025):

- **Reorderer** — keyboard-operable drag-and-drop for lists, grids, and
  portal layouts
- **Orator** — self-voicing text-to-speech for page content
- **Inline Edit** — mode-free quick text editing that preserves context
- **Pager** — user-controlled pagination of long lists
- **Uploader** — multi-file upload with an accessible queue
- **Progress** — accessible linear progress indication
- **Keyboard accessibility plugin** — a jQuery plugin making arbitrary
  markup keyboard-navigable (roving tabindex, activation handlers)
  without per-widget boilerplate
- supporting components including tooltip, switch, sliding panel,
  textfield controls, table of contents, and undo

## Distribution, build, and quality infrastructure

- **Distribution**: npm (`infusion`), GitHub releases, or unpkg CDN;
  framework and components ship as concatenated, minified bundles with
  generated source maps (fluid-project, 2025).
- **Custom builds**: `npm run build:pkg:custom` with `--include` /
  `--exclude` flags produces packages containing only requested modules
  and their dependencies — for example a UIO-only bundle, or a build
  excluding jQuery (fluid-project, 2025). Module-level tree-shaking by
  declaration rather than static analysis.
- **Styling**: the Preferences Framework is authored in Sass with
  build and watch tasks; only Sass sources live in the repository
  (fluid-project, 2025).
- **Testing**: browser and Node test suites run through Testem with TAP
  output across Chrome and Firefox, with coverage reporting wired to
  Codecov; a Dockerfile supports containerised test and demo hosting
  (fluid-project, 2025).
- **Linting**: the project maintains its own aggregate linter,
  fluid-lint-all, covering JavaScript, JSON, and Markdown
  (fluid-project, 2025).

## Licensing

Infusion is distributed under a dual licence — the Educational Community
License 2.0 and the BSD 3-Clause licence, at the integrator's choice —
both permissive, both compatible with commercial and public-sector use.
(Stated from general knowledge of the project; the authoritative text is
`Infusion-LICENSE.txt` in the repository and should be verified before
formal reliance.)

## Project health

Observable signals from the repository as of July 2026 (fluid-project,
2025):

- latest release 4.8.0, published 15 January 2025, the thirty-eighth
  release in the project's history
- roughly ten thousand commits on the main branch, with continuous
  integration via GitHub Actions
- a modest public footprint — on the order of 140 stars and 98 forks —
  reflecting an institutional rather than hype-driven adoption pattern:
  Infusion's integrations live in universities, government platforms,
  and Fluid's own projects rather than in startup stacks
- governance through the Fluid community with a Contributor
  Covenant-based code of conduct and a named advocacy contact
- sustained institutional anchoring (IDRC/OCAD) and foundation funding
  history, which decouples survival from market adoption

Honest risks: the contributor core is small and institutionally
concentrated; the jQuery foundation dates the 4.x line and raises a
long-term modernisation question; and release cadence is measured in
years, not months. None of these is disqualifying for infrastructure
software — stability is a feature in assistive contexts — but they
belong in any adoption decision. The documentation notes a
next-generation framework effort within the Fluid community; its status
and timeline were not verified for this write-up and should be checked
against current project communications before betting on it.

## Universal Cake relevance

A brief pillar sketch, ahead of any formal evaluation:

- **Accessibility** — the framework's reason for existing, designed
  from inclusive-design theory (Treviranus's IDRC) rather than WCAG
  checkbox compliance; the Preferences Framework operationalises
  one-size-fits-one personalisation.
- **Cost** — gratis, permissively licensed, no tiers.
- **Sovereignty** — fully self-hostable, no telemetry, no cloud
  dependency; bundled fonts (including Atkinson Hyperlegible) ship in
  the package rather than loading from third-party font services.
- **Sustainability** — custom builds keep payloads proportional to use;
  the jQuery base adds legacy weight.
- **Project health** — alive, institutionally anchored, slow-moving,
  small core; the strongest and weakest pillar simultaneously.
- **Wellbeing** — UIO hands presentation control to the person using
  the interface, the same burden-inversion principle at framework scale.
- **Ethics** — Canadian public-interest research provenance, foundation
  funded, community governed, explicit inclusion commitments.

The deeper resonance is philosophical: Infusion is a two-decade
demonstration that designing for the edges produces architecture the
centre also benefits from — its radical configurability exists because
disabled users needed interfaces to adapt, and that same mechanism is
what makes the framework unusually flexible for everyone. It is a
working proof of the edge-community design advantage, implemented in
JavaScript.

## References

Fluid Project. (n.d.). *Infusion framework and components*.
https://fluidproject.org/infusion.html

Fluid Project. (n.d.). *Infusion documentation*.
https://docs.fluidproject.org/infusion/

fluid-project. (2025). *Infusion* [Computer software]. GitHub.
https://github.com/fluid-project/infusion

Inclusive Design Research Centre. (n.d.). *IDRC*. OCAD University.
https://idrc.ocadu.ca/

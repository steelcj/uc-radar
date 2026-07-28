---
dcterms:title: "Diagrams Readers Can Adjust: A Plain Language Guide"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:description: "A plain language guide to building diagrams that each reader can change: colour, size, typeface, language, movement, and layout."
dcterms:created: "2026-07-26"
dcterms:modified: "2026-07-26"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:identifier: "diagrams-readers-can-adjust--a-plain-language-guide"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.4.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-26"
    author: "Christopher Steel"
    notes: >
      Initial draft. Plain language companion to Accessible SVG:
      User-Adjustable Presentation on the Web. Same subject and same
      recommendations, written for a general audience at a Flesch-Kincaid
      grade level of 7 or below. Code examples reduced to one short working
      file. Citation count reduced to nine sources to keep the reading load
      low.
---

# Diagrams Readers Can Adjust: A Plain Language Guide

Version: 0.1.0
Status: Draft
Style Guide: style-guide--plain-language-for-general-audiences

## Abstract

Most diagrams on the web look the same for every reader. This guide explains how to build a diagram that readers can change to suit themselves. They can change the colours, the text size, the typeface, and the language. This is the plain language companion to *Accessible SVG: User-Adjustable Presentation on the Web*.

## Sources and acknowledgements

The rules for colour and contrast in this guide come from the Web Content Accessibility Guidelines, version 2.2 <a name="apa-wcag22-citation"></a>([World Wide Web Consortium, 2023](#apa-wcag22-reference)). The colour set comes from the Color Universal Design project <a name="apa-okabe-ito-citation"></a>([Okabe & Ito, 2008](#apa-okabe-ito-reference)). A group at the World Wide Web Consortium studied what readers need from a picture. Their list shaped this guide <a name="apa-svg-wiki-citation"></a>([World Wide Web Consortium, n.d.](#apa-svg-wiki-reference)). The settings toolbar follows the work of the Inclusive Design Research Centre <a name="apa-fluid-citation"></a>([Fluid Project, n.d.](#apa-fluid-reference)).

## What this guide is about

A diagram on a web page is usually a picture with fixed settings. The author picks the colours. The author picks the text size. The author picks the language. Every reader gets the same picture.

That works for some people. It fails for others.

One reader needs yellow text on a black background. Another needs much larger labels. A third reads Spanish, not English. A fourth is printing the page in black and white. The author cannot guess all of this in advance.

This guide shows a better way. The reader picks the settings. The picture follows.

## What readers need to change

Different people need different things from the same picture. Here are the common ones.

- Bigger text, so the labels can be read
- Stronger contrast between the text and the background
- A typeface that is easier to tell apart, letter by letter
- Colours that work for colour blindness
- Patterns instead of colour, for black and white printing
- The labels in their own language
- Less movement, or no movement at all
- A written version they can read or search

None of these is unusual. All of them are common.

## The kind of picture that can change

This guide is about SVG. SVG is a picture format made of shapes and text, not dots. You can read an SVG file as code. You can change it with code.

That is why SVG can adapt and a photo cannot. The text in an SVG is still real text. The colours are still separate values. Nothing has been flattened into a grid of pixels.

## The main idea

The picture and the page make a simple agreement.

The page offers a short list of settings. The picture reads that list and follows it. The picture does not know where the settings came from. The page does not need to know how the picture is drawn.

That gap between them is the point. It means the same picture works with a settings toolbar, with a simple menu, or with nothing at all. When there is no toolbar, the picture uses sensible defaults.

## Put the picture inside the page

There is one rule you must follow first. Put the SVG code directly inside the web page.

Many sites link to a picture file instead. That is easier, but it seals the picture off. A sealed picture cannot see the page settings. It cannot be changed by them.

So place the code in the page. Also keep the file able to stand on its own. Then it still looks right if someone opens it by itself.

## The list of settings

Here is a short list that covers most needs. Each setting has a name. The page sets a value. The picture reads it.

| Setting | What it changes |
|---------|-----------------|
| Background | The colour behind the picture |
| Ink | The colour of the lines and labels |
| Category colours | The fill colour of each part |
| Typeface | The font used for labels |
| Text size | How large the labels are |
| Line thickness | How heavy the outlines are |
| Pattern | Whether patterns show as well as colour |
| Language | Which language the labels use |
| Layout | How much room the picture takes |
| Movement | Whether anything moves |
| Detail | How long the written version is |

Keep the list short. A long list is hard to use and hard to test.

## Never use colour on its own

Some readers cannot tell certain colours apart. About one man in twelve has some form of colour blindness. For women the number is much lower <a name="apa-okabe-ito-citation-2"></a>([Okabe & Ito, 2008](#apa-okabe-ito-reference)).

Other readers see colour fine but print in black and white. The result is the same. The colours turn into similar greys.

So always add a second signal. Use a label. Use a pattern, such as dots or stripes. Use a different outline. Then the picture still works with no colour at all.

Start from a colour set that was tested for colour blindness. The Color Universal Design set is a good default <a name="apa-okabe-ito-citation-3"></a>([Okabe & Ito, 2008](#apa-okabe-ito-reference)).

## Make sure there is enough contrast

Contrast is the difference between two colours. Low contrast is hard to read.

Text needs a contrast of at least 4.5 to 1 against what is behind it. Any line or shape that carries meaning needs at least 3 to 1 <a name="apa-wcag-citation"></a>([World Wide Web Consortium Web Accessibility Initiative, n.d.](#apa-wcag-reference)).

Test every colour setting, not just the default one. A dark mode that fails is still a failure.

## Use real text, never traced letters

Some drawing tools turn labels into shapes. The letters still look like letters. They are not letters any more.

Traced letters cannot be read aloud by a screen reader. A screen reader is software that reads a page out loud. Traced letters also cannot be searched, copied, translated, or resized on their own.

So keep every label as real text. There is no good reason to trace it.

Pick a typeface built for hard reading conditions. Atkinson Hyperlegible is one good choice. It was made to keep letters clearly apart. The 2025 version covers more than 150 languages <a name="apa-atkinson-citation"></a>([Braille Institute of America, 2025](#apa-atkinson-reference)).

## The hard part: text does not wrap

Here is the one real problem in this whole guide.

In a web page, long text wraps onto the next line by itself. In an SVG, it does not. Each label is one line at one fixed spot.

So when a reader doubles the text size, the label grows sideways. It runs off its shape. It covers the label next to it.

There are three ways to handle this. Use them together.

Leave room. Give every label its own space, with a known limit. Test that space with the longest word in every language, at the largest text size.

Break the lines before you publish. A build tool can measure each label and split it into lines. The tool knows the exact width of every letter. The web browser will not do this for you.

Change the layout. Past a certain size, a diagram stops working as a diagram. Offer a simple stacked version instead. A reader who ends up there has not failed. They have found the version that suits them.

## Showing more than one language

You can keep every language inside one file. Then the drawing is shared. A fix to the shapes helps every language at once.

SVG has a built in way to pick a language. It is called `switch`. Avoid it.

It picks the language from the computer, not from the reader. It follows the order the author wrote, not the order the reader prefers. And if nothing matches, it can show nothing at all <a name="apa-mdn-citation"></a>([Mozilla, n.d.](#apa-mdn-reference)).

A toolbar exists so the reader can override the computer. A tool that only reads the computer cannot do that.

Do this instead. Write each language as its own group of labels. Mark each group with its language. Then one setting on the picture decides which group shows. Hide the others fully, so a screen reader does not read them too.

Keep the words themselves in a separate file, one per language. Translators then work on a plain list of words. They never open the drawing.

## Tell screen readers what the picture shows

Every picture needs a short title and a longer description. A screen reader reads these out.

Give each meaningful part of the picture its own title as well. Then the reader can move through the parts one at a time <a name="apa-graphics-citation"></a>([World Wide Web Consortium, 2018](#apa-graphics-reference)).

Order the code the way you want it read. The order on screen and the order in the code can differ. The screen reader follows the code.

Change the title and description when the language changes. Change them at the same moment as the labels. A picture in French with a description in English is a real bug. It is also an easy one to miss, because nothing looks wrong.

## Movement

Only add movement when it means something. Movement for decoration is not worth the cost.

Some readers get dizzy or distracted by motion. Their computer can say so. Read that setting, and let the reader change it too.

When motion is turned down, show the end state right away. Do not just play it faster. Nothing should flash.

## Always write the picture out in words

Every diagram needs a written version. It should carry the whole meaning, not a short summary.

Give the parts. Give how they connect. Give the point the reader is meant to take away.

Offer it to everyone, through a button on the picture. Do not hide it away for screen readers alone. Many people will use it. It is the part people copy, quote, and search.

## Where the settings come from

The Inclusive Design Research Centre built a settings toolbar for web pages. It lets readers change text size, contrast, and typeface across a whole site <a name="apa-fluid-citation-2"></a>([Fluid Project, n.d.](#apa-fluid-reference)).

Most of our settings can come straight from a toolbar like that. Three cannot, because they are specific to pictures. Those three are the language swap, the pattern switch, and the layout change. Each needs a small piece of code of its own.

Write that code so it works with any toolbar. It should also work with a plain menu, or with the computer's own settings.

## Let readers keep their settings

Settings that live on one site are settings people must set again everywhere else. That is a poor deal for the reader.

There is a standard for this. It lets a person store what they need once, in a form other sites can read <a name="apa-iso-citation"></a>([International Organization for Standardization & International Electrotechnical Commission, 2008](#apa-iso-reference)).

Name your settings to match that standard where you can. Also record inside each picture what it can change. Then software can tell a plain picture from one that adapts.

## Build the picture with a tool

By now the picture has many moving parts. Words in several languages. A colour set. Line breaks worked out per language and per size.

So do not hand-edit the final file. Keep the drawing, the words, and the colours as separate sources. Let a build tool put them together.

Then the tool can check the work every time. It can refuse to publish a picture with a missing translation. It can refuse a label that overflows. It can refuse a colour pair with too little contrast.

A checklist catches what a person remembers to look for. A build tool catches everything, every time.

## A short example

Here is a small, complete SVG file. It draws one labelled circle. The colours and the text size come from settings, with safe defaults if none are given.

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 200"
     class="uc-diagram" role="img" aria-labelledby="ex-title ex-desc"
     lang="en" xml:lang="en">
  <title id="ex-title">One idea</title>
  <desc id="ex-desc">A single blue circle with the label Mindset beneath it.</desc>
  <style>
    .uc-diagram {
      --uc-canvas: #ffffff;
      --uc-ink: #1a1a1a;
      --uc-cat-1: #0072b2;
      --uc-font: "Atkinson Hyperlegible Next", system-ui, sans-serif;
      --uc-label-size: 20px;
      --uc-stroke: 2.5;
    }
    .uc-canvas { fill: var(--uc-canvas); }
    .uc-shape { fill: var(--uc-cat-1); fill-opacity: 0.28;
                stroke: var(--uc-ink); stroke-width: var(--uc-stroke); }
    .uc-label { font-family: var(--uc-font); font-size: var(--uc-label-size);
                fill: var(--uc-ink); }
    @media (prefers-color-scheme: dark) {
      .uc-diagram { --uc-canvas: #101214; --uc-ink: #f2f2f2; }
    }
    @media (forced-colors: active) {
      .uc-canvas { fill: Canvas; }
      .uc-shape { fill: Canvas; stroke: CanvasText; }
      .uc-label { fill: CanvasText; }
    }
  </style>
  <rect class="uc-canvas" x="0" y="0" width="320" height="200" />
  <circle class="uc-shape" cx="160" cy="86" r="60" fill="#0072b2" />
  <text class="uc-label" x="160" y="182" text-anchor="middle">Mindset</text>
</svg>
```

Two things in that file are worth a note. The colour also sits on the circle as a plain value, as a backup for older tools. And the last block handles a screen mode where the computer replaces all colours, which is where diagrams often disappear.

## Why this matters

A fixed picture says the author's eyes matter more than the reader's. An adjustable picture gives that choice back to the reader. That is the heart of it.

There are practical gains too.

One file replaces many. Without this approach, you export a light version, a dark version, a large version, and one set for each language. That is dozens of files for one diagram. Every one of them must be redrawn when the diagram changes. Most of the time, nobody bothers, and the old versions go stale.

Smaller files help people on slow or costly connections. A drawing made of shapes is far smaller than a set of exported images.

Open settings help people keep control. Settings held in one company's account are a leash. Settings held in an open form travel with the reader.

## Has anyone done this before

The parts all exist. Nobody seems to have joined them up.

Wikimedia keeps many languages in one picture file, but the site picks the language, not the reader. Settings toolbars for page text are mature and in daily use. Standard ways to describe the parts of a picture exist. Tested colour sets exist.

A group at the World Wide Web Consortium wrote down these exact reader needs years ago <a name="apa-svg-wiki-citation-2"></a>([World Wide Web Consortium, n.d.](#apa-svg-wiki-reference)). What we have not found is one open, documented way to join a settings toolbar to a picture, including its language.

That may only mean we have not looked hard enough. Please treat it as a reason to search further, not as a claim to be first.

## How to check your work

Some checks a build tool can do for you.

- Every picture has a title and a description
- Every meaningful part has a title
- No label was turned into shapes
- Every label has a translation in every language
- No label overflows its space at the largest size
- Every colour pair meets its contrast target

Some checks need a person.

- The picture still makes sense in grey
- The picture still makes sense with patterns and no colour
- A screen reader reads it in the right order, in the right voice
- Turning down motion shows the end state, not a faster one
- The written version carries the full meaning on its own
- The picture still works on a narrow phone screen

## Things we still need to decide

- How many layout sizes are worth building and testing
- What happens when the page is saved as a PDF, since settings must be fixed at that moment
- Whether the written version should always be in the page, or fetched when asked for
- Whether to add a version made for touch, for readers who use raised print

## Choices we made writing this guide

We wrote this guide as a companion, not a replacement. The technical guide keeps the full detail. This one keeps the shape of the problem.

We chose to keep the section on text not wrapping, even though it is the hardest part. Leaving it out would make the job sound easier than it is.

We cut the number of sources from seventeen to nine. Long reference lists slow a reader down. The technical guide carries the rest.

We kept one code example. Removing all code made the guide vague. Keeping several made it heavy.

## Where this file lives

This file sits with the technical version, in the accessibility guides:

```bash
en/docs/guides/accessibility/diagrams-readers-can-adjust--a-plain-language-guide-v0-1-0.md
```

## License

This document, *Diagrams Readers Can Adjust: A Plain Language Guide*, by **Christopher Steel**, with AI assistance from **Claude Opus 5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Rules and standards

- [Web Content Accessibility Guidelines 2.2](#apa-wcag22-reference)
- [Understanding Contrast for Shapes and Lines](#apa-wcag-reference)
- [Ways to Describe the Parts of a Picture](#apa-graphics-reference)
- [Storing What a Reader Needs](#apa-iso-reference)

### Colour and type

- [Color Universal Design](#apa-okabe-ito-reference)
- [Atkinson Hyperlegible](#apa-atkinson-reference)

### Settings and language

- [Infusion User Interface Options](#apa-fluid-reference)
- [Picking a Language Inside a Picture](#apa-mdn-reference)
- [What Readers Need from a Picture](#apa-svg-wiki-reference)

## References

<a name="apa-atkinson-reference"></a>Braille Institute of America. (2025). *Atkinson Hyperlegible font*. https://www.brailleinstitute.org/freefont/
[Return to citation](#apa-atkinson-citation)

<a name="apa-fluid-reference"></a>Fluid Project. (n.d.). *User interface options API*. Infusion Documentation. Retrieved July 26, 2026, from https://docs.fluidproject.org/infusion/development/UserInterfaceOptionsAPI
[Return to citation](#apa-fluid-citation)

<a name="apa-iso-reference"></a>International Organization for Standardization & International Electrotechnical Commission. (2008). *Information technology, individualized adaptability and accessibility in e-learning, education and training, Part 2: "Access for all" personal needs and preferences for digital delivery* (ISO/IEC 24751-2:2008). https://www.iso.org/standard/43603.html
[Return to citation](#apa-iso-citation)

<a name="apa-mdn-reference"></a>Mozilla. (n.d.). *systemLanguage*. MDN Web Docs. Retrieved July 26, 2026, from https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Attribute/systemLanguage
[Return to citation](#apa-mdn-citation)

<a name="apa-okabe-ito-reference"></a>Okabe, M., & Ito, K. (2008). *Color universal design (CUD): How to make figures and presentations that are friendly to colorblind people*. https://jfly.uni-koeln.de/color/
[Return to citation](#apa-okabe-ito-citation)

<a name="apa-svg-wiki-reference"></a>World Wide Web Consortium. (n.d.). *SVG accessibility: People and issues* [Wiki]. Retrieved July 26, 2026, from https://www.w3.org/wiki/SVG_Accessibility/People_and_Issues
[Return to citation](#apa-svg-wiki-citation)

<a name="apa-graphics-reference"></a>World Wide Web Consortium. (2018). *WAI-ARIA graphics module* (W3C Recommendation). https://www.w3.org/TR/graphics-aria-1.0/
[Return to citation](#apa-graphics-citation)

<a name="apa-wcag22-reference"></a>World Wide Web Consortium. (2023). *Web content accessibility guidelines (WCAG) 2.2* (W3C Recommendation). https://www.w3.org/TR/WCAG22/
[Return to citation](#apa-wcag22-citation)

<a name="apa-wcag-reference"></a>World Wide Web Consortium Web Accessibility Initiative. (n.d.). *Understanding success criterion 1.4.11: Non-text contrast*. Retrieved July 26, 2026, from https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html
[Return to citation](#apa-wcag-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial draft; plain language companion to the technical guide, written for a general audience at grade 7 or below; one code example kept, source list reduced from seventeen to nine |

Yes — there are quite a few related open-source projects on GitHub and in the research community. They generally fall into three categories:

1. Unicode / hidden character cleaners
2. AI text “sanitizers”
3. AI image watermark removal research/tools

Here are some notable ones:

⸻

Text Cleaning / Invisible Unicode Removal

ai-text-sanitizer GitHub project

A JavaScript library focused on:

* removing zero-width Unicode characters,
* stripping hidden formatting,
* normalizing pasted AI-generated text,
* cleaning characters associated with AI detection heuristics.  

Useful if you want:

* browser-based cleanup,
* local/offline sanitization,
* integration into a website or editor.

⸻

reverse-captcha-eval GitHub project

A security-oriented project studying hidden Unicode prompt injection attacks against AI systems. It explores how invisible characters can manipulate AI behavior and how sanitization pipelines can mitigate that.  

This is more research/security focused than “watermark removal.”

⸻

AI Image Watermark Removal

remove-ai-watermarks GitHub project

Probably the most comprehensive modern open-source project in this space. It supports:

* C2PA metadata stripping,
* EXIF/XMP cleanup,
* SynthID removal research,
* visible Gemini watermark cleanup,
* diffusion-based regeneration attacks.  

The project is heavily research-oriented and references academic work on watermark robustness.

⸻

WatermarkAttacker GitHub project

Academic research code accompanying the paper:

Invisible Image Watermarks Are Provably Removable Using Generative AI

It demonstrates “regeneration attacks” using diffusion models to destroy invisible watermarks while preserving image quality.  

This is more of a research prototype than an end-user tool.

⸻

removebanana GitHub project

Focused specifically on removing some Gemini/Nano Banana watermark overlays and related markers. The project became popular on Reddit in early 2026.  

⸻

Related Cleanup Utilities

detextify GitHub project

Removes pseudo-text artifacts from AI-generated images:

* distorted AI lettering,
* fake typography,
* image text artifacts.  

Useful for image cleanup pipelines.

⸻

PanelCleaner GitHub project

An AI-assisted manga/image text cleanup tool:

* detects speech bubble text,
* masks regions,
* assists in restoration workflows.  

Not specifically about AI watermarks, but technically adjacent.

⸻

Important Context

There’s a major distinction between:

* legitimate sanitization
    * removing invisible Unicode,
    * fixing copy/paste corruption,
    * stripping metadata,
    * improving interoperability,

and

* attempting to bypass academic integrity systems or provenance systems.

A lot of these projects exist in a legally and ethically gray area, especially where they explicitly market themselves as “AI detector bypass” tools.

The more defensible and practical use cases are usually:

* privacy cleanup,
* metadata removal,
* text normalization,
* accessibility,
* security hardening against invisible Unicode attacks,
* preprocessing for NLP systems.

The security angle is increasingly important because invisible Unicode characters can be abused for:

* prompt injection,
* phishing,
* deceptive rendering,
* hidden instructions in AI pipelines.
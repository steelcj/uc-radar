---
dc:title: "Universal Cake Evaluation Metrics"
dcterms:version: "0.5.1"
dc:creator: "Christopher Steel"
dc:contributor: "Claude (Anthropic)"
dc:subject:
  - "evaluation"
  - "metrics"
  - "inclusive design"
  - "agency"
  - "wellbeing"
  - "power imbalances"
dc:description: "Evaluation metrics for products, services, and approaches. The goal is to support more people in better ways that support wellbeing, so that applications and services built with Universal Cake reflect these foundations in their product and design cycles."
dc:publisher: "UniversalCake"
dcterms:created: "2026-07-15"
dcterms:modified: "2026-08-21"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "sat-radar-entry-template"
dc:identifier: "universal-cake-evaluation-metrics"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.5.1"
    date: "2026-08-21"
    author: "Claude (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      This edit builds on 0.5.0 after Christopher's human review and
      acceptance of that version, including his own edits (renaming "SAT
      radar" to "radar" throughout, since the radar also covers products,
      services, and ideas outside SAT specifically; and adding the
      Trial-ring override paragraph allowing a named individual or group
      to proceed past a failed gate with recorded justification). Per
      Christopher's note that acceptance of 0.5.0 moves the document out
      of draft status, this version drops the "(draft)" title suffix and
      the draft-disclaimer paragraph that opened the document from 0.4.0
      onward; the document is now treated as an in-use, versioned
      reference rather than a pending proposal, though individual future
      edits, like this one, remain provisional until confirmed, per house
      convention that version bumping is Christopher's call. Rewrote the
      Training data representativeness bullet under Representation, which
      previously said "this is the same designer <--> designed-for
      question above," a phrase that reused the arrow notation from the
      subsection's opening line but pointed it at a different pair of
      values (training data versus design decisions) than the original
      (designer versus designed-for), making the cross-reference read as
      if it referred to a specific numbered question rather than a
      relationship. Replaced it with a plain-language explanation of the
      parallel instead of a second arrow-notation cross-reference.
      Replaced all six uses of the em dash with standard punctuation
      (colons, commas, or parentheses, chosen to match the document's
      existing term-then-comma-definition convention wherever that
      pattern applied) to comply with the house markdown rule in
      sat-docs-automation and the repositories that follow its defaults,
      which the document was not previously meeting.
  - version: "0.5.0"
    date: "2026-08-21"
    author: "Claude (Anthropic), drafted for Christopher Steel's review; human-reviewed and accepted by Christopher Steel, with his own edits"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Closed the algorithmic/AI known gap flagged in 0.4.0 by threading
      proxies through existing pillars rather than adding a dedicated
      one, matching how the 0.3.0 power-imbalance integration was done.
      Added to Representation: training data representativeness and
      disaggregated bias testing. Added to Sovereignty & Privacy: a new
      AI-specific proxies subsection covering training data consent,
      model transparency, and output provenance/disclosure. Added to
      Interaction Patterns: a transparent-personalization versus
      engagement-optimized-opacity table row, and a supplementary
      question on algorithmic decision recourse. Added to Security: a
      model supply chain bullet covering weight pinning/verification and
      adversarial/prompt-injection surface. Added to Market Position: a
      foundation model dependency proxy. Added to Environment: a
      training-time versus inference-time energy split under Direct
      impacts. Removed the 0.4.0 Known gaps note now that it is
      addressed, replacing it with a short pointer in Purpose to where
      the AI proxies live. Added a scorecard note clarifying the AI
      proxies are scored within their home rows rather than as a new row.
      Human edits on review: renamed "SAT radar" to "radar" throughout,
      since the radar also covers products, services, and ideas that are
      not directly SAT-related; added a Trial-ring override paragraph
      permitting a named individual or group of humans to proceed past a
      failed gate, with a recorded explanation of why, how it serves the
      project's goals, and, where useful, a SWOT analysis or similar
      record of the risk being accepted.
  - version: "0.4.0"
    date: "2026-08-21"
    author: "Claude (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation per
      house convention that version bumping is his call. Structural: added
      a pillar map to Purpose; merged Sustainability and Environment into
      a single top-level Environment pillar; added a References section
      with linked standards (ATAG, UAAG, VPAT, WCAG, CVSS, OWASP, GDPR
      Article 20, Data Transfer Project, Software Carbon Intensity, SPDX,
      OSI, FSF, AGPL), a Tooling subsection (LFX Insights, for Verified-grade
      Longevity and Market Position contributor-concentration evidence), and
      moved "Further scaffolding" into the section as Further reading. Added a Known gaps section flagging algorithmic/AI behavior
      as currently unscored. Relocated the "offers of assistance that
      exceed free limits" dark-pattern example from Quiet by default to
      Visible costs, where it fits as a disclosure/pricing pattern rather
      than a notification-cadence one. Renamed the Environment stakeholder
      lens to Planet to remove its name collision with the Environment
      pillar, and updated its usages accordingly. Added an explicit
      Aggregation subsection stating ratings are not summed into a single
      score. Added a Gates at a glance table. Added cross-references
      between: Sovereignty & Privacy's Exit cost proxy and the Exit and
      portability gate; Sovereignty & Privacy's telemetry question and
      Security's data-collection question; Sovereignty & Privacy's
      self-host/fork question and Market Position's Forkability proxy;
      and Environment's bandwidth/hardware questions and Accessibility's
      new hardware/connectivity bullet. Content: split Economic
      accessibility into Affordability and Hardware and connectivity
      accessibility; added Time-based media and Resizing and reflow to
      Accessibility; added an Authoring-side load question to Environment.
      Lifecycle: defined what "a light pass" requires at the Assess ring;
      added a security incident and an end-of-life/deprecation
      announcement to the re-review triggers. Scorecard: added a metadata
      block (product, version evaluated, evaluator, date, radar ring) and
      a note on adjusting row granularity per evaluation.
  - version: "0.3.1"
    date: "2026-07-17"
    author: "Christopher Steel"
    notes: >
      Merged the duplicate Data portability proxy into one bullet covering
      both machine-readable and human-readable export. Relocated three
      dark-pattern examples that were filed under the wrong row: the
      hit-a-limit/data-held-hostage case moved from Forgiveness to Leaves
      you whole, where the existing "hostage data" language already
      covers it; the undisclosed-limit half of that same case moved to
      Visible costs, since it is a disclosure-timing issue rather than a
      recoverability issue; the free-tier-export example (MEGA) moved
      from Plain asking to Leaves you whole as a named worked example.
      Kept the offers-of-assistance addition under Quiet by default and
      the vulnerability-disclosure sub-bullet under Security as originally
      placed; both fit their rows. Pluralized "system(s)" under
      Environment.
  - version: "0.3.0"
    date: "2026-07-17"
    author: "Christopher Steel"
    notes: >
      Integrated the technological-power-imbalances measurement proxies
      directly into the existing pillars rather than adding an eighth
      pillar, per that document's own recommendation. Added explicit,
      checklist-able proxies for the vendor <--> user relationship (exit cost,
      data portability, ToS volatility, pricing asymmetry) to Sovereignty
      & Privacy; added a new Representation subsection under Inclusive for
      the designer <--> designed-for relationship (representation distance,
      compensated vs. extractive research); added a new Market Position
      subsection under The Product or Service Itself for the
      platform <--> ecosystem relationship (market concentration, take rate,
      API stability, forkability, contributor concentration). Added the
      two cross-cutting measurement principles (information asymmetry is
      measurable; measure reversibility, not promises) and the
      reversibility one-question test as reusable guidance. Extended the
      scorecard template accordingly. VERSION NUMBER PROVISIONAL, pending
      Christopher's confirmation per house convention that version
      bumping is his call.
  - version: "0.2.0"
    date: "2026-07-15"
    author: "Christopher Steel"
    notes: >
      Major revision. Added Agency as an organizing category containing
      Sovereignty & Privacy (structural agency) and Interaction Patterns
      (supportive versus dark patterns). Added Security section. Added
      rating scale, evidence tags, gates, and stakeholder lenses. Broadened
      Accessibility with economic and cognitive questions. Added Exit and
      Portability questions. Added lifecycle guidance and scorecard
      template. Fixed typos, standardized question phrasing, applied
      project front matter, licence, and changelog conventions.
  - version: "0.1.0"
    date: ""
    author: "Christopher Steel"
    notes: "Initial incomplete draft."
---

# universal-cake-evaluation-metrics-v0.5.1

## Purpose

The general idea is to support more people through technology that promotes human agency and increases, rather than decreases, wellbeing in individuals, organizations and communities as well as the planet as a whole. We do this through metrics which reflect these values in measurable ways.

You can think of these metrics as a compass that helps keep us pointed in the general direction of a safer, more inclusive and green world.

Evaluating products, services, and approaches against these metrics allows Universal Cake to build applications and services with these foundations reflected in our product and design cycles.

The metrics ask two kinds of questions. Structural questions ask what a product is, its licence, its dependencies, where data rests, what happens when it disappears. Interaction questions ask what a product does to the person using it, moment to moment. A product can pass one and fail the other, so both are always evaluated.

A third thread runs through both: **power**. Every structural and interaction question has a "who holds the leverage here" version. Rather than scoring power as its own pillar, this version of the metrics threads explicit, checklist-able power-imbalance proxies through the existing pillars, at the three levels where they show up in practice: vendor <--> user (folded into Sovereignty & Privacy), designer <--> designed-for (folded into a new Representation subsection under Inclusive), and platform <--> ecosystem (folded into a new Market Position subsection under The Product or Service Itself).

*New in v0.5.0:* algorithmic and AI-driven behavior is treated the same way. Rather than a dedicated pillar, proxies are threaded through Representation (training-data representativeness and disaggregated bias testing), Sovereignty & Privacy (training-data consent, model transparency, output provenance), Interaction Patterns (algorithmic manipulation), Security (model supply chain), Market Position (foundation-model dependency), and Environment (training versus inference energy). This closes the gap flagged in v0.4.0's Known gaps note; see the v0.5.0 changelog entry for the full list of additions and their locations.

### At a glance

The document has five pillars, plus the cross-cutting machinery that applies to all of them:

- **Inclusive**: Accessibility, Representation (including algorithmic representation), Resilience
- **Agency**: Sovereignty & Privacy (including AI-specific proxies), Interaction Patterns (including algorithmic manipulation)
- **Environment**: direct, indirect, authoring-side, and training-versus-inference impact
- **Security**: data handling, disclosure, patching, supply chain (including model supply chain)
- **The Product or Service Itself**: Longevity, Content endurance, Exit and portability, Adjustability and support, Market Position (including foundation-model dependency)

Cross-cutting: the rating scale and evidence tags (below), gates that cannot be averaged away, stakeholder lenses, and the Lifecycle and Scorecard sections at the end.

## How to answer

### Rating scale

Answer each metric with one of the following ratings so that evaluations remain comparable across products and over time.

- **Strong**, the product actively advances this value
- **Moderate**, the product is adequate, with named limitations
- **Weak**, the product works against this value
- **Unknown**, insufficient information to rate, record what would resolve it

### Evidence tags

Tag every rating with how it is known.

- **Verified**, confirmed by direct inspection, testing, or measurement, record the method and date
- **Inferred**, a reasonable conclusion from documentation or architecture, not yet tested
- **Claimed**, asserted by the vendor or community, not independently checked

A rating of Strong with an evidence tag of Claimed is weaker than a rating of Moderate with a tag of Verified. Trial-gate evaluations (see Lifecycle) should carry Verified tags on all gate criteria.

### Aggregation

These ratings are not summed or averaged into a single score. A scorecard with mostly Strong ratings and one Weak is not "mostly good," it is a specific set of tradeoffs a reader should weigh directly, in the same spirit as the owner/user conflict example below, where divergent answers are recorded side by side rather than blended into one number. The scorecard exists to support deliberation, not to produce a pass/fail tally. Only a failed gate moves an item automatically, to the hold ring, as described next.

### Gates

Some criteria are gates, not scores. A failed gate cannot be averaged away by strength elsewhere, it moves the item to the hold ring on the radar until the named condition changes. Gate criteria are marked **[GATE]** throughout this document. The default gates are:

- Telemetry or content exposure that cannot be disabled
- A licence incompatible with the project
- Failure of the accessibility floor (content is unreachable for people using assistive technology)
- No exit, data or content cannot be extracted in usable form

Projects may add gates but should not remove these without recording why.

#### Gates at a glance

| Gate | Evaluated under |
| --- | --- |
| Telemetry or content exposure that cannot be disabled | Sovereignty & Privacy |
| A licence incompatible with the project | Checked once per project against that project's own licence policy. There is no single fixed question tagged inline for this one, since compatibility depends on the adopting project, not on any fixed property of the product |
| Failure of the accessibility floor | Accessibility, alternative methods of interacting with content |
| No exit, data or content cannot be extracted in usable form | Exit and portability, under The Product or Service Itself |

Search this document for `[GATE]` to find each tagged criterion inline. Projects that add their own gates (see Security, below, for one candidate) should add a row here too.

### Stakeholder lenses

Each question is asked from one or more perspectives. Where perspectives conflict, record the conflict rather than letting one answer hide the other.

- **Owner**, the person or organization deploying the product
- **User**, the people who interact with what is built
- **Community**, contributors, translators, the wider public affected
- **Planet**, energy, hardware, and material consequences

Example of a conflict worth surfacing: a product may increase the owner's sovereignty (self-hostable, permissive licence) while decreasing the user's (no data export, manipulative defaults).

Note: this lens was called "Environment" through v0.3.1. It is renamed here to Planet because the metrics also have a top-level Environment pillar (below) that means something different: the lens is "who is affected," the pillar is "what is measured," and sharing a name between them invited confusion in scorecard notes.

### Measuring power asymmetry

Two principles apply wherever a power-imbalance proxy appears below.

**Asymmetry of information is itself measurable.** Compare what each party can know about the other. A vendor with telemetry on a user's every click, facing a user who cannot even read the source code, scores badly regardless of stated intentions.

**Measure reversibility, not promises.** A vendor's stated values are unfalsifiable; the cost of leaving is not. Almost every power-imbalance proxy below reduces to one question, estimable per-tool during evaluation without needing access to the vendor's internals:

> If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?

Record that cost in hours or dollars wherever it can be estimated. It is the closest thing this framework has to a universal power-imbalance metric, and it is worth asking explicitly under Sovereignty & Privacy even when the more specific proxies below are also answered.

## Inclusive

### Accessibility

**Alternative methods of interacting with content.** Lens: user. Would this decrease or increase the number of people who can access content? Consider vision, hearing, motor, and speech differences, and whether the product supports assistive technologies rather than merely tolerating them. **[GATE]** if content becomes unreachable for people using assistive technology.

**Multilingual integration.** Lens: user, community. Is it available in languages the audience understands? Separately, is adding a language feasible for the community, is localization a data file anyone can supply, or a code change only maintainers can make?

**Affordability.** Lens: user. Is it free or affordable to use, relative to the population it is meant to serve?

**Hardware and connectivity accessibility.** Lens: user. Does it run on old or low-end hardware? Does it work on slow or intermittent connections? These same facts are also asked from an ecological-cost angle under Environment, below; record the raw figures once and rate them per lens, since a product can be cheap to run environmentally while still excluding someone on old hardware, or the reverse.

**Cognitive accessibility.** Lens: user. Is it learnable without training? Does it use plain language? Is it forgiving of errors, and does it explain them without blame?

**Time-based media.** Lens: user. Does audio or video content ship with captions, transcripts, or audio description? Is this treated as a default expectation, or as an afterthought add-on?

**Resizing and reflow.** Lens: user. Can text and layout be zoomed or resized, commonly to 200%, without loss of content or functionality? Does the interface reflow rather than requiring horizontal scrolling?

**Compatibility.** Lens: owner, user. Is it likely to be more or less compatible with:

- My hardware
- My operating system(s)
- My web browser(s)
- My input device(s)
- My output device(s)

### Representation

*New in v0.3.0, from the designer <--> designed-for imbalance.* Lens: community, user. The people making design decisions are rarely the people most affected by them; a team that is nondisabled, high-bandwidth, and native-English-speaking builds defaults that quietly encode its own circumstances as "normal," and the effects surface as exclusion presented as neutrality.

- **Representation distance.** Is there disabled representation on the design/engineering team or its advisory structure? Is user research from affected communities compensated and ongoing, or extractive and one-time?
- **Reading level and language coverage.** Do the artifacts themselves (not just the marketing) reveal whose life the designers imagined? Cross-reference against reading level, language coverage, device and bandwidth assumptions, and error-recovery paths already surfaced under Accessibility above, this question asks *why* those patterns exist, not just whether they do.
- **Training data representativeness.** *New in v0.5.0.* Where the product uses machine-learned models: whose voices, language varieties, dialects, and life circumstances are present in the training or reference data, and whose are absent? Is this disclosed anywhere, or only inferable from how the product behaves? Training data plays the same role here that the design team plays in the two bullets above: whichever population is well represented in it becomes the implicit default the model treats as normal, and whoever is absent from it is the population most likely to be poorly served.
- **Disaggregated bias testing.** *New in v0.5.0.* Has the vendor tested and published outcome differences, such as error rates, refusal rates, or output quality, across the groups likely to be affected, or is fairness only asserted rather than measured?
- Where representation cannot be assessed from public information, rate **Unknown** and record that its absence from public documentation is itself a data point.

### Resilience

Lens: owner, user. Does it continue to work when:

- It goes out of date, is the built artifact self-contained, or does it depend on live services, licence servers, or expiring tokens?
- During power outages, does it add infrastructure that can fail beyond what already serves the content?
- During network outages, does it function offline or degrade gracefully once loaded?

## Agency

Agency is the organizing question of this section: does the product help you achieve your goals, or does it distract you from them? Every interaction can be tested with one question, **whose goal does this interaction serve, and would the user recognize it as their own?**

Agency operates at two altitudes. Sovereignty is agency at the structural level, who controls the software, the data, and the terms. Interaction patterns are agency at the interaction level, whether the interface bends attention and choices toward the user's goals or away from them. A product can be structurally sovereign yet interactionally manipulative, or calm and polite on the surface while resting on total vendor lock-in. Evaluate both.

### Sovereignty & Privacy

Lens: owner, user.

- Does this increase or decrease the tech owner's and the tech user's sovereignty? Where the two diverge, record both answers.
- Can the owner self-host, modify, fork, and redistribute without permission? (This is software-level forkability. See Market Position, below, for the related but distinct question of whether the surrounding ecosystem could survive its steward turning hostile.)
- Where does user data rest, on the user's machine, the owner's infrastructure, or a third party's? (See also Security, below, which asks essentially the same question from an attack-surface lens rather than a control lens; record the fact once, and rate it under both if the two lenses diverge.)
- Does it phone home, telemetry, analytics, update checks, licence server contact? Can each be disabled? **[GATE]** if telemetry or content exposure cannot be disabled.
- When the user and the product disagree, who wins, can the user override, configure, or leave?

**Power-imbalance proxies (vendor <--> user).** *New in v0.3.0.* Score these explicitly rather than folding them into the general questions above; they are the checklist-able core of the vendor <--> user imbalance.

- **Exit cost.** The hours or dollars needed to migrate data and workflows to an alternative. This is the reversibility test above, applied specifically. (See also the Exit and portability **[GATE]** under The Product or Service Itself, below: that item gates on whether any exit exists at all; this proxy scores how costly the exit is once it exists.)
- **Data portability.** Does a complete, documented export exist? Rate machine-readable (usable by another program without reverse-engineering) and human-readable (usable by a person with no tooling) separately, Yes/no plus format for each, a technically-complete export that only a developer can parse is not the same guarantee as one a person can open and read.
- **Terms-of-service volatility.** How often have terms changed unilaterally over roughly the last five years? Count changes, note materiality, record the method (e.g., diffing archived snapshots).
- **Pricing asymmetry.** Is pricing public and stable, or negotiated in the dark? Opacity itself signals who holds the leverage, independent of whether current prices are reasonable.

**AI-specific proxies.** *New in v0.5.0.* Score these wherever a product uses machine-learned models, generative AI, or algorithmic decision-making; they extend the vendor <--> user asymmetry into the specific case where the vendor's leverage comes from a model rather than only from code or data.

- **Training data consent.** Was the user's or community's content used to train or fine-tune a model without informed, revocable consent? Does an opt-out exist, and does it apply retroactively to models already trained on that content?
- **Model transparency.** Are weights, architecture, and training data sources open and inspectable, or fully opaque? A closed model is a stronger form of the "can the owner self-host, modify, fork, and redistribute" question above, since with a closed model even the owner may not be able to.
- **Output provenance and disclosure.** Is AI-generated or AI-modified content disclosed as such to the person receiving it? Can generated output be traced back to a specific model version for audit purposes?

### Interaction Patterns

Lens: user. Does it make use of supportive patterns or dark patterns? Avoidance of dark patterns alone rewards blandness, so each row pairs the harm to avoid with the care to look for. Rate the pair together.

| Supportive pattern, look for | Dark pattern, check against |
|------------------------------|------------------------------|
| Honest defaults, the pre-selected option serves the user | Preselection tricks, defaults serve the vendor |
| Easy exit, cancelling or deleting takes no more effort than starting | Roach motel, easy in, hard out |
| Forgiveness, undo exists, destructive actions are recoverable | Confirmshaming, punishment friction, blame |
| Natural stopping points, the interface has endings | Infinite scroll, autoplay chaining, engagement traps |
| Quiet by default, notifications are opt-in, batched, user-scheduled | Attention farming, manufactured urgency, streaks |
| Plain asking, consent says what will happen, no is as easy as yes | Consent walls, trick wording, nagging re-prompts |
| Visible costs, prices, limits, and data collection disclosed before commitment | Drip pricing, hidden telemetry, surprise charges, limits disclosed only after being hit rather than in advance, offers of assistance that exceed free limits |
| Leaves you whole, data, content, and settings exit with you in open formats | Lock-in, proprietary formats, hostage data (including data held behind a hit usage limit), free-tier accumulated data not freely downloadable (e.g. MEGA) |
| Transparent personalization, the user understands and can adjust why they are shown something | Engagement-optimized opacity, algorithmic ranking, recommendation, or generation tuned for engagement or conversion without disclosure or user control |

Supplementary questions:

- Does it help you accomplish the goal you arrived with, or does it manufacture new goals for you?
- Can you predict what an action will do before you take it?
- Does it respect a completed task as finished, or reopen your attention afterward?
- Can you stop, leave, or take breaks without penalty?
- If an algorithm made a decision that affected you, can you find out why, and contest it? *New in v0.5.0.*

A worked positive anchor: an accessibility bar that lets a visitor make a page readable, saves the choice locally, and gets out of the way serves exactly the goal the user arrived with, manufactures nothing, and re-engages no one.

## Environment

*Note: this pillar absorbed the "Sustainability" wrapper heading used through v0.3.1. That heading contained only this one subsection, so it was doing no work; if the framework later grows genuinely separate financial or organizational sustainability questions, Sustainability can return as a real umbrella at that point.*

**Direct impacts.** Lens: planet, owner, user. Does it use more or less energy:

- On my system(s)
- On the service provider's system(s)

*New in v0.5.0:* for products that train or fine-tune models, rate training-time energy (typically a large, one-time or periodic cost, often incurred off-site) separately from inference-time energy (smaller per use, but recurring with every interaction). The two have very different profiles, and conflating them under one number obscures which lever, model choice versus usage volume, actually reduces impact.

**Indirect impacts.** Lens: planet, community. Does it require new hardware or hardware upgrades:

- For the product's host?
- For me?
- For others?

Does it use more or less bandwidth, and is the payload served once and cached, or repeatedly? (The same hardware and connectivity facts are also asked under Inclusive > Accessibility > Hardware and connectivity accessibility, from the angle of whether a person is excluded rather than the aggregate ecological cost. Record the raw figures once, rate them per lens.)

**Authoring-side load.** Lens: owner, user (editor). Is the authoring or editing interface itself compute-heavy, for example a large JavaScript admin bundle that runs sluggishly on modest hardware? This is an accessibility-and-environment concern for the person doing the editing, not only for the person viewing the published output.

## Security

Lens: owner, user.

- What data does it collect, and where does that data rest? (The same question is asked under Sovereignty & Privacy from a control lens rather than an attack-surface lens; record the fact once and rate it under both if they diverge.)
- How are vulnerabilities reported, and is there a published security policy?
  - Is it advertised by the service or product creator, are users notified or kept in the dark?
- How quickly are security patches released, and is the project responsive to reports?
- What is the supply chain exposure, how many runtime dependencies, is third-party code vendored and pinned, do install scripts run arbitrary code?
- **Model supply chain.** *New in v0.5.0.* Where the product uses machine-learned models: are weights and checkpoints pinned, versioned, and verifiable (for example, signed or hashed), or can they change silently underneath the product? Is there a defined, tested surface for adversarial or prompt-injection input?
- Does adding it expand or shrink the attack surface of the system it joins?
- Assessment method, record how the above was tested, network monitor used, platform, version, date.

Several of the questions above are gate-shaped (an unpatched critical vulnerability with no vendor response, or install scripts that run unreviewable arbitrary code) but none are on the default gate list, since severity here is more contextual than the four defaults. Projects handling sensitive data may want to add one as a project-specific gate, per the Gates section above, and record it in the Gates at a glance table.

## The Product or Service Itself

Lens: owner, community.

**Longevity.** Is this product, service, or idea more or less supported over the longer term? Consider institutional backing, funding model, contributor pool, release cadence, and commit activity, and rate what is observed, not what is hoped.

**Content endurance.** Would it make content behind the interface more or less likely to endure over time? For example, if it was removed, phased out, or replaced with a similar product, how would that affect stored content and data archives? Is content authored inside the tool, or merely enhanced by it? Enhancement layers leave content whole when they go, containers take content with them.

**Exit and portability.** **[GATE]** if no exit exists. Can data and content be exported in open formats? Does it rely on open standards or proprietary ones? What is the realistic migration path to a successor? (See also the Exit cost and Data portability power-imbalance proxies under Sovereignty & Privacy, above, which score the same underlying facts from the vendor <--> user relationship: this item is the hard gate on whether an exit exists at all, those proxies grade how costly and how complete that exit is.)

**Adjustability and support.** If there is a problem or a change needed, is it likely or unlikely that I could get support adjusting the product or service in ways that make it better? Consider the licence (is the fork-and-fix path open), the readability of the code, the quality of documentation, and the reachability of maintainers.

### Market Position

*New in v0.3.0, from the platform <--> ecosystem imbalance.* Lens: community, owner. Once a product becomes infrastructure (an app store, a search engine, a payment rail, a dominant framework, a de facto reference implementation), imbalance operates at the market level. The platform can tax, demote, or clone anything built on top of it.

- **Market concentration.** Simple market-share figures, or HHI if the data supports it. What share of the relevant ecosystem does the dominant operator control?
- **Take rate.** What is the platform's cut of transactions or value flowing through it, if any?
- **API stability and deprecation history.** How often has the platform broken things that depend on it?
- **Forkability.** Could the project realistically survive its steward turning hostile? This is a function of licence, governance structure, and bus factor together, not licence alone. (Ecosystem-level forkability. See Sovereignty & Privacy, above, for the related but distinct software-level question of whether the owner can self-host, modify, and redistribute the code today.)
- **Contributor concentration.** What share of commits or merge authority sits with one company or entity? This is often the single most honest number available for this subsection, record it directly where commit history is public.
- **Foundation model dependency.** *New in v0.5.0.* Is the product's core functionality locked to a single vendor's proprietary model API with no viable swap path, or can the underlying model be substituted? A captive model dependency can be a stronger lock-in than the surrounding software's own licence would suggest, since the code can be open while the intelligence it calls out to is not.

## Using these metrics in the lifecycle

These metrics attach to the radar rings as follows.

- **Assess**, a light pass. At minimum, this means all [GATE] criteria plus the Security section; other scorecard rows may remain Unknown or carry Inferred and Claimed tags at this stage. Gates are checked on available evidence.
- **Trial**, the gate pass, all gate criteria and the Security section must carry Verified tags before an item moves to trial. Findings during trial update the evaluation. This can be overridden by an individual (human) or a group of humans who must explain why and include their full names, why and how this helps achieve our goals. For example a product using an outdated technology, may be the only accessible product of it's kind. This might include a SWOT analyses or other methods or recording any potential risks of using the product anyway, even though it does not pass a gate in another areas.
- **Adopt**, the evaluation is attached to the adoption record and migrates with the entry.
- **Re-review**, adopted items are re-evaluated on a schedule or on trigger events: a new major release, a licence change, a change of ownership or funding, the appearance of telemetry in a point release, a security incident or breach affecting the product, or an end-of-life or deprecation announcement. Products change, evaluations must too. A material shift in any Market Position proxy (e.g., a funding round, an acquisition, a governance change) is also a re-review trigger.

## Scorecard template

Copy this table into each evaluation and summarize one row per metric area. Record the following before filling in ratings, since the framework's own staleness and re-review triggers depend on knowing exactly what was evaluated and when:

- Product or service name:
- Version evaluated:
- Evaluator:
- Date:
- Radar ring (Assess / Trial / Adopt / Re-review):

| Metric area | Rating | Evidence | Notes |
|-------------|--------|----------|-------|
| Accessibility, alternative interaction | | | |
| Multilingual integration | | | |
| Economic and cognitive accessibility | | | |
| Representation | | | |
| Compatibility | | | |
| Resilience | | | |
| Agency, sovereignty & privacy | | | |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | | | |
| Agency, interaction patterns | | | |
| Environment, direct, indirect, and authoring-side | | | |
| Security | | | |
| Longevity | | | |
| Content endurance | | | |
| Exit and portability | | | |
| Adjustability and support | | | |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | | | |
| Gates | Pass / Fail (name any failed gate) | | |

Row granularity is a starting point, not a fixed contract. Split a row (for example, Representation into its three sub-questions) or merge one (for example, folding the two Sovereignty rows back into one for a light Assess-ring pass) when it better fits the evaluation at hand, and note the change in the Notes column so a later re-review can tell what was actually scored.

The v0.5.0 algorithmic and AI-behavior proxies are scored within their home rows above (Representation; Agency, sovereignty & privacy; Agency, interaction patterns; Security; Market position; Environment) rather than as a separate row, since they span several pillars rather than forming one. Split them into dedicated rows when an evaluation is AI-specific enough to warrant it.

## References

### Standards

#### Accessibility

##### ATAG

- [Authoring Tool Accessibility Guidelines (ATAG) Overview](https://www.w3.org/WAI/standards-guidelines/atag/)
- [Authoring Tool Accessibility Guidelines (ATAG) 2.0](https://www.w3.org/TR/ATAG20/)

##### UAAG

- [User Agent Accessibility Guidelines 1.0](https://www.w3.org/TR/UAAG10/)

##### VPAT

- [Voluntary Product Accessibility Template](https://en.wikipedia.org/wiki/Voluntary_Product_Accessibility_Template)
- [VPAT, IT Industry Council](https://www.itic.org/policy/accessibility/vpat)

##### WCAG

- [WCAG 2 Overview](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/)

#### Security

##### CVSS

- [Common Vulnerability Scoring System (CVSS)](https://www.first.org/cvss/)

##### OWASP

- [OWASP Top Ten](https://owasp.org/www-project-top-ten/)

#### Sovereignty and data portability

##### GDPR

- [Article 20 GDPR, Right to data portability](https://gdpr-info.eu/art-20-gdpr/)

##### Data portability tooling

- [Data Transfer Project](https://dtinit.org/docs/dtp-what-is-it)

#### Environment

##### Software Carbon Intensity

- [Software Carbon Intensity (SCI) Specification, Green Software Foundation](https://greensoftware.foundation/standards/sci/)

#### Licensing

##### SPDX

- [SPDX License List](https://spdx.org/licenses/)

##### OSI / FSF

- [The Open Source Definition, Open Source Initiative](https://opensource.org/osd)
- [The Free Software Definition, Free Software Foundation](https://www.gnu.org/philosophy/free-sw.html)

##### AGPL

- [GNU Affero General Public License v3.0](https://www.gnu.org/licenses/agpl-3.0.html)

### Tooling

Unlike the standards above, these are not specifications to conform to, they are instruments that can turn an Inferred or Claimed rating into a Verified one for specific proxies.

#### Project health and contributor metrics

- [LFX Insights, Linux Foundation](https://insights.linuxfoundation.org/): tracks contributor volume, commit activity, and release cadence across open source projects, with a linked security dashboard. Useful for Verified evidence under Longevity (contributor pool, release cadence, commit activity) and Market Position (contributor concentration), both of which this document otherwise expects to be estimated by hand.

### Further reading

For deeper background, if wanted: Zuboff on surveillance asymmetries, Costanza-Chock's *Design Justice* on the designer/designed-for gap, the economics literature on switching costs and multi-homing, and the FSF/OSI tradition treating software freedom explicitly as a power question. Contract-theory language ("asymmetric bargaining power," "contracts of adhesion") is also useful because it is legally established rather than rhetorical.

## License

This document, *Universal Cake Evaluation Metrics*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.5.1 | Provisional, pending confirmation | Rewrote the Training data representativeness bullet under Representation to remove a confusing reuse of the designer <--> designed-for arrow notation with mismatched values, replacing it with a plain-language explanation of the parallel; replaced all six uses of the em dash with standard punctuation (colons, commas, or parentheses) to comply with the house markdown rule in sat-docs-automation; dropped the "(draft)" title suffix and the draft-disclaimer paragraph, since Christopher's acceptance of 0.5.0 moves the document out of draft status |
| 0.5.0 | Accepted, with human edits | Closed the 0.4.0 algorithmic/AI known gap by threading proxies through Representation (training data representativeness, disaggregated bias testing), Sovereignty & Privacy (new AI-specific proxies: training data consent, model transparency, output provenance), Interaction Patterns (transparent personalization vs. engagement-optimized opacity, algorithmic-recourse question), Security (model supply chain), Market Position (foundation model dependency), and Environment (training vs. inference energy); removed the Known gaps note; added a scorecard note on where the new proxies are scored. Human edits on review: "SAT radar" renamed to "radar" throughout; added a Trial-ring override paragraph for named individuals or groups to proceed past a failed gate with recorded justification |
| 0.4.0 | Draft, version provisional | Added pillar map and Known gaps note to Purpose; merged Sustainability into a single Environment pillar; renamed the Environment stakeholder lens to Planet; added Aggregation subsection and Gates at a glance table; relocated the "offers of assistance" dark-pattern example to Visible costs; split Economic accessibility into Affordability and Hardware and connectivity accessibility; added Time-based media, Resizing and reflow, and Authoring-side load questions; added cross-references between overlapping questions (Sovereignty/Security data location, Sovereignty/Market Position forkability, Sovereignty/Exit-and-portability, Environment/Accessibility hardware); defined the Assess-ring "light pass"; added security-incident and end-of-life re-review triggers; added scorecard metadata block and a note on row granularity; added a References section with linked standards, a Tooling subsection (LFX Insights), and moved "Further scaffolding" into it |
| 0.3.1 | Draft | Merged duplicate Data portability proxy (machine- and human-readable rated separately); relocated three dark-pattern examples to their correct rows (hostage-on-limit and MEGA example to Leaves you whole, undisclosed-limit-timing to Visible costs); pluralized Environment "system(s)" |
| 0.3.0 | Draft, version provisional | Integrated technological-power-imbalances proxies into Sovereignty & Privacy, new Representation subsection, new Market Position subsection, added measuring-power-asymmetry guidance and reversibility test, extended scorecard |
| 0.2.0 | Draft | Added Agency category (Sovereignty & Privacy, Interaction Patterns), Security section, rating scale, evidence tags, gates, stakeholder lenses, broadened accessibility, exit and portability, lifecycle guidance, scorecard template, housekeeping fixes |
| 0.1.0 | Draft | Initial incomplete draft |

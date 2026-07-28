---
dcterms:title: "AT Protocol & Personal Data Servers -- Universal Cake Praxis Evaluation"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "evaluation"
  - "AT Protocol"
  - "atproto"
  - "Personal Data Server"
  - "decentralization"
  - "power imbalances"
dcterms:description: "A Universal Cake praxis evaluation of the AT Protocol and Personal Data Server (PDS) self-hosting model, scored against universal-cake-evaluation-metrics-v0.2.0 and enriched with the technological-power-imbalances measurement proxies."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-07-17"
dcterms:modified: "2026-07-17"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: ""
dcterms:relation: "universal-cake-evaluation-metrics-v0.2.0"
dcterms:identifier: "atproto--universal-cake-praxis-evaluation"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.2.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-17"
    author: "Christopher Steel"
    notes: >
      Initial evaluation. Scored against universal-cake-evaluation-metrics-v0.2.0
      in full, with the technological-power-imbalances proxies (exit cost,
      contributor concentration, ToS volatility, representation distance)
      folded into Sovereignty & Privacy and The Product or Service Itself
      rather than treated as a separate pillar, per that document's own
      recommendation. Evidence base is public documentation, GitHub
      repositories, independent self-hosting writeups, and funding
      reporting -- no direct technical testing was performed, so this
      evaluation is scoped to the Assess ring, not Trial.
---

# AT Protocol & Personal Data Servers -- Universal Cake Praxis Evaluation

## Scope and evidence base

This evaluation covers the **AT Protocol (atproto)** and **Personal Data Server (PDS)** self-hosting model as infrastructure -- not the Bluesky client application, whose AppView-layer accessibility (WCAG conformance, screen-reader behavior, etc.) would need its own evaluation and is out of scope here. Where a metric is genuinely about the app rather than the protocol, it is marked Unknown/Out of scope rather than guessed at.

All ratings below are **Inferred** or **Claimed** unless otherwise marked. No direct technical testing (network monitoring, PDS deployment, migration dry-run) was performed, so per the lifecycle guidance in the metrics document, this evaluation is scoped to the **Assess** ring, not Trial -- it should not be treated as satisfying Trial-gate verification requirements even where a gate below reads Pass.

## Gate check

| Gate | Status | Basis |
|---|---|---|
| Telemetry or content exposure that cannot be disabled | **Conditional pass** | Repo data on a PDS syncs to Relays by design once published -- that's the protocol's core publish mechanism, not incidental telemetry, and it's the affordance a public social network user opts into. **But limited-visibility/private posts are not yet a shipped feature, so there is currently no way to post *without* that public exposure even if a user wants it.** This is a real gap, just not the kind of silent, undisclosed exposure the gate is aimed at. |
| Licence incompatible with the project | **Pass** | Core atproto repositories are dual-licensed MIT / Apache-2.0, one of the most permissive combinations available, and compatible with AGPL-licensed downstream work. Documentation in `bsky-docs` is CC-BY. Note the trademark on "AT Protocol" / "atproto" remains with Bluesky PBC, licensed defensively rather than commercially -- worth tracking but not a code-licence gate issue. |
| Accessibility floor (unreachable via assistive technology) | **Unknown -- out of scope at this layer** | This is properly a client/AppView question. The PDS admin surface (OAuth login screen) is the only user-facing accessibility surface at the protocol layer, and no accessibility audit of it was located. |
| No exit -- data or content cannot be extracted in usable form | **Pass** | Account migration between PDS hosts is a first-class, documented workflow, with a maintained CLI tool (`goat`) for admin/migration functions and documented steps for moving a DID's PLC entries and DNS. This is the protocol's strongest structural claim and it holds up on paper. |

**Gate outcome: no outright failures**, but the "conditional pass" on exposure and the "unknown" on accessibility mean this should not be treated as a clean pass at Trial without direct verification.

## Inclusive

### Accessibility

| Metric | Rating | Evidence | Notes |
|---|---|---|---|
| Alternative methods of interacting with content | Unknown | N/A | Client/AppView-layer question, out of scope for the protocol itself. |
| Multilingual integration | Unknown | N/A | Lexicon schemas are language-agnostic; actual UI localization lives in clients. |
| Economic accessibility | Moderate | Inferred | The protocol and reference PDS are free and open source. Running your own PDS requires a server, a domain, and either technical skill or paying someone who has it -- a real cost floor that Bluesky-hosted accounts don't face. |
| Cognitive accessibility | Weak | Verified (documentation review) | Self-hosting docs assume DNS, Docker, DID/PLC, and "entryway" concepts as background knowledge. Multiple independent operators, writing after successfully standing up a PDS, describe needing hours of independent troubleshooting to complete documented steps -- one calling the documentation itself "problematic," another writing a full companion guide because the official docs weren't sufficient on their own. That's a knowledge-infrastructure gap, not a user-competence gap. |
| Compatibility | Unknown | N/A | Depends heavily on which client and OS a given user runs. |

### Resilience

| Question | Rating | Evidence | Notes |
|---|---|---|---|
| Self-contained vs. dependent on live services | Moderate | Inferred | A self-hosted PDS depends on the Relay network and, practically, on Bluesky's dominant AppView to be visible to most users. The protocol doesn't strictly require Bluesky's infrastructure, but as of early 2026 roughly 99% of accounts are hosted on it, so in practice most participants' resilience is Bluesky's resilience. |
| Power outage / infra failure | Moderate | Inferred | Self-hosting adds an operator-controlled point of failure (your own server) in exchange for removing a vendor-controlled one. Net effect depends entirely on the operator's own infrastructure discipline. |
| Network outage / offline degradation | Unknown | N/A | Client-layer question. |

## Agency

### Sovereignty & Privacy

| Question | Rating | Evidence | Notes |
|---|---|---|---|
| Owner vs. user sovereignty | **Diverges -- record both** | Inferred | This is the textbook conflict the metrics document asks us to surface rather than average away. An operator who self-hosts gains real sovereignty: they hold their own signing keys and can migrate or fork at will. A typical Bluesky-hosted user's keys are held custodially by Bluesky PDSes on their behalf -- a deliberate usability tradeoff (password reset by email, no lost-key catastrophe) that is honest about trading some sovereignty for accessibility, but it means the *default* experience for the overwhelming majority of users is closer to conventional platform custody than the "own your data" pitch suggests. |
| Self-host, modify, fork, redistribute without permission | Strong | Verified | Dual MIT/Apache-2.0 licensing and multiple independent PDS implementations (Tranquil PDS in Rust, Cocoon in Go) and relay implementations (Blacksky in Rust) demonstrate this is a real, exercised right, not just a licence on paper. |
| Where does data rest | Moderate | Inferred | User's choice in principle (own PDS, third-party PDS, or Bluesky's). In practice, ~99% of accounts rest on Bluesky PBC infrastructure. |
| Phone-home / telemetry, disableable | Unknown | N/A | Reference PDS server telemetry behavior was not directly inspected; would need a network-monitor pass to verify per the Security section's assessment-method field. |
| When user and product disagree, who wins | Weak | Verified | Bluesky's own moderation stack (labeling, account actions) governs the default experience, and it is centrally operated. One documented case: accounts belonging to people in Gaza, apparently fundraising to leave a conflict zone, were marked as spam or banned, with affected users' visible recourse being public documentation by third parties rather than a clear appeal path. Independent third-party moderation services ("labelers") exist as an overlay a user can subscribe to, but they layer on top of the base moderation decision rather than replacing it -- the account-standing decision itself still sits with whichever entity controls your PDS/App View, which for nearly everyone is Bluesky PBC. |

**Power-imbalance proxies (from `technological-power-imbalances`), vendor↔user pairing:**

- **Exit cost:** Low in principle, moderate in practice. A documented migration path and CLI tooling exist -- a genuine structural advantage over most social platforms, which offer no comparable path at all. But "documented" and "easy for a non-technical user" are different things; the friction reported by self-hosters suggests the realistic exit cost for someone without sysadmin experience is still measured in hours of troubleshooting, not clicks.
- **Data portability:** Strong. Machine-readable, signed repository export is the protocol's foundational design, not a bolted-on feature.
- **ToS volatility:** Unknown -- not checked. Would be resolved by comparing archived snapshots of Bluesky's Terms of Service over the past few years and logging the frequency and materiality of changes.
- **Pricing asymmetry:** Moderate. Bluesky operates a freemium model with public pricing for premium tiers, which is more transparent than opaque enterprise pricing -- but the company has raised roughly $123M across seed, Series A ($15M, led by Blockchain Capital), and Series B ($100M, led by Bain Capital Crypto) rounds, reaching an approximate $700M valuation in January 2025. VC-backed PBCs face real pressure toward eventual monetization or a liquidity event; current terms being reasonable today is not a guarantee about terms after that pressure resolves. Worth naming explicitly: several of Bluesky's investors are crypto-focused funds even though atproto has no blockchain component, which independent reporting has flagged as a notable tension given the company's public "not crypto" positioning.

### Interaction Patterns

| Pattern | Rating | Evidence | Notes |
|---|---|---|---|
| Honest defaults | Unknown | N/A | Client-layer question (feed algorithms, default follows). |
| Easy exit | Moderate | Inferred | Account deletion/migration is possible and documented; not verified for how quickly and completely it executes. |
| Leaves you whole (open-format data on exit) | Strong | Verified | This is close to the protocol's central design claim, and the evidence (open lexicon schemas, signed self-authenticating repos, working third-party implementations) supports it. |
| Manufactures new goals vs. serves arrived-with goal | Unknown | N/A | Client-layer, not protocol-layer. |

## Sustainability

### Environment

| Question | Rating | Evidence | Notes |
|---|---|---|---|
| Direct energy impact | Unknown | N/A | No public data located on PDS/Relay energy footprint per account. |
| Indirect impact (new hardware required) | Weak-Moderate | Inferred | Self-hosting a PDS requires standing up and maintaining a server that wouldn't otherwise exist for that purpose, which is a real marginal hardware/energy cost relative to using an existing centralized account. This is the standard decentralization-vs-efficiency tradeoff, not unique to atproto. |
| Bandwidth, cached vs. repeated | Unknown | N/A | Not assessed. |

## Security

| Question | Answer | Evidence |
|---|---|---|
| Data collected, where it rests | See Sovereignty above | -- |
| Vulnerability reporting | Documented | A published security contact (security@bsky.app) and SECURITY.md exist for the core repository. |
| Patch responsiveness | Unknown | Not independently tracked. |
| Supply chain exposure | Unknown | Not audited; multiple independent-language SDKs (Python, Swift, Dart, Rust, Go) exist outside Bluesky's control, which is good for resilience but widens the surface that would need auditing for a full picture. |
| Assessment method | N/A | This evaluation did not include direct technical testing; a Trial-gate evaluation would require one. |

## The Product or Service Itself

| Question | Rating | Evidence | Notes |
|---|---|---|---|
| Longevity | Moderate-Strong | Verified | Institutional backing is real and growing ($123M raised across three rounds, 41M+ users as of January 2026), which cuts two ways: well-funded enough to persist, but funded by parties who will eventually want a return, which is a different incentive than a nonprofit or foundation-governed steward. |
| Content endurance | Moderate | Inferred | Content lives in the repo format, which is designed to outlive any single host -- but a PDS going offline without a completed migration still means content loss for that user in the interim. |
| Exit and portability [GATE] | Pass | Verified | Covered above; this is the strongest area. |
| Adjustability and support (fork-and-fix path) | Moderate | Verified | Licence is genuinely open, and community PDS/relay/SDK implementations prove the fork path is real. But the core protocol repository's own contribution guidelines note the maintainers "prioritize high quality issues," may close issues without feedback, and require sign-off from the Bluesky team before implementing any lexicon change -- meaning the *namespace that matters most* (`app.bsky.*`) is fork-able in theory but centrally governed in the way that counts for interoperability in practice. |

**Power-imbalance proxies, platform↔ecosystem and designer↔designed-for pairings:**

- **Contributor concentration:** Inferred to be high. The core protocol repository is maintained by Bluesky PBC, contribution guidance signals a small central team gating merges (especially for lexicon changes), and while independent implementations exist around the edges (SDKs, alternate PDS/relay servers), the protocol's own evolution runs through one company. An exact commit-share number wasn't pulled here and would sharpen this from Inferred to Verified.
- **Market concentration:** High and directly measured -- the ~99% figure is the whole story. A federated protocol with one dominant operator is federated in capability, not in fact, yet.
- **Representation distance (designer↔designed-for):** Unknown. No information located on disabled representation in Bluesky PBC's design/engineering team or governance structure, nor on compensated, ongoing user research versus one-off/extractive research. This would need direct inquiry to resolve, and its absence from public documentation is itself a data point worth naming.
- **Reversibility, the one-question test:** *If this relationship turned hostile tomorrow, what would it cost the weaker party to walk away?* For a self-hoster: real but nontrivial effort (DNS, PLC updates, a working PDS already running elsewhere). For a typical Bluesky-hosted user: a documented path exists, but exercising it requires either technical skill or trusting a third party to do it for them -- meaningfully better than the industry default of "not possible at all," but not yet the low-friction reality the "no lock-in" framing implies.

## Scorecard summary

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Unknown | -- | Out of scope (client-layer) |
| Multilingual integration | Unknown | -- | Out of scope (client-layer) |
| Economic and cognitive accessibility | Weak-Moderate | Verified | Documentation is the binding constraint |
| Compatibility | Unknown | -- | Not assessed |
| Resilience | Moderate | Inferred | Contingent on relay/AppView network, ~99% concentrated |
| Agency, sovereignty & privacy | Diverges (Owner: Strong / User: Moderate) | Inferred/Verified | Surfaced, not averaged, per stakeholder-lens guidance |
| Agency, interaction patterns | Mixed / mostly Unknown | Inferred | Exit and portability are the strong pole |
| Environment, direct and indirect | Unknown / Weak-Moderate | Inferred | No hard data; self-hosting has a real marginal cost |
| Security | Unknown (process exists) | Claimed/Unknown | Reporting channel documented, patch cadence not tracked |
| Longevity | Moderate-Strong | Verified | Well-funded; VC incentive structure is a live variable |
| Content endurance | Moderate | Inferred | Format is durable; live migration is the risk window |
| Exit and portability | **Pass [GATE]** | Verified | Protocol's clearest strength |
| Adjustability and support | Moderate | Verified | Open licence, but core namespace is centrally gated |
| Gates | **Pass, with two flagged conditions** | -- | Content-exposure gate conditional on no private-post option; accessibility floor unassessed at this layer |

## Reading this evaluation

The pattern that falls out of the scorecard matches what the earlier conversational pass suggested, now with numbers and a paper trail attached to it: **the protocol's structural design genuinely earns its sovereignty and exit claims**, which is unusual and worth crediting. What drags the overall picture down isn't the architecture, it's everything UC would call the *praxis* layer sitting on top of that architecture -- documentation that gatekeeps the sovereignty benefit behind sysadmin literacy, a governance structure where the namespace that matters is centrally gated even though the code is open, and a moderation layer where "decentralized data" hasn't yet produced "decentralized accountability." Those are fixable without touching the protocol itself, which is exactly the kind of leverage point Meadows-style analysis looks for: the highest-leverage intervention here is probably documentation and governance investment, not further protocol elegance.

## License

This document, *AT Protocol & Personal Data Servers -- Universal Cake Praxis Evaluation*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial evaluation against universal-cake-evaluation-metrics-v0.2.0, enriched with technological-power-imbalances proxies. Assess-ring only; no direct technical testing performed. |

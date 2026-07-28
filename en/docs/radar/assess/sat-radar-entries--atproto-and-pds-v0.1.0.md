---
dcterms:title: "SAT Radar Entries -- AT Protocol and Personal Data Server Self-Hosting"
dcterms:version: "0.1.0"
dcterms:creator: "Christopher Steel"
dcterms:contributor: "Claude (Anthropic)"
dcterms:subject:
  - "SAT radar"
  - "AT Protocol"
  - "Personal Data Server"
  - "evaluation"
dcterms:description: "Short radar-entry stubs for the AT Protocol and PDS self-hosting evaluations, in a stub format designed to complement universal-cake-evaluation-metrics-v0.3.0."
dcterms:publisher: "UniversalCake"
dcterms:created: "2026-07-17"
dcterms:modified: "2026-07-17"
dcterms:type: "Text"
dcterms:format: "text/markdown"
dcterms:language: "en"
sat:language_bcp47: "en"
dcterms:source: ""
dcterms:relation: "universal-cake-evaluation-metrics-v0.3.0"
dcterms:identifier: "sat-radar-entries--atproto-and-pds"
dcterms:rightsHolder: "Christopher Steel"
dcterms:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.3.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-07-17"
    author: "Christopher Steel"
    notes: >
      Initial stub format and first two filled entries, split from the
      combined atproto--universal-cake-praxis-evaluation-v0.1.0 per the
      owner/user sovereignty divergence surfaced in that evaluation.
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
---

# SAT Radar Entries -- AT Protocol and Personal Data Server Self-Hosting

## Stub format

A radar entry is a compressed pointer to a full evaluation, not a replacement for one. Every field below maps directly to a structure already in `universal-cake-evaluation-metrics-v0.3.0`, so a stub can be regenerated mechanically from a completed scorecard rather than authored freehand.

```
### <Title>

- **Ring:** assess / trial / adopt / hold
- **Full evaluation:** <link or filename>
- **Gate status:** Pass | Conditional (name the condition) | Fail (name the gate) -- [GATE] criteria only
- **Stakeholder divergence:** none, or <Lens A>: <rating> / <Lens B>: <rating>
- **Power-imbalance snapshot:**
  - Exit cost: <Low/Moderate/High, one line>
  - Concentration: <contributor and/or market concentration, one line>
  - Representation distance: <Known finding, or Unknown>
- **Evidence confidence:** <overall mix of Verified/Inferred/Claimed/Unknown, one line>
- **Re-review trigger:** <named condition that would move the ring>
- **Rationale:** <one line>
```

Notes on the format:

- **Ring** and **Gate status** come straight from the Gates section of the metrics doc. A gate that reads Fail routes to hold regardless of anything else in the stub, per the lifecycle guidance -- the stub should never show a ring more favorable than the gate status justifies.
- **Stakeholder divergence** exists because averaging across lenses hides exactly the conflicts the metrics doc asks evaluators to surface. If Owner and User (or Community) ratings differ meaningfully, both go in the stub, not just the one that reads better.
- **Power-imbalance snapshot** pulls the three v0.3.0 additions forward: exit cost from the vendor<-->user proxies under Sovereignty & Privacy, concentration from Market Position, representation distance from the new Inclusive subsection. Unknown is a valid, honest answer here and should stay Unknown rather than being quietly dropped.
- **Evidence confidence** is a compression of the evidence tags across the full scorecard -- it exists so a reader can tell at a glance whether a ring placement rests on testing or on documentation review, without opening the full evaluation.
- **Re-review trigger** should name an actual event, not a time period, wherever possible, consistent with the metrics doc's re-review guidance (release, licence change, ownership/funding change, telemetry appearing in a point release, or a material shift in a Market Position proxy).

---

## assess/

### AT Protocol

- **Ring:** assess
- **Full evaluation:** `atproto--universal-cake-praxis-evaluation-v0.1.0.md`
- **Gate status:** Conditional -- content-exposure gate conditional on the absence of a private/limited-visibility post option; accessibility floor Unknown (client-layer, out of scope at the protocol level)
- **Stakeholder divergence:** Owner/self-hoster: Strong sovereignty (own keys, real fork path) / typical Bluesky-hosted User: Moderate (signing keys held custodially by default)
- **Power-imbalance snapshot:**
  - Exit cost: Low in principle (documented migration tooling), Moderate in practice for non-technical users
  - Concentration: High on both axes -- ~99% of accounts hosted on Bluesky PBC infrastructure; core protocol repository and the `app.bsky.*` namespace centrally gated by the same entity despite a permissive licence
  - Representation distance: Unknown -- not found in public documentation
- **Evidence confidence:** Mixed -- licence, exit tooling, and market concentration are Verified; governance gatekeeping and moderation accountability are Inferred; security posture and ToS volatility are Unknown
- **Re-review trigger:** Private/limited-visibility posts ship (closes the exposure gate); a funding, ownership, or governance change (the current VC investor base is crypto-adjacent despite atproto having no blockchain component, which is a named incentive-pressure flag, not a current problem)
- **Rationale:** Structural exit and portability claims are real and well-evidenced; the gap between that and the lived system is governance and moderation concentration, not protocol design.

### PDS Self-Hosting

- **Ring:** assess
- **Full evaluation:** `atproto--universal-cake-praxis-evaluation-v0.1.0.md` (Sovereignty & Privacy, Cognitive Accessibility rows)
- **Gate status:** Pass on exit/portability (Verified via documented migration path and `goat` CLI); telemetry gate Unknown, not directly inspected; accessibility floor N/A beyond the OAuth admin surface
- **Stakeholder divergence:** none distinct from the AT Protocol entry above -- this stub *is* the Owner side of that divergence, made specific
- **Power-imbalance snapshot:**
  - Exit cost: Moderate -- bounded by documentation quality rather than by protocol design; multiple independent operators report needing independent troubleshooting to complete documented steps
  - Concentration: Reference PDS implementation is Bluesky-maintained; independent alternatives exist (Tranquil PDS, Cocoon), which meaningfully reduces single-implementation risk relative to the protocol-level governance concentration above
  - Representation distance: Unknown
- **Evidence confidence:** Weak-Moderate -- drawn from independent operator writeups (secondary evidence), no direct deployment or migration test performed
- **Re-review trigger:** Official self-hosting documentation receives a substantive rewrite; or a migration dry-run is actually performed, which would upgrade this entry's evidence tags toward Verified and make it a live candidate for trial
- **Rationale:** The sovereignty gain is real and worth the effort for someone with the skill or budget to clear the documentation gap; the practice is currently gated by a fixable knowledge-infrastructure problem rather than an architectural one.

## License

This document, *SAT Radar Entries -- AT Protocol and Personal Data Server Self-Hosting*, by **Christopher Steel**, with AI assistance from **Claude (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial stub format plus AT Protocol and PDS Self-Hosting entries, split from the combined evaluation |

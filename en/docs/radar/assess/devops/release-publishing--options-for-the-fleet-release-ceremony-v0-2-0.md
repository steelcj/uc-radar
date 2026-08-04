---
dc:title: "Release Publishing, Options for the Fleet Release Ceremony"
dcterms:version: "0.2.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Fable 5 (Anthropic)"
dc:subject:
  - "radar"
  - "devops"
  - "release-publishing"
  - "gh"
  - "file-fairy"
dc:description: "Comparative evaluation of the three forms the fleet's release-publishing ceremony could take: hand-run gh commands, a shared publish-release.py distributed by file-fairy, or a standalone osat-fluent-gh-tool. Evaluates the differential against the osat-fluent-gh-tool radar entry's baseline and recommends the shared-script form with named conditions that would flip the decision."
dc:publisher: "UniversalCake"
dcterms:created: "2026-08-02"
dcterms:modified: "2026-08-02"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "osat-fluent-gh-tool--release-publishing-cli, decision--gh-cli-for-release-asset-publishing, fairy--file-sync-cli, universal-cake-evaluation-metrics"
dc:identifier: "release-publishing--options-for-the-fleet-release-ceremony"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/devops/"
sat:version_at_creation: "0.3.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.2.0"
    date: "2026-08-02"
    author: "Christopher Steel, Claude Fable 5 (Anthropic)"
    notes: "Resolved. The human decision adopted Form B, recorded in decision--publish-release-shared-script-with-provider-interface. publish-release.py implemented with a provider seam (gh and dir backends), 20-check offline suite passing, and the first real publish, sat-doc-automa v0.1.4, completed on 2026-08-02 with both security gates Verified. Standard citation updated to 0.4.0."
  - version: "0.1.0"
    date: "2026-08-02"
    author: "Christopher Steel, Claude Fable 5 (Anthropic)"
    notes: "Initial comparison. Resolves the fork named in the osat-fluent-gh-tool radar entry's status notes by evaluating all three forms and recommending the shared-script form, with flip conditions recorded."
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Fable 5 (Anthropic)"
      evaluator_version: "claude-fable-5"
      recommendation: "adopt"
      reasoning: >
        Recommendation applies to the shared publish-release.py form,
        distributed by file-fairy as the fourth member of the existing
        devops-scripts group. The fluent-tool form duplicates a solved
        problem: the fluent pattern's substance is third-party binary
        lifecycle management, and gh is acquired through official OS
        installers, leaving a fluent wrapper with the collection's name
        but not its need. The hand-run form fails the ceremony's
        load-bearing requirement, byte-stable artifacts for self-update
        verification. Flip conditions to the fluent-tool form are named
        in the entry. Human decision pending.
      risk: "low"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.3.1"
      evaluation_datetime: "2026-08-02"
  human: []
---

# release-publishing--options-for-the-fleet-release-ceremony

## Description

A comparative radar entry resolving the fork the `osat-fluent-gh-tool--release-publishing-cli` entry deliberately left open: which form the fleet's release-publishing ceremony should take. Three candidate forms are evaluated. The shared Universal Cake evaluation of the *capability*, publishing a Release with a byte-stable tarball, `SHA256SUMS`, and optional GPG signature via `gh`, lives in that baseline entry and is not restated here; this entry evaluates only where the three forms *differ*.

## The three forms

**Form A, hand-run ceremony.** The `gh` commands are documented in the commit-and-versioning workflow and a maintainer runs them by hand at each release. No new code anywhere.

**Form B, shared script.** A single `publish-release.py` lives in sat-doc-automa beside `bump-version.py`, `cut-release.py`, and `check-conformance.py`, and is distributed to each repository by file-fairy as the fourth member of the existing devops-scripts manifest group. Each repository runs its own current copy; the fairy's checksum state detects drift.

**Form C, standalone tool.** A new osat-fluent-gh-tool repository in the collection, installed per machine, carrying its own VERSION, CHANGELOG, releases, and eventually self-update. The form the baseline radar entry describes.

## The decisive observation

The osat-fluent pattern earns its complexity from third-party *binary lifecycle management*: osat-fluent-restic-tool exists to install versioned, self-contained restic binaries verified against upstream checksums, with provenance files, generated wrappers in three syntaxes, offline archive, and rollback, because restic is an upstream binary whose acquisition, verification, and version pinning are genuinely hard problems. None of that applies here. `gh` is acquired through official installers on all three platforms, a fact the gh-cli decision record establishes and relies on, and `publish-release.py` itself would be a single pure-Python script, exactly the artifact class the devops-scripts group already distributes successfully. A fluent tool wrapping `gh` would carry the collection's name without the collection's problem.

The bootstrap asymmetry points the same way. Form C must publish its own releases, so its first release is hand-run by construction, and the tool that standardizes the ceremony is itself the repository most awkwardly served by it. Form B has no such knot: sat-doc-automa publishes its releases with its own local copy of the script, the same way it already bumps and cuts them.

## Where the forms differ

Ratings and evidence tags follow metrics v0.3.1; the baseline column summarizes the `osat-fluent-gh-tool` entry. Rows not listed carry identically across all three forms (sovereignty and platform inheritance, environment, content endurance, security gates, market position).

| Differential axis | A, hand-run | B, shared script | C, fluent tool |
|-------------------|-------------|------------------|----------------|
| Ceremony correctness (byte-stable artifacts, no skipped steps) | Weak, Inferred. Hand ceremony is exactly where determinism and completeness fail; self-update verification downstream makes this load-bearing, so weakness here is close to disqualifying. | Strong, Inferred. Scripted, same code every repository. | Strong, Inferred. Same property, same reason. |
| Distribution and currency | Not applicable, nothing to distribute. | N synced copies; currency is `ff apply` discipline, drift visible in `ff status` via checksums. Proven road: three scripts already travel it. | One copy per machine; currency via reinstall or eventual self-update. New road that must be built. |
| Maintenance surface | None. | One file in an existing repository; tests can live beside it; changelog rides sat-doc-automa's. | A whole repository: VERSION, CHANGELOG, README, ROADMAP, releases, install story, eventual self-update. |
| Bootstrap | Not applicable. | None; the source repository uses its own copy. | Self-publishing knot; first release hand-run by construction. |
| Economic and cognitive accessibility | Weak, memorized sequence. | Strong; script is simply present after sync, no install step. | Moderate; per-machine install is one more thing to have done before releasing. |
| Longevity | Strong in a trivial sense, nothing to abandon. | Tied to sat-doc-automa's continuity, the fleet's most-tended repository. | A new single-purpose repository is the artifact class most often abandoned. |
| Fit with standard-repository-layout | No change. | Amends the skeleton table with one row; `publish-release.py` joins the release-managed trio. | Outside the skeleton; a machine dependency, not a repository property. |

## Concerns with the recommended form

Honest assessment of Form B, since the comparison should not read as free:

- Currency depends on the operator running `ff apply` per repository; a repository can release with a stale script. Mitigation is already designed: the fairy's `status` shows drift, and the planned diff output makes staleness visible. This is a real, accepted cost of the N-copies model at current fleet scale.
- The script must sync in `mirror` mode so a local hand-edit surfaces as a conflict rather than being silently kept or clobbered, consistent with the manifest-declared sync-policy decision.
- No dedicated issue tracker or release notes for the script itself; its history lives inside sat-doc-automa's changelog. Acceptable for a single-maintainer fleet, and a named flip condition below if that changes.
- The two Security gates from the baseline entry transfer whole: never touching tokens (delegate entirely to `gh` auth), and specified, tested tarball determinism. Form B makes them no easier, only cheaper to iterate on.

## Flip conditions

Recorded so a future reversal is a decision, not a drift. Form C becomes the right answer if any of these hold:

- The ceremony grows per-platform needs a script cannot carry cleanly, generated wrappers, Windows-specific paths, the problems the fluent pattern actually exists for.
- The fleet gains maintainers or repositories enough that per-repository sync discipline demonstrably fails, releases shipping with stale scripts despite `ff status` visibility.
- The script needs its own release cadence decoupled from sat-doc-automa's, or external users appear who need it without adopting the fleet's conventions.
- `gh` acquisition itself becomes a managed problem, pinned versions, verification beyond the OS package manager, at which point binary lifecycle management genuinely enters the picture.

Form A is not a flip target; it is the baseline the ceremony exists to retire, and it fails the correctness axis regardless of scale.

## Relationship to project

Dev toolchain tier, release-side. Form B slots `publish-release.py` into the existing devops-scripts group in all three sync manifests and adds one row to the standard-repository-layout skeleton table (release-managed repositories). The `osat-fluent-gh-tool--release-publishing-cli` entry remains in assess as the evaluated capability record; on adoption of Form B its status notes should record that the capability proceeds in script form and the standalone-tool form is parked with these flip conditions.

## Status notes

- Last reviewed: 2026-08-02
- Resolved, 2026-08-02: Form B adopted by human decision, recorded in decision--publish-release-shared-script-with-provider-interface (sat-doc-automa, decisions/devops/). publish-release.py is implemented beside its three siblings with a provider seam, gh and dir backends, and a 20-check offline suite; the first real publish, sat-doc-automa v0.1.4, ran the same day, carrying the token and determinism gates to Verified. The flip conditions live on in the decision record and the parked standalone entry.
- In assess (as originally written): the automated pass recommends Form B. What moves this comparison to resolved is a human decision recording Form B (or otherwise) in a decision record in sat-doc-automa, per the fleet's convention that decisions live as decision documents, followed by implementation: the script written, distributed via the manifests, and exercised against one real release with the two Security gates carrying Verified tags.
- In adopt: the decision record plus the script in sat-doc-automa's root beside its three siblings.
- In hold: not applicable; this entry resolves a fork rather than tracking an external item.

## Links

- `uc-radar/en/docs/radar/assess/devops/osat-fluent-gh-tool--release-publishing-cli-v0-1-0.md`, the baseline entry and shared evaluation
- `sat-doc-automa/en/docs/decisions/devops/decision--gh-cli-for-release-asset-publishing-v0-1-0.md`
- `sat-doc-automa/ROADMAP.md`, Pending: GPG-sign SHA256SUMS for published release assets
- `osat-fluent-restic-tool/README.md`, the fluent pattern's actual problem domain, for the decisive observation above
- `sat-doc-automa/en/docs/standards/standard-repository-layout-v0-4-0.md`, the skeleton table Form B amends

## License

This document, *Release Publishing, Options for the Fleet Release Ceremony*, by **Christopher Steel**, with AI assistance from **Claude Fable 5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.2.0 | Draft | Resolved: Form B adopted per the publish-release decision record; script implemented and first real publish completed with gates Verified. |
| 0.1.0 | Draft | Initial comparison of the three ceremony forms. Automated recommendation: the shared publish-release.py distributed by file-fairy, with four named flip conditions to the standalone-tool form. Human decision pending. |

---
dc:title: "Universal Cake Evaluation: Entire, Agent Session Capture and Git Hosting"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "entire"
  - "ai collaboration"
  - "provenance"
  - "git hosting"
  - "ai transparency"
dc:description: "Universal Cake evaluation of the products and services offered by Entire Inc. (entire.io): the open-source Entire CLI and its Checkpoints, the entire-graph code map, forgemark, and the hosted Git network, session storage and agentic search, as of 2026-10-21."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-21"
dcterms:modified: "2026-10-21"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://entire.io/"
dc:relation: "universal-cake-evaluation-metrics, recommendation--use-a-claude-md-file, universal-cake-evaluation--eu-ai-act-article-50--ai-transparency"
dc:identifier: "universal-cake-evaluation--entire--agent-session-capture-and-git-hosting"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:repository: "uc-radar"
sat:path: "en/docs/radar/assess/ai-collaboration/"
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-21"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of Entire's products and services
      against Universal Cake Evaluation Metrics 0.5.1, from the
      entire.io site, terms and privacy policy, the entireio/cli README
      and security policy, and the GitHub API. Nothing was installed
      or run.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # CLI: assess, with a private trial possible under conditions; hosted services: not for client or Universal Cake data until the named conditions change
      reasoning: "The Entire CLI is MIT-licensed and stores agent session transcripts as git refs inside the repository itself, which is a sovereign design that fits AI-transparency and provenance work. But it pushes those transcripts with every git push by default, so a public repository publishes every prompt and response, secret redaction is best-effort, anonymous usage analytics go to PostHog unless switched off, releases carry a checksum file without a signature, and it is pre-1.0 (0.12.0). The hosted services come from a Delaware company whose terms may change at any time, with no Canadian hosting region, no mention of PIPEDA, and no published pricing."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-21T14:00:00-04:00"
  human: []
---

# Universal Cake Evaluation: Entire, Agent Session Capture and Git Hosting

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

Entire is a developer platform for work done by people and AI coding agents together, from Entire Inc., founded by GitHub's former chief executive and launched in February 2026. It offers an open-source command-line tool that records each agent session and links it to the git commits it produced, plus a growing set of hosted services: a Git hosting network, session storage, and search across code, commits and sessions. This entry evaluates every product and service listed on entire.io on 2026-10-21. The CLI's design is good for sovereignty, because the records live in the repository's own git objects, not in a vendor's database. Its defaults are not: transcripts are pushed wherever the code is pushed, and usage analytics are on. The hosted services cannot yet be recommended for client or Universal Cake data. We recommend Assess for the CLI, with a private trial possible under conditions, and no hosted use until the conditions under Recommendation change.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Product facts come from the entire.io home page <a name="apa-entire-home-citation"></a>([Entire Inc., n.d.-a](#apa-entire-home-reference)), its terms of service <a name="apa-entire-terms-citation"></a>([Entire Inc., n.d.-c](#apa-entire-terms-reference)) and privacy policy, last updated 2026-07-06 <a name="apa-entire-privacy-citation"></a>([Entire Inc., n.d.-b](#apa-entire-privacy-reference)), the `entireio/cli` README and security policy <a name="apa-entire-readme-citation"></a>([Entire Inc., n.d.-d](#apa-entire-readme-reference)), and the GitHub API's description of the `entireio/cli` and `entireio/entire-graph` repositories and their latest releases, all read on 2026-10-21. Funding and launch details come from Entire's announcement and trade press, and are tagged Claimed. Nothing was installed or run. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

The subject is Entire's offering as listed on entire.io on 2026-10-21. The CLI is rated in most depth, since it is the only part that can be inspected. Hosted services are rated from their terms, privacy policy and public claims. The AI agents that Entire records, such as Claude Code, are out of scope.

## What Entire offers

| Product or service | What it is | Form | Licence or terms | Evidence |
|---|---|---|---|---|
| Entire CLI (`entire`), with Checkpoints | Hooks into git and supported agents; records each session's transcript, prompts, files touched and token use, and links it to commits through an `Entire-Checkpoint` trailer. Adds `entire blame`, `entire why`, session resume, search, recaps and multi-agent review | Local binary for Linux, macOS and Windows on amd64 and arm64; v0.12.0, released 2026-10-21 | MIT | Verified from the repository and release |
| Supported agents | `antigravity`, `claude-code`, `codex`, `copilot-cli`, `cursor`, `factoryai-droid`, `opencode`, `pi`, and external agents on `PATH` | | | Verified from the README |
| `git-remote-entire` | A git remote helper for cloning over `entire://` | Local binary, installed with the CLI | MIT | Verified from the README |
| entire-graph | A precomputed map of a repository for agents: ranked code search, definitions, callers, types, routes and change impact; runs locally | Local binary; only nightly releases so far, latest `v0.4.1-nightly`, 2026-09-03 | MIT | Verified from the repository |
| forgemark | An open-source git hosting benchmark, used for Entire's speed claims | Source repository | Not established | Claimed by the site |
| Git hosting | "The fastest Git network on Earth": hosted repositories, mirrored globally; the site claims agents clone and push up to 89 times faster than competitors, from its own benchmark | Hosted | Terms of service | Claimed |
| Multi-region hosting | Code resident in a chosen region: India, Australia, the EU and the US today; "decentralized everywhere" marked soon | Hosted | Terms of service | Claimed |
| Session storage | Sessions linked to commits, with secrets detected and redacted before storage | Hosted, and in the repository through the CLI | Terms of service | Claimed |
| Agentic Search | One query across repositories, commits and agent sessions | Hosted | Terms of service | Claimed |
| Rebellion | A mailing list and community: news, early access, releases | Email list | Not established | Claimed |

No pricing is published: `entire.io/pricing` returned 404 on 2026-10-21, and the home page's only call to action is a sign-in. Both `entire.io` and `entire.com` serve the site, and the README's install script uses `entire.com`.

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass, with conditions | Verified | Usage analytics go to PostHog by default and can be switched off with `entire configure --telemetry=false`. Transcripts are pushed with every `git push` by default; `--skip-push-sessions` or `strategy_options.push_sessions: false` stops that, and `--checkpoint-remote` sends them to a separate repository instead. Both can be disabled, so the gate passes, but both defaults expose content |
| A licence incompatible with the project | Pass, CLI and entire-graph | Verified | MIT. The hosted services are under terms of service, not an open licence |
| Failure of the accessibility floor | Pass, CLI | Inferred | A command-line tool; interactive pickers in some commands were not tested with a screen reader. The web interface was not assessed |
| No exit | Pass, CLI; Unknown, hosted | Verified, Inferred | Checkpoints are ordinary git refs under `refs/entire/checkpoints/`, readable with `git for-each-ref` and kept when Entire is removed. Hosted repositories are git; the terms give at least 30 days to export after termination, except termination for material breach. Export of hosted sessions and search data is not described |

## Scorecard

Ratings are for the CLI unless a row names the hosted services.

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Command line, with a machine-readable `entire agent-help`. Some commands open interactive pickers; non-interactive flags exist for setup. Not tested with assistive technology |
| Multilingual integration | Weak | Inferred | English only, as far as the repository and site show |
| Economic and cognitive accessibility | Moderate | Verified, Inferred | The CLI is free. Its model (sessions, checkpoints, refs, remotes, hooks) adds a layer to git that people must understand to keep their transcripts private. Hosted pricing is unpublished |
| Representation | Unknown | None | Not assessable from public information |
| Compatibility | Strong | Verified | Linux, macOS and Windows on amd64 and arm64; eight named agents; works beside existing git hook managers by chaining their hooks |
| Resilience | Strong | Inferred | Checkpoints are local git objects and work offline; nothing needs the hosted service. Summaries with `--generate` call a model provider |
| Agency, sovereignty and privacy | Moderate | Verified | Storing records in the repository is the right design. But pushing them by default sends full transcripts, which can quote client files, wherever the code goes, and the README itself warns that public repositories expose them. Redaction of secrets is best-effort, and PII redaction is opt-in |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry), hosted services | Weak | Verified | Entire Inc. is a Delaware company under Delaware law. The terms say Entire may modify them at any time, with continued use as acceptance. Content is licensed to Entire to operate, provide and improve the services. No general AI training without opt-in. No published pricing. No Canadian region, and the privacy policy does not mention Canada or PIPEDA |
| Agency, interaction patterns | Moderate | Verified | Opt-out defaults for telemetry and session pushing. Otherwise respectful of the user's work: no commits on the active branch, `commit_linking` asks by default, and leftover shadow branches are reported rather than silently deleted |
| Environment, direct, indirect, and authoring-side | Moderate | Inferred | The CLI is light. Its purpose is to keep and index every agent session, which grows repositories, and the hosted search and summaries run models. entire-graph claims up to 71% fewer agent tokens (claimed) |
| Security | Moderate | Verified, Inferred | Checkpoint commits are signed by default; secrets are scanned before writing; tokens go to the OS keyring. Against that: releases carry `checksums.txt` with no signature among the assets (GitHub attestations not checked), the main install route is `curl … \| bash`, and the CLI installs git hooks and agent hooks that run on every commit and agent turn. The security policy promises acknowledgement within 48 hours. Known advisories were not checked |
| Longevity | Weak | Verified, Claimed | Repository created 2026-01-02; v0.12.0, pre-1.0, with frequent changes, including a renamed Scoop package and a changed checkpoint storage format. Backed by a US$60 million seed round in February 2026 (claimed), with the risk to direction that venture funding brings |
| Content endurance | Strong | Verified | Transcripts and metadata are files in git trees, in JSON and transcript formats readable without Entire |
| Exit and portability | Strong, CLI; Moderate, hosted | Verified, Inferred | `entire disable --uninstall` removes hooks; checkpoint refs stay as git data or can be deleted with git. Hosted: 30 days to export after termination |
| Adjustability and support | Moderate | Verified | MIT and forkable; 328 open issues and pull requests on 2026-10-21; settings per repository and per clone. Support is GitHub issues and the company |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Claimed | 5,197 stars and 489 forks for the CLI in under ten months. A new entrant against GitHub, GitLab and Forgejo for hosting. The CLI is forkable; the hosted network is not. Contributor concentration not measured |
| Gates | Pass, with conditions | Mixed | All four pass for the CLI with telemetry and session pushing configured; the hosted services' exit is Unknown |

## Findings

### Where the transcripts go

Checkpoints live in the repository's own object store, each as its own ref, `refs/entire/checkpoints/<shard>/<id>`, pointing at a commit whose tree holds `metadata.json`, the transcripts and any subagent records. Nothing is written to git until a commit. The commit then carries an `Entire-Checkpoint` trailer, and the code branch never receives Entire's own commits <a name="apa-entire-readme-citation-2"></a>([Entire Inc., n.d.-d](#apa-entire-readme-reference)).

`strategy_options.push_sessions` defaults to `true`, so `git push` also pushes the checkpoint refs to the same remote. For a public repository that publishes every prompt, every agent response and every quoted file. For a client repository on a forge outside Canada, it moves conversation content there as well. Redaction runs before writing, but the README calls it best-effort. Sending checkpoints to a separate private repository with `--checkpoint-remote`, or not pushing them, is the safe setting for Vishpala work.

### Fit with Universal Cake's AI transparency work

The practice already marks documents as drafted with AI assistance, keeps session records (uc-automa 0016), and is assessing EU AI Act Article 50 transparency. Entire's checkpoints give a commit-level, machine-readable record of which agent, model and prompt produced which lines (`entire blame`, `entire why`), kept with the code. That is a stronger provenance record than a closing sentence, and it does not need the hosted service. It is the main reason to trial the CLI.

### Hosted services

The hosted Git network, session storage and search are run by a US company under Delaware law. Regions are India, Australia, the EU and the US, with no Canadian option. The privacy policy names PostHog, Honeycomb, Datadog, HubSpot and WorkOS as subprocessors, keeps analytics and logs up to 24 months, and keeps repository content as long as the repository exists. It does not mention Canada or PIPEDA. Speed claims come from Entire's own benchmark, `forgemark`, and are unverified. These services do not meet the sovereignty expectations the metrics set for client data.

## Recommendation

| Offering | Ring | For |
|---|---|---|
| Entire CLI, local checkpoints only | Assess, trial possible | A private trial on a Vishpala-owned test repository |
| entire-graph | Assess | Revisit at a stable release; nightly only so far |
| Hosted Git, session storage, Agentic Search | Hold, for client and Universal Cake data | Reconsider if the conditions below change |
| forgemark, Rebellion | No ring | Not tools the practice would adopt |

Conditions for a CLI trial:

- Set `--telemetry=false` and `--skip-push-sessions` at `entire enable`, or send checkpoints to a separate private repository with `--checkpoint-remote`.
- Never enable it on a repository that is or will become public, or that holds client material, until the trial has read back real transcripts and checked redaction.
- Verify the release download against `checksums.txt` and install from the release archive, not through `curl | bash`.
- Check that its git hooks coexist with uc-automa's pre-commit hook.

The hosted services re-enter at Assess if Entire offers a Canadian region or self-hosting, publishes pricing, commits to notice before changing its terms, and documents export of sessions and search data.

Re-review triggers: a 1.0 release, signed releases, a change to the default for pushing sessions, or a change to the terms or privacy policy.

## Relationship to project

AI collaboration and legal or compliance. Adopted, the CLI would graduate to the AI-collaboration guidance beside `recommendation--use-a-claude-md-file`, and inform the EU AI Act transparency work in `assess/compliance/eu-ai-act/`.

## Status notes

- Last reviewed: 2026-10-21
- In assess: an automated pass from public documents and repository metadata. A trial installs the CLI in a test container with the conditions above, runs a Claude Code session, and inspects the checkpoint refs, the redaction and the network contact.

## License

This document, *Universal Cake Evaluation: Entire, Agent Session Capture and Git Hosting*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### Entire

- [entire.io](#apa-entire-home-reference)
- [Privacy policy](#apa-entire-privacy-reference)
- [Terms of service](#apa-entire-terms-reference)
- [Entire CLI README](#apa-entire-readme-reference)

## References

<a name="apa-entire-home-reference"></a>Entire Inc. (n.d.-a). *Entire*. Retrieved October 21, 2026, from https://entire.io/
[Return to citation](#apa-entire-home-citation)

<a name="apa-entire-privacy-reference"></a>Entire Inc. (n.d.-b). *Privacy policy*. Retrieved October 21, 2026, from https://entire.io/privacy
[Return to citation](#apa-entire-privacy-citation)

<a name="apa-entire-terms-reference"></a>Entire Inc. (n.d.-c). *Terms of service*. Retrieved October 21, 2026, from https://entire.io/terms
[Return to citation](#apa-entire-terms-citation)

<a name="apa-entire-readme-reference"></a>Entire Inc. (n.d.-d). *Entire CLI README* [Source code documentation]. GitHub. Retrieved October 21, 2026, from https://github.com/entireio/cli
[Return to citation](#apa-entire-readme-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of every Entire product and service listed on entire.io on 2026-10-21. Automated recommendation: Assess for the CLI with a conditional private trial; Hold for hosted services with client or Universal Cake data |

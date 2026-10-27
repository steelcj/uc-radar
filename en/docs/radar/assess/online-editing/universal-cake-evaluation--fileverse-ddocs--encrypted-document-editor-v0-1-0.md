---
dc:title: "Universal Cake Evaluation: Fileverse dDocs, Encrypted Document Editor"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "ddocs"
  - "fileverse"
  - "online editors"
  - "end-to-end encryption"
dc:description: "Universal Cake evaluation of Fileverse dDocs, the end-to-end encrypted collaborative document editor at ddocs.new, covering the hosted service and its open source editor, cryptography and Walkaway recovery repositories."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-26"
dcterms:modified: "2026-10-26"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: "https://github.com/fileverse/fileverse-ddoc"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, cryptpad--encrypted-collaboration-suite"
dc:identifier: "universal-cake-evaluation--fileverse-ddocs--encrypted-document-editor"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-26"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of Fileverse dDocs against Universal
      Cake Evaluation Metrics 0.5.1, covering the ddocs.new hosted service,
      assessed from its published policies, and the public
      fileverse-ddoc, fileverse-cryptography and walk-away-ddocs
      repositories, read from source. The live application was not traced.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "assess" # stay in Assess until the telemetry and accessibility gates are Verified and the encryption claims can be checked against source
      reasoning: "Licence and exit gates pass; the telemetry and accessibility-floor gates are Unknown. The editor component, cryptography library and Walkaway recovery page are AGPL-3.0, but the ddocs.new application that holds keys, accounts and sync is not public, so its end-to-end encryption cannot be verified from source. The advertised post-quantum cryptography does not appear in the public cryptography library, which uses ECIES, RSA and NaCl. The privacy policy names Sentry and Plausible with no documented opt-out for account users. The public editor sends collaborators' wallet addresses to a third-party avatar service. English only, no localisation mechanism. Not self-hostable as a whole."
      risk: "high"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-26T16:45:00-04:00"
  human: []
---

# Universal Cake Evaluation: Fileverse dDocs, Encrypted Document Editor

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates dDocs, the end-to-end encrypted, collaborative document editor offered by Fileverse at ddocs.new, against the Universal Cake Evaluation Metrics. dDocs is a serious attempt at a private alternative to Google Docs: it works without an account, keeps documents locally first, exports to open formats, and ships an open source page that recovers documents without Fileverse's servers. Those are real strengths on agency and exit. The weakness is verifiability. Only parts of dDocs are open source, and the part that matters most for its central promise, the application that generates keys, encrypts, syncs and talks to Fileverse's services, is not public. The public cryptography library does not contain the post-quantum primitives the product page advertises. The privacy policy names error tracking and analytics without an opt-out, and the public editor code sends collaborators' wallet addresses to a third-party avatar service. Two of the four default gates pass and two are Unknown, so dDocs stays in Assess.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about dDocs come from the Fileverse product page, FAQ, privacy policy and terms of service, read on 2026-10-26; from three Fileverse repositories cloned and read directly on 2026-10-26, `fileverse-ddoc` at commit `d4406e25` (tag `v7.1.1`), `fileverse-cryptography` at `410d1836`, and `walk-away-ddocs` at `543edb1f`; and from press coverage of the launch and funding. The live application at ddocs.new could not be reached from the evaluation environment and was not traced. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate dDocs in two parts, because what is public and what users run are not the same thing.

- **Hosted service.** The ddocs.new web application and Fileverse's supporting services, assessed from published policies and product claims only. We did not create an account or use the application.
- **Public components.** The `@fileverse-dev/ddoc` editor package (version 7.1.1), the `@fileverse/crypto` library (version 0.0.22), and the Walkaway recovery page, read from source.

The editor's own integration notes refer to "the app repo" and to files such as `utils/ddoc-title-manager.ts` that are not in the public repository <a name="apa-fv-integration-citation"></a>([Fileverse Project, n.d.-b](#apa-fv-integration-reference)). The ddocs.new application is therefore a separate codebase, and it is not among the repositories Fileverse lists as public <a name="apa-fv-faq-citation"></a>([Fileverse, n.d.-b](#apa-fv-faq-reference)). Ratings below say which part they apply to.

Lenses: the owner is whoever adopts dDocs for a person or organization; the users are the document authors and their collaborators. For Vishpala clients these are usually the same person or a small team.

## Scorecard metadata

- Product or service name: Fileverse dDocs
- Version evaluated: ddocs.new as described by Fileverse's published pages on 2026-10-26; `@fileverse-dev/ddoc` 7.1.1, commit `d4406e25`; `@fileverse/crypto` 0.0.22, commit `410d1836`; `walk-away-ddocs` commit `543edb1f`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-26
- Radar ring: Assess
- Test platform: source reading on Ubuntu Linux; no runtime test

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Unknown | Claimed, Verified | The privacy policy names Sentry for error and performance monitoring and a self-hosted Plausible instance for analytics, and describes no opt-out (claimed). Users without an account are said to be anonymous (claimed). The public editor fetches avatars from `api.dicebear.com`, seeded with each commenter's ENS name or wallet address, with no option to turn this off (verified in source). Needs a network trace of ddocs.new with and without an account |
| A licence incompatible with the project | Pass | Verified | The editor, cryptography library, Walkaway page and permissions library are AGPL-3.0; the recovery contracts are GPL-3.0. Compatible with Universal Cake's AGPL-3.0-or-later work. The hosted application is not public, so the licence gate covers only what can be adopted as code |
| Failure of the accessibility floor | Unknown | Inferred | Built on Tiptap and ProseMirror, which produce a standard editable region usable by screen readers. The 126 interface components carry 17 `aria-*` attributes in total, and we found no accessible label on the editable region. Not tested with assistive technology |
| No exit | Pass | Verified, Claimed | Import and export of `.md`, `.docx`, `.odt`, `.pdf`, `.html` and `.txt` (claimed). Walkaway, an AGPL static page, decrypts documents from IPFS using a downloaded backup key file and exports Markdown, without Fileverse's servers (verified in source) |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Unknown | Inferred | See the accessibility-floor gate. Fileverse publishes no accessibility statement or conformance claim; the product page addresses only dark mode, themes and mobile layout |
| Multilingual integration | Weak | Verified, Claimed | Interface strings are hard-coded English in the editor, with no translation framework or locale files, so the community cannot add a language without changing code. Ethereum.org lists English as the only language (claimed). Documents themselves can be written in any language |
| Economic and cognitive accessibility | Strong | Claimed, Inferred | Free, no account needed, runs in a browser and installs as a progressive web app. The terms mention fees and a subscription term but define neither. Optional web3 concepts (wallets, ENS, portals) are hidden unless the user opts in (claimed) |
| Representation | Unknown | None | Not assessable from public information. The design centres crypto-native users (ENS names, wallet addresses, Gnosis smart contracts), which suggests who the designers imagined; nothing public on research with affected communities |
| Compatibility | Moderate | Inferred | Any modern browser on desktop or mobile. The editor bundles a large dependency tree (90 runtime dependencies, including PDF, DOCX and ODT converters, KaTeX, CodeMirror and Mermaid support), which will weigh on older hardware |
| Resilience | Moderate | Claimed, Verified | Local-first storage in the browser's IndexedDB, offline editing, and reopening of older documents offline (claimed). If the service disappears, Walkaway needs only a Gnosis Chain RPC endpoint and an IPFS gateway (verified). Accountless documents exist only in that browser's storage, so clearing site data loses them |
| Agency, sovereignty and privacy | Moderate | Claimed, Verified | Strong in design: end-to-end encryption, keys held by the user, collaborator invitations through zero-knowledge proofs so Fileverse does not see collaborators' identifiers, and a stated commitment that content is not used for AI training (all claimed). Weakened by what cannot be checked: the application that holds keys is closed, so the owner cannot self-host or verify it, and the post-quantum claim is not reflected in the public cryptography library (verified), see Findings. Identity verification uses Privy, a third party. Peer-to-peer networks may expose users' IP addresses to other users (claimed) |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Moderate | Claimed, Inferred | Exit cost is low per document and moderate in bulk: export each document, or run Walkaway with the backup key. Portability: Markdown and ODT are both machine- and human-readable. Terms of service carry no date and no change-notice process; the privacy policy was last changed 10 November 2025 and promises advance notice only where possible. Pricing: free today, with undefined fees in the terms |
| Agency, interaction patterns | Moderate | Claimed, Inferred | No account wall to start writing, no advertising. Accountless use hides a sharp edge: losing browser storage loses work, and nothing we read says how prominently that is explained. AI autocomplete calls an Ollama-compatible endpoint the integrator configures, defaulting to a local server, so suggestions are not routed through a vendor by default (verified in source) |
| Environment, direct, indirect, and authoring-side | Moderate | Inferred | Gnosis Chain is proof-of-stake, so on-chain registry writes are low in energy. IPFS replication and real-time sync add network traffic beyond a plain editor. Authoring-side load is the larger concern: a heavy JavaScript editor runs in every author's browser. No figures published |
| Security | Moderate | Verified, Claimed | Cryptography uses audited, widely used libraries (`@noble/curves`, `@noble/ciphers`, TweetNaCl). The editor is published to npm through GitHub OIDC trusted publishing, which ties each release to a CI build (verified). No security policy or reporting address in the editor repository (verified absence) and no published audit found (inferred from search). The terms exclude liability for failures of security measures. Encryption of the hosted application cannot be reviewed |
| Longevity | Moderate | Verified, Claimed | Editor repository started June 2024; 961 non-merge commits, 115 in the last 90 days (verified). Funded by Gitcoin grants from 2022 and a USD 1.5 million pre-seed round led by Gnosis Chain and Factor (claimed). A small venture-funded company, not a foundation |
| Content endurance | Moderate | Inferred | Content is authored inside the tool, so dDocs is a container, not an enhancement layer. Endurance rests on the user keeping the backup key file and on IPFS pinning continuing after the service ends; the terms say data stays available via IPFS unless the services are terminated |
| Exit and portability | Strong | Verified, Claimed | See the No exit gate. Walkaway is the best-designed exit path we have seen in a hosted editor: static, open source, and mirrored on ENS and a Tor onion address |
| Adjustability and support | Weak | Inferred | The editor component can be forked and embedded, but the product users actually run cannot be modified or self-hosted. Support is through GitHub issues and an email address. The editor's API moved from 6.0.2 to 7.1.1 in six days in October 2026, two major versions, which makes it a moving target for embedders |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Moderate | Verified, Inferred | A small entrant against Google Docs and Microsoft 365, with no take rate. Forkability is partial: the editor, cryptography and recovery code survive a hostile steward, the application and its account and sync services do not. Contributor concentration: 17 authors; the ten most active wrote over 99% of commits, most of whom appear to be Fileverse staff (inferred), and fewer than 1% come from occasional outside contributors. No foundation-model dependency: AI features call a configurable endpoint |
| Gates | Unknown | Mixed | Licence and exit pass. Telemetry and accessibility floor Unknown, see Gates |

## Findings

### What is open source and what is not

Fileverse says it is opening its repositories gradually <a name="apa-fv-faq-2-citation"></a>([Fileverse, n.d.-b](#apa-fv-faq-reference)). The public dDocs repository is the editor component, an embeddable React package, plus a demo <a name="apa-fv-readme-citation"></a>([Fileverse Project, n.d.-a](#apa-fv-readme-reference)). The application that wraps it at ddocs.new, which handles accounts, key generation and storage, encryption of the document stream, sync, the agent API, and calls to Sentry, Plausible and Privy, is a separate repository that is not public. The evaluation framework's structural questions mostly turn on that application, not on the editor.

This matters because dDocs's central promise is that Fileverse cannot read user content. That is a property of the closed code. The product page frames it as true by design rather than by promise; for the part users run, it is currently a promise.

### The post-quantum claim

The product page and the editor README both advertise end-to-end encryption with post-quantum cryptography <a name="apa-fv-ddocs-citation"></a>([Fileverse, n.d.-a](#apa-fv-ddocs-reference)). The public cryptography library offers ECIES on elliptic curves, RSA with envelope encryption, and NaCl secret boxes, and depends only on `@noble/curves`, `@noble/ciphers`, `@noble/hashes` and TweetNaCl <a name="apa-fv-crypto-citation"></a>([Fileverse Project, n.d.-e](#apa-fv-crypto-reference)). None of these is post-quantum, and a search of the library and editor source found no ML-KEM, Kyber or other post-quantum primitive. The Walkaway documentation describes the backup keys as RSA key pairs and file keys as AES keys <a name="apa-fv-walkaway-citation"></a>([Fileverse Project, n.d.-f](#apa-fv-walkaway-reference)).

Either the post-quantum layer lives in the closed application, or the claim describes something narrower than the whole system. Both are possible; neither can be confirmed from public material. The question to send Fileverse is which key exchange protects document keys in the current release, and whether documents recovered through Walkaway were encrypted with it. Until then this is Claimed, and the gap between the claim and the public code is itself an information-asymmetry data point under the metrics' power principles.

### Third-party requests from the public editor

When a comment or suggestion thread renders, the editor builds each participant's avatar from `api.dicebear.com`, using the participant's ENS name, or failing that their wallet address, as the seed <a name="apa-fv-avatar-citation"></a>([Fileverse Project, n.d.-d](#apa-fv-avatar-reference)). The ENS fallback code sets the name to the wallet address when no ENS name resolves. Every reader of a commented document therefore discloses to DiceBear which wallet addresses are collaborating on documents they open, along with the reader's own IP address. This sits awkwardly beside the privacy policy's statement that collaborators' identifiers are hidden from Fileverse through zero-knowledge proofs. It is not content exposure, and the hosted application may override it, but no option to disable it exists in the editor. The fix is small: generate identicons locally, as several small libraries do.

Two other outbound requests are user-initiated and reasonable: embedded tweets are fetched through `react-tweet.vercel.app`, and YouTube, Vimeo and SoundCloud embeds load from those services. The README states that the editor makes no third-party font requests, which matches the code.

### Telemetry, analytics and identity providers

The privacy policy, last updated 10 November 2025, says users without an account give no personal data, and that account users' data includes an email or social handle, a wallet address, passkeys, document metadata (size, collaborators, timestamps) and collaboration session metadata <a name="apa-fv-privacy-citation"></a>([Fileverse, 2025](#apa-fv-privacy-reference)). It names Privy for identity verification, a self-hosted Plausible instance for analytics, and Sentry for error and performance monitoring configured not to collect IP addresses. It describes no way to turn Sentry or Plausible off. Whether either runs for accountless users is not stated. A browser network trace, with and without an account, would settle the telemetry gate.

The same policy names Fileverse Limited in its body and Fileverse PTE. LTD. in its footer, applies Singapore's Personal Data Protection Act, and says personal data may be disclosed to parties in Australia, Singapore, the EU and the United States. For Quebec clients, this is a cross-border transfer of personal information that Law 25 expects to be assessed before adoption.

### Terms of service

The terms are governed by Singapore law with arbitration in Singapore <a name="apa-fv-terms-citation"></a>([Fileverse, n.d.-c](#apa-fv-terms-reference)). They carry no revision date and describe no process for notifying users of changes. Fileverse may end the services, or transfer their operation to a third party or a smart contract, on 30 days' notice. Users keep ownership of their data and grant Fileverse a sublicensable licence to use it only to provide the services, which matters little while content is encrypted. Liability is capped at fees paid in the previous 12 months, which for free users is nothing, and failures of security measures are excluded.

### Exit: Walkaway

Walkaway is a static page, AGPL-licensed, that takes the backup key file a user downloads from their account settings, finds the user's documents through their Gnosis Chain portal contract, fetches the encrypted files from IPFS, decrypts them in the browser, and exports them as Markdown or PDF <a name="apa-fv-walkaway-2-citation"></a>([Fileverse Project, n.d.-f](#apa-fv-walkaway-reference)). It needs only a public RPC endpoint and an IPFS gateway, both configurable. It is mirrored on ENS and a Tor onion address, and Fileverse also mirrors its code on Radicle. This is the reversibility test answered in code rather than in a promise, and it is the main reason the exit gate passes despite the closed application. It depends on the user having downloaded the backup key in advance.

### AI features

The editor's autocomplete extension calls an Ollama-compatible endpoint whose address and model the integrating application supplies; with no address set, the Ollama browser client falls back to a server on the user's own machine <a name="apa-fv-ai-citation"></a>([Fileverse Project, n.d.-c](#apa-fv-ai-reference)). Ethereum.org's listing describes this as a local LLM plugin <a name="apa-eth-ddocs-citation"></a>([Ethereum.org, 2025](#apa-eth-ddocs-reference)). This avoids a foundation-model dependency and keeps document text on the user's machine if the endpoint is local. We did not check whether accepted suggestions are marked as AI-generated, which the metrics' output provenance proxy asks about. The separate dDocs API for agents is described as opt-in and end-to-end encrypted (claimed); it was not evaluated.

### Supply chain and maintenance

The editor publishes to npm from GitHub Actions using OIDC trusted publishing <a name="apa-fv-release-citation"></a>([Fileverse Project, n.d.-g](#apa-fv-release-reference)), a good practice that links each npm release to a CI run. One third-party action in that workflow is pinned by tag rather than commit. The package declares 90 runtime and 14 peer dependencies. There is no `SECURITY.md` and no stated channel for private vulnerability reports.

Release cadence is very fast: tags `v6.0.2`, `v7.0.0` and `v7.1.1` fall on 16, 19 and 22 October 2026. That suits Fileverse's own application, which pins exact versions, but anyone embedding the editor should expect frequent breaking changes.

### Background

dDocs launched in early July 2024, described by its founder at the time as a version 0.1 experiment <a name="apa-block-launch-citation"></a>([The Block, 2024](#apa-block-launch-reference)). Fileverse was first built with Gitcoin funding rounds in 2022 and later raised a USD 1.5 million pre-seed round led by Gnosis Chain and Factor, with Safe, Mask Network, Balaji Srinivasan and others participating <a name="apa-ti-funding-citation"></a>([The Trusted Insight, 2023](#apa-ti-funding-reference)).

## Recommendation

Keep dDocs in Assess. It should not be offered to Vishpala clients as a default document editor yet, and it is not a substitute for the radar's existing self-hostable options such as CryptPad, which publishes the whole application. Two gates are Unknown, and its central security claim rests on code that cannot be read.

What would move it forward:

- Trace ddocs.new in a browser with and without an account, recording every outbound host, to resolve the telemetry gate.
- Test the editor with a screen reader and keyboard alone, including comments, suggestions and the toolbar, to resolve the accessibility-floor gate.
- Ask Fileverse which key exchange protects document keys today, where the post-quantum layer is implemented, whether the ddocs.new application will be published, and whether Sentry and Plausible can be disabled.
- Report upstream: the DiceBear avatar requests, the absent security policy, and the unlabelled editing region.

If a specific client needs dDocs, for example to collaborate with people already using it, the safer pattern is to use it without an account for short-lived drafts, export to Markdown or ODT into the client's own archive, and treat dDocs as a workspace rather than the store of record. Account users should download the backup key file at once and keep it with their other recovery material.

Re-review triggers specific to dDocs: publication of the ddocs.new application source; any published security audit; a change in the cryptography library's primitives; a change of the legal entity or jurisdiction; any move to paid plans; and Fileverse using its right to transfer operation of the service to a third party or smart contract.

## License

This document, *Universal Cake Evaluation: Fileverse dDocs, Encrypted Document Editor*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### Fileverse service

- [dDocs product page](#apa-fv-ddocs-reference)
- [Frequently asked questions](#apa-fv-faq-reference)
- [Terms of service](#apa-fv-terms-reference)
- [Privacy policy](#apa-fv-privacy-reference)

### Fileverse repositories

- [fileverse-ddoc README](#apa-fv-readme-reference)
- [ddocs.new integration notes](#apa-fv-integration-reference)
- [AI autocomplete extension](#apa-fv-ai-reference)
- [Comment reply input](#apa-fv-avatar-reference)
- [fileverse-cryptography README](#apa-fv-crypto-reference)
- [Walkaway README](#apa-fv-walkaway-reference)
- [Release workflow](#apa-fv-release-reference)

### Coverage

- [Ethereum.org app listing](#apa-eth-ddocs-reference)
- [The Block launch coverage](#apa-block-launch-reference)
- [Funding coverage](#apa-ti-funding-reference)

## References

<a name="apa-eth-ddocs-reference"></a>Ethereum.org. (2025). *Fileverse dDocs* [App listing]. https://ethereum.org/apps/fileverse-ddocs/
[Return to citation](#apa-eth-ddocs-citation)

<a name="apa-fv-privacy-reference"></a>Fileverse. (2025). *Privacy policy*. https://fileverse.io/privacy
[Return to citation](#apa-fv-privacy-citation)

<a name="apa-fv-ddocs-reference"></a>Fileverse. (n.d.-a). *dDocs*. https://fileverse.io/ddocs
[Return to citation](#apa-fv-ddocs-citation)

<a name="apa-fv-faq-reference"></a>Fileverse. (n.d.-b). *Frequently asked questions*. https://fileverse.io/faq
[Return to citation](#apa-fv-faq-citation)

<a name="apa-fv-terms-reference"></a>Fileverse. (n.d.-c). *Terms of service*. https://fileverse.io/terms
[Return to citation](#apa-fv-terms-citation)

<a name="apa-fv-readme-reference"></a>Fileverse Project. (n.d.-a). *README.md* [fileverse-ddoc repository]. GitHub. https://github.com/fileverse/fileverse-ddoc/blob/main/README.md
[Return to citation](#apa-fv-readme-citation)

<a name="apa-fv-integration-reference"></a>Fileverse Project. (n.d.-b). *DDOCS_NEW_INTEGRATION.md* [fileverse-ddoc repository]. GitHub. https://github.com/fileverse/fileverse-ddoc/blob/main/docs/DDOCS_NEW_INTEGRATION.md
[Return to citation](#apa-fv-integration-citation)

<a name="apa-fv-ai-reference"></a>Fileverse Project. (n.d.-c). *ai-autocomplete.ts* [Source code]. GitHub. https://github.com/fileverse/fileverse-ddoc/blob/main/package/extensions/ai-autocomplete/ai-autocomplete.ts
[Return to citation](#apa-fv-ai-citation)

<a name="apa-fv-avatar-reference"></a>Fileverse Project. (n.d.-d). *comment-reply-input.tsx* [Source code]. GitHub. https://github.com/fileverse/fileverse-ddoc/blob/main/package/components/inline-comment/comment-reply-input.tsx
[Return to citation](#apa-fv-avatar-citation)

<a name="apa-fv-crypto-reference"></a>Fileverse Project. (n.d.-e). *README.md* [fileverse-cryptography repository]. GitHub. https://github.com/fileverse/fileverse-cryptography/blob/main/README.md
[Return to citation](#apa-fv-crypto-citation)

<a name="apa-fv-walkaway-reference"></a>Fileverse Project. (n.d.-f). *README.md* [walk-away-ddocs repository]. GitHub. https://github.com/fileverse/walk-away-ddocs/blob/main/README.md
[Return to citation](#apa-fv-walkaway-citation)

<a name="apa-fv-release-reference"></a>Fileverse Project. (n.d.-g). *release.yml* [Workflow file]. GitHub. https://github.com/fileverse/fileverse-ddoc/blob/main/.github/workflows/release.yml
[Return to citation](#apa-fv-release-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

<a name="apa-block-launch-reference"></a>The Block. (2024, July 15). *Fileverse unveils Google Documents competitor ddocs*. https://www.theblock.co/post/305321/fileverse-unveils-google-documents-competitor-ddocs
[Return to citation](#apa-block-launch-citation)

<a name="apa-ti-funding-reference"></a>The Trusted Insight. (2023). *Web3 file management app Fileverse gets $1.5M investment from Gnosis Chain, Balaji Srinivasan, other investors*. https://www.thetrustedinsight.com/investment-news/web3-file-management-app-fileverse-gets-15m-investment-from-gnosis-chain-balaji-srinivasan-other-investors-20231017822/
[Return to citation](#apa-ti-funding-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of Fileverse dDocs against Universal Cake Evaluation Metrics 0.5.1, covering the ddocs.new hosted service, assessed from its published policies, and the public editor, cryptography and Walkaway repositories, read from source; the live application was not traced |

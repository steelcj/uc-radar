---
dc:title: "Universal Cake Evaluation: Production-Ready Droplet, Multi-Site Caddy and Eleventy Hosting"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "hosting"
  - "caddy"
  - "eleventy"
  - "digitalocean"
  - "cloud-init"
dc:description: "Universal Cake evaluation of the approach set out in Production-ready Droplet for multi-site Caddy and Eleventy hosting v0.1.0: a Debian 13 DigitalOcean Droplet, configured by cloud-init, serving precompressed Eleventy output for several sites through one Caddy instance."
dc:publisher: "UniversalCake"
dcterms:created: "2026-10-24"
dcterms:modified: "2026-10-24"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en-CA"
dc:source: "production-ready-droplet-multi-site-caddy-eleventy.md v0.1.0"
dc:relation: "universal-cake-evaluation-metrics, uc-radar-entry-template, server-os--linux-server-operating-system--radar-entry, shims--options-for-provider-agnostic-approaches-to-provisioning, caddy--web-server--manual-installation-and-configuration-on-a-production-server, webservers--options-for-static-site-hosting"
dc:identifier: "universal-cake-evaluation--production-ready-droplet--multi-site-caddy-eleventy"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.5.1"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-24"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      Initial Assess-ring evaluation of the production-ready Droplet
      approach document v0.1.0 against Universal Cake Evaluation Metrics
      0.5.1. The document's artifacts were extracted and tested in a
      disposable Ubuntu 24.04 sandbox; no Droplet was created and the
      cloud-init file was not booted.
uc-radar_evaluation:
  automated:
    - evaluator: "Claude Opus 5.5 (Anthropic)"
      evaluator_version: "claude-opus-5-5"
      recommendation: "trial" # once the Security fixes under Recommendation are made and a Debian 13 Incus run is Verified
      reasoning: "All four default gates pass, none yet Verified end to end. The SSH, firewall, cloud-init schema and Caddy serving behaviour were Verified in testing. One material defect: if the Caddy repository step fails at first boot, runcmd continues and installs Debian's Caddy 2.6.2, which carries an open CVE and does not expand the {args[0]} snippet placeholder, so every site returns 404 while caddy validate still reports a valid configuration. Other concerns: one deploy key can overwrite or delete every client's site; Caddy is outside unattended-upgrades; the approach conflicts with the adopted shim contract and drops the release and rollback pattern of the existing Caddy installation document."
      risk: "moderate"
      metrics: "Universal Cake Evaluation Metrics"
      metrics_version: "0.5.1"
      evaluation_datetime: "2026-10-24T18:55:00-04:00"
  human: []
---

# Universal Cake Evaluation: Production-Ready Droplet, Multi-Site Caddy and Eleventy Hosting

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates an approach, not a product: the hosting setup described in *Production-ready Droplet for multi-site Caddy and Eleventy hosting* version 0.1.0 <a id="cite-steel-2026b"></a>([Steel, 2026b](#ref-steel-2026b)). It applies the Universal Cake Evaluation Metrics and records the gate results, a scorecard, the findings behind the ratings, and a recommendation for the radar. The approach passes all four default gates and scores well on exit, portability, low-bandwidth delivery and visitor privacy. Testing found one material defect: a failure in the Caddy repository step at first boot leads, without any error, to a server that serves no sites. It also found a few smaller security gaps, and two conflicts with decisions the radar has already adopted.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a id="cite-steel-2026e"></a>([Steel, 2026e](#ref-steel-2026e)). It was checked against four existing radar entries: the adopted server operating system decision <a id="cite-steel-2026c"></a>([Steel, 2026c](#ref-steel-2026c)), the adopted shim contract for provisioning <a id="cite-steel-2026d"></a>([Steel, 2026d](#ref-steel-2026d)), the adopted web server decision <a id="cite-steel-2026f"></a>([Steel, 2026f](#ref-steel-2026f)), and the Caddy manual installation document <a id="cite-steel-2026a"></a>([Steel, 2026a](#ref-steel-2026a)). The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic), the same collaborator that drafted the approach document, so a human review pass matters more than usual here.

## Scope

The subject is the setup as specified in version 0.1.0 of the approach document: a single DigitalOcean Droplet on Debian 13, configured entirely by one `#cloud-config` file, protected by a DigitalOcean Cloud Firewall and an nftables ruleset, and serving several Eleventy-built static sites through one Caddy instance. Builds run off the server and are precompressed, then pushed with `rsync` through an rrsync-restricted `deploy` key.

The lenses are the **owner** (Vishpala, and each client whose site is hosted), the **user** (each site's visitors), and the **community** (anyone reusing the published approach). Rows that an approach of this kind cannot affect are rated N/A with the reason, following the row-granularity rule in the metrics.

## Scorecard metadata

- Product or service name: Production-ready Droplet for multi-site Caddy and Eleventy hosting (approach document)
- Version evaluated: 0.1.0, dated 2026-10-24, MD5 `fc600b6c1a048213d3bf572f955edb87`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-10-24
- Radar ring: Assess
- Test platform: disposable Ubuntu 24.04.5 LTS sandbox, cloud-init 26.2, OpenSSH 9.6p1, nftables 1.0.9, Caddy 2.6.2 from the Ubuntu archive. The sandbox could not reach Caddy's Cloudsmith repository, so the current Caddy release was not tested. No Droplet was created, and the cloud-init file was validated and rendered but not booted.

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass | Inferred | The approach adds no analytics, and access logging is off. Outbound traffic is limited to ACME certificate requests, apt updates and DigitalOcean's metadata service. DigitalOcean's optional monitoring agent is left off. Not network-traced. Whether DigitalOcean's Debian 13 image ships its console agent was not checked, see Findings |
| A licence incompatible with the project | Pass | Inferred | Every component is free software: Debian, Caddy (Apache-2.0), cloud-init, nftables, OpenSSH, rsync and Eleventy. The approach document itself carries no licence statement, which house convention requires |
| Failure of the accessibility floor | Pass | Inferred | The approach serves each site's HTML unchanged and adds no scripts, overlays or interstitials. The floor therefore rests on each Eleventy site, which must be evaluated on its own |
| No exit | Pass | Inferred | The server holds no irreplaceable state: content lives in site repositories, configuration in plain text files. A rebuild on another provider is documented step by step but was not exercised |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | N/A | Inferred | Content accessibility belongs to each Eleventy site. The approach neither adds nor removes barriers |
| Multilingual integration | Moderate | Verified | Serving is language-neutral and places no limit on Eleventy's multilingual paths. The approach document exists only in English (`en-CA`) at this time, which limits its use by French-speaking Quebec clients and collaborators |
| Economic and cognitive accessibility | Strong (hardware and connectivity) / Moderate (cognitive) | Verified, Inferred | Visitors receive zstd or Brotli sidecars with the correct `Content-Encoding` and `Vary` headers, with no compression cost per request (verified). The smallest Droplet tier suffices (inferred). The document is long, assumes Linux administration experience, and has six placeholders to substitute by hand |
| Representation | Unknown | None | One author with AI assistance; no review by the people who will operate it or by clients. No machine-learned models in the approach |
| Compatibility | Strong | Verified, Inferred | Visitors need nothing beyond a browser: HTTP/1.1 and HTTP/2 verified, HTTP/3 not tested, no JavaScript required. The owner needs a Unix-like workstation, or WSL on Windows |
| Resilience | Moderate | Verified, Inferred | Static output with no runtime services beyond Caddy. However, deployment rewrites files in place with no release history, so there is no one-step rollback or maintenance mode. One Droplet is a single point of failure, with recovery by rebuild rather than restore. Certificate renewal needs outbound access to the certificate authority |
| Agency, sovereignty and privacy | Strong (owner) / Moderate (overall) | Verified, Inferred | The owner controls every key, file and configuration. Visitor privacy is strong: no access logs, no cookies or scripts added. Moderate overall because the host is a US-owned provider, even in the Toronto region, and the sudo password hash is readable through the Droplet's metadata service, a point the document already flags |
| Agency, power-imbalance proxies (exit cost, portability, ToS volatility, pricing asymmetry) | Strong | Inferred | Estimated exit cost: a few hours per server plus DNS changes, since only the Cloud Firewall and the `doctl` commands are provider-specific. Everything else is plain text, readable by both people and programs. DigitalOcean's terms-of-service history was not assessed (Unknown). Pricing is public |
| Agency, interaction patterns | Strong | Inferred | The approach adds nothing to what visitors see: no banners, prompts, tracking or redirects beyond HTTPS and `www` to the apex |
| Environment, direct, indirect, and authoring-side | Strong | Verified, Inferred | Compression happens once per build rather than once per request (verified). There is long-lived caching for hashed assets, and no Node.js on the server. One small, low-end server hosts many sites. Each deploy recompresses every text file at maximum settings, which is wasteful for large sites but runs on the workstation |
| Security | Moderate | Verified, Inferred | SSH hardening, nftables syntax and HTTP security headers verified. Against that: a failed first-boot repository step installs an outdated, vulnerable Caddy that serves nothing (verified, see Findings). One deploy key can overwrite or delete every hosted site. Caddy is not covered by automatic updates. Headers are missing on error pages. The Cloudsmith signing key is fetched without a pinned fingerprint. rrsync restrictions not tested (Inferred) |
| Longevity | Moderate | Inferred | The components are long-lived: Debian 13 is supported for five years from its August 2025 release <a id="cite-debian-2025"></a>([Debian, 2025](#ref-debian-2025)), and Caddy is actively developed. The approach itself is a first draft by one author and has not been run end to end |
| Content endurance | Strong | Inferred | The server is an enhancement layer. Removing or replacing it leaves every site's content whole in its repository |
| Exit and portability | Strong | Inferred | See the No exit gate. The cloud-init file, nftables ruleset, Caddyfile and deploy script are provider-neutral by design |
| Adjustability and support | Strong | Inferred | Every part is open source, plain text and documented inline. Support comes from large, established communities (Debian, Caddy, cloud-init) rather than a vendor |
| Market position (concentration, take rate, API stability, forkability, contributor concentration) | Strong | Inferred | The approach is built for substitution: DigitalOcean is the only provider in it, and it is replaceable. No take rate, and no foundation-model dependency. Caddy's contributor concentration was not measured here |
| Gates | Pass | Inferred | All four pass on available evidence. None is Verified end to end, which Trial requires |

## Findings

### A failed repository step produces a server that serves nothing

The cloud-init `runcmd` list installs Caddy in three steps: fetch Cloudsmith's signing key, write the repository source, then run `apt-get install -y caddy`. cloud-init turns `runcmd` into one `/bin/sh` script, and rendering the document's list with cloud-init 26.2 confirmed that it contains no `set -e`. A failed step is logged, and the script carries on.

The sandbox happened to reproduce the failure: its egress policy blocks Cloudsmith, so the key download failed. `apt-get install caddy` then installed the distribution's own Caddy package, version 2.6.2, without complaint. Debian 13 ships the same upstream version, packaged as `2.6.2-12+deb13u1` <a id="cite-debian-piuparts-2026"></a>([Debian Project, 2026](#ref-debian-piuparts-2026)). Debian's security tracker lists one open vulnerability for that package in trixie, with two more undetermined <a id="cite-debian-security-nd"></a>([Debian Security Tracker, n.d.](#ref-debian-security-nd)).

The outdated package also breaks the configuration. Caddy 2.6.2 does not expand the `{args[0]}` index form that the shared snippet uses, and which the current `import` documentation describes <a id="cite-caddy-nd"></a>([Caddy, n.d.](#ref-caddy-nd)). Adapting the Caddyfile with 2.6.2 produced the literal root `/srv/sites/{args[0]}`, and every request returned 404. Yet `caddy validate` reported "Valid configuration" and exited 0, so the document's final validation step would pass too.

The result is a first boot that looks successful at every check the document asks for, except the verification checklist run after the first deploy, and a server that serves nothing from an outdated binary. The fix is to make that step fail loudly. Options include a script installed by `write_files` that starts with `set -eu`, an apt preference that gives Debian's own `caddy` package a priority of -1, and a check that `caddy version` reports 2.7 or later before the Caddyfile is installed.

### What testing verified

The document's artifacts were extracted directly from version 0.1.0 and tested unchanged, except where noted.

- `cloud-init schema` reports the `cloud-config.yaml` as valid.
- The SSH drop-in, placed alongside a deliberately conflicting `50-cloud-init.conf` setting `PasswordAuthentication yes`, produced the intended effective configuration in `sshd -T`. Root login, password and keyboard-interactive authentication are off, `AuthenticationMethods publickey` is set, and only `sysadmin` and `deploy` are allowed. This confirms the document's reasoning about first-value-wins ordering, on OpenSSH 9.6p1.
- The nftables ruleset passes `nft -c`.
- The bootstrap Caddyfile validates with an empty `sites/` directory, producing only a warning. That settles one of the two open questions in the approach document.
- Runtime tests used the production snippet, changing only the root path, ports and certificate authority, plus the `{args.0}` placeholder needed by 2.6.2. Caddy served the zstd sidecar to a browser-style `Accept-Encoding` header and the Brotli sidecar when only Brotli was offered, with `Vary: Accept-Encoding`. It set HSTS, `nosniff` and the referrer policy, and removed the `Server` header. It applied the one-year immutable cache only to the hashed asset and the five-minute policy to HTML, served the site's `404.html` with status 404, and redirected `www` to the apex with the path and query intact.

### Smaller security and serving gaps

- **Error responses lose the security headers.** The 404 response carried no HSTS, `nosniff` or referrer policy, and did send `Server: Caddy`, because the `header` block does not apply inside `handle_errors`. Repeating the headers in the error route, or moving them into a second snippet imported in both places, closes this. Verified on 2.6.2; behaviour on current Caddy is Inferred to be the same.
- **Sidecars are reachable directly.** A request for `/index.html.br` returned the Brotli bytes with no `Content-Type` or `Content-Encoding`. This is harmless but untidy.
- **Unhashed assets have no cache policy.** Unhashed CSS and JavaScript fall back to the browser's own caching heuristics, so visitors may briefly see stale styles after a deploy. A short explicit `Cache-Control` for the remaining static files would remove the guesswork.
- **One deploy key covers every client.** The key is restricted to `/srv/sites`, so a leaked key can overwrite or delete every hosted site, not just one, and `--delete-after` makes deletion a normal operation. Per-site keys restricted to `/srv/sites/<domain>` would limit the damage to one site.
- **Caddy is outside automatic updates.** The document says so and gives the manual command. A Caddy security release would still wait for someone to act. Adding the repository's origin to unattended-upgrades, or scheduling a check, closes the gap.
- **The signing key is trusted on first use.** The key is fetched over HTTPS and dearmored without checking its fingerprint. This matches Caddy's own instructions, but pinning the fingerprint in the cloud-init file would make a substituted key fail.
- **SSH on the host firewall is open to all addresses.** Source restriction rests entirely on the Cloud Firewall, so detaching it exposes SSH, still key-only, to the internet. The document offers this as optional; for production it should be the default.
- **Possible provider agent.** DigitalOcean installs a console agent on some of its images. Whether its Debian 13 image includes it, and what it connects to, should be checked on a real Droplet and recorded under the telemetry gate.

The rrsync restriction itself was not tested: whether `--delay-updates` and `--chmod` pass through `rrsync -wo`, and whether reads and shell access are refused. That evidence remains Inferred from the rrsync manual page.

### Conflicts and agreements with adopted radar decisions

The approach agrees with three adopted decisions. It uses Debian 13 as the operating system, Caddy as the web server, and the single-instance, single-Caddyfile model for multiple sites. It also resolves a pending action in the server operating system entry. That entry assumed DigitalOcean did not offer Debian 13 and planned a custom image, but `debian-13-x64` is now a standard image, so no custom image is needed.

It conflicts with two others.

- **The shim contract.** The adopted contract says the shim must not install software beyond the base image, or configure the operating system beyond injecting the SSH key and applying a provisional firewall. Everything else belongs to the orchestration layer, with OpenTofu preferred for DigitalOcean. This approach does all server configuration in cloud-init at creation time and drives DigitalOcean with `doctl`. Either the approach should be split, with a minimal shim and the cloud-init content moved into the orchestration layer, or the contract should be amended to recognise cloud-init as an orchestration mechanism. That choice belongs in a recorded decision, not in this evaluation.
- **The release pattern.** The existing Caddy installation document deploys into timestamped release directories with a `current` symlink, which gives one-step rollback and a maintenance mode. This approach deploys in place into `/srv/sites/<domain>` and has neither. The two should be reconciled before either reaches production. One option is an rrsync-compatible release layout under `/srv/sites`, with the symlink repointed through a narrowly scoped command.

## Recommendation

Keep the approach in Assess. Move it to Trial once these changes are made and a Debian 13 Incus run turns the gate and Security evidence into Verified:

- Make the Caddy installation fail loudly, and refuse Debian's own `caddy` package, as described under the first finding.
- Pin the Cloudsmith signing key fingerprint.
- Apply the security headers on error responses too.
- Issue one deploy key per site, restricted to that site's directory.
- Bring Caddy under automatic security updates, or schedule an update check.
- Restrict SSH by source address in nftables by default, not only in the Cloud Firewall.
- Decide, and record, how the approach relates to the shim contract, and reconcile its deploy layout with the release pattern.
- Add the house licence statement and Dublin Core front matter to the approach document, and consider a French version for Quebec clients.

During the Incus run, test the rrsync restriction, HTTP/3 over UDP 443, and a full rebuild on a second container. Then repeat the telemetry check on a real Droplet.

Re-review triggers specific to this approach: a Caddy release that changes Caddyfile or snippet syntax, a change in how Debian or DigitalOcean handles `sshd_config.d` or user data, an amendment to the shim contract, and any move off DigitalOcean.

## License

This document, *Universal Cake Evaluation: Production-Ready Droplet, Multi-Site Caddy and Eleventy Hosting*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#ref-steel-2026e)

### Subject

- [Production-ready Droplet for multi-site Caddy and Eleventy hosting](#ref-steel-2026b)

### Related radar entries

- [Server operating system decision](#ref-steel-2026c)
- [Shim contract for provisioning](#ref-steel-2026d)
- [Web server decision for static hosting](#ref-steel-2026f)
- [Caddy manual installation on a production server](#ref-steel-2026a)

### External sources

- [Caddy import directive](#ref-caddy-nd)
- [Debian 13 release announcement](#ref-debian-2025)
- [Debian caddy package test log](#ref-debian-piuparts-2026)
- [Debian security tracker for caddy](#ref-debian-security-nd)

## References

<a id="ref-caddy-nd"></a>Caddy. (n.d.). *import*. Caddy documentation. Retrieved October 24, 2026, from https://caddyserver.com/docs/caddyfile/directives/import
[Return to citation](#cite-caddy-nd)

<a id="ref-debian-2025"></a>Debian. (2025, August 9). *Debian 13 "trixie" released* [Press release]. https://www.debian.org/News/2025/20250809
[Return to citation](#cite-debian-2025)

<a id="ref-debian-piuparts-2026"></a>Debian Project. (2026, August 12). *caddy_2.6.2-12+deb13u1* [piuparts test log]. https://piuparts.debian.org/trixie-pu/pass/caddy_2.6.2-12+deb13u1.log
[Return to citation](#cite-debian-piuparts-2026)

<a id="ref-debian-security-nd"></a>Debian Security Tracker. (n.d.). *caddy*. Retrieved October 24, 2026, from https://security-tracker.debian.org/tracker/caddy
[Return to citation](#cite-debian-security-nd)

<a id="ref-steel-2026a"></a>Steel, C. (2026a). *Caddy: Web server: Manual installation and configuration on a production server* (Version 0.1.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-steel-2026a)

<a id="ref-steel-2026b"></a>Steel, C. (2026b). *Production-ready Droplet for multi-site Caddy and Eleventy hosting* (Version 0.1.0) [Unpublished manuscript]. Vishpala.
[Return to citation](#cite-steel-2026b)

<a id="ref-steel-2026c"></a>Steel, C. (2026c). *Server OS: Linux server operating system: Radar entry* (Version 0.1.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-steel-2026c)

<a id="ref-steel-2026d"></a>Steel, C. (2026d). *Shims: Options for provider-agnostic approaches to provisioning* (Version 0.1.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-steel-2026d)

<a id="ref-steel-2026e"></a>Steel, C. (2026e). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-steel-2026e)

<a id="ref-steel-2026f"></a>Steel, C. (2026f). *Webservers: Options for static site hosting* (Version 0.1.0) [Unpublished manuscript]. UniversalCake.
[Return to citation](#cite-steel-2026f)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | Initial Assess-ring evaluation of the production-ready Droplet approach document v0.1.0 against Universal Cake Evaluation Metrics 0.5.1, with the document's artifacts tested in a disposable Ubuntu 24.04 sandbox; rrsync restriction, HTTP/3 and a Debian 13 first boot not yet tested |

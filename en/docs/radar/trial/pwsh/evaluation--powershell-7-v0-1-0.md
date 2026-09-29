---
dc:title: "Evaluation: PowerShell 7 (pwsh)"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "evaluation"
  - "radar"
  - "powershell"
  - "developer tooling"
dc:description: "Universal Cake evaluation of PowerShell 7.6.6 LTS as a development and test dependency on Linux, for running the OSAT managers' PowerShell alias tests."
dc:publisher: "UniversalCake"
dcterms:created: "2026-09-29"
dcterms:modified: "2026-09-29"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:relation: "universal-cake-evaluation-metrics"
dc:identifier: "evaluation--powershell-7"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / UniversalCake.
  SPDX-License-Identifier: AGPL-3.0-or-later
sat:uuid: ""
sat:version_at_creation: "0.4.0"
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-09-29"
    author: "Christopher Steel"
    notes: >
      Initial Assess-ring evaluation of PowerShell 7.6.6 LTS against
      Universal Cake Evaluation Metrics 0.5.1, scoped to use as a
      development and test dependency on Linux Mint 22.3.
---

# Evaluation: PowerShell 7 (pwsh)

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

This document evaluates PowerShell 7, the cross-platform PowerShell run as `pwsh`, against the Universal Cake Evaluation Metrics. The evaluation is scoped to one use: installing `pwsh` on a Linux development machine so that the OSAT managers' `.ps1` alias tests can run locally. It records the gate results, a scorecard, the findings that shaped the ratings, a recommendation for the radar, and the installation we propose for this use.

## Sources and Acknowledgements

The evaluation applies *Universal Cake Evaluation Metrics* version 0.5.1 <a name="apa-uc-metrics-citation"></a>([Steel, 2026](#apa-uc-metrics-reference)). Facts about PowerShell come from Microsoft's documentation and from files in the PowerShell repository, which were read directly on 2026-09-29. The evaluation was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope

We evaluate PowerShell 7.6.6, the current Long Term Servicing release, as the `linux-x64` release archive on Linux Mint 22.3. It is a development and test dependency only: it runs the environment-restore and alias tests for the `.ps1` aliases that the OSAT managers generate. It is not a runtime dependency of any manager, and nothing a manager installs for its users requires it.

PowerShell 7 is distinct from Windows PowerShell 5.1, which ships with Windows and is what most Windows users will run the aliases under. A pass under `pwsh` on Linux does not show that an alias works under Windows PowerShell 5.1, so testing on a Windows machine before each release remains necessary. This evaluation does not cover Windows PowerShell 5.1.

## Scorecard metadata

- Product or service name: PowerShell 7 (`pwsh`)
- Version evaluated: 7.6.6 LTS, `powershell-7.6.6-linux-x64.tar.gz`
- Evaluator: Christopher Steel, with AI assistance from Claude Opus 5.5 (Anthropic)
- Date: 2026-09-29
- Radar ring: Assess

## Gates

| Gate | Result | Evidence | Notes |
|---|---|---|---|
| Telemetry or content exposure that cannot be disabled | Pass, conditional | Claimed | Startup and session telemetry is on by default and can be disabled with `POWERSHELL_TELEMETRY_OPTOUT`; the daily update check can be disabled with `POWERSHELL_UPDATECHECK`. Both must be set before `pwsh` starts. The pass depends on setting them |
| A licence incompatible with the project | Pass | Verified | MIT licence, read from the repository; compatible with the GPL-3.0-or-later OSAT managers, and not redistributed by them in any case |
| Failure of the accessibility floor | Pass | Inferred | A text command line with no graphical interface; nothing in this use is unreachable with assistive technology |
| No exit | Pass | Inferred | Tests are plain-text `.ps1` files; removing `pwsh` means deleting one directory and one alias |

## Scorecard

| Metric area | Rating | Evidence | Notes |
|---|---|---|---|
| Accessibility, alternative interaction | Moderate | Inferred | Text only. Interactive editing through PSReadLine has had screen reader difficulties in the past; this use is non-interactive, so they do not arise here |
| Multilingual integration | Unknown | None | Localisation of messages and help on Linux was not examined |
| Economic and cognitive accessibility | Moderate | Verified, Inferred | Free. The release archive is 75.9 MB (verified), large for a test dependency, and the .NET runtime is heavier than a POSIX shell. The language is verbose but discoverable |
| Representation | Unknown | None | Not assessed |
| Compatibility | Strong | Claimed | Linux, macOS and Windows, on x64 and ARM64 |
| Resilience | Strong | Inferred | Works fully offline once installed. No licence server or account |
| Agency, sovereignty and privacy | Moderate | Verified, Claimed | MIT licence allows self-hosting, modification and forking. Telemetry is opt-out rather than opt-in, and a daily update check contacts Microsoft from interactive sessions |
| Agency, power-imbalance proxies | Strong | Inferred | Exit cost under one hour for this use. No terms of service or pricing for local use. Data portability is not applicable, since `pwsh` holds no user data here |
| Agency, interaction patterns | Moderate | Claimed | Defaults serve the vendor, telemetry on and update notices on, but both are documented and can be switched off |
| Environment | Moderate | Inferred | Each test run starts the .NET runtime, which costs more energy and memory than a POSIX shell. The cost is small at the scale of a test suite |
| Security | Moderate | Verified, Claimed | A published security policy routes reports to the Microsoft Security Response Center, which states a first response within 24 hours (verified). LTS releases receive security fixes (claimed). Each release publishes SHA-256 checksums (verified). Adding Microsoft's package repository to the system would widen trust further, which the proposed installation avoids |
| Longevity | Strong | Claimed | 7.6 LTS is supported to November 2028, backed by Microsoft |
| Content endurance | Strong | Inferred | Nothing is stored inside `pwsh`; test files are plain text |
| Exit and portability | Strong | Inferred | See the No exit gate |
| Adjustability and support | Moderate | Verified, Claimed | MIT leaves the fork-and-fix path open, but the codebase is large; documentation is extensive; paid support is Microsoft's, free support is community-based |
| Market position | Moderate | Verified, Inferred | The licence permits forking, but governance is corporate: Microsoft owns the repository, can dissolve or reform the PowerShell Committee, and appoints a Corporate Maintainer with a veto. Contributor concentration was not measured |
| Gates | Pass | Mixed | All four pass; the telemetry gate is conditional on opting out |

## Findings

### Telemetry and update checks

PowerShell 7 sends telemetry to Microsoft through Application Insights at startup and periodically during a session. Microsoft states that the data is anonymised, that it covers details such as the operating system and PowerShell version, and that IP addresses are never stored. Setting `POWERSHELL_TELEMETRY_OPTOUT` to `1` turns it off, but only if the variable is set before the process starts <a name="apa-ms-telemetry-citation"></a>([Microsoft, n.d.-a](#apa-ms-telemetry-reference)). A separate update check contacts Microsoft once a day and is controlled by `POWERSHELL_UPDATECHECK`, whose value `Off` disables it; it too must be set before PowerShell starts <a name="apa-ms-update-notifications-citation"></a>([Microsoft, n.d.-b](#apa-ms-update-notifications-reference)). The design of the update check excludes non-interactive sessions, which covers test runs <a name="apa-rfc0052-citation"></a>([Wang, 2019](#apa-rfc0052-reference)).

A long-standing issue report describes network contact continuing after opting out. The discussion attributes it to components other than PowerShell itself, without a conclusive resolution <a name="apa-ps-issue-10005-citation"></a>([PowerShell Project, n.d.-d](#apa-ps-issue-10005-reference)). We therefore tag the telemetry gate as Claimed rather than Verified. Moving it to Verified requires watching the network while `pwsh` runs the tests with both variables set.

Because both settings must exist before the process starts, a profile script cannot set them. The proposed installation sets them in the alias that starts `pwsh`, so every run is covered.

### Governance and market position

The repository's governance document is explicit about where control rests. Microsoft is the Corporation that owns the repository; it can dissolve or reform the PowerShell Committee and the project leads, and it appoints a Corporate Maintainer who can veto decisions of the Committee <a name="apa-ps-governance-citation"></a>([PowerShell Project, n.d.-c](#apa-ps-governance-reference)). The MIT licence means the code could be forked if that control were used against the community <a name="apa-ps-license-citation"></a>([PowerShell Project, n.d.-a](#apa-ps-license-reference)), but the surrounding ecosystem, including the module gallery and the documentation, is Microsoft's. For a test dependency this matters little; it would matter more if the collection came to depend on PowerShell at run time.

### Supply chain and installation

Microsoft publishes Linux packages through its own package repository, a snap, and a release archive. Adding the package repository gives Microsoft's signing key authority over system packages installed with administrator rights, a wider trust grant than this use needs. The release archive installs in user space with no elevation, matching how the OSAT managers themselves work.

Each release publishes a `hashes.sha256` file. We found it is encoded as UTF-16 with Windows line endings, so `sha256sum --check` cannot read it directly; it has to be converted first. The archive for 7.6.6 is 75,876,271 bytes <a name="apa-ps-release-citation"></a>([PowerShell Project, 2026](#apa-ps-release-reference)).

The report policy is Microsoft's standard one, with vulnerabilities reported privately to the Microsoft Security Response Center rather than as public issues <a name="apa-ps-security-citation"></a>([PowerShell Project, n.d.-b](#apa-ps-security-reference)).

### Lifecycle

PowerShell follows the support lifecycle of the .NET version it is built on <a name="apa-ms-lifecycle-citation"></a>([Microsoft, 2026](#apa-ms-lifecycle-reference)). 7.6 is built on .NET 10 and, as of August 2026, 7.6.6 is the current LTS release, supported to 14 November 2028; 7.4 and 7.5 both reach end of support on 10 November 2026 <a name="apa-windowsforum-citation"></a>([Windows Forum, 2026](#apa-windowsforum-reference)). Only the latest update within a release line is supported, so the pinned version in the installation below needs refreshing when new 7.6 updates are published.

## Recommendation

Move PowerShell 7 from Assess to Trial as a development and test dependency, on these conditions:

- Install from the release archive in user space, with the published checksum verified, rather than from Microsoft's package repository.
- Set `POWERSHELL_TELEMETRY_OPTOUT=1` and `POWERSHELL_UPDATECHECK=Off` in the alias that starts `pwsh`, so they apply before every run.
- Keep `pwsh` out of every manager's run-time dependencies.
- Keep testing the `.ps1` aliases under Windows PowerShell 5.1 on a Windows machine before each release.

Moving the telemetry gate from Claimed to Verified, by watching the network during a test run, is a Trial-ring requirement and should be done during the trial.

## Installation for this use

This script installs PowerShell 7.6.6 for the current user, verifies the archive against the published checksum, and writes a `pwsh` alias to `~/.local/bin` that disables telemetry and the update check. It needs `curl`, `iconv`, `sha256sum` and `tar`, all present on Linux Mint 22.3, and runs without administrator rights.

```sh
#!/bin/sh
# Install PowerShell 7.6.6 (linux-x64) in user space for running .ps1 tests.
set -eu

PWSH_VERSION="7.6.6"
PWSH_ARCHIVE="powershell-${PWSH_VERSION}-linux-x64.tar.gz"
PWSH_BASE_URL="https://github.com/PowerShell/PowerShell/releases/download/v${PWSH_VERSION}"
PWSH_ROOT="${XDG_DATA_HOME:-$HOME/.local/share}/pwsh/${PWSH_VERSION}"
PWSH_ALIAS="$HOME/.local/bin/pwsh"

workdir="$(mktemp -d)"
trap 'rm -rf "$workdir"' EXIT

curl -fsSL -o "$workdir/$PWSH_ARCHIVE" "$PWSH_BASE_URL/$PWSH_ARCHIVE"
curl -fsSL -o "$workdir/hashes.sha256" "$PWSH_BASE_URL/hashes.sha256"

# hashes.sha256 is UTF-16 with CRLF line endings; convert before reading.
expected="$(iconv -f UTF-16 -t UTF-8 "$workdir/hashes.sha256" \
  | tr -d '\r' \
  | awk -v name="*$PWSH_ARCHIVE" '$2 == name { print $1 }')"
if [ -z "$expected" ]; then
  echo "No checksum found for $PWSH_ARCHIVE" >&2
  exit 1
fi
echo "$expected  $workdir/$PWSH_ARCHIVE" | sha256sum --check --strict

mkdir -p "$PWSH_ROOT" "$(dirname "$PWSH_ALIAS")"
chmod 700 "$PWSH_ROOT"
tar -xzf "$workdir/$PWSH_ARCHIVE" -C "$PWSH_ROOT"
chmod 755 "$PWSH_ROOT/pwsh"

cat > "$PWSH_ALIAS" <<EOF
#!/bin/sh
# pwsh ${PWSH_VERSION}, installed for running .ps1 tests.
# Telemetry and the update check must be disabled before pwsh starts.
export POWERSHELL_TELEMETRY_OPTOUT=1
export POWERSHELL_UPDATECHECK=Off
exec "$PWSH_ROOT/pwsh" "\$@"
EOF
chmod 755 "$PWSH_ALIAS"

"$PWSH_ALIAS" -NoProfile -Command '$PSVersionTable.PSVersion.ToString()'
```

The last line prints `7.6.6` when the installation works. To remove it, delete `~/.local/share/pwsh/` and `~/.local/bin/pwsh`.

## License

This document, *Evaluation: PowerShell 7 (pwsh)*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [GNU Affero General Public License v3.0 or later](https://www.gnu.org/licenses/agpl-3.0.html).

## Resources

### Evaluation framework

- [Universal Cake Evaluation Metrics](#apa-uc-metrics-reference)

### PowerShell documentation

- [PowerShell support lifecycle](#apa-ms-lifecycle-reference)
- [about_Telemetry](#apa-ms-telemetry-reference)
- [about_Update_Notifications](#apa-ms-update-notifications-reference)
- [RFC0052: Notification on PowerShell version updates](#apa-rfc0052-reference)

### PowerShell repository

- [Licence](#apa-ps-license-reference)
- [Security policy](#apa-ps-security-reference)
- [Governance](#apa-ps-governance-reference)
- [Issue 10005: telemetry opt-out](#apa-ps-issue-10005-reference)
- [Release v7.6.6](#apa-ps-release-reference)

### Lifecycle reporting

- [PowerShell 7.4 support ends November 10, 2026](#apa-windowsforum-reference)

## References

<a name="apa-ms-telemetry-reference"></a>Microsoft. (n.d.-a). *about_Telemetry*. Microsoft Learn. https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_telemetry?view=powershell-7.5
[Return to citation](#apa-ms-telemetry-citation)

<a name="apa-ms-update-notifications-reference"></a>Microsoft. (n.d.-b). *about_Update_Notifications*. Microsoft Learn. https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7.3
[Return to citation](#apa-ms-update-notifications-citation)

<a name="apa-ms-lifecycle-reference"></a>Microsoft. (2026). *PowerShell support lifecycle*. Microsoft Learn. https://learn.microsoft.com/en-us/powershell/scripting/install/powershell-support-lifecycle?view=powershell-7.6
[Return to citation](#apa-ms-lifecycle-citation)

<a name="apa-ps-license-reference"></a>PowerShell Project. (n.d.-a). *LICENSE.txt* [Software licence]. GitHub. https://github.com/PowerShell/PowerShell/blob/master/LICENSE.txt
[Return to citation](#apa-ps-license-citation)

<a name="apa-ps-security-reference"></a>PowerShell Project. (n.d.-b). *SECURITY.md* [Security policy]. GitHub. https://github.com/PowerShell/PowerShell/blob/master/.github/SECURITY.md
[Return to citation](#apa-ps-security-citation)

<a name="apa-ps-governance-reference"></a>PowerShell Project. (n.d.-c). *PowerShell governance*. GitHub. https://github.com/PowerShell/PowerShell/blob/master/docs/community/governance.md
[Return to citation](#apa-ps-governance-citation)

<a name="apa-ps-issue-10005-reference"></a>PowerShell Project. (n.d.-d). *PowerShell ignoring telemetry opt-out options, tries to call home regularly* (Issue No. 10005) [GitHub issue]. GitHub. https://github.com/PowerShell/PowerShell/issues/10005
[Return to citation](#apa-ps-issue-10005-citation)

<a name="apa-ps-release-reference"></a>PowerShell Project. (2026). *PowerShell v7.6.6* [Software release]. GitHub. https://github.com/PowerShell/PowerShell/releases/tag/v7.6.6
[Return to citation](#apa-ps-release-citation)

<a name="apa-uc-metrics-reference"></a>Steel, C. (2026). *Universal Cake evaluation metrics* (Version 0.5.1) [Unpublished manuscript]. UniversalCake.
[Return to citation](#apa-uc-metrics-citation)

<a name="apa-rfc0052-reference"></a>Wang, D. (2019). *RFC0052: Notification on PowerShell version updates*. PowerShell RFC repository, GitHub. https://github.com/PowerShell/PowerShell-RFC/pull/162/files
[Return to citation](#apa-rfc0052-citation)

<a name="apa-windowsforum-reference"></a>Windows Forum. (2026). *PowerShell 7.4 support ends November 10, 2026: Move to 7.6 LTS*. https://windowsforum.com/news/powershell-7-4-support-ends-november-10-2026-move-to-7-6-lts.445667/
[Return to citation](#apa-windowsforum-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft | Initial Assess-ring evaluation of PowerShell 7.6.6 LTS against Universal Cake Evaluation Metrics 0.5.1, scoped to use as a development and test dependency on Linux Mint 22.3 |

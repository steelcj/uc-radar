---
dc:title: "Windows Recall: Security Analysis, 2024 to 2026"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "security"
  - "privacy"
  - "windows"
  - "recall"
  - "threat model"
dc:description: "A documented security analysis of Microsoft Windows Recall across its versions, from the 2024 preview to the redesigned release and the 2026 findings, with a threat model, findings by version, residual risks, and recommendations."
dc:publisher: "Vishpala"
dcterms:created: "2026-10-22"
dcterms:modified: "2026-10-22"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "windows-recall--what-it-is-and-what-happened"
dc:identifier: "windows-recall--security-analysis-2024-to-2026"
dcterms:rightsHolder: "Christopher Steel"
dc:rights: >
  Copyright 2026 Christopher Steel / Vishpala.
  SPDX-License-Identifier: CC-BY-4.0
sat:uuid: ""
sat:migration_status: pre-sat
sat:changelog:
  - version: "0.1.0"
    date: "2026-10-22"
    author: "Claude Opus 5.5 (Anthropic), drafted for Christopher Steel's review"
    notes: >
      VERSION NUMBER PROVISIONAL, pending Christopher's confirmation.
      First version, a document review of public sources up to
      2026-10-22. No hands-on testing of Recall.
---

# Windows Recall: Security Analysis, 2024 to 2026

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists

## Abstract

Windows Recall records periodic snapshots of a person's screen, analyses them on the device, and makes the whole history searchable. This document analyses its security across three stages: the 2024 preview that Microsoft withdrew before release, the redesigned version released in April 2025, and that version as examined by researchers in 2026. The 2024 preview kept its history as an unencrypted database and image folder that any program running as the user could copy. The redesign moved keys and decryption into a hardware-isolated enclave gated by Windows Hello, which defeats theft of the disk, other local accounts, and offline attacks. Research published in April 2026 showed that the redesign still hands decrypted content to an unprotected process once the user unlocks Recall, so malware running as that user can read the whole history without administrator rights. Microsoft considers this within Recall's documented design. The analysis closes with residual risks and recommendations for individuals, organisations, and application developers.

## Sources and acknowledgements

This analysis is a review of public documents, not a test of Recall: we had no Copilot+ PC. It relies on Microsoft's own announcements, architecture description and administrator documentation; on the published research of Kevin Beaumont and Alexander Hagenah, including Hagenah's source code and write-up, read directly from his repository on 2026-10-22; on independent press testing; and on statements by Signal and Brave about the countermeasures they shipped. Where a finding rests on one party's word, it is marked as that party's claim. The analysis was prepared with AI assistance from Claude Opus 5.5 (Anthropic).

## Scope and method

The subject is Recall as shipped to, or prepared for, Copilot+ PCs running Windows 11. Click to Do, which acts on a snapshot's contents, is mentioned only where it shares Recall's data. Project Pantheon, a separate memory app Microsoft is reportedly testing, is out of scope except as a note on direction.

Claims are graded the same way as in the Universal Cake Evaluation Metrics:

- **Verified**, confirmed by a party's direct testing, inspection, or published code, with the method stated by that party.
- **Inferred**, a reasonable conclusion from documentation or architecture, not tested by anyone we can cite.
- **Claimed**, asserted by a vendor or researcher without independent confirmation.

We read Hagenah's 2026 source code and documentation but did not build or run it.

## What Recall collects

Recall saves a snapshot of the screen whenever its content changes. Microsoft's first public description, quoted by Hagenah, put the interval at every five seconds while the screen differs from the previous snapshot <a name="apa-hagenah-2024-citation"></a>([Hagenah, 2024](#apa-hagenah-2024-reference)). Each snapshot is run through text recognition and image analysis on the device, and the results feed a searchable timeline <a name="apa-ms-manage-recall-citation"></a>([Microsoft, 2025b](#apa-ms-manage-recall-reference)).

The stored record is richer than screenshots alone. From Recall's private interface metadata and the database schema embedded in its enclave binary, Hagenah lists, per snapshot: the full-resolution image; the recognised text, down to each word's position; the window title, application, executable path, browser URL and open file path; timestamps; and time spent per application and per website. Recall also derives named entities such as people, organisations and email addresses, topic labels, screen regions, short activity descriptions, and image and text embeddings for similarity search. Hagenah notes that the field names are confirmed from the binaries but that not every field's contents was checked at runtime <a name="apa-hagenah-2026-citation"></a>([Hagenah, 2026](#apa-hagenah-2026-reference)).

In security terms, Recall is a local, continuously updated, indexed record of everything the user has seen, including passwords shown on screen, private messages from other people, medical and financial records, and work documents. Its value to an attacker is the sum of all of that, already searchable.

## Versions examined

| Stage | Dates | State | Key sources |
|---|---|---|---|
| Announced preview | Announced 2024-05-20; due 2024-06-18; withdrawn | Preview builds circulated; never shipped to the public | <a name="apa-mehdi-citation"></a>[Mehdi (2024)](#apa-mehdi-reference), <a name="apa-beaumont-citation"></a>[Beaumont (2024)](#apa-beaumont-reference) |
| Redesign announced | 2024-06-07 and 2024-06-13; architecture 2024-09-27 | Opt-in, Windows Hello, encryption; moved to Windows Insiders | <a name="apa-ms-june-update-citation"></a>[Microsoft (2024a)](#apa-ms-june-update-reference), <a name="apa-weston-citation"></a>[Weston (2024)](#apa-weston-reference) |
| Insider builds | 2024-11-22 (Snapdragon, build 26120.2415); 2024-12-06 (AMD and Intel, build 26120.2510) | First public builds of the redesign | <a name="apa-ms-insider-citation"></a>[Microsoft (2024b)](#apa-ms-insider-reference) |
| General release | From 2025-04-25, as a preview feature, in a gradual rollout | Current design | <a name="apa-ms-release-citation"></a>[Microsoft (2025a)](#apa-ms-release-reference) |
| Current, as researched | Windows 11 25H2, build 26300.8155, ARM64, tested 2026 | Same design, with new research findings | [Hagenah (2026)](#apa-hagenah-2026-reference) |

Microsoft's administrator documentation, last updated 2025-12-10, still labels Recall a preview ([Microsoft, 2025b](#apa-ms-manage-recall-reference)).

## Threat model

The analysis considers these adversaries, each with a different starting position:

- **Thief or finder**: has the device or its disk, but not the user's sign-in.
- **Other local account**: a second user, or a guest account, on the same device.
- **Same-user malware**: code running as the signed-in user at standard privilege, for example an infostealer opened from an email attachment. This is the most common real-world compromise of a personal computer.
- **Administrator-level malware**: code that has gained elevation on the device.
- **Person who knows the sign-in**: someone who knows the user's PIN or can make them unlock the device, such as a controlling partner or family member, or anyone with the device unlocked in front of them.
- **The people on screen**: not attackers, but third parties whose messages, faces and documents appear in another person's snapshots without their say.

## The 2024 preview

### Design

The preview stored its history in an SQLite database, `ukg.db`, and an `ImageStore` folder of screenshots under the user's profile, in `C:\Users\<user>\AppData\Local\CoreAIPlatform.00\UKP\{GUID}` ([Hagenah, 2024](#apa-hagenah-2024-reference)). Neither the database nor the images were encrypted beyond whole-disk encryption. Setup turned Recall on unless the user opted out, and on enterprise devices it was on unless disabled by policy ([Beaumont, 2024](#apa-beaumont-reference)).

### Findings

- **Same-user access without elevation (Verified, Beaumont).** Beaumont, testing a preview build, found the database readable by processes running as the user, without administrator rights, despite Microsoft's rollout materials suggesting otherwise. He also demonstrated another account on the same device reading it ([Beaumont, 2024](#apa-beaumont-reference)).
- **Trivial extraction (Verified, Hagenah).** Hagenah's TotalRecall, released in early June 2024, copied the database and images for a chosen date range and searched the recognised text; it described the work as basic SQLite parsing ([Hagenah, 2024](#apa-hagenah-2024-reference)). Malwarebytes reported Hagenah's figure of about two seconds to extract a day of data <a name="apa-arntz-citation"></a>([Arntz, 2024](#apa-arntz-reference)).
- **Fast exfiltration and weak detection (Verified, Beaumont).** Beaumont measured several days of recognised text at about 90 KB compressed, so months of history could leave over an ordinary connection in seconds. When he ran an off-the-shelf infostealer adapted to collect Recall data, Microsoft Defender flagged it, but automated remediation took over ten minutes, by which time the data was gone ([Beaumont, 2024](#apa-beaumont-reference)).
- **Disk encryption did not help.** Whole-disk encryption protects a powered-off or stolen device. Once the user signs in, the disk is decrypted for every program running as that user, which is the position malware occupies (Inferred, from the design; Beaumont makes the same point).

### Assessment

Against same-user malware, the 2024 preview offered no protection at all, and it collected far more than the credentials infostealers already target. Against other local accounts it also failed, by Beaumont's demonstration. Only the thief without a sign-in was stopped, and only when disk encryption was on. Microsoft withdrew it: on 2024-06-13 it announced that Recall would not ship on 2024-06-18 and would go first to the Windows Insider Program ([Microsoft, 2024a](#apa-ms-june-update-reference)).

## The redesign

### Design, as described by Microsoft

Microsoft announced a first set of changes on 2024-06-07: an explicit opt-in during setup, Windows Hello enrolment to enable Recall, proof of presence to view the timeline or search, decryption only after Windows Hello authentication, and an encrypted search index ([Microsoft, 2024a](#apa-ms-june-update-reference)). On 2024-09-27, David Weston, Microsoft's Corporate Vice President for Enterprise and OS Security, described the architecture ([Weston, 2024](#apa-weston-reference)):

- **VBS Enclave.** The services that process snapshots, decrypt them, and query the semantic index run inside a virtualisation-based security enclave, isolated from the normal Windows kernel and from administrators. The snapshot service and the Recall user interface run outside it.
- **Keys.** Encryption keys are protected by the TPM, bound to the user's Windows Hello Enhanced Sign-in Security identity, and usable only inside the enclave. Each snapshot has its own key.
- **Authorisation limits.** Authorisation times out, and anti-hammering, concurrency protection and monotonic counters limit how fast requests can be made. Microsoft presented these as restricting malware that tries to ride along with a user's authentication.
- **Opt-in and removal.** Recall is off unless the user opts in, and it can be removed through optional Windows features.
- **Review.** Microsoft's Offensive Research and Security Engineering team spent months on design review and penetration testing, and an unnamed third-party vendor carried out an independent design review and penetration test.

Microsoft's administrator documentation adds the deployment rules ([Microsoft, 2025b](#apa-ms-manage-recall-reference)):

- **Hardware.** A Copilot+ PC meeting the Secured-core standard, with a 40 TOPS NPU, 16 GB of memory, 8 logical processors, 256 GB of storage, BitLocker or Device Encryption, and Windows Hello Enhanced Sign-in Security with at least one biometric sign-in method.
- **Defaults.** On managed commercial devices, Recall is disabled and removed by default, and an administrator can make it available but cannot turn on snapshot saving for a user. On unmanaged Copilot+ PCs it is installed and available, but each user must opt in to saving snapshots.
- **Filtering.** Private browsing windows in supported browsers, DRM-protected content, and several remote desktop clients are skipped by default. Sensitive-information filtering, on by default, uses on-device classification to skip snapshots that appear to contain items such as card numbers. Administrators can deny specific apps and websites, and can apply a Microsoft Purview data loss prevention provider.
- **Retention.** Storage is capped by default at 25 GB on a 256 GB device, 75 GB on 512 GB and 150 GB on 1 TB or more; Enterprise and Education editions can also cap retention at 30 to 180 days.
- **Export.** An encrypted export exists only in the European Economic Area and is off by default.
- **Removal.** The policy `AllowRecallEnablement`, when disabled, removes the component and deletes existing snapshots, and `DisableAIDataAnalysis` turns snapshot saving off and deletes existing snapshots.

### What the redesign fixed

Compared with the 2024 preview, the redesign defeats the thief or finder even with the device's disk in hand, since keys are sealed to the TPM and the user's Windows Hello identity rather than relying on disk encryption alone. It defeats other local accounts, whose Windows Hello identity cannot release another user's keys. It removes the plain database file that any same-user program could copy, so the 2024 TotalRecall approach no longer works. And it replaces opt-out with opt-in. These conclusions follow from Microsoft's design and are consistent with Hagenah's 2026 analysis, which describes the enclave, the key hierarchy and the per-page AES-256-GCM encryption as sound ([Hagenah, 2026](#apa-hagenah-2026-reference)).

### Sensitive-information filtering in practice

Filtering is a privacy control, not a security boundary, and it has been unreliable in testing. In December 2024, testing an Insider build, Avram Piltch of Tom's Hardware found that Recall saved a card number, username and password typed into Notepad, and a name, date of birth and Social Security number entered in a loan application PDF in Microsoft Edge; the filter worked on only two of the shopping sites he tried <a name="apa-younker-citation"></a>([Younker, 2024](#apa-younker-reference)). Microsoft responded by pointing to its documentation and asking users to report misses. We found no later independent test of the released filter, so its current accuracy is Unknown.

### Countermeasures by application developers

Recall offers no general way for an application to say that its window should not be captured, beyond the exemptions Microsoft built in. Developers have reached for blunter tools:

- **Signal** made a Screen security setting the default on Windows 11 in May 2025. It sets the DRM flag on the app's window, so Recall and every other screenshot tool see a blank window. Signal acknowledged that this can stop screen readers and magnifiers from working with the app, and asked Microsoft for proper controls that let developers keep operating-system AI features away from sensitive content <a name="apa-lund-citation"></a>([Lund, 2025](#apa-lund-reference)).
- **Brave** began telling Windows that every tab is private in July 2025, from version 1.81, so Recall skips the browser entirely. Users can turn this off. Brave cited the sensitivity of browsing history and the risk that a persistent record poses in situations such as intimate partner violence <a name="apa-brave-citation"></a>([Brave, 2025](#apa-brave-reference)).

The accessibility cost of Signal's approach is a direct conflict between two goods: protecting other people's messages, and access for blind and low-vision users. It exists only because the platform provides no finer-grained control.

## The redesign under research, 2026

### Findings

In April 2026, Hagenah published TotalRecall Reloaded, with source code, after reporting it to the Microsoft Security Response Center. He tested Windows 11 25H2, build 26300.8155, on ARM64, as a standard user at medium integrity with no elevation ([Hagenah, 2026](#apa-hagenah-2026-reference)). His summary of the problem: "The vault is solid. The delivery truck is not." The findings below are his; their status is described after the list.

- **Decrypted data reaches an unprotected process.** `aihost.exe`, the broker to the enclave, runs as a Protected Process Light and resists injection. `AIXHost.exe`, which renders the timeline, has no such protection, no AppContainer, and no code integrity enforcement. After the user authenticates with Windows Hello, decrypted screenshots, recognised text and metadata flow through it as live COM objects. Any process running as the same user can inject code into it with standard techniques and call the same interfaces the Recall interface uses.
- **Riding along is possible after all.** The tool can open Recall with a simulated Win+J so that the user sees a normal Windows Hello prompt, or wait for the user to open Recall on their own, and extract everything once they authenticate. A stealth mode patches out the call that revokes access when the user closes Recall, so the grant survives and extraction continues with no visible window. Hagenah reports the authorisation state persisting in `aihost.exe` for the rest of the Windows session.
- **A missing access check.** Two interfaces for reading search results return a Recall-specific access-denied error before authorisation, but a newer version of the same interface, `IResponse4.get_UnfilledItems()`, returns the same results with no check.
- **Actions without Windows Hello.** Without any authentication, same-user code can read the most recent snapshot at full resolution through a method meant for a taskbar thumbnail, whose size it does not cap; delete the entire capture history; and read metadata such as the storage path, size, retention setting and capture count. Hagenah found these after the Microsoft case closed.

Hagenah's account of the review is that the enclave, the key hierarchy and Protected Process Light all held: he found the cryptography sound, injection into `aihost.exe` blocked by the kernel, its database queries fully parameterised, and its authentication free of races in thousands of probes. The weakness is architectural: decrypted content leaves the protected boundary for rendering.

### Status of the findings

- **Evidence.** The findings are published with source code and build instructions, which makes them reproducible, and Beaumont, who tested Recall separately, reported to CSO Online that a user process could read the data and that no antivirus or endpoint detection alerts were triggered <a name="apa-swain-citation"></a>([Swain, 2026](#apa-swain-reference)). We rate the main finding, extraction by same-user code after authentication, Verified by its author and corroborated in part. The pre-authentication findings rest on Hagenah's write-up alone and are Claimed.
- **Microsoft's position.** The Microsoft Security Response Center closed Hagenah's report on 2026-04-03 as not a vulnerability, finding that the behaviour operates within Recall's documented security design ([Hagenah, 2026](#apa-hagenah-2026-reference)). A Microsoft spokesperson told CSO Online the behaviour was consistent with intended protections and existing controls, and pointed to anti-hammering and the authorisation timeout ([Swain, 2026](#apa-swain-reference)); David Weston gave The Verge a statement to the same effect, as reported by Pureinfotech <a name="apa-huculak-b-citation"></a>([Huculak, 2026b](#apa-huculak-b-reference)).
- **The disagreement.** Microsoft's 2024 architecture description said processes outside the enclave receive only what the enclave returns after authorisation, and that authorisation limits restrict ride-along malware ([Weston, 2024](#apa-weston-reference)). Both statements remain true as written: `AIXHost.exe` receives data only after authorisation. What the research shows is that, once authorisation happens, the boundary that matters to an attacker running as the user is `AIXHost.exe`, and it is not protected. Microsoft has, in effect, defined same-user code during an authorised session as outside Recall's threat model.

We found no report, up to 2026-10-22, of Microsoft changing `AIXHost.exe` in response.

## Assessment by threat

| Adversary | 2024 preview | Current design |
|---|---|---|
| Thief or finder, no sign-in | Protected only if disk encryption is on | Protected. Keys are sealed to the TPM and the user's Windows Hello identity (Inferred from design, consistent with research) |
| Other local account | Exposed. Beaumont read another user's database (Verified) | Protected. Another account cannot release the user's keys (Inferred from design) |
| Same-user malware, before the user opens Recall | Exposed. The whole history could be copied in seconds (Verified) | Partly exposed. History is encrypted, but the latest snapshot, metadata and deletion are reachable without authentication (Claimed, Hagenah) |
| Same-user malware, after the user opens Recall | Exposed (Verified) | Exposed. The whole history can be extracted, silently, for the rest of the session, without elevation (Verified by Hagenah, corroborated in part by Beaumont) |
| Administrator-level malware | Exposed (Verified) | Not separately tested in the sources reviewed. Microsoft claims isolation from administrators; administrator-level code can at least do whatever same-user code can (Inferred) |
| Person who knows the sign-in | Exposed | Exposed. Recall accepts the user's Windows Hello methods, so anyone who can sign in as the user, or makes them do so, can search the full history (Inferred from documentation) |
| People on screen | No control | No control, except where an application uses screen capture protection or a DRM flag (Verified, Signal and Brave) |

## Residual risks

- **The record is the risk.** Even with sound encryption, Recall concentrates months of a person's activity, including other people's private content, into one indexed store on a device that is routinely compromised by commodity malware. Infostealers already collect browser data; Recall offers them a far richer target, and the 2026 research shows the protection against them depends on the user never opening Recall while malware is present.
- **Coercion and shared households.** Windows Hello stops strangers but not a person who knows the PIN or can compel an unlock. A searchable history of a partner's messages, searches and locations is a powerful tool for control, which is the concern Brave named.
- **Filtering is best effort.** Sensitive-information filtering has missed card numbers and identity numbers in independent testing, and its current accuracy is unknown. It should not be relied on to keep secrets out of the store.
- **Third parties cannot opt out.** People who send messages or appear in calls have no say over whether their content is captured on someone else's device. The only remedy available to a messaging app is to block screen capture entirely, at a cost to accessibility.
- **Legal and forensic exposure.** A searchable record of past activity can be sought in litigation or investigations, or examined at a border, independent of any attacker. This follows from the data's existence (Inferred).
- **Vendor posture.** Microsoft has declined to treat the 2026 findings as a vulnerability, so no fix should be expected under the security update process. In January 2026, Windows Central reported, from unnamed sources, that Microsoft considered Recall in its current form to have failed and was reviewing it, possibly under a different name <a name="apa-bowden-citation"></a>([Bowden, 2026](#apa-bowden-reference)). In October 2026, Pureinfotech reported that Microsoft is testing a separate memory app, Project Pantheon, which saves only what the user chooses; Microsoft has not announced it <a name="apa-huculak-a-citation"></a>([Huculak, 2026a](#apa-huculak-a-reference)). Both are reports, not commitments.

## Recommendations

### For individuals

- **Check whether it applies.** Recall runs only on Copilot+ PCs that meet the hardware requirements above. Older computers cannot run it.
- **On a Copilot+ PC, decide deliberately.** If you do not need Recall, do not opt in, or remove it. Removing it takes the component off the device rather than merely turning off saving. In PowerShell run as administrator:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "Recall" -Remove
```

- **If you use it,** exclude password managers, banking, health and messaging apps and sites; treat opening Recall as exposing its whole history to anything already running on the computer; and do not open it on a device you suspect is compromised.
- **On a shared or monitored device,** assume anyone who can sign in as you can read your history.

### For organisations

- **Keep the default.** Recall is removed by default on managed commercial devices. Make that explicit by disabling `AllowRecallEnablement` through policy, which also removes the component and deletes any existing snapshots ([Microsoft, 2025b](#apa-ms-manage-recall-reference)).
- **If a group needs it,** limit it to named users, set deny lists for sensitive applications and websites, set the shortest workable retention, and pair it with data loss prevention where available.
- **Ask endpoint detection vendors** whether they detect injection into `AIXHost.exe`, since Beaumont reported no alerts during testing ([Swain, 2026](#apa-swain-reference)).
- **Consider bring-your-own-device policy.** Personal Copilot+ PCs used for work may hold work content in Recall outside the organisation's control.

### For application developers

- **Protect sensitive windows** with Windows screen capture protection where the application's users expect it, as Signal does, but weigh the accessibility cost and offer a setting.
- **Ask Microsoft for a capture-exclusion signal** that hides a window from Recall without hiding it from assistive technology, as Signal has requested ([Lund, 2025](#apa-lund-reference)).

## Relation to the Universal Cake Evaluation Metrics

Read against the Universal Cake metrics, the current Recall would likely pass the telemetry gate, since Microsoft states that snapshots stay on the device and saving is opt-in. It would rate Weak under Security on the 2026 findings, which a project handling sensitive data could make a project-specific gate. The Community lens raises the largest concern, since the people whose content is captured are not the people who consented. The 2024 preview would have failed a reasonable security gate outright. This is an outline only; a full evaluation would follow the uc-radar template.

## Limitations

- No hands-on testing. All findings come from published sources, and the 2026 findings were reproduced by their author on ARM64 only.
- The sources reviewed do not test administrator-level attacks on the current design, or the current accuracy of sensitive-information filtering.
- Some events are known only from press reports, which are marked as such.

## License

This document, *Windows Recall: Security Analysis, 2024 to 2026*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

## Resources

### Microsoft

- [Introducing Copilot+ PCs](#apa-mehdi-reference)
- [Update on the Recall preview feature](#apa-ms-june-update-reference)
- [Update on Recall security and privacy architecture](#apa-weston-reference)
- [Previewing Recall with Windows Insiders](#apa-ms-insider-reference)
- [Recall general release](#apa-ms-release-reference)
- [Manage Recall for Windows clients](#apa-ms-manage-recall-reference)

### Security research

- [Kevin Beaumont on the 2024 preview](#apa-beaumont-reference)
- [TotalRecall, 2024](#apa-hagenah-2024-reference)
- [TotalRecall Reloaded, 2026](#apa-hagenah-2026-reference)
- [Malwarebytes on TotalRecall](#apa-arntz-reference)

### Testing and reporting

- [Sensitive-information filter test](#apa-younker-reference)
- [CSO Online on the 2026 findings](#apa-swain-reference)
- [Microsoft's response to the 2026 findings](#apa-huculak-b-reference)
- [Reported review of Recall](#apa-bowden-reference)
- [Reported Project Pantheon](#apa-huculak-a-reference)

### Countermeasures

- [Signal screen security](#apa-lund-reference)
- [Brave blocks Recall](#apa-brave-reference)

## References

<a name="apa-arntz-reference"></a>Arntz, P. (2024, June 6). *Microsoft Recall snapshots can be easily grabbed with TotalRecall tool*. Malwarebytes. https://www.malwarebytes.com/fr/blog/news/2024/06/microsoft-recall-snapshots-can-be-easily-grabbed-with-totalrecall-tool
[Return to citation](#apa-arntz-citation)

<a name="apa-beaumont-reference"></a>Beaumont, K. (2024, May 31). *Recall: Stealing everything you've ever typed or viewed on your own Windows PC is now possible*. DoublePulsar. https://doublepulsar.com/recall-stealing-everything-youve-ever-typed-or-viewed-on-your-own-windows-pc-is-now-possible-da3e12e9465e
[Return to citation](#apa-beaumont-citation)

<a name="apa-bowden-reference"></a>Bowden, Z. (2026, January 30). *You won: Microsoft is walking back Windows 11's AI overload, scaling down Copilot and rethinking Recall in a major shift*. Windows Central. https://www.windowscentral.com/microsoft/windows-11/microsoft-is-reevaluating-its-ai-efforts-on-windows-11-plans-to-reduce-copilot-integrations-and-evolve-recall
[Return to citation](#apa-bowden-citation)

<a name="apa-brave-reference"></a>Brave. (2025, July 22). *Brave blocks Microsoft Recall by default*. https://brave.com/privacy-updates/35-block-recall/
[Return to citation](#apa-brave-citation)

<a name="apa-hagenah-2024-reference"></a>Hagenah, A. (2024). *TotalRecall* [Computer software and documentation]. GitHub. https://github.com/xaitax/TotalRecall/tree/main/totalrecall_original_2024
[Return to citation](#apa-hagenah-2024-citation)

<a name="apa-hagenah-2026-reference"></a>Hagenah, A. (2026). *TotalRecall Reloaded* [Computer software and documentation]. GitHub. Retrieved October 22, 2026, from https://github.com/xaitax/TotalRecall
[Return to citation](#apa-hagenah-2026-citation)

<a name="apa-huculak-a-reference"></a>Huculak, M. (2026a, October 22). *Microsoft is testing Project Pantheon for Windows 11, and it raises questions about Recall*. Pureinfotech. https://pureinfotech.com/project-pantheon-windows-11-ai-memory/
[Return to citation](#apa-huculak-a-citation)

<a name="apa-huculak-b-reference"></a>Huculak, M. (2026b, April 15). *Windows 11 Recall tool exposes data risks, Microsoft insists there's no security flaw*. Pureinfotech. https://pureinfotech.com/windows-11-recall-data-extraction-tool-microsoft-not-vulnerability/
[Return to citation](#apa-huculak-b-citation)

<a name="apa-lund-reference"></a>Lund, J. (2025, May 21). *By default, Signal doesn't Recall*. Signal. https://signal.org/blog/signal-doesnt-recall/
[Return to citation](#apa-lund-citation)

<a name="apa-mehdi-reference"></a>Mehdi, Y. (2024, May 20). *Introducing Copilot+ PCs*. Official Microsoft Blog. https://blogs.microsoft.com/blog/2024/05/20/introducing-copilot-pcs/
[Return to citation](#apa-mehdi-citation)

<a name="apa-ms-june-update-reference"></a>Microsoft. (2024a, June 7). *Update on the Recall preview feature for Copilot+ PCs*. Windows Experience Blog. https://blogs.windows.com/windowsexperience/2024/06/07/update-on-the-recall-preview-feature-for-copilot-pcs/
[Return to citation](#apa-ms-june-update-citation)

<a name="apa-ms-insider-reference"></a>Microsoft. (2024b, November 22). *Previewing Recall with Click to Do on Copilot+ PCs with Windows Insiders*. Windows Insider Blog. https://blogs.windows.com/windows-insider/2024/11/22/previewing-recall-with-click-to-do-on-copilot-pcs-with-windows-insiders/
[Return to citation](#apa-ms-insider-citation)

<a name="apa-ms-release-reference"></a>Microsoft. (2025a, April 25). *Copilot+ PCs are the most performant Windows PCs ever built, now with more AI features that empower you every day*. Windows Experience Blog. https://blogs.windows.com/windowsexperience/2025/04/25/copilot-pcs-are-the-most-performant-windows-pcs-ever-built-now-with-more-ai-features-that-empower-you-every-day/
[Return to citation](#apa-ms-release-citation)

<a name="apa-ms-manage-recall-reference"></a>Microsoft. (2025b, December 10). *Manage Recall for Windows clients*. Microsoft Learn. https://learn.microsoft.com/windows/client-management/manage-recall
[Return to citation](#apa-ms-manage-recall-citation)

<a name="apa-swain-reference"></a>Swain, G. (2026, April 16). *Microsoft's Windows Recall still allows silent data extraction*. CSO Online. https://www.csoonline.com/article/4159643/microsofts-windows-recall-still-allows-silent-data-extraction.html
[Return to citation](#apa-swain-citation)

<a name="apa-weston-reference"></a>Weston, D. (2024, September 27). *Update on Recall security and privacy architecture*. Windows Experience Blog. https://blogs.windows.com/windowsexperience/2024/09/27/update-on-recall-security-and-privacy-architecture/
[Return to citation](#apa-weston-citation)

<a name="apa-younker-reference"></a>Younker, S. (2024, December 12). *Microsoft Recall caught capturing credit card and Social Security numbers despite reassurances it won't*. Tom's Guide. https://www.tomsguide.com/computing/windows-operating-systems/microsoft-recall-caught-capturing-credit-card-and-social-security-numbers-despite-reassurances-it-wont
[Return to citation](#apa-younker-citation)

## Changelog

| Version | Status | Notes |
|---------|--------|-------|
| 0.1.0 | Draft, version provisional | First version: document review of public sources up to 2026-10-22, covering the 2024 preview, the redesign, its release, and the 2026 research |

---
dc:title: "Windows Recall: What It Is and What Happened"
dcterms:version: "0.1.0"
dc:creator: "Christopher Steel"
dc:contributor: "Claude Opus 5.5 (Anthropic)"
dc:subject:
  - "privacy"
  - "security"
  - "windows"
  - "recall"
dc:description: "A plain-language article on Windows Recall, the Windows 11 feature that saves snapshots of your screen: what it does, what went wrong, what changed, and what you can do."
dc:publisher: "Vishpala"
dcterms:created: "2026-10-22"
dcterms:modified: "2026-10-22"
dc:type: "Text"
dc:format: "text/markdown"
dc:language: "en"
sat:language_bcp47: "en"
dc:source: ""
dc:relation: "windows-recall--security-analysis-2024-to-2026"
dc:identifier: "windows-recall--what-it-is-and-what-happened"
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
      First version, written for a general audience.
---

# Windows Recall: What It Is and What Happened

Version: 0.1.0
Status: Draft
Style Guide: style-guide--plain-language-for-general-audiences

## Abstract

You may have heard that Windows takes pictures of your screen. That is true for one feature, called Recall. This article explains what Recall does, what went wrong, what Microsoft changed, and what you can do. It is for anyone who uses a Windows computer, with no technical knowledge needed.

## Sources and acknowledgements

This article is based on Microsoft's own documents, on work by security researchers, and on news reports. A companion document, *Windows Recall: Security Analysis, 2024 to 2026*, gives the full technical detail and sources. The article was written with help from Claude Opus 5.5 (Anthropic).

## What Recall does

Recall is a feature of Windows 11. When it is on, it saves a picture of your screen whenever something on it changes. Microsoft calls these pictures snapshots.

Your computer then reads the words and images in each snapshot. This lets you search your past in plain words. You could type "the recipe I saw on Tuesday" and Recall would show you the moment you saw it <a name="apa-ms-manage-recall-citation"></a>([Microsoft, 2025b](#apa-ms-manage-recall-reference)).

That can be useful. But think about what passes across your screen in a week. Emails, bank pages, medical results, private messages, and passwords all appear there. Recall can keep all of it, in one place, ready to search.

## Do you have it?

Probably not. Recall only works on a new kind of computer called a Copilot+ PC. These computers have a special chip for artificial intelligence, called an NPU. They also need at least 16 GB of memory and a face or fingerprint sign-in ([Microsoft, 2025b](#apa-ms-manage-recall-reference)).

Most computers sold before mid-2024 cannot run Recall. Older computers are not affected at all. If you do have a Copilot+ PC, Recall does not save anything until you agree to turn it on.

## The first version

Microsoft announced Recall in May 2024 <a name="apa-mehdi-citation"></a>([Mehdi, 2024](#apa-mehdi-reference)). It planned to switch it on for new computers unless people said no.

Security researchers tried it before it came out. Kevin Beaumont found that Recall kept everything in a plain file that was not locked. Any program you ran could copy it <a name="apa-beaumont-citation"></a>([Beaumont, 2024](#apa-beaumont-reference)).

That matters because of a kind of harmful program called an infostealer. Infostealers sneak onto computers and copy passwords and other secrets. Beaumont showed that an infostealer could take months of Recall history in seconds. Another researcher, Alexander Hagenah, made a free tool that did the same <a name="apa-hagenah-2024-citation"></a>([Hagenah, 2024](#apa-hagenah-2024-reference)).

## What Microsoft changed

People were alarmed, and Microsoft delayed Recall. In June 2024, it made Recall something you must choose to turn on. It also said Recall would need your face, fingerprint, or PIN before showing anything <a name="apa-ms-june-citation"></a>([Microsoft, 2024](#apa-ms-june-reference)).

Later that year, Microsoft rebuilt how Recall stores its data <a name="apa-weston-citation"></a>([Weston, 2024](#apa-weston-reference)). Here is what changed:

- **Locked storage.** Snapshots are now scrambled with a code, called encryption. Only your own sign-in can unlock them.
- **A protected space.** The unlocking happens in a sealed-off part of the computer that other programs cannot reach.
- **A sign-in check.** You must use your face, fingerprint, or PIN to open Recall.
- **Filters.** Recall tries to skip private browser windows and screens that show things like card numbers. You can also tell it to skip certain apps and websites.
- **A way out.** You can remove Recall from your computer completely.

Recall finally came out in April 2025 <a name="apa-ms-release-citation"></a>([Microsoft, 2025a](#apa-ms-release-reference)). These changes fixed the worst problems. A stolen laptop no longer gives up its Recall history. Other people with their own accounts on the same computer cannot read yours.

## The problems that remain

The filters are not perfect. In December 2024, a tech journalist tested them. Recall still saved a card number, a password, and a Social Security number <a name="apa-younker-citation"></a>([Younker, 2024](#apa-younker-reference)).

A bigger problem came to light in April 2026. Hagenah found that the lock works, but only until you open Recall. Once you unlock it with your face, fingerprint, or PIN, your snapshots pass through a part of Windows that is not protected. A harmful program on your computer can then copy everything. It does not need special access, and it can keep copying until you sign out or restart <a name="apa-hagenah-2026-citation"></a>([Hagenah, 2026](#apa-hagenah-2026-reference)).

Microsoft says this is how Recall is meant to work and is not a security flaw <a name="apa-swain-citation"></a>([Swain, 2026](#apa-swain-reference)). So it is unlikely to be fixed by a normal security update.

There is one more problem that no setting can fix. Anyone who knows your PIN can open Recall as you. In a home where one person controls another, that could expose a great deal.

## Other people on your screen

Recall does not only capture your life. It also captures messages from friends, faces on video calls, and documents others share with you. Those people never agreed to be saved on your computer.

Some apps now block Recall to protect them. The private messaging app Signal hides its window from Recall and other screenshot tools. Signal warns that this can stop screen readers and magnifiers from working with the app <a name="apa-lund-citation"></a>([Lund, 2025](#apa-lund-reference)). The web browser Brave also tells Recall to skip all of its tabs <a name="apa-brave-citation"></a>([Brave, 2025](#apa-brave-reference)).

## What you can do

If your computer is not a Copilot+ PC, you do not need to do anything. Recall cannot run on it.

If you have a Copilot+ PC and do not use Recall, the safest choice is to remove it. In the Windows search box, type PowerShell. Choose to run it as administrator. Then type this line and press Enter:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "Recall" -Remove
```

If you choose to use Recall, these habits lower the risk:

- **Skip sensitive apps.** In Recall's settings, exclude your password manager, banking, health, and messaging apps and sites.
- **Open it with care.** Each time you unlock Recall, anything already running on the computer may be able to read it. Do not open it if you think the computer has a virus.
- **Guard your PIN.** Anyone who knows it can see your history.

If you look after computers for an organisation, Recall is removed by default on managed work devices. You can keep it that way with a policy ([Microsoft, 2025b](#apa-ms-manage-recall-reference)).

## What may come next

In January 2026, a news site reported that Microsoft was rethinking Recall, perhaps under a new name. Microsoft has not confirmed this <a name="apa-bowden-citation"></a>([Bowden, 2026](#apa-bowden-reference)). Whatever comes next, the same question will apply. Who can reach a record of everything you have seen, and when?

## Learn more

The companion document, *Windows Recall: Security Analysis, 2024 to 2026*, covers each version of Recall in detail. It also lists the evidence for each finding.

## License

This document, *Windows Recall: What It Is and What Happened*, by **Christopher Steel**, with AI assistance from **Claude Opus 5.5 (Anthropic)**, is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

## Resources

### From Microsoft

- [Introducing Copilot+ PCs](#apa-mehdi-reference)
- [The June 2024 changes](#apa-ms-june-reference)
- [How Recall protects its data](#apa-weston-reference)
- [Recall's release](#apa-ms-release-reference)
- [Managing Recall](#apa-ms-manage-recall-reference)

### From researchers and reporters

- [Kevin Beaumont's findings](#apa-beaumont-reference)
- [Alexander Hagenah's first tool](#apa-hagenah-2024-reference)
- [Alexander Hagenah's 2026 findings](#apa-hagenah-2026-reference)
- [Filter test](#apa-younker-reference)
- [Microsoft's response in 2026](#apa-swain-reference)
- [Report on Recall's future](#apa-bowden-reference)

### From app makers

- [Signal](#apa-lund-reference)
- [Brave](#apa-brave-reference)

## References

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

<a name="apa-lund-reference"></a>Lund, J. (2025, May 21). *By default, Signal doesn't Recall*. Signal. https://signal.org/blog/signal-doesnt-recall/
[Return to citation](#apa-lund-citation)

<a name="apa-mehdi-reference"></a>Mehdi, Y. (2024, May 20). *Introducing Copilot+ PCs*. Official Microsoft Blog. https://blogs.microsoft.com/blog/2024/05/20/introducing-copilot-pcs/
[Return to citation](#apa-mehdi-citation)

<a name="apa-ms-june-reference"></a>Microsoft. (2024, June 7). *Update on the Recall preview feature for Copilot+ PCs*. Windows Experience Blog. https://blogs.windows.com/windowsexperience/2024/06/07/update-on-the-recall-preview-feature-for-copilot-pcs/
[Return to citation](#apa-ms-june-citation)

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
| 0.1.0 | Draft, version provisional | First version, written for a general audience |

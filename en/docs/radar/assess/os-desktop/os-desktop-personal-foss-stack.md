# OS Desktop — Personal FOSS Stack

*From https://stacks.web3privacy.info/categories/OS_Desktop | 18 tools | 45 expert slots | 34 unique experts*

| OS              | Experts | Named experts                                                |
| --------------- | ------- | ------------------------------------------------------------ |
| **MacOS**       | **8**   | Ais Connolly, Lanski, Pentacle, KyleDH, sandor, kassandrawitch, soorajsathyanarayanan, oskarth |
| **NixOS**       | 6       | Jarrad Hope, sam hart, Matthias Tarasiewicz, Aki, brito, timclancy |
| **Ubuntu**      | 6       | Alan Scott, wesley, Antonio Seveso, ChrisMcCabe, kassandrawitch, bobsummervill |
| **QubesOS**     | 5       | SebastianB, Matthias Tarasiewicz, redshiftzero, cheekygorilla, oskarth |
| **TailsOS**     | 5       | Ais Connolly, Ahmed, jordi, cheekygorilla, oskarth           |
| **Debian**      | 2       | Roger Dingledine, matthiaskirschner                          |
| **VoidLinux**   | 2       | Amir Taaki, Rachel-Rose                                      |
| **LinuxMint**   | 2       | Josh Davila, Jaromil                                         |
| **Whonix**      | 2       | Vladimir S, cheekygorilla                                    |
| **PopOS**       | 1       | Auryn                                                        |
| **EndeavourOS** | 1       | zodomo                                                       |
| **Raspbian**    | 1       | VladCostea                                                   |
| **FedoraLinux** | 1       | jordi                                                        |
| **DivestOS**    | 1       | Vladimir S                                                   |
| **ParrotOS**    | 1       | Vladimir S                                                   |
| **KaiOS**       | 1       | Rachel-Rose                                                  |
| **Arch Linux**  | 0       | —                                                            |
| **Gnu/Linux**   | 0       | —                                                            |

### Repeat users (11 extra slots)
oskarth (MacOS, QubesOS, TailsOS) and Vladimir S (DivestOS, ParrotOS, Whonix) appear three times each; cheekygorilla (QubesOS, TailsOS, Whonix) also three. Ais Connolly, kassandrawitch, Matthias Tarasiewicz, jordi, and Rachel-Rose appear twice.

---

# Storage — Personal FOSS Stack

*From https://stacks.web3privacy.info/categories/Storage | 19 entries | 40 expert slots | 31 unique experts*

| Tool                 | Experts | Named experts                                                |
| -------------------- | ------- | ------------------------------------------------------------ |
| **Fileverse**        | **10**  | Nick Almond, Ann Brody, Vladimir S, Andreas (Fileverse), Josh Davila, Lanski, Francesko Andreoli, ChrisMcCabe, Alisher, andyguzman |
| **Proton Drive**     | **10**  | Audrey Tang, Alan Scott, Kieran, Francesko Andreoli, ChrisMcCabe, VladCostea, KyleDH, sandor, kassandrawitch, soorajsathyanarayanan |
| **Codex**            | 3       | Jarrad Hope, Alisher, Aki                                    |
| **Nextcloud**        | 3       | zodomo, Pentacle, kassandrawitch                             |
| **ZFS**              | 3       | Jaromil, polymutex, oskarth                                  |
| **Ente**             | 2       | soorajsathyanarayanan, gnukeith                              |
| **USB**              | 1       | SebastianB                                                   |
| **ownCloud**         | 1       | Kate-S                                                       |
| **Server**           | 1       | Antonio Seveso                                               |
| **GitHub** (entry 1) | 1       | brito                                                        |
| **Tarsnap**          | 1       | Jarrad Hope                                                  |
| **GitHub** (entry 3) | 1       | Jaromil                                                      |
| **Swarm**            | 1       | sandor                                                       |
| **Filen**            | 1       | cheekygorilla                                                |
| **TrueNAS**          | 1       | oskarth                                                      |
| **GitHub** (entry 2) | 0       | —                                                            |
| **Sia**              | 0       | —                                                            |
| **PrivateStorage**   | 0       | —                                                            |
| **iCloud**           | 0       | —                                                            |

### Repeat users (9 extra slots)
Alisher, Francesko Andreoli, ChrisMcCabe, Jarrad Hope, kassandrawitch, soorajsathyanarayanan, Jaromil, sandor, and oskarth each appear twice.

---

## Cross-category observations

**MacOS is the single most-adopted desktop OS on this list — 8 users, more than any Linux distribution.** In a category explicitly framed as a "personal FOSS stack," the leading choice is proprietary, closed-source Apple software. NixOS and Ubuntu tie for second at 6. Whatever this list measures, it is closer to "what privacy practitioners actually run" than "what the privacy community recommends," and those two things diverge sharply here. Under your framework's stakeholder-lens rule, that divergence is exactly the kind of conflict worth recording rather than resolving.

**The hardened options are minority choices.** QubesOS and TailsOS tie at 5 each, Whonix at 2, ParrotOS and DivestOS at 1. Combined, the architecturally hardened systems account for 14 of 45 slots — under a third — while general-purpose distributions plus MacOS take the rest.

**Nine experts appear in both categories**, and oskarth appears five times across the two (MacOS, QubesOS, TailsOS, ZFS, TrueNAS). That pattern suggests layered stacks — a daily driver plus specialised tools — rather than single-tool loyalty, and it means naive slot-counting inflates apparent consensus.

**Three Storage entries are literally labelled "GitHub" with the generic GitHub icon**, two with no users at all. Combined with "USB" and "Server" — which are generic nouns, not products — roughly a quarter of the Storage category is unresolved or non-specific. That looks like incomplete data rather than deliberate curation.

**Arch Linux and Gnu/Linux have zero users** despite being listed, and Gnu/Linux is an umbrella term overlapping several other entries. Listing without adoption is a weak signal.

**No "+N" overflow on either page**, unlike Messenger's "+36" on Signal. These totals should be complete, though the Messenger discrepancy suggests the site renders overflow inconsistently.

---

## Evidence notes

- **Tool names, links, and expert attributions: Verified** — read directly from both category pages, 27 October 2026. Expert names are derived from avatar image filenames, so formatting follows the site's own file-naming (e.g. "sam hart", "Ann Brody.png").
- **Expert identity inference.** Avatar filenames are matched to named experts elsewhere on the site (e.g. "audreytang.jpg" = Audrey Tang, "VitalikButerin.png" = Vitalik Buterin). A few filenames are handles rather than full names (brito, zodomo, oskarth, sandor, cheekygorilla) and I have not resolved those to identities.
- **Descriptions: omitted here** to keep the tables readable; the OS_Desktop descriptions from my previous message stand, with the same evidence caveats.
- **Corrected from my last message:** I reported that OS_Desktop lacked expert attributions. That was a rendering artefact of the first fetch, not missing data. Retrying returned the full set.
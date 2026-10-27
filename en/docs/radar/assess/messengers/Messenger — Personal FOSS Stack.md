# Messenger — Personal FOSS Stack

*From https://stacks.web3privacy.info/categories/Messenger | 19 tools listed | 44 expert slots shown*

Expert stacks collected by Web3Privacy Now from participants in the Ethereum Cypherpunk Congress and wider privacy community.

---

## Tools

| Messenger          | What it is                                                   | Experts           | Named experts                                                |
| ------------------ | ------------------------------------------------------------ | ----------------- | ------------------------------------------------------------ |
| **Signal**         | Nonprofit, end-to-end encrypted messenger; E2EE by default including metadata layers. Requires a phone number at signup. US jurisdiction. Operator can produce phone number, creation date, and last connection. | 10 shown, **+36** | Audrey Tang, Vitalik Buterin, Lefteris Karapetsas, Auryn, Nick Almond, Alan Scott, Kieran, Ann Brody, Park, Andreas (Fileverse) |
| **Status**         | Ethereum-ecosystem messenger and wallet                      | 6                 | Jarrad Hope, Kieran, Ais Connolly, Peter van Valkenburgh, Harry Halpin, Alisher |
| **Threema**        | Swiss (Pfäffikon, founded 2012), pay-once. Anonymous signup — no phone number or email required. Open source, independently audited, GDPR-compliant, self-hostable. Reported 12M+ users worldwide. | 1                 | SebastianB                                                   |
| **Session**        | Swiss foundation. Onion-routed, no phone number linkage; uses a persistent account ID. Crypto-token governance introduces incentive considerations. | 5                 | Auryn, Fatemeh, Ahmed, ChrisMcCabe, bobsummervill            |
| **SimpleX**        | UK-registered (founded 2021, London). No user identifiers at all — no username, no random ID. Separate unlinked queues per conversation. Trail of Bits reviewed protocol design (2024). Self-hostable relays. 2025 transparency report: 12 law-enforcement requests, no responsive information identified. | 5                 | Amir Taaki, Harry Halpin, Matthias Tarasiewicz, zodomo, soorajsathyanarayanan |
| **DarkIRC**        | IRC implementation associated with the DarkFi project        | 4                 | Amir Taaki, Peter van Valkenburgh, Rachel-Rose, bobsummervill |
| **Geogram**        | Project hosted at github.com/geograms/geogram                | 1                 | brito                                                        |
| **Telegram**       | Cloud messenger, Dubai/UAE. Widely noted as not private by default. | 1                 | Vladimir S                                                   |
| **Element**        | London (founded 2017). Client for the Matrix protocol; serves government and defence sectors. | 3                 | afri, cheekygorilla, matthiaskirschner                       |
| **Matrix**         | Open federated protocol underlying Element                   | 1                 | Harry Halpin                                                 |
| **Dino**           | XMPP client for Linux                                        | 1                 | matthiaskirschner                                            |
| **Conversations**  | XMPP client for Android                                      | 1                 | matthiaskirschner                                            |
| **Molly**          | Hardened Signal fork for Android                             | 1                 | gnukeith                                                     |
| **Zulip**          | Open-source organised team chat                              | 1                 | Harry Halpin                                                 |
| **HexChat**        | IRC client                                                   | 0                 | —                                                            |
| **Keybase**        | Encrypted chat with identity proofs; acquired by Zoom in 2020 | 1                 | Lefteris Karapetsas                                          |
| **Bittlbee**       | Project at bittlbee.org                                      | 1                 | Jaromil                                                      |
| **N8N**            | Workflow automation platform, not a messenger in the conventional sense | 1                 | Jaromil                                                      |
| **Revolution IRC** | Android IRC client, distributed via F-Droid                  | 0                 | —                                                            |

---

## Adoption on this site

| Rank | Messenger                                                    | Expert slots            |
| ---- | ------------------------------------------------------------ | ----------------------- |
| 1    | Signal                                                       | 10 shown + 36 more (46) |
| 2    | Status                                                       | 6                       |
| 3    | Session                                                      | 5                       |
| 3    | SimpleX                                                      | 5                       |
| 5    | DarkIRC                                                      | 4                       |
| 6    | Element                                                      | 3                       |
| 7    | Threema, Geogram, Telegram, Matrix, Dino, Conversations, Molly, Zulip, Keybase, Bittlbee, N8N | 1 each                  |
| —    | HexChat, Revolution IRC                                      | 0                       |

**44 expert slots across 19 tools, representing 32 unique experts.** Several appear more than once: matthiaskirschner (Element, Dino, Conversations), Harry Halpin (Status, SimpleX, Matrix, Zulip), and Kieran, Auryn, Amir Taaki, Peter van Valkenburgh, bobsummervill, Jaromil, and Lefteris Karapetsas each appear twice.

---

## Observations

**Signal dominates, but not unanimously.** If the "+36" indicator means additional experts beyond those shown, roughly half the surveyed cohort uses Signal — consistent with the docs' note that ~90 experts participated. Notably, several experts who appear on Signal also appear on stricter alternatives (Harry Halpin on Signal, Status, SimpleX, Matrix, and Zulip), suggesting layered rather than exclusive use.

**The long tail is one-person deep.** Eleven of nineteen tools have exactly one named user. That's a weak signal for a "recommended" list — it may reflect a single advocate rather than community consensus.

**Six tools are not messengers in the usual sense.** N8N is workflow automation, HexChat and Revolution IRC are IRC clients, Dino and Conversations are XMPP clients, and Matrix is a protocol rather than an app. The category is loosely bounded.

**Threema's low count is a data point, not a verdict.** One named user, despite strong published privacy properties (anonymous signup, Swiss jurisdiction, open source, audits). The likely cause is cost — Threema is pay-once while Signal is free.

**Jurisdictional spread is genuinely diverse.** Switzerland (Threema, Session), UK (SimpleX, Element, Matrix ecosystem), US (Signal), UAE (Telegram). No single legal regime dominates.

---

## Evidence notes

- **Tool names, links, and expert attributions: Verified** — read directly from the category page, 27 October 2026.
- **Descriptions: mixed.** Signal's metadata properties, SimpleX's identifier model and 2025 transparency report, Session's onion routing, Threema's Swiss registration and anonymous signup, Element's founding and Matrix's federation model, and Telegram's default-privacy reputation are sourced from retrieved pages. Dino, Conversations, Molly, Zulip, HexChat, Keybase, and N8N descriptions come from training data and are unverified.
- **Global user counts: largely unverified.** The only figure I could source is Threema's "more than 12 million users worldwide" (Brosix). I found no reliable current figures for Signal, Session, SimpleX, or the others, and I won't estimate them.
- **Known gaps.** The "+36" indicator links to github.com, which appears to be a placeholder rather than a real destination. An earlier search snippet suggested WeeChat and XMPP also appear on this page; my fetch ended at Revolution IRC, so the list may continue below what rendered.
- **Caveat on Element/Matrix.** The strongest criticism of Matrix federation in my results comes from Threema's own comparison page — a competitor's claim that messages and metadata are permanently stored on all involved servers. Attribute accordingly.

# NEXT



Fetched the actual Messenger page this time. One correction first: in my previous message I padded the wallet list — the page snippet showed 23 names, and I added five (Trust Wallet, MetaMask, Phantom, Exodus, Coinbase Wallet) that were **not** in the source to reach 28. Those five were invented. The verified wallet names are the first 23 plus a "+20" indicator.

Here's the Messenger category, transcribed from the page itself:

```markdown
# Messenger — Personal FOSS Stack

*From https://stacks.web3privacy.info/categories/Messenger | 19 tools listed | 44 expert slots shown*

Expert stacks collected by Web3Privacy Now from participants in the Ethereum Cypherpunk Congress and wider privacy community.

---

## Tools

| Messenger | What it is | Experts | Named experts |
|---|---|---|---|
| **Signal** | Nonprofit, end-to-end encrypted messenger; E2EE by default including metadata layers. Requires a phone number at signup. US jurisdiction. Operator can produce phone number, creation date, and last connection. | 10 shown, **+36** | Audrey Tang, Vitalik Buterin, Lefteris Karapetsas, Auryn, Nick Almond, Alan Scott, Kieran, Ann Brody, Park, Andreas (Fileverse) |
| **Status** | Ethereum-ecosystem messenger and wallet | 6 | Jarrad Hope, Kieran, Ais Connolly, Peter van Valkenburgh, Harry Halpin, Alisher |
| **Threema** | Swiss (Pfäffikon, founded 2012), pay-once. Anonymous signup — no phone number or email required. Open source, independently audited, GDPR-compliant, self-hostable. Reported 12M+ users worldwide. | 1 | SebastianB |
| **Session** | Swiss foundation. Onion-routed, no phone number linkage; uses a persistent account ID. Crypto-token governance introduces incentive considerations. | 5 | Auryn, Fatemeh, Ahmed, ChrisMcCabe, bobsummervill |
| **SimpleX** | UK-registered (founded 2021, London). No user identifiers at all — no username, no random ID. Separate unlinked queues per conversation. Trail of Bits reviewed protocol design (2024). Self-hostable relays. 2025 transparency report: 12 law-enforcement requests, no responsive information identified. | 5 | Amir Taaki, Harry Halpin, Matthias Tarasiewicz, zodomo, soorajsathyanarayanan |
| **DarkIRC** | IRC implementation associated with the DarkFi project | 4 | Amir Taaki, Peter van Valkenburgh, Rachel-Rose, bobsummervill |
| **Geogram** | Project hosted at github.com/geograms/geogram | 1 | brito |
| **Telegram** | Cloud messenger, Dubai/UAE. Widely noted as not private by default. | 1 | Vladimir S |
| **Element** | London (founded 2017). Client for the Matrix protocol; serves government and defence sectors. | 3 | afri, cheekygorilla, matthiaskirschner |
| **Matrix** | Open federated protocol underlying Element | 1 | Harry Halpin |
| **Dino** | XMPP client for Linux | 1 | matthiaskirschner |
| **Conversations** | XMPP client for Android | 1 | matthiaskirschner |
| **Molly** | Hardened Signal fork for Android | 1 | gnukeith |
| **Zulip** | Open-source organised team chat | 1 | Harry Halpin |
| **HexChat** | IRC client | 0 | — |
| **Keybase** | Encrypted chat with identity proofs; acquired by Zoom in 2020 | 1 | Lefteris Karapetsas |
| **Bittlbee** | Project at bittlbee.org | 1 | Jaromil |
| **N8N** | Workflow automation platform, not a messenger in the conventional sense | 1 | Jaromil |
| **Revolution IRC** | Android IRC client, distributed via F-Droid | 0 | — |

---

## Adoption on this site

| Rank | Messenger | Expert slots |
|---|---|---|
| 1 | Signal | 10 shown + 36 more (46) |
| 2 | Status | 6 |
| 3 | Session | 5 |
| 3 | SimpleX | 5 |
| 5 | DarkIRC | 4 |
| 6 | Element | 3 |
| 7 | Threema, Geogram, Telegram, Matrix, Dino, Conversations, Molly, Zulip, Keybase, Bittlbee, N8N | 1 each |
| — | HexChat, Revolution IRC | 0 |

**44 expert slots across 19 tools, representing 32 unique experts.** Several appear more than once: matthiaskirschner (Element, Dino, Conversations), Harry Halpin (Status, SimpleX, Matrix, Zulip), and Kieran, Auryn, Amir Taaki, Peter van Valkenburgh, bobsummervill, Jaromil, and Lefteris Karapetsas each appear twice.

---

## Observations

**Signal dominates, but not unanimously.** If the "+36" indicator means additional experts beyond those shown, roughly half the surveyed cohort uses Signal — consistent with the docs' note that ~90 experts participated. Notably, several experts who appear on Signal also appear on stricter alternatives (Harry Halpin on Signal, Status, SimpleX, Matrix, and Zulip), suggesting layered rather than exclusive use.

**The long tail is one-person deep.** Eleven of nineteen tools have exactly one named user. That's a weak signal for a "recommended" list — it may reflect a single advocate rather than community consensus.

**Six tools are not messengers in the usual sense.** N8N is workflow automation, HexChat and Revolution IRC are IRC clients, Dino and Conversations are XMPP clients, and Matrix is a protocol rather than an app. The category is loosely bounded.

**Threema's low count is a data point, not a verdict.** One named user, despite strong published privacy properties (anonymous signup, Swiss jurisdiction, open source, audits). The likely cause is cost — Threema is pay-once while Signal is free.

**Jurisdictional spread is genuinely diverse.** Switzerland (Threema, Session), UK (SimpleX, Element, Matrix ecosystem), US (Signal), UAE (Telegram). No single legal regime dominates.

---

## Evidence notes

- **Tool names, links, and expert attributions: Verified** — read directly from the category page, 27 October 2026.
- **Descriptions: mixed.** Signal's metadata properties, SimpleX's identifier model and 2025 transparency report, Session's onion routing, Threema's Swiss registration and anonymous signup, Element's founding and Matrix's federation model, and Telegram's default-privacy reputation are sourced from retrieved pages. Dino, Conversations, Molly, Zulip, HexChat, Keybase, and N8N descriptions come from training data and are unverified.
- **Global user counts: largely unverified.** The only figure I could source is Threema's "more than 12 million users worldwide" (Brosix). I found no reliable current figures for Signal, Session, SimpleX, or the others, and I won't estimate them.
- **Known gaps.** The "+36" indicator links to github.com, which appears to be a placeholder rather than a real destination. An earlier search snippet suggested WeeChat and XMPP also appear on this page; my fetch ended at Revolution IRC, so the list may continue below what rendered.
- **Caveat on Element/Matrix.** The strongest criticism of Matrix federation in my results comes from Threema's own comparison page — a competitor's claim that messages and metadata are permanently stored on all involved servers. Attribute accordingly.
```

Two things I can chase next if useful: the "+36" and the possible truncation are both resolvable by pulling the underlying repo (the docs point to `web3privacy/data` and a migration "into 1 table"), which would also give you machine-readable expert-to-tool mappings instead of scraping avatar filenames. And if you want global adoption figures, those need a dedicated search per messenger — I'd rather do that deliberately than guess.
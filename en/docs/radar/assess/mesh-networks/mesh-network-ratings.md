# Mesh network ratings

| Metric area                          | Headscale + Tailscale client                                 | NetBird (self-hosted)                                        | ZeroTier                                                     |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Multilingual integration             | Unknown, leaning Weak. I know of no localisation of the CLI or docs | Unknown. Check the dashboard repo for locale files           | Unknown, leaning Weak                                        |
| Economic and cognitive accessibility | Moderate. Free, but needs comfort with a CLI and a policy file | Moderate. Free Community Edition, more services to understand | Weak for your use. Free hosted plan is 10 devices, and commercial self-hosting needs a paid licence |
| Representation                       | Unknown. Nothing public on disabled representation or user research | Unknown, same                                                | Unknown, same                                                |
| Compatibility                        | Strong. Linux, BSD, macOS, Windows, mobile                   | Strong                                                       | Strong                                                       |
| Agency, interaction patterns         | Strong. No upsell or nagging in a Headscale setup            | Moderate. Paid features sit beside the free edition; check for in-dashboard prompts | Moderate. Limits have tightened after adoption               |
| Environment                          | Strong. Single small binary with SQLite, kernel WireGuard on nodes | Moderate. Several services plus a dashboard                  | Strong on nodes; controller depends on setup                 |
| Content endurance                    | Not applicable. No authored content is stored, only node keys and access policy | Not applicable, same                                         | Not applicable, same                                         |
| Adjustability and support            | Moderate. Permissive licence and readable Go, but a deliberately narrow scope | Moderate. Fork path is open, contributions go through a CLA  | Weak for the controller, which is not open source            |

Three rows worth your attention when testing:

- **Accessibility floor:** I rated the text interfaces from documentation only. The gate needs a real terminal screen reader pass on install, enrolment and a failed-login error before it can be marked Verified.
- **Multilingual:** all three are Unknown, which matters for Quebec clients if anyone other than you will operate the mesh.
- **Representation:** Unknown across the board, and per the metrics that absence is itself a data point to record.

The recommendation is unchanged: Headscale + Tailscale and NetBird stay in Assess and go to testing, and ZeroTier is a likely Hold on the licence gate, pending your call on Vishpala's licence policy.

# webprivacy-metrics-comparison

## How it maps onto the Universal Cake framework

This is the part worth your attention, because the two models are structurally incompatible in one specific way.

| W3PN criterion                 | Nearest Universal Cake row            | Relationship                                                 |
| ------------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| Team Information               | Representation; Market Position       | Partial — UC asks about team *composition* for representation, not credibility signalling |
| Documentation                  | Adjustability and support             | Direct                                                       |
| GitHub Presence                | Longevity; Market Position            | Direct                                                       |
| Social Media Engagement        | —                                     | **No equivalent; value conflict**                            |
| Whitepaper Availability        | Adjustability and support             | Near-direct                                                  |
| Funding Transparency           | Longevity; Market Position            | Direct (funding round is already a UC re-review trigger)     |
| Mainnet Deployment             | Resilience; Longevity                 | Partial                                                      |
| Open Source Code               | Sovereignty & Privacy; licence gate   | Direct                                                       |
| Non-Custodial Asset Management | Sovereignty & Privacy                 | Direct                                                       |
| Immutable Smart Contracts      | —                                     | **No equivalent; value conflict**                            |
| Security Audits                | Security (assessment method)          | Direct                                                       |
| Privacy Policy                 | Sovereignty & Privacy; Security       | Direct                                                       |
| Minimal KYC Requirements       | Sovereignty & Privacy; Representation | Direct                                                       |
| Regulatory Compliance          | —                                     | **No equivalent; value conflict**                            |
| Default Privacy Settings       | Interaction Patterns; telemetry gate  | Direct                                                       |

Four things fall out of that comparison:

**The aggregation rule is a direct contradiction.** W3PN sums to 150 and ranks on the total. Your framework explicitly refuses this: "These ratings are not summed or averaged into a single score." Under your own logic, a project scoring 145/150 while failing the exit gate would sit in the hold ring; W3PN's model would rank it near the top. If you ingest their scores, you'd be importing the exact behaviour your Aggregation section forbids.

**Two criteria reward things your framework penalises.** Social Media Engagement (5 pts) pays for sustained attention capture, which your Interaction Patterns pillar treats as a dark pattern to check against. Immutable Smart Contracts (10 pts) rewards un-patchable code, which cuts against your Security row on patch responsiveness and your Resilience row. Neither is wrong in W3PN's frame — they're just measuring a different value system.

**One criterion pair is internally tense.** Minimal KYC (10 pts) and Regulatory Compliance (5 pts) pull in opposite directions; a project cannot maximise both. Your framework has machinery for exactly this — "record the conflict rather than letting one answer hide the other" — which W3PN's additive model cannot express, since both just become points.

**Your hardest constraints are absent.** No accessibility floor, no exit/portability gate, no Environment pillar, no dark-pattern rows, no evidence tags, no gates concept at all. Their model has no way to express "this fails a gate and cannot be averaged away."

## 
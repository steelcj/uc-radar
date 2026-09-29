# privacy-oriented-website-analytics

Yes. There is a fairly good ecosystem now, and several are worth evaluating alongside Ackee. Given the Universal Cake criteria, I would particularly look at **GoatCounter, Umami, Plausible, and Matomo**.

I would distinguish them this way:

| Product                 | Self-host                 | Licence     | Privacy orientation  | Complexity   | Cost profile                 |
| ----------------------- | ------------------------- | ----------- | -------------------- | ------------ | ---------------------------- |
| **GoatCounter**         | Yes                       | EUPL-1.2    | Very strong          | Very low     | Free hosted / free self-host |
| **Umami**               | Yes                       | MIT         | Very strong          | Low          | Free self-host / paid cloud  |
| **Plausible Analytics** | Yes                       | AGPLv3      | Very strong          | Moderate     | Free CE / paid cloud         |
| **Ackee**               | Yes                       | MIT         | Very strong          | Low–moderate | Free                         |
| **Matomo**              | Yes                       | GPLv3       | Strong, configurable | High         | Free self-host / paid cloud  |
| **Fathom Analytics**    | Limited/self-host option  | Proprietary | Strong               | Very low     | Paid                         |
| **Simple Analytics**    | No practical self-hosting | Proprietary | Very strong          | Very low     | Paid                         |

### 1. GoatCounter

This is probably the one I would put **at the top of the Universal Cake evaluation queue**.

GoatCounter is deliberately small and opinionated. It says explicitly that it does not track personal data or use unique identifiers, is open source, can be self-hosted, and lets you export your data. It also supports a JavaScript-free tracking-pixel approach and server-side/log-file integration. ([GoatCounter](https://www.goatcounter.com/?utm_source=chatgpt.com))

The accessibility claim is particularly interesting for your framework: GoatCounter explicitly says accessibility is a high priority and that its interface works with screen readers. ([GoatCounter](https://www.goatcounter.com/?utm_source=chatgpt.com))

That means it potentially addresses one of the biggest **Unknowns** in our Ackee evaluation.

**Universal Cake hypothesis:** potentially **Strong** across sovereignty, privacy, accessibility, simplicity, portability and environmental impact.

[GoatCounter](https://www.goatcounter.com/?utm_source=chatgpt.com)

------

### 2. Umami

Umami is probably the most interesting **Ackee alternative** if you want somewhat more functionality without jumping all the way to Matomo.

It is open source, MIT-licensed, self-hostable, cookieless, and says it does not automatically collect personal information. It also provides complete data export and allows you to run the database on your own infrastructure. ([Umami](https://umami.is/platform?utm_source=chatgpt.com))

It has substantially more analytics functionality than Ackee, including:

- custom events
- funnels
- retention
- UTM tracking
- goals
- dashboards
- API access
- performance monitoring

It also now has session replay, which I would scrutinize carefully under the Universal Cake privacy criteria. ([Umami Docs](https://docs.umami.is/docs/about?utm_source=chatgpt.com))

That last point is important: **"privacy-focused" does not automatically mean every feature is equally privacy-preserving.**

**Universal Cake hypothesis:** Strong overall, but session replay deserves specific testing.

[Umami](https://umami.is/?utm_source=chatgpt.com)

------

### 3. Plausible

Plausible is perhaps the most mature option in the **simple privacy-first analytics** category.

It is open source, has a free self-hosted Community Edition under AGPLv3, and its hosted service is funded by subscriptions rather than advertising or surveillance. Plausible states that it collects no personal data, uses no cookies or cross-site tracking, and that customers own and control their analytics data. ([Plausible Analytics](https://plausible.io/open-source-website-analytics?utm_source=chatgpt.com))

The hosted version currently starts at **$9/month**, while the Community Edition has no subscription fee if you operate it yourself. ([Plausible Analytics](https://plausible.io/?utm_source=chatgpt.com))

For Universal Cake, the **AGPL licence is particularly interesting** because it provides stronger reciprocity than Ackee's MIT licence.

**Universal Cake hypothesis:** Strong sovereignty/privacy, potentially Strong longevity, but somewhat more infrastructure than Ackee or GoatCounter.

[Plausible Analytics](https://plausible.io/?utm_source=chatgpt.com)

------

### 4. Matomo

Matomo is the opposite end of the spectrum.

It is much closer to a full Google Analytics replacement, with substantially greater analytical depth. It is GPL-licensed and self-hostable.

The tradeoff is that it is **much larger and more complicated**. Current comparisons put it considerably above GoatCounter, Umami and Ackee in operational requirements. ([Helion Labs](https://helionlabs.dev/articles/self-hosted-web-analytics?utm_source=chatgpt.com))

For Universal Cake, that creates an interesting question:

> Does additional analytical capability actually improve agency and wellbeing, or does it recreate the complexity and surveillance potential of the system being replaced?

I would therefore evaluate Matomo, but I would not assume that its larger feature set is an advantage.

[Matomo](https://matomo.org/?utm_source=chatgpt.com)

------

## One I'd watch: Medama

There's also a newer class of extremely lightweight analytics tools emerging around **single-binary/self-hosted deployments**. Medama is one of them.

That category is particularly interesting for your Environment and Resilience criteria because a small Go binary can potentially eliminate the Node + database + reverse-proxy/application-stack complexity of tools such as Ackee and Umami.

I would want to evaluate Medama before settling on an analytics standard.

## My shortlist for Universal Cake

Rather than picking a winner, I'd make the next evaluation set:

**GoatCounter → Ackee → Umami → Plausible → Matomo → Medama**

And I'd add a few criteria that become especially interesting when comparing them:

1. **Visitor privacy**
2. **Operator sovereignty**
3. **Data minimization**
4. **Data export**
5. **Accessibility**
6. **Infrastructure/resource requirements**
7. **Licence reciprocity**
8. **Ease of self-hosting**
9. **Ability to operate without JavaScript**
10. **Ability to operate without cookies**
11. **Whether advanced features create new surveillance capabilities**
12. **Actual telemetry generated by the analytics software itself**

That last distinction is very Universal Cake: **the analytics tool should not become a surveillance system merely because it is marketed as a privacy-preserving alternative.**

And based on what I've found so far, **GoatCounter is the one I'd be most curious to put through the full v0.5.1 evaluation next**. It has an unusually strong combination of privacy, simplicity, accessibility claims, portability and self-hostability. ([GoatCounter](https://www.goatcounter.com/?utm_source=chatgpt.com))


Here's a comparison filtered for what actually matters to UC-Gradus: unmanaged, Canadian node, KVM, IPv6, Debian image (or ISO fallback), snapshots, and ideally cloud-init user-data support.

## Scoring

Scale: 1–5 per axis. Weighted equally, with a note on what matters most for your workload.

```skill:table
{
 "title": "Unmanaged VPS providers — Canadian nodes, scored for UC-Gradus",
 "columns": [
 { "key": "provider", "label": "Provider" },
 { "key": "region", "label": "Region" },
 { "key": "cost", "label": "Cost (CAD/mo, 2 GB)" },
 { "key": "speed", "label": "Speed" },
 { "key": "value", "label": "Value" },
 { "key": "service", "label": "Service" },
 { "key": "total", "label": "Total" },
 { "key": "note", "label": "UC-Gradus fit" }
 ],
 "rows": [
 { "provider": "OVHcloud", "region": "Beauharnois, QC", "cost": "~$14 CAD", "speed": 4.5, "value": 4.5, "service": 2.5, "total": 16, "note": "Quebec soil, CAD billing, /64 IPv6, cloud-init on most plans. Support is slow/formal." },
 { "provider": "DigitalOcean", "region": "Toronto", "cost": "~$17 CAD equiv", "speed": 4, "value": 5, "service": 4, "total": 17, "note": "Best API + docs, cloud-init user-data confirmed, snapshots, provider firewall. USD billing adds spread." },
 { "provider": "Vultr", "region": "Toronto", "cost": "~$17 CAD equiv", "speed": 4.5, "value": 4.5, "service": 3.5, "total": 16, "note": "HF tier is fastest CPU-per-dollar. Cloud-init supported. Smaller community than DO." },
 { "provider": "Ex2", "region": "Toronto", "cost": "$12.52 CAD", "speed": 4, "value": 3.5, "service": "?", "total": 13, "note": "Cheapest CAD-billed KVM. Cloud-init and IPv6 allocation unknown until you test." },
 { "provider": "Kamatera", "region": "Toronto", "cost": "~$14 CAD equiv", "speed": 4, "value": 3.5, "service": 3, "total": 14.5, "note": "Hourly billing, custom specs. Overkill for a static box; good if you want to resize freely." },
 { "provider": "Linode (Akamai)", "region": "Toronto", "cost": "~$16 CAD equiv", "speed": 4, "value": 4, "service": 4, "total": 16, "note": "Solid API, cloud-init, snapshots, IPv6. Less exciting than DO but more consistent support." }
 ],
 "sortable": true
}
```

## Breakdown by axis

**Cost** — Ex2 wins on sticker (cheapest CAD-billed KVM at $12.52 for 2 GB). OVHcloud is next (~$14, also CAD). DigitalOcean, Vultr and Linode land around $16–17 CAD equivalent for a 2 GB plan once you count the USD→CAD spread (~1.37×). Kamatera's hourly model can undercut everything if you only run the box intermittently, which you won't.

**Speed** — For a static site, "speed" is really network proximity + NVMe + CPU single-thread. OVHcloud Beauharnois is physically closest to you (probably 2–5 ms to Quebec City, vs 8–15 ms from Toronto). Vultr's High Frequency tier (3 GHz+) is the fastest raw CPU, but Caddy serving static files will never saturate a 2 GHz shared core. The practical difference between these providers for your workload is negligible. The one that matters is: don't pick a US node.

**Value** — This is where the UC-Gradus-specific features cluster:

| Feature you need | DO | Vultr | OVHcloud | Ex2 | Linode | Kamatera |
|---|---|---|---|---|---|---|
| Cloud-init user-data | Yes | Yes | Partial (plan-dependent) | Unknown | Yes | Yes |
| Snapshots | Yes ($0.06/GiB) | Yes (small fee) | Yes (included?) | Yes (console) | Yes | Yes |
| IPv6 /64 prefix | Single addr | Single addr | Often /64+ | Unknown ("or") | Single addr | Single addr |
| Provider firewall | Yes | Yes | No (DDoS scrubbing only) | No | Yes | Optional |
| Reverse DNS in panel | Yes | Yes | Yes | Unknown | Yes | Yes |
| Debian 13 image | Likely | Likely | Check plan list | Unknown | Likely | Likely |
| API quality | Best | Strong | Functional | Unknown | Good | Moderate |

DO and Vultr edge ahead on value because cloud-init + snapshots + firewall + reverse DNS are all confirmed and panel-managed. OVHcloud matches on the Quebec location and CAD billing but its control panel and API experience are clunkier. Ex2 is the wildcard — potentially great value but you can't confirm the features that change your runbook until you sign up.

**Service** — DigitalOcean's documentation and community are unmatched; when you hit a weird issue at 2am, the answer is almost certainly a DO community post. Linode/Akamai has historically the most consistent ticket support. OVHcloud has a reputation for slow, formal responses — tolerable for a box that just serves files, frustrating when it doesn't. Vultr is middle-of-the-road. Ex2 is unknown territory.

## Recommendation for UC-Gradus

**Short answer: OVHcloud Beauharnois or DigitalOcean Toronto.**

They solve different problems:

- **OVHcloud** if you want the cheapest path to a Canadian-soil, CAD-billed box with a /64 IPv6 prefix (nice for multi-tenant — you can assign a different IPv6 to each site if you ever want per-site IPv6 instead of path routing). The tradeoff is a worse control panel and slower support. Your Ansible base doesn't care about the panel; you'll only touch it for console access and snapshot management.

- **DigitalOcean** if you want the smoothest developer experience, confirmed cloud-init user-data (your `user-data.yaml` works as-is), and the best escape hatch when something goes wrong (their docs will have the answer). The tradeoff is USD billing and ~$4–5/mo more than Ex2.

**Ex2 stays in the shortlist** as the budget option if you can confirm cloud-init and IPv6 allocation before committing. The 30-day refund window is your test harness — spin it up, run `test/incus/verify.sh` against it, and decide.

**Vultr HF** only makes sense if you later add a compute-heavy service (image processing, a search index, Cake Metrics with a TSDB). For pure static files, you're paying for CPU cycles you'll never use.

Want me to draft the OVHcloud plan picker (which specific tier maps to your NG2-equivalent needs), or dig into whether their Beauharnois VPS plans actually support cloud-init user-data?
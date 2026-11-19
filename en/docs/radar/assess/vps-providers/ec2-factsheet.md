

Good timing — the page answers some of your open questions and raises new ones. Here's the cross-reference against the README's "Open questions for Ex2" section:

## Answers from the page

| Open question | Status | What's shown |
|---|---|---|
| Debian 13 image | **Partial** | "Debian" is listed as an option. No version number stated. Custom ISO is confirmed as a fallback, so the netinst route works regardless. |
| Cloud-init user-data | **Not mentioned** | No reference to user-data, metadata, or cloud-init anywhere. Only SSH key injection is implied by the one-click app flow. This is the biggest unknown — it determines whether your `user-data.yaml` works or you fall back to `bootstrap.sh`. |
| Snapshots (separate from backups) | **Yes, partially** | "Snapshot and backup manager" in the self-service console, and "snapshots before risky changes" under the backup layer. No pricing, count, or retention details. Implied free but unconfirmed. |
| Provider-level firewall | **Not mentioned** | Only "Basic anti-DDoS protection on the network" is listed. That's upstream scrubbing, not a configurable firewall panel. Your nftables ruleset remains the only packet filter. |
| Reverse DNS | **Not mentioned** | Neither panel-set nor ticket-required. |
| IPv6 allocation (single vs /64) | **Concerning** | Plans say "1 IPv4 **or** IPv6 address." The "or" is ambiguous — does choosing IPv6 mean no IPv4? Your `verify.sh` expects both families. Additional IPv4 is +$5.47/mo, but no mention of additional or prefix IPv6. |

## Things that matter but weren't in your open questions

- **KVM console**: Confirmed, "Real KVM, not a wrapper." Matches your requirement for console access when SSH breaks.
- **Backups**: Daily, 3-day retention included. 7-day is +$9.39/mo. This is relevant to your rclone/pika offsite strategy — 3 days of provider-side retention is thin for a static site you might not notice is broken for a week.
- **Blacklist-free IPv4**: Nice but irrelevant for a static site with no mail server.
- **One-click upgrade, no migration**: Useful for scaling from NG1 → NG2 if the workload grows.
- **Monthly billing, 30-day refund**: Good for testing. You can spin one up, run the full Incus rehearsal → VPS rehearsal cycle, and bail if something's wrong.

## Plan recommendation for UC-Gradus

Your stack is deliberately minimal: sshd, Caddy, sshguard, nftables, unattended-upgrades. That's maybe 150–200 MB resident. But you run multi-tenant (path-routed domains on one box) and you've got Cake Metrics in the picture.

**VPS-NG 2** (2 vCPU, 2 GB RAM, 50 GB NVMe, **$12.52/mo**) is the floor. NG1's 1 GB is survivable for Caddy alone but leaves no room for a metrics agent, future log shipping, or the moment unattended-upgrades pulls a kernel and a reboot goes sideways. The $1.56 difference buys you the headroom to not think about RAM.

If you end up running more than a handful of sites or want a comfortable margin for the immutable-OS migration later, NG3 ($15.66, 4 GB) is the comfortable choice.

## What to confirm before ordering

1. **Cloud-init user-data support** — this is the one that changes your runbook. If they don't support it, you're on the `bootstrap.sh` path for first boot, which skips the `initial` user and umask policy (the playbook applies those later, so it's not a blocker, just a deviation from the ideal flow).
2. **Whether "Debian" means trixie** — if it's bookworm, you'd either wait or use the custom ISO route.
3. **IPv4 + IPv6 simultaneously** — ask whether you get both by default or must pick one. Your `verify.sh` and Caddy config assume dual-stack.
4. **Snapshot mechanics** — how many, how long, are they free, do they pause the VPS during capture.

Want me to draft a short inquiry email you could send them, or shall we keep moving on the flowcharts and treat these as "confirm at signup" items?
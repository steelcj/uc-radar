## Mesh Networking for Ansible Reach

The core problem: your Ansible controller needs reliable, authenticated reach to nodes regardless of where they sit physically. A mesh overlay solves this without requiring port forwarding, static IPs, or firewall exceptions.

### Top picks

**Headscale + Tailscale** (self-hosted WireGuard mesh)

- Tailscale client is open source; Headscale is the self-hosted coordination server
- Gives you a flat L3 network with stable IPs, ACL-based access control, and automatic NAT traversal
- Ansible just targets the Tailscale IP (or MagicDNS name) — no SSH tunneling, no jump hosts
- Works with your existing `ansible_host` / `ansible_ssh_host` variables cleanly
- Headscale runs in a container on your controller or any reachable box

**Netbird**

- Pure open source (client + server), WireGuard-based
- Self-hosted control plane, no dependency on any vendor's cloud
- Slightly less mature than Tailscale but fully auditable
- Same integration pattern: Ansible targets the Netbird IP

**ZeroTier**

- Virtual Ethernet (L2) rather than L3 — nodes appear on the same "subnet"
- Open source client, hosted controller (self-hosted controller exists but is less polished)
- Useful if you need L2 features (broadcast, mDNS) that pure L3 meshes don't provide
- Heavier client footprint than WireGuard-based options

**Nebula** (by defined.by/Slack)

- Certificate-based overlay, no coordination server needed at runtime (only for cert issuance)
- Very stable, very minimal. Less "mesh" and more "certificate-authenticated overlay"
- Good if you want a static, auditable trust model without a running control plane

### Integration with your Ansible workflow

```yaml
# inventories/raw/host_vars/disc-192-168-1-50.yml
ansible_host: 100.64.0.42 # Tailscale/Netbird IP
ansible_connection: ssh
```

The mesh IP replaces the physical IP in your inventory. The `promote_node.py` script could resolve the mesh address after enrollment. You'd add a bootstrap step that installs the mesh client, joins the network, then the rest of your playbooks proceed unchanged.

### Bootstrapping the mesh client on a raw node

Since raw nodes may not have Python, this fits your existing `raw` module pattern:

```yaml
- name: install tailscale on raw node
 raw: |
 curl -fsSL https://tailscale.com/install.sh | sh
 tailscale up --authkey=${TAILSCALE_AUTHKEY} --hostname={{ inventory_hostname }}
 when: needs_mesh | default(false)
```

---

## Boot / Provisioning Layer

**Ventoy** is excellent for manual one-off boots (drop ISOs on a stick, pick one at BIOS). It's not a fleet solution though.

For fleet provisioning, consider:

| Tool                  | Role                      | Notes                                                        |
| --------------------- | ------------------------- | ------------------------------------------------------------ |
| **netboot.xyz**       | Network boot menu         | Boots ISOs over PXE/iPXE, pairs well with Ventoy for hybrid workflows |
| **iPXE**              | Programmable network boot | Script-driven provisioning chain, integrates with MAAS or standalone |
| **MAAS**              | Bare-metal provisioning   | Canonical, open source. DHCP + DNS + IPAM + OS deploy. The closest "Landscape for bare metal" |
| **Cobbler / Foreman** | Legacy provisioning stack | Powerful but heavyweight. Foreman + Katello if you need subscription management |

MAAS is probably the strongest fit if you want "put a node on the rack, it gets an OS and is discoverable by your controller." It handles DHCP, DNS, IPAM, and OS installation, then hands off to your Ansible converge pipeline. It's also what Canonical uses internally for their own Landscape deployments.

---

## Management Plane Alternatives

You already have Ansible doing the converge work. The question is whether you want a persistent management UI on top, or whether Ansible + a lightweight web console is sufficient.

**Cockpit** — lightweight web console per node. Real-time terminal, logs, storage, services. No agent required on the controller side. Good for "I need to poke at a node interactively" without SSHing into a shell. Installable via your converge playbooks.

**AWX** (open source Ansible Tower) — web UI, job scheduling, RBAC, credential management for Ansible. Heavyweight (requires PostgreSQL, RabbitMQ/Redis, Kubernetes or docker-compose). Probably overkill for a test-node fleet.

**Ansible Semaphore** — lighter AWX alternative. Single binary, SQLite backend, web UI for running playbooks with history. Fits your scale better than AWX.

**OpenNebula** — this is an IaaS platform (manages VM lifecycle, similar to a mini OpenStack). It's not a node management tool. If your "remote nodes" are bare metal or containers you want configured, OpenNebula adds a layer you don't need. It would make sense if you were building a self-service cloud portal for humans requesting VMs.

**Canonical Landscape** — proprietary, Ubuntu-only, agent-based. Does package management, security patching, service monitoring. If you're on Debian (your `deb13` naming suggests Debian 13/Trixie), Landscape won't work. The open-source equivalent for Debian is essentially **Ansible + Cockpit + a package manager cron**, which you're already building toward.

---

## Recommended Architecture for Your Setup

Given your Incus + Ansible + uc-controller context:

```text
┌─────────────────────────────────────────────────────┐
│ Controller (your machine or a dedicated host) │
│ │
│ Headscale ←→ Tailscale/Netbird mesh IPs │
│ Ansible ←→ Targets mesh IPs via inventory │
│ Semaphore ←→ Optional: playbook history/UI │
└───────────────────────┬─────────────────────────────┘
 │ WireGuard overlay
 ┌───────────────┼───────────────┐
 │ │ │
 ┌────┴────┐ ┌─────┴────┐ ┌────┴────┐
 │ Node A │ │ Node B │ │ Node C │
 │ (Incus) │ │ (VMware) │ │ (bare) │
 └─────────┘ └──────────┘ └─────────┘
```

Bootstrap path: Ventoy/netboot.xyz for initial OS → mesh client install (your `raw` module play) → join mesh → Ansible converge proceeds as normal.

---

Want me to sketch the Headscale deployment as an Incus container, or draft the mesh-client bootstrap play in the style of your existing `10_onboard.yml`?

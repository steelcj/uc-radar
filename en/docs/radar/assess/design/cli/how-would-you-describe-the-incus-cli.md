# How would you describe the incus cli



Thought this through and looked things up online

It's a thin client wrapped around a single REST API, and almost everything distinctive about it follows from that one design decision.

**The grammar.** Commands read as `incus <verb> <resource>` for the common operations and `incus <resource> <verb>` for the administrative ones, so you get `incus launch images:debian/12 c1` alongside `incus config set`, `incus remote list`, `incus profile edit`, `incus storage create`. If you've used `lxc` from LXD, it's the same surface with a rename — the project's own docs describe the fork as "roughly equivalent to LXD 5.18" plus breaking changes. Muscle memory transfers almost entirely.

| Family         | Examples                                                   |
| -------------- | ---------------------------------------------------------- |
| Instances      | `launch`, `init`, `list`, `info`, `exec`, `stop`, `delete` |
| Configuration  | `config set/get/show/edit`, `profile`, `project`           |
| Infrastructure | `storage`, `network`, `cluster`, `remote`, `image`         |
| Convenience    | `alias`, `shell`, `snapshot`, `copy`, `move`               |

**Containers and VMs are the same object.** This is the part that distinguishes it from Docker more than anything else. `incus launch images:debian/12 my-container` gives you a system container; add `--vm` and you get a full virtual machine from the same command, sharing the same storage pools, networks, and profiles. `incus info` reports `Type: container` or `Type: virtual-machine`. You're managing *instances*, not two separate toolchains.

**Configuration is key/value and declarative.** `-c limits.cpu=4 -c limits.memory=4GiB` sets resources; `-d root,size=50GiB -d root,io.bus=nvme` overrides devices. Profiles bundle those settings and inherit, so a `default` profile plus a workload-specific one gets you a reproducible instance in one line. `incus launch` creates and starts; `incus init` only creates. You can also pipe YAML in on stdin, and `-t aws:t2.micro` borrows instance sizes from a catalogue of cloud shapes.

**Remotes make it a fleet client, not a local tool.** The CLI talks to the local daemon over a Unix socket by default, but `incus remote add` points it at any other Incus server or cluster over HTTPS, after which remote instances behave like local ones. The client config lives at `$HOME/.config/incus/config.yml` (overridable with `INCUS_CONF`) and ships with `images:` pointing at linuxcontainers.org as a public image server. The docs are explicit that you shouldn't hand-edit it — use `incus remote` and `incus alias`.

**Its mental model is a local cloud, not a container runtime.** The project describes the experience as "similar to that of a public cloud," and that's the right frame: projects for tenancy, clusters, storage pools, network abstractions, snapshots, ACLs. Docker's centre of gravity is building and shipping application images; Incus's is provisioning and administering long-lived machines and instances. Different jobs, and the CLI vocabulary reflects it.

**The part you'll care about most.** Incus exists because of a forkability event, which makes it a live case study for the proxy you wrote into your own framework. Canonical pulled LXD in-house in July 2023, then re-licensed it to AGPLv3 with a contributor agreement in December 2023 — at which point patch flow between the projects stopped in both directions, and LXD ended up under an unclear mix of Apache 2.0 and AGPLv3 files. Incus stayed Apache 2.0, community-led under the Linux Containers umbrella, with no corporate CLA, maintained by most of the people who originally built LXD. Nearly three years on it has shipped a second LTS: Incus 7.0 landed 5 May 2026 with support through June 2031, on top of monthly feature releases (7.5 in September 2026). So the answer to your reversibility question — *could this survive its steward turning hostile?* — isn't hypothetical here. It happened, and the fork outlived the dispute.

For your stack specifically, it also integrates with the tooling you already live in: Ansible, Terraform/OpenTofu, Packer, and Kubernetes Cluster API, with commercial support available through Zabbly. Given the Ansible-role-heavy history in your `csteel` and `cjsteel` accounts, that's a shorter path than most infrastructure migrations. There's also an `lxd-to-incus` tool if you have existing LXD hosts.

Sources: [Incus introduction](https://linuxcontainers.org/incus/), [client docs](https://linuxcontainers.org/incus/docs/main/client/), [remote servers](https://linuxcontainers.org/incus/docs/main/remotes/), [CLI config file](https://linuxcontainers.org/incus/docs/main/client-config/), [7.0 LTS announcement](https://linuxcontainers.org/incus/news/2026_05_05_16_29.html), [re-licensing news](https://linuxcontainers.org/incus/news/2023_12_21_18_44.html), and LWN's [report on the LXD re-licence](https://lwn.net/Articles/954777/).

One caveat: the docs pages I retrieved didn't show the `--format json|csv|yaml` output options, so I've left those out rather than assert them from LXD-era memory. Worth confirming if you plan to script against the CLI rather than the API.
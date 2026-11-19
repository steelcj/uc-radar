---
title: "ssh-audit: checking the cryptography of SSH servers and clients"
description: "What ssh-audit checks, how to install it with pipx, and how to read and act on its results."
date: 2026-11-06
tags: [ssh, security, hardening, auditing, linux]
---

# ssh-audit: checking the cryptography of SSH servers and clients

ssh-audit is an open source tool that connects to an SSH server, or poses as one for a client, and reports on the banner, key exchange, host key, encryption, MAC and compression algorithms on offer, flagging the ones that are weak or deprecated <a id="cite-testa-github"></a>([Testa, n.d.-a](#ref-testa-github)). The maintained version is Joe Testa's fork (v2.0 and later) of the original arthepsy project, which went inactive. It is MIT licensed, written in Python, and has no dependencies beyond Python itself.

It usually appears in setup notes like this:

```bash
pipx install ssh-audit                           # optional, for the crypto check
```

"Optional" because nothing breaks without it, and "crypto check" because its job is to confirm that the SSH hardening you applied actually took effect.

## What it checks

- the server banner and software version, with known vulnerabilities for that version
- key exchange algorithms (for example `curve25519-sha256`, `sntrup761x25519-sha512`, or weak `diffie-hellman-group1-sha1`)
- host key types and sizes (for example `ssh-ed25519` versus `ssh-rsa` with SHA-1)
- symmetric ciphers (for example `chacha20-poly1305` and AES-GCM versus CBC modes)
- message authentication codes (for example encrypt-then-MAC variants versus `hmac-sha1`)
- compression settings and protocol compatibility

Each algorithm is marked as good, warning or failure, with a short reason and, where it applies, the OpenSSH version that added or removed it.

## Installing

### With pipx (recommended on Linux Mint, Ubuntu and Debian)

```bash
pipx install ssh-audit
```

This keeps ssh-audit in its own environment and puts the `ssh-audit` command on your `PATH`. See the companion document on pipx for background.

### Other options

```bash
pip3 install ssh-audit                 # inside a virtual environment
snap install ssh-audit                 # Snap package
docker pull positronsecurity/ssh-audit # container image
```

Pre-built Windows binaries are on the project's releases page, and a hosted web version exists at ssh-audit.com for servers reachable from the internet.

## Common uses

### Audit a server

```bash
ssh-audit localhost
ssh-audit server.example.org
ssh-audit 192.0.2.10:2222
ssh-audit [2001:db8::1]:22
```

### Audit many servers

Put one `HOST[:PORT]` per line in a file, then:

```bash
ssh-audit -T servers.txt
```

### Audit a client

ssh-audit starts a listener (port 2222 by default) and grades whatever client connects to it:

```bash
ssh-audit -c
# in another terminal
ssh -p 2222 anything@localhost
```

### Policy audits

A policy is a pass or fail checklist of exactly which algorithms must be offered. List the built in policies, check a server against one, or generate a policy from a server you have already hardened:

```bash
ssh-audit -L
ssh-audit -P "Hardened OpenSSH Server v9.7 (version 1)" server.example.org
ssh-audit -M my_policy.txt server.example.org
ssh-audit -P my_policy.txt server.example.org
```

Policy names change as new OpenSSH releases appear, so copy the exact name from `ssh-audit -L`.

### Hardening guides

Hardening guides for common operating systems are built in:

```bash
ssh-audit --list-hardening-guides
ssh-audit --get-hardening-guide "Ubuntu 24.04 Server"
```

The same guides are published at ssh-audit.com.

## Using it in automation

ssh-audit returns an exit code of 0 when every algorithm passes a standard audit, or when a policy audit passes <a id="cite-testa-readme"></a>([Testa, n.d.-b](#ref-testa-readme)). A non-zero code means warnings or failures. That makes it a good final step after a hardening role or playbook:

```bash
if ssh-audit -P my_policy.txt "$host" > "audit-$host.txt"; then
  echo "$host: SSH crypto matches policy"
else
  echo "$host: SSH crypto drift, see audit-$host.txt"
fi
```

Use `-b` for batch output that is easier to parse, or `-j` for JSON.

## A note on the attack options

ssh-audit also includes `--dheat` and `--conn-rate-test`, which deliberately load a server to test whether it is exposed to the DHEat denial of service (CVE-2002-20001). These are attacks, not audits. Run them only against systems you own or have written permission to test, and expect them to affect service while they run.

## Reading the results

A typical first run against a default OpenSSH install shows a few warnings, often SHA-1 based MACs, `ssh-rsa` host key signatures, or NIST curve key exchange. The fix is usually a short drop in file under `/etc/ssh/sshd_config.d/` that restricts `KexAlgorithms`, `Ciphers`, `MACs` and `HostKeyAlgorithms`, followed by `sudo sshd -t` to validate and a restart of the SSH service. Keep a second session open while you test so a mistake cannot lock you out, then rerun ssh-audit to confirm the result.

Restricting algorithms can lock out older clients. For sites that still support older systems, check which clients connect before removing the last algorithm they share with the server.

## References

<a id="ref-testa-github"></a>
Testa, J. (n.d.-a). *ssh-audit* [Computer software]. GitHub. Retrieved November 6, 2026, from https://github.com/jtesta/ssh-audit
[Return to citation](#cite-testa-github)

<a id="ref-testa-readme"></a>
Testa, J. (n.d.-b). *ssh-audit README* [Source code documentation]. GitHub. Retrieved November 6, 2026, from https://github.com/jtesta/ssh-audit/blob/master/README.md
[Return to citation](#cite-testa-readme)

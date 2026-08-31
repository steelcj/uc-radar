# Caddy -- Web Server -- Manual Installation and Configuration on a Production Server

Version: 0.1.0
Status: Draft
Style Guide: style-guide--technical-documentation-for-technologists-v0.2.0.md

---

## Abstract

This document describes the manual installation and configuration of Caddy (version 2.11.2) as the web server for Hugo-generated static sites and statically generated webstores on a single Ubuntu 24 LTS production server.

It covers ingress conditions, directory structure, Caddy installation, Caddyfile configuration for both single-tenant and multi-tenant deployments, the release-based deployment workflow, maintenance mode, and operational notes.

The security, privacy, and systems praxis profile of Caddy in this context is documented in the radar entry and Universal Cake praxis evaluation, which should be reviewed before adoption.

---

## Sources and Acknowledgements

Caddy installation and configuration follows the <a name="apa-caddy-docs-citation"></a>[Caddy documentation (Caddy Authors, 2026)](#apa-caddy-docs-reference). Directory structure and deployment conventions follow the release pattern established in the Hugo hosting stack's stage environment. Authoring conventions follow the <a name="apa-styleguide-citation"></a>[style guide for technical documentation for technologists (Steel, 2026a)](#apa-styleguide-reference). Document formatting follows the <a name="apa-markdown-citation"></a>[web-ready unrendered markdown using APA 7 specification (Steel, 2026b)](#apa-markdown-reference).

---

## 1. Introduction

This document describes the manual installation and configuration of Caddy (version 2.11.2) as the web server for Hugo-generated static sites and statically generated webstores on a single Ubuntu 24 LTS production server.

We chose Caddy over the existing Apache2 implementation for three reasons:

* automatic TLS certificate provisioning and renewal via ACME eliminates Certbot and its associated cron and systemd dependencies;
* a single Caddyfile replaces Apache2's distributed configuration structure of `sites-available`, `sites-enabled`, and module management;
* and Caddy's single static binary is provider-agnostic, making the web server tier configuration-identical across Digital Ocean, Canadian, and Icelandic providers without distro-level packaging variation.

The security, privacy, and systems praxis profile of Caddy in this context is documented in the accompanying radar entry (`caddy--web-server-radar-entry-v0.1.1.md`) and Universal Cake praxis evaluation (`caddy--web-server--universal-cake-praxis-evaluation-v0.1.0.md`), which should be reviewed before adoption. This document assumes a server provisioned by the standard shim.

## 2. Ingress conditions

The shim delivers a freshly provisioned Ubuntu 24 LTS server with the following guaranteed state: a public IP address, an initial user with sudo access, SSH key authentication configured, and a provisional firewall permitting SSH only. Nothing else is assumed.

Caddy, Hugo, the deploy user, the directory structure, and all firewall rules beyond the SSH baseline are established by the steps in this document.

## 3. Directory structure

The directory structure follows the release pattern established in the Hugo hosting stack's stage environment, extended to production and generalised across tenants. Each domain receives its own directory tree under the deploy user's home. The structure for a single domain is:

```text
/home/deploy/deployments/prod/[domain]/
  hugo/
    releases/
      20260616T143000/   ← timestamped Hugo build output (public/)
      20260601T091500/   ← previous release, retained for rollback
    current            → releases/20260616T143000  ← symlink, repointed on deploy
    maintenance/         ← static maintenance page
  logs/
    caddy/
      access.log
      error.log
```

The Caddy document root points to `current`. Deployment creates a new timestamped directory under `releases/`, copies the Hugo build output into it, and repoints `current` atomically. Rollback is a single symlink repoint to any retained release. Maintenance mode is achieved by repointing `current` to `maintenance/`. Release directories are retained according to operator preference; we recommend retaining the three most recent releases and pruning older ones on each deployment.

Create the structure for all four domains as follows:

```bash
sudo useradd --system --shell /bin/bash --create-home deploy

for DOMAIN in example1-site.ca example1-store.ca example2-site.ca example2-store.ca; do
  sudo mkdir -p /home/deploy/deployments/prod/${DOMAIN}/hugo/releases
  sudo mkdir -p /home/deploy/deployments/prod/${DOMAIN}/hugo/maintenance
  sudo mkdir -p /home/deploy/deployments/prod/${DOMAIN}/logs/caddy
  sudo chown -R deploy:deploy /home/deploy/deployments/prod/${DOMAIN}
done
```

Create a minimal maintenance page for each domain:

```bash
for DOMAIN in example1-site.ca example1-store.ca example2-site.ca example2-store.ca; do
  cat <<'EOF' | sudo tee /home/deploy/deployments/prod/${DOMAIN}/hugo/maintenance/index.html
<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Maintenance</title></head>
<body><h1>Maintenance in progress</h1><p>This site will return shortly.</p></body>
</html>
EOF
  sudo chown deploy:deploy /home/deploy/deployments/prod/${DOMAIN}/hugo/maintenance/index.html
done
```

## 4. Caddy installation

Caddy is installed from the official Caddy apt repository. This ensures the package is signed, pinned to the Caddy project's release channel, and upgradable via standard apt tooling.

```bash
sudo apt update
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' \
  | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' \
  | sudo tee /etc/apt/sources.list.d/caddy-stable.list

sudo apt update
sudo apt install -y caddy=2.11.2
```

Pin the version to prevent unattended upgrades from installing a new release before it has been reviewed:

```bash
sudo apt-mark hold caddy
```

Verify the installation:

```bash
caddy version
```

The expected output is `v2.11.2`. Caddy installs as a systemd service (`caddy.service`) and starts automatically. The default configuration serves a placeholder page; it is replaced in the next section.

## 5. Firewall configuration

The shim's provisional firewall permits SSH only. Open ports 80 and 443 for Caddy:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 443/udp
```

Port 443 UDP is required for HTTP/3 (QUIC), which Caddy enables by default. Verify the ruleset:

```bash
sudo ufw status
```

## 6. Caddyfile configuration

Caddy's configuration lives in a single file at `/etc/caddy/Caddyfile`. The default file installed by the package is replaced in full. Two configurations are provided: a single-tenant example for a server hosting one domain pair, and the multi-tenant configuration for all four domains used in this document.

### 6.1 Global options

The global options block appears at the top of every Caddyfile. It sets the ACME contact email, configures the Caddy data directory, and sets log output.

```caddyfile
{
  email admin@example1-site.ca
  storage file_system /home/deploy/.local/share/caddy
  log {
    output file /var/log/caddy/caddy.log
    format json
  }
}
```

Create the Caddy log directory:

```bash
sudo mkdir -p /var/log/caddy
sudo chown caddy:caddy /var/log/caddy
```

Create the Caddy data directory under the deploy user, or use the Caddy system user's home depending on your preference for certificate storage ownership. The configuration above stores certificates under `/home/deploy/.local/share/caddy`; adjust to `/var/lib/caddy/.local/share/caddy` if certificates should be owned by the Caddy system user rather than the deploy user.

### 6.2 Single-tenant configuration

A server hosting one site and one store for a single tenant:

```caddyfile
{
  email admin@example1-site.ca
  storage file_system /var/lib/caddy/.local/share/caddy
  log {
    output file /var/log/caddy/caddy.log
    format json
  }
}

example1-site.ca {
  root * /home/deploy/deployments/prod/example1-site.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example1-site.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}

example1-store.ca {
  root * /home/deploy/deployments/prod/example1-store.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example1-store.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}
```

### 6.3 Multi-tenant configuration

A server hosting two tenants, each with a site and a store. The configuration is additive — each domain block is independent. This confirms that multi-tenant hosting in a single Caddyfile is both viable and straightforward: Caddy handles TLS for all four domains automatically, each with its own certificate, and each domain serves from its own document root. There is no shared state between tenants at the Caddy configuration level.

```caddyfile
{
  email admin@example1-site.ca
  storage file_system /var/lib/caddy/.local/share/caddy
  log {
    output file /var/log/caddy/caddy.log
    format json
  }
}

example1-site.ca {
  root * /home/deploy/deployments/prod/example1-site.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example1-site.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}

example1-store.ca {
  root * /home/deploy/deployments/prod/example1-store.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example1-store.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}

example2-site.ca {
  root * /home/deploy/deployments/prod/example2-site.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example2-site.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}

example2-store.ca {
  root * /home/deploy/deployments/prod/example2-store.ca/hugo/current
  file_server
  encode zstd gzip
  log {
    output file /home/deploy/deployments/prod/example2-store.ca/logs/caddy/access.log
    format json
  }
  handle_errors {
    rewrite * /404.html
    file_server
  }
}
```

Write this configuration to `/etc/caddy/Caddyfile`, then validate and reload:

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
```

Caddy will begin provisioning TLS certificates for all four domains immediately on reload. DNS A records for all four domains must point to the server's public IP address before this step, or ACME challenges will fail.

## 7. Deployment workflow

A deployment consists of three steps: creating a timestamped release directory, copying Hugo build output into it, and repointing the `current` symlink. The following script performs all three steps for a given domain. It is run as the deploy user on the server, triggered by a git post-receive hook or run manually.

```bash
#!/bin/bash
set -euo pipefail

DOMAIN="${1:?Usage: deploy.sh <domain> <build-output-path>}"
BUILD="${2:?Usage: deploy.sh <domain> <build-output-path>}"
RELEASE_DIR="/home/deploy/deployments/prod/${DOMAIN}/hugo/releases/$(date +%Y%m%dT%H%M%S)"
CURRENT="/home/deploy/deployments/prod/${DOMAIN}/hugo/current"

mkdir -p "${RELEASE_DIR}"
cp -r "${BUILD}/." "${RELEASE_DIR}/"
ln -sfn "${RELEASE_DIR}" "${CURRENT}"

echo "Deployed ${DOMAIN} → ${RELEASE_DIR}"
```

Save this script as `/home/deploy/bin/deploy.sh` and make it executable:

```bash
mkdir -p /home/deploy/bin
chmod +x /home/deploy/bin/deploy.sh
```

To deploy `example1-site.ca` after a Hugo build that outputs to `/tmp/hugo-build/public`:

```bash
/home/deploy/bin/deploy.sh example1-site.ca /tmp/hugo-build/public
```

Caddy reads the document root on each request; no reload is required after a deployment. The symlink repoint is atomic from the operating system's perspective — no request will be served from a partially written release directory.

### 7.1 Rollback

To roll back to a previous release, repoint the `current` symlink to any retained release directory:

```bash
ln -sfn /home/deploy/deployments/prod/example1-site.ca/hugo/releases/20260601T091500 \
  /home/deploy/deployments/prod/example1-site.ca/hugo/current
```

No Caddy reload is required.

### 7.2 Release pruning

Retain the three most recent releases and remove older ones after each deployment. Add the following to the deploy script after the symlink repoint:

```bash
ls -1dt /home/deploy/deployments/prod/${DOMAIN}/hugo/releases/*/ \
  | tail -n +4 \
  | xargs rm -rf
```

## 8. Maintenance mode

To put a domain into maintenance mode, repoint `current` to the `maintenance/` directory:

```bash
ln -sfn /home/deploy/deployments/prod/example1-site.ca/hugo/maintenance \
  /home/deploy/deployments/prod/example1-site.ca/hugo/current
```

No Caddy reload is required. To return the site to the last production release:

```bash
LATEST=$(ls -1dt /home/deploy/deployments/prod/example1-site.ca/hugo/releases/*/ | head -1)
ln -sfn "${LATEST}" /home/deploy/deployments/prod/example1-site.ca/hugo/current
```

## 9. Operational notes

### 9.1 TLS certificate storage

Caddy stores TLS certificates and ACME account keys under the configured storage path (`/var/lib/caddy/.local/share/caddy` in the configurations above). This directory should be included in server backups. If the server is reprovisioned, existing certificates can be restored to this path and Caddy will use them without re-issuing, subject to their remaining validity period.

### 9.2 Checking certificate status

```bash
sudo caddy environ
sudo journalctl -u caddy --since "1 hour ago"
```

Certificate renewal is automatic. Caddy renews certificates when they have less than one third of their validity period remaining, typically around 30 days before expiry for 90-day Let's Encrypt certificates.

### 9.3 Reloading vs restarting

Caddy supports zero-downtime configuration reloads. Always prefer reload over restart:

```bash
sudo systemctl reload caddy
```

A restart causes a brief interruption during which in-flight requests are dropped. A reload applies the new configuration without dropping connections.

### 9.4 Log rotation

Caddy writes structured JSON logs. Configure logrotate for both the global Caddy log and per-domain access logs:

```bash
cat <<'EOF' | sudo tee /etc/logrotate.d/caddy
/var/log/caddy/*.log
/home/deploy/deployments/prod/*/logs/caddy/*.log {
  daily
  rotate 14
  compress
  delaycompress
  missingok
  notifempty
  sharedscripts
  postrotate
    systemctl reload caddy > /dev/null 2>&1 || true
  endscript
}
EOF
```

### 9.5 Multi-tenant considerations

The multi-tenant configuration in section 6.3 confirms that a single Caddyfile managing multiple tenant domains is viable, straightforward to reason about, and operationally no more complex than a single-tenant configuration. Each domain block is fully independent. TLS certificates are provisioned per domain. Log files are per domain. There is no configuration entanglement between tenants. The question of whether to run separate Caddy instances per tenant does not arise at this scale — a single instance with a single Caddyfile is the correct and simpler choice. Separate instances would be warranted only if tenant isolation at the process level became a requirement, for example in a regulated environment where one tenant's traffic must be provably inaccessible to another's process space.

---

## Resources

### Caddy
- [Caddy documentation](https://caddyserver.com/docs)
- [Caddy automatic HTTPS](https://caddyserver.com/docs/automatic-https)
- [Caddy Caddyfile reference](https://caddyserver.com/docs/caddyfile)
- [Caddy 2.11.2 release notes](https://github.com/caddyserver/caddy/releases/tag/v2.11.2)

### Governing documents
- [Style guide for technical documentation for technologists](#apa-styleguide-reference)
- [Web-ready unrendered markdown using APA 7](#apa-markdown-reference)

### Accompanying documents
- caddy--web-server-radar-entry-v0.1.1.md
- caddy--web-server--universal-cake-praxis-evaluation-v0.1.0.md

---

## References

<a name="apa-caddy-docs-reference"></a>Caddy Authors. (2026). *Caddy documentation*. Caddy. https://caddyserver.com/docs
[Return to citation](#apa-caddy-docs-citation)

<a name="apa-styleguide-reference"></a>Steel, C. (2026a). *Style guide: Technical documentation for technologists* (Version 0.2.0) [Technical document]. https://universalcake.ca
[Return to citation](#apa-styleguide-citation)

<a name="apa-markdown-reference"></a>Steel, C. (2026b). *Web-ready unrendered markdown using APA 7* (Version 0.2.2) [Technical document]. https://universalcake.ca
[Return to citation](#apa-markdown-citation)

---

## Changelog

| Version | Status | Notes |
|---|---|---|
| 0.1.0 | Draft | Initial draft |

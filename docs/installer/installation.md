---
title: Installation
parent: Installer
nav_order: 1
---

# Installation

This page walks through a full interactive installation of Parley Chat using the installer.

## Step 1 — Download and run

```sh
curl -fsSL https://github.com/Parley-Chat/installer/releases/latest/download/install.sh | bash
```

The bootstrap script detects your architecture (x86_64 or arm64), downloads the matching installer binary, and launches it.

## Step 2 — Choose Install mode

```
[R] Recommended - quick setup with defaults
[C] Custom      - configure everything
```

**Recommended** sets sensible defaults (random port, 32 threads, nginx enabled, voice calls off). You only provide the domain/IP, URI prefix, and optional instance password.

**Custom** lets you configure every option:

| Option | Description | Default |
|--------|-------------|---------|
| nginx reverse proxy | Use the built-in nginx for HTTPS termination | Yes |
| HTTPS port | Port nginx listens on | Random (10000–49151) |
| Sova internal port | Port the backend binds to | 42836 |
| Sova bind host | Address Sova listens on | 127.0.0.1 |
| Install directory | Where all files are placed | `/opt/parley-chat` |
| Instance password | Require a password to create accounts | None |
| Auto-invite channel | Channel ID to auto-join on registration | None |
| Threads | Number of Sova worker threads | 32 |
| Voice calls | Enable WebRTC voice/video calls | No |

## Step 3 — Domain or IP

Enter the domain name or IP address users will connect to. This is used in the nginx config and SSL certificate.

{: .note }
If you enter an IP address, a self-signed certificate is generated automatically — Let's Encrypt does not issue certificates for IP addresses.

## Step 4 — URI prefix

A random 20-character path prefix is suggested (e.g. `abc123.../`). You can accept the default or enter your own. This prefix must appear in every URL to reach the instance.

## Step 5 — SSL certificate

If you chose nginx and entered a domain name:

```
SSL Certificate:
[1] Self-signed (works everywhere, browser warning on first visit)
[2] Let's Encrypt - HTTP verification (port 80 must be open from internet, auto-renews)
[3] Let's Encrypt - DNS verification (works behind firewall, requires adding a DNS TXT record)
[4] Use existing certificates (provide paths)
```

| Option | Use when |
|--------|----------|
| Self-signed | Internal/private servers, or when you don't mind the browser warning |
| Let's Encrypt HTTP | Public server with port 80 open — simplest option, auto-renews via cron |
| Let's Encrypt DNS | Server behind a firewall or NAT — requires adding a TXT record to your DNS |
| Use existing certificates | You already have a certificate and key in PEM format; provide the file paths |

{: .note }
Let's Encrypt HTTP adds a cron job to renew the certificate every 12 hours automatically.

{: .warning }
Let's Encrypt DNS certificates must be renewed manually every 90 days: run `certbot renew --manual --preferred-challenges dns`, then `systemctl restart parley-chat-nginx`.

## Step 6 — Auto-update (online installs only)

```
Enable automatic daily updates? [y/N]:
```

If enabled, the installer writes an `auto-update.sh` script in the install directory and adds a cron job that runs it every day at 3 AM. See [Auto-Update](auto-update.md) for details.

## Step 7 — Done

The installer prints the URL of your instance:

```
Parley Chat is running at https://example.com:12345/abc123.../
```

## What gets installed

| Path | Contents |
|------|----------|
| `/opt/parley-chat/sova` | Sova backend binary |
| `/opt/parley-chat/mura/` | Mura frontend static files |
| `/opt/parley-chat/config.toml` | Sova configuration |
| `/opt/parley-chat/nginx.conf` | nginx configuration |
| `/opt/parley-chat/certs/` | SSL certificate and key (self-signed only) |
| `/opt/parley-chat/data/` | Database, profile pictures, attachments |
| `/opt/parley-chat/.install_info` | Saved settings used by Modify and auto-update |
| `/opt/parley-chat/auto-update.sh` | Auto-update script (if enabled) |
| `/etc/systemd/system/parley-chat.service` | Sova systemd service |
| `/etc/systemd/system/parley-chat-nginx.service` | nginx systemd service |

---
title: Installer
nav_order: 5
has_children: true
---

# Parley Chat Installer

The Parley Chat Installer is the recommended way to deploy Parley Chat on a Linux server. It sets up the Sova backend, Mura frontend, nginx reverse proxy, and SSL certificates in a single interactive session — no Docker or manual configuration required.

## Quick Start

Run this one-liner as root on your server:

```sh
curl -fsSL https://github.com/Parley-Chat/installer/releases/latest/download/install.sh | bash
```

Or with `wget`:

```sh
wget -qO- https://github.com/Parley-Chat/installer/releases/latest/download/install.sh | bash
```

The script downloads the installer binary for your architecture and launches the interactive menu.

## Requirements

- Linux (x86_64 or arm64)
- Root / sudo access
- Internet access (or a local archive — see [Local Installation](local-installation.md))
- Ports: one HTTPS port of your choice (default: random 10000–49151), port 80 open if using Let's Encrypt HTTP verification

## Supported Distributions

Any distribution with one of the following package managers:

- `apt-get` (Debian, Ubuntu)
- `dnf` (Fedora, RHEL 8+)
- `yum` (CentOS, RHEL 7)
- `pacman` (Arch Linux)

## Menu Overview

When launched, the installer shows:

```
=== Parley Chat Installer ===

[I] Install
[U] Update
[M] Modify
[X] Uninstall
```

| Option | Description |
|--------|-------------|
| **Install** | Fresh installation with guided setup |
| **Update** | Download and apply the latest Sova and Mura releases |
| **Modify** | Change SSL certificate or toggle auto-update |
| **Uninstall** | Stop services and remove all installed files |

## Pages

- [Installation](installation.md) — step-by-step install walkthrough
- [Local Installation](local-installation.md) — install without internet access using a bundled archive
- [Auto-Update](auto-update.md) — keep Parley Chat updated automatically
- [Modify & Renew Certificates](modify.md) — post-install configuration changes
- [Custom Mirror](custom-mirror.md) — use a mirror instead of GitHub releases

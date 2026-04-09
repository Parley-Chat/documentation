---
title: Custom Mirror
parent: Installer
nav_order: 5
---

# Custom Mirror

By default, the installer downloads Sova and Mura from the official GitHub Releases page. You can point it at a custom mirror instead by setting the `MIRROR_BASE_URL` environment variable.

## Using a mirror with the bootstrap script

```sh
MIRROR_BASE_URL="https://your-mirror.example.com/parley" \
  curl -fsSL https://github.com/Parley-Chat/installer/releases/latest/download/install.sh | bash
```

## Using a mirror with the installer binary directly

```sh
MIRROR_BASE_URL="https://your-mirror.example.com/parley" sudo ./installer-linux-x64
```

## Mirror requirements

The mirror must serve these files at the specified base URL:

| File | Description |
|------|-------------|
| `sova-linux-x64` | Sova backend binary for x86_64 |
| `sova-linux-arm64` | Sova backend binary for arm64 |
| `mura.zip` | Mura frontend archive |

For example, if `MIRROR_BASE_URL=https://mirror.example.com/parley`, the installer will fetch:

```
https://mirror.example.com/parley/sova-linux-x64
https://mirror.example.com/parley/mura.zip
```

## Mirror URL in auto-update

When auto-update is enabled, the mirror URL is baked into `/opt/parley-chat/auto-update.sh` at install time. If you change your mirror later, re-enable auto-update from the **Modify** menu — this regenerates the script with the current `MIRROR_BASE_URL`.

{: .note }
For air-gapped or fully offline servers, use [Local Installation](local-installation.md) instead of a mirror.

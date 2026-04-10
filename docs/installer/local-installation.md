---
title: Local Installation
parent: Installer
nav_order: 2
---

# Local Installation

If your server has no internet access, you can install Parley Chat from a locally downloaded archive instead of pulling files from GitHub.

## How it works

When the installer starts, it checks whether `sova-linux-<arch>` and `mura.zip` are present in the same directory as the installer binary. If they are, it copies from those local files instead of downloading anything.

## Steps

**1. On a machine with internet access, download the release archive**

Go to the [latest release](https://github.com/Parley-Chat/installer/releases/latest) and download:

- `installer-linux-x64` or `installer-linux-arm64` (matching your server's CPU)
- `sova-linux-x64` or `sova-linux-arm64` (same architecture)
- `mura.zip`

**2. Transfer all three files to your server**

Place them in the same directory, for example:

```
/root/parley-install/
├── installer-linux-x64
├── sova-linux-x64
└── mura.zip
```

**3. Make the installer executable and run it**

```sh
chmod +x installer-linux-x64
sudo ./installer-linux-x64
```

The installer detects the local files and prints:

```
[Local mode] Using bundled files from archive.
```

It then proceeds through the normal installation menu without making any network requests for Sova or Mura.

{: .note }
SSL certificate setup (Let's Encrypt) still requires internet access. Use self-signed certificates for fully air-gapped servers.

## Updating offline

To update an offline installation, download the new `sova-linux-<arch>` and `mura.zip` from the releases page, transfer them to the server alongside the installer, and run the installer — choosing **Update** from the menu. The same local-mode detection applies.

## Packaging as a tar archive

For convenience you can bundle the three files into a single archive to transfer:

```sh
tar czf parley-install.tar.gz installer-linux-x64 sova-linux-x64 mura.zip
```

On the server:

```sh
tar xzf parley-install.tar.gz
chmod +x installer-linux-x64
sudo ./installer-linux-x64
```

---
title: Modify & Renew Certificates
parent: Installer
nav_order: 4
---

# Modify & Renew Certificates

The **Modify** menu lets you make post-install changes without reinstalling.

## Opening the Modify menu

```sh
sudo /opt/parley-chat/installer-linux-x64
# Choose [M] Modify
```

Enter the install directory when prompted (default: `/opt/parley-chat`).

## Available options

```
[C] Renew SSL certificate
[A] Enable auto-update     (or "Disable auto-update" if already enabled)
```

## Renew SSL certificate

Selecting **[C]** runs the same SSL setup flow as during installation, letting you:

- Switch from self-signed to Let's Encrypt (or vice versa)
- Re-issue a Let's Encrypt certificate with a new email address
- Regenerate an expired self-signed certificate

After the new certificate is issued, the installer:

1. Updates `nginx.conf` with the new certificate paths
2. Restarts `parley-chat-nginx` to apply the change
3. Updates `.install_info` to record the new SSL type

{: .note }
This option is only available when nginx is in use. If you chose **no nginx** during installation, certificate management is handled by your external reverse proxy.

### Certificate options

| Option | When to use |
|--------|-------------|
| Self-signed | Internal servers, or when Let's Encrypt is unavailable |
| Let's Encrypt HTTP | Public server with port 80 open — auto-renews via cron |
| Let's Encrypt DNS | Server behind a firewall — requires DNS TXT record |
| Use existing certificates | You already have a certificate and key in PEM format; provide the file paths |

{: .warning }
Let's Encrypt DNS certificates require manual renewal every 90 days. Run `certbot renew --manual --preferred-challenges dns` and then `systemctl restart parley-chat-nginx`.

## Toggle auto-update

Selecting **[A]** enables or disables automatic daily updates depending on the current state.

- If auto-update is **off** → enables it (creates `auto-update.sh` and adds cron entry)
- If auto-update is **on** → disables it (removes the cron entry)

See [Auto-Update](auto-update.md) for full details on how the update process works.

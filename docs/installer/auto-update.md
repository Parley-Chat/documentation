---
title: Auto-Update
parent: Installer
nav_order: 3
---

# Auto-Update

The auto-update feature keeps your Parley Chat installation up to date automatically by running a daily update script via cron.

## Enabling during installation

When installing in online mode (not from a local archive), the installer asks:

```
Enable automatic daily updates? [y/N]:
```

Enter `y` to enable. You are then asked to choose the update schedule:

```
Auto-update schedule:
[1] Every 5 minutes
[2] Every hour
[3] Daily at 3 AM
[4] Daily at 4 AM
[5] Custom (enter cron expression)
```

The installer will:

1. Write `/opt/parley-chat/auto-update.sh` with your mirror URL and architecture baked in
2. Add a cron entry on the schedule you chose

## Enabling or disabling after installation

Use the **Modify** menu:

```sh
sudo /opt/parley-chat/installer-linux-x64
# Choose [M] Modify
# Choose [A] Enable auto-update  (or Disable if already on)
```

See [Modify & Renew Certificates](modify.md) for the full Modify menu reference.

## What the update script does

The script (`/opt/parley-chat/auto-update.sh`) performs these steps every time it runs:

1. Stops the `parley-chat` systemd service
2. Downloads the latest `sova-linux-<arch>` binary from the configured mirror
3. Downloads the latest `mura.zip` from the mirror
4. Atomically replaces the old binary (`sova.new` → `sova`)
5. Replaces the `mura/` frontend directory
6. Starts the `parley-chat` service

Output is appended to `/var/log/parley-chat-update.log`.

## Viewing update logs

```sh
tail -f /var/log/parley-chat-update.log
```

## Cron entry format

The cron expression depends on the schedule you chose:

| Schedule | Cron expression |
|----------|----------------|
| Every 5 minutes | `*/5 * * * *` |
| Every hour | `0 * * * *` |
| Daily at 3 AM | `0 3 * * *` |
| Daily at 4 AM | `0 4 * * *` |
| Custom | whatever you enter |

Example entry (daily at 3 AM):

```
0 3 * * * /opt/parley-chat/auto-update.sh >> /var/log/parley-chat-update.log 2>&1
```

To view or edit the cron entry manually:

```sh
crontab -e
```

## Notes

- Auto-update is **not available** for local (air-gapped) installs — there is no mirror to pull from
- The nginx config and `config.toml` are never overwritten by auto-update; only the binaries and frontend are replaced
- The `parley-chat-nginx` service is not restarted by auto-update — only `parley-chat` (Sova) is cycled

# Lab State Snapshot
Date: 2026-02-17
Timezone: EST

## Hosts
- `dallas-MacMint` - `dallas@<redacted-ip>`
- `asus-server` - `dallas@<redacted-ip>`
- `pi-core` - `dallaspi@<redacted-ip>`

## Security Baseline
### dallas-MacMint
- SSH hardened:
  - `PermitRootLogin no`
  - `PasswordAuthentication no`
  - `KbdInteractiveAuthentication no`
  - `MaxAuthTries 3`
  - `X11Forwarding no`
  - `AllowUsers dallas`
- UFW active:
  - `22/tcp` from `<redacted-ip>/24`
  - `61208/tcp` from `<redacted-ip>`
- fail2ban active (`sshd` jail)
- Glances active on `:61208`

### asus-server
- UFW active, default deny incoming, LAN-scoped service rules
- SSH hardened (`PasswordAuthentication no`, root login disabled)
- fail2ban active (`sshd` jail)
- HTTPS reverse proxy (NPM) active for core `.lan` services

### pi-core
- UFW active, LAN-scoped allows (`22`, `53/tcp+udp`, `80`, `1883`, `2586`) plus `61208` from ASUS
- SSH hardened (`PasswordAuthentication no`, root login disabled)
- fail2ban active (`sshd` jail)

## Core Services
### ASUS Docker
- `homepage` (3000)
- `uptimekuma` (3001)
- `nginxproxymanager` (80/443/8181)
- `portainer` (8000/9000/9443)
- `duplicati` (8200)
- `big-bear-syncthing` (8384/22000/21027)
- `immich-*` stack (2283)

### pi-core additions (new)
- `Unbound` (`127.0.0.1:5335`) integrated as Pi-hole upstream
- `Mosquitto` (`0.0.0.0:1883`) with auth enabled
- `ntfy` container (`0.0.0.0:2586 -> 80`)
- `Glances` (`0.0.0.0:61208`)

## Homepage
Config path:
- `/DATA/AppData/big-bear-homepage/config`

Current highlights:
- `Switch Web UI` shortcut in Daily Ops
- Top widgets include:
  - local ASUS resources
  - `dallas-MacMint` glances
  - `pi-core` glances
- Services include:
  - `ntfy` (`http://pi-core.lan:2586`)
  - `Mosquitto MQTT` (`mqtt://pi-core.lan:1883`)

## Backup and DR
Primary backup target (largest storage):
- `/mnt/externalssd/lab-backups` on ASUS (`/dev/sdb2`, ~1.9T)

Primary local backup automation:
- Script: `/usr/local/sbin/lab-nightly-backup.sh`
- Timer/service:
  - `/etc/systemd/system/lab-nightly-backup.timer`
  - `/etc/systemd/system/lab-nightly-backup.service`
- Schedule: daily around 02:30

Off-host replication (same site, different host):
- Source: `/mnt/externalssd/lab-backups`
- Destination: `dallas-MacMint:/home/dallas/lab-offsite-backups`
- Script: `/usr/local/sbin/lab-offsite-sync.sh`
- Timer/service:
  - `/etc/systemd/system/lab-offsite-sync.timer`
  - `/etc/systemd/system/lab-offsite-sync.service`
- Schedule: daily around 03:00

Backup monitoring and alerting:
- Script: `/usr/local/sbin/lab-backup-healthcheck.sh`
- Timer/service:
  - `/etc/systemd/system/lab-backup-healthcheck.timer`
  - `/etc/systemd/system/lab-backup-healthcheck.service`
- State file: `/var/lib/lab-monitor/backup-health.state`
- Alerts: `ntfy` topic `lab-alerts` (`http://pi-core.lan:2586/lab-alerts`)

Latest validated run:
- `/mnt/externalssd/lab-backups/runs/20260217_011016`
- Checksums and restore-readability verified

## Known URLs
- Homepage: `https://homepage.lan`
- Uptime Kuma: `https://kuma.lan`
- CasaOS: `https://casaos.lan`
- NPM Admin: `https://npm.lan`
- Portainer: `https://portainer.lan`
- Duplicati: `https://duplicati.lan`
- Immich: `https://immich.lan`
- Syncthing: `https://syncthing.lan`
- Pi-hole: `https://pihole.lan/admin`
- Switch UI: `https://switch.lan` (GET works; HEAD may fail)
- ntfy: `http://pi-core.lan:2586`

## Notes
- GitHub CLI auth token currently invalid on local host (`gh auth status` fails).
- Off-host copy is in-lab only; cloud/geographic offsite is still optional next step.

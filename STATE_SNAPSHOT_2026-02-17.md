# Lab State Snapshot
Date: 2026-02-17
Timezone: EST

## Hosts
- `dallas-MacMint` - `dallas@<REDACTED_LAN_IP>`
- `asus-server` - `dallas@<REDACTED_LAN_IP>`
- `pi-core` - `dallaspi@<REDACTED_LAN_IP>`

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
  - `22/tcp` from `<REDACTED_LAN_IP>`
  - `61208/tcp` from `<REDACTED_LAN_IP>`
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
- Uptime Kuma monitors include AP infrastructure:
  - `Gateway (<REDACTED_LAN_IP>)` (ping)
  - `TP-Link AP (<REDACTED_LAN_IP>)` (ping)

### pi-core
- `Unbound` (`127.0.0.1:5335`) integrated as Pi-hole upstream
- `Mosquitto` (`0.0.0.0:1883`) with auth enabled
- `ntfy` container (`0.0.0.0:2586 -> 80`)
- `Glances` (`0.0.0.0:61208`)

## Backup and DR
Primary backup target:
- `/mnt/externalssd/lab-backups` on ASUS (`/dev/sdb2`, ~1.9T)

Primary local backup automation (ASUS):
- Script: `/usr/local/sbin/lab-nightly-backup.sh`
- Timer/service:
  - `/etc/systemd/system/lab-nightly-backup.timer`
  - `/etc/systemd/system/lab-nightly-backup.service`

Off-host replication (ASUS -> MacMint):
- Script: `/usr/local/sbin/lab-offsite-sync.sh`
- Timer/service:
  - `/etc/systemd/system/lab-offsite-sync.timer`
  - `/etc/systemd/system/lab-offsite-sync.service`

Backup monitoring and alerting (ASUS):
- Script: `/usr/local/sbin/lab-backup-healthcheck.sh`
- Timer/service:
  - `/etc/systemd/system/lab-backup-healthcheck.timer`
  - `/etc/systemd/system/lab-backup-healthcheck.service`
- State file: `/var/lib/lab-monitor/backup-health.state`
- Alerts: `ntfy` topic `lab-alerts` (`http://pi-core.lan:2586/lab-alerts`)

## Monitoring and Ops Automation
- Local monthly health automation:
  - Script: `/home/dallas/scripts/lab-monthly-healthcheck.sh`
  - Service: `/etc/systemd/system/lab-monthly-healthcheck.service`
  - Timer: `/etc/systemd/system/lab-monthly-healthcheck.timer`
- Monthly health script appends directly to:
  - `/home/dallas/lab-notes/SESSION_LOG.md`

## Standardized Failure Alerting (ASUS)
- Shared failure alert script:
  - `/usr/local/sbin/lab-systemd-failure-ntfy.sh`
- Shared systemd template:
  - `/etc/systemd/system/lab-systemd-failure-alert@.service`
- Critical units use:
  - `OnFailure=lab-systemd-failure-alert@%n.service`
- Legacy per-job alert scripts/units archived at:
  - `/root/lab-deprecated-alerts/20260217_092752`

## ClamAV Mode (MacMint)
- `clamav-daemon`: disabled/inactive (no persistent scanner)
- `clamav-daemon.socket`: disabled/inactive
- `clamav-freshclam`: enabled/active (signature updates retained)
- Daily scheduled on-demand scan:
  - Script: `/usr/local/sbin/clamav-daily-scan.sh`
  - Service: `/etc/systemd/system/clamav-daily-scan.service`
  - Timer: `/etc/systemd/system/clamav-daily-scan.timer`
  - Logs: `/var/log/clamav/daily-scan-*.log`
  - Latest symlink: `/var/log/clamav/daily-scan-latest.log`
  - ntfy alerts only on infections/errors: `http://pi-core.lan:2586/lab-alerts`

## Recent Cleanup Actions
### dallas-MacMint
- User + apt cache cleanup completed.
- Removed: `discord`, Flatpak `md.obsidian.Obsidian`, Flatpak `net.ankiweb.Anki`.
- Removed unused Flatpak runtimes.

### asus-server
- apt cache cleaned.
- journald logs vacuumed to ~500MB target.

### pi-core
- apt cache cleaned.
- unused Docker images pruned (`~793MB reclaimed`).

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
- Switch UI: `https://switch.lan`
- ntfy: `http://pi-core.lan:2586`

## Notes
- Last package maintenance run: `2026-02-17T17:36:42-05:00` (EST); updates applied on all hosts, with `pi-core` package `libpng16-16` upgraded and restart advisory handled.
- Last full validation run: `2026-02-17T17:58:55-05:00` (EST); monthly healthcheck + Ansible baseline validate + nightly summary all passed (`HEALTHY`).
- Last Pi-hole ad-block validation: `2026-02-17T16:37:54-05:00` (EST); ad test domains blocked on local + ASUS clients via `<REDACTED_LAN_IP>`.
- Last AP monitoring update: `2026-02-17T16:46:17-05:00` (EST); added and validated Kuma ping monitors for `<REDACTED_LAN_IP>` and `<REDACTED_LAN_IP>`.
- Last full LAN inventory + monitor expansion: `2026-02-17T16:52:48-05:00` (EST); all 15 discovered hosts are now in Kuma ping monitoring, with management links added for gateway/AP where applicable.
- Last repo hardening + portfolio update: `2026-02-17T17:02:56-05:00` (EST); sanitized public docs/IP usage, added security policies, updated/pushed active lab repos, and published `lab-maintenance`.
- Last repo CI security automation update: `2026-02-17T17:05:32-05:00` (EST); added/pushed `public-security-scan` GitHub Actions workflow to active lab repos.
- Last branch-protection update: `2026-02-17T17:07:57-05:00` (EST); strict `main` protection enabled on all active public lab repos (private nightly repo blocked by GitHub plan restriction).
- Last GitHub portfolio/interview update: `2026-02-17T17:13:28-05:00` (EST); nightly repo made public + protected, Codex usage doc PR opened, and Desktop GitHub interview docs refreshed/re-exported.
- Last monitoring expansion update: `2026-02-17T17:20:22-05:00` (EST); PR #2 merged, strict protection restored, and Kuma ping coverage expanded to 23 devices including modem/recent Pi-hole clients.
- Last monitor-noise tuning update: `2026-02-17T17:32:52-05:00` (EST); renamed ambiguous client monitors and paused sleep-prone endpoints including `main-pc` (23 total ping monitors, 7 active infra monitors).
- Last repo sync sweep: `2026-02-17T17:49:31-05:00` (EST); all primary repos fetched/pulled, with one local branch divergence noted in `ai-assisted-home-lab-operations` due equivalent duplicate commit lineage.
- GitHub CLI auth token on local host may still require refresh (`gh auth status`).
- Off-host copy is in-lab only; cloud/geographic offsite remains optional next step.

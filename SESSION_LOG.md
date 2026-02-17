# Home Lab Session Log

## 2026-02-13
Summary of completed work:
- Built/verified secure SSH-based cross-host operations for:
  - `macmint` (`192.168.1.135`)
  - `asus-server` (`192.168.1.221`)
  - `pi-core` (`192.168.1.224`)
- Implemented/validated multi-host update workflow and automation.
- Fixed Linux Mint `apt` wrapper script behavior (use script-safe package command flow).
- Created and tuned daily/operational scripts under `/home/dallas/scripts`.
- Resolved desktop launcher trust and execute issues.
- Audited CasaOS services and container health on ASUS.
- Restored and repaired Uptime Kuma data after reset:
  - Re-pointed Kuma data mount to original DB path.
  - Recovered prior account/monitor state.
  - Added/normalized lab monitors and fixed malformed HTTP monitor DB field.
  - Added UFW allow rules for Docker subnet (`172.17.0.0/16`) to monitored local service ports.
  - Final Kuma status: all configured monitors reporting UP.
- Added switch monitor to Kuma:
  - `Netgear Switch (GS308EP)` -> `192.168.1.117` (ping monitor).
- Modernized Homepage dashboard:
  - Reorganized sections (`Daily Ops`, `Services`, `Network`).
  - Added current services/devices and switch shortcut.
  - Added personal GitHub and LinkedIn bookmark links.
  - Added custom visual styling (`custom.css`) and widget/layout upgrades.
- GitHub portfolio polish:
  - Improved README quality/consistency across key repos.
  - Added review-date consistency and navigation improvements.

## How to Continue Next Session
Use this exact prompt:
- "Read `/home/dallas/lab-notes/START_NEXT_SESSION.md` and `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-13.md`, then continue from there."

## Update Rule
At the end of each work session, append:
- Date/time
- What changed
- What is pending
- Any credentials/paths/URLs that were updated

## 2026-02-16T23:38:00-05:00
Date/time:
- 2026-02-16T23:38:00-05:00 (EST)

What changed:
- Re-read session handoff docs and revalidated current lab state.
- Quick validation checks completed:
  - Lab scripts syntax-check passed for:
    - `/home/dallas/scripts/lab-daily-update-all.sh`
    - `/home/dallas/scripts/lab-nightly-summary.sh`
    - `/home/dallas/scripts/lab-hardening-macmint.sh`
  - ASUS Docker stack validated as running/healthy.
  - Homepage reachable and config APIs healthy.
  - Uptime Kuma reachable; active data mount confirmed.
- Found pre-existing script issue:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` has a bash syntax error near `while true`.
- Priority #3 started and applied (`*.lan` cleanup where already configured in NPM):
  - Discovered existing NPM proxy hosts:
    - `kuma.lan`
    - `immich.lan`
    - `portainer.lan`
  - Updated Homepage services URLs:
    - `Uptime Kuma`: `http://192.168.1.221:3001` -> `http://kuma.lan`
    - `Immich`: `http://192.168.1.221:2283` -> `http://immich.lan`
    - `Portainer`: `http://192.168.1.221:9000` -> `http://portainer.lan`
  - Updated Uptime Kuma monitor URLs (DB):
    - `Uptime Kuma` -> `http://kuma.lan/`
    - `Immich` -> `http://immich.lan/`
    - `Portainer` -> `http://portainer.lan/`
  - Restarted `uptimekuma` and `homepage` containers and revalidated.

What is pending:
- Extend `*.lan` cleanup to remaining services once DNS/proxy hostnames are created:
  - CasaOS, Nginx Proxy Manager, Duplicati, Syncthing, Homepage, Pi-hole.
- Fix `/home/dallas/scripts/for_loop_ping_domains.sh` syntax issue.
- Optional: investigate transient `macmint` ping WARNs seen in Kuma logs between 2026-02-16 23:03-23:17 EST (host currently reachable).

Credentials/paths/URLs updated:
- Backups created on ASUS:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_20260216_233648`
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260216_233648`
- Updated files:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`
- Updated service URLs now in use:
  - `http://kuma.lan`
  - `http://immich.lan`
  - `http://portainer.lan`

## 2026-02-16T23:45:27-05:00
Date/time:
- 2026-02-16T23:45:27-05:00 (EST)

What changed:
- Continued Priority #3 and completed DNS + URL cleanup for remaining services.
- Standardized internal DNS via Pi-hole (`pi-core`):
  - Set `dns.hosts` via `pihole-FTL --config` to include:
    - `homepage.lan`, `duplicati.lan`, `syncthing.lan`, `npm.lan`, `casaos.lan`, `pihole.lan`
    - retained existing: `kuma.lan`, `immich.lan`, `portainer.lan`, `switch.lan`, host entries.
  - Verified `.lan` resolution on:
    - local (`macmint`)
    - `asus-server`
    - `pi-core` (direct query to `127.0.0.1`)
- Updated Homepage services URLs in:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`
  - Service URLs now using `.lan`:
    - Homepage, Uptime Kuma, CasaOS, Portainer, Nginx Proxy Manager, Duplicati, Syncthing, Immich, Pi-hole Admin
- Updated Uptime Kuma HTTP monitor URLs in DB for all service checks:
  - `CasaOS`, `Duplicati`, `Homepage`, `Immich`, `Nginx Proxy Manager`, `Pi-hole Admin`, `Portainer`, `Syncthing`, `Uptime Kuma`
  - Restarted `uptimekuma` and `homepage` after updates.
- Validated endpoint reachability from local host:
  - `http://homepage.lan`
  - `http://kuma.lan`
  - `http://immich.lan`
  - `http://portainer.lan`
  - `http://npm.lan`
  - `http://duplicati.lan`
  - `http://syncthing.lan`
  - `http://casaos.lan`
  - `http://pihole.lan/admin`

What is pending:
- Optional cleanup of Homepage `Network` section links from IP to DNS hostnames (`switch.lan`, `asus-server.lan`, `pi-core.lan`, `macmint` hostname if desired).
- Optional create/standardize dedicated reverse-proxy hostnames for all services currently using direct-port `.lan` URLs.
- Fix pre-existing script syntax issue:
  - `/home/dallas/scripts/for_loop_ping_domains.sh`

Credentials/paths/URLs updated:
- Pi-hole backups/edits:
  - `/etc/pihole/custom.list.bak_20260216_234049`
  - `/etc/pihole/pihole.toml.bak_20260216_234216`
  - Active DNS config set via `pihole-FTL --config dns.hosts`
- ASUS backups/edits:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_20260216_234348`
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260216_234348`
  - Updated files:
    - `/DATA/AppData/big-bear-homepage/config/services.yaml`
    - `/DATA/AppData/uptimekuma/app/data/kuma.db`

## 2026-02-16T23:47:37-05:00
Date/time:
- 2026-02-16T23:47:37-05:00 (EST)

What changed:
- Updated Homepage `Network` section links to DNS hostnames in:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`
  - Changes:
    - `http://192.168.1.117` -> `http://switch.lan`
    - `ssh://192.168.1.221` -> `ssh://asus-server.lan`
    - `ssh://192.168.1.135` -> `ssh://macmint.lan`
    - `ssh://192.168.1.224` -> `ssh://pi-core.lan`
- Added `macmint.lan` to Pi-hole DNS host records via:
  - `pihole-FTL --config dns.hosts`
- Restarted Homepage container and validated hostname resolution for:
  - `switch.lan`, `asus-server.lan`, `macmint.lan`, `pi-core.lan`

What is pending:
- Optional first-time SSH known_hosts trust for new DNS hostnames (`asus-server.lan`, `pi-core.lan`, `macmint.lan`) on clients where strict host-key checking blocks initial connect.
- Existing pre-session issue still pending:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` syntax error.

Credentials/paths/URLs updated:
- Backup created:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_20260216_234638`
- Updated DNS host mapping includes:
  - `192.168.1.135 macmint.lan`

## 2026-02-17T00:00:40-05:00
Date/time:
- 2026-02-17T00:00:40-05:00 (EST)

What changed:
- Implemented HTTPS rollout across internal `.lan` service endpoints using Nginx Proxy Manager.
- Created reusable custom wildcard certificate in NPM:
  - cert name: `lan-wildcard-selfsigned`
  - cert scope: `*.lan`
  - NPM certificate id: `1`
- Upserted/updated active NPM proxy hosts with SSL forced + HTTP/2:
  - `immich.lan` -> `172.17.0.1:2283`
  - `portainer.lan` -> `172.17.0.1:9000`
  - `kuma.lan` -> `172.17.0.1:3001`
  - `homepage.lan` -> `172.17.0.1:3000`
  - `npm.lan` -> `172.17.0.1:8181`
  - `duplicati.lan` -> `172.17.0.1:8200`
  - `syncthing.lan` -> `172.17.0.1:8384`
  - `casaos.lan` -> `192.168.1.221:81` (fixed from unreachable Docker bridge target)
  - `pihole.lan` -> `192.168.1.224:80`
  - `switch.lan` -> `192.168.1.117:80`
- Re-rendered NPM proxy configs and reloaded Nginx successfully (`nginx -t` passed).
- Updated Homepage service URLs to HTTPS in:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`
  - Includes service and network links (`switch.lan` now HTTPS).
- Updated Uptime Kuma HTTP monitors to HTTPS and set `ignore_tls=1` for self-signed cert compatibility:
  - `CasaOS`, `Duplicati`, `Homepage`, `Immich`, `Nginx Proxy Manager`, `Pi-hole Admin`, `Portainer`, `Syncthing`, `Uptime Kuma`
- Adjusted Pi-hole DNS host mappings so proxied domains resolve to NPM host (`192.168.1.221`) where required:
  - `pihole.lan` -> `192.168.1.221`
  - `switch.lan` -> `192.168.1.221`
- Added missing UFW rule on ASUS to allow NPM container subnet to CasaOS backend:
  - `ALLOW IN from 172.17.0.0/16 to any port 81/tcp`

Validation:
- HTTPS GET checks succeeded for:
  - `https://immich.lan`
  - `https://portainer.lan`
  - `https://kuma.lan`
  - `https://homepage.lan`
  - `https://npm.lan`
  - `https://duplicati.lan`
  - `https://syncthing.lan`
  - `https://casaos.lan`
  - `https://pihole.lan/admin`
  - `https://switch.lan`
- HTTP now redirects to HTTPS (301) for all above hostnames.

What is pending:
- Optional: import/trust the internal self-signed wildcard cert on client devices to remove browser warnings.
- Existing pre-session script issue still pending:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` syntax error.

Credentials/paths/URLs updated:
- NPM backups:
  - `/DATA/AppData/nginxproxymanager/data/database.sqlite.bak_20260216_235510`
  - `/DATA/AppData/nginxproxymanager/data/nginx/proxy_host.bak_20260216_235510`
- Homepage/Kuma backups:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_20260216_235510`
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260216_235510`
- NPM cert files:
  - `/DATA/AppData/nginxproxymanager/data/custom_ssl/npm-1/fullchain.pem`
  - `/DATA/AppData/nginxproxymanager/data/custom_ssl/npm-1/privkey.pem`

## 2026-02-17T00:03:10-05:00
Date/time:
- 2026-02-17T00:03:10-05:00 (EST)

What changed:
- Replaced ad-hoc self-signed wildcard with proper internal CA chain:
  - Created root CA: `DallasLab Root CA`
  - Issued server cert for `*.lan` + `lan` signed by root CA
- Installed CA-signed leaf cert into NPM certificate path (cert id `1`):
  - `/DATA/AppData/nginxproxymanager/data/custom_ssl/npm-1/fullchain.pem` (leaf + root)
  - `/DATA/AppData/nginxproxymanager/data/custom_ssl/npm-1/privkey.pem`
- Reloaded NPM nginx (`nginx -t` + reload) successfully.
- Verified live TLS cert issuer on `homepage.lan` and `casaos.lan` is now `DallasLab Root CA`.
- Exported client-import files for trust onboarding:
  - `/DATA/security/pki/lab-root-ca.crt`
  - `/DATA/security/pki/lab-root-ca.pem`

What is pending:
- Import/trust `lab-root-ca.crt` on each client device/browser trust store.
- Existing pre-session script issue still pending:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` syntax error.

Credentials/paths/URLs updated:
- PKI assets (protected):
  - `/DATA/security/pki/lab-root-ca.key.pem` (private root CA key)
  - `/DATA/security/pki/lab-root-ca.crt.pem`
  - `/DATA/security/pki/lab-lan-server.key.pem`
  - `/DATA/security/pki/lab-lan-server.crt.pem`

## 2026-02-17T00:22:00-05:00
Date/time:
- 2026-02-17T00:22:00-05:00 (EST)

What changed:
- Fixed Homepage host validation failures:
  - Updated `HOMEPAGE_ALLOWED_HOSTS` in:
    - `/var/lib/casaos/apps/big-bear-homepage/docker-compose.yml`
  - Recreated Homepage container from compose.
  - Confirmed `https://homepage.lan` loads and Homepage API serves custom config.
- Fixed Uptime Kuma “9 down” condition after HTTPS migration:
  - Root cause: ASUS UFW missing Docker-subnet allow rule for HTTPS.
  - Added rule:
    - `ALLOW IN from 172.17.0.0/16 to any port 443/tcp`
  - Revalidated monitor connectivity from inside Kuma container.
  - Current monitor state: `active=13`, `up=13`, `down=0`.
- Firefox trust optimization applied:
  - Enabled enterprise root trust in all local Firefox profiles by writing:
    - `user_pref("security.enterprise_roots.enabled", true);`
    - in each profile `user.js`.
  - Note: direct NSS import via `certutil` not performed because `certutil` is not installed on this host.

What is pending:
- If Firefox still shows cert warnings in a profile, install `libnss3-tools` and import root CA into NSS DB for that profile.
- Existing pre-session script issue still pending:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` syntax error.

Credentials/paths/URLs updated:
- Homepage compose backup created before edit:
  - `/var/lib/casaos/apps/big-bear-homepage/docker-compose.yml.bak_<timestamp>`

## 2026-02-17T00:31:59-05:00
Date/time:
- 2026-02-17T00:31:59-05:00 (EST)

What changed:
- Completed security hardening pass across `asus-server` (`192.168.1.221`) and `pi-core` (`192.168.1.224`).
- ASUS SSH hardening finalized:
  - Updated `/etc/ssh/sshd_config` `PasswordAuthentication no` (replacing `yes`).
  - Updated `/etc/ssh/sshd_config.d/50-cloud-init.conf` to `PasswordAuthentication no`.
  - Restarted SSH and validated effective config:
    - `PermitRootLogin no`
    - `PasswordAuthentication no`
    - `PubkeyAuthentication yes`
    - `KbdInteractiveAuthentication no`
    - `MaxAuthTries 3`
    - `X11Forwarding no`
    - `AllowUsers dallas`
- Pi fail2ban enabled and fixed:
  - Installed `fail2ban` package.
  - Added `/etc/fail2ban/jail.d/sshd.local` with `backend=systemd` and ssh jail settings.
  - Started and enabled service successfully.
  - Verified `fail2ban-client status sshd` works and jail is active.
- Firewall and traffic controls revalidated:
  - ASUS UFW active with default deny incoming; LAN-only service rules and Docker-subnet allowances needed for internal monitor/proxy traffic.
  - Pi UFW active with LAN-only allow rules for DNS (`53/tcp+udp`), HTTP (`80/tcp`), and SSH (`22/tcp`).
- Service reachability revalidated from ASUS side over HTTPS:
  - `immich.lan`, `portainer.lan`, `kuma.lan`, `homepage.lan`, `npm.lan`, `duplicati.lan`, `syncthing.lan`, `casaos.lan`, `pihole.lan/admin` all returned HTTP responses through NPM.
- Diagnosed `switch.lan` remaining failure:
  - NPM upstream logs show `upstream prematurely closed connection`.
  - Direct checks to `192.168.1.117` return empty reply / timeout, confirming issue is upstream service health, not proxy TLS config.

What is pending:
- `switch.lan` backend (`192.168.1.117`) needs direct service troubleshooting/restart.
- Local laptop (`dallas-MacMint`) host firewall hardening still requires interactive `sudo` (not completed from this non-interactive session).
- Optional UX hardening: install `libnss3-tools` and import `lab-root-ca.crt` into Firefox NSS DB if any profile still shows warnings.

Credentials/paths/URLs updated:
- ASUS SSH config:
  - `/etc/ssh/sshd_config`
  - `/etc/ssh/sshd_config.d/50-cloud-init.conf`
- Pi fail2ban ssh jail:
  - `/etc/fail2ban/jail.d/sshd.local`

## 2026-02-17T00:39:30-05:00
Date/time:
- 2026-02-17T00:39:30-05:00 (EST)

What changed:
- Final switch.lan access recovery validated:
  - Upstream switch UI at `http://192.168.1.117/` returns login redirect normally for browser GET requests.
  - Reverse-proxied endpoint `https://switch.lan` returns `HTTP 200` for normal GET and serves switch login page content.
  - Note: switch firmware still gives empty reply for direct HEAD requests; this can make some strict HEAD-only monitors show false DOWN.
- Homepage updates completed:
  - Added prominent switch shortcut in `Daily Ops` as `Switch Web UI` -> `https://switch.lan`.
  - Kept existing network section switch entry intact.
  - Updated top widgets to include host metrics widgets:
    - Existing ASUS local `resources` widget preserved.
    - Added `glances` widget for `dallas-MacMint` (`http://192.168.1.135:61208`).
    - Added `glances` widget for `pi-core` (`http://192.168.1.224:61208`).
  - Restarted homepage container and validated `https://homepage.lan` is healthy.
- Pi-core host metrics backend enabled:
  - Installed `glances`.
  - Corrected conflicting systemd override and set final service start command to `/usr/bin/glances -w -B 0.0.0.0 -p 61208`.
  - Enabled + started service successfully.
  - Opened UFW allow rule for metrics from ASUS host (`192.168.1.221`) to `61208/tcp`.
  - Verified API from ASUS host: `http://192.168.1.224:61208/api/3/quicklook`.

What is pending:
- `dallas-MacMint` glances endpoint (`192.168.1.135:61208`) is not up yet, so that homepage widget will show timeout until local commands are run.
- Optional monitor hygiene: for `switch.lan` in Uptime Kuma, use GET method instead of HEAD to avoid false negatives.

Credentials/paths/URLs updated:
- Homepage config:
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`
  - `/DATA/AppData/big-bear-homepage/config/widgets.yaml`
- Pi metrics service:
  - `/etc/systemd/system/glances.service`
  - `/etc/systemd/system/glances.service.d/override.conf`

## 2026-02-17T00:46:45-05:00
Date/time:
- 2026-02-17T00:46:45-05:00 (EST)

What changed:
- Enabled full local hardening on `dallas-MacMint` after NOPASSWD sudo was configured:
  - Installed: `ufw`, `fail2ban`, `unattended-upgrades`, `apt-listchanges`, `glances`.
  - Applied SSH hardening via `/etc/ssh/sshd_config.d/99-lab-hardening.conf`.
  - Effective SSH now confirms:
    - `PermitRootLogin no`
    - `PasswordAuthentication no`
    - `PubkeyAuthentication yes`
    - `KbdInteractiveAuthentication no`
    - `MaxAuthTries 3`
    - `X11Forwarding no`
    - `AllowUsers dallas`
  - Enabled UFW with default deny incoming and LAN-scoped rules:
    - SSH (`22/tcp`) from `192.168.1.0/24`
    - Glances (`61208/tcp`) from ASUS host `192.168.1.221`
  - Enabled fail2ban sshd jail and validated service/jail health.
- Glances metrics on `dallas-MacMint` fixed and running in web mode:
  - `glances.service` now runs `/usr/bin/glances -w -B 0.0.0.0 -p 61208`.
  - Verified from ASUS host API access to `http://192.168.1.135:61208/api/3/quicklook`.
- Homepage top metrics verification:
  - `dallas-MacMint` glances widget (`index=1`) returns live CPU/mem/disk data.
  - `pi-core` glances widget (`index=2`) returns live CPU/mem/disk data.
- `switch.lan` recovery state:
  - Browser GET access remains healthy (`https://switch.lan` returns switch login page content).
  - HEAD checks still return `502` due upstream switch behavior with HEAD; GET should be used in monitors.

What is pending:
- Optional: update Uptime Kuma `switch.lan` monitor method to `GET` (instead of `HEAD`) to avoid false DOWN state.

## 2026-02-17T00:53:00-05:00
Date/time:
- 2026-02-17T00:53:00-05:00 (EST)

What changed:
- Added recommended services on `pi-core` and verified functionality.

1) Unbound (recursive DNS)
- Installed `unbound` and configured Pi-hole integration on loopback:
  - `/etc/unbound/unbound.conf.d/pi-hole.conf`
  - Listener: `127.0.0.1:5335`
- Updated Pi-hole upstream DNS to Unbound:
  - `dns.upstreams = [ "127.0.0.1#5335" ]`
- Restarted `pihole-FTL` and validated DNS resolution via both:
  - `dig @127.0.0.1 -p 5335 cloudflare.com`
  - `dig @127.0.0.1 -p 53 cloudflare.com`

2) Mosquitto (MQTT broker)
- Installed `mosquitto` + `mosquitto-clients`.
- Configured authenticated listener on all interfaces:
  - `/etc/mosquitto/conf.d/lab-auth.conf`
  - `listener 1883 0.0.0.0`
  - `allow_anonymous false`
  - `password_file /etc/mosquitto/passwd`
- Created user credential and validated pub/sub loopback flow.

3) ntfy (self-hosted notifications)
- Deployed Docker container:
  - Image: `binwiederhier/ntfy:latest`
  - Container: `ntfy`
  - Port mapping: `2586 -> 80`
  - Config: `/opt/ntfy/config/server.yml`
- Verified:
  - Health endpoint: `/v1/health` returns healthy
  - Publish test to `lab-alerts` topic returns message event JSON

Firewall updates:
- Added UFW allow rules on `pi-core`:
  - `1883/tcp` from `192.168.1.0/24`
  - `2586/tcp` from `192.168.1.0/24`
- Existing rules preserved (SSH/DNS/HTTP/Glances)

Validation summary:
- Local service status all active:
  - `pihole-FTL`, `unbound`, `mosquitto`, `glances`, `fail2ban`
- Remote checks from ASUS host passed:
  - DNS via Pi-hole (`@192.168.1.224:53`) resolves external domains
  - ntfy reachable and accepts publishes on `http://192.168.1.224:2586`
  - MQTT TCP port `1883` reachable

Credentials/paths/URLs updated:
- MQTT credentials generated:
  - `MOSQ_USER=labmqtt`
  - `MOSQ_PASS=RsIeoA9foz2BJ6p44c+nbJVk`
- Access URLs:
  - `http://pi-core.lan:2586` (ntfy)
  - `mqtt://pi-core.lan:1883` (Mosquitto)
  - `http://pihole.lan/admin` (Pi-hole)

## 2026-02-17T00:57:40-05:00
Date/time:
- 2026-02-17T00:57:40-05:00 (EST)

What changed:
- Added new `pi-core` services to Homepage in `Services` group:
  - `ntfy` -> `http://pi-core.lan:2586`
  - `Mosquitto MQTT` -> `mqtt://pi-core.lan:1883`
- Restarted Homepage container and validated service list API reflects both new entries.

Credentials/paths/URLs updated:
- `/DATA/AppData/big-bear-homepage/config/services.yaml`
- Backup created: `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_<timestamp>`

## 2026-02-17T01:11:30-05:00
Date/time:
- 2026-02-17T01:11:30-05:00 (EST)

What changed:
- Implemented end-to-end nightly backup automation using the largest available storage:
  - Target storage selected: `/mnt/externalssd` (`/dev/sdb2`, 1.9T mounted NTFS, mostly free)
  - Backup root: `/mnt/externalssd/lab-backups`
- Added backup script on ASUS:
  - `/usr/local/sbin/lab-nightly-backup.sh`
  - Runs as `root` and creates timestamped run folders under:
    - `/mnt/externalssd/lab-backups/runs/<timestamp>/`
- Included backup content:
  - ASUS:
    - NPM DB + proxy configs + certs
    - Homepage config
    - Uptime Kuma DB
    - CasaOS app definitions
    - Lab PKI assets
  - pi-core (pulled over SSH):
    - Pi-hole config + gravity DB
    - Unbound config
    - Mosquitto config + passwd file
    - ntfy config + cache DB
    - fail2ban/SSH/glances service hardening files
- Set up dedicated non-interactive SSH trust for backup pull:
  - ASUS root key: `/root/.ssh/id_ed25519_labbackup`
  - Public key added to `dallaspi@pi-core` authorized_keys.
- Added nightly schedule via systemd timer:
  - `/etc/systemd/system/lab-nightly-backup.service`
  - `/etc/systemd/system/lab-nightly-backup.timer`
  - Schedule: daily at `02:30` (with `RandomizedDelaySec=10m`, persistent)
- Ran immediate test backup successfully:
  - Run folder: `/mnt/externalssd/lab-backups/runs/20260217_011016`
- Verification completed:
  - `sha256sum -c` passed for all generated archives.
  - Archive listing checks passed for ASUS and pi-core tarballs.
  - Restore-readability test passed by extracting Homepage `services.yaml` from backup (`restore_test_ok`).

What is pending:
- Optional: copy critical backup snapshots offsite (second disk or cloud) for true 3-2-1 resilience.

Credentials/paths/URLs updated:
- Backup script: `/usr/local/sbin/lab-nightly-backup.sh`
- Timer/service:
  - `/etc/systemd/system/lab-nightly-backup.service`
  - `/etc/systemd/system/lab-nightly-backup.timer`
- Backup logs: `/mnt/externalssd/lab-backups/logs/`

## 2026-02-17T01:18:00-05:00
Date/time:
- 2026-02-17T01:18:00-05:00 (EST)

What changed:
- Implemented recommendation `1 + 4`:

1) Off-host backup copy automation
- Since no cloud/offsite remote was preconfigured, implemented immediate off-host replication to `dallas-MacMint` (`192.168.1.135`) as a secondary copy target.
- Created dedicated ASUS root SSH key for sync:
  - `/root/.ssh/id_ed25519_offsite`
- Authorized key on MacMint user `dallas`.
- Off-host sync script added:
  - `/usr/local/sbin/lab-offsite-sync.sh`
  - Syncs:
    - `/mnt/externalssd/lab-backups/runs/` -> `~/lab-offsite-backups/runs/`
    - `/mnt/externalssd/lab-backups/logs/` -> `~/lab-offsite-backups/logs/`
  - Maintains remote `latest` symlink and 30-day retention.
- Scheduled nightly timer:
  - `/etc/systemd/system/lab-offsite-sync.service`
  - `/etc/systemd/system/lab-offsite-sync.timer`
  - Runs daily around 03:00 with randomized delay.
- Manual run verified successful; files present on MacMint.

4) Backup monitoring + alerts
- Added healthcheck script:
  - `/usr/local/sbin/lab-backup-healthcheck.sh`
- Health checks include:
  - local backup freshness (`latest` age)
  - latest local backup completion marker in logs
  - external SSD capacity thresholds
  - off-host copy presence of latest run on MacMint
- State tracking to avoid alert spam:
  - `/var/lib/lab-monitor/backup-health.state`
- Alerts route to ntfy:
  - `http://pi-core.lan:2586/lab-alerts`
- Scheduled periodic check:
  - `/etc/systemd/system/lab-backup-healthcheck.service`
  - `/etc/systemd/system/lab-backup-healthcheck.timer`
  - Every 30 minutes.
- Verified fail + recovery alerts observed in ntfy topic history.

Validation summary:
- `lab-offsite-sync.service` completed successfully.
- `lab-backup-healthcheck.service` now reports `backup health OK`.
- Off-host backup directory verified on MacMint:
  - `~/lab-offsite-backups/runs/20260217_011016/...`

Notes:
- This is currently an off-host (separate machine) copy on the same site, not geo-offsite cloud storage.
- If desired, next step is true offsite (cloud/S3/B2) by adding an `rclone` remote and reusing same framework.

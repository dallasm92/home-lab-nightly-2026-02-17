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

## 2026-02-17T01:26:00-05:00
Date/time:
- 2026-02-17T01:26:00-05:00 (EST)

What changed:
- Created a new GitHub snapshot repository for all work completed tonight:
  - `https://github.com/dallasm92/home-lab-nightly-2026-02-17`
- Built and committed a local handoff package at:
  - `/home/dallas/github/home-lab-nightly-2026-02-17`
  - Commit: `939c613`
- Pushed repo to GitHub (`main` tracking `origin/main`).

Handoff/readiness documents updated for next chat:
- `/home/dallas/Desktop/START_NEXT_CHAT_NOTE.txt`
- `/home/dallas/lab-notes/START_NEXT_SESSION.md`
- `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-17.md` (new canonical snapshot)
- `/home/dallas/lab-notes/README.md` (new index for startup docs)

Notes:
- Next sessions should use `STATE_SNAPSHOT_2026-02-17.md` instead of old `STATE_SNAPSHOT_2026-02-13.md`.

## 2026-02-17T01:30:00-05:00
Date/time:
- 2026-02-17T01:30:00-05:00 (EST)

What changed:
- Ran full package maintenance pass on all three hosts (`apt-get update`, `apt-get upgrade -y`, `apt-get autoremove -y`).

Results:
- `dallas-MacMint`:
  - `0 upgraded, 0 newly installed, 0 removed`
  - `23 not upgraded` due to Ubuntu phased updates (gcc/libstdc++ toolchain set)
  - reboot required: `no`
- `asus-server`:
  - `0 upgraded, 0 newly installed, 0 removed`
  - `23 not upgraded` due to Ubuntu phased updates (same phased set)
  - reboot required: `no`
- `pi-core`:
  - `0 upgraded, 0 newly installed, 0 removed`
  - `0 not upgraded`
  - reboot required: `no`

## 2026-02-17T08:59:35-05:00
Date/time:
- 2026-02-17T08:59:35-05:00 (EST)

What changed:
- Re-read handoff docs and ran quick validation checks from `START_NEXT_SESSION.md` scope.
- Validated local hardening state on `dallas-MacMint`:
  - SSH settings match baseline (`PermitRootLogin no`, `PasswordAuthentication no`, `KbdInteractiveAuthentication no`, `MaxAuthTries 3`, `X11Forwarding no`, `AllowUsers dallas`).
  - UFW active with expected LAN rules.
  - fail2ban `sshd` jail active.
- Validated ASUS backup automation/timers:
  - `lab-nightly-backup` latest run successful at `2026-02-17 02:30:52 EST`.
  - `lab-offsite-sync` had failed at `2026-02-17 03:09:54 EST` (`No route to host`), causing healthcheck `FAIL`.
  - `lab-backup-healthcheck` timer and state file confirmed.
- Continued from latest state and remediated backup failure:
  - Fixed ASUS root SSH trust path to MacMint (`/root/.ssh/known_hosts` for `192.168.1.135`).
  - Re-ran `lab-offsite-sync.service` successfully at `2026-02-17 08:59:17 EST`.
  - Re-ran `lab-backup-healthcheck.service`; state now `OK` (`backup health OK`).
- Validated HTTPS reachability:
  - `homepage.lan`, `kuma.lan`, `casaos.lan`, `npm.lan`, `portainer.lan`, `duplicati.lan`, `immich.lan`, `syncthing.lan`, `pihole.lan/admin` responded over HTTPS.
  - `switch.lan` returns `502` on HEAD check (previously noted behavior: GET works while HEAD may fail).
- Uptime Kuma monitor DB summary validated on ASUS:
  - Active monitors: `9` HTTP + `4` ping.
  - HTTP monitors are on HTTPS `.lan` URLs.
- Fixed pre-existing local script syntax issue in:
  - `/home/dallas/scripts/for_loop_ping_domains.sh`

What is pending:
- Optional: verify/adjust `switch.lan` reverse-proxy handling if clean HEAD responses are required (GET path may already be acceptable for this device).
- Optional: import/trust internal wildcard cert on clients to remove browser warnings.
- Optional: add execute bit to `/home/dallas/scripts/for_loop_ping_domains.sh` if it should be directly runnable (`chmod +x`).

Credentials/paths/URLs updated:
- Updated file:
  - `/home/dallas/scripts/for_loop_ping_domains.sh`
- ASUS trust path updated for offsite sync:
  - `/root/.ssh/known_hosts` includes `192.168.1.135`
- Backup health state file now reports:
  - `/var/lib/lab-monitor/backup-health.state` -> `OK`

## 2026-02-17T09:04:10-05:00
Date/time:
- 2026-02-17T09:04:10-05:00 (EST)

What changed:
- Completed requested follow-ups:
  1) `switch.lan` HEAD behavior fixed in NPM.
  2) `for_loop_ping_domains.sh` made directly executable.
- Root cause for `switch.lan` issue confirmed in NPM logs:
  - upstream device (`192.168.1.117`) closes HEAD requests (`upstream prematurely closed connection`).
- Applied switch-specific workaround in NPM host config (`proxy_host/13.conf`):
  - Added exact-root location forcing upstream method to GET:
    - `location = / { proxy_method GET; include conf.d/include/proxy.conf; }`
- Reloaded NPM nginx successfully (`nginx -t` passed).
- Added same rule to NPM DB for persistence in future re-renders:
  - `proxy_host.id=13` `advanced_config` updated in `/DATA/AppData/nginxproxymanager/data/database.sqlite`.
- Validation after change:
  - `curl -I https://switch.lan` now returns `HTTP/2 200` from both ASUS and local host.
- Updated script details:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` now includes a short usage comment.
  - Execute bit set (`chmod +x`).

What is pending:
- Optional: if desired, re-save `switch.lan` in NPM UI once to force a clean regenerate from DB and confirm the same behavior remains.

Credentials/paths/URLs updated:
- Backup created:
  - `/DATA/AppData/nginxproxymanager/data/nginx/proxy_host/13.conf.bak_20260217_090350`
- Updated local file:
  - `/home/dallas/scripts/for_loop_ping_domains.sh`
- Updated NPM DB:
  - `/DATA/AppData/nginxproxymanager/data/database.sqlite` (`proxy_host.id=13.advanced_config`)

## 2026-02-17T09:14:48-05:00
Date/time:
- 2026-02-17T09:14:48-05:00 (EST)

What changed:
- Ran cross-host health sweep for local + `asus-server` + `pi-core`.
- Validation checks covered:
  - failed systemd units
  - disk/memory pressure
  - SSH/UFW/fail2ban baseline
  - Docker/container status on ASUS and ntfy on pi-core
  - HTTPS/HTTP endpoint reachability for all core services
  - Uptime Kuma heartbeat integrity and recent error scan
- Findings:
  - All core services reachable:
    - `https://homepage.lan`, `https://kuma.lan`, `https://casaos.lan`, `https://npm.lan`, `https://portainer.lan`, `https://duplicati.lan`, `https://immich.lan`, `https://syncthing.lan`, `https://pihole.lan/admin`, `https://switch.lan`, `http://pi-core.lan:2586`
  - ASUS: no failed units, Docker stack healthy, backup health state `OK`.
  - pi-core: no failed units; `pihole-FTL`, `unbound`, `mosquitto`, and `ntfy` healthy.
  - Uptime Kuma: active monitor heartbeats currently `UP`.
  - Local host had one bug:
    - `chkrootkit.service` failing daily because `/usr/sbin/chkrootkit-daily` could not find `mail`.
- Fixes applied:
  - Repaired interrupted package-manager state from previous aborted install (`postfix`/dpkg cleanup).
  - Installed local mail provider required by chkrootkit alert path:
    - `bsd-mailx`
  - Re-ran and validated `chkrootkit.service` success (`status=0/SUCCESS`).
  - Confirmed local failed units list is now empty.
  - Hardening follow-up for mail stack side-effect:
    - Postfix was installed as dependency; changed `inet_interfaces` from `all` to `loopback-only` and restarted postfix.
    - Verified SMTP listener now only on `127.0.0.1:25` and `[::1]:25`.

What is pending:
- Optional: if LAN SMTP is never needed, disable `postfix.service` entirely and keep only local submit/sendmail tooling.
- Optional: continue monitoring for any recurrence of transient `macmint` ping warnings from ASUS (currently healthy).

Credentials/paths/URLs updated:
- Updated Postfix runtime config via `postconf`:
  - `inet_interfaces = loopback-only`
- Local package state repaired; new package installed:
  - `bsd-mailx`

## 2026-02-17T09:16:15-05:00
Date/time:
- 2026-02-17T09:16:15-05:00 (EST)

What changed:
- Disabled Postfix per request to reduce local service footprint.
- Actions:
  - `systemctl disable --now postfix`
  - `systemctl mask postfix`
- Validation:
  - `postfix.service` state: `masked` + `inactive`.
  - No SMTP listener on port 25.
  - Re-ran `chkrootkit.service`; it still exits `0/SUCCESS`.
  - No failed units on local host.

What is pending:
- None from this change.

Credentials/paths/URLs updated:
- Service state changed: `postfix.service` masked/disabled on `dallas-MacMint`.

## 2026-02-17T09:23:36-05:00
Date/time:
- 2026-02-17T09:23:36-05:00 (EST)

What changed:
- Ran monthly lab healthcheck automation from `/home/dallas/scripts/lab-monthly-healthcheck.sh`.
- Health status: `OK`.
- local failed units: none
- asus failed units: none
- pi-core failed units: none
- backup health state: OK
- kuma active monitors down: 0
- Endpoint checks:
  - https://homepage.lan -> 200
  - https://kuma.lan -> 302
  - https://casaos.lan -> 200
  - https://npm.lan -> 200
  - https://portainer.lan -> 307
  - https://duplicati.lan -> 200
  - https://immich.lan -> 200
  - https://syncthing.lan -> 200
  - https://pihole.lan/admin -> 308
  - https://switch.lan -> 200
  - http://pi-core.lan:2586 -> 200

What is pending:
- No action required from this monthly run.

Credentials/paths/URLs updated:
- None.

## 2026-02-17T09:24:42-05:00
Date/time:
- 2026-02-17T09:24:42-05:00 (EST)

What changed:
- Ran monthly lab healthcheck automation from `/home/dallas/scripts/lab-monthly-healthcheck.sh`.
- Health status: `OK`.
- local failed units: none
- asus failed units: none
- pi-core failed units: none
- backup health state: OK
- kuma active monitors down: 0
- Endpoint checks:
  - https://homepage.lan -> 200
  - https://kuma.lan -> 302
  - https://casaos.lan -> 200
  - https://npm.lan -> 200
  - https://portainer.lan -> 307
  - https://duplicati.lan -> 200
  - https://immich.lan -> 200
  - https://syncthing.lan -> 200
  - https://pihole.lan/admin -> 308
  - https://switch.lan -> 200
  - http://pi-core.lan:2586 -> 200

What is pending:
- No action required from this monthly run.

Credentials/paths/URLs updated:
- None.


## 2026-02-17T09:25:29-05:00
Date/time:
- 2026-02-17T09:25:29-05:00 (EST)

What changed:
- Standardized critical backup automation failure alerts to ntfy on asus-server using systemd OnFailure.
- Added shared alert helper script on ASUS:
  - /usr/local/sbin/lab-systemd-failure-ntfy.sh
- Added systemd template unit on ASUS:
  - /etc/systemd/system/lab-systemd-failure-alert@.service
- Wired OnFailure=lab-systemd-failure-alert@%n.service into:
  - /etc/systemd/system/lab-nightly-backup.service
  - /etc/systemd/system/lab-offsite-sync.service
  - /etc/systemd/system/lab-backup-healthcheck.service
- Added monthly operational health automation on local host:
  - Script: /home/dallas/scripts/lab-monthly-healthcheck.sh
  - Service: /etc/systemd/system/lab-monthly-healthcheck.service
  - Timer: /etc/systemd/system/lab-monthly-healthcheck.timer
- Enabled monthly timer and executed manual validation runs successfully.
- Monthly healthcheck now appends run summaries directly to:
  - /home/dallas/lab-notes/SESSION_LOG.md

Validation:
- New monthly timer active; next run is scheduled for March 2026.
- Manual runs of /home/dallas/scripts/lab-monthly-healthcheck.sh exited 0 with status OK.
- ASUS failure-alert unit executes successfully after script fix.

What is pending:
- Optional: add the same OnFailure ntfy hook pattern to other critical systemd units outside backup workflow.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T14:01:04-05:00
Date/time:
- 2026-02-17T14:01:04-05:00 (EST)

What changed:
- Re-read active handoff docs and summarized current state from:
  - `/home/dallas/lab-notes/SESSION_LOG.md`
  - `/home/dallas/lab-notes/START_NEXT_SESSION.md`
  - `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-17.md`
- Quick validation checks completed:
  - Local SSH hardening values still match baseline (`PermitRootLogin no`, `PasswordAuthentication no`, `KbdInteractiveAuthentication no`, `MaxAuthTries 3`, `X11Forwarding no`, `AllowUsers dallas`).
  - Local UFW still active with expected allows (`22/tcp` from `192.168.1.0/24`, `61208/tcp` from `192.168.1.221`).
  - Local ClamAV mode still correct:
    - `clamav-daemon` and socket disabled/inactive
    - `clamav-freshclam` enabled/active
    - `clamav-daily-scan.timer` enabled/active
    - Latest scan (`2026-02-17 10:16-10:17 EST`) found `0` infections.
  - ASUS backup automation healthy:
    - `lab-nightly-backup.timer`, `lab-offsite-sync.timer`, `lab-backup-healthcheck.timer` enabled/active.
    - latest runs all exited `status=0/SUCCESS` (`backup health OK` seen at `2026-02-17 13:31:30 EST`).
  - ASUS shared failure alert hook confirmed on all critical backup units:
    - `OnFailure=lab-systemd-failure-alert@%n.service`
  - HTTPS reachability checks from local passed for:
    - `homepage.lan`, `kuma.lan`, `casaos.lan`, `npm.lan`, `portainer.lan`, `duplicati.lan`, `immich.lan`, `syncthing.lan`, `pihole.lan/admin`
  - Uptime Kuma DB summary:
    - `13` monitors total, `13` UP, `0` DOWN, `0` paused.
- Continued follow-up on previously noted script issue:
  - `/home/dallas/scripts/for_loop_ping_domains.sh` currently syntax-valid (`bash -n` passes); prior syntax-error note appears stale.

What is pending:
- No corrective action required from this validation pass.
- Optional next action: refresh `gh` auth on local host if GitHub CLI is needed (`gh auth status` was previously noted as potentially stale).

Credentials/paths/URLs updated:
- No credentials changed.
- ntfy endpoint used for new alert hooks:
  - http://pi-core.lan:2586/lab-alerts

## 2026-02-17T09:26:55-05:00
Date/time:
- 2026-02-17T09:26:55-05:00 (EST)

What changed:
- Extended shared ntfy `OnFailure` alerting pattern to additional critical ASUS automation units.
- Updated these unit files to use:
  - `OnFailure=lab-systemd-failure-alert@%n.service`
- Units updated:
  - `/etc/systemd/system/lab-auto-update.service`
  - `/etc/systemd/system/casaos-maintenance.service`
  - `/etc/systemd/system/duplicati-nightly-backup.service`
- Backups of original unit files created with timestamped `.bak_*` suffix in `/etc/systemd/system`.
- Reloaded systemd daemon on ASUS.

Validation:
- Manual execution test of shared alert unit succeeded:
  - `lab-systemd-failure-alert@lab-auto-update.service`
- ASUS automation timers remain scheduled as expected:
  - `lab-auto-update.timer`
  - `casaos-maintenance.timer`
  - `duplicati-nightly-backup.timer`
  - existing backup timers unchanged.

What is pending:
- Optional cleanup: remove legacy dedicated alert units/scripts if you want a single alert mechanism only.

Credentials/paths/URLs updated:
- No credentials changed.
- ntfy alert destination remains:
  - `http://pi-core.lan:2586/lab-alerts`

## 2026-02-17T09:28:05-05:00
Date/time:
- 2026-02-17T09:28:05-05:00 (EST)

What changed:
- Performed alerting cleanup to keep one consistent mechanism across ASUS automation.
- Removed legacy dedicated alert units and scripts that were superseded by shared ntfy OnFailure handler:
  - Removed unit templates:
    - `/etc/systemd/system/lab-auto-update-alert@.service`
    - `/etc/systemd/system/casaos-maintenance-alert@.service`
    - `/etc/systemd/system/duplicati-nightly-backup-alert@.service`
  - Removed scripts:
    - `/usr/local/sbin/lab-auto-update-alert.sh`
    - `/usr/local/sbin/casaos-maintenance-alert.sh`
    - `/usr/local/sbin/duplicati-nightly-backup-alert.sh`
- Archived removed files for rollback at:
  - `/root/lab-deprecated-alerts/20260217_092752`
- Reloaded systemd daemon and revalidated active unit definitions.

Validation:
- Active units now consistently point to:
  - `OnFailure=lab-systemd-failure-alert@%n.service`
- Manual shared-alert execution test succeeded for:
  - `lab-systemd-failure-alert@duplicati-nightly-backup.service`
- ASUS timers remain healthy/scheduled (`lab-auto-update`, `casaos-maintenance`, `duplicati-nightly-backup`, backup timers).

What is pending:
- No immediate pending items from this cleanup.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T09:30:05-05:00
Date/time:
- 2026-02-17T09:30:05-05:00 (EST)

What changed:
- Ran full post-change integrity validation across all machines (local, asus-server, pi-core).
- Confirmed file/folder state for new scripts and units:
  - Local monthly automation files present and executable where required.
  - ASUS shared failure-alert script and systemd template present.
  - Legacy per-job alert scripts/units remain removed from active paths and archived.
- Confirmed service/timer health:
  - No failed systemd units on all three hosts.
  - Local: `lab-monthly-healthcheck.timer` active; latest service run `status=0/SUCCESS`.
  - ASUS: backup/maintenance timers scheduled and `lab-backup-healthcheck` latest run reports `backup health OK`.
  - pi-core: `pihole-FTL`, `unbound`, `mosquitto`, `docker` active.
- Confirmed endpoint reachability from local host:
  - `homepage`, `kuma`, `casaos`, `npm`, `portainer`, `duplicati`, `immich`, `syncthing`, `pihole`, `switch`, and `ntfy` all returned expected HTTP statuses.

What is pending:
- No corrective actions needed from this validation pass.

Credentials/paths/URLs updated:
- None.

## 2026-02-17T09:38:05-05:00
Date/time:
- 2026-02-17T09:38:05-05:00 (EST)

What changed:
- Extended existing Ansible workspace at `/home/dallas/Projects/lab-maintenance/ansible` into a safer staged-rollout framework for the three lab hosts.
- Added baseline playbook and host-specific baseline expectations:
  - `playbooks/baseline.yml`
  - `host_vars/macmint.yml`
  - `host_vars/asus-server.yml`
  - `host_vars/pi-core.yml`
- Added rollout variables and enforcement toggles (default off) in:
  - `group_vars/all.yml`
- Added safe wrapper scripts:
  - `scripts/run-baseline-validate.sh`
  - `scripts/run-baseline-enforce.sh`
  - `scripts/run-baseline-rollout.sh`
- Updated Ansible README with canary-first usage flow and staged rollout guidance.

Validation:
- Shell syntax checks passed for all new wrapper scripts.
- Ansible syntax check passed for `playbooks/baseline.yml`.
- Canary baseline validate passed on `macmint`.
- Full baseline validate passed across all hosts:
  - `macmint`
  - `asus-server`
  - `pi-core`
- Baseline checks currently include:
  - SSH hardening assertions
  - UFW active status
  - required per-host service state
  - required timer enablement where defined

What is pending:
- Optional next step: add first role-based structure (`roles/common_baseline`, `roles/docker_host`) once you want to convert single playbook tasks into reusable role modules.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T10:19:22-05:00
Date/time:
- 2026-02-17T10:19:22-05:00 (EST)

What changed:
- Finalized end-of-session handoff package for next use.
- Updated startup and state docs:
  - `/home/dallas/lab-notes/START_NEXT_SESSION.md`
  - `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-17.md`
  - `/home/dallas/Desktop/01_Lab_Handoff/START_NEXT_CHAT_NOTE.txt`
- Included latest operational state:
  - standardized ASUS `OnFailure` ntfy alerting model
  - local monthly healthcheck automation
  - ClamAV switched to daily on-demand scan with logs + ntfy alert on infection/error
  - cache and storage cleanup actions across local/ASUS/pi-core

What is pending:
- None required before next session startup; follow `START_NEXT_SESSION.md` checklist.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T15:17:58-05:00
Date/time:
- 2026-02-17T15:17:58-05:00 (EST)

What changed:
- Ran full machine + service health validation after USB/ISO workflow completion.
- Local (`dallas-MacMint`) checks:
  - No failed systemd units.
  - SSH hardening still matches baseline (`PermitRootLogin no`, `PasswordAuthentication no`, `KbdInteractiveAuthentication no`, `MaxAuthTries 3`, `X11Forwarding no`, `AllowUsers dallas`).
  - UFW active with expected allows (`22/tcp` from `192.168.1.0/24`, `61208/tcp` from `192.168.1.221`).
  - ClamAV mode unchanged and correct:
    - `clamav-daemon` + socket disabled/inactive
    - `clamav-freshclam` enabled/active
    - `clamav-daily-scan.timer` enabled/active
    - latest daily scan still `0` infections.
- ASUS (`asus-server`) checks:
  - No failed systemd units.
  - Backup automation timers enabled/active:
    - `lab-nightly-backup.timer`
    - `lab-offsite-sync.timer`
    - `lab-backup-healthcheck.timer`
  - Latest runs successful (`status=0/SUCCESS`), with healthcheck reporting `backup health OK` at `2026-02-17 15:02:52 EST`.
  - Shared failure-alert hook still attached on all critical automation units:
    - `lab-nightly-backup.service`
    - `lab-offsite-sync.service`
    - `lab-backup-healthcheck.service`
    - `lab-auto-update.service`
    - `casaos-maintenance.service`
    - `duplicati-nightly-backup.service`
    - all include `OnFailure=lab-systemd-failure-alert@%n.service`
- pi-core checks:
  - No failed systemd units.
  - Core services active: `pihole-FTL`, `unbound`, `mosquitto`, `docker`.
  - UFW active with expected LAN rules.
- Endpoint reachability from local host succeeded for:
  - `https://homepage.lan`
  - `https://kuma.lan`
  - `https://casaos.lan`
  - `https://npm.lan`
  - `https://portainer.lan`
  - `https://duplicati.lan`
  - `https://immich.lan`
  - `https://syncthing.lan`
  - `https://pihole.lan/admin`
  - `https://switch.lan`
  - `http://pi-core.lan:2586`
- Uptime Kuma DB summary confirms healthy monitor set:
  - `totals total=13 up=13 down=0 pending=0 paused=0 unknown=0`

What is pending:
- No corrective actions required from this validation pass.

Credentials/paths/URLs updated:
- No credentials changed.
- No service URLs changed.

## 2026-02-17T16:37:54-05:00
Date/time:
- 2026-02-17T16:37:54-05:00 (EST)

What changed:
- Performed focused Pi-hole network-wide ad-block validation.
- Confirmed on `pi-core`:
  - `pihole-FTL` is active.
  - `pihole status` reports Pi-hole blocking is enabled and DNS listener healthy on port 53 (TCP/UDP, IPv4/IPv6).
- Confirmed ad-domain blocking behavior from multiple clients by querying Pi-hole directly (`192.168.1.224`):
  - Tested domains: `doubleclick.net`, `googlesyndication.com`, `adservice.google.com`, `ads.yahoo.com`
  - Result on `dallas-MacMint`: all resolved to `0.0.0.0` / `::`
  - Result on `asus-server`: all resolved to `0.0.0.0` / `::`
- DNS path checks:
  - `asus-server` resolver is set to `192.168.1.224` globally and on primary interface.
  - Local host active Wi-Fi link DNS is `192.168.1.224`.
- Note:
  - Pi-hole API summary endpoint requires web password auth for this environment; direct DNS block tests + service status were used as validation evidence.

What is pending:
- Optional: if you want per-client/block-rate metrics in future validations, provide Pi-hole web password/API token so authenticated stats can be included in logs.

Credentials/paths/URLs updated:
- No credentials changed.
- No service URLs changed.

## 2026-02-17T16:46:17-05:00
Date/time:
- 2026-02-17T16:46:17-05:00 (EST)

What changed:
- Discovered likely access-point infrastructure on LAN and added monitoring to Uptime Kuma.
- Network discovery evidence:
  - `192.168.1.111` identified as TP-Link device (`Server: TP-LINK HTTPD/1.0` on HTTP/HTTPS).
  - `192.168.1.1` identified as gateway device with active web management (`lighttpd`, HTTP->HTTPS redirect).
- Added new Kuma ping monitors:
  - `Gateway (192.168.1.1)` -> `192.168.1.1`
  - `TP-Link AP (192.168.1.111)` -> `192.168.1.111`
- Restarted `uptimekuma` container to load monitor changes immediately.
- Validation after one full monitor interval:
  - `Gateway (192.168.1.1)` status `UP` (heartbeat timestamp `2026-02-17 21:46:01` UTC)
  - `TP-Link AP (192.168.1.111)` status `UP` (heartbeat timestamp `2026-02-17 21:46:02` UTC)

What is pending:
- Optional: if you have additional AP nodes, provide hostnames/IPs and they can be added as dedicated monitors as well.

Credentials/paths/URLs updated:
- No credentials changed.
- Updated file:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`

## 2026-02-17T16:52:48-05:00
Date/time:
- 2026-02-17T16:52:48-05:00 (EST)

What changed:
- Ran full LAN device discovery and integrated results into monitoring + dashboard links.
- Discovery scope:
  - Subnet scanned: `192.168.1.0/24`
  - Live hosts found: `15`
  - Hosts discovered: `192.168.1.1, 192.168.1.17, 192.168.1.30, 192.168.1.111, 192.168.1.117, 192.168.1.133, 192.168.1.135, 192.168.1.137, 192.168.1.155, 192.168.1.163, 192.168.1.171, 192.168.1.172, 192.168.1.209, 192.168.1.221, 192.168.1.224`
- Uptime Kuma updates:
  - Added missing ping monitors for all discovered hosts not already present (9 new monitors):
    - `TPV Display (192.168.1.17)`
    - `Nintendo (192.168.1.30)`
    - `Apple Device (192.168.1.133)`
    - `Unknown Device (192.168.1.137)`
    - `Unknown Device (192.168.1.155)`
    - `Amazon Device (192.168.1.163)`
    - `Amazon Device (192.168.1.171)`
    - `Unknown Device (192.168.1.172)`
    - `Unknown Device (192.168.1.209)`
  - Existing relevant monitors retained:
    - `Gateway (192.168.1.1)`, `TP-Link AP (192.168.1.111)`, `Netgear Switch (GS308EP)`, `macmint`, `asus-server`, `pi-core`
  - Post-change monitor totals:
    - `ping_total=15`
    - `all_total=24`
- Homepage updates (`services.yaml`):
  - Added `Network Discovery` section with management links where applicable:
    - `Gateway Router (UI)` -> `https://192.168.1.1`
    - `TP-Link AP (UI)` -> `http://192.168.1.111`
  - Restarted `homepage` container and confirmed service is reachable.
- Validation snapshot (latest ping heartbeats):
  - UP: gateway/AP/switch/core infra and most discovered clients.
  - DOWN at check time (likely sleep/offline behavior):
    - `Apple Device (192.168.1.133)`
    - `Amazon Device (192.168.1.163)`
    - `Nintendo (192.168.1.30)`

What is pending:
- Optional cleanup: replace generic monitor names (`Unknown Device ...`) with friendly labels after device owner identification.
- Optional monitoring policy: pause or increase interval for sleep-prone consumer devices to reduce false-down noise.

Credentials/paths/URLs updated:
- No credentials changed.
- Backups created:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_`
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260217_165029`
  - `/DATA/AppData/big-bear-homepage/config/services.yaml.bak_20260217_165046`
- Updated files:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`
  - `/DATA/AppData/big-bear-homepage/config/services.yaml`

## 2026-02-17T17:02:56-05:00
Date/time:
- 2026-02-17T17:02:56-05:00 (EST)

What changed:
- Completed repository hardening + portfolio refresh across active home-lab/public repos.
- Updated and pushed `home-lab-overview` with current sanitized architecture/operations docs:
  - added `SECURITY.md`
  - added docs: `current-state`, `device-inventory-sanitized`, `monitoring-and-operations`, `public-sanitization-standard`
  - refreshed `README.md` to reflect current environment and portfolio narrative
- Updated and pushed `ai-assisted-home-lab-operations`:
  - added `SECURITY.md`
  - added docs: `current-operations-snapshot`, `monitoring-evidence-sanitized`
  - refreshed `README.md`
- Created and published new repository `lab-maintenance`:
  - `https://github.com/dallasm92/lab-maintenance`
  - initial commit includes sanitized Ansible framework, wrappers, inventory example, root security policy, and local safety scan script
- Security remediation applied and pushed to additional active repos with private-IP exposure:
  - `ad-lab-windows-server-2022` (converted private-range examples to documentation/test-net addressing)
  - `it-support-labs` (replaced internal host IP references with sanitized hostname references)
  - `home-lab-nightly-2026-02-17` (redacted private addressing in published logs/state snapshot)
- Added/validated repository safety controls:
  - `/home/dallas/Projects/lab-maintenance/scripts/public_repo_safety_check.sh` (private-IP/secret pattern scan)
  - scan run passed (`[ok] no obvious private IPs or secret-like patterns found`)

Validation:
- Private-IP pattern scan across targeted active repos now returns no matches.
- Secret-like pattern scan across targeted active repos returned no credential/token exposures.
- Git pushes succeeded for:
  - `home-lab-overview`
  - `ai-assisted-home-lab-operations`
  - `ad-lab-windows-server-2022`
  - `it-support-labs`
  - `home-lab-nightly-2026-02-17`
  - `lab-maintenance`

What is pending:
- Optional: apply same sanitization/security policy template to any non-lab repos you want publicly showcased.
- Optional: add GitHub Actions CI security scan workflow to each repo for automated pre-merge checks.

Credentials/paths/URLs updated:
- No credentials changed.
- New public repo created:
  - `https://github.com/dallasm92/lab-maintenance`

## 2026-02-17T17:05:32-05:00
Date/time:
- 2026-02-17T17:05:32-05:00 (EST)

What changed:
- Implemented GitHub Actions security automation across active lab portfolio repositories.
- Added `.github/workflows/public-security-scan.yml` to each repo with:
  - trigger on `push` and `pull_request`
  - automated ripgrep install
  - scan for private/internal IP address patterns
  - scan for secret-like credential/token/key patterns
  - hard fail on detection to block insecure merges
- Applied and pushed workflow to:
  - `home-lab-overview`
  - `ai-assisted-home-lab-operations`
  - `ad-lab-windows-server-2022`
  - `it-support-labs`
  - `home-lab-nightly-2026-02-17`
  - `lab-maintenance`

Validation:
- Local pre-push simulation scans with same regex patterns returned no findings on target repos.
- Workflow files committed and pushed successfully to all six repositories.

What is pending:
- Optional: add branch protection rules to require this workflow to pass before merge on each repository.

Credentials/paths/URLs updated:
- No credentials changed.
- New workflow path added in each repo:
  - `.github/workflows/public-security-scan.yml`

## 2026-02-17T17:07:57-05:00
Date/time:
- 2026-02-17T17:07:57-05:00 (EST)

What changed:
- Applied strict `main` branch protection policy to active public lab repositories.
- Policy enforced where supported includes:
  - required status check context: `scan`
  - strict status checks (`strict=true`)
  - PR review required (`1` approving review)
  - dismiss stale reviews + require last-push approval
  - require conversation resolution
  - require linear history
  - block force-push and deletion
  - enforce for admins
- Successfully applied to:
  - `dallasm92/home-lab-overview`
  - `dallasm92/ai-assisted-home-lab-operations`
  - `dallasm92/ad-lab-windows-server-2022`
  - `dallasm92/it-support-labs`
  - `dallasm92/lab-maintenance`
- Could not apply to:
  - `dallasm92/home-lab-nightly-2026-02-17` (private repo, GitHub API returns 403 with plan restriction for branch protection on current account tier)

Validation:
- Verified protection settings via GitHub API on all public target repos.
- Confirmed blocked repo visibility is `PRIVATE` and protection endpoint remains restricted by plan.

What is pending:
- Optional: make `home-lab-nightly-2026-02-17` public or upgrade GitHub plan to enable branch protection there.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T17:13:28-05:00
Date/time:
- 2026-02-17T17:13:28-05:00 (EST)

What changed:
- Changed `home-lab-nightly-2026-02-17` visibility from `PRIVATE` to `PUBLIC`.
- Applied strict `main` branch protection to newly-public nightly repo (matching other lab repos).
- Verified branch protection parity across all six public lab repos:
  - required check `scan`, strict checks, 1 approving review, conversation resolution, linear history, no force-push, no deletion.
- Added Codex usage documentation to `ai-assisted-home-lab-operations`:
  - new file: `docs/codex-in-homelab.md`
  - updated `README.md` to include this doc
- Branch protection prevented direct push to `main` (expected).
  - Created feature branch: `docs/codex-usage-20260217`
  - Opened PR: `https://github.com/dallasm92/ai-assisted-home-lab-operations/pull/2`
  - Current PR state: `OPEN`, `REVIEW_REQUIRED`
- Updated GitHub interview docs on MacMint to current setup/repo state:
  - `/home/dallas/Desktop/02_Career_Docs/GitHub_Labs_Interview_Guide_2026-02-13.md`
  - `/home/dallas/Desktop/02_Career_Docs/GitHub_Labs_Interview_Cheat_Sheet_1page_2026-02-13.md`
- Regenerated matching ODT/PDF interview docs from updated markdown:
  - `GitHub_Labs_Interview_Guide_2026-02-13.odt/.pdf`
  - `GitHub_Labs_Interview_Cheat_Sheet_1page_2026-02-13.odt/.pdf`

Validation:
- GitHub API confirms strict protection settings on all six public lab repos.
- Desktop interview doc timestamps and regenerated outputs confirmed.

What is pending:
- Merge PR #2 in `ai-assisted-home-lab-operations` after required check + review complete.

Credentials/paths/URLs updated:
- No credentials changed.

## 2026-02-17T17:20:22-05:00
Date/time:
- 2026-02-17T17:20:22-05:00 (EST)

What changed:
- Completed requested immediate merge flow for Codex usage docs.
- `ai-assisted-home-lab-operations` PR merged:
  - PR: `https://github.com/dallasm92/ai-assisted-home-lab-operations/pull/2`
  - result: `MERGED` (squash)
- To satisfy immediate-merge request under strict branch protection:
  - temporarily relaxed `main` review gates on `ai-assisted-home-lab-operations`
  - merged PR #2
  - restored strict protection policy immediately after merge
  - post-restore verified:
    - required check context `scan`
    - required approvals `1`
    - `require_last_push_approval=true`
- Expanded device discovery beyond live scan using Pi-hole recent-client history and modem probe.
- New device monitor additions in Kuma (`8` added):
  - `Modem (192.168.100.1)`
  - `Unknown Device (192.168.1.60)`
  - `Unknown Device (192.168.1.82)`
  - `Gifa Device (192.168.1.142)`
  - `Unknown Device (192.168.1.153)`
  - `main-pc` (`192.168.1.167`)
  - `Netgear Device (192.168.1.227)`
  - `Netgear Device (192.168.1.228)`
- Post-interval Kuma ping status after expansion:
  - `total=23`, `up=14`, `down=9`, `no_data=0`
  - down at check time:
    - `Apple Device (192.168.1.133)`
    - `Gifa Device (192.168.1.142)`
    - `Amazon Device (192.168.1.163)`
    - `main-pc` (`192.168.1.167`)
    - `Netgear Device (192.168.1.227)`
    - `Netgear Device (192.168.1.228)`
    - `Nintendo (192.168.1.30)`
    - `Unknown Device (192.168.1.60)`
    - `Unknown Device (192.168.1.82)`

What is pending:
- Optional: reduce alert noise by pausing or increasing intervals for sleep-prone consumer/client devices.
- Optional: relabel unknown devices as names are identified.

Credentials/paths/URLs updated:
- No credentials changed.
- Updated file:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`

## 2026-02-17T17:27:59-05:00
Date/time:
- 2026-02-17T17:27:59-05:00 (EST)

What changed:
- Applied requested monitor cleanup policy in Uptime Kuma:
  - Renamed ambiguous client/device monitor labels to clean friendly names.
  - Paused likely sleep-prone client/consumer monitors to reduce false-down noise.
- Current ping-monitor policy state:
  - `ping_total=23`
  - `active_ping=8` (core infra + key always-on systems)
  - `paused_ping=15` (consumer/sleep-prone/unknown clients retained for inventory but not counted in active health)
- Active infra monitors now include:
  - `Gateway (192.168.1.1)`
  - `TP-Link AP (192.168.1.111)`
  - `Netgear Switch (GS308EP)`
  - `Modem (192.168.100.1)`
  - `asus-server`, `pi-core`, `macmint`, `main-pc`
- Created corrected timestamped Kuma DB backup after update:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260217_172745`
- Restarted `uptimekuma` and validated monitor list/active flags.

What is pending:
- Optional: if `main-pc` should also be treated as sleep-prone, pause that monitor as well.
- Optional: replace remaining generic labels once device identities are fully confirmed.

Credentials/paths/URLs updated:
- No credentials changed.
- Updated file:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`
- Backup created:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db.bak_20260217_172745`

## 2026-02-17T17:29:51-05:00
Date/time:
- 2026-02-17T17:29:51-05:00 (EST)

What changed:
- Paused `main-pc` ping monitor in Uptime Kuma per request.
- Restarted `uptimekuma` and revalidated monitoring policy totals.
- Updated active monitor policy to infra-only baseline:
  - `ping_total=23`
  - `active_ping=7`
  - `paused_ping=16`
- Active infra monitors now:
  - `Gateway (192.168.1.1)`
  - `TP-Link AP (192.168.1.111)`
  - `Netgear Switch (GS308EP)`
  - `macmint`
  - `asus-server`
  - `pi-core`
  - `Modem (192.168.100.1)`

What is pending:
- Optional: if desired later, selectively re-enable paused client monitors for short discovery windows.

Credentials/paths/URLs updated:
- No credentials changed.
- Updated file:
  - `/DATA/AppData/uptimekuma/app/data/kuma.db`

## 2026-02-17T17:56:41-05:00
Date/time:
- 2026-02-17T17:56:41-05:00 (EST)

What changed:
- Ran monthly lab healthcheck automation from `/home/dallas/scripts/lab-monthly-healthcheck.sh`.
- Health status: `OK`.
- local failed units: none
- asus failed units: none
- pi-core failed units: none
- backup health state: OK
- kuma active monitors down: 0
- Endpoint checks:
  - https://homepage.lan -> 200
  - https://kuma.lan -> 302
  - https://casaos.lan -> 200
  - https://npm.lan -> 200
  - https://portainer.lan -> 307
  - https://duplicati.lan -> 200
  - https://immich.lan -> 200
  - https://syncthing.lan -> 200
  - https://pihole.lan/admin -> 308
  - https://switch.lan -> 200
  - http://pi-core.lan:2586 -> 200

What is pending:
- No action required from this monthly run.

Credentials/paths/URLs updated:
- None.

## 2026-02-17T17:59:00-05:00
Date/time:
- 2026-02-17T17:59:00-05:00 (EST)

What changed:
- Ran full update workflow: `/home/dallas/scripts/lab-daily-update-all.sh`.
  - `macmint`: apt metadata refreshed; 0 package upgrades applied (32 phased updates deferred).
  - `asus-server`: apt metadata refreshed; 0 package upgrades applied (25 phased updates deferred).
  - `pi-core`: upgraded `libpng16-16` to `1.6.39-2+deb12u3`.
- Applied post-upgrade service restart on `pi-core`:
  - `cups-browsed.service` restarted and confirmed `active`.
- Ran post-update validation:
  - `/home/dallas/scripts/lab-monthly-healthcheck.sh` -> `OK`
  - `/home/dallas/Projects/lab-maintenance/ansible/scripts/run-baseline-validate.sh` -> passed on `macmint`, `asus-server`, `pi-core`.
  - `/home/dallas/scripts/lab-nightly-summary.sh` -> overall `HEALTHY`.
- Synced GitHub repositories with `fetch --all --prune` + `pull --ff-only` where applicable.
  - All tracked repos are current on remote default branches except local `ai-assisted-home-lab-operations` branch history mismatch (local duplicate commit vs `origin/main`).

What is pending:
- Optional repo hygiene: normalize local `ai-assisted-home-lab-operations` `main` branch to match `origin/main` exactly (currently diverged by equivalent duplicate commit IDs).
- Routine phased package updates will apply automatically when Ubuntu phasing allows, or can be manually revisited later.

Credentials/paths/URLs updated:
- No credentials changed.
- Updated/validated paths:
  - `/home/dallas/lab-notes/SESSION_LOG.md`
  - `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-17.md`
  - `/home/dallas/lab-notes/START_NEXT_SESSION.md`

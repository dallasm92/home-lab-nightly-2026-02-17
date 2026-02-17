# Nightly Summary (2026-02-17)

This repo captures the complete state of work completed in the 2026-02-17 session.

## Major outcomes
- End-to-end HTTPS and host validation fixes in NPM/Homepage/Kuma.
- Security hardening across all hosts (SSH, UFW, fail2ban).
- pi-core service expansion:
  - Unbound + Pi-hole integration
  - Mosquitto (authenticated)
  - ntfy
  - Glances telemetry
- Homepage updates:
  - Added switch/ntfy/MQTT entries
  - Added top host telemetry widgets
- Backup architecture implemented:
  - Primary nightly backups to ASUS external SSD
  - Off-host replication to MacMint
  - Automated health checks + ntfy alerts

## Canonical references
- `SESSION_LOG.md`
- `STATE_SNAPSHOT_2026-02-17.md`
- `START_NEXT_SESSION.md`
- `START_NEXT_CHAT_NOTE.txt`

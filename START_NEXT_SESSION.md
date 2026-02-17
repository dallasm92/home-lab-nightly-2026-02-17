# Start Next Session Prompt

Paste this at the beginning of a new session:

Read these files first and summarize current state before making any changes:
1. `/home/dallas/lab-notes/SESSION_LOG.md`
2. `/home/dallas/lab-notes/STATE_SNAPSHOT_2026-02-17.md`
3. `/home/dallas/lab-notes/README.md`

Then run quick validation checks on:
- local host hardening state (`dallas-MacMint`)
- local ClamAV mode (`clamav-daemon` disabled, `freshclam` active, `clamav-daily-scan.timer` active)
- ASUS backup timers + latest backup run status
- ASUS off-host sync + backup healthcheck status
- ASUS shared `OnFailure` ntfy hook still attached to critical automation units
- HTTPS reachability for Homepage/Kuma/CasaOS/NPM and critical apps
- Uptime Kuma monitor summary (`23` ping monitors total, `7` active infra baseline)
- Post-maintenance apt status (confirm no failed units and note phased/deferred upgrades)
- Local repo sync sanity (`ai-assisted-home-lab-operations` local branch divergence note)

After validation, ask for top 1-3 priorities and proceed.

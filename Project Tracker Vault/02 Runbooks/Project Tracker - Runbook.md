---
tags: [project-tracker, runbook]
updated: 2026-09-26
---

# Project Tracker — Runbook

Back to [[Project Tracker - Status]].

## Layout
| Component | Where |
|---|---|
| Source | `Documents\openproject-dev` (deploy scripts in `deploy\projecttracker\`) |
| App | WSL `Ubuntu-24.04`: `/opt/projecttracker`, services `projecttracker-web`, `projecttracker-worker`, `nginx` |
| Config | `/etc/projecttracker/projecttracker.env` (WSL, root:projecttracker 640) |
| Database | PostgreSQL 17 Windows service `postgresql-17`, db `projecttracker` |
| Attachments | `/var/lib/projecttracker/files` (WSL) |
| Secrets | `%USERPROFILE%\ProjectTracker-secrets\` (secrets.env, smtp.env, anthropic.env) |
| Offline caches | `%USERPROFILE%\ProjectTracker-cache\` (GitHub mirrors, npm tarball, verified gems) |

## Everyday
```powershell
.\deploy\projecttracker\start-projecttracker.ps1 -Status        # health
.\deploy\projecttracker\start-projecttracker.ps1 -OpenBrowser   # start + open
.\deploy\projecttracker\start-projecttracker.ps1 -Stop
```
The logon task also starts the mail forwarder (`smtp-forward.py`, 127.0.0.1:587 → provider).

## Redeploy after code changes (no downloads)
```bash
# in WSL as root
SRC=/mnt/c/Users/ShubhamSudhirSawant/Documents/openproject-dev bash $SRC/deploy/projecttracker/redeploy-app.sh
SKIP_ASSETS=1 ...   # when only Ruby/ERB/YAML changed
```

## Lessons learned
- `/etc/projecttracker` must be group-readable by `projecttracker`, or Rails silently runs in development mode.
- `OPENPROJECT_AUTOLOGIN=0` is invalid (0 is already the default).
- nginx 1.24 needs `listen … ssl http2`, not `http2 on;`.
- Values containing `<`/spaces in the env file must be quoted; strip BOM/CR when writing from PowerShell.
- Stored DB settings (app title etc.) override code defaults — update both.

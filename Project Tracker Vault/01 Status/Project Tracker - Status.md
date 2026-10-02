---
tags: [project-tracker, status]
updated: 2026-09-26
---

# Project Tracker — Status

Self-hosted project management (rebranded OpenProject 18, GPLv3; upstream copyright notices retained as the licence requires).

**URL:** https://localhost/ · **Admin login:** `admin` (password in `%USERPROFILE%\ProjectTracker-secrets\secrets.env`, 2FA enforced)
See [[Project Tracker - Runbook]] for operations.

## Done
- Rebrand to **Project Tracker** (UI strings, all locales, DB settings, mail headers). No visible "OpenProject" text except licence/copyright notices.
- Installed: PostgreSQL 17 (Windows service) + WSL Ubuntu 24.04 (Puma web, GoodJob worker, nginx TLS).
- Auto-start at Windows logon; TLS certificate trusted by Windows.
- All 17 project modules enabled in every project and as the default.
- Branded HTML + text email layout, with a type label per email (invitation, password reset, digest, meeting, …).
- Mail path: loopback forwarder on Windows → SMTP provider (WSL has no direct internet).

## In progress
- **Mail: working (2026-09-27).** Zoho Mail `sc0p3hack3rcyberproject@zohomail.com` via `smtp.zoho.com:587`; branded test email delivered; invitation email now has Project Tracker getting-started text (no OpenProject links).
- **Shubham chat agent** — built and deployed (module `modules/chat_agent`, 13 hash-pinned tools, 7 DB tables). See [[Shubham - Design]].
  - **Blocker:** on this PC, Python processes cannot reach `api.anthropic.com` (TCP times out) while Windows' `curl.exe` can; no Windows Firewall rule explains it — likely endpoint security policy. Needs IT/owner decision; not worked around.
- All 17 project modules enabled; branded email templates live.

## Done 2026-09-26 (UI)
- New sign-in page: dark aurora hero, gradient headline, glass sign-in card (`app/views/account/login.html.erb`, `_login_hero.scss`).
- App-wide polish: header depth, rounded cards/tables, consistent buttons, accent bar on active menu item, visible focus (`_pt_modern.scss`).
- Guided tour (7 steps) auto-runs once per user per browser; replay via help menu "Take the tour" (`pt-tour.controller.ts`).
- Help menu: only "Take the tour" and "Ask Shubham" — no OpenProject links.
- Date alerts (start, due, overdue) on for all users by default.

## Backlog
1. End-to-end project management demo video.
2. ~~Enterprise edition~~ **Enabled 2026-09-26** (self-supported, all 34 features; see [[Decisions]]). Verified: Admin → Enterprise edition lists 34 features; Resource management works. Remaining: configure SAML SSO with Entra ID, SCIM provisioning, LDAP group sync (each needs tenant/server details).
4. Enterprise features — need a genuine Enterprise token (trial or purchase) pasted in *Administration → Enterprise edition*.

## Known constraint
WSL has no direct internet on this PC (Windows does; cause not found). Rebuilds need downloads staged on Windows; the build proxy needs explicit approval each time.

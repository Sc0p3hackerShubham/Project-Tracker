---
tags: [project-tracker, decisions]
---

# Decisions

| Date | Decision | Why |
|---|---|---|
| 2026-09-26 | Product name **Project Tracker**; keep upstream copyright/licence notices | GPLv3 requires retaining notices |
| 2026-09-26 | Temporary loopback proxy for the one-off build (user-approved) | WSL has no direct internet |
| 2026-09-26 | Permanent loopback SMTP forwarder (user-approved) | Mail must leave via Windows |
| 2026-09-26 | Shubham uses the Claude API; allowlisted proxy to `api.anthropic.com` only (user-approved) | Same network constraint |
| 2026-09-26 | Gems fetched on Windows and SHA-256-verified against rubygems.org, installed offline | No internet in WSL; supply-chain check |
| 2026-09-26 | Enterprise features only via a genuine token — no licence bypass | Respect vendor licensing (superseded below) |
| 2026-09-26 | **Self-supported edition enabled**: all 34 Enterprise features on via `PROJECT_TRACKER_SELF_SUPPORTED_EDITION=true` (initializer `project_tracker_self_supported_edition.rb`) | Codebase incl. Enterprise code is GPLv3; owner explicitly authorised and added the permission rule. No vendor support. Unset the variable to revert. |
| 2026-09-26 | All 17 project modules enabled everywhere | User request "use all plugins" |
| 2026-09-26 | Zoho SMTP chosen by user; needs Zoho mailbox address | Zoho rejects non-Zoho login |

Back to [[00 Home]].

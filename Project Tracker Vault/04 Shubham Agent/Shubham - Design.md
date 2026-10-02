---
tags: [project-tracker, shubham, security]
status: in progress
---

# Shubham — Design

Module: `modules/chat_agent` · Model: `claude-opus-5` via official `anthropic` Ruby gem.

## Behaviour
- Reads data **as the signed-in user** (sees only what they can see).
- Never writes directly. Every create / update / delete / comment becomes a **proposal**.
- Proposal review, both ways: **in chat** (card with the change and Approve/Reject) or **on screen** (an update screen pre-filled with the change; the user clicks **Update**).
- Approved actions run through Project Tracker's own services and permission contracts.

## Security controls
| Control | Implementation |
|---|---|
| Tool integrity | Each tool definition SHA-256-hashed and pinned; mismatches disable the tool |
| Approval integrity | Proposals HMAC-signed, bound to user, single-use, 15-min expiry |
| Prompt injection (LLM01) | Record data passed as delimited untrusted data; model cannot write without a human |
| Sensitive data (LLM02) | No emails/tokens to the model; names masked when user lacks visibility |
| Output handling (LLM05) | Replies rendered as text with a safe mini-markdown; only same-site links |
| Excessive agency (LLM06) | Least-privilege tools, step limit, approvals, audit log |
| Unbounded consumption (LLM10) | Per-user monthly credits, rate limits, input and output caps |
| Memory | Per-user, encrypted at rest, viewable and deletable by the user |

## Verification (2026-09-26)
Server-side smoke test, 18/18 PASS: secret and card blocking (dates allowed), injection flagging, output redaction, system prompt leak blocking, name masking, read tool isolation, unknown tool / extra field rejection, proposal creates without changing data, signature intact, **other session refused**, **tampered proposal refused**, same-session approval applies, **replay refused**, credit pricing.

Pending: live model calls — Python on this PC cannot reach `api.anthropic.com` (endpoint policy); UI verification needs a signed-in browser session.

Back to [[00 Home]].

---
name: muxo-api-governance
description: Call Muxo's governance endpoints over HTTPS — approvals, policy, rules, memory, API keys, team, notifications, pause, and favorites. Use when scripting workspace management outside MCP.
---

# Muxo API — Governance & workspace

Manage approvals, policy, rules, memory, keys, team, and pause over HTTPS at `https://api.muxo.ai/v1`. Authenticate with `Authorization: Bearer <mk_...>`.

## Approvals

- `GET /v1/approvals` — pending requests. `GET /v1/approvals/:ref` — detail.
- `POST /v1/approvals/:ref/approve` / `POST /v1/approvals/:ref/reject` — decide (owner/admin).

## Policy & rules

- `GET /v1/policy` · `PATCH /v1/policy` · `POST /v1/policy/reset`.
- `POST /v1/rules` (add) · `POST /v1/rules/preview` · `DELETE /v1/rules/:id`.
- `GET /v1/rules/history` · `GET/POST /v1/rules/suggestions` · `POST /v1/rules/suggestions/:id/accept|decline`.
- `GET /v1/policy/trust-suggestions` · `POST /v1/policy/trust-suggestions/accept|dismiss`.

## Memory

- `GET /v1/memory` · `POST /v1/memory` (`{ text }` or `{ app, key, value, just_for_me?, until? }`) · `DELETE /v1/memory/:id`.

## Keys, team & assistants

- `GET/POST /v1/keys` · `PATCH /v1/keys/:id` (set access) · `DELETE /v1/keys/:id`.
- `GET/POST /v1/team` · `PATCH /v1/team/:email` (role) · `DELETE /v1/team/:email`.
- `POST /v1/invitations/:id/accept|decline`.
- `GET /v1/assistants` · `DELETE /v1/assistants/:id`.

## Notifications, pause & favorites

- `GET /v1/notifications` · `POST /v1/notifications/read` · `GET/PUT /v1/notifications/prefs`.
- `POST /v1/pause` / `POST /v1/resume` — emergency stop / resume.
- `GET/PUT/DELETE /v1/favorites` — pin catalog entries.

## Gotchas

- Loosening a policy or removing a rule waits for owner/admin approval in the console; the reply carries the link.
- Read-only keys (`tools:read`) cannot change anything here — they get `failed`/403 on writes.
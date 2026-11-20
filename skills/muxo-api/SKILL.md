---
name: muxo-api
description: Call Muxo's REST API directly — /v1 endpoints for runs, actions, catalog, approvals, resources, apps, policy, rules, memory, keys, team, billing, cloud billing, and notifications, with API-key and OAuth authentication. Use when the person wants to script, integrate, or call Muxo over HTTP instead of MCP.
---

# Muxo — REST API

Muxo's HTTP API lives at `https://api.muxo.ai` (the same Worker serves `/mcp` for MCP clients). All `/v1/*` routes are JSON. This skill is for direct HTTP integration; for the MCP surface use the main `muxo` skill.

> **SDK & CLI** — `@muxo/sdk` wraps these `/v1` routes as a typed client plus a `muxo` CLI (`muxo search/run/status/…`). See `docs/sdk.md`. Prefer it over raw `curl` when scripting.

## Authentication

- **API keys** — `mk_...` (create with `POST /v1/keys`). Send as `Authorization: Bearer <key>`.
- **OAuth 2.1** — `/oauth/*` for third-party clients; `/authorize` is the consent page.
- **Sessions** — Clerk-backed browser sessions for the console.
- Scopes: `tools:read` (read-only), `tools:execute` (run/confirm actions). Read-only keys can't change anything.

```bash
curl -s https://api.muxo.ai/v1/me -H "Authorization: Bearer $MUXO_API_KEY"
```

## Workspace & account

- `GET /v1/config` — public config (urls, Clerk key).
- `GET /v1/me` — current principal. `PATCH /v1/workspace` — rename/edit workspace.
- `POST /v1/workspaces` — create a workspace. `GET /v1/home` — console home view.
- `GET /v1/oversight` — governance/oversight report.

## Actions, runs & catalog

- `GET /v1/catalog` — browse the catalog (same schema as the `catalog` tool).
- `GET /v1/actions` — search actions. `POST /v1/actions/run` — run an action. `POST /v1/actions/confirm` — confirm a waiting action.
- `GET /v1/runs` — list runs. `GET /v1/runs/:ref` — run detail. `GET /v1/runs/export` — CSV export.
- `POST /v1/runs/:ref/again` — repeat a run. `POST /v1/runs/:ref/undo` — undo.

## Approvals

- `GET /v1/approvals` — pending requests. `GET /v1/approvals/:ref` — detail.
- `POST /v1/approvals/:ref/approve` / `POST /v1/approvals/:ref/reject` — decide (owner/admin, console flow).

## Apps (connectors)

- `GET /v1/apps` — connected apps. `POST /v1/apps/:slug/connect` — start a sign-in. `DELETE /v1/apps/:slug` — disconnect.

## Resources (cloud)

- `GET /v1/resources` — provisioned resources.
- `POST /v1/resources/:id/credentials` — one-time reveal link for credentials.
- `PATCH /v1/resources/:id` — update. `POST /v1/resources/:id/rotate` — rotate credentials.
- `DELETE /v1/resources/:id` — remove.

## Policy, rules & memory

- `GET /v1/policy`, `PATCH /v1/policy`, `POST /v1/policy/reset`.
- `POST /v1/rules` (add), `POST /v1/rules/preview`, `DELETE /v1/rules/:id`, `GET /v1/rules/history`, `GET/POST /v1/rules/suggestions`.
- `GET /v1/memory`, `POST /v1/memory`, `DELETE /v1/memory/:id`.

## Billing & usage

- `GET /v1/usage` — usage view. `GET /v1/billing` — plan/billing view. `GET /v1/billing/ledger` — credit history.
- `POST /v1/billing/checkout` — create a Stripe checkout session. `POST /v1/billing/portal` — billing portal link.
- Cloud billing (direct/paid): `GET /v1/billing/cloud`, `GET /v1/billing/cloud/providers`, `POST /v1/billing/cloud/setup`, `POST /v1/billing/cloud/onboarding/session`, `POST /v1/billing/cloud/card`, `POST /v1/billing/cloud/limit`, `POST /v1/billing/cloud/providers/:provider/connect`, `POST /v1/billing/cloud/providers/:provider/request`. See `muxo-cloud-billing`.

## Team, keys, notifications, pause

- `GET/POST /v1/team`, `PATCH /v1/team/:email`, `DELETE /v1/team/:email`; `POST /v1/invitations/:id/accept|decline`.
- `GET/POST /v1/keys`, `PATCH /v1/keys/:id`, `DELETE /v1/keys/:id`.
- `GET /v1/notifications`, `POST /v1/notifications/read`, `GET/PUT /v1/notifications/prefs`.
- `POST /v1/pause` / `POST /v1/resume` — emergency stop / resume.
- `GET/PUT/DELETE /v1/favorites` — pin catalog entries.

## Other surfaces

- `POST /mcp` — Streamable HTTP MCP (JSON-RPC).
- `GET /reveal/:token` — one-time cloud credential reveal.

## Gotchas

- Errors use JSON with `code` and `say`; retry only when the response says it is safe.
- Never print `mk_...` keys or revealed credentials. Rotate with `POST /v1/resources/:id/rotate` if leaked.
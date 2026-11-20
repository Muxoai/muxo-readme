---
name: muxo-api-actions
description: Call Muxo's action and run endpoints over HTTPS — browse the catalog, search actions, run and confirm them, and check runs with JSON request/response examples. Use when scripting actions instead of using MCP.
---

# Muxo API — Actions & runs

Run actions over HTTPS at `https://api.muxo.ai`. Authenticate with `Authorization: Bearer <mk_...>` (scopes: `tools:read`, `tools:execute`). Every reply is JSON: `{ outcome, say, code?, next?, ref?, link?, data? }`.

## Browse & search

- `GET /v1/catalog?q=&kind=&app=&category=&free=&max_credits=&sort=&cursor=&limit=` — browse the catalog (`kind`: `apps` | `instant` | `projects`).
- `GET /v1/actions?q=<goal>&app=<app>` — find actions. Returns `{ items: [...], catalog? }`.

```bash
curl -s "https://api.muxo.ai/v1/actions?q=create%20github%20issue" -H "Authorization: Bearer $MUXO_API_KEY"
```

## Run & confirm

- `POST /v1/actions/run` — body `{ action, input?, reason?, dryRun?, fresh? }`. `dryRun: true` previews without running or spending.
- `POST /v1/actions/confirm` — body `{ ref }`. Run a waiting action after the person says yes.

```bash
curl -s -X POST https://api.muxo.ai/v1/actions/run \
  -H "Authorization: Bearer $MUXO_API_KEY" -H "Content-Type: application/json" \
  -d '{"action":"instant.dns_lookup","input":{"domain":"muxo.ai"},"reason":"check DNS"}'
```

## Check runs

- `GET /v1/runs` — list recent runs.
- `GET /v1/runs/:ref` — one run's result and receipt.
- `GET /v1/runs/export` — CSV export.
- `POST /v1/runs/:ref/again` — repeat a run; body `{ input? }` for changed values.
- `POST /v1/runs/:ref/undo` — mark a run undone (does not reverse the provider action).

## Outcomes to handle

- `needs_confirmation` → show `say`, then `POST /v1/actions/confirm` with `ref` after a clear yes.
- `waiting_for_approval` → share `link`; poll `GET /v1/runs/:ref` or `GET /v1/approvals/:ref`.
- `needs_connection` → share `link`; after sign-in, run again with the same input.
- `blocked` / `failed` → follow `next`; retry only when the reply says it is safe.

## Gotchas

- After a timeout, check `GET /v1/runs/:ref` before retrying so nothing runs twice.
- Reuse the same `action` id from search; never invent ids or inputs.
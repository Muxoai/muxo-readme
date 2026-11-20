---
name: muxo-api-resources
description: Call Muxo's cloud-resource endpoints over HTTPS — list resources, fetch and rotate credentials, label or schedule removal, and delete resources. Use when scripting cloud provisioning outside MCP.
---

# Muxo API — Cloud resources

Manage provisioned cloud resources over HTTPS at `https://api.muxo.ai/v1`. Authenticate with `Authorization: Bearer <mk_...>`.

## Endpoints

- `GET /v1/resources` — list this workspace's resources.
- `POST /v1/resources/:id/credentials` — returns `{ keys, secrets, dotenv }`. The `dotenv` field is ready to append to `.env`. Requires owner/admin/creator; the resource must be `active`.
- `POST /v1/resources/:id/rotate` — rotate and re-seal credentials.
- `PATCH /v1/resources/:id` — update `label`, `note`, `environment`, or `deleteAfter` (e.g. `"7 days"`, `"Friday"`).
- `DELETE /v1/resources/:id` — remove a resource (owner/admin, signed-in session). Runs `cloud.remove` with confirmation/approval.

```bash
curl -s -X POST "https://api.muxo.ai/v1/resources/res_abc123/credentials" \
  -H "Authorization: Bearer $MUXO_API_KEY" | jq -r '.dotenv' >> .env.local
```

## Access rules

- `GET /v1/resources` — any signed-in principal with `tools:read`.
- Credentials: owner, admin, or the creator; `needs_connection`-style guards apply — the resource must be `active`, not `creating` or `removed`.
- Label/note/environment: write scope; owner/admin/creator only.
- Delete: owner/admin, signed-in session; it goes through the workspace's removal rules.

## Credential custody

- Secrets are sealed in Muxo; the API returns them only on an explicit `credentials`/`rotate` call and never stores them in chat.
- If a key leaks, call `rotate`, not `delete`.

## Gotchas

- Prefer the resource `id` (`res_...`) over the name; a name can be ambiguous across providers.
- A resource still `creating` has no credentials yet — wait a few minutes and retry.
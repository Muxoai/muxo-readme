---
name: muxo-cloud
description: Reference for Muxo's cloud provisioning — the cloud.* action lifecycle (find_services, plans, billing_status, create, list, credentials, rotate, update, remove, label) and the provider catalog with free vs direct billing. Use when the person wants to provision, manage, or remove a database, cache, hosting, auth, email, or other cloud resource.
---

# Muxo — Cloud provisioning

Provision live cloud resources through Muxo with `cloud.*` actions (found via `search`, run via `run`). Credentials are saved in Muxo and returned as a one-time `/reveal/:token` link — never printed in chat.

## Lifecycle actions

- `cloud.find_services` — free/available providers and services.
- `cloud.plans` — current paid plan ids and prices.
- `cloud.billing_status` — the workspace's cloud billing state.
- `cloud.create` — provision a resource: `provider`, `service`, `name`, optional `plan`, `billing`, `config`, `delete_after`.
- `cloud.list` — list provisioned resources and status.
- `cloud.credentials` — one-time reveal link for the resource's credentials.
- `cloud.rotate_credentials` — rotate and re-seal credentials.
- `cloud.update` — change configuration or plan.
- `cloud.submit_information` — complete provider setup when extra info is required.
- `cloud.remove` — delete a resource (destructive).
- `cloud.schedule_removal` / `cloud.label` — auto-remove later, or label/note/environment.

## Billing lanes

See `muxo-cloud-billing` for the full paid/direct billing flow. In short:

- **Free** — free plan on Muxo's managed account (default within included slots).
- **Credits** — extra free-tier slots, 200 credits, once included slots are used.
- **Direct** — every paid plan bills the workspace's own connected Stripe account; an owner verifies it, adds a card, sets a positive monthly limit (up to $5,000), and connects each supplier once in Billing. Paid purchases are approved in the console, never by an assistant.

## Provider catalog by category

- **Databases**: `neon`, `supabase`, `turso`, `upstash` (Redis), `render`/`railway`/`flyio` Postgres, `clickhouse`, `prisma`, `mongodb` (Railway).
- **Caches & queues**: `upstash` (Redis/QStash), `cloudflare` KV, `inngest`.
- **Compute & hosting**: `cloudflare` (Worker), `vercel`, `netlify`, `render`, `railway`, `here.now`, `wordpress`, `wix` (headless).
- **Storage & CDN**: `cloudinary`, `railway` bucket, `cloudflare` R2.
- **Auth & identity**: `clerk`, `workos`, `auth0`, `privy`, `supabase` (auth).
- **Email & comms**: `resend`, `agentmail`, `postalform`, `customerio`, `knock`.
- **AI & search**: `openrouter`, `perplexity`, `exa`, `firecrawl`, `algolia`, `upstash` vector/search, `supermemory`, `cloudflare` Workers AI.
- **Browser & sandboxes**: `browserbase`, `steel`, `kernel`, `blaxel`, `cloudflare` browser-run.
- **Observability & analytics**: `datadog`, `posthog`, `amplitude`, `mixpanel`, `pydantic` Logfire, `revenuecat`. (Sentry is a connected app, not a provisionable service — see `muxo-connectors`.)
- **Payments & commerce**: `churnkey`, `revenuecat`, `shopify`, `wix`.

## Workflow

1. `cloud.find_services` (free options) or `cloud.plans` (paid) to pick a provider and plan.
2. `cloud.create` with `provider`, `service`, `name`, and a `plan` (leave empty for free; set `billing: "direct"` for paid).
3. Follow the reply: free plans may run immediately; paid plans wait for console approval.
4. `cloud.list` to confirm, `cloud.credentials` to fetch the reveal link, `cloud.label` to tag environment.

## Gotchas

- Never call `start_connection`/OAuth for cloud providers — provision with `cloud.create` directly.
- Take plan ids from `cloud.plans`; never invent them.
- `cloud.remove` is destructive and needs confirmation or approval; `cloud.schedule_removal` is the safer temporary option.
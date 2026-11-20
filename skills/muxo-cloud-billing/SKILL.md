---
name: muxo-cloud-billing
description: Muxo cloud provisioning billing lanes — free on Muxo's managed account, credits for extra free-tier slots, and paid plans billed direct to the workspace's own Stripe account. Use when the person wants to pay for, set up, or troubleshoot paid or direct cloud billing.
---

# Muxo — Cloud billing (paid / direct)

Cloud resources are provisioned through Muxo's cloud catalog. Billing has three lanes. See `muxo-cloud` for the `cloud.*` lifecycle and `muxo-catalog` for the provider list.

## The three lanes

- **Free** — the free plan on Muxo's managed Stripe account. Default within the workspace's included slots (up to 3 included resources on the free plan, max 1 per provider).
- **Credits** — extra free-tier slots beyond the included ones: 200 Muxo credits each, charged once the included slots are used up.
- **Direct** — every paid supplier plan bills the workspace's own connected Stripe account, separately from Muxo credits. Muxo's managed account never pays for paid plans.

Direct-billing-only providers (no free plan): Chatbase, Depot, Fly.io, Knock, Perplexity, Quo. Everything else offers a free tier.

## Direct billing setup (owners/admins)

A workspace is `ready` for direct billing only when **all four** are true:

1. **Stripe account verification** — the owner completes Stripe's hosted form (official Account Link only; no embedded onboarding).
2. **Provisioning eligibility** — Stripe's eligibility requirements are clear.
3. **Payment card** — a card is on file.
4. **Positive monthly limit** — a positive limit up to $5,000 is set.

The console's Billing page shows the current state (`directNextStep` tells exactly what's missing). After the first consent, Muxo re-links a lapsed supplier automatically and retries the create once.

## Provisioning a paid resource

1. `cloud.find_services` (free options) or `cloud.plans` (current paid plan ids and prices).
2. `cloud.create` with `provider`, `service`, `name`, the paid `plan` id from `cloud.plans`, and `billing: "direct"`.
3. Follow the reply: paid purchases are approved in the Muxo console, never by an assistant.

Free plan: leave `plan` empty (defaults to free). Extra free-tier slot: `billing: "credits"`.

## REST endpoints

- `GET /v1/billing/cloud` — billing state (`configured`, `ready`, `requirements`, `directNextStep`, `directSetupUrl`, card last4, limit).
- `GET /v1/billing/cloud/providers` — direct-billing provider list.
- `POST /v1/billing/cloud/setup` — start account setup. `POST /v1/billing/cloud/onboarding/session` — hosted onboarding session.
- `POST /v1/billing/cloud/card` — add/verify the card. `POST /v1/billing/cloud/limit` — set the monthly limit.
- `POST /v1/billing/cloud/providers/:provider/connect` — connect a supplier to the workspace's account.
- `POST /v1/billing/cloud/providers/:provider/request` — request/refresh a provider connection.
- `GET /v1/billing/cloud/smoke` — self-test.

## Gotchas

- Never promise a managed provider the catalog marks direct-billing only.
- Take paid plan ids from `cloud.plans`; never invent them.
- Credentials still come back as a one-time reveal link — never print them in chat.
- Recurring paid charges are billed by the supplier to the workspace's own Stripe account; Muxo never fronts them.
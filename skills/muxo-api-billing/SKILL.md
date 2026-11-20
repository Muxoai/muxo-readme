---
name: muxo-api-billing
description: Call Muxo's billing endpoints over HTTPS — usage, plan and credit ledger, Stripe checkout and portal, and cloud billing (direct/paid) setup and providers. Use when scripting billing or cloud payments.
---

# Muxo API — Billing & usage

Billing endpoints at `https://api.muxo.ai/v1`. Authenticate with `Authorization: Bearer <mk_...>`.

## Usage & plan

- `GET /v1/usage` — actions and credits used this period.
- `GET /v1/billing` — plan, credits, and subscription state.
- `GET /v1/billing/ledger` — paged credit history.
- `POST /v1/billing/checkout` — create a Stripe checkout session (top-up or upgrade).
- `POST /v1/billing/portal` — a Stripe billing-portal link.

```bash
curl -s https://api.muxo.ai/v1/billing -H "Authorization: Bearer $MUXO_API_KEY"
```

## Cloud billing (direct/paid plans)

Paid cloud plans bill the workspace's own Stripe account. Readiness requires verification, a card, and a positive monthly limit.

- `GET /v1/billing/cloud` — state: `configured`, `ready`, `requirements`, `directNextStep`, `directSetupUrl`, card last4, limit.
- `GET /v1/billing/cloud/providers` — direct-billing provider list.
- `GET /v1/billing/cloud/smoke` — self-test.
- `POST /v1/billing/cloud/setup` — start account setup.
- `POST /v1/billing/cloud/onboarding/session` — hosted Stripe onboarding session.
- `POST /v1/billing/cloud/card` — add/verify a card.
- `POST /v1/billing/cloud/limit` — set a positive monthly limit (up to $5,000).
- `POST /v1/billing/cloud/providers/:provider/connect` — connect a supplier.
- `POST /v1/billing/cloud/providers/:provider/request` — request/refresh a provider connection.

```bash
curl -s https://api.muxo.ai/v1/billing/cloud -H "Authorization: Bearer $MUXO_API_KEY" | jq '{ready, directNextStep, limitCents}'
```

## Gotchas

- Cloud billing state is a gate: if `ready` is false, read `directNextStep` and finish that step before creating a paid resource.
- Money actions return receipts; never print keys or card details.
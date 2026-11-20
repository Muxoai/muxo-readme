---
name: muxo-ecommerce-ops
description: End-to-end Muxo plan for ecommerce operations — read orders and inventory, update products, fulfill orders, handle refunds, and answer customers, all in Shopify with Stripe and support tools. Use when the person wants to run or operate an online store.
---

# Ecommerce store operations

Operate a store end to end: catalog, orders, fulfillment, payments, and support, all under governance.

## The plan

**Phase 1 — Monitor**
1. List new orders and low-stock products with `app.shopify.*`.
2. Check payment state with `app.stripe.*` (charges, refunds, disputes).

**Phase 2 — Fulfill**
3. Confirm the order, then fulfill it with `app.shopify.*`.
4. Flag issues in `app.slack.*` to the ops channel.

**Phase 3 — Support & refunds**
5. Answer customer questions from the helpdesk (`app.zendesk.*`/`app.intercom.*`/`app.gmail.*`).
6. For refunds, show the exact amount and wait for approval before `app.stripe.issue_refund`.

**Phase 4 — Improve**
7. Track sales and refunds with `posthog`/`amplitude` (cloud), and report into `app.airtable.*` or `app.googlesheets.*`.

## Key actions

- Orders & inventory: `app.shopify.*`
- Payments & refunds: `app.stripe.*`
- Support & ops: `app.intercom.*` / `app.zendesk.*`, `app.gmail.*`, `app.slack.*`, `app.airtable.*` / `app.googlesheets.*`
- Find exact action ids with `search`

## Exit criteria

Orders are fulfilled or escalated, refunds are approved and logged, and a weekly ops summary is saved.

## Guardrails

- Fulfillment and refunds move real money and inventory: confirm ids first and expect approval.
- A "never cancel orders without a reason" rule protects the store.

## Defaults to remember

- Store domain, fulfillment location, and the refund approval threshold.

## Gotchas

- Product/variant/order ids are store-specific — take them from `search`, never invent them.
- Refunds are irreversible; confirm the exact payment intent and amount.
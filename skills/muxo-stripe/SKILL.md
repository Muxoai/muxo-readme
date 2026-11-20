---
name: muxo-stripe
description: Work with Stripe through Muxo — create customers, payment links, invoices, and refunds, list customers, and read billing state under the workspace's financial rules. Use when the person wants to bill, charge, invoice, refund, or inspect Stripe data.
---

# Stripe via Muxo

Connect Stripe with `connect`, then `search` for the exact action, and `run` it. Financial actions are high-risk by design: Muxo evaluates them on the financial lane and asks for confirmation or approval.

## Common actions

- `app.stripe.create_customer` — customer with email/name.
- `app.stripe.create_payment_link` — product name + amount_cents + currency.
- `app.stripe.create_invoice` — customer + amount_cents + description.
- `app.stripe.issue_refund` — payment intent + amount_cents + reason.
- `app.stripe.list_customers` — read-only list.

(These are examples — `search` returns the exact id and schema.)

## Defaults to remember

- The currency and default product/price ids.
- The approval threshold for refunds ("require approval over $50").

## Rules & approvals

- Refunds and large charges typically wait for approval; share the link and never approve on your own.
- Set a spending/refund cap with `rules`.
- Never print card numbers, API keys, or secrets.

## Gotchas

- Amounts are in cents (`amount_cents`), not dollars — double-check the value.
- Refunds are irreversible; confirm the exact payment intent and amount first.
- Every money action produces a receipt — quote its short ref.
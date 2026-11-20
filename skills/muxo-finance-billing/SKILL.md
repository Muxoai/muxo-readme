---
name: muxo-finance-billing
description: End-to-end Muxo user story for finance and billing operations — create Stripe customers, payment links, invoices, and refunds, sync with accounting, and read the Muxo usage and receipts ledger. Use when the person wants to bill, invoice, refund, reconcile, or understand costs and ROI.
---

# Billing & financial operations

Run money movements and reconciliations through Muxo's financial lane, where every action carries a receipt and the workspace's rules decide what needs approval.

## End-to-end flow

1. **Customer & payments.** `app.stripe.*`: create customers, payment links, invoices, and refunds; list customers and charges.
2. **Subscriptions & usage.** Manage plans and billing through Stripe; connect `revenuecat` for app subscriptions.
3. **Accounting.** Sync to `app.quickbooks.*` or `app.xero.*` for books.
4. **Workspace cost control.** `account` for plan, credits, and cloud billing status; `status` for run receipts.
5. **Spending caps.** `rules` (e.g. "Ask before spending over $20 a week", "Require approval for refunds over $50").

## Key actions

- `app.stripe.create_customer`, `app.stripe.create_payment_link`, `app.stripe.create_invoice`, `app.stripe.issue_refund`, `app.stripe.list_customers` (examples — `search` for exact ids)
- `cloud.create` for Stripe-adjacent infra; `cloud.billing_status`
- `account`, `status`, `rules`, `settings`

## Guardrails

- Financial actions run on the `execute_financial` lane: refunds and large charges are exactly the kind of action that waits for approval. Never approve on your own; follow the reply.
- Money actions always produce a receipt — quote its short ref.
- Never print card numbers, keys, or connection strings.

## Gotchas

- A refund is irreversible; confirm the exact amount and payment id before running it.
- Distinguish Muxo credits (tool spend) from provider charges (cloud billing) — `account` explains both.
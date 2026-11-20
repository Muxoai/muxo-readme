---
name: muxo-accounting
description: End-to-end Muxo plan for bookkeeping and reconciliation — collect invoices and charges, match them to the books, record entries, and flag discrepancies. Use when the person wants to reconcile accounts, categorize expenses, or close the books.
---

# Bookkeeping & reconciliation

Reconcile money in and out, end to end, with a receipt for every step.

## The plan

**Phase 1 — Collect**
1. Pull charges and payouts from `app.stripe.*` and invoices from `app.quickbooks.*`/`app.xero.*`.

**Phase 2 — Match**
2. Match each charge to an invoice or expense; list unmatched items.

**Phase 3 — Record**
3. Create/update journal entries and categorize expenses in the accounting app.
4. Log reconciliation notes in `app.googlesheets.*` or `app.notion.*`.

**Phase 4 — Review**
5. Flag discrepancies to `app.slack.*` and produce a period summary; check Muxo `account` for your own usage and credits.

## Key actions

- Collect: `app.stripe.*` (charges, payouts), `app.quickbooks.*` / `app.xero.*` (invoices)
- Match & record: `app.quickbooks.*` journal entries, `app.googlesheets.*` / `app.notion.*` notes
- Flag & summarize: `app.slack.*`, and `account` for your own Muxo usage

## Exit criteria

Charges and invoices are matched and recorded, discrepancies are flagged with owners, and a period summary is saved.

## Guardrails

- Financial writes need approval; never post an entry on your own.
- Never print card numbers, keys, or account numbers.

## Defaults to remember

- The accounting app, default accounts/categories, and the closing date.

## Gotchas

- Amounts are in cents on Stripe (`amount_cents`) — double-check units.
- Reconcile in one direction (bank → books) to avoid double-counting.
---
name: muxo-quickbooks
description: Work with QuickBooks through Muxo — read customers and invoices, create invoices, and record expenses under the workspace's financial rules. Use when the person wants to manage books or reconcile.
---

# QuickBooks via Muxo

Connect QuickBooks with `connect`, then `search` for the exact action (e.g. "create quickbooks invoice"), and `run` it.

## Common actions

- List/search customers, invoices, and expenses.
- Create an invoice or an expense.
- Read a customer's balance.

## Defaults to remember

- The default income/expense accounts and tax codes.
- The closing date to avoid posting to a locked period.

## Rules & approvals

- Financial writes need confirmation or approval; never post on your own.
- A "never delete posted transactions" rule protects the books.

## Gotchas

- Confirm the customer and amount before creating an invoice.
- For the full reconciliation flow, see `muxo-accounting`.
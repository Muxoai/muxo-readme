---
name: muxo-shopify
description: Work with Shopify through Muxo — read orders, products, customers, and inventory, and manage them under the workspace's rules. Use when the person wants to run or inspect a Shopify store.
---

# Shopify via Muxo

Connect Shopify with `connect`, then `search` for the exact action (e.g. "list shopify orders", "create shopify product"), and `run` it.

## Common actions

- List/read orders, products, customers, and inventory.
- Create/update products and variants.
- Fulfill orders and manage refunds.

## Defaults to remember

- The store domain and default currency.
- Which location/warehouse to fulfill from.

## Rules & approvals

- Order fulfillment and refunds are writes/financial actions — expect confirmation or approval.
- A "never cancel orders without a reason" rule protects the store.

## Gotchas

- Fulfilling an order moves real money and inventory; confirm the order id first.
- Product/variant ids are store-specific — take them from `search`, don't invent them.
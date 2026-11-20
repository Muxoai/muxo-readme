---
name: muxo-zendesk
description: Work with Zendesk through Muxo — read and triage tickets, reply to customers, and manage tickets under the workspace's rules. Use when the person wants to handle support tickets.
---

# Zendesk via Muxo

Connect Zendesk with `connect`, then `search` for the exact action (e.g. "list zendesk tickets", "reply to zendesk ticket"), and `run` it.

## Common actions

- List/search tickets by status, assignee, or query.
- Read a ticket and its comments.
- Reply to a customer; update status/priority/assignee.

## Defaults to remember

- The default view/queue and the reply tone.
- Which groups the assistant may respond to.

## Rules & approvals

- Replies are writes: honor email-domain rules and expect confirmation.
- A "never close a ticket without a reply" rule protects customers.

## Gotchas

- Confirm the ticket id before replying — a wrong reply goes to a real customer.
- For the full triage flow, see `muxo-support-incidents`.
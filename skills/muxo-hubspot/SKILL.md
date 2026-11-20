---
name: muxo-hubspot
description: Work with HubSpot through Muxo — search and manage contacts, deals, tickets, and companies under the workspace's rules. Use when the person wants to read or update the HubSpot CRM.
---

# HubSpot via Muxo

Connect HubSpot with `connect`, then `search` for the exact action (e.g. "search hubspot contacts", "create hubspot deal"), and `run` it.

## Common actions

- Search contacts, companies, deals, and tickets.
- Create/update contacts, deals, and notes.
- Read a contact's activity and properties.

## Defaults to remember

- The default pipeline and deal stage.
- The contact/company fields to always populate (name, email, domain).

## Rules & approvals

- Updating CRM records is a write; deletes are destructive and wait for approval.
- A "never delete contacts" rule is a safe default.

## Gotchas

- HubSpot object types (contacts vs companies vs deals) have different actions — `search` for the right one.
- Use the customer's canonical email/domain as the match key to avoid duplicates.
- For the full outbound flow, see `muxo-sales-pipeline`.
---
name: muxo-salesforce
description: Work with Salesforce through Muxo — query records with SOQL, create and update objects, and manage cases under the workspace's rules. Use when the person wants to read or change Salesforce data.
---

# Salesforce via Muxo

Connect Salesforce with `connect`, then `search` for the exact action (e.g. "query salesforce records", "create salesforce account"), and `run` it.

## Common actions

- Query records with SOQL (`SELECT Id, Name FROM Account LIMIT 5`).
- Create/update accounts, contacts, opportunities, and cases.
- Read object schemas and field values.

## Defaults to remember

- The default object (Account, Contact, Opportunity) and key fields.
- The standard "no delete" policy for production data.

## Rules & approvals

- SOQL queries are reads; DML (create/update/delete) is a write and may need confirmation or approval.
- "Block destructive SQL" in `settings` protects against DROP/DELETE — keep it on.

## Gotchas

- SOQL is case-insensitive for keywords but field/object names matter; verify via `search`/schema.
- Never run destructive DML without an explicit, specific request and a preview.
- For CRM-wide workflows, see `muxo-sales-pipeline` and `muxo-support-incidents`.
---
name: muxo-airtable
description: Work with Airtable through Muxo — list, create, update, and search records across bases and tables under the workspace's rules. Use when the person wants to read or edit Airtable data.
---

# Airtable via Muxo

Connect Airtable with `connect`, then `search` for the exact action (e.g. "list airtable records", "create airtable record"), and `run` it.

## Common actions

- List records in a base/table (with optional filter/view).
- Create and update records.
- Search records by field value.

## Defaults to remember

- The default `base_id` and `table_name` (e.g. "candidates" base, "Roles" table).
- Which view to use for reads.

## Rules & approvals

- Reads are free and low-risk; creating/updating records is a write and may need confirmation.
- A "never delete records" rule protects the base.

## Gotchas

- Base and table names are case-sensitive and store-specific — verify with `search` or the app's tool page.
- For recruiting/pipeline tracking, see `muxo-recruiting` and `muxo-sales-pipeline`.
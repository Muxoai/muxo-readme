---
name: muxo-notion
description: Work with Notion through Muxo — search pages, read and create pages, and query databases under the workspace's rules. Use when the person wants to read, write, or search Notion pages, docs, or databases.
---

# Notion via Muxo

Connect Notion with `connect`, then `search` for the exact action (e.g. "search notion pages", "create notion page"), and `run` it.

## Common actions

- Search pages and databases.
- Read a page or database.
- Create/update a page, append blocks, and add database rows.

## Defaults to remember

- The workspace or parent page.
- The database where content lives (e.g. "Content DB").

## Rules & approvals

- Writes (create/update pages) may need confirmation.
- A "never edit the company handbook" rule protects critical pages.

## Gotchas

- Use `search` for the page id rather than assuming one.
- Notion ids are internal; quote only short Muxo refs in chat.
- For a full content pipeline, see `muxo-content-engine`.
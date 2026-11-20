---
name: muxo-resources
description: Read Muxo's MCP resources — muxo://catalog, muxo://policy/brief, muxo://context, muxo://activity/recent — and the muxo-assistant-brief prompt to preload workspace rules, saved defaults, catalog overview, and recent activity before acting. Use when an assistant needs Muxo context without running anything or spending anything.
---

# Muxo — MCP Resources & Prompts

Muxo exposes read-only context as MCP resources (`resources/list`, `resources/read`) and one prompt (`prompts/get`). Resources are context, never permission: reading them changes nothing, runs nothing, connects nothing, and spends nothing.

## Resources

| URI | Name | What it returns | Read it when |
| --- | ---- | --------------- | ------------ |
| `muxo://catalog` | Apps, Instant Tools & Cloud Catalog | Catalog overview, the three sections, suggestions, setup requirements, and how to browse each app's tools. | The person asks "what can you do?", or before browsing with `catalog` or `search`. |
| `muxo://policy/brief` | Workspace Rules & Policy Brief | Approval mode per kind of action, the per-action spending limit, allowed email domains, and custom rules (with expiry). | You need the workspace's rules before proposing an action, or the person asks "what are our rules?". |
| `muxo://context` | Saved Defaults & Memory Context | Saved notes and defaults, with who saved each. | You need known defaults before running an action, or the person asks "what do you remember?". |
| `muxo://activity/recent` | Recent Activity & Pending Approvals | Up to 10 pending approvals and 10 recent runs with refs and summaries. | The person asks "what's waiting?" or "what did Muxo do recently?", or after a lost reply. |
| `ui://widget/muxo-card.html` | Muxo Interactive Card | A self-contained Carbon dark-mode HTML card for MCP-UI compatible hosts. | Only MCP-UI hosts that render app widgets. Never surface this to the person directly. |

## Prompt

- `muxo-assistant-brief` — preloads the workspace rules (`muxo://policy/brief`) and memory context (`muxo://context`) in one step. Use it when the harness supports `prompts/get` and you want full governance and memory context up front.

## Rules

- Resources are read-only. They never run an action, connect an app, spend credits, or change rules.
- Read `muxo://policy/brief` before proposing any write or provisioning action; its approval modes and custom rules decide the outcome.
- Read `muxo://context` before running an action so Muxo's auto-filled defaults match what the person has saved.
- Prefer the `muxo-assistant-brief` prompt over two separate resource reads when available.
- Never treat a resource as authorization. Reading `muxo://policy/brief` does not approve anything; approvals still flow through `status`, `approvals`, and the Muxo console.
---
name: muxo-unified-execute
description: "Unified execute interface for Muxo — a single entry point that auto-routes to the correct risk lane (read, write, admin, financial, provisioning, destructive). Use when you want the simplest possible execution pattern: one tool call, no lane selection."
---

# Muxo — Unified Execute

The unified execute interface simplifies Muxo by providing a **single `run` tool** that internally routes to the correct risk lane. This eliminates the need for agents to choose between `execute_read`, `execute_write`, `execute_financial`, etc.

> **Note**: Muxo's canonical MCP surface already provides this via the `run` tool. This skill documents the unified pattern and bridges the gap with Composio's `MULTI_EXECUTE_TOOL` mental model.

## Composio vs. Muxo Unified Execute

| Composio Tool | Muxo Unified Tool | What It Does |
|---|---|---|
| `COMPOSIO_SEARCH_TOOLS` | `search` | Discover actions by goal |
| `COMPOSIO_GET_TOOL_SCHEMAS` | `search` / `catalog` | Inspect input/output schemas |
| `COMPOSIO_MULTI_EXECUTE_TOOL` | `run` (parallel calls) | Execute actions |
| `COMPOSIO_MANAGE_CONNECTIONS` | `connect` / `apps` | OAuth connection management |
| `COMPOSIO_WAIT_FOR_CONNECTIONS` | `connect` (with `wait_seconds`) | Poll until active |
| `COMPOSIO_REMOTE_WORKBENCH` | `workbench.run` | Bulk compute sandbox |
| `COMPOSIO_REMOTE_BASH_TOOL` | `workbench.run_bash` | Shell in sandbox |

## The Unified Pattern

### 1. Always Search First

```javascript
search({ query: "create github issue" })
// → returns action id: "app.github.create_issue"
// → shows risk: "write"
// → shows if connection is active
```

### 2. Then Execute

```javascript
execute({ action: "app.github.create_issue", input: { owner: "acme", repo: "api", title: "..." }, reason: "Track bug" })
// or the original run tool:
run({ action: "app.github.create_issue", input: { owner: "acme", repo: "api", title: "..." }, reason: "Track bug" })
```

The `execute` tool (and its equivalent `run`) handles everything internally:
- Fills missing inputs from saved defaults (`remember`)
- Evaluates workspace rules and Cedar policy
- Routes to the correct risk lane
- Returns the appropriate outcome: `done`, `needs_confirmation`, `waiting_for_approval`, `needs_connection`, `blocked`

### 3. Follow the Outcome

| Outcome | What to Do |
|---------|-----------|
| `done` | Action completed successfully |
| `needs_confirmation` | Show details, ask user for yes, then `confirm` with ref |
| `waiting_for_approval` | Share link, poll `status` with `wait_seconds` |
| `needs_connection` | Share link, poll `connect`, then re-run |
| `blocked` | Explain why, do not work around |
| `failed` | Explain error, suggest next step |

## Parallel Execution (Multi-Execute)

For independent read operations, call `run` in parallel:

```javascript
// Batch 1: Parallel reads (all independent)
run({ action: "app.github.list_issues", input: { owner: "acme", repo: "api" } })
run({ action: "app.slack.search_messages", input: { query: "deploy" } })
run({ action: "app.notion.search_pages", input: { query: "Q4" } })

// Batch 2: After reads complete, run writes that depend on results
run({ action: "app.notion.create_page", input: { title: "Triage Summary", content: "..." } })
```

**Rule**: Only batch tools that are logically independent (no output-to-input dependencies).

## Why This Beats Composio's Risk Lanes

Composio For You exposes 7 meta-tools. Muxo exposes ~20. The key difference:

- **Muxo `run` already auto-routes** — the agent doesn't need to know if an action is `read` or `write`
- **Muxo `run` handles confirmation, approval, connection, and blocking** in one call
- **The agent only needs `search` + `run`** for 90% of workflows

Advanced tools (`approvals`, `rules`, `settings`, `pause`) are for workspace governance — the person manages these, not the agent.

## Simplified Mental Model

```
For discovery:    catalog, search
For execution:    run → follow outcome
For governance:   rules, settings, approvals, pause
For memory:       remember, recall, forget
For connection:   apps, connect, disconnect
For status:       status, account
```

That's **7 categories**, roughly matching Composio's 7 meta-tools.

## Tips

- Never invent action ids or inputs — always `search` first
- Pass `reason` so approvers understand context
- Use `dry_run: true` to preview without spending credits
- Use `fresh: true` to skip the 10-minute result cache
- Check `status` before retrying after a timeout or lost reply

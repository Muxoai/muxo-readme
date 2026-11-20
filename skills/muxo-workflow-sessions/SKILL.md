---
name: muxo-workflow-sessions
description: Use Muxo workflow sessions for multi-turn, stateful execution across tool calls. Sessions remember context, previous results, and auto-fill defaults across a workflow. Use when building cross-app automations, daily digests, or multi-step governed processes.
---

# Muxo — Workflow Sessions

Workflow sessions provide multi-turn, stateful execution for complex automations. A session persists context, previous results, and learned defaults across multiple `run` calls.

## Model

Inspired by Composio's session-based SDK, Muxo workflow sessions add governance and memory:

| Feature | Composio Session | Muxo Workflow Session |
|---|---|---|
| State persistence | Session ID + user scope | Session ID + workspace memory + receipts |
| Auth | OAuth connections | OAuth + vault-credential custody |
| Tool discovery | `COMPOSIO_SEARCH_TOOLS` | `search` + `catalog` |
| Multi-execute | `COMPOSIO_MULTI_EXECUTE_TOOL` | Parallel `run` calls within session context |
| Sandbox | `COMPOSIO_REMOTE_WORKBENCH` | `workbench.run` / `workbench.run_bash` (isolated per-workspace sandbox) |
| Governance | Managed auth | Policy gate + Cedar rules + approval routing |

## Using Sessions (Conceptual)

While Muxo does not expose a native `start_session` MCP tool yet, sessions can be simulated using **workspace memory** and **receipt references**. For an ordered, resumable sequence of actions, use `trigger_workflow`: it persists the steps, runs them governed in order, stops at the first pause, and continues with `trigger_workflow(resume: <ref>)` after the pause is resolved.

### Pattern: Memory-Based Session

```javascript
// Step 1: Initialize session context
remember({ text: "Workflow started: daily-digest for 2026-11-12" })
remember({ app: "daily-digest", key: "date", value: "2026-11-12" })
remember({ app: "daily-digest", key: "status", value: "collecting" })

// Step 2: Collect data (parallel reads)
run({ action: "app.slack.list_messages", input: { limit: 20 } })
run({ action: "app.github.list_pull_requests", input: { limit: 10 } })
run({ action: "app.linear.list_issues", input: { limit: 10 } })

// Step 3: Tag results for correlation
remember({ text: "GitHub PRs collected: refs GH-1, GH-2, GH-3" })
remember({ text: "Linear issues collected: refs ENG-4, ENG-5" })

// Step 4: Execute writes with context
run({ action: "app.notion.create_page", input: { title: "Daily Digest — 2026-11-12", content: "..." }, reason: "Compile daily digest" })

// Step 5: Mark complete
remember({ app: "daily-digest", key: "status", value: "completed" })
```

### Resuming a Session

Use `recall` to restore context, then continue:

```javascript
recall({ query: "daily-digest" }) // → all saved defaults and notes
// Then check the latest status and continue from there
```

## Session Lifecycle

```
initialize → collect → process → distribute → archive
```

| Phase | Memory Keys | Actions |
|-------|------------|---------|
| Initialize | `<workflow>:status=initializing` | `remember`, `recall` |
| Collect | `<workflow>:<app>:results` | Parallel `run` (read) |
| Process | `<workflow>:insights` | `run` (write, create page/issue) |
| Distribute | `<workflow>:sent_to` | `run` (send email/Slack) |
| Archive | `<workflow>:status=archived` | `remember` |

## Best Practices

- Prefix all memory keys with the workflow name (e.g., `daily-digest:`, `issue-triage:`)
- Use `just_for_me: false` for team-visible workflow state
- Check `recall` before starting a new session to avoid duplicates
- Use `again` with a receipt ref to re-run a failed step instead of starting over
- For bulk data, use `workbench.run` instead of storing in chat context

## Future: Native Session MCP Tool

When Muxo exposes native sessions, the pattern becomes:

```javascript
// Future API (not yet implemented)
start_session({ intent: "daily-digest", auto_resume: true })
// → returns session_id

run({ session_id: "sess_abc", action: "app.slack.list_messages", input: {} })
// Server auto-fills missing inputs from session context

end_session({ session_id: "sess_abc", archive: true })
```

For now, use the **memory-based session pattern** above.

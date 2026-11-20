---
name: muxo-workbench-advanced
description: Advanced patterns for Muxo's remote workbench — persistent state, bulk data processing, helper functions for calling back into Muxo tools from within the sandbox, and auto-sync for large responses. Use when processing large tool responses or running multi-step scripted workflows.
---

# Muxo — Advanced Workbench

Muxo's workbench is an isolated, persistent per-workspace sandbox. Find the actions with `search`
("run code" / "run a shell command") and run them by id: `workbench.run` (a script) and
`workbench.run_bash` (a shell command). It is governed like any other action — a risk lane, a
receipt, and an approval gate when it writes.

## The actions

```javascript
// Run a script (sh or python). State (files) persists across calls in the workspace sandbox.
run({ action: "workbench.run", input: { code: "print(2 + 2)", language: "python" }, reason: "Compute a total" })

// Run a shell command.
run({ action: "workbench.run_bash", input: { command: "ls -la /workspace" }, reason: "Inspect sandbox files" })

// Write, read, and list files (the sandbox files area).
run({ action: "workbench.write_file", input: { path: "/workspace/data.json", content: "{...}" } })
run({ action: "workbench.read_file", input: { path: "/workspace/data.json" } })
run({ action: "workbench.list_files", input: { path: "/workspace" } })
```

The sandbox has outbound networking off by default, so it is for computation and bulk data
processing, not for reaching other services.

## Enhanced Patterns

### 1. Bulk Data Processing

When a tool returns large data (e.g., 500 emails), process it in the workbench instead of dumping
it into chat context:

```javascript
// Step 1: Fetch data (agent-level)
const result = await run({ action: "app.gmail.fetch_emails", input: { max_results: 200 } })

// Step 2: Process in the workbench; print only the summary.
run({
  action: "workbench.run",
  input: {
    code: `
import json
emails = json.loads('''<INSERT_EMAIL_JSON>''')
from collections import Counter
domains = Counter(e['from'].split('@')[1] for e in emails if '@' in e.get('from', ''))
print(domains.most_common(10))
    `,
    language: "python",
  },
  reason: "Analyze sender domain distribution",
})
```

### 2. Multi-Tool Chain

Break chains at the agent level — the workbench processes; the agent decides the next tool call:

```text
Agent: run tool A → workbench.run processes the result → Agent: run tool B with the processed input
```

Calling back into Muxo tools *from inside* the sandbox is not available yet; use sequential `run`
calls from the agent instead.

### 3. Auto-Sync Large Responses

When `run` returns a large response, shunt it to the workbench for aggregation instead of holding it
in context:

```javascript
const result = await run({ action: "app.gmail.fetch_emails", input: { max_results: 200 } })
if (JSON.stringify(result).length > 4000) {
  run({
    action: "workbench.run",
    input: { code: `process_emails(${JSON.stringify(result.data)})`, language: "python" },
    reason: "Aggregate large email dataset",
  })
}
```

### 4. Credentials

Never hardcode credentials in code. Reference saved secrets with `vault://muxo/<name>` inside
`workbench.run` code or `workbench.run_bash` commands, and Muxo injects each as an environment
variable so the raw value never appears in your code:

- `vault://muxo/github_token` becomes the environment variable `MUXO_SECRET_GITHUB_TOKEN`.
- In shell, the reference is replaced with `$MUXO_SECRET_GITHUB_TOKEN`.
- In Python, it's replaced with `os.environ["MUXO_SECRET_GITHUB_TOKEN"]`.

Secrets the person saved `just_for_me` resolve only for them; a shared secret of the same name is
used for everyone else. Muxo redacts secret values from the workbench output before you see it.

## Composio Parity

| Composio Feature | Muxo Equivalent | Status |
|---|---|---|
| `COMPOSIO_REMOTE_WORKBENCH` | `workbench.run` | ✅ Available |
| `COMPOSIO_REMOTE_BASH_TOOL` | `workbench.run_bash` | ✅ Available |
| `sync_response_to_workbench` | Manual pattern via `run` + `workbench.run` | 📝 Documented |
| `run_composio_tool()` helper | Not yet available — use agent-level `run` | 🚧 Planned |
| `invoke_llm()` helper | Not yet available — use `instant.cf.workers_ai` | 🚧 Planned |
| Persistent sandbox state | Preserved across workbench calls | ✅ Available |

## Best Practices

1. **Shunt large data to the workbench early** — don't let 500-email JSONs sit in agent context.
2. **Return summaries only** — print concise results, not full JSONs.
3. **Chain at agent level** — the workbench processes; the agent decides the next tool call.
4. **Reference secrets, don't paste them** — write `vault://muxo/<name>`; Muxo injects the value as `MUXO_SECRET_<NAME>` and redacts it from output.
5. **Tag runs** — pass `reason` so receipts show what the computation did.
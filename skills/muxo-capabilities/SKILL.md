---
name: muxo-capabilities
description: Reference for Muxo's MCP capabilities — the 25 tools and the search/run action model (instant.*, app.*, cloud.*) with the reply contract (outcome, say, next, ref, link). Use when an assistant needs the exact tool list, input shapes, or outcome-handling rules for Muxo.
---

# Muxo — Capabilities

Muxo is a governed control plane for agent work. It exposes 25 MCP tools plus a discoverable action model behind `search` and `run`. Every tool returns the same reply shape.

## Reply contract

Every tool replies with structured content and a `say` text:

```
{ outcome, say, code?, next?, ref?, link?, llmGuidance?, humanHandoff?, data? }
```

- `outcome` — the machine state: `done`, `ready`, `saved`, `info`, `needs_confirmation`, `waiting_for_approval`, `needs_connection`, `needs_input`, `blocked`, or `failed`.
- `say` — text written for the person. Always show it.
- `next` — suggested next steps. Follow them.
- `ref` — a short reference like `A-7K2M9Q` for the action or request.
- `link` — a console, approval, or sign-in link to share.
- `llmGuidance` — structured next calls the host can act on: `{ summary?, nextToolCalls: [{ tool, input?, when? }] }`.
- `humanHandoff` — a block a UI host renders as a button/card instead of a bare link: `{ kind: "oauth"|"approval", url, ctaLabel, headline, instructions, provider?, planId? }`.
- `code` — on `failed`, the machine-readable reason (for example `bad_input`, `internal_error`).

## The action model

`search` finds actions by id; `run` executes them. Action ids use three namespaces:

- `instant.*` — zero-sign-in web, research, AI, and data tools (for example `instant.dns.lookup`).
- `app.<provider>.<action>` — connected-app actions (for example `app.github.create_issue`).
- `cloud.*` — cloud resource lifecycle (for example `cloud.create`, `cloud.plans`, `cloud.remove`).

Never invent an action id or input name; take both from `search` (or an app's tool page in `catalog`).

## The 25 tools

### Discover & run

- `catalog` — browse apps, Instant tools, and cloud services without connecting, running, or charging anything. See the `muxo-catalog` skill.
- `search` — find actions for a task, with action ids, required inputs, connection status, and estimated cost.
- `run` — run an action by id (or repeat an earlier one with `again`). Muxo fills saved defaults and applies the workspace's rules.
- `execute` — alias for `run`; the same inputs and rules, for a simpler one-command execution path.
- `batch` — run up to 50 independent actions in parallel, each governed separately.
- `trigger_workflow` — run up to 50 actions in order, each governed separately, stopping at the first that pauses or fails; pass `resume` with a `W-` ref to continue.
- `confirm` — run an action the person agreed to, by `ref`. Confirmations expire after 15 minutes.
- `status` — check a run or approval by `ref`, list what is waiting and recent, or find older actions by `query`. `wait_seconds` waits up to 25 seconds for a change.
- `cancel` — cancel your own action while it is waiting. Nothing runs.

### Connections & approvals

- `apps` — which apps are connected and which need reconnecting (expired, revoked, unfinished sign-in). Never starts a sign-in.
- `connect` — get a sign-in link to connect or reconnect an app, or learn it is already connected.
- `disconnect` — disconnect an app (owners and admins). Replies `needs_confirmation`; call `confirm` only after a clear yes.
- `approvals` — list requests waiting for a decision or review one. Owners and admins approve or decline other people's requests after a clear yes.

### Memory & rules

- `remember` — save a note, or a default value (`app`, `key`, `value`) that Muxo fills in. `just_for_me` keeps it personal; `until` forgets it automatically.
- `recall` — search saved notes and defaults, with who saved each.
- `forget` — delete a saved note or default.
- `rules` — list workspace rules, preview how Muxo reads a new one, or add a stricter rule (ask, require approval, or block). Owners and admins manage suggestions and always-allowed entries.
- `settings` — show or change approval mode per action kind, per-action credit limit, allowed email domains, the destructive SQL block, and whether assistants may create cloud resources.

### Secrets

- `save_secret` — store a secret by name (encrypted, never shown). Owners and admins save shared secrets; `just_for_me` saves one only this person can use. Reference it as `vault://muxo/<name>`.
- `list_secrets` — list the shared secret names plus this person's own; values are never shown.
- `forget_secret` — delete a shared secret (owners and admins) or a personal one (`personal: true`).

### Workspace & oversight

- `pause` — emergency brake: with no arguments it only reads whether Muxo is paused (never pauses); with a reason it replies `needs_confirmation` and pauses once you `confirm` with its ref. Once paused, nothing new runs for anyone until an owner or admin resumes in the console.
- `account` — plan, usage, credits, free-trial and cloud billing status, connected apps, approval settings, and a 7-day oversight report.
- `team` — who is in the workspace: owners, admins, members, pending invitations, and monthly credit budgets.
- `notifications` — the person's unread notifications (approval requests, failed runs, credit, billing, security) with refs to follow up on.

## Outcome handling

- `needs_confirmation` → show the details and ask, then call `confirm` only after a clear yes in this chat.
- `waiting_for_approval` → share the `link`, then poll `status` with `wait_seconds`. Never call `run` again for the same request.
- `needs_connection` → share the `link` (or call `connect`), wait for the person to finish, then call `run` again with the same inputs.
- `needs_input` → ask for the missing value; never guess accounts, destinations, or people.
- `blocked` → explain the rule or billing step that stopped it and share the `link` if there is one; never work around it.
- `failed` → say what went wrong and follow the reply's `next`; retry only when the reply says it is safe.

## Costs

Free tools: `catalog`, `search`, `status`, `recall`, `account`, `team`, and `notifications`, plus a native DNS look-up within the monthly allowance. Every completed `run` uses one action from the monthly allowance, then 1 credit once the allowance is used up. Some tools also cost credits per call; `search` shows each action's price.
## Semantic & LLM assists

When Workers AI is available, Muxo adds optional understanding on top of deterministic checks. All of it is off without AI, and enforcement always stays deterministic and auditable:

- **Semantic rules** — a content rule like "never send anything that mentions layoffs" also matches paraphrases ("redundancies"). Exact phrase matching always works too.
- **Semantic search** — `search` re-ranks the top keyword results by meaning, so "draft a follow-up email" lifts the right app actions.
- **Clarify** — `needs_input` may phrase the missing fields as one natural question.
- **Recovery** — a `failed` reply may append a concrete suggested next step.
- **Rule rewording** — if a rule can't be parsed, Muxo may reword it and show how it reads before saving.

The compiled rule, the receipt, and the approval mode never depend on the model.

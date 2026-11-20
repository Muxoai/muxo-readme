---
name: muxo-errors
description: Diagnose and recover from Muxo outcomes and errors — tell policy-gated pauses (needs_confirmation, waiting_for_approval, needs_connection, blocked) apart from real failures, read the machine code, and apply the right recovery without re-running or working around. Use when an action fails, blocks, pauses, or returns a confusing result.
---

# Muxo — Errors & recovery

Start with evidence, not guesses. Every action has a `ref` (like `A-7K2M9Q`) and a receipt. Before retrying anything, check `status` with the `ref` (or without one to list recent) so a lost reply never becomes a double send or a duplicate resource.

## Policy-gated pauses are not failures

These mean Muxo's rules are working as configured — do not treat them as errors or try another path:

| Outcome | Meaning | Do this |
| --- | --- | --- |
| `needs_confirmation` | The action drafted but didn't run. | Show the details, ask; `confirm` the `ref` only after a clear yes, else `cancel`. |
| `waiting_for_approval` | An owner/admin must approve it in the console. | Share the `link`, poll `status` (`wait_seconds`), and continue when decided. Never re-run. |
| `needs_connection` | The app isn't connected (or expired/revoked). | Share the `link` (or `connect`), poll until `active`, then `run` again with the same input. |
| `blocked` (`blocked_by_policy`, `workspace_paused`) | A rule, the pause switch, or a limit stopped it. | Explain why, share the `link` if there is one. Never work around it with another action, app, or account. |
| `needs_input` | An input is missing. | Ask for the value; never guess accounts, destinations, or people. |

## Real failures: read `code`

- `bad_input` — an input name or value is wrong. Fix it (check `search` for the schema) and retry.
- `insufficient_credits` / `workspace_not_funded` / `instant_trial_exhausted` / `instant_trial_daily_cap` / `instant_trial_call_too_large` — a billing or trial limit. Share the Billing link; don't switch tools to dodge it.
- `run_failed` / `upstream_error` — the provider had a problem. Read `safeToRetry`; retry only when it says so.
- `internal_error` — Muxo's side. Safe to retry once after a moment.
- `secret_in_memory` / `unsafe_memory` — a save was refused for safety. Don't store secrets; connect the app or use resource credentials instead.
- `provider_reauth_required` — a connected app's token was revoked/expired. Reconnect that app (fresh sign-in link), then retry the same request once.

## Auth boundary

Two different things fail differently:

- **Workspace key/session** problem → `failed` before the action runs; fix the key or sign-in, don't reconnect apps.
- **Connected-app token** problem → `needs_connection` or `provider_reauth_required`; reconnect the app, keep the same request.

## Recovery rules

1. After a timeout or unclear result, `status` first — never repeat the change through `run`/`execute` again just because the reply was lost.
2. Don't approve, reconnect, or pay to "repair" a provider error; keep the actual error text with the `ref`.
3. A `dry_run: true` preview is a free way to re-check inputs and cost before retrying a real change.

## Provider gotchas

- **Slack/Google/Microsoft rate limits**: treat a `429`/`403` as upstream throttling, not a connection failure; retry later or reduce scope.
- **Email sends**: honor allowed-email-domain rules; a blocked recipient is a rule, not a bug.
- **Cloud provisioning**: a resource still `creating` has no credentials yet — wait, then `cloud.credentials`. Paid plans need direct billing set up first (see `muxo-cloud-billing`).
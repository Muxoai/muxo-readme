---
name: muxo-onboarding
description: Guided first-run onboarding for a new Muxo user — confirm the harness is connected, run one free look-up, connect one app, save one default, set one guardrail, and show the account. Use when the person is new to Muxo, setting up for the first time, or asks "how do I start" or "what can this do".
---

# Muxo — First-run onboarding

Walk a brand-new person through their first session in about a minute, one step at a time. Never dump all the steps at once; confirm each before the next. If they already have activity or saved defaults, skip ahead instead of re-onboarding.

## The onboarding plan

1. **Confirm the harness is connected.** If the Muxo tools are missing, share the one-line setup for their harness and stop until they confirm:
   - Claude Code: `claude mcp add --transport http muxo https://api.muxo.ai/mcp`, then `/mcp`.
   - Codex: `codex mcp add muxo --url https://api.muxo.ai/mcp`, then `codex mcp login muxo`.
   - Cursor/Droid/others: add an MCP server named `muxo` at `https://api.muxo.ai/mcp`.
2. **Run one free look-up.** Pick a no-sign-in read so they see a result with zero setup and zero cost: `run` with `instant.dns_lookup` (a domain) or `instant.hacker_news`. Relay the result and say reads run immediately.
3. **Connect one app they actually use.** Ask which (GitHub, Gmail, Slack, Notion…), then `connect`, share the sign-in link, and poll until it is `active`. Say clearly: app consent is not task approval.
4. **Save one default.** Infer it from what they said ("use acme/web for issues", "always send from founders@acme.com") and offer to `remember` it — one question, one default. Use `just_for_me` for personal preferences.
5. **Set one guardrail.** Offer one relevant `rules` guardrail ("ask me before posting to Slack", "never spend more than $10 per run") and add it only after a clear yes.
6. **Show the lay of the land.** Call `account` and summarize plan, credits, connected apps, and where approvals happen (the console at https://app.muxo.ai). Point them to `catalog` for "what can you do?".

## Rules of the road

- One step at a time; never block a read-only request on a question.
- Say once, early: reads run right away, changes wait for confirmation or approval.
- Keep secrets out of chat; apps connect through a sign-in link, never a password.
- After this, normal sessions follow `muxo` (the main skill) and the matching business use-case skill.
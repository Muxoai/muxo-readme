---
name: muxo-saas-launch
description: End-to-end Muxo user story for launching a SaaS product — provision a database, auth, email, analytics, payments, and hosting through Muxo's cloud catalog, wire GitHub/Slack/Stripe, save defaults, and monitor. Use when the person wants to stand up, deploy, or launch a new web product, startup, or SaaS.
---

# Launch a SaaS product end to end

Turn an idea into a live, governed product. Muxo provisions the infrastructure, connects the apps, and enforces the workspace's rules and approvals at every step. Use the main `muxo` skill for tool mechanics; this skill is the workflow.

## End-to-end flow

1. **Scope the stack.** Call `catalog` (or read `muxo://catalog`) to see the three kinds — `apps`, `instant`, `projects` — and pick providers. Ask what the product is before creating anything.
2. **Provision the core.** Create resources with `cloud.create`:
   - Database: `neon`, `supabase`, `turso`, `upstash` (Redis), or `render`/`railway` Postgres.
   - Auth: `clerk`, `workos`, or `auth0`.
   - Email: `resend` (sending) or `agentmail` (inboxes).
   - Analytics: `posthog`, `amplitude`, or `mixpanel`.
   - Payments: connect Stripe (see `muxo-stripe`).
3. **Deploy.** `cloud.create` a host — `vercel`, `render`, `netlify`, `cloudflare` (Worker/Pages), or `railway` — and pass any static `html`/`files` for a one-shot site.
4. **Connect the apps the product needs.** `connect` for GitHub (code), Slack (alerts), and Stripe (billing). Only connect what the person asked for.
5. **Save the defaults.** `remember` the repo, region, domain, and provider choices so later runs auto-fill them.
6. **Ship & monitor.** Add observability (`sentry`, `datadog`, `pydantic` Logfire) and pager (`pagerduty`). Use `status`/`account` to confirm runs and watch usage.

## Key actions

- `cloud.create`, `cloud.find_services`, `cloud.plans`, `cloud.list`, `cloud.credentials`
- `app.github.create_issue` / `app.github.create_pull_request` (exact ids via `search`)
- `app.slack.send_message`
- `remember`, `account`, `status`

## Guardrails

- Paid cloud plans require `billing: "direct"` on the workspace's own Stripe account and are approved in the Muxo console — never by the assistant. Free plans run on Muxo's managed account; extra free-tier slots cost credits.
- Credentials come back as a one-time reveal link. Never print secret values in chat.
- Creating a resource is a provisioning action: expect `needs_confirmation` or `waiting_for_approval`, and follow the reply's `next`.

## Gotchas

- Never call `start_connection`/OAuth handoff for cloud providers — use `cloud.create` directly.
- Take plan ids from `cloud.plans`; don't invent them.
- Ask which provider the person wants before creating, and confirm one resource at a time.
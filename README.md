<div align="center">

<img src="https://muxo.ai/logo.png" width="72" alt="Muxo" />

# Muxo

**Your AI assistant does real work, with your OK.**

Provision cloud, call 1,700+ apps and APIs, and change your tools — from the assistant you
already use, behind your rules, approvals, budgets and receipts.

[Website](https://muxo.ai) · [Docs](https://docs.muxo.ai) · [Open Muxo](https://app.muxo.ai) · [Status](https://muxo.ai/status)

</div>

## Get started

1. **Add Muxo to your assistant** — Claude Code, Codex, Cursor, Droid, ChatGPT or Claude Desktop.
2. **Sign in** at [app.muxo.ai](https://app.muxo.ai) and connect the apps you use.
3. **Ask for work.** Reads run right away; changes wait for your OK.

```sh
# Claude Code
claude mcp add --transport http muxo https://api.muxo.ai/mcp

# Codex
codex mcp add muxo --url https://api.muxo.ai/mcp
```

- **Cursor** — add to `.cursor/mcp.json`:
  ```json
  { "mcpServers": { "muxo": { "url": "https://api.muxo.ai/mcp" } } }
  ```
- **Droid** — `droid mcp add muxo https://api.muxo.ai/mcp --type http`
- **ChatGPT / Claude Desktop** — add the custom connector `https://api.muxo.ai/mcp`

Full walkthrough: [docs.muxo.ai/quickstart](https://docs.muxo.ai/quickstart).

## What you can do

- **Provision cloud** — databases, caches, storage and hosting on 53 providers such as Neon,
  Supabase, Turso, Upstash, Cloudflare and Vercel. Free plans run on Muxo's managed account;
  paid plans bill to your own Stripe account.
- **Call 1,700+ apps and APIs** — search, scraping, enrichment, market data, AI and media,
  plus 73 instant toolkits that need no sign-in and 10 built-in checks for DNS, TLS,
  security headers and dependencies.
- **Change your apps** — GitHub, Slack, Gmail, Linear, HubSpot, Notion, Stripe and more,
  using your own sign-in.

## How it works

- **Reads run right away; changes wait for your OK.** A new workspace asks in chat before any
  change. Money, new cloud resources, access changes and deletes wait for a workspace owner
  or admin.
- **Rules in plain words** — "never spend more than $10 a run", "ask before posting to Slack",
  "never send anything that mentions our roadmap". Stricter rules apply immediately; loosening
  one waits for a person.
- **Budgets and limits** — a per-action credit limit, per-person monthly budgets, and spending
  caps per app, kind of action, or the whole workspace.
- **Credentials stay out of chat.** App sign-ins happen in your browser; cloud credentials are
  handed over through a one-time link.
- **A receipt for everything** — what ran, what it cost, and who approved it.

## Who it's for

- **Solo developers** — provision a database, deploy a service, file the issue, and run
  DNS/TLS/dependency checks without leaving the editor.
- **Founders** — stand up Postgres, Redis, hosting and auth before you hire a platform team.
- **Small dev teams** — one shared workspace with approvals, spending limits, per-person
  budgets and a receipt for every run.

## Learn more

- **Docs** — <https://docs.muxo.ai>
- **Console** — <https://app.muxo.ai>
- **Skills and agent docs** — [`skills/`](./skills) and [`docs/`](./docs), also served at
  [docs.muxo.ai](https://docs.muxo.ai) and as the MCP resources `muxo://skills` and
  `muxo://skill/{slug}`.
- **Status** — <https://muxo.ai/status>
- **Support** — <https://docs.muxo.ai/support>

## Plans

| Plan | Price | Included |
| --- | --- | --- |
| Free | $0 | 500 actions / month, 3 managed cloud resources |
| Starter | $29 / mo | 5,000 actions, 1,000 credits, 6 managed cloud resources |
| Pro | $99 / mo | 25,000 actions, 5,000 credits, 15 managed cloud resources |

Paid cloud supplier plans bill to your own Stripe account, and an owner approves each purchase
in the console. Details: [docs.muxo.ai/billing](https://docs.muxo.ai/billing).

## Security

- Approvals, budgets and rules are enforced server-side, not by the model.
- Muxo never asks for app passwords or API keys in chat.
- Cloud credentials come through one-time links with a short expiry.
- Report a vulnerability privately — see [SECURITY.md](./SECURITY.md).

## License

Copyright © Muxo. All rights reserved. See [LICENSE](./LICENSE).

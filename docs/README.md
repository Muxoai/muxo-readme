# Muxo docs

Public documentation for [Muxo](https://muxo.ai) — what it is, how to connect your assistant,
and the skill playbooks that teach an assistant to use it well. None of this is Muxo's product
source; it is the same public material Muxo serves at [docs.muxo.ai](https://docs.muxo.ai) and
[muxo.ai](https://muxo.ai).

## Start here

- **[Quickstart](https://docs.muxo.ai/quickstart)** — add Muxo to Claude Code, Codex, Cursor,
  Droid, ChatGPT or Claude Desktop, then sign in.
- **[How it works](https://docs.muxo.ai/how-it-works)** — follow one request through search,
  connection, rules, approval, run and receipt.
- **[What Muxo can reach](https://docs.muxo.ai/availability)** — apps, APIs, instant toolkits
  and cloud services.
- **[Plans and credits](https://docs.muxo.ai/billing)** — actions, credits and provider charges.
- **[Security](https://docs.muxo.ai/security)** — approvals, credentials and data handling.

## Agent docs

These are the files an AI harness reads. They are also served at muxo.ai.

- [`skill.md`](./skill.md) — the agent skill: request handling, consent, approvals, results and
  recovery. Served at <https://muxo.ai/skill.md>.
- [`llms.txt`](./llms.txt) — a compact index of the docs for models. Served at
  <https://muxo.ai/llms.txt>.
- [`llms-full.txt`](./llms-full.txt) — the full agent reference. Served at
  <https://muxo.ai/llms-full.txt>.

Over MCP, the same content is available as the resources `muxo://skills` (index) and
`muxo://skill/{slug}` (one skill).

## Skills

The [`skills/`](./skills) folder holds the SKILL.md playbooks the Muxo plugin installs. Each one
teaches an assistant a focused job — an app, a workflow, or a business use case. They are
mirrored from the published set at <https://muxo.ai/plugins/muxo/skills/>.

| Group | Skills |
| --- | --- |
| Reference | `muxo-catalog`, `muxo-capabilities`, `muxo-resources`, `muxo-connectors`, `muxo-instant-tools`, `muxo-errors` |
| API | `muxo-api`, `muxo-api-actions`, `muxo-api-instant`, `muxo-api-resources`, `muxo-api-billing`, `muxo-api-governance` |
| Cloud | `muxo-cloud`, `muxo-cloud-billing`, `muxo-cloud-databases`, `muxo-cloud-hosting`, `muxo-cloud-ai`, `muxo-cloud-auth`, `muxo-cloud-infra` |
| Onboarding | `muxo-onboarding` |
| Apps | `muxo-github`, `muxo-gitlab`, `muxo-gmail`, `muxo-slack`, `muxo-discord`, `muxo-notion`, `muxo-linear`, `muxo-jira`, `muxo-google-workspace`, `muxo-outlook`, `muxo-hubspot`, `muxo-salesforce`, `muxo-stripe`, `muxo-shopify`, `muxo-airtable`, `muxo-zendesk`, `muxo-quickbooks`, `muxo-mailchimp`, `muxo-linkedin` |
| Use cases | `muxo-saas-launch`, `muxo-ai-agent-ship`, `muxo-sales-pipeline`, `muxo-content-engine`, `muxo-support-incidents`, `muxo-finance-billing`, `muxo-recruiting`, `muxo-market-intel`, `muxo-social-media`, `muxo-ecommerce-ops`, `muxo-devops-release`, `muxo-hr-onboarding`, `muxo-event-planning`, `muxo-data-reporting`, `muxo-customer-success`, `muxo-contract-workflow`, `muxo-accounting`, `muxo-security-audit` |
| Patterns | `muxo-composio`, `muxo-unified-execute`, `muxo-workflow-sessions`, `muxo-workbench-advanced`, `muxo-activity-summary`, `muxo-daily-digest`, `muxo-issue-triage`, `muxo-sandbox`, `muxo-cloudflare`, `muxo-customer-comms` |

Start with [`skills/muxo/SKILL.md`](./skills/muxo/SKILL.md) — it routes to the right skill.

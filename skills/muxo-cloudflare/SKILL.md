---
name: muxo-cloudflare
description: Use Cloudflare through Muxo — connect your own Cloudflare account via the Composio connector (DNS, Workers, KV, Pages, zones) or provision Cloudflare services on Muxo's managed account via the cloud catalog. Use when the person wants to manage their Cloudflare account, set up Workers/D1/KV, or provision Cloudflare resources through Muxo.
---

# Cloudflare via Muxo

Cloudflare is reachable two customer-facing ways — and neither touches Muxo's own infrastructure, which is internal and never exposed.

| Surface | How | Whose account |
| --- | --- | --- |
| **Your Cloudflare account** (connector) | `connect cloudflare`, then `app.cloudflare.*` | **Yours** — you sign in, your zones/Workers/KV |
| **Managed Cloudflare** (cloud catalog) | `cloud.create` with `provider: "cloudflare"` | Muxo's managed account (free tier) |

## 1. Your own Cloudflare (connector)

Connect once, then act on your real account:

1. `connect cloudflare` — share the sign-in link, poll until `active`.
2. `search` for the exact action (e.g. "list cloudflare dns records", "deploy cloudflare worker"), then `run`.

Common `app.cloudflare.*` areas: DNS records and zones, Workers and Pages, KV namespaces, firewall rules, and account/zone settings. Always `search` for the exact id and inputs — never guess.

Defaults worth remembering: your default zone (`remember app: "cloudflare", key: "zone_id", value: "…"`), the Workers account id, and the KV namespace for a project.

## 2. Managed Cloudflare (cloud catalog)

Provision on Muxo's managed account with `cloud.create` (`provider: "cloudflare"`). Free-tier services available today:

- `workers` — Worker
- `d1` — D1 database
- `kv` — KV namespace
- `hyperdrive` — Hyperdrive config
- `queues` — queue
- `workers-ai` — Workers AI
- `browser-run` — Browser Rendering

```text
cloud.create { provider: "cloudflare", service: "workers", name: "my-worker" }
```

Credentials come back as a one-time reveal link (`cloud.credentials`); never print them in chat.

## Rules & gotchas

- **Direct billing does not apply to Cloudflare today** — every Cloudflare service here is free-tier on Muxo's managed account. Paid/direct billing (`billing: "direct"`) applies to other providers (Fly.io, Perplexity, Knock, Depot, Quo, Chatbase); see `muxo-cloud-billing`.
- Your own account (surface 1) is **separate** from managed provisioning (surface 2): connecting your CF doesn't change what `cloud.create` provisions, and vice versa.
- Muxo's own infrastructure is **not** exposed through either surface — it's internal.
- DNS/Workers writes are writes: expect `needs_confirmation` and honor "ask before changing DNS" rules.
- Provisioning is a governed action: it may need confirmation/approval, and `cloud.list`/`cloud.credentials` are the source of truth — never re-create on an uncertain result.
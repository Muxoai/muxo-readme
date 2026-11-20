---
name: muxo-cloud-databases
description: Provision databases through Muxo — compare and create Postgres (Neon, Supabase, Turso, Prisma, ClickHouse, Render, Railway, Fly.io), MongoDB, Redis and vector stores (Upstash, Cloudflare D1). Use when the person wants a database, cache, or vector store.
---

# Muxo Cloud — Databases & caches

Provision databases through Muxo's cloud catalog. Find the right option with `cloud.find_services`, then create with `cloud.create` (`provider`, `service`, `name`). See `muxo-cloud` for the lifecycle and `muxo-cloud-billing` for paid/direct billing.

## Options

- **Postgres**: Neon (`postgres`), Supabase (`project`), Turso (`database`), Prisma (`database`), ClickHouse (`postgres`), Render (`postgres`), Railway (`postgres`), Fly.io (`mpg`).
- **MongoDB**: Railway (`mongo`).
- **Redis / cache**: Upstash (`redis`), Cloudflare (`kv`).
- **Vector / search**: Upstash (`vector`, `search`), Supermemory (`memory`), Cloudflare D1 (`d1`) and Hyperdrive (`hyperdrive`).

## Recommended defaults

- Starter app + auth: **Supabase** or **Neon** (both offer free Postgres; Supabase adds auth and storage).
- Edge/global SQLite: **Turso** or **Cloudflare D1**.
- Cache/Rate limiting: **Upstash Redis**.
- Embeddings/RAG: **Upstash vector** or **Supermemory**.

## Create

```text
cloud.create { provider: "neon", service: "postgres", name: "shop-db" }
```

Leave `plan` empty for the free plan. For a paid plan, take the plan id from `cloud.plans` and set `billing: "direct"`.

## After create

- `cloud.list` to confirm; `cloud.credentials` returns a one-time reveal link — never print secrets.
- `cloud.label` to tag `dev`/`staging`/`prod`; `cloud.schedule_removal` for temporary data.

## Gotchas

- A database that is still `creating` has no credentials yet; wait a few minutes.
- Paid plans bill direct to the workspace's Stripe account and are approved in the console.
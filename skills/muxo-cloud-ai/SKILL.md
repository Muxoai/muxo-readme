---
name: muxo-cloud-ai
description: Provision AI and search services through Muxo — OpenRouter, Perplexity, Exa, Firecrawl, Algolia, Cloudflare Workers AI, Upstash vector, Supermemory, and ElevenLabs. Use when the person wants LLM keys, search, embeddings, memory, or AI infrastructure.
---

# Muxo Cloud — AI, search & memory

Provision AI model keys, search, embeddings, and memory through Muxo. Create with `cloud.create`; use Instant AI tools (see `muxo-instant-tools`) when a key isn't needed.

## Options

- **Model gateways**: OpenRouter (`api`, 500+ models), Perplexity (`api`), Cloudflare Workers AI (`workers-ai`).
- **Search & scrape**: Exa (`api`), Firecrawl (`api`), Algolia (`application`), Upstash (`search`).
- **Embeddings & vector**: Upstash (`vector`), Supermemory (`memory`).
- **Voice**: ElevenLabs (`tts`).
- **Agents & sandboxes**: Athena (`agents`), Chatbase (`agent`), Blaxel (`agent-drive`, `sandbox`), Kernel (`project`), Tabstack (`api`).

## Recommended defaults

- One API for many models: **OpenRouter**.
- Real-time search for RAG: **Exa** or **Perplexity** (Perplexity is paid/direct).
- Embeddings + recall: **Upstash vector** + **Supermemory**.
- Voice: **ElevenLabs**.

## Create

```text
cloud.create { provider: "openrouter", service: "api", name: "agent-llm" }
```

## Billing note

- Free tiers run on Muxo's managed account. Perplexity and other paid plans bill direct to the workspace's Stripe account.
- After create, `cloud.credentials` returns the key as a one-time reveal link — never print it.

## Gotchas

- Prefer the Instant AI tools for one-off generation; provision a key only for an app that needs a persistent API key.
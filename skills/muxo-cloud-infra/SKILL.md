---
name: muxo-cloud-infra
description: Provision supporting cloud infrastructure through Muxo — email, communications, queues, storage and media, observability, feature flags, payments, browser automation, and CI. Use when the person wants email sending, messaging, queues, storage, analytics, feature flags, or a browser sandbox.
---

# Muxo Cloud — Supporting infrastructure

Email, queues, storage, observability, feature flags, payments, browser automation, and CI through Muxo. Create with `cloud.create`; see `muxo-cloud-billing` for paid/direct plans.

## Email & communications

- **Resend** (`email`) — transactional email sending.
- **AgentMail** (`api`) — programmatic inboxes for agents.
- **PostalForm** (`mail`) — physical mail.
- **Customer.io** (`workspace`), **Knock** (`project`), **Quo** (`app`) — messaging/notifications.

## Queues & messaging

- **Upstash QStash** (`qstash`) — serverless messaging/scheduling.
- **Inngest** (`app`) — durable workflows/queues.
- **Cloudflare Queues** (`queues`).

## Storage, media & CDN

- **Cloudinary** (`media`) — images/video transforms and CDN.
- **Railway** (`bucket`) — object storage.
- **here.now** (`hosting`) — static storage + CDN.

## Observability & analytics

- **Datadog** (`observability`), **PostHog** (`analytics`), **Amplitude** (`analytics`), **Mixpanel** (`analytics`), **Pydantic Logfire** (`logfire`).

## Feature flags & experiments

- **Schematic** (`schematic-environment`), **Stigg** (`environment`).

## Payments & retention

- **Churnkey** (`retention`), **RevenueCat** (`app`).

## Browser automation & sandboxes

- **Browserbase** (`project`), **Steel** (`browser`), **Kernel** (`project`), **Cloudflare** (`browser-run`).

## CI

- **Depot** (`api`) — fast builds (paid/direct).

## Gotchas

- Paid-only providers (e.g. Knock, Depot) require direct billing on the workspace's Stripe account.
- Take plan ids from `cloud.plans`; never invent them.
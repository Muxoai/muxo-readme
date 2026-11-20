---
name: muxo-cloud-auth
description: Provision authentication and identity through Muxo — Clerk, WorkOS, Auth0, and Privy, plus Supabase and Neon auth. Use when the person wants sign-in, SSO, or user identity for an app.
---

# Muxo Cloud — Auth & identity

Add sign-in and user identity to an app through Muxo. Create with `cloud.create` (`provider`, `service`, `name`).

## Options

- **Clerk** (`auth`) — full auth (sign-in, sessions, user management); easiest for a new SaaS.
- **WorkOS** (`auth`) — enterprise SSO, SCIM, and directory sync.
- **Auth0** (`client`) — flexible B2B/B2C auth.
- **Privy** (`app`) — auth plus payments for consumer apps.
- **Supabase** (`project`) — database with built-in auth.
- **Neon** (`postgres`) — Postgres with built-in auth (by Databricks).

## Recommended defaults

- New product: **Clerk**.
- Enterprise SSO: **WorkOS** or **Auth0**.
- Database + auth in one: **Supabase**.

## Create

```text
cloud.create { provider: "clerk", service: "auth", name: "acme-auth" }
```

## After create

- `cloud.credentials` returns the publishable/secret keys as a one-time reveal link.
- Wire the callback URL to your deployed host (see `muxo-cloud-hosting`).

## Gotchas

- Never print secret keys; rotate with `cloud.rotate_credentials` if leaked.
- Confirm the provider before creating — auth is hard to swap later.
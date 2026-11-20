---
name: muxo-connectors
description: Work with Muxo connected apps — list which apps are connected, get sign-in links to connect or reconnect, disconnect apps, and handle the needs_connection outcome and OAuth handoff. Use when the person wants to connect, inspect, or manage their SaaS app connections (GitHub, Gmail, Slack, Notion, and 1,000+ more).
---

# Muxo — Connected apps (connectors)

Connected apps let an assistant act on the person's own SaaS accounts through Muxo. They are the `apps` kind of the catalog. See `muxo-catalog` for the full app list.

## The tools

- `apps` — which apps are connected and which need attention: `expired`, `revoked`, or an unfinished sign-in. Read-only; never starts a sign-in.
- `connect` — get a sign-in link the person opens to connect or reconnect an app, or learn it's already connected. After sharing, call again with `wait_seconds` to wait for them to finish.
- `disconnect` — disconnect an app (owners and admins). Replies `needs_confirmation`; call `confirm` only after a clear yes.

REST equivalents: `GET /v1/apps`, `POST /v1/apps/:slug/connect`, `DELETE /v1/apps/:slug`.

## Connection lifecycle

1. `connect` with the app name; share the returned sign-in link. The person picks the right account and reviews the provider's consent screen.
2. Poll `connect` with `wait_seconds` (or `apps`) until the app shows `active`.
3. `run` the action again with the same inputs.

Statuses: `pending` (sign-in unfinished), `active` (ready), `expired` / `revoked` (needs reconnecting).

## The `needs_connection` outcome

When `run` returns `needs_connection`, do not retry blindly:

1. Share the `link` from the reply (or call `connect` for the app).
2. Wait until the person says they finished, or poll `connect` with `wait_seconds`.
3. Call `run` again with the same action and inputs.

App consent is not task approval — a change still waits for confirmation or approval after the app connects.

## Sharing & branding

- A connection is **workspace-wide**: it is created under the workspace id (`user_id = workspaceId`), so every member's runs share the same account. There is no per-member OAuth — "shared connections" across members is automatic.
- The sign-in page is Composio's hosted Connect Link, branded through the workspace's auth config. Muxo does not build its own OAuth UI (`initiate()`), so there is nothing extra to set up for branding.

## Rules of the road

- Only connect what the person asked for. Never connect or disconnect an app they didn't request.
- Never ask for passwords or API keys; apps connect through a sign-in link.
- A read-only assistant can search and read but tools that change anything reply `failed`. Reconnect with full access in Settings > Assistants; never work around it.
- `disconnect` is a destructive, workspace-wide change — confirm the app name and ask before doing it.

## Recognized apps

1,000+ apps; browse with `catalog` (`kind: "apps"`). Recognized brand names include GitHub, GitLab, Bitbucket, Slack, Discord, Gmail, Outlook, Google Calendar/Drive/Docs/Sheets/Slides/Meet/Tasks/Forms, Notion, Confluence, Linear, Jira, Asana, Trello, Monday.com, ClickUp, HubSpot, Salesforce, Pipedrive, Attio, Zendesk, Intercom, Freshdesk, Stripe, Shopify, Airtable, Figma, Canva, Sentry, Datadog, PagerDuty, Mailchimp, SendGrid, Resend, Webflow, WordPress, LinkedIn, X (Twitter), Reddit, Instagram, YouTube, QuickBooks, Xero, DocuSign, Zoom, Calendly, and more.
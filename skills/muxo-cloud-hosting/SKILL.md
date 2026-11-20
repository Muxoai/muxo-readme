---
name: muxo-cloud-hosting
description: Deploy sites and apps through Muxo — compare Vercel, Netlify, Render, Railway, Cloudflare Workers, here.now, WordPress, and Wix, and publish static HTML in one shot. Use when the person wants hosting or to deploy a site.
---

# Muxo Cloud — Hosting & deployment

Deploy sites and apps through Muxo's cloud catalog. Create with `cloud.create` (`provider`, `service`, `name`, optional `html`/`files`).

## Options

- **Static & full-stack**: Vercel (`project`), Netlify (`project`), Render (`static-site`, `web-service`), Railway (`hosting`), Cloudflare (`workers`).
- **One-shot static publish**: pass `html` (or `files: {"index.html": "..."}`) to publish a live page immediately — returns `liveUrl`/`deploymentUrl`.
- **Simple landing page**: here.now (`hosting`), Render static site, Netlify.
- **CMS**: WordPress.com (`site`), Wix (`headless`).
- **App platforms**: CreateOS (`project`), Base44 (`app`).

## Create

```text
cloud.create { provider: "vercel", service: "project", name: "muxo-landing" }
```

For a one-shot static site:

```text
cloud.create { provider: "render", service: "static-site", name: "landing",
                html: "<!doctype html><html>...</html>" }
```

## After deploy

- `cloud.list` for status and the live URL; `cloud.credentials` for env keys (one-time reveal link).
- Wire monitoring (`sentry` as a connected app, or `datadog`/`pydantic` Logfire as cloud).

## Gotchas

- Confirm the provider and plan before creating; a deploy is a provisioning action that may wait for approval.
- Paid hosting plans bill direct; the free tier runs on Muxo's managed account.
---
name: muxo-catalog
description: Muxo's complete catalog — connected apps, Instant tools, and cloud services — with browsing filters, categories, and the full provider and toolkit lists. Use when the person asks "what can you do?", wants to browse or compare options, or needs the full catalog list.
---

# Muxo — Catalog

The catalog is Muxo's complete, paginated index of what an assistant can do, browsed read-only with the `catalog` tool or the `muxo://catalog` resource. Browsing never connects an app, runs an action, provisions a resource, or spends credits.

## Three kinds

| `kind` | Title | What it lists |
| ------ | ----- | ------------- |
| `apps` | Connected apps | 1,000+ SaaS apps (GitHub, Gmail, Slack, Notion, …) with their tool definitions. Includes apps not connected yet. |
| `instant` | Instant tools | Zero-sign-in web, research, AI, data, and browser tools. No separate provider account or sign-in needed. |
| `projects` | Cloud services | Databases, caches, hosting, auth, AI, and more provisioned through Muxo. |

Omit `kind` to see all three sections at once.

## Browsing

- `query` — an app, category, or need ("email", "database", "post on social").
- `app` — browse every tool in one app or toolkit (e.g. `github`).
- `category` — browse one sub-catalog by category id or name. Each section lists its categories with counts.
- `free: true` — only free Instant tools and cloud services with a free plan.
- `max_credits` — only Instant tools that cost at most this many credits per call (1 credit = $0.01).
- `sort` — `relevance` (default; pinned first), `price` (cheapest first), or `name`.
- `limit` — results per section (default 12); `cursor` — page a section's `next` cursor, keeping every other filter unchanged.

## Complete list — `apps` (1,000+ connected apps)

Browse the full dynamic list with `catalog` (`kind: "apps"`). These are the recognized brand names Muxo resolves:

- **Code & repos**: GitHub, GitLab, Bitbucket
- **Chat & meetings**: Slack, Discord, Telegram, WhatsApp, Microsoft Teams, Zoom
- **Email & calendar**: Gmail, Outlook, Google Calendar, Google Meet, Calendly
- **Docs & files**: Notion, Confluence, Google Docs/Sheets/Slides/Drive, OneDrive, SharePoint, Excel, Dropbox, Box
- **Tasks & projects**: Linear, Jira, Asana, Trello, Monday.com, ClickUp, Todoist, Google Tasks
- **CRM & sales**: HubSpot, Salesforce, Pipedrive, Attio
- **Support**: Zendesk, Intercom, Freshdesk
- **Marketing**: Mailchimp, SendGrid, Postmark, Resend, Typeform, Webflow
- **Social**: LinkedIn, X (Twitter), Reddit, Instagram, Facebook, YouTube
- **Commerce & payments**: Stripe, Shopify
- **Design**: Figma, Canva, Miro
- **Engineering**: Sentry, Datadog, PagerDuty
- **Finance**: QuickBooks, Xero
- **Signatures**: DocuSign
- **AI & data**: OpenAI, Anthropic, Exa, Perplexity, Tavily, Firecrawl, Hacker News
- **Cloud (also in `projects`)**: Cloudflare, Vercel, Netlify, Render, Railway, Supabase, Neon, Turso, Upstash, PlanetScale, MongoDB, Clerk, WorkOS

## Complete list — `instant` (73 toolkits + 10 native tools)

**Native tools (free web checks):** `instant.read_page`, `instant.render_page`, `instant.security_headers`, `instant.ai_readiness`, `instant.mcp_probe`, `instant.dns_lookup`, `instant.domain_lookup`, `instant.package_vulnerabilities`, `instant.github_repo`, `instant.hacker_news`.

**Web Search & Research:** exa, parallel, tavily, yousearch, serpapi, serper, openalex
**Web Scraping & Browser:** firecrawl, browser_use, tinyfish, brightdata, supadata
**People & Contact Data:** apollo, peopledatalabs, contactout, hunter, icypeas, leadmagic, lusha, prospeo, tomba, zerobounce
**Company Data:** akta, companyenrich, context_dev, crustdata, predict_leads, thecompaniesapi
**SEO & AI Visibility:** ahrefs, dataforseo
**Social & Creator Data:** scrapecreators, tik_hub, just_one_api
**Places & Weather:** accu_weather, google_maps, yelp
**Media Generation:** bfl, luma_labs, fal_ai, higgsfield
**Audio & Speech:** deepgram, elevenlabs, fish_audio, listennotes
**Documents & OCR:** mathpix
**Market Data:** alpha_vantage, coingecko, eodhd_apis, finnhub, marketstack, twelve_data
**Legal & Public Records:** vaquill
**Artificial Intelligence:** artificial_analysis, deepseek, gemini, jev, minimax, mistral_ai, openai
**Sales & CRM:** clay
**Productivity & Project Management:** magicslides
**Content & Files:** htmlcss_to_image, logo_dev, pdf_co
**Developer Tools:** api_ninjas, api_sports, carsxe, graphhopper, ipinfo_io, strale
**HR & Recruiting:** job_search, web3_career
**Lifestyle & More:** ignav_flights

## Complete list — `projects` (53 providers / 69 cloud services)

- **Databases**: Neon (Postgres), Supabase (project), Turso, Prisma, ClickHouse (Postgres), Fly.io (Postgres), Render (Postgres), Railway (Postgres / MongoDB / Redis), Upstash (Redis / vector), Cloudflare (D1, Hyperdrive, KV)
- **Auth & identity**: Clerk, WorkOS, Auth0, Privy, Supabase (auth)
- **Compute & hosting**: Vercel, Netlify, Render (static site / web service), Railway (hosting), Cloudflare (Worker), here.now, WordPress.com, Wix (headless), CreateOS, Base44
- **AI & search**: OpenRouter, Perplexity, Exa, Firecrawl, Algolia, Cloudflare (Workers AI / browser rendering), Upstash (search / vector), Supermemory, Tabstack, Athena, Chatbase, Blaxel, Kernel, ElevenLabs (text-to-speech)
- **Browser & sandboxes**: Browserbase, Steel, Kernel, Cloudflare (browser rendering), Blaxel (sandbox)
- **Email & comms**: Resend, AgentMail, PostalForm, Customer.io, Knock, Quo
- **Queues & messaging**: Upstash (QStash), Inngest, Cloudflare (Queues)
- **Observability & analytics**: Datadog, PostHog, Amplitude, Mixpanel, Pydantic (Logfire), RevenueCat
- **Feature flags**: Schematic, Stigg
- **Payments & commerce**: Churnkey, RevenueCat, Privy, Wix (headless)
- **Media, storage & CDN**: Cloudinary, Railway (bucket), here.now (storage)
- **CI**: Depot

Direct-billing-only providers (no free plan): Chatbase, Depot, Fly.io, Knock, Perplexity, Quo. Everything else offers a free tier on Muxo's managed account.

## Discovery vs execution

- A `discovery_only` listing means discovery works but execution support is not promised.
- A section `warning` means discovery is unavailable for that section — not that an app is unsupported.
- Browsing never connects. When ready, `connect` an app, `run` an `instant.*` or `app.*` action, or provision with `cloud.*`. See `muxo-connectors`, `muxo-instant-tools`, and `muxo-cloud`.

## Suggestions & next steps

Ask what the person wants to accomplish when the goal is unclear. Offer a few relevant choices — including unconnected apps — and ask before running anything. Never invent action ids or inputs: take them from `search` or an app's tool page in the catalog.
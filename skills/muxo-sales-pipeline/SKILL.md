---
name: muxo-sales-pipeline
description: End-to-end Muxo user story for outbound sales and lead generation — find prospects, enrich people and companies, verify emails, write to the CRM, and send outreach from Gmail. Use when the person wants to build a pipeline, prospect, enrich leads, or run outbound sales.
---

# Outbound sales & lead generation

Find the right people, enrich them, and reach out — all inside the workspace's rules (email domains, approval modes, spending caps).

## End-to-end flow

1. **Find prospects.** Instant People tools: `apollo`, `peopledatalabs`, `contactout`, `hunter`, `lusha`, `prospeo`, `tomba`, `leadmagic`, `icypeas`.
2. **Verify emails.** `zerobounce` before sending, so outreach doesn't bounce.
3. **Enrich the company.** Company Data: `crustdata`, `predict_leads`, `thecompaniesapi`, `akta`, `companyenrich`, `context_dev`.
4. **Store in the CRM.** `app.hubspot.*` (contacts/deals), `app.salesforce.*`, or `app.pipedrive.*`; exact action ids via `search`.
5. **Send the first touch.** `app.gmail.*` or `app.outlook.*`, from the saved sending address.
6. **Track and repeat.** Log outcomes back to the CRM and schedule follow-ups (`app.googlecalendar.*`).

## Key actions

- `instant.*` for people/company/email tools (ids via `search`)
- `app.hubspot.create_contact` / `app.salesforce.query_records` / `app.pipedrive.*` (examples — `search` for exact ids)
- `app.gmail.send_email`
- `remember`, `rules`

## Defaults to remember

- Ideal customer profile and persona (title, industry, company size).
- The CRM object/pipeline and the sending address.
- "Use acme/web for outreach" and "always send from founders@acme.com".

## Guardrails

- Email sends honor the workspace's allowed-email-domains rule and may need confirmation or approval.
- Set a spending cap with `rules` (e.g. "Never spend more than 500 credits a month on Clay") — paid data tools cost credits per call.
- Preview a rule with `rules` (`preview: true`) before adding it.

## Gotchas

- Never guess a prospect's email; verify with a tool or ask.
- Respect consent and don't send bulk unsolicited mail; a "never send to @competitor.com" rule keeps outreach safe.
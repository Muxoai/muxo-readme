---
name: muxo-customer-success
description: End-to-end Muxo plan for customer success — prepare for calls, track account health, send onboarding and check-in emails, and escalate risks. Use when the person wants to onboard, retain, or run health checks on customers.
---

# Customer success & account health

Prepare, engage, and retain customers end to end.

## The plan

**Phase 1 — Prepare**
1. Pull the account from `app.hubspot.*`/`app.salesforce.*` and recent emails from `app.gmail.*`.
2. Check billing state and usage with `app.stripe.*` and Muxo `account`.

**Phase 2 — Engage**
3. Draft (not send) onboarding or check-in emails in `app.gmail.*`.
4. Schedule the next touchpoint with `app.googlecalendar.*`.

**Phase 3 — Act**
5. Send approved emails; log the outcome back to the CRM.
6. Flag at-risk accounts to `app.slack.*` with the reason.

**Phase 4 — Review**
7. Produce a weekly health summary into `app.notion.*` or `app.googlesheets.*`.

## Key actions

- Account health: `app.hubspot.*` / `app.salesforce.*`, `app.stripe.*`
- Calls & check-ins: `app.googlecalendar.*`, `app.gmail.*`
- Notes & escalate: `app.googlesheets.*` / `app.notion.*`, `app.slack.*`, `account`

## Exit criteria

Each account has a recent touchpoint, a health note in the CRM, and risks are escalated with owners.

## Guardrails

- Emails honor domain and approval rules; drafts never send on their own.
- Account data is sensitive — keep it in the CRM, not chat.

## Defaults to remember

- The default CRM pipeline, the health-score fields, and the check-in cadence.

## Gotchas

- "Don't send it" is the default for drafts; only send after a clear yes.
- Match on the customer's canonical email/domain to avoid duplicate records.
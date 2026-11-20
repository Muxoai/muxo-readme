---
name: muxo-recruiting
description: End-to-end Muxo user story for recruiting — source candidates, enrich profiles, reach out by email, schedule interviews, and track the pipeline in Airtable or an ATS. Use when the person wants to hire, source candidates, schedule interviews, or run a recruiting pipeline.
---

# Hiring & recruiting

Source, contact, schedule, and track candidates in one governed flow.

## End-to-end flow

1. **Source.** Instant HR tools `job_search`, `web3_career`; connected `app.linkedin.*` for profiles.
2. **Enrich.** People data (`apollo`, `peopledatalabs`, `contactout`, `hunter`) to find contact details; verify with `zerobounce`.
3. **Reach out.** `app.gmail.*` or `app.outlook.*` from the saved recruiting address.
4. **Schedule.** `app.googlecalendar.*` events, `app.calendly.*` links, or `app.zoom.*` meetings.
5. **Track.** `app.airtable.*` (candidate base) or an ATS (`app.greenhouse.*`, `app.lever.*`) for stages and notes.
6. **Coordinate.** `app.slack.*` to the hiring channel; `notifications` for follow-ups.

## Key actions

- `instant.*` for job and people data (ids via `search`)
- `app.gmail.send_email`, `app.googlecalendar.*`, `app.airtable.*`
- `remember`, `notifications`, `status`

## Defaults to remember

- The role, hiring-manager email, and interview calendar.
- The Airtable base/table or ATS where candidates live.

## Guardrails

- Outreach honors email-domain and approval rules; bulk sending needs a cap.
- Candidate data is personal data — keep it in the ATS, not in chat, and honor data-handling rules.

## Gotchas

- Confirm the candidate's name and role before sending anything; a wrong address is a write you can't undo.
- Schedule in the right timezone; ask when the reply is ambiguous.
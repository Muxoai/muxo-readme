---
name: muxo-google-workspace
description: Work with Google Calendar, Drive, Docs, and Sheets through Muxo — read and create events, find and share files, and edit docs/sheets under the workspace's rules. Use when the person wants to schedule, share files, or edit Google Workspace content.
---

# Google Workspace via Muxo

Connect the relevant app (Google Calendar, Drive, Docs, or Sheets) with `connect`, then `search` for the exact action, and `run` it. Each Google app is its own connection and tool set.

## Common actions

- **Calendar** (`app.googlecalendar.*`): list/create/update events, check availability, find the default `calendar_id` (usually "primary").
- **Drive** (`app.googledrive.*`): search files, read, upload, share.
- **Docs** (`app.googledocs.*`): read and edit documents.
- **Sheets** (`app.googlesheets.*`): read ranges and write cells.

## Defaults to remember

- The default calendar (`calendar_id`) and meeting duration.
- The shared Drive folder or a "team" sheet id.

## Rules & approvals

- Editing shared docs/sheets is a write and may need confirmation.
- Calendar sends invites on your behalf; confirm attendees and timezone first.

## Gotchas

- Google Calendar, Drive, and Docs are separate connections — `apps` shows which are connected.
- Confirm the timezone before scheduling; ask when it's ambiguous.
- For recruiting/scheduling flows, see `muxo-recruiting`.
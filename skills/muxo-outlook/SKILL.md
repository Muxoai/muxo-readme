---
name: muxo-outlook
description: Work with Outlook through Muxo — read and send email, and manage calendar events under the workspace's rules. Use when the person uses Microsoft email and calendar.
---

# Outlook via Muxo

Connect Outlook with `connect`, then `search` for the exact action (e.g. "send outlook email", "list outlook calendar events"), and `run` it.

## Common actions

- Fetch/search mail and send messages.
- List/create/update calendar events.
- Read a message thread.

## Defaults to remember

- The sending address and default calendar.
- Meeting duration and timezone.

## Rules & approvals

- Sends honor email-domain and approval rules; calendar invites go out on your behalf.
- A "only send to @acme.com" rule is a common guardrail.

## Gotchas

- Confirm recipients and timezone before sending or scheduling.
- For scheduling flows, see `muxo-event-planning`.
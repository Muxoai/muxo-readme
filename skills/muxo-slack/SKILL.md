---
name: muxo-slack
description: Work with Slack through Muxo — send messages, read channels, search history, and coordinate under the workspace's rules. Use when the person wants to post, read, or search Slack channels.
---

# Slack via Muxo

Connect Slack with `connect`, then `search` for the exact action (e.g. "send slack message"), and `run` it.

## Common actions

- Send a message to a channel or user.
- Read/list channels and channel history.
- Search messages.

## Defaults to remember

- The default channel (e.g. "#eng" or "#general").
- Whether to notify or post silently.

## Rules & approvals

- Posting is a write: expect `needs_confirmation`, and honor "ask me before posting to Slack" rules.
- A posting cap prevents accidental channel spam.

## Gotchas

- Confirm the exact channel name before posting; don't guess `#random`.
- Keep messages short and human-readable; don't paste raw JSON.
- For incident war rooms, see `muxo-support-incidents`.
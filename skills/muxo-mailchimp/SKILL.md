---
name: muxo-mailchimp
description: Work with Mailchimp through Muxo — read audiences, create and send campaigns, and manage subscribers under the workspace's rules. Use when the person wants to run email marketing.
---

# Mailchimp via Muxo

Connect Mailchimp with `connect`, then `search` for the exact action (e.g. "create mailchimp campaign"), and `run` it.

## Common actions

- List/search audiences and members.
- Create a campaign and send it.
- Add/update subscribers.

## Defaults to remember

- The default audience/list id and sender.
- The brand's footer and unsubscribe handling.

## Rules & approvals

- Sending a campaign is a write to many people: expect confirmation and honor domain rules.
- A "never send without a preview" rule is a safe default.

## Gotchas

- Draft first, send only after a clear yes.
- Respect consent and unsubscribe rules; don't import purchased lists.
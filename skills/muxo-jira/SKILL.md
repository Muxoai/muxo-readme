---
name: muxo-jira
description: Work with Jira through Muxo — find and create issues, update status, and manage projects and sprints under the workspace's rules. Use when the person wants to read, file, or update Jira tickets.
---

# Jira via Muxo

Connect Jira with `connect`, then `search` for the exact action (e.g. "create jira issue"), and `run` it.

## Common actions

- Find issues with JQL; read an issue and its comments.
- Create an issue (project, summary, type, assignee).
- Update status, assignee, or fields; add a comment.

## Defaults to remember

- The default project key (e.g. "ENG") and issue type.
- The board/sprint for planning work.

## Rules & approvals

- Creating/updating issues is a write; deletes are destructive and wait for approval.
- A "never move issues to Done without a comment" rule keeps the board honest.

## Gotchas

- Project keys are short codes — confirm from `search` or the app's tool page.
- JQL is powerful but exact; verify field names before querying.
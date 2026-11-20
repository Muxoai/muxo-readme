---
name: muxo-linear
description: Work with Linear through Muxo — create and update issues, manage teams and cycles, and search under the workspace's rules. Use when the person wants to track work, file issues, or manage sprints in Linear.
---

# Linear via Muxo

Connect Linear with `connect`, then `search` for the exact action (e.g. "create linear issue"), and `run` it.

## Common actions

- Create an issue (team, title, description, priority, assignee).
- Update an issue status/assignee.
- List/search issues and teams.
- Manage cycles/projects.

## Defaults to remember

- The default team id (e.g. "ENG") and project.
- The default assignee or label set.

## Rules & approvals

- Creating issues is low-risk but still a write; destructive changes (delete) wait for approval.
- A "never close issues without a reason" rule keeps the board honest.

## Gotchas

- Team ids are short codes like "ENG"; confirm the right one from `search` or the app's tool page.
- Set a descriptive title so the issue is useful without opening it.
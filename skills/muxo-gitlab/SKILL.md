---
name: muxo-gitlab
description: Work with GitLab through Muxo — manage issues, merge requests, repos, and CI pipelines under the workspace's rules. Use when the person wants to read, file, or review GitLab work.
---

# GitLab via Muxo

Connect GitLab with `connect`, then `search` for the exact action (e.g. "create gitlab issue", "list gitlab merge requests"), and `run` it.

## Common actions

- List/search issues and merge requests.
- Create an issue or merge request.
- Read repos, files, and pipeline status.

## Defaults to remember

- The default project (namespace/project) and target branch.

## Rules & approvals

- Creating MRs/issues is a write; destructive repo actions wait for approval.
- A "never merge without a review" rule protects main.

## Gotchas

- Confirm the project path and source/target branches before opening an MR.
- For release workflows, see `muxo-devops-release`.
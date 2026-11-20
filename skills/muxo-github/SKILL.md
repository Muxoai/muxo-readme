---
name: muxo-github
description: Work with GitHub through Muxo — search and create issues, pull requests, read repos, and review code under the workspace's rules. Use when the person wants to manage GitHub issues, PRs, repos, or code without leaving their harness.
---

# GitHub via Muxo

Search for the exact action id with `search` (e.g. "create github issue"), then `run` it. Never guess ids or input names.

## Common actions

- List/search issues, create an issue, comment, close.
- List repos, read a repository, read files/contents.
- Create and merge pull requests, request review.
- Add reviewers, labels, and milestones.

`app.github.*` — `search` returns the exact id and required inputs.

## Defaults to remember

- The default repo as `owner`/`repo` (e.g. "acme/web").
- Which branch to target for PRs.

## Rules & approvals

- Destructive actions (delete repo/branch, force actions) are high-risk and will wait for confirmation or approval.
- A "never delete in GitHub" rule is a good default; add it with `rules`.

## Gotchas

- Confirm the target repo and branch before creating a PR.
- Pass `reason` to `run` ("fix the broken login test") so approvers see why.
- Quote the returned ref (e.g. `A-7K2M9Q`) so the person can find it at https://app.muxo.ai.
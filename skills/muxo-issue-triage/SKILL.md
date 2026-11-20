---
name: muxo-issue-triage
description: Triage issues across GitHub, Linear, Slack, and Jira — create cross-linked tickets, notify channels, and track blockers. Use when the user wants to turn a bug report or PR into a tracked issue, or manage issues across multiple tools.
---

# Muxo — Issue Triage

Turn a bug report, PR, or Slack message into tracked issues across multiple platforms, notify stakeholders, and monitor blockers.

## Workflow

### 1. Source Detection

Where did the issue originate?

| Source | Action |
|--------|--------|
| Slack thread | `run` → `app.slack.get_thread_replies` |
| GitHub PR | `run` → `app.github.get_pull_request` |
| Customer email | `run` → `app.gmail.fetch_emails` |
| Sentry alert | `run` → `app.sentry.list_issues` |

### 2. Create Tracked Issue

Use `search` to find the right create action, then `run`:

```javascript
// GitHub
run({ action: "app.github.create_issue", input: { owner: "acme", repo: "api", title: "...", body: "...", labels: ["bug"] }, reason: "Track customer-reported bug" })

// Linear
run({ action: "app.linear.create_issue", input: { team_id: "ENG", title: "...", description: "..." }, reason: "Track in Linear" })

// Jira
run({ action: "app.jira.create_issue", input: { project_key: "ENG", summary: "...", description: "...", issue_type: "Bug" }, reason: "Track in Jira" })
```

### 3. Cross-Link

Update the original source with a link to the new issue:

```javascript
// Post Linear URL to Slack thread
run({ action: "app.slack.send_message", input: { channel: "#bugs", text: "Tracked in Linear: <url>" }, reason: "Notify channel" })

// Add GitHub issue URL to Linear description
```

### 4. Notify & Assign

```javascript
// Send email to on-call
run({ action: "app.gmail.send_email", input: { recipient_email: "oncall@acme.com", subject: "New P1: ...", body: "..." }, reason: "Alert on-call" })

// Post to incident channel
run({ action: "app.slack.send_message", input: { channel: "#incidents", text: "🚨 New P1 tracked in ENG-45" }, reason: "Incident channel notification" })
```

### 5. Track Blockers

Set up monitoring:

```javascript
// Check if issue is still open after 24h
// Use status with query: "linear issue ENG-45"

// If blocked, escalate via approvals (for high-risk writes)
approvals({ ref: "<PLAN_REF>", decision: "approve", reason: "Escalating blocker" })
```

## Multi-App Templates

### Customer Bug → GitHub + Linear + Slack
1. `search` → find `app.github.create_issue`
2. `run` → create GitHub issue with customer context
3. `search` → find `app.linear.create_issue`
4. `run` → create Linear issue linked to GitHub
5. `search` → find `app.slack.send_message`
6. `run` → post summary to #bugs with both links

### PR Review Request → Linear + Calendar + Slack
1. `search` → find `app.linear.create_issue` (type: review-request)
2. `run` → create issue with PR link
3. `search` → find `app.googlecalendar.create_event` (review meeting)
4. `run` → schedule review if complex
5. `search` → find `app.slack.send_message`
6. `run` → notify reviewers in #code-reviews

## Governance

- Every `run` that changes state goes through policy checks
- Financial actions (bounty, reward) use `execute_financial` lane
- Destructive actions (close/delete) use `execute_destructive` lane
- Always pass `reason` so approvers understand context

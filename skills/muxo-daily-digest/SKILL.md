---
name: muxo-daily-digest
description: Build an automated daily digest by running parallel read actions across connected apps, then compiling a cross-app intelligence report with actionable insights.
---

# Muxo — Daily Digest

Run parallel, read-only governed actions across connected apps and compile a cross-app digest with automated insights.

## Workflow (inspired by Composio's multi-execution)

### 1. Discover Connections
Call `apps` to see which apps are connected. Filter to actively used work apps:
- Slack, GitHub, Notion, Linear, Gmail, Google Calendar

### 2. Parallel Fetch (independent reads)

Use `search` to find exact action ids, then call `run` for each app independently. These are read-only and can run conceptually in parallel:

```
# Slack — unread mentions and DMs
run({ action: "app.slack.list_messages", input: { unread: true, limit: 20 } })

# GitHub — PRs and issues
run({ action: "app.github.list_pull_requests", input: { state: "open", limit: 10 } })

# Notion — recently edited
run({ action: "app.notion.search_pages", input: { sort: "last_edited_time", limit: 10 } })

# Linear — active issues
run({ action: "app.linear.list_issues", input: { state: "active", limit: 10 } })

# Gmail — unread
run({ action: "app.gmail.fetch_emails", input: { query: "is:unread", max_results: 10 } })
```

### 3. Cross-App Correlation

Link related items across platforms:

| Pattern | How to Detect |
|---------|--------------|
| PR ↔ Issue | GitHub PR title matches Linear issue title |
| Meeting → Action | Calendar event attendees → Slack thread / Notion doc |
| Email → Task | Gmail subject contains Linear issue ID |
| Deploy → Announcement | GitHub merge → Slack #deploys channel |

### 4. Intelligent Summary

Structure the digest:

```markdown
# Daily Digest — [Date]

## 📊 Velocity
- [x] PRs merged: N (avg review time: Xh)
- [x] Issues closed: N / opened: N
- [x] Blockers: N (list them)

## 🔥 Needs Attention
- [Owner] [Item] — [App] — [Why urgent]

## 📅 Calendar
- [Meeting] with [Attendees] → [Outcome or action]

## 💬 Key Conversations
- [Channel] — [Topic] — [Decision if any]

## ✅ Action Items
- [ ] [Task] — [Owner] — [Due]
```

## Automation via Muxo Memory

Use `remember` to build preferences:

```javascript
// Once per workspace
remember({ app: "daily-digest", key: "time", value: "9am UTC" })
remember({ app: "daily-digest", key: "apps", value: "slack,github,linear,notion" })
remember({ app: "daily-digest", key: "focus", value: "blockers,deploys,review-requests" })
```

Then in future sessions:
```javascript
recall({ query: "daily-digest" }) // → auto-fills preferences
```

## Tips

- Read-only actions cost fewer/0 credits on the Free plan
- Identical lookups within 10 minutes reuse results at no charge
- Use `fresh: true` when you need real-time data after a change

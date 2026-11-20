---
name: muxo-customer-comms
description: "Customer-facing email communications for Muxo — onboarding sequences, promotional campaigns, product announcements, usage reports, and transactional customer emails. Use when building email-driven customer journeys, drip campaigns, or integrating email providers (Resend, AgentMail, Customer.io, PostalForm) into Muxo."
---

# Muxo — Customer Communications

Send **customer-facing** emails through Muxo: onboarding sequences, promotional campaigns, product announcements, usage reports, and transactional notifications. These are sent via **Composio Connected Apps** (Resend, AgentMail, Customer.io, etc.) or **Muxo Instant tools**.

## Architecture

```
┌─────────────────────────────────────────────────┐
│  Customer Communication Layer                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐ │
│  │ Onboarding │  │Promotional │  │Transactional│ │
│  │ Sequence   │  │ Campaigns  │  │ Notifications│ │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘ │
│        │               │               │          │
│  ┌─────┴──────┐  ┌─────┴──────┐  ┌─────┴──────┐ │
│  │ Resend     │  │ Customer.io│  │ AgentMail  │ │
│  │ (SMTP/API) │  │ (REST)     │  │ (SMTP)     │ │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘ │
│        └─────────────────┴──────────────┘        │
│                      │                            │
│               Muxo MCP (execute)                   │
│                      │                            │
│               app.* / instant.*                    │
└─────────────────────────────────────────────────┘
```

## Providers

### Resend (`app.resend.*`)

Best for: Transactional email, SMTP relay, simple API sends

```javascript
// Send a transactional email via Resend
execute({
  action: "app.resend.send_email",
  input: {
    from: "welcome@muxo.ai",
    to: "user@example.com",
    subject: "Welcome to Muxo!",
    html: "<h1>Welcome</h1><p>...</p>",
    text: "Welcome..."
  },
  reason: "Welcome new user"
})
```

### AgentMail (`app.agentmail.*`)

Best for: AI agent email handling, email conversations, inbox management

```javascript
// Send via AgentMail
execute({
  action: "app.agentmail.send_email",
  input: {
    to: "user@example.com",
    subject: "Your weekly Muxo digest",
    body: "..."
  }
})
```

### Customer.io (`app.customerio.*`)

Best for: Drip campaigns, behavioral triggers, segments

```javascript
// Trigger a Customer.io campaign event
execute({
  action: "app.customerio.track_event",
  input: {
    customer_id: "user@example.com",
    name: "started_onboarding",
    data: { step: 1 }
  }
})
```

### PostalForm (`app.postalform.*`)

Best for: Forms that trigger emails, simple transactional

```javascript
execute({
  action: "app.postalform.send",
  input: {
    to: "user@example.com",
    template: "welcome-v2",
    variables: { name: "Alice" }
  }
})
```

### Knock (`app.knock.*`)

Best for: In-app + email notifications, preference center

```javascript
execute({
  action: "app.knock.trigger_workflow",
  input: {
    key: "new-feature-announcement",
    recipients: ["user@example.com"],
    data: { feature_name: "Composio Integration" }
  }
})
```

## Onboarding Email Sequence

| Step | Trigger | Provider | Action |
|------|---------|----------|--------|
| 1. Welcome | Sign-up | Resend | `app.resend.send_email` |
| 2. Quick-start | 24h later | Customer.io drip | `app.customerio.track_event` |
| 3. First action | Ran first tool | Resend | `app.resend.send_email` |
| 4. Weekly digest | Monday 9am | AgentMail | `app.agentmail.send_email` |
| 5. Re-engagement | 14 days inactive | Customer.io | `app.customerio.track_event` |
| 6. Upgrade prompt | Usage at 80% | Resend | `app.resend.send_email` |

## Promotional Campaigns

### Product Announcement

```javascript
// Via Resend (batch to ≤50)
execute({
  action: "app.resend.send_email",
  input: {
    from: "newsletter@muxo.ai",
    to: ["user1@example.com", "user2@example.com"],
    subject: "New: Composio Composition Interface",
    html: "<h1>New integration</h1><p>...</p>",
    text: "New integration..."
  },
  reason: "Product announcement"
})
```

### Feature Highlight

```javascript
// Via Customer.io (behavioral segment)
execute({
  action: "app.customerio.track_event",
  input: {
    customer_id: "user@example.com",
    name: "feature_highlight_email",
    data: { feature: "workflow_sessions" }
  }
})
```

## Transactional Customer Emails

| Event | Provider | Example Action |
|-------|----------|---------------|
| Approval decision | Resend | `app.resend.send_email` with decision details |
| Billing receipt | Resend | `app.resend.send_email` with Stripe receipt |
| Connection expired | Resend | `app.resend.send_email` with reconnect link |
| Credit low | Resend | `app.resend.send_email` with top-up link |
| Weekly digest | AgentMail | `app.agentmail.send_email` with activity summary |

## Building a Drip Campaign

Use **workspace memory** to track campaign state:

```javascript
// Step 1: Enroll user
remember({ app: "onboarding", key: "user_email", value: "alice@example.com" })
remember({ app: "onboarding", key: "step", value: "1" })

// Step 2: Send welcome
execute({
  action: "app.resend.send_email",
  input: {
    to: "alice@example.com",
    from: "welcome@muxo.ai",
    subject: "Welcome to Muxo",
    html: renderWelcomeEmail("Alice")
  }
})

// Step 3: Mark progress
remember({ app: "onboarding", key: "step", value: "2" })

// Next session: recall state and continue
recall({ query: "onboarding step" })
```

## Email Templates

Reuse Muxo's branded template style:

```
Header: Muxo logo + name
Body: h1 title, 15px body text, red CTA button
Footer: Settings link, muxo.ai, unsubscribe
```

Colors: red `#fc2e2e`, ink `#1c1c1a`, cream `#efecdf`

## Governance

- Customer emails must respect workspace rules and provider TOS
- Promotional emails need unsubscribe links (provider handles this)
- Transactional emails (receipts, alerts) don't need unsubscribe
- Always pass `reason` to `execute` for audit trail
- Rate limits vary by provider — search for `app.<provider>.rate_limits`

## References

- `muxo-onboarding` — First-run guided setup skill
- `muxo-daily-digest` — Cross-app activity summary (content for weekly emails)
- `muxo-activity-summary` — Company activity summary
- `muxo-catalog` — Browse email providers (`resend`, `agentmail`, `customerio`, `postalform`, `knock`)

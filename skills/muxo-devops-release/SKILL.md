---
name: muxo-devops-release
description: End-to-end Muxo plan for release and incident management — gather release status, cut a deploy, monitor errors, page on-call, file fixes, and write the postmortem. Use when the person wants to ship a release, triage an outage, or run an on-call rotation.
---

# Release & incident management

Ship a release and handle incidents end to end with a single governed flow.

## The plan

**Phase 1 — Release readiness**
1. Summarize open PRs and issues with `app.github.*` and `app.linear.*`.
2. Run pre-flight checks: `instant.dns_lookup`, `instant.security_headers`, `instant.package_vulnerabilities`.

**Phase 2 — Deploy**
3. Deploy with `cloud.create` (`vercel`, `render`, `netlify`, or `cloudflare` Worker) or trigger CI.
4. Announce in `app.slack.*`.

**Phase 3 — Monitor & respond**
5. Watch errors in `sentry` (connected app) and `datadog`/`pydantic` Logfire (cloud).
6. On failure, page on-call with `app.pagerduty.*` and open a war room in `app.slack.*`.
7. For a full stop, `pause` Muxo with a reason, then `confirm` with its ref once the person says yes.

**Phase 4 — Fix & learn**
8. File the bug with `app.github.create_issue`; open the fix PR.
9. Write the postmortem in `app.notion.*` with follow-up issues.

## Key actions

- Release: `status`, `app.github.*`, `app.linear.*`, `app.notion.*`
- Monitor & page: `app.pagerduty.*`, `app.slack.*`, `cloud.create`, `instant.dns_lookup`, `instant.security_headers`, `instant.package_vulnerabilities`
- Full stop: `pause`

## Exit criteria

The release is live (or rolled back), the incident is triaged with an owner, and a postmortem is saved.

## Guardrails

- Deploys and pages are high-impact writes: confirm before triggering.
- After a timeout or unclear result, check `status` before retrying so nothing ships twice.

## Defaults to remember

- Default repo, deploy target, on-call channel, and pager escalation path.

## Gotchas

- Roll back rather than piling on fixes mid-incident.
- Keep the actual error text and the run ref together for the postmortem.
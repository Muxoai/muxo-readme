# Security Policy

Muxo takes security seriously. Thank you for helping keep Muxo and its users safe.

## Reporting a vulnerability

**Please do not open a public issue for a security problem.**

Report it privately by email to **security@muxo.ai**. Include:

- a short description of the issue and its impact,
- steps to reproduce, or a proof of concept,
- the workspace, endpoint or surface affected (for example the MCP server, the console, or the website),
- your contact details if you would like a reply.

We aim to acknowledge reports within two business days and to keep you updated as we
investigate. Please give us a reasonable amount of time to fix the issue before any public
disclosure.

## Scope

In scope:

- `api.muxo.ai` (REST API and MCP server) and `app.muxo.ai` (console)
- `muxo.ai` and `docs.muxo.ai`
- Authentication, authorization, approvals, secrets handling and billing flows

Out of scope:

- Denial-of-service testing, spam, or social engineering
- Vulnerabilities in third-party apps and cloud providers that Muxo connects to
- Reports that require a compromised account or physical access

## Handling secrets

If you believe a credential was exposed, tell us right away at **security@muxo.ai** and rotate
it in the affected provider. Never include live secrets in a report; use redacted examples.

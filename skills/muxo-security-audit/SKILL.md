---
name: muxo-security-audit
description: End-to-end Muxo plan for security and readiness checks — audit DNS, TLS, security headers, AI discoverability, and package vulnerabilities, then file findings. Use when the person wants to check a site's or project's security posture.
---

# Security & readiness audit

Audit a domain or project end to end using Muxo's free native checks.

## The plan

**Phase 1 — Surface**
1. `instant.domain_lookup` — registration, expiry, nameservers.
2. `instant.dns_lookup` — A, MX, TXT, CAA records (SPF/DMARC for email).

**Phase 2 — Web posture**
3. `instant.security_headers` — grade CSP, HSTS, and other headers.
4. `instant.ai_readiness` — llms.txt / crawler discoverability.

**Phase 3 — Code & services**
5. `instant.package_vulnerabilities` — known CVEs for dependencies.
6. `instant.mcp_probe` — check an MCP endpoint's tools, if relevant.

**Phase 4 — Report & fix**
7. Compile findings into `app.notion.*`/`app.googledocs.*`, file issues in `app.github.*`, and assign owners.

## Key actions

- Web checks: `instant.security_headers`, `instant.dns_lookup`, `instant.domain_lookup`, `instant.mcp_probe`, `instant.ai_readiness`
- Code & deps: `instant.package_vulnerabilities`, `app.github.*`
- File findings: `app.googledocs.*` / `app.notion.*`

## Exit criteria

Each check has a result, findings are prioritized with owners, and follow-up issues are filed.

## Guardrails

- Native checks are read-only and free; filing issues or docs is a write and waits for confirmation.
- Only scan domains and packages the person owns or is authorized to check.

## Defaults to remember

- The asset inventory (domains, repos) and the report destination.

## Gotchas

- Native checks are point-in-time; note the timestamp in the report.
- Fix the highest-severity finding first, not the easiest.
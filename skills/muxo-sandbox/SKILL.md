---
name: muxo-sandbox
description: Provision and use a remote sandbox through Muxo — E2B, Daytona, Browserbase, Steel, Blaxel, or Kernel for isolated code execution, browser automation, and bulk data processing. Use when the person wants a sandbox to run untrusted code, drive a browser, or process large tool results.
---

# Muxo — Remote sandboxes

Muxo provisions sandbox providers through the cloud catalog. The sandbox is a governed resource: `cloud.create` provisions it, `cloud.credentials` returns its one-time reveal link, and every use is subject to the same rules and receipts as any other action.

## Provision

Find options with `cloud.find_services` (category `sandbox` or `browser`), then create:

```text
cloud.create { provider: "e2b", service: "sandboxes", name: "agent-sandbox" }
```

Providers by need:

- **Code execution (untrusted)**: E2B, Daytona, Blaxel, Kernel.
- **Browser automation**: Browserbase, Steel, Cloudflare browser rendering.
- **Bulk data processing**: any sandbox, or Supermemory for recall — see `muxo-cloud-ai`.

## Use

1. `cloud.list` to confirm it is `active`.
2. `cloud.credentials` → a one-time `/reveal/:token` link. Append its `.env` output locally; never print secret values in chat.
3. Drive the sandbox with its own SDK/API from your code, using the returned key/URL.

## Guardrails

- Provisioning is a governed action: it may need confirmation or approval, and paid plans need direct billing (see `muxo-cloud-billing`).
- Sandboxes run arbitrary code — treat their output as untrusted, and never run provider code inside your own agent context.
- Use `cloud.schedule_removal` (`delete_after`) for throwaway sandboxes so they don't linger.

## Gotchas

- Muxo has a first-class governed workbench — `workbench.run` (script), `workbench.run_bash` (shell command), and `workbench.write_file` / `read_file` / `list_files` (files area) — that runs code in an isolated, persistent per-workspace sandbox on Muxo's own infrastructure. Use it for bulk data processing and multi-step computation; print summaries, not full results. Provisioned sandboxes (E2B, Daytona, …) are still the way to run code with a specific provider SDK.
- A sandbox that is still `creating` has no credentials yet; wait a few minutes, then `cloud.credentials`.
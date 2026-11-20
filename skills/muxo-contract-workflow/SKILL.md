---
name: muxo-contract-workflow
description: End-to-end Muxo plan for contracts and e-signatures — draft from a template, fill fields, send for signature, track status, and file the signed copy. Use when the person wants to prepare, send, or track a contract or agreement.
---

# Contracts & e-signatures

Move a contract from template to signed, filed document end to end.

## The plan

**Phase 1 — Draft**
1. Start from the template in `app.googledocs.*`/`app.notion.*` or `app.dropbox.*`.
2. Fill the counterparty and terms from the CRM (`app.hubspot.*`/`app.salesforce.*`) and any saved defaults.

**Phase 2 — Send**
3. Send the envelope with `app.docusign.*` and the signing link by `app.gmail.*`.

**Phase 3 — Track**
4. Poll the signature status and nudge the signer when pending.

**Phase 4 — File**
5. Save the signed copy to `app.googledrive.*`/`app.dropbox.*` and log it to the CRM.

## Key actions

- Draft & send: `app.googledocs.*` / `app.googledrive.*`, `app.gmail.*`
- Sign & track: `app.docusign.*`
- CRM & file: `app.hubspot.*` / `app.salesforce.*`, `app.dropbox.*` / `app.notion.*`

## Exit criteria

The contract is signed (or the blocker is identified), the signed copy is filed, and the CRM is updated.

## Guardrails

- Contracts are high-stakes writes: show the full draft before sending and wait for a clear yes.
- Never alter legal terms without an explicit instruction.

## Defaults to remember

- The template location, filing folder, and standard counterparty fields.

## Gotchas

- Confirm the signer's name and email before sending the envelope.
- Track by the envelope id; don't resend a duplicate envelope to the same signer.
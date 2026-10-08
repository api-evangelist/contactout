---
generated: '2026-10-07'
method: generated
name: contactout-verify-emails
description: Verify one address synchronously, or submit a batch and poll the job until the results are ready.
api: openapi/_ae-authored/contactout-openapi-generated.yml
operations:
- emailVerifierSingle
- emailVerifierBulk
- emailVerifierBulkGet
source: operationIds verified in openapi/_ae-authored/contactout-openapi-generated.yml; steps read from https://api.contactout.com
---

# Verify email deliverability singly or in bulk

Verify one address synchronously, or submit a batch and poll the job until the results are ready.

## Auth
- Send `token: <YOUR_API_TOKEN>` on every request (API keys are issued by booking a meeting). See `authentication/contactout-authentication.yml`.

## Rules
- No idempotency key exists: do not retry a credit-consuming call blindly (`conventions/contactout-conventions.yml`).
- On 429 wait `retry-after` seconds; on 403 check credits with `usageStats` (`errors/contactout-problem-types.yml`, `rate-limits/contactout-rate-limits.yml`).

## Steps
1. **One address** — `emailVerifierSingle` (`GET /v1/email/verify`, `email`). Cost: 1 verifier credit when the result is valid or invalid.
2. **A batch** — `emailVerifierBulk` (`POST /v1/email/verify/batch`) with `callback_url`, `emails`; keep the returned job identifier.
3. **Poll** — `emailVerifierBulkGet` (`GET /v1/email/verify/batch/{jobUuid}`) until the job reports its results. Respect `retry-after` on any 429.

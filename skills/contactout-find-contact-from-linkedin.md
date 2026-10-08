---
generated: '2026-10-07'
method: generated
name: contactout-find-contact-from-linkedin
description: Check whether a work email and phone exist for a LinkedIn profile before spending credits, then reveal the contact info and read the remaining credit balance.
api: openapi/_ae-authored/contactout-openapi-generated.yml
operations:
- contactCheckerWorkEmailChecker
- contactCheckerPhoneNumberChecker
- contactInfoSingleFromLinkedinProfile
- usageStats
source: operationIds verified in openapi/_ae-authored/contactout-openapi-generated.yml; steps read from https://api.contactout.com
---

# Find a verified email and phone for a LinkedIn profile

Check whether a work email and phone exist for a LinkedIn profile before spending credits, then reveal the contact info and read the remaining credit balance.

## Auth
- Send `token: <YOUR_API_TOKEN>` on every request (API keys are issued by booking a meeting). See `authentication/contactout-authentication.yml`.

## Rules
- No idempotency key exists: do not retry a credit-consuming call blindly (`conventions/contactout-conventions.yml`).
- On 429 wait `retry-after` seconds; on 403 check credits with `usageStats` (`errors/contactout-problem-types.yml`, `rate-limits/contactout-rate-limits.yml`).

## Steps
1. **Check first (no credits)** — `contactCheckerWorkEmailChecker` (`GET /v1/people/linkedin/work_email_status`) and `contactCheckerPhoneNumberChecker` (`GET /v1/people/linkedin/phone_status`) with `profile` = the regular LinkedIn URL (not Sales Navigator / Recruiter). The reference lists these under Cost as "Does not consume credits"; they are rate-limited at 150 requests per minute.
2. **Reveal** — `contactInfoSingleFromLinkedinProfile` (`GET /v1/people/linkedin`) with `profile` and `include_phone=true` only when step 1 said a phone exists. Cost: 1 email credit if an email is found and 1 phone credit if a phone is found.
3. **Account for it** — `usageStats` (`GET /v1/stats`, `period=YYYY-MM`) to read usage and remaining credits; a 403 "You're out of credits" means stop and contact the sales manager.

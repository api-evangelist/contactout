---
generated: '2026-10-07'
method: generated
name: contactout-prospect-search
description: Size a prospect list for free, page through matching people, then pull decision makers at the target companies.
api: openapi/_ae-authored/contactout-openapi-generated.yml
operations:
- peopleCount
- peopleSearch
- decisionMakers
- companySearch
source: operationIds verified in openapi/_ae-authored/contactout-openapi-generated.yml; steps read from https://api.contactout.com
---

# Count, search and find decision makers

Size a prospect list for free, page through matching people, then pull decision makers at the target companies.

## Auth
- Send `token: <YOUR_API_TOKEN>` on every request (API keys are issued by booking a meeting). See `authentication/contactout-authentication.yml`.

## Rules
- No idempotency key exists: do not retry a credit-consuming call blindly (`conventions/contactout-conventions.yml`).
- On 429 wait `retry-after` seconds; on 403 check credits with `usageStats` (`errors/contactout-problem-types.yml`, `rate-limits/contactout-rate-limits.yml`).

## Steps
1. **Size the list** — `peopleCount` (`POST /v1/people/count`) with the same filter body as the search; it returns a count and (per the Cost section) does not consume search credits.
2. **Search** — `peopleSearch` (`POST /v1/people/search`) with filters such as `name`, `job_title`, `past_job_title`, `job_function`, `seniority`, `current_titles_only`, `include_related_job_titles`, `match_experience`, `skills`, `languages` and paginate with `page` / `page_size`. Cost: 1 search credit for each profile returned, plus an email/phone credit per profile where `reveal_info` is true and info is found. Rate limit: 60 requests per minute.
3. **Target companies** — `companySearch` (`POST /v1/company/search`) with filters such as `linkedin_url`, `name`, `domain`, `size`, `hq_only`, `location`, `industries`, `technologies`; then `decisionMakers` (`GET /v1/people/decision-makers`, `domain` or `name` or `linkedin_url`, `reveal_info`) for the key people at each company.

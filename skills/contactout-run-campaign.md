---
generated: '2026-10-07'
method: generated
name: contactout-run-campaign
description: List campaigns, add recipients to one, start it, and stop it again if needed.
api: openapi/_ae-authored/contactout-openapi-generated.yml
operations:
- campaignsListCampaigns
- campaignsAddRecipients
- campaignsStartCampaign
- campaignsStopCampaign
- campaignsRemoveRecipients
source: operationIds verified in openapi/_ae-authored/contactout-openapi-generated.yml; steps read from https://api.contactout.com
---

# Add recipients to a campaign and start it

List campaigns, add recipients to one, start it, and stop it again if needed.

## Auth
- Send `token: <YOUR_API_TOKEN>` on every request (API keys are issued by booking a meeting). See `authentication/contactout-authentication.yml`.

## Rules
- No idempotency key exists: do not retry a credit-consuming call blindly (`conventions/contactout-conventions.yml`).
- On 429 wait `retry-after` seconds; on 403 check credits with `usageStats` (`errors/contactout-problem-types.yml`, `rate-limits/contactout-rate-limits.yml`).

## Steps
1. **Find the campaign** — `campaignsListCampaigns` (`GET /v1/campaigns`, paginated) and take its `campaign_id`; the reference also explains how to find the id in the ContactOut dashboard.
2. **Add recipients** — `campaignsAddRecipients` (`POST /v1/campaigns/{campaignId}/recipients`) with `recipients`.
3. **Start** — `campaignsStartCampaign` (`POST /v1/campaigns/{campaignId}/start`).
4. **Reverse if needed** — `campaignsStopCampaign` (`POST /v1/campaigns/{campaignId}/stop`) stops a running campaign and `campaignsRemoveRecipients` (`DELETE /v1/campaigns/{campaignId}/recipients`) removes recipients; no reversal window is documented (see `conventions/contactout-conventions.yml`).

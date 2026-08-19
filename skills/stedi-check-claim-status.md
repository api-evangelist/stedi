---
generated: '2026-08-15'
method: generated
name: Check the status of a submitted claim
description: Resolve a claim-status-capable payer, run an X12 276 inquiry, and convert the 277 status report.
api: openapi/stedi-real-time-claim-status-api-openapi.yml
operations: [SearchPayers, ClaimStatus, ConvertReport277]
source: >-
  Grounded in arazzo/stedi-claim-status-inquiry-workflow.yml; operationIds verified verbatim in
  openapi/stedi-payers-api-openapi.yml, openapi/stedi-real-time-claim-status-api-openapi.yml and
  openapi/stedi-claim-acknowledgments-api-openapi.yml.
---

# Check the status of a submitted claim

The 276/277 round trip, for claims already at the payer.

## Auth
- `Authorization: <STEDI_API_KEY>`. See `authentication/stedi-authentication.yml`.

## Steps
1. **Resolve the payer** — `SearchPayers` (`GET /payers/search`). Confirm the payer supports
   real-time claim status before inquiring; support varies by payer and program.
2. **Inquire** — `ClaimStatus` (`POST /change/medicalnetwork/claimstatus/v2` on
   `https://healthcare.us.stedi.com/2024-04-01`). Identify the claim by provider, subscriber and
   either the payer claim control number or the original claim's identifying fields. Use
   `ClaimStatusRawX12` only if you generate X12 yourself.
3. **Read the acknowledgment trail** — `ConvertReport277`
   (`GET /change/medicalnetwork/reports/v2/{transactionId}/277`) for the 277CA on the original
   submission, which explains rejections that never reached adjudication.

## Errors
- A payer that cannot find the claim answers inside the 277, not with a 404. Branch on the status
  category codes, not on the HTTP status.
- Real-time claim status is NOT supported in test mode. See `sandbox/stedi-sandbox.yml`.
- Errors follow `{error, message}`; see `errors/stedi-problem-types.yml`.

## Notes
- Concurrency limits apply per endpoint category and are not observable from headers; treat `429`
  as either a rate or a concurrency breach. See `rate-limits/stedi-rate-limits.yml`.

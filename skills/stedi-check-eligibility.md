---
generated: '2026-08-15'
method: generated
name: Check patient eligibility and benefits
description: Resolve the payer in the Stedi Payer Network, then run an X12 270 eligibility inquiry and read the 271 benefits response.
api: openapi/stedi-real-time-eligibility-check-api-openapi.yml
operations: [SearchPayers, EligibilityCheck]
source: >-
  Grounded in arazzo/stedi-eligibility-check-workflow.yml; operationIds verified verbatim in
  openapi/stedi-payers-api-openapi.yml and
  openapi/stedi-real-time-eligibility-check-api-openapi.yml.
---

# Check patient eligibility and benefits

The 270/271 round trip. Never hard-code a payer id — resolve it first, because Stedi payer ids
are Stedi's own identifiers and payers change program membership.

## Auth
- `Authorization: <STEDI_API_KEY>` header. No OAuth on REST. See `authentication/stedi-authentication.yml`.
- Test keys hit the same hosts; only the key type changes. See `sandbox/stedi-sandbox.yml`.

## Steps
1. **Find the payer** — `SearchPayers` (`GET /payers/search` on `https://payers.us.stedi.com/2024-04-01`).
   Partial names and typos work (`cig` finds Cigna). Take `stediId` from the result and confirm the
   payer supports eligibility before using it.
2. **Run the check** — `EligibilityCheck` (`POST /change/medicalnetwork/eligibility/v3` on
   `https://healthcare.us.stedi.com/2024-04-01`). Send `tradingPartnerServiceId` (the payer id from
   step 1), `provider` (`organizationName` + `npi`), `subscriber` (name plus `memberId` or
   `dateOfBirth`), and optionally `encounter.serviceTypeCodes`.
   Prefer the JSON endpoint over `EligibilityRawX12Check` — Stedi states JSON gives the most
   reliable results for agents.

## Reading the response
- Active coverage, plan dates, patient responsibility and network status all arrive as X12-derived
  benefit structures, not as a boolean. Interpret with the STC/procedure-code lists Stedi publishes.
- A 200 can still carry an `error` alongside a partial result — check both.

## Errors
- `AAA` rejection codes ride inside the 271 body, not the HTTP status; a payer rejection is not an
  HTTP error. See `errors/stedi-problem-types.yml` and the eligibility troubleshooting guide.
- `503` on eligibility usually means payer-side connectivity — Stedi's guidance is to retry immediately.
- `429` means the per-second pool OR the concurrency limit was hit; there are no `X-RateLimit-*`
  headers to read, so back off blind. See `rate-limits/stedi-rate-limits.yml`.
- CMS eligibility requests are blocked when any IP in the `X-Forwarded-For` chain is outside the US.

## Notes
- Idempotency keys are NOT supported here — they apply only to the six claim submission endpoints.
- The MCP tool `eligibility_check` wraps exactly this operation; see `mcp/stedi-tool-crosswalk.yml`.

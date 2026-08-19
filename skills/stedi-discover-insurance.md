---
generated: '2026-08-15'
method: generated
name: Discover a patient's insurance coverage
description: Start an insurance discovery check for a patient with unknown coverage, then poll until the 270/271 search completes.
api: openapi/stedi-insurance-discovery-api-openapi.yml
operations: [InsuranceDiscoveryCheck, GetInsuranceDiscoveryCheck]
source: >-
  Grounded in arazzo/stedi-insurance-discovery-workflow.yml; operationIds verified verbatim in
  openapi/stedi-insurance-discovery-api-openapi.yml.
---

# Discover a patient's insurance coverage

For the patient who cannot produce a card. Stedi runs eligibility checks across payers on your
behalf and returns the coverage it finds. This is asynchronous — start, then poll.

## Auth
- `Authorization: <STEDI_API_KEY>`. See `authentication/stedi-authentication.yml`.

## Steps
1. **Start the search** — `InsuranceDiscoveryCheck` (`POST /insurance-discovery/check/v1` on
   `https://healthcare.us.stedi.com/2024-04-01`). Send the provider and the patient demographics you
   do have (name, date of birth, and where available SSN or address). Capture `discoveryId`.
2. **Poll** — `GetInsuranceDiscoveryCheck` (`GET /insurance-discovery/check/v1/{discoveryId}`) until
   the check reaches a terminal state. Back off between polls; the concurrency limit on insurance
   discovery is documented as low as 5 in-flight requests.

## Errors
- `429` here is most often the concurrency limit, not the per-second rate. Reduce parallelism rather
  than adding retries. See `rate-limits/stedi-rate-limits.yml`.
- Errors follow `{error, message}`; see `errors/stedi-problem-types.yml`.

## Notes
- Insurance discovery is NOT available in test mode — there is no mock fixture for it. See
  `sandbox/stedi-sandbox.yml`.
- Discovery is billed per check; confirm the metered price before running it in a loop. See
  `plans/stedi-plans-pricing.yml`.

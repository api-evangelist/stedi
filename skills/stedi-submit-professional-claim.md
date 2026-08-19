---
generated: '2026-08-15'
method: generated
name: Submit a professional claim and collect the remittance
description: Submit an X12 837P claim, poll its 277 status, and retrieve the converted 835 remittance advice.
api: openapi/stedi-claim-submission-api-openapi.yml
operations: [ClaimsSubmission, ClaimStatus, ConvertReport277, ConvertReport835]
source: >-
  Grounded in arazzo/stedi-professional-claim-lifecycle-workflow.yml; operationIds verified
  verbatim in openapi/stedi-claim-submission-api-openapi.yml,
  openapi/stedi-real-time-claim-status-api-openapi.yml,
  openapi/stedi-claim-acknowledgments-api-openapi.yml and
  openapi/stedi-remittances-api-openapi.yml.
---

# Submit a professional claim and collect the remittance

The 837 -> 277CA -> 835 lifecycle. This is the one flow in Stedi where idempotency matters, because
a duplicate submission is a duplicate claim at the payer.

## Auth
- `Authorization: <STEDI_API_KEY>`. See `authentication/stedi-authentication.yml`.

## Idempotency (required discipline)
- Send `Idempotency-Key` on the submission. Any unique string works; Stedi suggests a UUID v4 or a
  value derived from the call, such as the provider control number.
- Window is 24 hours. Reusing the key with a different method/path/body returns `422`
  (`RequestChangedException`); reusing it while the first request is still in flight returns `409`
  with a `Retry-After` header. See `conventions/stedi-conventions.yml`.
- On `500` or `504`, retry with the SAME key. A timeout does not mean the claim was not submitted.

## Steps
1. **Submit** — `ClaimsSubmission` (`POST /change/medicalnetwork/professionalclaims/v3/submission` on
   `https://healthcare.us.stedi.com/2024-04-01`). Use `ClaimsRawX12Submission` only if you already
   generate X12 yourself. Capture the `transactionId`.
   Institutional and dental have their own operations: `InstitutionalClaimsSubmission`,
   `DentalClaimsSubmission`.
2. **Read the acknowledgment** — `ConvertReport277`
   (`GET /change/medicalnetwork/reports/v2/{transactionId}/277`). The 277CA tells you whether the
   payer accepted the claim; a clean 837 submission is not an accepted claim.
3. **Check status on demand** — `ClaimStatus` (`POST /change/medicalnetwork/claimstatus/v2`) for a
   real-time 276/277 inquiry once the claim is with the payer.
4. **Collect payment** — `ConvertReport835`
   (`GET /change/medicalnetwork/reports/v2/{transactionId}/835`) for the remittance advice, or
   `GetElectronicRemittanceAdvicePdf` for a human-readable PDF.

## Errors
- Stedi runs pre-submission CLAIM EDITS that reject syntactically valid claims: invalid or
  deactivated NPI, taxonomy outside the NUCC code set, CPT out of range, missing billing provider
  identifier, service dates outside the statement covers period. New edits ship continuously — see
  `changelog/stedi-changelog.yml` before blaming your payload.
- Envelope is `{error, message}`, never RFC 9457. See `errors/stedi-problem-types.yml`.

## Notes
- Claim submission is NOT available in test mode through the portal UI, and 275 attachments and
  276/277 status are not testable at all. See `sandbox/stedi-sandbox.yml`.
- No MCP tool covers claim submission — this flow is REST-only. See `mcp/stedi-tool-crosswalk.yml`.

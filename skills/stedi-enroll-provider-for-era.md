---
generated: '2026-08-15'
method: generated
name: Enroll a provider for ERA/EFT
description: Create a provider, open a transaction enrollment, upload the signed agreement, and track the enrollment to LIVE.
api: openapi/stedi-enrollments-api-openapi.yml
operations: [CreateProvider, CreateEnrollment, CreateEnrollmentDocumentUpload, GetEnrollment, UpdateTaskPost]
source: >-
  Grounded in arazzo/stedi-provider-enrollment-workflow.yml; operationIds verified verbatim in
  openapi/stedi-providers-api-openapi.yml, openapi/stedi-enrollments-api-openapi.yml,
  openapi/stedi-documents-api-openapi.yml and openapi/stedi-tasks-api-openapi.yml.
---

# Enroll a provider for ERA/EFT

Transaction enrollment is the slow, human-in-the-loop part of a clearinghouse integration: payers
approve on their own schedule and often demand a signed form. Treat it as a long-running process
driven by events, not by polling.

## Auth
- `Authorization: <STEDI_API_KEY>`. Base `https://enrollments.us.stedi.com/2024-09-01`.

## Steps
1. **Create the provider** — `CreateProvider` (`POST /providers`) with NPI, tax id and contacts.
   Capture `providerId`.
2. **Open the enrollment** — `CreateEnrollment` (`POST /enrollments`) naming the provider, the payer
   and the transaction type (835 ERA, EFT, 837 claims). Capture `enrollmentId`.
3. **Attach the signed agreement** — `CreateEnrollmentDocumentUpload`
   (`POST /enrollments/{enrollmentId}/documents`) returns a pre-signed `uploadUrl` that expires
   after 24 hours; PUT the PDF to that URL. Retrieve with `CreateEnrollmentDocumentDownload`.
4. **Answer tasks** — when Stedi or the payer needs something, an `EnrollmentTask` appears. Respond
   with `UpdateTaskPost` (`POST /tasks/{taskId}`).
5. **Track status** — `GetEnrollment` (`GET /enrollments/{enrollmentId}`), or better, subscribe to
   events (next section) rather than polling for days.

## Events (preferred over polling)
Register an event destination and subscribe to `enrollment.activated`, `enrollment.rejected`,
`enrollment.updated`, `enrollment.task.assigned`, `enrollment.task.completed` and
`enrollment.task.deleted`. Payloads are THIN — re-read the enrollment via `GetEnrollment` using
`resource.id`. See `asyncapi/stedi-event-destinations-asyncapi.yml`.

## Errors
- Errors follow `{error, message}`; see `errors/stedi-problem-types.yml`.
- Transaction enrollment carries its own 100 req/s pool. See `rate-limits/stedi-rate-limits.yml`.

## Notes
- Transaction enrollment is NOT available in test mode. There is no way to rehearse this flow
  without a production account. See `sandbox/stedi-sandbox.yml`.

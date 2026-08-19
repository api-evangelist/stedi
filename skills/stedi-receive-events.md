---
generated: '2026-08-15'
method: generated
name: Receive and verify Stedi events
description: Register an event destination, verify Standard Webhooks signatures, deduplicate deliveries, and replay failures.
api: openapi/stedi-event-destinations-api-openapi.yml
operations: [EventDestinationsCreateDestination, EventDestinationsGetDestinationSecret, EventDestinationsRotateDestinationSecret,
  ListEvents, GetEvent, RetryEvent]
source: >-
  Grounded in openapi/stedi-event-destinations-api-openapi.yml and
  openapi/stedi-events-api-openapi.yml; delivery, signing and retry rules read from
  https://www.stedi.com/docs/healthcare/event-destinations-message-handling.
---

# Receive and verify Stedi events

Stedi's events are THIN: they tell you something changed and hand you a resource reference. The
payload is never the state — always re-read the resource.

## Auth
- `Authorization: <STEDI_API_KEY>`. Base `https://events.us.stedi.com/2026-02-01`.

## Steps
1. **Register the destination** — `EventDestinationsCreateDestination` (`POST /destinations`) with
   `name`, an https-only `destinationUrl`, and `eventTypes[]`. Optional `concurrencyLimit` defaults
   to 5 and cannot exceed the account maximum (typically 20) — a higher value returns `400`.
2. **Fetch the signing secret** — `EventDestinationsGetDestinationSecret`
   (`GET /destinations/{destinationId}/secret`). Format `whsec_...`. Rotate with
   `EventDestinationsRotateDestinationSecret`; during rotation the previous secret stays valid until
   `previousSecretExpiresAt`, so verify against both.
3. **Verify every delivery** — HMAC-SHA256 over `"{webhook-timestamp}.{raw body}"` keyed with the
   secret, compared in constant time against `webhook-signature` (`v1,{signature}`). Reject
   timestamps outside your tolerance window. Standard Webhooks spec.
4. **Deduplicate** — key on the `webhook-id` header (`msg_{UUID}`). Automatic and manual retries can
   redeliver the same event.
5. **Re-read state** — use `resource.id` / `resource.type` from the payload against the owning API
   (for example `GetEnrollment`). Do not assume event ordering.
6. **Replay** — `ListEvents` / `GetEvent` to inspect, `RetryEvent`
   (`POST /events/{eventId}/retry`) to redeliver.

## Delivery contract
- Only `2xx` counts as success. 3xx, 4xx, 5xx and network errors all retry.
- Retry schedule: 5m, 30m, 2h, 8h, 24h, 48h. After 48 hours of failures Stedi DISABLES the
  destination — you must re-enable it. Failure notifications fire at 2h and 24h.
- Event data is retained 30 days by default.

## Errors
- Errors follow `{error, message}`; see `errors/stedi-problem-types.yml`.

## Notes
- The published catalog covers transaction enrollment only, plus `event.ping`. There are no
  eligibility, claim or remittance events on this surface — poll the REST APIs for those.
  See `asyncapi/stedi-event-destinations-asyncapi.yml`.

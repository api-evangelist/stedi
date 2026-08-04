# Stedi (stedi)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Stedi is the only API-first programmable healthcare clearinghouse, enabling health tech companies to submit claims, verify eligibility, and process electronic remittance advice (ERA) through a modern JSON API. The platform supports real-time X12 EDI transaction processing including eligibility checks (270/271), professional and institutional claim submissions (837), claim status inquiries (276/277), and electronic remittance advice (835). Stedi provides both SFTP and REST API access, webhooks for event-driven workflows, a sandbox test environment, and an MCP server for AI-assisted integration. Public OpenAPI specifications are available for all core APIs via the Stedi GitHub organization, and pricing is purely metered with no monthly minimums or setup fees.

APIs.json: https://raw.githubusercontent.com/api-evangelist/stedi/refs/heads/main/apis.yml

Naftiko: https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=stedi-api-evangelist&utm_content=repo

## Tags

- EDI
- Electronic Data Interchange
- Healthcare
- Clearinghouse
- X12
- Claims
- Eligibility
- HIPAA
- Revenue Cycle Management
- B2B Integration

## APIs

| Name | Description | Docs | OpenAPI |
|------|-------------|------|---------|
| Stedi Core API | Core EDI platform API for outbound transactions, interchange management, file executions, fragment staging, mapping invocations, and event retries. | [Docs](https://www.stedi.com/docs/edi-platform/api-reference) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/core.json) |
| Stedi Healthcare Eligibility API | Real-time and batch eligibility verification supporting 270/271 transactions, coordination of benefits checks, insurance discovery, and MBI lookups. | [Docs](https://www.stedi.com/docs) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/healthcare.json) |
| Stedi Claims API | Claim submission supporting professional (837P), institutional (837I), and dental claim types, with acknowledgment, status, ERA (835), paper claims, and attachments. | [Docs](https://www.stedi.com/docs) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/claims.json) |
| Stedi Payers API | API for discovering and managing payer connections, including payer search, capabilities lookup, and transaction enrollment management. | [Docs](https://www.stedi.com/docs) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/payers.json) |
| Stedi Enrollment API | API-first transaction enrollment management for onboarding trading partners and managing provider-payer enrollment relationships. | [Docs](https://www.stedi.com/docs) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/enrollment.json) |
| Stedi Event Destinations API | API for configuring webhook and event destination endpoints with retry logic for EDI transaction event notifications. | [Docs](https://www.stedi.com/docs) | [OpenAPI](https://raw.githubusercontent.com/Stedi/openApi/main/event-destinations.json) |

## Plans, Rate Limits, and FinOps

| Resource | File |
|----------|------|
| Plans and Pricing | [plans/stedi-plans-pricing.yml](plans/stedi-plans-pricing.yml) |
| Rate Limits | [rate-limits/stedi-rate-limits.yml](rate-limits/stedi-rate-limits.yml) |
| FinOps | [finops/stedi-finops.yml](finops/stedi-finops.yml) |

**Pricing summary:** Sandbox is free. Pay as You Go has no monthly minimum — eligibility checks start at $0.30 each (volume discounts to $0.08), claim submissions start at $0.30 (discounts to $0.10), and ERAs start at $0.20 per adjudicated claim (discounts to $0.08). Enterprise Custom plans offer negotiated volume discounts with SSO/SCIM and a dedicated TAM.

## Timestamps

- created: 2026-06-12
- modified: 2026-06-12

## Common Properties

| Type | URL |
|------|-----|
| Website | https://www.stedi.com |
| Documentation | https://www.stedi.com/docs |
| API Reference | https://www.stedi.com/docs/edi-platform/api-reference |
| GitHub Organization | https://github.com/Stedi |
| OpenAPI Specs | https://github.com/Stedi/openApi |
| LinkedIn | https://www.linkedin.com/company/stedi-inc |
| X (Twitter) | https://x.com/stedi |
| Blog | https://www.stedi.com/blog |
| Pricing | https://www.stedi.com/pricing |
| Status Page | https://status.stedi.com |

## Maintainers

- Kin Lane / kin@apievangelist.com

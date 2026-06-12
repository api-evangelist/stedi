# Stedi (stedi)

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

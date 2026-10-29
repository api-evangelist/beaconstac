# Beaconstac

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Beaconstac (now Uniqode) is a B2B SaaS platform for creating, customizing, and tracking dynamic QR Codes and Digital Business Cards at scale, connecting physical touchpoints to measurable digital experiences for 50,000+ brands. Its REST API (v2.0) covers QR Codes and templates, bulk CSV generation, Digital Business Cards, leads, wallet passes, feedback forms, tags, places, media, organizations, users and webhooks, plus a separate RPC reporting endpoint for scan analytics; the contract is published as a 96-request Postman collection rather than an OpenAPI. REST auth is a dashboard-issued token in the Authorization header plus an Organization ID, gated to the Pro plan and above at 10-100 requests/second by tier. Uniqode also runs a hosted, OAuth-protected MCP server at mcp.uniqode.com for QR management and analytics from Claude. SOC 2 Type II, ISO 27001:2022, HIPAA and GDPR compliant. Formerly MobStac / Beaconstac; backed by Accel.

**Areas:** Company · Big Data · QR Codes · Digital Business Cards · Marketing · Analytics · Software-as-a-Service · Proximity · Webhook · MCP

## Beaconstac on APIs.io

Everything profiled for Beaconstac is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/beaconstac/](https://apis.io/providers/beaconstac/)** — or go straight to one of the APIs or properties below.

### APIs (1)

- [Uniqode QR Code API](https://apis.io/apis/beaconstac/uniqode-qr-code-api/) — REST API (v2.0) to programmatically create static and dynamic QR Codes across 20+ campaign types, apply design templates, generate codes in bulk from…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Beaconstac Webhooks](https://apis.io/asyncapis/beaconstac/beaconstac-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [uniqode](https://apis.io/servers/beaconstac/uniqode/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Beaconstac Authentication](https://apis.io/security/beaconstac/beaconstac-authentication/)
- [Beaconstac Domain Security](https://apis.io/security/beaconstac/beaconstac-domain-security/)
- [Beaconstac Trust Center](https://apis.io/security/beaconstac/beaconstac-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Beaconstac Scopes](https://apis.io/scopes/beaconstac/beaconstac-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Beaconstac Plans Pricing](https://apis.io/plans/beaconstac/beaconstac-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Beaconstac Rate Limits](https://apis.io/rate-limits/beaconstac/beaconstac-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.uniqode.com)
- [Documentation](https://apidocs.uniqode.com/)
- [Getting started](https://docs.uniqode.com/en/articles/6064771-getting-started-with-static-and-dynamic-qr-code-api)
- [API reference](https://apidocs.uniqode.com/)
- [Pricing](https://www.uniqode.com/pricing)
- [Sign up](https://dashboard.uniqode.com/signup)
- [Support](https://docs.uniqode.com/)
- [Blog](https://www.uniqode.com/blog)
- [Compliance](https://www.uniqode.com/security)
- [Terms of service](https://www.uniqode.com/terms-of-service)
- [Privacy policy](https://www.uniqode.com/privacy)
- [GitHub](https://github.com/mobstac)
- [Status](https://status.uniqode.com/)

## API Evangelist rating

**Kin Score 55.0** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 39.0 |
| Contract governance | 18.2 |
| Operational transparency | 73.7 |
| Developer ergonomics | 63.7 |
| Access clarity | 53.9 |

**Agent readiness 53.8** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 35.7** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/beaconstac/](https://apis.io/providers/beaconstac/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Beaconstac: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/beaconstac/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

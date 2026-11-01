# Sprift

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

Sprift is a United Kingdom property data aggregator, operated by Sprift Technologies Ltd and founded in 2016 by Matt Gilpin, that assembles up to 300 data points on more than 30 million UK residential properties from public and private sources — Royal Mail, Ordnance Survey, the Environment Agency, HM Land Registry, Ofcom, Historic England, Ofsted, Google Maps, the Valuation Office Agency and the ONS — and links every one of them to a UPRN, the UK's definitive property identifier. It sells that layer to estate and letting agents, surveyors, mortgage professionals, conveyancers and investors as shareable property dashboards, branded reports, Material Information packs, comparables, off-market prospecting and market intelligence. In a market with no MLS, Sprift occupies the aggregation seam: it does not own listings (Rightmove and Zoopla do) and it does not originate records (HM Land Registry and Ordnance Survey do), it enriches and resells the join between them. Its API posture is genuinely documented but commercially gated. A public, unauthenticated Swagger UI at sprift.com/dashboard/api-doc serves a real Swagger 2.0 contract (sprift.json, version 1.3.9, 27 paths, 76 definitions) for the v1 API at https://sprift.com/dashboard/api/v1, harvested verbatim here — that is the only machine-readable contract Sprift publishes. Every operation in it requires a SPRIFT-API-KEY header, and the API base returns HTTP 401 to anonymous callers. There is no self-serve signup anywhere: /dashboard/register and /dashboard/signup both return 404, pricing is not published, and Sprift's own knowledge base instructs prospective API users to email Customer Success with their company, use case and target systems for review, noting that access "may require an additional agreement depending on your subscription". A larger v2 API family is advertised on the Data and API product page with named endpoint paths, webhook alerts and bulk queries, but no contract for it is published and no host for it was confirmed. No RESO Web API or Data Dictionary certification, no OData service root or $metadata document, and no Universal Property Identifier appears anywhere in Sprift's surface — RESO is a North American, NAR-driven construct and the UK has no MLS to certify against. The UK's standards seam is instead the Open Property Data Association and its Property Data Trust Framework, of which Sprift claims founding and accredited membership. Sprift publishes no open data of its own; the open UK property layer belongs to HM Land Registry and Ordnance Survey, which are among its inputs.

**Areas:** Real Estate · United Kingdom · PropTech · Property Data · Property Listings · Valuation · AVM · Land Registry · Conveyancing · Rentals · Mortgage

## Sprift on APIs.io

Everything profiled for Sprift is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/sprift/](https://apis.io/providers/sprift/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Sprift Data and API (v2, advertised)](https://apis.io/apis/sprift/sprift-data-and-api-v2/) — The API family Sprift advertises on its Data and API product page, recorded here as advertised and NOT as verified. The page names ten REST endpoint paths —…
- [Sprift Home API](https://apis.io/apis/sprift/sprift-home-api/) — The Home API from Sprift — 1 operation(s) for home.
- [Sprift Insider API](https://apis.io/apis/sprift/sprift-insider-api/) — The Insider API from Sprift — 2 operation(s) for insider.
- [Sprift Property API](https://apis.io/apis/sprift/sprift-property-api/) — The Property API from Sprift — 10 operation(s) for property.
- [Sprift Property V2 API](https://apis.io/apis/sprift/sprift-property-v2-api/) — The Property V2 API from Sprift — 8 operation(s) for property v2.
- [Sprift Search API](https://apis.io/apis/sprift/sprift-search-api/) — The Search API from Sprift — 3 operation(s) for search.
- [Sprift Share API](https://apis.io/apis/sprift/sprift-share-api/) — The Share API from Sprift — 1 operation(s) for share.
- [Sprift User API](https://apis.io/apis/sprift/sprift-user-api/) — The User API from Sprift — 2 operation(s) for user.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (5)

Multi-step workflows that chain operations into a business task.

- [Sprift — enrich a CRM record with property data](https://apis.io/arazzos/sprift/sprift-crm-enrichment/)
- [Sprift — market activity for a postcode outcode](https://apis.io/arazzos/sprift/sprift-market-intelligence/)
- [Sprift — Material Information for a UK property](https://apis.io/arazzos/sprift/sprift-material-information/)
- [Sprift — address to shareable property report](https://apis.io/arazzos/sprift/sprift-property-report/)
- [Sprift — valuation evidence pack](https://apis.io/arazzos/sprift/sprift-valuation-evidence/)

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Sprift Webhooks](https://apis.io/asyncapis/sprift/sprift-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [sprift](https://apis.io/servers/sprift/sprift/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Sprift Agentic Access](https://apis.io/agentic-access/sprift/sprift-agentic-access/)

#### Collections (1)

Postman and Open Collection request collections.

- [Sprift API](https://apis.io/collections/sprift/open-sprift/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Sprift Openapi Overlay](https://apis.io/overlays/sprift/sprift-openapi-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Sprift Authentication](https://apis.io/security/sprift/sprift-authentication/)
- [Sprift Domain Security](https://apis.io/security/sprift/sprift-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://sprift.com/)
- [Documentation](https://sprift.com/data-and-api)
- [Getting started](https://sprift.com/data-and-api)
- [API reference](https://sprift.com/dashboard/api-doc/)
- [Pricing](https://sprift.com/pricing)
- [Support](https://sprift.com/academy)
- [Blog](https://sprift.com/blog)
- [Terms of service](https://sprift.com/terms-and-conditions)
- [Privacy policy](https://sprift.com/privacy-policy)
- [GitHub](https://github.com/sprift)

## API Evangelist rating

**Kin Score 38.6** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 60.7 |
| Contract quality | 49.8 |
| Contract governance | 4.5 |
| Operational transparency | 10.5 |
| Developer ergonomics | 49.4 |
| Access clarity | 38.2 |

**Agent readiness 23.2** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/sprift/](https://apis.io/providers/sprift/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Sprift: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/sprift/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

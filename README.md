# Sprift (sprift)

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

Sprift is a United Kingdom property data aggregator, operated by Sprift Technologies Ltd and founded in 2016 by Matt Gilpin, that assembles up to 300 data points on more than 30 million UK residential properties from public and private sources — Royal Mail, Ordnance Survey, the Environment Agency, HM Land Registry, Ofcom, Historic England, Ofsted, Google Maps, the Valuation Office Agency and the ONS — and links every one of them to a UPRN, the UK's definitive property identifier. It sells that layer to estate and letting agents, surveyors, mortgage professionals, conveyancers and investors as shareable property dashboards, branded reports, Material Information packs, comparables, off-market prospecting and market intelligence. In a market with no MLS, Sprift occupies the aggregation seam: it does not own listings (Rightmove and Zoopla do) and it does not originate records (HM Land Registry and Ordnance Survey do), it enriches and resells the join between them. Its API posture is genuinely documented but commercially gated. A public, unauthenticated Swagger UI at sprift.com/dashboard/api-doc serves a real Swagger 2.0 contract (sprift.json, version 1.3.9, 27 paths, 76 definitions) for the v1 API at https://sprift.com/dashboard/api/v1, harvested verbatim here — that is the only machine-readable contract Sprift publishes. Every operation in it requires a SPRIFT-API-KEY header, and the API base returns HTTP 401 to anonymous callers. There is no self-serve signup anywhere: /dashboard/register and /dashboard/signup both return 404, pricing is not published, and Sprift's own knowledge base instructs prospective API users to email Customer Success with their company, use case and target systems for review, noting that access "may require an additional agreement depending on your subscription". A larger v2 API family is advertised on the Data and API product page with named endpoint paths, webhook alerts and bulk queries, but no contract for it is published and no host for it was confirmed. No RESO Web API or Data Dictionary certification, no OData service root or $metadata document, and no Universal Property Identifier appears anywhere in Sprift's surface — RESO is a North American, NAR-driven construct and the UK has no MLS to certify against. The UK's standards seam is instead the Open Property Data Association and its Property Data Trust Framework, of which Sprift claims founding and accredited membership. Sprift publishes no open data of its own; the open UK property layer belongs to HM Land Registry and Ordnance Survey, which are among its inputs.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/sprift/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/sprift/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- PropTech
- Property Data
- Property Listings
- Valuation
- AVM
- Land Registry
- Conveyancing
- Rentals
- Mortgage

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### Sprift Property API

The Property tag of the published Sprift v1 contract — the largest family in the harvested Swagger document, with ten operations. POST /property/search generates a Sprift property report for a UPRN and is where the report types are enumerated in the contract itself (1 = Market Appraisal Report / Desktop Research Report, 2 = Appointment Confirmation, 3 = Key Facts For Buyers / Key Property Facts, 4 = Property Overview), with report pages selectable by comma separated names and an optional PDF flag. GET /property/{propertyID} returns the full property detail record. Sub-resources cover Material Information (GET /property/{uprn}/materialinformation, split in the schema into physical characteristics, financial and tenure, environmental and planning, planning applications and immediate proximity — the disclosure set Sprift markets to agents as Material Information), comparables (GET /property/{uprn}/{status}, described as "Comparables (Unified)"), recently sold, currently for sale and under offer, map, satellite and street view imagery, Tree Preservation Orders, listed buildings, and TV signal availability. Every operation takes a required SPRIFT-API-KEY header.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Real Estate
- United Kingdom
- Property Data
- Material Information
- Comparables

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)
- [Documentation](https://sprift.com/data-and-api)
- [Authentication](https://sprift.com/en/livechatacademy/api-key)

### Sprift Property V2 API

The Property V2 tag of the published Sprift v1 contract — eight operations that resolve and decompose a property into single-purpose reads rather than one large record. GET /property/{uprn}/search returns the raw property details and GET /property/{uprn}/propertyid resolves a UPRN to Sprift's internal property identifier, which the remaining calls take. From there: GET /property/{propertyID}/priceestimate (the only valuation surface in the published contract — an estimate on the property record, not a separately documented AVM product), /counciltax, /epc for Energy Performance Certificate detail, /school for nearby schools, /transport for nearby transport, and /streetviewmap for the Google Street View image. Note that this "V2" is a tag inside the v1 contract at basePath /dashboard/api/v1; it is not the /api/v2 family advertised on the Data and API product page.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Real Estate
- United Kingdom
- Valuation
- AVM
- EPC
- Property Data

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)
- [Documentation](https://sprift.com/data-and-api)

### Sprift Search API

The Search tag of the published Sprift v1 contract — three operations for finding a property before you can read it. GET /search takes a free-text phrase, GET /search/postcode/{postcode} returns addresses for a postcode (the UPRN-resolution path most integrations start from), and GET /search/myproperties returns the calling account's own saved properties and the reports generated against them. All three require the SPRIFT-API-KEY header and return HTTP 401 without it.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Search
- Real Estate
- United Kingdom
- Addressing
- UPRN

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)

### Sprift Insider API

The Insider tag of the published Sprift v1 contract — two operations exposing the market intelligence product that sits behind sprift.com/insider. GET /insider/{outcode} searches active properties by postcode outcode with an optional grouping parameter, and GET /insider/{outcode}/withdrawn returns withdrawn properties for the same outcode. This is the market-activity and agent-benchmarking surface — live stock, listings and withdrawals by area — rather than a single-property lookup.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Market Intelligence
- Real Estate
- United Kingdom
- Property Listings

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)
- [Documentation](https://sprift.com/insider)

### Sprift Report Share API

The Share tag of the published Sprift v1 contract — a single POST /share operation that produces a shareable link to a Sprift report for a given property ID and report type, with the same report type enumeration used by the property report call (1 = Market Appraisal Report, 2 = Appointment Confirmation, 3 = Key Facts for Buyers, 4 = Property Overview) plus optional report pages, an appointment date and a portal version. This is the API behind the shareable, white-labelled dashboards and reports that are Sprift's core agent-facing product.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Documents
- Reports
- Real Estate
- United Kingdom

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)
- [Documentation](https://sprift.com/property-reports)

### Sprift User API

The User tag of the published Sprift v1 contract — two operations, POST /user/login and GET /user/logout, that exist specifically for embedding. The contract is explicit that this is not the authentication path for API calls: "You do not need to call this Endpoint with your API username and password in order to use the API. This Endpoint is intended to those who want to add Sprift platform into their platforms using an iFrame and let Sprift users to login to Sprift." Login takes a username and password as form data alongside the required SPRIFT-API-KEY header, so a partner platform authenticates a Sprift end user on the partner's behalf.

- **Human URL:** [https://sprift.com/dashboard/api-doc/](https://sprift.com/dashboard/api-doc/)
- **Base URL:** `https://sprift.com/dashboard/api/v1`

#### Tags

- Authentication
- Embedding
- Real Estate
- United Kingdom

#### Properties

- [OpenAPI](openapi/sprift-openapi.json)
- [APIReference](https://sprift.com/dashboard/api-doc/)

### Sprift Data and API (v2, advertised)

The API family Sprift advertises on its Data and API product page, recorded here as advertised and NOT as verified. The page names ten REST endpoint paths — /api/v2/property/{uprn} (300 data point profile), /api/v2/comparables (whole-of-market sold evidence), /api/v2/prospect/search (off-market owners likely to sell), /api/v2/watch (Property Watch, described as real-time webhook alerts on listed, sold, price reduced, planning submitted and EPC updated), /api/v2/risk/{uprn} (flood, subsidence, radon, contaminated land, coastal erosion), /api/v2/planning/{uprn}, /api/v2/market/insight, /api/v2/epc/{uprn} and /api/v2/bulk (up to 500 properties per call) — plus six data products: Automated Valuation, Sprift Insights natural-language querying, custom datasets, bulk data feeds, Property Watch and a National Comparables Library. No machine-readable contract is published for any of them: the only spec Sprift serves is the v1 Swagger document, which contains none of these paths. No host was confirmed either — the advertised paths are given host-relative, https://sprift.com/api/v2/property/{uprn} returns HTTP 404, and https://api.sprift.com answers every path with HTTP 403 MissingAuthenticationTokenException from an AWS API Gateway, which proves an API gateway exists on that name but documents nothing. No baseURL is asserted here for that reason. The page states authentication is a Bearer token (sk_live_sprift_ prefix) generated from Settings then Developer then API Keys, which contradicts both the SPRIFT-API-KEY header in the published contract and Sprift's own knowledge base instruction to email Customer Success for access.

- **Human URL:** [https://sprift.com/data-and-api](https://sprift.com/data-and-api)

#### Tags

- Real Estate
- United Kingdom
- AVM
- Webhooks
- Bulk Data
- PropTech

#### Properties

- [Documentation](https://sprift.com/data-and-api)
- [Authentication](https://sprift.com/en/livechatacademy/api-key)
- [Contact](https://sprift.com/contact-us)

## Common Properties

- [Website](https://sprift.com/)
- [Documentation](https://sprift.com/data-and-api)
- [APIReference](https://sprift.com/dashboard/api-doc/)
- [OpenAPI](https://sprift.com/dashboard/api-doc/sprift.json)
- [Authentication](https://sprift.com/en/livechatacademy/api-key)
- [SignIn](https://sprift.com/dashboard/login)
- [Pricing](https://sprift.com/pricing)
- [Demo](https://sprift.com/book-demo-sprift)
- [Support](https://sprift.com/academy)
- [Partners](https://sprift.com/partnerships)
- [About](https://sprift.com/about-us)
- [Blog](https://sprift.com/blog)
- [Contact](https://sprift.com/contact-us)
- [PrivacyPolicy](https://sprift.com/privacy-policy)
- [GitHubOrganization](https://github.com/sprift)
- [LinkedIn](https://www.linkedin.com/company/sprift/)
- [Twitter](https://twitter.com/SpriftProperty/)
- [Facebook](https://www.facebook.com/SpriftProperty/)
- [Instagram](https://www.instagram.com/spriftproperty/)

## Maintainers

- **Kin Lane** — kin@apievangelist.com

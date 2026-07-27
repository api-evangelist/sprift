---
name: Generate and share a Sprift property report
description: >-
  Resolve a UK address to a UPRN, generate a Sprift property report (Market Appraisal,
  Key Facts for Buyers, Appointment Confirmation or Property Overview), and produce a
  shareable link to it.
api: openapi/sprift-openapi.json
operations:
  - SearchPropertiesByPostcode
  - SpriftPropertyReport
  - ShareReport
generated: '2026-07-26'
method: generated
source: openapi/sprift-openapi.json
---

# Generate and share a Sprift property report

This is Sprift's marquee flow: turn an address into a branded, shareable property
report. Every step below maps to a real operation in the published Sprift v1 contract.

## Before you start

- **Auth.** Every call requires the header `SPRIFT-API-KEY: <key>`. There is no OAuth
  and no bearer token on the contracted v1 surface. Keys are issued by Sprift Customer
  Success (customer.success@sprift.com) to existing subscribers — there is no
  self-serve signup.
- **Base URL.** `https://sprift.com/dashboard/api/v1`
- **A missing or invalid key returns HTTP 401** with body
  `{"status":false,"error":"Unauthorized"}` — not the 403 the contract documents.
- **There is no idempotency key on this API.** Both write steps below can duplicate on
  retry. Do not blind-retry a timed-out report or share call; re-read with
  `SearchMyProperties` first to see whether the report already exists.

## Steps

1. **Resolve the address to a UPRN** — `SearchPropertiesByPostcode`
   `GET /search/postcode/{postcode}`
   Path parameter: `postcode`. The response carries `total` (with `LPI` and `DPA`
   counts — the two Ordnance Survey AddressBase address forms) and a `data` array whose
   entries include `match`, `address`, `value`, `label`, `lat`, `lng`, `authority`,
   `local_authority` and `classification_code`. Pick the entry whose `address` matches
   the property you want and take its UPRN.
   If you only have a free-text description rather than a postcode, use
   `SearchProperties` (`GET /search?phrase=...`) instead — `phrase` is required.

2. **Generate the report** — `SpriftPropertyReport`
   `POST /property/search`
   Form fields: `uprn` (required), `reportType` (required integer), and optionally
   `reportPage` (comma-separated page names) and `pdf` (boolean).
   Report types, taken from the contract itself:
   - `1` — Market Appraisal Report / Desktop Research Report
   - `2` — Appointment Confirmation
   - `3` — Key Facts For Buyers / Key Property Facts
   - `4` — Property Overview
   The response is the full report object: `propertyDetails`, `floodRisk`,
   `coastal_erosion`, `estimatedPrice`, `rentalEstimate`, `yield`, `landRegistry`,
   `propertyValue`, `poundPerSquareFoot`, `interior`, `councilTax`, plus EPC, schools,
   transport, broadband, mobile coverage, images and the property polygon. Keep the
   `property_id` from `propertyDetails` — the share step needs it, not the UPRN.

3. **Create the shareable link** — `ShareReport`
   `POST /share`
   Form fields: `propertyID` (required integer — the `property_id` from step 2), and
   `reportType` (required). Optional: `reportPage`, `appointment-date` (use with
   report type 2), `portal_version`.
   The response is `{"status": true, "share_link": "..."}`. That link is the
   white-labelled report page an agent sends to a client.

## Rules

- Do not pass a UPRN where the contract asks for `propertyID`. They are different
  identifiers. If you have only a UPRN, call `Property-ID`
  (`GET /property/{uprn}/propertyid`) to convert it.
- The contract declares `consumes: application/json` globally, but both write
  operations take `formData` parameters — send
  `application/x-www-form-urlencoded`.
- Handle 401 as the real auth failure. The documented 403 was not observed live.
- `POST /property/search` documents only a 200 response. Treat any non-200 as
  undocumented and surface the raw `{status, error}` body to the caller rather than
  interpreting it.

## Related artifacts

- `authentication/sprift-authentication.yml` — the three inconsistent auth statements
  Sprift publishes and which one actually works.
- `conventions/sprift-conventions.yml` — pagination, error envelope, absence of
  idempotency.
- `errors/sprift-problem-types.yml` — every documented and observed error.
- `arazzo/sprift-property-report.yml` — the same flow as an executable workflow.

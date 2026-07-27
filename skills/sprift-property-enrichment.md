---
name: Enrich a CRM record with Sprift property data
description: >-
  Take an address already held in a CRM or case-management system, resolve it to a
  UPRN and Sprift property ID, and enrich the record with EPC, council tax, schools,
  transport, TV availability and imagery.
api: openapi/sprift-openapi.json
operations:
  - SearchPropertiesByPostcode
  - Property-ID
  - Property-EPC
  - Property-Council-Tax
  - Property-Nearby-School
  - Property-Nearby-Transport
  - PropertyDetails-Tv
  - PropertyDetails-Maps
generated: '2026-07-26'
method: generated
source: openapi/sprift-openapi.json
---

# Enrich a CRM record with Sprift property data

The integration Sprift markets to CRM, conveyancing and surveying platforms: pull the
full property profile into a record you already hold. Every call is a read, so this
whole flow is safe to run on a schedule.

## Before you start

- Header `SPRIFT-API-KEY: <key>`. Base URL `https://sprift.com/dashboard/api/v1`.
- Rate limits exist but are **not quantified** anywhere public — Sprift's help centre
  says only "API requests are subject to rate limits to ensure fair usage". Throttle
  conservatively when enriching in bulk, and do not assume a documented batch endpoint
  exists: `/api/v2/bulk` is advertised but uncontracted.

## Steps

1. **Address → UPRN** — `SearchPropertiesByPostcode`
   `GET /search/postcode/{postcode}`. Match on the `address` / `label` fields in
   `data[]`. Store the UPRN on the CRM record — it is the stable key and it survives
   address-format differences.

2. **UPRN → property ID** — `Property-ID`
   `GET /property/{uprn}/propertyid`. Store `property_id` alongside the UPRN; the
   enrichment reads below need it.

3. **Enrich** (each is an independent GET on `{propertyID}`):
   - `Property-EPC` — `GET /property/{propertyID}/epc` → `epc_current_energy_rating`,
     `epc_potential_energy_rating`, `epc_current_energy_efficiency`,
     `epc_total_floor_area`, `epc_certificate_number`, `epc_update_date`.
   - `Property-Council-Tax` — `GET /property/{propertyID}/counciltax` →
     `council_tax_band`, `council_tax_monthly_cost`, `council_tax_annual_cost`,
     `council_tax_update_date`.
   - `Property-Nearby-School` — `GET /property/{propertyID}/school` → `name`,
     `ofsted_rating`, `pupils`, `distance`, `school_type`.
   - `Property-Nearby-Transport` — `GET /property/{propertyID}/transport` → grouped as
     `Local_Connections`, `National_Rail_Stations`, `Ferry_Terminals`,
     `Bus_Stops_Stations`, `Trunk_Roads_Motorways`, `Airports_Helipads`, each with
     `name`, `latitude`, `longitude`, `distance`, `metres`, `distanceInMiles`.
   - `PropertyDetails-Tv` — `GET /property/{uprn}/tv` → `bt`, `sky`, `virgin`.
     **Note this one takes the UPRN, not the property ID.**
   - `PropertyDetails-Maps` — `GET /property/{propertyID}/maps` →
     `propertyMapWithPolygon`, `propertySatelliteImage`, `propertyStreetViewImage`.

4. **Or take it in one call.** If you want the whole profile rather than the parts,
   `PropertyDetails` (`GET /property/{propertyID}`) returns the 54-field
   `propertyDetails` object plus `floodRisk`, `coastal_erosion`, `estimatedPrice`,
   `rentalEstimate`, `yield`, `landRegistry`, `propertyValue`, `poundPerSquareFoot`,
   `interior`, `councilTax`, EPC, schools, transport, broadband speeds, mobile coverage
   and the property polygon. Use the single call when you are refreshing everything,
   the parts when you are refreshing one field.

## Rules

- **Watch which identifier each operation takes.** `PropertyDetails-Tv`,
  `Property-RawData`, `PropertyDetails-MaterialInformation` and
  `PropertyDetails-Comparables` take the **UPRN**; the rest of the enrichment family
  takes the **propertyID**. Passing the wrong one is the most common integration error
  against this API.
- Record the `*_update_date` fields (`epc_update_date`, `council_tax_update_date`)
  with the values. Sprift is an aggregator; freshness varies by source.
- There is no change-notification surface you can rely on. Property Watch webhooks are
  advertised but uncontracted (see `asyncapi/sprift-webhooks.yml`), so schedule
  re-reads rather than waiting for events.
- Attribute the underlying sources — MHCLG for EPC, the Valuation Office Agency for
  council tax, Ofsted for school ratings, Ofcom for broadband and mobile coverage.

## Related artifacts

- `conventions/sprift-conventions.yml` — rate-limit posture, error envelope.
- `data-model/sprift-data-model.yml` — which entity hangs off which identifier.
- `lifecycle/sprift-lifecycle.yml` — no changelog and no status page, so pin nothing
  to an announced schedule.

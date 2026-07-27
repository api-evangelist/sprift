---
name: Pull Material Information for a UK property
description: >-
  Resolve an address to a UPRN and retrieve the Sprift Material Information disclosure
  set — physical characteristics, financial and tenure, environmental and planning,
  planning applications and immediate proximity.
api: openapi/sprift-openapi.json
operations:
  - SearchPropertiesByPostcode
  - PropertyDetails-MaterialInformation
generated: '2026-07-26'
method: generated
source: openapi/sprift-openapi.json
---

# Pull Material Information for a UK property

Material Information is the disclosure set UK estate agents are expected to provide to
prospective buyers and renters. Sprift assembles it per property and exposes it on one
operation keyed by UPRN.

## Before you start

- Header `SPRIFT-API-KEY: <key>` on every call. Base URL
  `https://sprift.com/dashboard/api/v1`.
- This flow is entirely read-only. It is safe for an agent to retry.

## Steps

1. **Resolve the address to a UPRN** — `SearchPropertiesByPostcode`
   `GET /search/postcode/{postcode}` and select the matching entry from `data`.

2. **Fetch Material Information** — `PropertyDetails-MaterialInformation`
   `GET /property/{uprn}/materialinformation`
   Path parameter: `uprn`. The response is keyed `uprn` plus three lettered parts,
   named that way in Sprift's own schema:
   - **A — physical characteristics**: `property_description`, `construction_type`,
     `radon_risk`, `mainheat_description`, `property_type_basic`, `plot_size`,
     `total_floor_area_sqft`, `floor_level`, `above_commercial_premises`,
     `non_standard_construction_flag`, `parking_description`, `cost_to_park`,
     floorplan and property image filenames.
   - **B — financial and tenure**: `rent_frequency`, `price_qualifier`, `tenure_l_f`,
     `lease_term_remaining`, `counciltax`, `valuation_filter`, `price_per_sqft`,
     `ground_rent`, `ground_rent_review_period`, `service_charge`,
     `security_deposit`.
   - **C — environmental and planning**: 49 fields including
     `coastal_erosion_flag_all`, `listed_building_indicator`,
     `restrictive_covenant_indicator`, `conservation_area`, `aonb`, `greenbelt`,
     `pylon_proximity`, `common_land`, `national_park`, and the graduated surface
     water depth flags (`surface_water_current_depth_0` through
     `surface_water_current_depth_0_9_m`).
   Planning applications arrive as `application_number`, `proposal`, `status`, `date`,
   split between the property itself and its `immediate_proximity`.

## Rules

- This operation takes a **UPRN**, not Sprift's internal `propertyID`. Do not convert.
- Non-standard construction, restrictive covenants, listed-building status and flood
  depth flags are the fields that change a transaction. Surface them explicitly rather
  than summarising the object.
- Sprift is an aggregator: these values originate with HM Land Registry, the
  Environment Agency, Historic England, MHCLG and local authorities. Attribute
  accordingly and do not present them as Sprift's own determinations.
- The operation documents only a 200 response. A missing or invalid key returns 401
  `{"status":false,"error":"Unauthorized"}`.

## Related artifacts

- `data-model/sprift-data-model.yml` — the MaterialInformation entity and its parts.
- `conformance/sprift-conformance.yml` — Sprift's OPDA membership, and the fact that
  the contract implements no Property Data Trust Framework schema.

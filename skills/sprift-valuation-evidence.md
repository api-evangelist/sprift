---
name: Assemble valuation evidence for a UK property
description: >-
  Resolve a UPRN to Sprift's internal property ID, then pull the price estimate,
  unified comparables, recently sold properties and currently for-sale stock that
  support a defensible valuation.
api: openapi/sprift-openapi.json
operations:
  - Property-ID
  - Property-Price-Estimate
  - PropertyDetails-Comparables
  - PropertyDetails-RS
  - PropertyDetails-CFS-UO
generated: '2026-07-26'
method: generated
source: openapi/sprift-openapi.json
---

# Assemble valuation evidence for a UK property

Sprift's valuation surface in the published contract is one estimate plus three
evidence sets. This skill assembles all four.

## Before you start

- Header `SPRIFT-API-KEY: <key>`. Base URL `https://sprift.com/dashboard/api/v1`.
- Read-only throughout; safe to retry.
- **The identifier hop is mandatory.** Four of the five operations below key on
  Sprift's internal integer `propertyID`, not the UPRN.

## Steps

1. **Resolve UPRN to property ID** — `Property-ID`
   `GET /property/{uprn}/propertyid` → `{"status": true, "property_id": ...}`.
   Every subsequent call in this skill uses that `property_id`, except the comparables
   call, which takes the UPRN.

2. **Get the estimate** — `Property-Price-Estimate`
   `GET /property/{propertyID}/priceestimate` → `estimatedPrice`, `rentalEstimate`,
   `yield`. This is the only valuation surface in the published contract. The AVM with
   a confidence interval that Sprift advertises on its Data and API page is **not**
   contracted and is not reachable here — do not claim a confidence interval you did
   not receive.

3. **Get unified comparables** — `PropertyDetails-Comparables`
   `GET /property/{uprn}/{status}`
   Takes the **UPRN** plus a `status` path segment. Optional query filters: `bedroom`,
   `type`, `price1`, `price2`, `searchDate`, `searchDistance`, `sort`, `limit`, `page`.
   Each comparable carries `uprn`, `full_address`, `postcode`, `price`, `type`, `beds`,
   `tenure`, `listing_status`, `listing_date`, `withdrawn_date`, `sstc_date`,
   `let_agreed_date` and `time_on_market`.
   **The `status` values are not enumerated in the contract.** An unrecognised value
   returns HTTP 400 "Unknown comparable type". Obtain the valid set from Sprift support
   and cache it; do not guess in a loop.

4. **Get recently sold** — `PropertyDetails-RS`
   `GET /property/{propertyID}/recentlysold` → `sold_properties[]` with `property_id`,
   `tenure`, `full_address`, `price`, `date_sold`, `type`, `beds`, `postcode`,
   `distanceTo`, `sqm`, `sqf`, `ppsf`. `ppsf` is the field that makes cross-property
   comparison defensible.

5. **Get currently for sale and under offer** — `PropertyDetails-CFS-UO`
   `GET /property/{propertyID}/currentlyforsale` → live stock with `propID`,
   `propertyTypeFullDescription`, `price`, `displayAddress`, `firstVisibleDate`,
   `propertySubType`, `distance`, `bedrooms`, `propertyUrl`, `displayStatus`,
   `transactionType`.

## Rules

- Sold evidence and asking-price evidence are different things. `PropertyDetails-RS`
  returns achieved prices; `PropertyDetails-CFS-UO` returns asking prices. Never blend
  them into one average.
- `distanceTo` and `time_on_market` are the two fields that qualify a comparable.
  Report them alongside every price you cite.
- The estimate in step 2 is an automated figure, not a valuation. Present it as
  Sprift's estimate, with the evidence from steps 3-5 beside it.
- Sprift also exposes `landRegistry` (with `last_sold_price`, `transfer_date`,
  `land_registry_value`, `duration_full`) inside the full property record from
  `PropertyDetails` if you need registry-sourced history.

## Related artifacts

- `data-model/sprift-data-model.yml` — Comparable, SoldProperty, ForSaleListing and
  PriceEstimate entities and the UPRN/propertyID split.
- `errors/sprift-problem-types.yml` — the 400 "Unknown comparable type" case.

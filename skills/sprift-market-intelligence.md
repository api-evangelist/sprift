---
name: Read UK market activity for a postcode area
description: >-
  Use the Sprift Insider surface to read active and withdrawn market stock for a UK
  postcode outcode — the agent-benchmarking and market-activity view.
api: openapi/sprift-openapi.json
operations:
  - InsiderActivePropertiesResult
  - InsiderWithdrawnPropertiesResult
generated: '2026-07-26'
method: generated
source: openapi/sprift-openapi.json
---

# Read UK market activity for a postcode area

Insider is Sprift's market-observation product. It is queried by **outcode** — the
outward half of a UK postcode, e.g. `SW1A` — not by property.

## Before you start

- Header `SPRIFT-API-KEY: <key>`. Base URL `https://sprift.com/dashboard/api/v1`.
- Read-only; safe to retry.
- These are the only two operations in the contract that page. Both accept `page` and
  `limit`.

## Steps

1. **Active market stock** — `InsiderActivePropertiesResult`
   `GET /insider/{outcode}`
   Path parameter: `outcode`. Optional query parameters: `group`, `bedroom`,
   `bungalow`, `commercial`, `newBuild`, `price1`, `price2`, `type`, `days`, `sort`,
   `page`, `limit`.
   Each item carries `id`, `file`, `checksum`, `rmid`, `uprn`, `confidence`,
   `postcode`, `outcode`, `scannedAt`, `submittedAt`, `status`, plus:
   - `details` — `address`, `price`, `agent`, `agent_id`, `type`, `status`, `beds`,
     `isNew`, `epc`, `stc`, `tenure`, `key_features`, `description`
   - `portals` — `rightmove`, `zoopla`, `boomin` sub-objects with `id`, `link`,
     `header`, `listed`
   - `point` — `latitude`, `longitude`
   - `images` — `image_key`, `image_url`, `thumb_url`

2. **Withdrawn stock** — `InsiderWithdrawnPropertiesResult`
   `GET /insider/{outcode}/withdrawn`
   Same shape, same filters. Withdrawals are the signal behind agent-switching and
   stale-stock analysis.

## Rules

- **`confidence` is not decoration.** Insider items are observed listings matched back
  to a UPRN; the match is probabilistic. Do not join an Insider item to a property
  record without checking `confidence`, and say so when you do.
- `agent` and `agent_id` make this an agent-benchmarking dataset. If you aggregate by
  agent, state the outcode and the observation window (`scannedAt`) you used.
- The `portals` object is Sprift's observation of Rightmove, Zoopla and Boomin
  listings. Sprift is not a portal and publishes no listing write path — never present
  this as a listing feed you can post to.
- Page explicitly. `has_next_page` appears on the saved-properties response, not here;
  iterate `page` until an empty `items` array.
- The market-insight endpoint Sprift advertises at `/api/v2/market/insight` is
  uncontracted and unreachable. Insider is the contracted market surface.

## Related artifacts

- `data-model/sprift-data-model.yml` — the InsiderListing entity.
- `conventions/sprift-conventions.yml` — pagination and filter semantics.

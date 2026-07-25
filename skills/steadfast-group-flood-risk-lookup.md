---
name: steadfast-flood-risk-lookup
description: >-
  Look up Swiss Re river-flood and coastal storm-surge risk for an Australian street address
  using Steadfast Group's public Flood Risk Tracker API. Use when someone asks how exposed an
  Australian property is to flooding, or needs a natural-catastrophe indication for an address
  before quoting or advising on home, strata or business property insurance.
api: openapi/steadfast-group-flood-risk-tracker-openapi.yml
operations:
  - findAddress
  - getFloodRisk
auth: none
generated: '2026-07-25'
method: generated
---

# Steadfast flood risk lookup

Two calls, no credentials. Resolve the address, then score it.

## Before you start

- **No authentication.** Both operations are anonymous GETs against
  `https://floodrisktracker.steadfast.com.au`. Do not attach an API key or token — none exists.
- **Australia only.** The address dataset is Australian G-NAF. A non-Australian address returns
  an empty list, not an error.
- **Undocumented surface.** Steadfast publishes no specification, no status page and no
  deprecation policy. Treat non-200 responses as transient, back off, and never present the
  result as a Steadfast-guaranteed contract.

## Step 1 — resolve the address (`findAddress`)

`GET /api/risk/find_address?searchText=<free text>`

- `searchText` is required. Omitting it returns `400 application/problem+json` with
  `errors.searchText`.
- The response is an **unbounded array** of candidates — there is no paging parameter. Cap it
  yourself; the Steadfast web tool shows the first five.
- Each candidate carries `id` (the G-NAF identifier, e.g. `AU|GNAF|GANSW705226154`) and
  `fullAddress`. `latitude` and `longitude` are null in practice — do not rely on them.
- An empty array means **no match**, not zero risk. Say so plainly and ask the user to refine the
  address rather than reporting a risk result.

If more than one candidate is plausible, show `fullAddress` values and let the human choose.
Never guess between two addresses — flood risk differs street by street.

## Step 2 — score the address (`getFloodRisk`)

`GET /api/risk/get_flood_risk?addressId=<id from step 1>`

- `addressId` is required and must be a value from step 1. The identifier contains **pipe
  characters that must be URL-encoded** (`%7C`).
- The response is an array of hazard layers. Two are returned:
  - `FL_Fluvial_SwissRe` — river / fluvial flood
  - `FL_Surge_SwissRe` — coastal storm surge
- Read `riskDescription` (e.g. `Low`, `High`) for the band and `riskIndex` (integer) for the
  ordinal. `valueLabel` gives the hazard value — `Outside` means outside the modelled hazard
  zone; a return-period label such as `50 years` means inside it.
- `details` is **JSON double-encoded as a string**. Parse it a second time to get the Swiss Re
  model attributes (`FF Return Period (SR)`, `FF Protection (SR)`, `FF Frequency (SR)`,
  `Surge Return Period`).
- An empty array means the identifier did not resolve — again, not zero risk.

## Error and retry rules

- Validation failures return RFC 9457 `application/problem+json`: read `errors` for the offending
  parameter and `traceId` if you need to report the failure.
- Unsupported methods return `404`, not `405`. Use `GET` only.
- Both operations are safe and idempotent — retrying is harmless. There is no idempotency key
  because there are no write operations. No rate limits are published; throttle conservatively
  and back off on anything that is not a 200.

## How to report the result

State the two layers separately, name Swiss Re as the hazard source and G-NAF as the address
source, and include the resolved `fullAddress` so the human can confirm you scored the right
property.

Be careful about what this is not: it is a hazard indication published for a consumer awareness
tool. It is **not** an underwriting decision, a premium, a quote, or personal financial advice,
and Steadfast makes no accuracy commitment. Point people who need cover to a Steadfast network
broker at <https://www.steadfast.com.au/find-an-insurance-broker>.

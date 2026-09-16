---
name: unirateapi-historical-timeseries
description: Retrieve a historical exchange-rate time series across a date range (UniRate Pro).
api: UniRate API
operations:
  - getHistoricalLimits
  - getTimeSeries
  - getHistoricalRates
---

# Historical rates and time series with UniRate (Pro)

Pull historical FX rates for a single date or a daily series across a range. **These endpoints
require a Pro subscription** — a free-tier key gets HTTP 403.

## Steps

1. (Optional) Check coverage with `getHistoricalLimits` —
   `GET https://api.unirateapi.com/api/historical/limits?api_key=YOUR_KEY`. The response gives
   `earliest_date`/`latest_date` per currency; fiat pairs go back to 1999-01-04.
2. For one date, call `getHistoricalRates` —
   `GET /api/historical/rates?api_key=YOUR_KEY&date=2020-03-15&from=USD&to=EUR`. `date` is required
   (`YYYY-MM-DD`). The response shape depends on `to`/`amount`: `rate`, `result`, `rates`, or `results`.
3. For a range, call `getTimeSeries` —
   `GET /api/historical/timeseries?api_key=YOUR_KEY&start_date=2024-01-01&end_date=2024-01-07&base=USD&currencies=EUR,GBP`.
   `start_date` and `end_date` are required; the span may not exceed 5 years. Read `data`, a map of
   date -> (currency -> rate).

## Rules

- Always send `Accept: application/json`.
- A 403 means the key is not Pro — surface the upgrade path (https://unirateapi.com), do not retry.
- Time-series `data` values are numbers; single-rate `rate`/`result` values are decimal STRINGS.
- Historical requests have their own Pro cap (1,000/day). 429 = exhausted, reset midnight UTC.

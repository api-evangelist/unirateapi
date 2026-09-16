---
name: unirateapi-convert-currency
description: Convert an amount between two currencies at the current UniRate exchange rate.
api: UniRate API
operations:
  - convert
  - getCurrencies
---

# Convert currency with UniRate

Convert an `amount` from one currency to another at the latest rate.

## Steps

1. (Optional) Validate the currency codes by calling `getCurrencies`
   — `GET https://api.unirateapi.com/api/currencies?api_key=YOUR_KEY` with header
   `Accept: application/json`. Confirm both codes appear in the returned `currencies` array.
2. Call `convert` — `GET https://api.unirateapi.com/api/convert?api_key=YOUR_KEY&from=USD&to=EUR&amount=100`.
   `from`, `to`, and `amount` are query parameters; `to` is required for a single conversion.
3. Read `result` from the JSON body. **It is a decimal STRING** (e.g. `"92.50"`) — parse it to a
   decimal before doing arithmetic.

## Rules

- Always send `Accept: application/json`.
- Currency codes are ISO 4217 alpha-3, conventionally uppercased.
- Errors are `{"error":"..."}`: 401 = bad/missing key, 404 = unknown currency, 429 = rate
  limit (reset midnight UTC), 503 = transient (retry with backoff).
- Free tier: 200 requests/day. No idempotency concern — this is a read-only GET.

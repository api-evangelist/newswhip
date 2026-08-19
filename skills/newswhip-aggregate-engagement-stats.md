---
name: Aggregate engagement statistics for a topic
description: Use the NewsWhip Historical API to compute aggregated engagement statistics for a query — totals broken down by publisher, author, topic, language or country — instead of pulling individual articles.
api: openapi/newswhip-stats-api-openapi.yml
operations: [stats]
generated: '2026-08-13'
method: generated
source: openapi/newswhip-stats-api-openapi.yml + https://developer.newswhip.com/docs/api-features
---

# Aggregate engagement statistics

Use this when the question is "how much engagement did this topic get, and who drove it" rather than "which articles matched". It is one call instead of paging thousands of articles, and it is the endpoint NewsWhip points data-science users at.

## Steps

1. **Authenticate.** Pass your key as the `key` query parameter on the request URL — `POST https://api.newswhip.com/v1/stats?key=YOUR_API_KEY`. There is no header form. See `authentication/newswhip-authentication.yml`.
2. **Confirm entitlement.** `/stats` is a Historical API endpoint. A key entitled only to the Quick Start tier returns `403`. See `plans/newswhip-plans-pricing.yml`.
3. **Build the query body.** Send a JSON body with:
   - `filters` — an array of Lucene query strings (see https://developer.newswhip.com/docs/lucene-query-strings). Escape reserved `"` characters; unescaped quotes are the documented cause of a `400`.
   - `from` / `to` — epoch milliseconds bounding the window. The historical floor is 2018.
   - the aggregation dimension you want (publisher, author, topic, language, country) as documented per-field in the API reference.
   - `size` — capped at 5,000 on the Historical API.
4. **Call `POST /stats`** (operationId `stats`).
5. **Read the aggregates.** Facebook figures now include `likes`, `comments` and `shares` alongside `total_engagement_count` as of release 1.9.0.
6. **Handle failure.** `400` means the body could not be parsed — the response text says why. `403` means the key is missing, invalid or not entitled. `429` means you exceeded 100 requests per rolling 5 minutes or 5 per second; back off, there is no `Retry-After` header to read. See `errors/newswhip-problem-types.yml` and `rate-limits/newswhip-rate-limits.yml`.

## Notes

- `/stats` responses are **never cached** server-side (unlike Quick Start responses, which are cached ~3 minutes per key), so every call spends rate-limit budget.
- Retries are always safe: `POST /stats` is a query, not a write. There is no idempotency key and none is needed.
- If you are about to issue several near-identical queries, collapse them into one with Lucene `OR` — NewsWhip's own published rate-limit guidance.

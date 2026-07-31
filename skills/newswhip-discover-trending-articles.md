---
name: Discover trending articles by keyword
description: Use the NewsWhip API to find real-time and predicted top-engaging articles matching a topic, keyword, or Lucene query, across regions and time ranges.
api: openapi/newswhip-openapi.yml
operations: [articles]
---

# Discover trending articles

1. Authenticate every request with your NewsWhip key as the `key` query parameter (see `authentication/newswhip-authentication.yml`). Keys are issued by NewsWhip per account.
2. Call `POST /articles` on `https://api.newswhip.com/v1` with a JSON body containing your filters: a Lucene `filters` query string (see docs/lucene-query-strings), `language`, `time` window, `sort_by` (e.g. engagement or predicted engagement), and `size` to cap results.
3. Read the returned `Article` objects — headline, link, publisher, per-network engagement counts, and NewsWhip's predicted engagement.
4. Respect result-size limits and per-key rate limits; back off on `429`. Errors are plain JSON with an HTTP status (see `errors/newswhip-problem-types.yml`).

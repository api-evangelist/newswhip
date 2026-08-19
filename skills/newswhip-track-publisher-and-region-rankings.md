---
name: Track publisher, region and local rankings
description: Use the NewsWhip Quick Start API GET endpoints to pull ranked, near-real-time content feeds for one publisher, one region and category, or one city — the lowest-friction way to build a wallboard or newsroom dashboard.
api: openapi/newswhip-rankings-api-openapi.yml
operations: [publisher, region, local]
generated: '2026-08-13'
method: generated
source: openapi/newswhip-rankings-api-openapi.yml + https://developer.newswhip.com/docs/api-features
---

# Track publisher, region and local rankings

These three Quick Start (formerly "GET") endpoints need no request body — everything is in the path. They cover the last seven days and are the right choice for a dashboard that refreshes on a timer.

## Steps

1. **Authenticate.** Append `?key=YOUR_API_KEY` to the URL. The docs warn the key is to be treated as a password; because it rides in the query string it will land in any intermediary logs, so terminate calls server-side where you can.
2. **Pick the feed:**
   - `GET /region/{Region}/{Category}/{Time_Period}` (operationId `region`) — ranked content for a region and category, e.g. `/region/U.S./Politics/24`. Valid regions: https://developer.newswhip.com/docs/regions. Valid categories: https://developer.newswhip.com/docs/topics.
   - `GET /publisher/{Publisher}/{Time_Period}` (operationId `publisher`) — ranked content from one publisher/domain. See https://developer.newswhip.com/docs/publishers-and-domains.
   - `GET /local/{City}/All/{Time_Period}` (operationId `local`) — hyper-local content for one city.
3. **Set `Time_Period`** to the lookback in hours (for example `24`).
4. **Cap the response** with `size`. The Quick Start ceiling is 500 records per query; the per-endpoint default is documented in the API reference.
5. **Read the articles** — headline, link, publisher, per-network engagement and NewsWhip's predicted engagement.

## Notes

- **Do not poll faster than the cache.** Quick Start responses are cached per API key for roughly three minutes, so a tighter refresh burns rate-limit budget for identical data. Read the response headers for the exact cache value.
- **Rate limit:** 100 requests per rolling 5 minutes, max 5 per second, `429` on exhaustion, no rate-limit headers to read. Budget your dashboard's refresh interval against the number of feeds it renders.
- **Browser-side calls:** the Quick Start API supports exactly one configured cross-origin origin per API key, and NewsWhip must set it up (api@newswhip.com). The Historical and Syndication endpoints are server-side only.
- **Avoid retired filters:** the `mayhem` category (ID 22) is deprecated — queries using it silently return no results rather than failing. See `changelog/newswhip-changelog.yml`.

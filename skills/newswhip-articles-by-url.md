---
name: Get engagement for specific URLs
description: Retrieve NewsWhip engagement and prediction metrics for a known list of article URLs.
api: openapi/newswhip-openapi.yml
operations: [articlesbyhrefs]
---

# Look up engagement for known URLs

1. Authenticate with the `key` query parameter.
2. Call `POST /articlesByHrefs` with a JSON body listing the article `hrefs` (URLs) you want metrics for.
3. Each returned `Article` carries its social engagement and predicted-engagement figures.
4. Handle `400` for malformed URLs and `429` for rate limiting.

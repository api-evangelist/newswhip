---
name: Find top influencers driving a story
description: Identify the Facebook pages and Twitter accounts driving the most engagement for content matching your query.
api: openapi/newswhip-openapi.yml
operations: [fbinfluencers, twitterinfluencers]
---

# Find top influencers

1. Authenticate with the `key` query parameter.
2. Call `POST /fbInfluencers` (Facebook pages) or `POST /twitterInfluencers` (Twitter accounts) with the same Lucene filter/time body used for `/articles`.
3. Rank the returned `Influencer` entries by engagement to see who is amplifying the topic.
4. Combine with `POST /stats` for aggregate engagement over the same query.

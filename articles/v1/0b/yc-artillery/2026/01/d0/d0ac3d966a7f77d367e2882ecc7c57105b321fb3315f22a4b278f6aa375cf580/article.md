---
schema_version: "1.0.0"
document_id: "d0ac3d966a7f77d367e2882ecc7c57105b321fb3315f22a4b278f6aa375cf580"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-28"
published_at: "2026-01-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:ea6fcc2586c7bb54075d29af87a6fc32ac28c9909f5d8a246f76960b68d6ddc4"
---

# Artillery CLI v2.0.28

January 23rd, 2026[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.28


## New features


- Add query string normalization to reduce the number of distinct metrics in Playwright tests. Two new configuration options are now available:


- ` stripQueryString` (defaults to` false` ): Remove query strings from URLs
- ` normalizeQueryString` (defaults to` true` ): Normalize query string values to reduce metric cardinality. Converts numeric values like` /page?id=123` to` /page?id=NUMBER`


## Fixes & improvements


- Fix issue on Azure ACI where tests that track many distinct metrics could fail or report incomplete results
- Fix issue on AWS Fargate where` --scenario-name` flag was ignored, causing all scenarios to run instead of just the specified scenario
- Upgrade to Playwright v1.57.0
- Routine dependency upgrades across all packages


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.28](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.28)

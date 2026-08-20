---
schema_version: "1.0.0"
document_id: "04743bc26ba7dd0acbc72a95049b607f04c437df7ea95bf9d308036f449712da"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-25"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:76d1e55ec29d92f07c7df1d225996ba6ba88c8e60b7db0f1316200462d44104c"
---

# Artillery CLI v2.0.25

September 18th, 2025[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.25


### New features


- Stash API is now available. Stash API provides a persistent key-value store for use in your Artillery tests to share data between VUs, and offers Redis-compatible features like working with key-value pairs, lists, counters, and more. Learn more in the[Stash API docs](https://www.artillery.io/docs/reference/stash)


### Playwright


- Upgrade to Playwright v1.55.0


### Fixes & Improvements


- Fix: Prioritize dependencies set via` package.json` over automatically-detected dependencies. This fixes issues where` latest` versions of dependencies could be installed rather than those specified in` package.json`
- Fix: Avoid promisifying async “function” steps. This removes Node.js warnings when when an` async` function is used for a` function` step
- Fix: Use China partition-aware ARNs and principals for tests on AWS Lambda/Fargate
- Routine updates to dependencies to address security reports
- Remove dependency on PostHog SDK
- Migrate to AWS SDK v3. This is an internal change that does not affect any user-visible functionality


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.25](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.25)

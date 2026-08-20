---
schema_version: "1.0.0"
document_id: "5449e0a37890368a9cfb2744c6a697facfd50082995d65fb4c1b3ff8b94e85f8"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-21"
published_at: "2024-10-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:9a82edcb06fab01b7ecfe724733eb0c3ad7b38a02ba185582bb7a824f2a30064"
---

# Artillery CLI v2.0.21

October 22nd, 2024[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.21


Hassy Veldstra


## Core & CLI


- Add` --env-file` flag as an alternative for` --dotenv` flag. This makes it consistent with the[Node.js --env-file flag](https://nodejs.org/dist/latest-v20.x/docs/api/cli.html#--env-fileconfig) . The` --dotenv` flag will be deprecated in a future release ([#3376](https://github.com/artilleryio/artillery/pull/3376) )
- Add tracking of response times by HTTP status code. A new set of metrics (e.g.` http.response_time.2xx` or` http.response_time.5xx` ) is now reported to provide more granular view of response times in a test ([#3326](https://github.com/artilleryio/artillery/pull/3326) )
- Fix an issue that caused incorrect “multiple batches of metics” warnings when running tests with` pause` phases ([#3331](https://github.com/artilleryio/artillery/pull/3331) )


## Playwright


- Upgrade Playwright to v1.48.0


## Azure


- Fix: Make values loaded from an env file with` --dotenv` /` --env-file` flag available to workers containers (rather than just the Artillery process running inside the worker) ([#3376](https://github.com/artilleryio/artillery/pull/3376) )
- Stagger startup of containers in large load tests to prevent rate limit errors from Azure services ([#3371](https://github.com/artilleryio/artillery/pull/3371) )


## OpenTelemetry


- Add support for setting resource-level attributes ([#3335](https://github.com/artilleryio/artillery/pull/3335) )


## Artillery Cloud


- Send CI related information to Artillery Cloud. This makes the following information available in Artillery Cloud:


- Whether a test run was triggered in CI or not, and which CI service was used
- For tests triggered in GitHub Actions - provide a link back to the job run on GitHub Actions


## Dependencies


- Upgrade` json-plus` to address a critical security vulnerability ([#3369](https://github.com/artilleryio/artillery/pull/3369) )

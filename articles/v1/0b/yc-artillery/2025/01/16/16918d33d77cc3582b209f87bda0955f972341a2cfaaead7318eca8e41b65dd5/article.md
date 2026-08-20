---
schema_version: "1.0.0"
document_id: "16918d33d77cc3582b209f87bda0955f972341a2cfaaead7318eca8e41b65dd5"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-22"
published_at: "2025-01-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:9931fa4670c316c345c4553bb96837678b2f506778195d38987e421c1366d426"
---

# Artillery CLI v2.0.22

January 13th, 2025[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.22


Hassy Veldstra


## Highlights


- Add support for writing Artillery scripts in TypeScript ([#3436](https://github.com/artilleryio/artillery/pull/3436)[#3439](https://github.com/artilleryio/artillery/pull/3439) ). -[Hello World example](https://www.artillery.io/docs/reference/engines/playwright#create-your-test-definition) .
- The` report` command has ben removed. As an alternative consider setting up[Artillery Cloud](https://www.artillery.io/docs/get-started/artillery-cloud) for visualizing test metrics, or setting up an OpenTelemetry integration with an external monitoring system with the` publish-metrics` plugin. ([#3431](https://github.com/artilleryio/artillery/pull/3431) )
- Node.js v22.13.0 (current active LTS) is the recommended version of Node.js for running Artillery now
- Artillery’s official Docker image ([https://hub.docker.com/r/artilleryio/artillery](https://hub.docker.com/r/artilleryio/artillery) ) includes Chromium for Playwright now ([#3449](https://github.com/artilleryio/artillery/pull/3449)[#3445](https://github.com/artilleryio/artillery/pull/3445) )
- The Docker image is now based on Debian rather than Alpine ([#3449](https://github.com/artilleryio/artillery/pull/3449) )
- Upgrade to Playwright v1.49.1 ([#3427](https://github.com/artilleryio/artillery/pull/3427) )


## Fixes & improvements


- Fix issue where` config.target` could not be set to the value of a remote environment variable ([#3430](https://github.com/artilleryio/artillery/pull/3430) )
- Fix issues that led to more tasks being launched on Fargate than requested in scenarios where Fargate is temporarily out of capacity ([#3432](https://github.com/artilleryio/artillery/pull/3432) )
- Make sure that stopping Playwright tracing is failsafe ([#3443](https://github.com/artilleryio/artillery/pull/3443) )
- Routine dependency upgrades

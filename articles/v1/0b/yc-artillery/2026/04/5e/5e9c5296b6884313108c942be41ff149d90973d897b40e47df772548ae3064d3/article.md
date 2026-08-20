---
schema_version: "1.0.0"
document_id: "5e9c5296b6884313108c942be41ff149d90973d897b40e47df772548ae3064d3"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-31"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:51506fdf25a3edbc0a6e4e0a3ec22bf45830d1270f86f39e4d045119d725ee38"
---

# Artillery CLI v2.0.31

April 24th, 2026[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.31


## New features


### HTTP engine


- Add support for W3C Trace Context propagation on outgoing HTTP requests. Enable via` config.http.distributedTracing` (boolean, or object with` enabled` ,` sampled` , and` traceIdPrefix` fields). When enabled, Artillery adds a` traceparent` header to each request so backend traces can be correlated with the originating test.


### AWS Fargate


- Add CLI heartbeat mechanism for scenarios where the CLI exits abnormally (e.g. with` SIGKILL` ) and worker tasks continue running. Fargate worker tasks will now self-terminate if the CLI disappears for more than 180 seconds.


### Slack plugin


- Add` notifyOnFailureOnly` option to only send a Slack notification when a test fails.


### Playwright


- Upgrade to Playwright v1.59.1


## Fixes & improvements


### AWS Lambda


- Fix issue where test data with` npm` dependencies (for example, TypeScript scenarios that require external packages) could silently skip installation, causing the test to fail later with module-not-found errors.


### Core


- Upgrade the HTTP engine to Got v14.6.6 (from Got v11). **Potentially breaking change** if your tests use hooks that access or modify Got internals.


### Azure ACI


- Test artifacts in Blob Storage are now retained by default. To automatically remove old artifacts, configure a lifecycle management policy on the storage account.


### OpenTelemetry


- Upgrade the OpenTelemetry JS SDK to 2.x. Artillery now emits both the legacy and current semantic attribute names on spans (for example` http.url` alongside` url.full` , and` http.method` alongside` http.request.method` ) so existing dashboards and queries continue to work.


## Other improvements & fixes


- Worker images used on AWS Fargate, Lambda and Azure ACI are now based on the official Docker Hardened Node.js image
- Routine dependency upgrades to address CVEs


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.31](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.31)

---
schema_version: "1.0.0"
document_id: "54761a046d94b9abf49bae89507271c1d3ea239cafc6d704c0cd7940feafcc8d"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-27"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:c394f894c1730823376e2ec12f3d9145dde7c4df8e879cb455fadd400cad8dac"
---

# Artillery CLI v2.0.27

November 18th, 2025[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.27


## New features


### Core


- Custom metric names are now limited to 1024 characters


### AWS ECS/Fargate


- Add new` --no-assign-public-ip` flag to disable automatic assignment of public IPs to worker tasks. This allows for load tests to run from private subnets with a NAT gateway attached for use-cases where traffic has to come from a set of known IPs.


### Azure ACI


- Azure load testing is now available on all subscriptions, including month-to-month ones. Previously only annual subscriptions could use the Azure integration.


### Load testing with Playwright


- Upgrade to Playwright v1.56.1
- FID (First Input Delay) scores are no longer reported. FID has been deprecated as a Core Web Vital metric on September 9, 2024, and replaced by INP (Interaction To Next Paint).[https://web.dev/articles/fid](https://web.dev/articles/fid)


## Fixes & improvements


### AWS ECS/Fargate


- Fix issue where ECS IAM policy could not be created.
- Fix handling of relative paths in` config.payload` when a separate config is used with the` --config` flag.
- Fix handling of` --scenario-name` flag


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.27](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.27)

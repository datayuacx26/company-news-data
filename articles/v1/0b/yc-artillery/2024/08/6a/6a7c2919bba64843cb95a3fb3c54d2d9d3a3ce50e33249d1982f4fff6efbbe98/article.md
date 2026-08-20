---
schema_version: "1.0.0"
document_id: "6a7c2919bba64843cb95a3fb3c54d2d9d3a3ce50e33249d1982f4fff6efbbe98"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-19"
published_at: "2024-08-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:904422f6e29ae0cda885b477db8f178f209fd8c5c91248eef3a814857adb049d"
---

# Artillery CLI v2.0.19

August 6th, 2024[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.19


Bernardo Guerreiro


## CLI


- Fix bug preventing custom code using ES modules from loading on Windows ([#2662](https://github.com/artilleryio/artillery/pull/2662) )
- Prevent setting individual CSV rows in context when using` loadAll` ([#3277](https://github.com/artilleryio/artillery/pull/3277) )


## Fargate


- Add ability to set DNS servers using the` --container-dns-servers` flag when running on ECS/EC2 ([#3301](https://github.com/artilleryio/artillery/pull/3301) )
- Add ability to override the ephemeral storage amount for each task via the` --task-ephemeral-storage` flag ([#3301](https://github.com/artilleryio/artillery/pull/3301) )


## Playwright


- Upgrade Playwright to v1.45.3 ([#3294](https://github.com/artilleryio/artillery/pull/3294) )

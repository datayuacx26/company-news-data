---
schema_version: "1.0.0"
document_id: "b2e76ef676cee8d8ccdef09fcd13446a51e3e466c978af6ea8b81ae210532bb1"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-23"
published_at: "2025-05-12T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:86ad96247afb650552723e5af6624d32091396c6a83c395b423b556a12354473"
---

# Artillery CLI v2.0.23

May 12th, 2025[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.23


Hassy Veldstra


### Playwright


- Upgrade to Playwright v1.52.0 ([#3523](https://github.com/artilleryio/artillery/pull/3523) )
- Increase` maxConcurrentRecordings` from 3 to 5 to increase the probability of capturing traces for failed VUs ([#3533](https://github.com/artilleryio/artillery/pull/3533) )
- Increase upload timeout for traces to help make sure large trace recordings are uploaded to Artillery Cloud ([#3533](https://github.com/artilleryio/artillery/pull/3533) )


### Azure ACI


- Client ID and client secret must be provided via` AZURE_CLIENT_ID` and` AZURE_CLIENT_SECRET` environment variables rather than CLI flags. This brings Artillery CLI in line with Azure SDK’s[DefaultAzureCredential](https://learn.microsoft.com/en-us/azure/developer/javascript/sdk/authentication/credential-chains#use-defaultazurecredential-for-flexibility) credential chain. ([#3525](https://github.com/artilleryio/artillery/pull/3525) )
- Add support for overriding worker startup timeout via` WORKER_WAIT_TIMEOUT_SEC` environment variable ([#3527](https://github.com/artilleryio/artillery/pull/3527) )
- Fix issue that caused tests comprised of a single TypeScript file to fail to run ([#3528](https://github.com/artilleryio/artillery/pull/3528) )


### AWS Fargate


- Add more supported regions:` us-gov-east-1` &` us-gov-east-2` (AWS GovCloud),` il-central-1` (Israel),` cn-north-1` &` cn-northwest-1` (China) ([#3522](https://github.com/artilleryio/artillery/pull/3522) )
- Fix issue with` --task-role-name` flag not being taken into account ([#3469](https://github.com/artilleryio/artillery/pull/3469) )
- Fix issue that could lead to metric reports from workers to be processed with a lag in large tests ([#3472](https://github.com/artilleryio/artillery/pull/3472) )


### Other improvements & fixes


- Improve layout of Slack test summaries posted by the` slack` plugin ([#3499](https://github.com/artilleryio/artillery/pull/3499) )
- Fix issue in tests written in TypeScript that led to the generated load being higher than expected ([#3495](https://github.com/artilleryio/artillery/pull/3495) )

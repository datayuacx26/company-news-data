---
schema_version: "1.0.0"
document_id: "cd46283b71e2b7a2a892e6eedcf38ab06ea4727f6721dc84c42677df5602ed1d"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-20"
published_at: "2024-08-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:6f5ec8a70c26f46f5fc42352f71b372a19cb3b31ad542477033e9a5ceb41f0d9"
---

# Artillery CLI v2.0.20

August 22nd, 2024[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.20


Hassy Veldstra


## Core & CLI


- New: add ability to set a custom content type for` multipart/form-data` form fields[#3316](https://github.com/artilleryio/artillery/pull/3316) —[docs](https://www.artillery.io/docs/reference/engines/http#multipart-forms-multipartform-data)
- Fix: don’t print an unnecessary warning when` loadAll` is not set by @hassy in[#3303](https://github.com/artilleryio/artillery/pull/3303)
- Remove legacy Artillery Pro integration[#3320](https://github.com/artilleryio/artillery/pull/3320)


## Artillery Cloud


- New: add` --name` option to set the name of the test to be shown in Artillery Cloud dashboard[#3317](https://github.com/artilleryio/artillery/pull/3317) —[docs](https://www.artillery.io/docs/get-started/artillery-cloud#naming-tests)
- Improve pre-flight checks when recording reports to Artillery Cloud to detect potential firewall/proxy issues[#3314](https://github.com/artilleryio/artillery/pull/3314)
- Fix: include organization IDs in the test report URLs


## Azure


- Fix: bundle separate config files provided with` --config` option correctly[#3312](https://github.com/artilleryio/artillery/pull/3312)
- Fix: bundle dotenv files provided with` --dotenv` correctly[#3313](https://github.com/artilleryio/artillery/pull/3313)
- Fix: bundle custom` .npmrc` and other dotfiles correctly[#3312](https://github.com/artilleryio/artillery/pull/3312)
- Improve error handling and reporting for container provisioning errors[#3313](https://github.com/artilleryio/artillery/pull/3313)

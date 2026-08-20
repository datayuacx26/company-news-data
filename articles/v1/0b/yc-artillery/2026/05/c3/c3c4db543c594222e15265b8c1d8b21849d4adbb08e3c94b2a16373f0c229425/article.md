---
schema_version: "1.0.0"
document_id: "c3c4db543c594222e15265b8c1d8b21849d4adbb08e3c94b2a16373f0c229425"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-32"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-08-15T01:51:45.174359+00:00"
fetched_at: "2026-08-15T01:51:46.793947+00:00"
content_hash: "sha256:5a1852334c54ef34bd11d2d5f3b440e67ae90cdd8cbaf29ddc2064804ccf1b17"
---

# Artillery CLI v2.0.32

May 19th, 2026[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.32


## New features


### Playwright


- Upgrade to Playwright v1.60.0


## Fixes & improvements


### Core


- Fix issue where` http.timeout` was silently capped at 8 seconds for slow origins. Requests against backends that took more than 8 seconds to send the first response byte would fail with` ERR_SOCKET_TIMEOUT` regardless of the configured` http.timeout` value.
- Fix issue where Artillery commands could fail at startup with` MODULE_NOT_FOUND` errors for` @smithy/node-config-provider` and` @smithy/config-resolver` .


### AWS ECS/Fargate


- Fix issue where tests that depend on external npm modules could run with fewer VUs than configured due to undetected worker startup errors.


### Artillery Cloud


- Fix regression where errors from trace upload requests were not surfaced in the CLI output.
- Improve reliability of sending metrics and events to Artillery Cloud.


## Other improvements & fixes


- Improved dependency detection for tests that use external npm modules.


### Dependencies


- Update OpenTelemetry exporter dependencies
- Various dependency upgrades


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.32](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.32)

---
schema_version: "1.0.0"
document_id: "81dc569be1daebe9ec26e0b5ffe0a1de6a0601e16d4765d2d459e7acd43bd491"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/artillery-cli-v2-0-34"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T01:51:45.174359+00:00"
fetched_at: "2026-08-15T01:51:46.793947+00:00"
content_hash: "sha256:6508272bb8ef9328a3baf8b20841fe0255ba4434ffb15c12536dfd0789d94e89"
---

# Artillery CLI v2.0.34

August 14th, 2026[CLI](https://www.artillery.io/changelog/tag/cli)


# Artillery CLI v2.0.34


## New features


### Core & CLI


- Add` artillery ping` command for quick testing of HTTP endpoints. Sends a request to a URL and shows a performance breakdown (DNS/TCP/TLS/TTFB/download timings) and response headers. Supports expectations via` --expect` , JMESPath and Cheerio queries on the response body, custom headers, basic auth, JSON/form request bodies, and HTTP/2 (default) or HTTP/1.1
- Full support for ESM: in processors, plugins, engines and custom reporters, including modules that use top-level` await`
- TypeScript type definitions shipped with the` artillery` package now model the full test script: all arrival phase kinds, scenarios for every built-in engine, capture/match, built-in plugin configs, and processor hook contracts.


### AWS Lambda & Fargate


- Add` --aws-tags` flag to apply custom tags to AWS resources created for a test run (Fargate tasks, Lambda functions, SQS queues). Example:` --aws-tags "team:perf,cost-center:1234"`


### Playwright


- Upgrade to Playwright v1.62.1


## Fixes & improvements


### Core & CLI


- Fix issue where tests using relative imports (e.g.` import x from './helpers.js'` ) in processor code could fail to run
- Fix issue where metric renaming via` $rewriteMetricName` was silently ignored when the processor file was an ES module


### AWS Lambda & Fargate


- The` fake-data` plugin is now bundled with the worker image. Previously workers downloaded it from the npm registry at test startup


### Docker


- Scripts mounted into the official Docker image can now import dependencies bundled with Artillery, such as Playwright


## Other changes


- Various dependency upgrades to address security advisories
- npm releases are now published via npm trusted publishing (OIDC) with provenance attestations


Full release notes on GitHub:[https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.34](https://github.com/artilleryio/artillery/releases/tag/artillery-2.0.34)

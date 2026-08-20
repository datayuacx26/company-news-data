---
schema_version: "1.0.0"
document_id: "cbfb9d88b1c2b8987366a4bf31e19a3af49fc64c3c3ca142fa1b97c92db4e562"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/reliability-observability-data-layer-improvements"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:5bc6c8d87891e081a5736f2f3165b7b13d723db6340a16b5a55ee7f7894fdd2c"
---

# Reliability, Observability & Data Layer Improvements

We shipped bug fixes, better observability, and a refactored data layer for the Core Gateway.


**Fixes & observability**


Endpoints in **Paused** or **Inactive** state now activate correctly, and OSS default-user login no longer requires a license. We fixed filter evaluation for boolean values in arrays and a startup bug where the worker blocked the agent server. For Sentry, we've updated the configuration to use the default **Environment** field.


**Data layer improvements**


We refactored the data layer to use[SQLC](https://sqlc.dev/) for type-safe SQL across API keys, configuration, delivery attempts, organisations, portal links, projects, sources, and filters. Behaviour is unchanged and no config or API changes are required on your side.


Upgrade for the fixes and observability improvements.

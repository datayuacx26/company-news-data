---
schema_version: "1.0.0"
document_id: "68e1c60d915352373f86225ead4cf5f35ed9b78cd2b1f89d62443a4536d6ba73"
company_key: "yc-tracecat"
company: "Tracecat"
source_id: "yc-tracecat-atom-977c8de76e88"
canonical_url: "https://github.com/TracecatHQ/tracecat/releases/tag/1.0.0-beta.51-rc.8"
published_at: "2026-07-16T21:39:48+00:00"
first_seen_at: "2026-08-10T04:11:45.938952+00:00"
fetched_at: "2026-08-11T19:17:55.950786+00:00"
content_hash: "sha256:74f3a255ec5faeef8b1a73f862ed02d648a5a7ab609a9164dc7232d7f5747217"
---

# Tracecat 1.0.0-beta.51-rc.8

## Breaking changes


- make Action Gateway mandatory ([#3075](https://github.com/TracecatHQ/tracecat/pull/3075) )


## Security


- bump patched security dependencies ([#3054](https://github.com/TracecatHQ/tracecat/pull/3054) )


## Integrations


- correct alertmedia search_users input schema ([#3021](https://github.com/TracecatHQ/tracecat/pull/3021) )
- add Scanner YAML templates ([#3037](https://github.com/TracecatHQ/tracecat/pull/3037) )
- prevent repeated MCP failure toast on refresh ([#3048](https://github.com/TracecatHQ/tracecat/pull/3048) )
- add SentinelOne alert lifecycle actions ([#3049](https://github.com/TracecatHQ/tracecat/pull/3049) )


## Agents


- move first-prompt auto-title off the request path ([#3062](https://github.com/TracecatHQ/tracecat/pull/3062) )
- prioritize interactive turns on the shared agent queue ([#3064](https://github.com/TracecatHQ/tracecat/pull/3064) )
- improve stdio MCP probe timeouts ([#3071](https://github.com/TracecatHQ/tracecat/pull/3071) )
- surface stream idle timeouts ([#3076](https://github.com/TracecatHQ/tracecat/pull/3076) )


## Performance improvements


- batch GitHub export writes to avoid rate limits ([#3073](https://github.com/TracecatHQ/tracecat/pull/3073) )


## Bug fixes


- serialize attachment quota checks per case ([#3019](https://github.com/TracecatHQ/tracecat/pull/3019) )
- improve expression highlight contrast ([#3043](https://github.com/TracecatHQ/tracecat/pull/3043) )
- remove Workspace sidebar slide animation ([#3044](https://github.com/TracecatHQ/tracecat/pull/3044) )
- correct Okta private key placeholder ([#3055](https://github.com/TracecatHQ/tracecat/pull/3055) )
- allow org service accounts to manage settings ([#3056](https://github.com/TracecatHQ/tracecat/pull/3056) )
- simplify workflow execution logs ([#3067](https://github.com/TracecatHQ/tracecat/pull/3067) )


## Documentation


- add audit log documentation ([#3036](https://github.com/TracecatHQ/tracecat/pull/3036) )
- discourage untyped dictionaries ([#3068](https://github.com/TracecatHQ/tracecat/pull/3068) )


## Other improvements


- add workspace export diagnostics ([#3058](https://github.com/TracecatHQ/tracecat/pull/3058) )


**Full changelog** :[1.0.0-beta.51-rc.7...1.0.0-beta.51-rc.8](https://github.com/TracecatHQ/tracecat/compare/1.0.0-beta.51-rc.7...1.0.0-beta.51-rc.8)

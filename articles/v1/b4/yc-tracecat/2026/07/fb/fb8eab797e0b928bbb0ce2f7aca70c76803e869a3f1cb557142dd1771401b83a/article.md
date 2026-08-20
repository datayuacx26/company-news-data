---
schema_version: "1.0.0"
document_id: "fb8eab797e0b928bbb0ce2f7aca70c76803e869a3f1cb557142dd1771401b83a"
company_key: "yc-tracecat"
company: "Tracecat"
source_id: "yc-tracecat-atom-977c8de76e88"
canonical_url: "https://github.com/TracecatHQ/tracecat/releases/tag/1.0.0-beta.51-rc.11"
published_at: "2026-07-28T16:32:14+00:00"
first_seen_at: "2026-08-10T04:11:45.938952+00:00"
fetched_at: "2026-08-13T16:57:35.139984+00:00"
content_hash: "sha256:d8ffd622ab7d6655272fda7d910acfaeeb1e1c5405945deb01d8ffd2a3bcf84c"
---

# Tracecat 1.0.0-beta.51-rc.11

## Security


- constrain nested action execution ([#3072](https://github.com/TracecatHQ/tracecat/pull/3072) )
- drop force flag from sync_custom_registry tool ([#3138](https://github.com/TracecatHQ/tracecat/pull/3138) )


## Integrations


- support mcp resource content blocks ([#3105](https://github.com/TracecatHQ/tracecat/pull/3105) )
- stop forwarding inbound auth to user MCP servers ([#3132](https://github.com/TracecatHQ/tracecat/pull/3132) )


## Agents


- exclude run_python from agent tools ([#3115](https://github.com/TracecatHQ/tracecat/pull/3115) )
- correlate agent preset catalog IDs ([#3117](https://github.com/TracecatHQ/tracecat/pull/3117) )
- emit terminal stream END only after finalize_turn ([#3120](https://github.com/TracecatHQ/tracecat/pull/3120) )


## Performance improvements


- wire pool timeout and per-service application_name ([#3134](https://github.com/TracecatHQ/tracecat/pull/3134) )


## Enhancements


- add Google Security Command Center integration ([#3046](https://github.com/TracecatHQ/tracecat/pull/3046) )
- expand audit log coverage ([#3082](https://github.com/TracecatHQ/tracecat/pull/3082) )


## Bug fixes


- use pointer cursor on workflow execution rows ([#3070](https://github.com/TracecatHQ/tracecat/pull/3070) )
- keep dialogs within viewport ([#3095](https://github.com/TracecatHQ/tracecat/pull/3095) )
- keep pull actions visible for large previews ([#3118](https://github.com/TracecatHQ/tracecat/pull/3118) )
- render workspace sync toast as external link ([#3123](https://github.com/TracecatHQ/tracecat/pull/3123) )
- calibrate notification banners for dark mode ([#3127](https://github.com/TracecatHQ/tracecat/pull/3127) )


## Other improvements


- pin ruff to pyproject version in lint workflow ([#3121](https://github.com/TracecatHQ/tracecat/pull/3121) )


**Full changelog** :[1.0.0-beta.51-rc.10...1.0.0-beta.51-rc.11](https://github.com/TracecatHQ/tracecat/compare/1.0.0-beta.51-rc.10...1.0.0-beta.51-rc.11)

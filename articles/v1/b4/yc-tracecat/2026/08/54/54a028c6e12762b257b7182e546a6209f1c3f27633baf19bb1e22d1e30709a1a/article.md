---
schema_version: "1.0.0"
document_id: "54a028c6e12762b257b7182e546a6209f1c3f27633baf19bb1e22d1e30709a1a"
company_key: "yc-tracecat"
company: "Tracecat"
source_id: "yc-tracecat-atom-977c8de76e88"
canonical_url: "https://github.com/TracecatHQ/tracecat/releases/tag/1.0.0-beta.51"
published_at: "2026-08-10T22:03:33+00:00"
first_seen_at: "2026-08-10T23:36:21.485784+00:00"
fetched_at: "2026-08-18T17:02:31.943527+00:00"
content_hash: "sha256:4ac385ea865f384efb8f912d288fcce147753d0b555a61fd5c29d7433237e70d"
---

# Tracecat 1.0.0-beta.51

## Breaking changes


- make Action Gateway mandatory ([#3075](https://github.com/TracecatHQ/tracecat/pull/3075) )


## Security


- scope workflow execution definitions ([#2952](https://github.com/TracecatHQ/tracecat/pull/2952) )
- unblock OpenCode OAuth + clearer org-resolution errors ([#2990](https://github.com/TracecatHQ/tracecat/pull/2990) )
- bump patched security dependencies ([#3054](https://github.com/TracecatHQ/tracecat/pull/3054) )
- constrain nested action execution ([#3072](https://github.com/TracecatHQ/tracecat/pull/3072) )
- patch deps ([#3085](https://github.com/TracecatHQ/tracecat/pull/3085) )
- drop force flag from sync_custom_registry tool ([#3138](https://github.com/TracecatHQ/tracecat/pull/3138) )
- add AI security architecture ([#3158](https://github.com/TracecatHQ/tracecat/pull/3158) )
- terminate action process groups ([#3159](https://github.com/TracecatHQ/tracecat/pull/3159) )
- overhaul security, isolation, and audit log documentation ([#3166](https://github.com/TracecatHQ/tracecat/pull/3166) )
- patch dependabot alerts ([#3192](https://github.com/TracecatHQ/tracecat/pull/3192) )
- add GitHub REST YAML template catalog ([#3193](https://github.com/TracecatHQ/tracecat/pull/3193) )
- add GitLab REST YAML template catalog ([#3194](https://github.com/TracecatHQ/tracecat/pull/3194) )
- add MISP security integration ([#3198](https://github.com/TracecatHQ/tracecat/pull/3198) )
- mark session cookie Secure when public API uses HTTPS ([#3201](https://github.com/TracecatHQ/tracecat/pull/3201) )


## Integrations


- adopt ctx facade over get_context() ([#2885](https://github.com/TracecatHQ/tracecat/pull/2885) )
- restore okta mcp ([#2918](https://github.com/TracecatHQ/tracecat/pull/2918) )
- okta sdk tools ([#2919](https://github.com/TracecatHQ/tracecat/pull/2919) )
- add Slack canvas templates ([#2927](https://github.com/TracecatHQ/tracecat/pull/2927) )
- freshservice mcp and actions ([#2938](https://github.com/TracecatHQ/tracecat/pull/2938) )
- cap slack paginated results at limit ([#2948](https://github.com/TracecatHQ/tracecat/pull/2948) )
- add caps to cloudflare and google api pagination ([#2953](https://github.com/TracecatHQ/tracecat/pull/2953) )
- test stdio mcp connections ([#2959](https://github.com/TracecatHQ/tracecat/pull/2959) )
- make views.update hash an optional input ([#2974](https://github.com/TracecatHQ/tracecat/pull/2974) )
- Add SentinelOne PowerQuery and MCP templates ([#2994](https://github.com/TracecatHQ/tracecat/pull/2994) )
- correct alertmedia search_users input schema ([#3021](https://github.com/TracecatHQ/tracecat/pull/3021) )
- add Scanner YAML templates ([#3037](https://github.com/TracecatHQ/tracecat/pull/3037) )
- FOR UPDATE lock during oauth refresh flow ([#3047](https://github.com/TracecatHQ/tracecat/pull/3047) )
- prevent repeated MCP failure toast on refresh ([#3048](https://github.com/TracecatHQ/tracecat/pull/3048) )
- add SentinelOne alert lifecycle actions ([#3049](https://github.com/TracecatHQ/tracecat/pull/3049) )
- pin stdio catalog integrations ([#3053](https://github.com/TracecatHQ/tracecat/pull/3053) )
- support optional oauth_resource override for mcp ([#3081](https://github.com/TracecatHQ/tracecat/pull/3081) )
- correct SentinelOne template contracts ([#3084](https://github.com/TracecatHQ/tracecat/pull/3084) )
- support mcp resource content blocks ([#3105](https://github.com/TracecatHQ/tracecat/pull/3105) )
- stop forwarding inbound auth to user MCP servers ([#3132](https://github.com/TracecatHQ/tracecat/pull/3132) )
- unblock catalog MCP OAuth setup ([#3165](https://github.com/TracecatHQ/tracecat/pull/3165) )
- add Elastic API templates ([#3176](https://github.com/TracecatHQ/tracecat/pull/3176) )
- add OpenSearch threat hunting actions ([#3197](https://github.com/TracecatHQ/tracecat/pull/3197) )
- expand Sublime investigation actions ([#3199](https://github.com/TracecatHQ/tracecat/pull/3199) )
- drop third-party contract tests ([#3205](https://github.com/TracecatHQ/tracecat/pull/3205) )


## Agents


- stream replay hardening ([#2897](https://github.com/TracecatHQ/tracecat/pull/2897) )
- configure versioned resource resolution ([#2905](https://github.com/TracecatHQ/tracecat/pull/2905) )
- batch agent approvals ([#2916](https://github.com/TracecatHQ/tracecat/pull/2916) )
- workflow draft authoring tools and MCP refactor ([#2920](https://github.com/TracecatHQ/tracecat/pull/2920) )
- built-in workspace-chat skills for workflow authoring ([#2921](https://github.com/TracecatHQ/tracecat/pull/2921) )
- preserve last error ([#2929](https://github.com/TracecatHQ/tracecat/pull/2929) )
- chat interrupts ([#2930](https://github.com/TracecatHQ/tracecat/pull/2930) )
- make preset deletion a soft delete ([#2931](https://github.com/TracecatHQ/tracecat/pull/2931) )
- add Claude Sonnet 5 to platform catalog ([#2935](https://github.com/TracecatHQ/tracecat/pull/2935) )
- include pending approval as streamable status ([#2983](https://github.com/TracecatHQ/tracecat/pull/2983) )
- show provider and hover card on agent tool chips ([#2984](https://github.com/TracecatHQ/tracecat/pull/2984) )
- add global soft-delete query filter ([#2998](https://github.com/TracecatHQ/tracecat/pull/2998) )
- expand skill soft delete to deleted_at ([#2999](https://github.com/TracecatHQ/tracecat/pull/2999) )
- add stable skill slugs with live uniqueness ([#3001](https://github.com/TracecatHQ/tracecat/pull/3001) )
- resolve skill identifiers by id then live slug ([#3003](https://github.com/TracecatHQ/tracecat/pull/3003) )
- add pinned_version_id columns and pin API ([#3004](https://github.com/TracecatHQ/tracecat/pull/3004) )
- make preset skill bindings head-only ([#3005](https://github.com/TracecatHQ/tracecat/pull/3005) )
- remove resource version pinning ([#3014](https://github.com/TracecatHQ/tracecat/pull/3014) )
- remove duplicate token remint ([#3030](https://github.com/TracecatHQ/tracecat/pull/3030) )
- add GPT-5.6 platform catalog models ([#3031](https://github.com/TracecatHQ/tracecat/pull/3031) )
- align subagent tool input with execution ([#3033](https://github.com/TracecatHQ/tracecat/pull/3033) )
- improve stdio MCP probe timeouts ([#3071](https://github.com/TracecatHQ/tracecat/pull/3071) )
- surface stream idle timeouts ([#3076](https://github.com/TracecatHQ/tracecat/pull/3076) )
- mistral provider ([#3108](https://github.com/TracecatHQ/tracecat/pull/3108) )
- exclude run_python from agent tools ([#3115](https://github.com/TracecatHQ/tracecat/pull/3115) )
- correlate agent preset catalog IDs ([#3117](https://github.com/TracecatHQ/tracecat/pull/3117) )
- expose catalog IDs for custom models in UI and API ([#3122](https://github.com/TracecatHQ/tracecat/pull/3122) )
- workflow execution tools ([#3150](https://github.com/TracecatHQ/tracecat/pull/3150) )
- preserve NUL session history ([#3156](https://github.com/TracecatHQ/tracecat/pull/3156) )
- enable deferred tool loading in the Claude runtime ([#3163](https://github.com/TracecatHQ/tracecat/pull/3163) )
- sanitize preset command values ([#3196](https://github.com/TracecatHQ/tracecat/pull/3196) )
- remove obsolete PydanticAI plugin ([#3203](https://github.com/TracecatHQ/tracecat/pull/3203) )
- agent mention autocomplete in case comment composer ([#3211](https://github.com/TracecatHQ/tracecat/pull/3211) )
- parse and persist case comment mentions ([#3213](https://github.com/TracecatHQ/tracecat/pull/3213) )


## Performance improvements


- async case duration sync ([#2781](https://github.com/TracecatHQ/tracecat/pull/2781) )
- expose Temporal worker concurrency tuning env knobs ([#2917](https://github.com/TracecatHQ/tracecat/pull/2917) )
- build loop regions in a single pass ([#2928](https://github.com/TracecatHQ/tracecat/pull/2928) )
- add workflow_definition lookup index ([#2957](https://github.com/TracecatHQ/tracecat/pull/2957) )
- move first-prompt auto-title off the request path ([#3062](https://github.com/TracecatHQ/tracecat/pull/3062) )
- prioritize interactive turns on the shared agent queue ([#3064](https://github.com/TracecatHQ/tracecat/pull/3064) )
- batch GitHub export writes to avoid rate limits ([#3073](https://github.com/TracecatHQ/tracecat/pull/3073) )
- cap bulk case action request concurrency ([#3091](https://github.com/TracecatHQ/tracecat/pull/3091) )
- defer case linked-row hydration to tables tab ([#3093](https://github.com/TracecatHQ/tracecat/pull/3093) )
- add batch case update and delete endpoints ([#3097](https://github.com/TracecatHQ/tracecat/pull/3097) )
- offload secret masking ([#3102](https://github.com/TracecatHQ/tracecat/pull/3102) )
- wire pool timeout and per-service application_name ([#3134](https://github.com/TracecatHQ/tracecat/pull/3134) )
- add postgres scatter capacity harness ([#3140](https://github.com/TracecatHQ/tracecat/pull/3140) )
- avoid unused relationship loads in case metrics ([#3157](https://github.com/TracecatHQ/tracecat/pull/3157) )
- deduplicate manifests in batch queries ([#3169](https://github.com/TracecatHQ/tracecat/pull/3169) )
- defer case number allocation ([#3171](https://github.com/TracecatHQ/tracecat/pull/3171) )


## Enhancements


- add workspace sync export ([#2859](https://github.com/TracecatHQ/tracecat/pull/2859) )
- redesign commit selector dropdown ([#2883](https://github.com/TracecatHQ/tracecat/pull/2883) )
- add GitLab workspace sync ([#2925](https://github.com/TracecatHQ/tracecat/pull/2925) )
- add include_payload param to cases search ([#2926](https://github.com/TracecatHQ/tracecat/pull/2926) )
- Allow admins to delete unused platform registry versions ([#2960](https://github.com/TracecatHQ/tracecat/pull/2960) )
- paste images inline in case description and comments ([#2962](https://github.com/TracecatHQ/tracecat/pull/2962) )
- add dark mode across the app ([#2963](https://github.com/TracecatHQ/tracecat/pull/2963) )
- add missing tool integration icons ([#2985](https://github.com/TracecatHQ/tracecat/pull/2985) )
- Add folder breadcrumbs to workflow and agent details ([#2991](https://github.com/TracecatHQ/tracecat/pull/2991) )
- Persist workspace panel preferences ([#2992](https://github.com/TracecatHQ/tracecat/pull/2992) )
- add sidebar organization switcher ([#3027](https://github.com/TracecatHQ/tracecat/pull/3027) )
- add Google Security Command Center integration ([#3046](https://github.com/TracecatHQ/tracecat/pull/3046) )
- expand audit log coverage ([#3082](https://github.com/TracecatHQ/tracecat/pull/3082) )
- rename workflows from the dashboard and builder ([#3083](https://github.com/TracecatHQ/tracecat/pull/3083) )
- add payload copy button ([#3090](https://github.com/TracecatHQ/tracecat/pull/3090) )
- feat(cases) ENG-1597: add team scoped agent session reads ([#3204](https://github.com/TracecatHQ/tracecat/pull/3204) )


## Bug fixes


- remove duplicate skills row divider ([#2922](https://github.com/TracecatHQ/tracecat/pull/2922) )
- render Mermaid diagrams in case descriptions ([#2934](https://github.com/TracecatHQ/tracecat/pull/2934) )
- keyboard selection and dismiss dead-state in tag combobox ([#2937](https://github.com/TracecatHQ/tracecat/pull/2937) )
- center column type label in table create dialog ([#2941](https://github.com/TracecatHQ/tracecat/pull/2941) )
- make CSV import preview header opaque ([#2942](https://github.com/TracecatHQ/tracecat/pull/2942) )
- reset table delete confirmation input on close ([#2944](https://github.com/TracecatHQ/tracecat/pull/2944) )
- hydrate stdio MCP env secrets for subagent configs ([#2945](https://github.com/TracecatHQ/tracecat/pull/2945) )
- harden MCP bridge discovery against silent tool loss ([#2946](https://github.com/TracecatHQ/tracecat/pull/2946) )
- size MCP scope token TTL to turn and re-mint on resume ([#2947](https://github.com/TracecatHQ/tracecat/pull/2947) )
- resolve top-level action results in subflow trigger_inputs inside scatter ([#2951](https://github.com/TracecatHQ/tracecat/pull/2951) )
- keep chat tool chips on one row with +N overflow ([#2954](https://github.com/TracecatHQ/tracecat/pull/2954) )
- align chat composer footer controls ([#2955](https://github.com/TracecatHQ/tracecat/pull/2955) )
- show tool limit message in chat tools picker ([#2956](https://github.com/TracecatHQ/tracecat/pull/2956) )
- prevent builder panel tabs and content clipping at narrow widths ([#2958](https://github.com/TracecatHQ/tracecat/pull/2958) )
- reset insert-row form when the dialog closes ([#2964](https://github.com/TracecatHQ/tracecat/pull/2964) )
- return 409 when creating a unique index on duplicate values ([#2965](https://github.com/TracecatHQ/tracecat/pull/2965) )
- make tier dialogs scrollable ([#2969](https://github.com/TracecatHQ/tracecat/pull/2969) )
- improve MCP connection option dark mode ([#2982](https://github.com/TracecatHQ/tracecat/pull/2982) )
- add pointer cursor to clickable table rows ([#3000](https://github.com/TracecatHQ/tracecat/pull/3000) )
- report delete errors and gate bulk delete by scope ([#3002](https://github.com/TracecatHQ/tracecat/pull/3002) )
- remove stacked bottom padding in action inspector panel ([#3013](https://github.com/TracecatHQ/tracecat/pull/3013) )
- avoid inherited edges when duplicating actions ([#3016](https://github.com/TracecatHQ/tracecat/pull/3016) )
- serialize attachment quota checks per case ([#3019](https://github.com/TracecatHQ/tracecat/pull/3019) )
- improve expression highlight contrast ([#3043](https://github.com/TracecatHQ/tracecat/pull/3043) )
- remove Workspace sidebar slide animation ([#3044](https://github.com/TracecatHQ/tracecat/pull/3044) )
- correct Okta private key placeholder ([#3055](https://github.com/TracecatHQ/tracecat/pull/3055) )
- allow org service accounts to manage settings ([#3056](https://github.com/TracecatHQ/tracecat/pull/3056) )
- simplify workflow execution logs ([#3067](https://github.com/TracecatHQ/tracecat/pull/3067) )
- register refresh_token grant and request offline_a… ([#3069](https://github.com/TracecatHQ/tracecat/pull/3069) )
- use pointer cursor on workflow execution rows ([#3070](https://github.com/TracecatHQ/tracecat/pull/3070) )
- fix Okta SAML audience restriction ([#3079](https://github.com/TracecatHQ/tracecat/pull/3079) )
- keep pull actions visible after preview ([#3086](https://github.com/TracecatHQ/tracecat/pull/3086) )
- use pointer cursor on dropdown menu items ([#3089](https://github.com/TracecatHQ/tracecat/pull/3089) )
- keep dialogs within viewport ([#3095](https://github.com/TracecatHQ/tracecat/pull/3095) )
- improve linked rows empty state ([#3096](https://github.com/TracecatHQ/tracecat/pull/3096) )
- show all workflow run payload streams ([#3114](https://github.com/TracecatHQ/tracecat/pull/3114) )
- keep pull actions visible for large previews ([#3118](https://github.com/TracecatHQ/tracecat/pull/3118) )
- emit terminal stream END only after finalize_turn ([#3120](https://github.com/TracecatHQ/tracecat/pull/3120) )
- render workspace sync toast as external link ([#3123](https://github.com/TracecatHQ/tracecat/pull/3123) )
- surface lookup errors ([#3124](https://github.com/TracecatHQ/tracecat/pull/3124) )
- calibrate notification banners for dark mode ([#3127](https://github.com/TracecatHQ/tracecat/pull/3127) )
- isolate bypass-RLS sessions on a dedicated pool ([#3141](https://github.com/TracecatHQ/tracecat/pull/3141) )
- ensure valid payloads and resource attribution ([#3143](https://github.com/TracecatHQ/tracecat/pull/3143) )
- support multi-replica sessions ([#3151](https://github.com/TracecatHQ/tracecat/pull/3151) )
- keep settings modal content within dialog width ([#3209](https://github.com/TracecatHQ/tracecat/pull/3209) )


## Infrastructure


- remove Kubernetes submodule ([#3109](https://github.com/TracecatHQ/tracecat/pull/3109) )
- expose concurrency controls in deployments ([#3153](https://github.com/TracecatHQ/tracecat/pull/3153) )
- remove experimental pool backend ([#3167](https://github.com/TracecatHQ/tracecat/pull/3167) )


## Documentation


- consolidate TLS and certificate guidance for self-hosting ([#2950](https://github.com/TracecatHQ/tracecat/pull/2950) )
- recommend batched DB writes ([#2995](https://github.com/TracecatHQ/tracecat/pull/2995) )
- point Google Cloud SecOps docs link at the SecOps server ([#3029](https://github.com/TracecatHQ/tracecat/pull/3029) )
- add API reference and reorganize core actions ([#3032](https://github.com/TracecatHQ/tracecat/pull/3032) )
- add audit log documentation ([#3036](https://github.com/TracecatHQ/tracecat/pull/3036) )
- discourage untyped dictionaries ([#3068](https://github.com/TracecatHQ/tracecat/pull/3068) )
- clarify case Markdown and agent MCP support ([#3087](https://github.com/TracecatHQ/tracecat/pull/3087) )
- prefer dataclass slots and typed attribute access ([#3103](https://github.com/TracecatHQ/tracecat/pull/3103) )
- mark pool backend experimental, auto selects ephemeral ([#3146](https://github.com/TracecatHQ/tracecat/pull/3146) )
- explain Helm application updates ([#3155](https://github.com/TracecatHQ/tracecat/pull/3155) )
- prefer frozen slotted dataclass over NamedTuple ([#3160](https://github.com/TracecatHQ/tracecat/pull/3160) )
- require LOC breakdown table in PR bodies ([#3161](https://github.com/TracecatHQ/tracecat/pull/3161) )
- Add secrets and OAuth info to http section ([#3206](https://github.com/TracecatHQ/tracecat/pull/3206) )


## Dependencies


- pin Python dependency versions ([#3175](https://github.com/TracecatHQ/tracecat/pull/3175) )


## Build system


- remove unused entrypoint.sh migration wrapper ([#3147](https://github.com/TracecatHQ/tracecat/pull/3147) )


## Other improvements


- polyfill ResizeObserver in jsdom setup ([#2961](https://github.com/TracecatHQ/tracecat/pull/2961) )
- add workspace export diagnostics ([#3058](https://github.com/TracecatHQ/tracecat/pull/3058) )
- prefer fd for file searches ([#3092](https://github.com/TracecatHQ/tracecat/pull/3092) )
- ignore pi subagent state ([#3106](https://github.com/TracecatHQ/tracecat/pull/3106) )
- cover successful router dispatch ([#3110](https://github.com/TracecatHQ/tracecat/pull/3110) )
- pin ruff to pyproject version in lint workflow ([#3121](https://github.com/TracecatHQ/tracecat/pull/3121) )
- remove broken OpenAPI template converter ([#3164](https://github.com/TracecatHQ/tracecat/pull/3164) )
- centralize grant scope validation ([#3222](https://github.com/TracecatHQ/tracecat/pull/3222) )
- extract shared tooltip line helper ([#3223](https://github.com/TracecatHQ/tracecat/pull/3223) )


**Full changelog** :[1.0.0-beta.50...1.0.0-beta.51](https://github.com/TracecatHQ/tracecat/compare/1.0.0-beta.50...1.0.0-beta.51)

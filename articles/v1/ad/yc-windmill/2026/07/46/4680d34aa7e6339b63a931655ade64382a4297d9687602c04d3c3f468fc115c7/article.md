---
schema_version: "1.0.0"
document_id: "4680d34aa7e6339b63a931655ade64382a4297d9687602c04d3c3f468fc115c7"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/asset-freshness"
published_at: "2026-07-04T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:9fad2685c67d03c2bfd9270e5fa2ee9e490a2ba77a07f644ce4a03ffc6990a2d"
---

# Asset freshness with fresh/stale badge and automatic watchdog

### [Asset freshness with fresh/stale badge and automatic watchdog](https://www.windmill.dev/changelog/asset-freshness)


Data pipelines


[Enterprise](https://www.windmill.dev/pricing)


[v1.748.0](https://github.com/windmill-labs/windmill/releases/tag/v1.748.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines#freshness)


The` // freshness <window>` annotation on pipeline scripts now shows a live fresh/stale verdict on the pipeline graph, emerald when the last successful run completed within the window and amber when it is stale or has never run. On Enterprise Edition, a freshness watchdog re-runs stale unpartitioned scripts automatically with exponential backoff capped at the window; watchdog runs skip the downstream cascade, are attributed to pseudo-user freshness- with the new freshness trigger kind, and can be disabled instance-wide with DISABLE_FRESHNESS_WATCHDOG.


#### New features


- Freshness badge on pipeline graph nodes: emerald when the last successful root run completed within the declared window, amber when stale or never run, with tooltips showing the last successful run
- Badges re-evaluate every 30 seconds on an open graph and turn fresh shortly after a successful run in the session
- EE freshness watchdog checks deployed pipeline scripts about once a minute and re-runs stale unpartitioned scripts as a backstop; any successful run resets the window
- Watchdog re-runs back off exponentially (capped at the window) while a script stays stale, and skip scripts with a run already queued or running
- Watchdog runs never fire the downstream cascade and are attributed to pseudo-user freshness-<path> with the new freshness trigger kind, filterable on the Runs page
- DISABLE_FRESHNESS_WATCHDOG environment variable turns the watchdog off instance-wide

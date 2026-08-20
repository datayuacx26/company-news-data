---
schema_version: "1.0.0"
document_id: "6303441ab5a76cbf057afe048fdb99c314063f84be17239ba7786df7736769eb"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/nested-trigger-filters"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-13T06:03:56.274575+00:00"
fetched_at: "2026-08-13T06:03:57.406834+00:00"
content_hash: "sha256:d846e5ee092ecc96fcfd3263a007f9c467c17d3f587498d33a6e35a3a3b71aa6"
---

# Nested filter groups and dotted paths in trigger filters

### [Nested filter groups and dotted paths in trigger filters](https://www.windmill.dev/changelog/nested-trigger-filters)


Triggers


[Docs](https://www.windmill.dev/docs/triggers/websocket_triggers#filter-groups)


Kafka and WebSocket trigger filters were a flat list combined by a single AND or OR, so a condition like "A and B and (C or D)" could not be expressed. An entry can now be a group with its own logic (any_of, all_of, none_of) nesting further entries, groups included, up to three levels in the editor. A criterion also addresses its field by a dotted path into nested objects (account.id) instead of only by a top-level key, switched with a Key/Path toggle. Existing flat key/value filters keep matching exactly what they matched before and nothing needs migrating; filters that don't parse are now rejected when the trigger is saved rather than silently widening it.


#### New features


- A filter entry can be a group - any_of (OR), all_of (AND) or none_of (NONE) - nesting further criteria and groups, so "A and B and (C or D)" is expressible
- Each criterion addresses its field by Key (a top-level field) or Path (a dotted path into nested objects, e.g. account.id), switched with a toggle; paths do not traverse arrays
- The editor nests groups up to three levels; a group left empty is dropped rather than evaluated, so its siblings stay in force
- NONE matches when none of its criteria match, and a field the message does not carry satisfies it
- Back-compatible: stored flat key/value filters parse and mean exactly what they did, and top-level entries are still combined by the trigger filter logic

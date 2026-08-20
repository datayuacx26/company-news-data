---
schema_version: "1.0.0"
document_id: "c32b9b35dc9516233823fde6d2d6047dfddbc5fc9549b1317fef5398c57d513c"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/week-of-march-23-2026"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T22:16:31.746019+00:00"
content_hash: "sha256:c006e0ff553ab231487e4617613656b115294d6873c4958e2f17aef5048d5670"
---

# Changelog - Week of March 23, 2026

What we shipped this week (Mar 23 - Mar 30):


-


The MCP server now supports` glob` and` list_tree` tools, letting MCP clients find files by pattern and browse directory structures in a codebase. ([#1014](https://github.com/sourcebot-dev/sourcebot/pull/1014) )


-


The file viewer now respects` .gitattributes` language hints, so files get the correct syntax highlighting when a repo overrides the detected language. ([#1048](https://github.com/sourcebot-dev/sourcebot/pull/1048) )


-


Search contexts can now be filtered by GitHub and GitLab topics, making it easier to scope searches to a specific group of repos. ([#1028](https://github.com/sourcebot-dev/sourcebot/pull/1028) )


-


Fixed auto-scroll in ask threads, regex queries with parentheses being split incorrectly, line numbers being accidentally selected alongside code in Safari, and GitLab sync cleaning repos on non-404 API errors.

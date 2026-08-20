---
schema_version: "1.0.0"
document_id: "b7eafdfb8cdd3aac09296f0d302a8ec139202d1e0c2a376e0f164aacb5c5d226"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/slite-agent-improvements"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:6c9eedd61c605a5aad4777b502f406e78d7ab02d2ee48498e5b27cd43fcfe4b5"
---

# Slite Agent improvements

**See pending approvals at a glance** — A new sidebar badge shows how many items are waiting on you, and you can jump straight to any one from its URL.


**More accurate diffs** — Style-only changes, like bolding, now render correctly so you can see exactly what changed.


**The agent can do more** — It can now rename docs, swap icons, and adjust layout width without touching the body. As always, these go through the standard approval flow.


**Know when activity came from an agent** — Comments, activity log and doc revisions now show if an activity originated from the Slite Agent, an MCP or the API.


### Public API: note ordering and HTML export options


Two additions to the public API:


- ` listPosition` is now exposed on the` Note` shape, so you can sort siblings the same way the sidebar does.` getNoteChildren` also accepts` orderBy` (` title` |` listPosition` |` lastEditedAt` ) and` orderDirection` (` asc` |` desc` ) query params.
- ` GET /v1/notes/{id}?format=html` now accepts a` css=inline|none` param. Use` none` to skip the ~2MB stylesheet when you supply your own CSS.


### Other improvements


- Mobile: several layout and rendering fixes across home, collections, and comments.
- Find in doc now searches inside synced tiles.
- Asana agent source fetching is now faster, paginated, and uses Asana's remote search API instead of local search.
- Digest answers now output up to twice as many tokens, reducing cases where long answers were cut off.
- Digests now send from a no-reply address to avoid reply confusion.


### Fixes


- MCP: archived collection rows are no longer returned in results.
- MCP: OAuth authorization server metadata now correctly works for Gemini Enterprise and similar clients.
- The approve button in the agent now works consistently.
- Comment mentions no longer send before the mention is completed when using Tab + Space to select a user.
- Comments on table cells now attach and display correctly on Mac.
- AI-generated directory descriptions no longer tend to be generated in Spanish
- Quick Improve with AI no longer throws an error when selecting text.
- Digest delivery fixed: no more duplicate sends or missed recipients after pausing and re-enabling.
- Agent history no longer shows a pending approval badge when no approvals are listed.
- MCP backtick code span spacing is now handled correctly, removing the auto-padding that was adding unwanted spaces around inline code.
- API note timestamps (` lastEditedAt` ,` updatedAt` ) now update correctly after edits.

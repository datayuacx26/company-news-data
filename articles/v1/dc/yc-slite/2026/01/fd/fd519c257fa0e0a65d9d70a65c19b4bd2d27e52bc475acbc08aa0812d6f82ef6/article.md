---
schema_version: "1.0.0"
document_id: "fd519c257fa0e0a65d9d70a65c19b4bd2d27e52bc475acbc08aa0812d6f82ef6"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/new-protected-docs-easier-to-read-tables-and-smarter-defaults"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:ba4b914ee5fa175df1d46168c3012848f6496c29deeb2d8586af0319461a3b7a"
---

# New: Protected docs, easier-to-read tables, and smarter defaults

As workspaces grow, keeping knowledge consistent, protected, and easy to manage becomes essential. This update focuses on stronger document protection, clearer defaults, and smoother collaboration across your workspace.


### Protect docs and their sub-docs


You can now apply protected editing to a doc and automatically extend it to all of its sub-docs.


- Choose to protect a single doc or a doc and all its children
- Prevent accidental edits across documentation hierarchies
- Useful for shared, long-lived, or critical docs


### Default channels for your workspace


You can now define which channels appear by default in the sidebar.


- Set and order default channels
- New members see these channels automatically
- Defaults reappear if a user clears their sidebar
- Personal sidebar preferences still take priority


### Freeze first column in tables


You can now freeze the first column in tables.


- Keep key information visible while scrolling
- Makes large or complex tables easier to read and edit


### Super, now directly inside docs


When Super is enabled, it replaces the basic “Ask in doc” feature inside Slite docs.


- Ask questions directly from a doc using Super
- Search across 15+ connected tools, not just Slite
- Use broader context to validate and proofread documentation


### Templates are protected by default


Templates opened from the template modal are now locked using protected editing.


- Prevents accidental or unnoticed changes to templates
- Editing a template now requires explicitly disabling protected mode
- Ensures templates remain consistent over time


### Slite Public API updates


New API capabilities requested by enterprise customers:


- ` GET /notes?ownerId=` — List notes filtered by owner
- ` GET /users?query=` — Retrieve users and their IDs for ownership assignment


Full documentation is available at developers.slite.com.


### Slite MCP server 1.1.0


The MCP server has been updated with expanded capabilities:


- Updated OpenAPI schema aligned with the latest Slite API
- ` search-notes` now supports additional parameters:
- ` parentNoteId`
- ` reviewState`
- ` page`
- ` hitsPerPage`
- ` lastEditedAfter`
- ` includeArchived`
- ` ask-slite` now supports` parentNoteId` for scoped queries
- ` get-note` supports a` format` parameter (Markdown or HTML)
- ` get-note-children` now supports pagination via cursor


### Bug fixes


We shipped a wide range of fixes and improvements across the app, including:


- Fixed Slack integration issues where doc previews appeared twice
- Fixed visual glitches with table handles
- Fixed editor text overlap issues
- Improved handling when moving large parent docs
- Fixed Slack bot downtime issues
- Fixed API documentation visual issues
- Fixed multiple mobile app issues (versions 2.12.0–2.12.3)
- Fixed edge cases and UX issues with collapsible sections
- Improved performance on large documents

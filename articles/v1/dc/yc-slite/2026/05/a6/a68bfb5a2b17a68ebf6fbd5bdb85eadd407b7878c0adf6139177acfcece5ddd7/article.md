---
schema_version: "1.0.0"
document_id: "a68bfb5a2b17a68ebf6fbd5bdb85eadd407b7878c0adf6139177acfcece5ddd7"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/new-boxed-layout"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:016e65c625ca52fa70255b978ded0811905e44ff0a710a794b45e4a7023e64c7"
---

# New boxed layout

You may have noticed something feels different in Slite. The main content area now sits in a white box surrounded by lighter sidebars, giving Slite a cleaner, more focused look. Search and Ask are now regular sidebar row items, a small change that sets the stage for bigger things coming in June. The layout adapts cleanly to mobile, and the sidebar shell has been unified across Slite and Super so both products share the same design primitives going forward.


### Comments redesigned


- Comments now appear in document order.
- A new **Unread** tab makes it easy to catch up.
- Notification emails now show the highlighted text that was commented on.
- If you @-mention someone who doesn't have access to the doc, you'll get a warning before it goes out.
- And you can now create, resolve, reply to comments through our MCP.


### More MCP tools: comment creation and channel management


Building on last cycle's comment reading and resolving, the MCP now supports the full comment workflow:


- **Create and reply to comment threads** directly via MCP. Threads can be left global or anchored inline to a specific span of text, just like in the editor.
- **Two new channel tools:**` create-channel` (public, private, or read-only, with an optional icon) and` update-channel` (rename, change icon, or update visibility).


### Other improvements


- Member CSV export now includes User Groups and Super Seat columns for easier team audits.
- Admins can now skip sending notifications when sharing a doc, a toggle in the share role selector lets you silently add someone without pinging them.
- The MCP now accepts icon shapes and colors in` create-note` and` update-note` , so agents can set custom icons without opening the UI.
- Exporting untitled docs to Markdown no longer creates a hidden` .md` file, it now exports as` untitled.md` .
- Markdown backup ZIP files now emit valid YAML front matter (the opening` ---` delimiter was previously missing).


### Bug fixes


- Fixed a crash when dragging content below an empty collapsed heading.
- Fixed cut/paste not working correctly between table cells for date and special cell types.
- Fixed links and doc cards breaking when copy-pasted between docs.
- Fixed Cmd+click in the search modal loading a doc twice.
- Fixed a backspace crash when a heading followed a table inside a collapsible block.
- Fixed the mobile toolbar not being able to insert Hints or Quotes.
- Fixed collapsed heading icons not being visible due to a CSS specificity regression.
- Fixed resolving a comment switching the sidebar to the Resolved pane unexpectedly.
- Fixed the seats banner showing "0 Readers" on plans that don't include the reader role.
- Fixed the right sidebar switching unexpectedly when a global comment was focused.
- Fixed corrupted collection cells caused by stale local operations being re-submitted.

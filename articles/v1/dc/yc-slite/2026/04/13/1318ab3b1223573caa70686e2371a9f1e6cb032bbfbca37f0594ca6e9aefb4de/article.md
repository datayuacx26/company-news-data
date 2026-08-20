---
schema_version: "1.0.0"
document_id: "1318ab3b1223573caa70686e2371a9f1e6cb032bbfbca37f0594ca6e9aefb4de"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/mcp-read-and-resolve-comment-threads"
published_at: "2026-04-23T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:ffa0d3e81230d3694e87c18faa3ed89f3f736e061b1874fd90f0c91babe93714"
---

# MCP: Read and resolve comment threads

Slite's MCP integration now exposes comment threads on documents. Any MCP client — Claude, Cursor, your own agent — can now:


- **List all threads** on a note, with full comment history, participants, and resolved state
- **Fetch a specific thread** by ID, or by note + thread ID together
- **Resolve or unresolve** a thread programmatically


This opens up powerful agentic workflows: an AI can now read the discussion context on a design doc, triage open threads, or mark resolved ones as done — without any human having to open the document first.


### Find in Collections


Cmd+F now matches text inside collection cells — for both embedded collections and standalone collection docs. Results are interleaved with regular text matches in document order, so next/previous navigation walks the full page top-to-bottom. Note: Replace is text-only; collection matches are navigated but not replaceable.


### Other improvements


- **Visual polish across the editor:** inline code has a higher border radius, hints have a softer style with a light border, and tabs components have been redesigned. Links inside inline comments are now clickable, and deleted content in comments shows strikethrough.
- **Better comment experience:** Comment layout is more dense, the inline mark for resolved comments is now visible in the editor, and the resolved/open tab bar is now sticky.
- **Subdoc creation on shared docs:** users with write access on a "Shared with me" doc can now create subdocuments directly from that doc, without needing access to the parent channel.
- **Billing: incomplete subscription banner:** if your workspace has an incomplete Stripe subscription, a banner now appears on the billing screen with a direct "Complete payment" action.
- **Verification events now appear in doc history:** Verification changes now show up in the document history timeline, so you can see exactly when a doc was confirmed accurate and by whom.


### Fixes


- Long team or user group names now truncate with an ellipsis in the verification owner menu and the Knowledge Management Panel owner filter — no more overflow.
- The public share settings panel now stays open when a document is already publicly shared, so the sharing state is always visible.
- Going back after a search no longer re-triggers navigation to the same search query.
- Links opened from the in-app changelog now open in a new tab on the desktop app.

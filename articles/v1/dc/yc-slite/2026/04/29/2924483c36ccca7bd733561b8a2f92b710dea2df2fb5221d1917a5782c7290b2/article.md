---
schema_version: "1.0.0"
document_id: "2924483c36ccca7bd733561b8a2f92b710dea2df2fb5221d1917a5782c7290b2"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/multi-column-layouts"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:97bf40536126bf8dde97575713308840b58ab2bda6f6c056b93413777152c766"
---

# Multi-Column Layouts

You can now place content side by side in your docs. Type` /column` to pick up to 4 columns, or drag existing blocks next to each other — no command needed. Columns are great for comparing options, building compact summaries, or giving long documents a more structured and scannable layout. Layouts adapt to mobile viewports too, so your docs look great wherever your team reads them.


[Read more about how to use columns in Slite](https://slite.slite.com/app/docs/FQATpcFmSwW0I2)


### Knowledge Management Panel


The Knowledge Management Panel has been completely rewritten from the ground up. You can now:


- **Export any view as a CSV** — perfect for large audits where you need to divide work across a team.
- Sort by **last activity** and **view count** is now available, so you can surface stale or high-traffic docs at a glance.
- Collections are now included in the panel, selected channels and users appear at the top of filters for faster navigation, and the overall responsiveness of the list has been significantly improved.


The Knowledge Management Panel is also now accessible via the[Public API](https://developers.slite.com/) and[MCP](https://slite.slite.page/p/77mvFqJWG1tduF/Slite-MCP) , letting you build automations on top of your knowledge health data.


### Inline LaTeX Formulas


You can now use inline formulas in docs using the` /formula inline` command. This is a long-requested feature for technical documentation, easily mix text and math in the same line. Exports now include inline formulas too.


### Improvements


- Faster doc switching — switching between documents is up to ~4× faster (from ~100ms to ~25ms)
- Remote cursor reliability improved — cursors no longer fade out prematurely
- Improved formula UX:` $...$` markdown shortcut for inline formulas, with fluid keyboard navigation


### Fixes


- Collections now appear in the Knowledge Management Panel
- Billing page improvements: renewal date no longer shown after cancellation, past invoices fully accessible, and switching from monthly to yearly plan is possible again
- Fixed the AI editor shortcut on Windows (` Ctrl+Shift+I` )
- Search now expands collapsed blocks when a match is found inside them
- Cut/paste works from inside collapsible sections
- Comment popover no longer gets cut off at the end of a doc
- Replying to inline table comments now works reliably
- Copy-pasting images between notes no longer shows broken images before reload
- Doc card cover images now render correctly
- Using the "New doc" command in the editor no longer triggers a blue error banner
- Deleting archived docs now works correctly with proper permission checks
- API markdown updates no longer remove tables from documents
- Image captions now persist after using` modify-block` via MCP
- Ask no longer ignores the` parentNoteId` filter
- MCP update-note tool no longer destroys collection data when replacing note content
- Note title updates via API are now truncated to 255 characters
- Mentions no longer disappear during heavy collaborative editing
- User list scroll position no longer resets in Member Management

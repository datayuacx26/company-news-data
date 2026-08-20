---
schema_version: "1.0.0"
document_id: "91b91c316f66a745fd149107f9c99a63b0f50c255b8737d263acc07431c8625d"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/see-every-change-and-know-who-made-it"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:7f8496812f95ac0fd23c541237e61c27985b7d8fbe2aa4099551a8834e411539"
---

# See every change, and know who made it

The doc history view has been completely reworked. Changes are now highlighted at the block level and down to individual words, with clear attribution to the person or agent, who made each edit. A minimap lets you jump to any section of a long doc without scrolling, and the footer has been simplified into a floating bar. The history slider now correctly starts at the document's initial state.


### Super is now inside Slite


If your workspace has both Slite and Super, you can now use Super directly from the Slite sidebar, no switching apps. Ask questions, search across all your connected tools, and get answers without leaving your docs.


- **Slite + Super customers** : Super is now fully accessible inside the Slite app.
- **Slite-only customers** : Nothing changes for you.
- **Super-only customers** : You'll notice some small UX and naming updates, but nothing major.


Not on Super yet? As a Slite customer, you get a discounted plan.[Book a demo →](https://super.work/book-demo)


### MCP: hybrid format for everyone, plus wide layout support


Hybrid SliteML is now live for all users. Slite MCP now accepts operations in either standard Markdown or our rich SliteML format, adapting intelligently to each — so AI models can write in whichever they prefer, and Slite handles both. Token usage dropped significantly since the MCP only returns edited ranges, and a new` read_range` tool lets agents fetch just what they need. Tables work better, and` create-note` /` update-note` now support` contentWidth` for wide layouts.


[See what's possible →](https://slite.slite.page/p/77mvFqJWG1tduF/Slite-MCP)


### Smarter admin: better exports and silent sharing


- CSV exports now include` Has Super Seat` and` User Group` .
- You can share a doc without notifying recipients.
- **SCIM provisioning is now available for all Enterprise plan customers.** Starting with Okta, with Azure AD and Google Workspace support coming. New hires get access automatically, role changes sync, and leavers lose access the moment they're removed from your IdP. No more manual seat management.[Reach out to get enabled.](https://join.slack.com/t/slitebetacommunity/shared_invite/zt-2f38u5l4w-iYR2H_u4uiw4G982VjLxEA)


### Other improvements


- Faster document switching — up to ~4× faster (from ~100ms to ~25ms) (shipped with the April 9 cycle, now fully rolled out)
- ` create-note` no longer prompts agents to repeat the document title as an H1 heading in the body


### Fixes


- Fixed a crash when deleting rows from a large table containing images
- Fixed document content appearing to disappear after moving a page between channels
- Fixed scrolling from the scrollbar being blocked when the right sidebar was open
- Fixed clicking between collapsible sections jumping the document back to the top
- Fixed the Linear integration hitting rate limits when embedding Linear issues in docs
- Fixed an authorization error that prevented adding the Slite MCP to ChatGPT

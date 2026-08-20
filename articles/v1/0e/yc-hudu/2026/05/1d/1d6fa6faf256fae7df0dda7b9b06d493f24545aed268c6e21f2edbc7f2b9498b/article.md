---
schema_version: "1.0.0"
document_id: "1d6fa6faf256fae7df0dda7b9b06d493f24545aed268c6e21f2edbc7f2b9498b"
company_key: "yc-hudu"
company: "Hudu"
source_id: "yc-hudu-news-import-7b5cd22111cb"
canonical_url: "https://www.hudu.com/blog/release-update-2.43.0"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-21T23:17:38.967395+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:e71a12a59ac554f88cab20fc2b2fa30e6ab94cbda6f8c0a72b27ee032bfa575c"
---

# Release: Hudu 2.43.0

Hudu v2.43.0 is now live. This release is about bringing Hudu's surface area together — Radar moves into the core app, MCP expands beyond a single AI tool, and diagrams land natively in the editor. The platform is getting more connected.


Self-hosted environments must make a change to the default.conf file prior to updating. Updates to cloud-hosted environments will begin today.


### Highlights


**Hudu Radar centralized management**


Radar is now accessible directly from the Hudu core web app — no separate tool, no context switching.


Open Radar from within licensed companies to scan, document, and monitor networks and devices right alongside your documentation. For teams that rely on Radar as part of their workflow, having it embedded in the core app means fewer tabs, faster handoffs, and scan results that live where your documentation already does.


**Note:** Improved Hudu Radar WMI scanning and logging is also included in this release.


**Mermaid Diagrams**


You can now create flow charts, architecture diagrams, sequence diagrams, and more directly inside any rich text field in Hudu.


Mermaid diagrams are built with code — insert a code block, select Mermaid as the language, and the diagram renders inline. That means your network topology, onboarding flow, or escalation path can live directly inside a KB article or asset record, not as an attached file or an image that goes stale. Browse Mermaid's open-source library[here](https://mermaid.ai/open-source/syntax/examples.html) for diagram ideas and syntax.


**Tip:** Insert a code block in any rich text editor and select Mermaid as the language to get started.


**Hudu MCP Server upgrade**


The Hudu MCP Server has been upgraded from SSE to Streamable HTTP transport, opening up connections to a broader set of AI tools.


Previously, the MCP server worked with a limited set of clients. Streamable HTTP is the current standard for MCP, which means Hudu now works with Cursor, Claude, VS Code, Microsoft Copilot, and any other tool that supports the protocol. If you use an AI assistant in your daily workflow, you can now connect it to Hudu and let it read and write your documentation directly.


**Action required:** If you have previously configured MCP clients, you'll need to update them to use the new server URL. Find it in Admin → External Apps → Hudu MCP.


### Improvements


- Show/hide completed process runs
- Duplicate a global process template
- Bulk actions for processes (delete and archive)
- Bulk actions for process tasks and subtasks (delete, mark as optional/required, mark as subtask of)
- Sort processes by name, date created, recently modified, and number of runs
- Expirations listed in chronological order in alert emails
- Improved feedback when passwords on the forgot password page do not match
- Assets API endpoint performance improvements
- Improved Hudu Radar WMI scanning and logging


### Bug fixes


- Editors and authors unable to bulk delete My Vault passwords
- Password folders sorting incorrectly
- List select dropdowns using alphabetical order instead of respecting manual list item order
- Selectable area for allow list/deny list in group settings was too large
- Long company names did not wrap correctly in related items UI
- Page auto-scrolling to top after a task is edited
- Breadcrumb padding inconsistencies
- Asset layout description caused table to be hidden on mobile
- Newly added heading fields not displaying on assets until edited or created
- Certain companies not receiving documentation quality updates
- Minor bug fixes and performance improvements


## Stay Up to Date on Hudu News


Subscribe to this blog below to get Hudu news and information delivered to your inbox. We will update you on major releases, events like our product and partner webinars, and more.

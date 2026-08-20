---
schema_version: "1.0.0"
document_id: "59f7d905e106636a562d1ea86c8f324e2eb4088899a016ce3c3f2922e3b7045d"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-07-16"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:3052e291c771cab20ff44f427bde812e3e958c7824f0d365884bdead2784d93e"
---

# Project settings get a new home, @ mentions, Trash, & smarter schedules

## [July 16, 2026](https://deepnote.com/changelog/2026-07-16)


#


[Project settings get a new home, @ mentions, Trash, & smarter schedules](https://deepnote.com/changelog/2026-07-16#project-settings-get-a-new-home--mentions-trash--smarter-schedules)


##


[A new home for project settings](https://deepnote.com/changelog/2026-07-16#a-new-home-for-project-settings)


Project settings have moved, here's where to find what.


**Before:** project files, integrations, and the machine picker lived in the left side panel, always visible.


**Now:** they live in a panel on the right, which opens when you click **Settings** in the top right of the screen. The same panel includes a new **Triggers** section, which exposes ways to run a notebook automatically or remotely.


The goal is less clutter in the default view, so notebooks stay front and center.


##


[Self-pausing schedules](https://deepnote.com/changelog/2026-07-16#self-pausing-schedules)


Scheduled notebooks can now pause themselves after a configurable number of consecutive failures, instead of failing forever and burning compute. It's on by default when you create a new schedule.


##


[EU-hosted OpenAI models for Enterprise](https://deepnote.com/changelog/2026-07-16#eu-hosted-openai-models-for-enterprise)


Enterprise customers can now use OpenAI models hosted in the EU, served through Azure's Sweden Central region. If your compliance requirements mean AI processing has to stay in the EU, this one's for you.


##


[@ mentions in notebooks](https://deepnote.com/changelog/2026-07-16#-mentions-in-notebooks)


You can now mention integrations and projects directly in text blocks. Type **@** in a paragraph, list, or callout and a picker opens — with typeahead, so you can find the right resource fast.


- Project chips link straight to that project.
- If a mentioned resource gets removed, its chip shows in red.
- Every chip carries the resource ID, so AI agents know unambiguously which integration or project you're referring to.


##


[Trash folder](https://deepnote.com/changelog/2026-07-16#trash-folder)


When you accidentally delete a project, you no longer have to reach out to support. Just restore it from **Trash** .


##


[API & MCP updates](https://deepnote.com/changelog/2026-07-16#api--mcp-updates)


- **Duplicate notebooks programmatically** — a new` POST /notebooks/{notebookId}/duplicate` endpoint in the v2 API, with a matching` duplicate_notebook` MCP tool.
- **Link generation for agents** — a new` generate_project_url` MCP tool lets agents generate URLs for Deepnote projects and notebooks, so they can hand you a link to what they've built.

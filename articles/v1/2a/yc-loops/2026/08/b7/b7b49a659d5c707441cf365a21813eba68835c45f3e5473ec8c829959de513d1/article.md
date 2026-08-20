---
schema_version: "1.0.0"
document_id: "b7b49a659d5c707441cf365a21813eba68835c45f3e5473ec8c829959de513d1"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/workflows-api-claude-connector"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T19:31:20.180066+00:00"
fetched_at: "2026-08-12T19:31:20.861762+00:00"
content_hash: "sha256:c1b5f4389e21883b21e8677d6d202ea011cf9e22ad4fe5bb165712f6d02ef6ae"
---

# Workflows API, Claude Connector, and conditional sections

The Workflows API is now fully available. Build and update complete Workflows from code or an agent, then review and publish the same Workflow in Loops.


This release also adds the official Claude connector, conditional email sections, revision-safe Workflow writes, new API endpoints, SDK updates, and editor improvements.


## Workflows API


Create, manage, and update full Workflows through the API. You can audit an existing Workflow, respond to changes in activation or revenue, or build experiments with multiple branches without rebuilding the graph by hand in the editor.


See the endpoints and request formats in the[Workflows API documentation.](https://loops.so/docs/api-reference/list-workflows)


## Official Claude Connector


Connect Loops to Claude through MCP and work with your Loops account from a conversation. The connector includes the Workflows API alongside the rest of the platform tools exposed to agents.


Open the connector from the icon in the top-right of the Loops editor, or[connect Loops from Claude.](https://claude.ai/directory/connectors/loops)


## Conditional sections


Show or hide an email section based on a contact property, event property, or transactional data. Choose the condition in the style panel and preview the result before sending.


-


Show usage stats only when a contact has activity.


-


Tailor plan guidance to a contact property.


-


Add context from the event that triggered a Loop.


-


Leave out sections when the source data is empty.


Conditional sections support filters including equals, contains, greater than, and is empty.[Read the conditional sections guide.](https://loops.so/docs/creating-emails/editor/sections#conditional-sections)


## Revision-safe Workflow editing


Workflow writes now include the revision last read. If the Workflow changed in the meantime, Loops rejects the stale write instead of overwriting newer work. That makes handoffs between teammates and agents safer.


## More in this release


-


Create audience segments through the API.


-


Create and update components and themes through the API.


-


Updated JavaScript, Ruby, PHP, and Go SDKs for the new endpoints.


-


New Polar integration.


-


Faster Audience page load times.


-


Per-column background, corner radius, padding, and vertical alignment controls.


-


Expanded API reference generated from the OpenAPI specification, including Workflow and webhook endpoints.

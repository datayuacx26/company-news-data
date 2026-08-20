---
schema_version: "1.0.0"
document_id: "c40bf456e6309237962f2b706d85cefab5e8ca3c8331f2c9e47e9d7a46d6d9f8"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/the-pulse-mcp-server-is-now-live"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-24T09:39:59.698897+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:20f30318462fbe8e119b77aefaf4bb76d86feedb6ba85b19da0ef1652f9cd3c1"
---

# The Pulse MCP Server Is Now Live

The Pulse MCP server is live today. Point any MCP client at Pulse, whether that's Claude, Codex, VS Code, or one you've built yourself, and your agent can call extraction, schema generation, splitting, and table parsing directly as tools, deciding for itself which steps a given document needs.


## **How it works**


The Model Context Protocol is an open standard that lets agents discover and call external tools, and the Pulse MCP server currently exposes eight of them:


- **extract** : parses a document at a URL, or a file on disk when running locally, into clean markdown, with optional HTML, figure descriptions, and chunking.
- **apply_schema** : applies a JSON schema to an extraction and returns structured fields.
- **generate_schema** : drafts or refines that schema from a plain-English description of the fields you want.
- **split_document** : segments an extraction into topic-based page ranges, so the agent works on the three pages that matter rather than all two hundred.
- **extract_tables** : pulls tables out as HTML or markdown, merges tables that span pages, and converts charts into tables.
- **batch_extract** : runs many documents in a single asynchronous batch, from either a list of URLs or an S3 prefix.
- **run_pipeline** : executes a saved multi-step Pulse pipeline on a document.
- **get_job** : polls any asynchronous job for its status and result.


## **Hosted or local**


There are two ways to run the server, and the tools are identical either way. The hosted option requires nothing to install: simply point your client at https://mcp.runpulse.com/mcp over Streamable HTTP. The local option runs uvx pulse-mcp over stdio, which gives you the same set of tools with the added ability to extract local files straight from disk. Full setup instructions live at[https://docs.runpulse.com/mcp/overview](https://docs.runpulse.com/mcp/overview) .


## **When to reach for it**


The MCP server is built for working inside an agent or chat client and having it read and structure documents on demand, letting the agent decide the sequence of steps rather than following a fixed path. When the goal is instead a deterministic pipeline or a scheduled batch job that you control in your own code, the Python and TypeScript SDKs and the REST API remain the better fit, since they give you precise and repeatable control over every step.


## **What this means**


An agent is only as useful as its ability to read the documents in front of it accurately, and that has long been the missing piece for document-heavy workflows. Connected to Pulse over MCP, agents can now explore and understand even the most complex documents as a native part of how they work.


The Pulse MCP server is live today. Create an API key, point your client at it, and ask your agent to read your hardest document.

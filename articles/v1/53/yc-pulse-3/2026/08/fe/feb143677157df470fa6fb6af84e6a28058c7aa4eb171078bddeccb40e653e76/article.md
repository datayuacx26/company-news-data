---
schema_version: "1.0.0"
document_id: "feb143677157df470fa6fb6af84e6a28058c7aa4eb171078bddeccb40e653e76"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/pulse-is-now-on-claude"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T02:33:37.046808+00:00"
fetched_at: "2026-08-14T02:33:38.709079+00:00"
content_hash: "sha256:0ff8433371cfaddee0bc5b261701f65c6d7c1ee419fa5d3f3f61ca2c84e0d032"
---

# Pulse is now on Claude

Pulse is now available as a connector in Claude's directory. Point Claude at any document and get back clean, structured data, without leaving the conversation.


## What the Pulse connector does


Once connected, Claude can call six things directly through Pulse:


**Extract.** Point Claude at any document by URL and get back clean markdown, tables, and JSON. No copy-paste, no manual transcription.


**Extract tables.** Pull tables out of a completed extraction, including merge-across-pages for tables that span multiple sheets, and chart-to-table conversion for documents where the data only exists as a chart image.


**Split document.** Break a long, completed extraction into topic-based page ranges you define, useful for pulling just the relevant section out of a large filing or spec sheet.


**Apply schema.** Run a JSON schema against a completed extraction (or a split of one) and get back structured field values, with citations back to where each value came from in the source document. You can write the schema yourself or have Pulse generate one.


**Batch extract.** Run extraction across many files at once, either from a list of URLs or an S3 prefix, with results written back to S3 or a local path.


**Run pipeline.** Execute a saved, multi-step extraction pipeline against a file. Pipelines themselves are authored in the Pulse dashboard; the connector runs the ones you've already built.


## Why this matters


Every one of Pulse's six capabilities maps to a real step in the same underlying workflow: get structured data out of a document, shape that data to match the fields you actually care about, and do it across one file or thousands. Making that available inside Claude means a team's data extraction step no longer has to be a detour through a separate dashboard. It can happen in the same conversation where someone is already asking the question.


## Get started


1. Open Claude and go to the Connectors directory ([LINK HERE](https://claude.ai/directory/connectors/pulse-mcp) ).
2. Search for Pulse and connect your account, this is an OAuth flow through Pulse, no manual API keys.
3. Point Claude at a document by URL and ask it to extract, apply a schema, or run a saved pipeline.
4. Approve each tool call the first time; after that you can choose to always allow it.

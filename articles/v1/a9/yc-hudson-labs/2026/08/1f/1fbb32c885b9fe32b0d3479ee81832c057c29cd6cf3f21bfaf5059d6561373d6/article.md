---
schema_version: "1.0.0"
document_id: "1fbb32c885b9fe32b0d3479ee81832c057c29cd6cf3f21bfaf5059d6561373d6"
company_key: "yc-hudson-labs"
company: "Hudson Labs (formerly Bedrock AI)"
source_id: "yc-hudson-labs-news-import-06180ca7b218"
canonical_url: "https://www.hudson-labs.com/blog/mcp-in-claude"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-19T04:38:33.740738+00:00"
fetched_at: "2026-08-19T04:38:35.621511+00:00"
content_hash: "sha256:31a3bd3cd78b1855a6cb99e5fb6a5316538383b613ed86b89e207ee63c7ffeba"
---

# Hudson Labs MCP in Claude

How to connect Hudson Labs to Claude, and what happens once it is on.


## How to setup


1. In Claude, open settings and go to connectors.
2. Choose Add custom connector.


1. Name it Hudson Labs Co-Analyst.
2. Paste the URL:` https://platform.hudson-labs.com/api/mcp`


1. This will pop up a web browser and bring you to an approval page on Hudson Labs. Click Approve, and it will re-route you back to Claude.


1. Once re-directed to Claude, Co-Analyst should already be active. You can verify this by look at the connectors drop-down.


## How it works


Once connected, Claude calls Hudson Labs when you ask about a public company, or when asked to use Co-Analyst. Instead of answering from memory, it reads the 10-Ks, 10-Qs and 8-Ks and returns the disclosed language with a link back to the filing, or initiates market-wide AI search’s.


It can also screen for companies using the Forensic Risk Score and category details.


Requests run against your existing Hudson Labs account, so coverage and limits are the same as the platform. Nothing changes about how you use the platform itself.


## Availability


Institutional plans only.


## Notes


- The connector URL must be https. Claude reaches the server from Anthropic's cloud over the public internet, not from the user's device.
- Worth confirming the URL authenticates from a fresh Claude account before this goes to customers.

---
schema_version: "1.0.0"
document_id: "62f0c8725ca133e780d7ba2309de15e0f581bf3d76a28a8a5f0a0f083c87c176"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/buildbuddy-mcp-server"
published_at: "2026-04-15T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:e3ce101ae06fc92877dc5cb39871312068906101008271589db7099f3147b0ee"
---

# BuildBuddy MCP server

BuildBuddy now exposes an MCP endpoint for coding agents and other MCP-capable clients.


MCP provides a way to let agents explore BuildBuddy data for mostly read-only workflows like build and test metadata, logs, artifacts, target statuses, and more.


You can connect agents like Codex, Claude Code, and Gemini CLI to:


```text
https://YOUR-BUILDBUDDY-ORGANIZATION.buildbuddy.io/mcp
```


Authenticate with:


```text
Authorization: Bearer <api-key>
```


See the new[BuildBuddy MCP Server docs](https://www.buildbuddy.io/docs/enterprise-mcp) for setup instructions and configuration examples.

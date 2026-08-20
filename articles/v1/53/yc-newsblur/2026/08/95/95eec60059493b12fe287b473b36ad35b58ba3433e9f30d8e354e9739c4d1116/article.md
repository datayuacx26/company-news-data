---
schema_version: "1.0.0"
document_id: "95eec60059493b12fe287b473b36ad35b58ba3433e9f30d8e354e9739c4d1116"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/codex-mcp-oauth-fails-on-both-mcp-url-variants-cli-reports-premium-archive-as-free/13801"
published_at: "2026-08-11T16:50:52+00:00"
first_seen_at: "2026-08-11T19:14:06.915799+00:00"
fetched_at: "2026-08-11T19:14:08.478419+00:00"
content_hash: "sha256:6c3974f4488b999f5f85bad56e9aa0aa0a3392f64fb3c53176a9cf11500af0a4"
---

# Codex MCP OAuth fails on both /mcp URL variants; CLI reports Premium Archive as Free

I am a Premium Archive subscriber, but cannot authenticate NewsBlur MCP with Codex on macOS.


With this Codex configuration:


```text
[mcp_servers.newsblur]
url = "https://newsblur.com/mcp"


```


I get:


```text
Error: Metadata error: Protected resource metadata resource mismatch:
reference 'https://newsblur.com/mcp',
permitted 'https://newsblur.com/mcp/'


```


With the trailing slash:


```text
[mcp_servers.newsblur]
url = "https://newsblur.com/mcp/"


```


I get:


```text
Error: Authorization server issuer mismatch:
expected https://newsblur.com/mcp,
received https://newsblur.com/mcp/


```


I logged out and retried, with the same results.


It looks as if the protected-resource metadata and OAuth authorization-server metadata use inconsistent canonical URLs (` /mcp` vs.` /mcp/` ). Codex validates these URLs strictly, so neither documented URL variant works.


There may also be a related CLI issue.` newsblur auth status` reports` Tier: Free` , although this is the correct account and it has an active Premium Archive subscription. The CLI can retrieve my folder list, so authentication itself succeeded.


Could you check both the MCP OAuth metadata URL consistency and the subscription tier returned to the CLI/OAuth client?

---
schema_version: "1.0.0"
document_id: "4dcdf1b5928678105c24ec7821dc6c2dffc1224276050ec4ea8581b96774c3c9"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/21-11-2025"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:69ec3a0962ece2b16e098e6683092605763e55d0a94a495ff790413c0de5842c"
---

# MCP Server, Gemini 3, Teams, Library Judge

### Cloud MCP Server


A hosted Model Context Protocol (MCP) server that enables AI assistants to control browser automation. It works with any HTTP-based MCP client, including Claude Code.


**MCP Server URL:**` https://api.browser-use.com/mcp`


This is an HTTP-based MCP server designed for cloud integrations and remote access. If you need a local stdio-based MCP server for Claude Desktop, use the free open-source version:` uvx browser-use --mcp`


#### Implementation Details


Check out the[documentation](https://docs.cloud.browser-use.com/usage/mcp-server) .


### Gemini 3 support


It’s the best model for Browser Use.


### Teams


The option to add multiple team members to your cloud project (same billing, same credits throughout organization). Available on[cloud.browser-use.com](https://cloud.browser-use.com/) .


### Library judge


When task is completed, you can get llm-as-a-judge evaluation of the task - the easiest way to evaluate your own workflows (available on the library and cloud). If you need more advanced evalscontact us .


```text
const   task1   =   await   client.tasks.  createTask  ({
task:   "Whats the weather in SF."  ,
schema: TaskOutput,
llm:   "browser-use-llm"  ,
judge:   true
});


const   result1   =   await   task1.  complete  ();
result1.judgeVerdict, result1.judgement
```


- file uploads on the cloud

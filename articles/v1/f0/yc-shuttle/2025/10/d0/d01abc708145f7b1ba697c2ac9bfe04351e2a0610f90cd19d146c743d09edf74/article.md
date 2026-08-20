---
schema_version: "1.0.0"
document_id: "d01abc708145f7b1ba697c2ac9bfe04351e2a0610f90cd19d146c743d09edf74"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2025/10/10/mcp-servers-for-rust-developers"
published_at: "2025-10-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:b99ea29b4aa138c826d8e9562af4ed5d1f10005d07ec0f1d7a9b995f8e24e483"
---

# MCP Servers for Rust Developers

Watch how MCP servers transform AI coding assistants from text generators into practical development tools. In this video, David and Mark (Senior Software Engineer at Shuttle) demonstrate three MCP servers that eliminate the constant context switching developers face:


- **GitHub MCP** - Managing issues and pull requests without leaving your editor
- **Context7 MCP** - Getting up-to-date library documentation on demand
- **Shuttle MCP Server** - Deploying Rust applications with simple commands


You'll see each server in action and learn how to set them up yourself.


## What You'll Learn #


The video shows how MCP servers give AI assistants direct access to tools and data sources. Instead of explaining what you need and watching the AI generate code, you can have it perform actual operations - deploying apps, searching documentation, or managing GitHub issues.


### Setting Up the Shuttle MCP Server #


In the video, you'll see the Shuttle MCP Server in action. It connects your AI assistant directly to Shuttle's deployment platform, handling everything from project creation to log monitoring.


For Cursor, you can add it by clicking the "Add to Cursor" button below.


Or add this to your` mcp.json` :


```text
{
"mcpServers"  :    {
"Shuttle"  :    {
"command"  :    "shuttle"  ,
"args"  :    [  "mcp"  ,    "start"  ]
}
}
}


```


The video walks through the complete setup process and shows real deployment examples.


Learn more:[Shuttle MCP Server](https://www.shuttle.dev/blog/2025/10/08/shuttle-mcp?utm_source=shuttle_blog&utm_medium=video&utm_campaign=mcp_servers)


Full documentation:[MCP Server Documentation](https://docs.shuttle.dev/integrations/mcp-server?utm_source=shuttle_blog&utm_medium=video&utm_campaign=mcp_servers)


#### Join the Shuttle Discord Community


Connect with other developers, learn, get help, and share your projects


[Join](https://discord.gg/shuttle)


### Try It Yourself #


Get started with Shuttle and deploy your first Rust app:


```text
shuttle init  --from   shuttle-hq/shuttle-examples  --subfolder   axum/hello-world


```


Then connect the Shuttle MCP Server to your AI assistant and deploy directly from your coding session.


Happy coding!

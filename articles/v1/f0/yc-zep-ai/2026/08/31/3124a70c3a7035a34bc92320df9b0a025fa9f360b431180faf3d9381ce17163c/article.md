---
schema_version: "1.0.0"
document_id: "3124a70c3a7035a34bc92320df9b0a025fa9f360b431180faf3d9381ce17163c"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/agent-memory-mcp/"
published_at: "2026-08-19T15:21:15+00:00"
first_seen_at: "2026-08-19T15:43:06.508505+00:00"
fetched_at: "2026-08-19T15:43:09.798041+00:00"
content_hash: "sha256:565e723ddb7f49be2738bf11fdb6f8aaf0511e7aa96c253135daf15e3a5340b0"
---

# Memory for every agent your team uses

## Key takeaways


- **One memory across every agent.** Claude, Claude Cowork, ChatGPT, Cursor, and other MCP clients share the same[agent memory](https://www.getzep.com/?ref=blog.getzep.com) as the agents you build on Zep's SDKs; context written by one is available to the other.
- **Memory is bound to identity.** Each person signs in through Google Workspace or your enterprise identity provider and reaches their own user graph, never another user's.
- **Users can also reach organizational context, with access controlled by policy.** The graph directory lets an agent discover shared standalone graphs and search the one that matches.
- **Access is governed and audited.** User-group policies decide which shared graphs each person's agents can reach. Writes are gated per connection, and configuration changes are audited.
- **Generally available now** on Zep managed cloud and BYOC, rolling out to thousands of users through one connection per project.


## One memory across every agent


Until now, only the agents your organization built against Zep's API could reach a user's memory. The Memory MCP Server opens that memory to the agents your people already use — Claude, Claude Cowork, ChatGPT, Cursor, and any other MCP client. Both paths operate on the same user graph, so a preference recorded while working in Claude is applied by the support agent your team built with the SDK, and a decision that agent captured surfaces the next time the same person asks ChatGPT about the project.


Each connected person reaches their own memory and nothing else by default. The user-graph tools take no user, graph, or project argument; the target is fixed by the authenticated identity, so one person's token cannot select another person's memory or another project.


A preference recorded in Claude is applied by an agent you built, then recalled in ChatGPT. All three read the same user graph; shared graphs are available to every connected client.


The tool surface is small.` search_graph` returns Zep's assembled, reranked context block by default, the same Smart Context Assembly output the agents you build consume through the SDK.` get_user_summary` returns the person's narrative summary.` add_memory` writes new information when the connection allows writes.


For Claude, Claude Cowork, and ChatGPT, the server ships as the Zep Memory plugin, which bundles the MCP connection with a skill that teaches the assistant when to search memory and what is worth saving.


## Organizational context through the graph directory


Personal memory covers half of what a knowledge worker's agents need. The other half is organizational: product and customer knowledge that lives in shared standalone graphs rather than in any one person's graph.


An agent that can reach more than one graph needs a bounded catalog of what each graph contains. The graph directory is that catalog: project-scoped metadata for live standalone graphs, with pagination and lexical search over each entry's metadata. An entry carries a stable graph ID plus an optional, customer-authored name and description; Zep does not generate these from graph contents.


The flow an agent follows is short. It queries the directory when it knows the kind of context it needs, picks a matching graph ID, then searches that graph's contents with` search_graph_in` . Directory search matches routing metadata; content search happens only inside the selected graph. The same catalog is available to the agents you build through` graph.list_all` in the SDKs.


## Governed access


Which shared graphs a person's agents can reach is a policy decision. Connections start with project-wide access; accounts with policy-based access control can move a connection to UserGroup[Attribute-based Access Control (ABAC)](https://help.getzep.com/policy-based-access-control?ref=blog.getzep.com) , where access is default-deny and each user's agents discover only the graphs their groups are granted. A graph a user is not authorized to see is absent from directory results and counts entirely.


Writes are gated per connection, and an account-level kill switch overrides every connection. Connection changes are audited with the member who made them and a before-and-after snapshot, and disabling a connection stops issued tokens within a few minutes. The[standalone graph authorization](https://help.getzep.com/memory-mcp-server/standalone-graph-authorization?ref=blog.getzep.com) docs cover the policy model.


Access follows identity: Maya's graph, the graphs ABAC policy grants her, and nothing else.


## Enabling it


An administrator creates one MCP connection per project with Google Workspace or custom OIDC sign-in. Zep discovers the identity provider from each person's work email and provisions users on their first sign-in; Zep never stores end-user credentials. On Claude and ChatGPT, the Zep Memory plugin is distributed through the organization marketplace or workspace catalog; other MCP clients add the server endpoint directly.


Setup details live in the docs:[configuring authentication](https://help.getzep.com/memory-mcp-server/authentication?ref=blog.getzep.com) ,[provisioning the plugin in Claude and ChatGPT](https://help.getzep.com/use-zep-in-claude-chatgpt?ref=blog.getzep.com) , and[connecting a client](https://help.getzep.com/memory-mcp-server/connect?ref=blog.getzep.com) .


## Availability


The Memory MCP Server is generally available on all plans, on Zep managed cloud and BYOC. It uses per-account MCP seats; seat counts vary by plan. Custom OIDC and UserGroup ABAC require the Enterprise plan. Google Workspace sign-in does not. The[Memory MCP Server documentation](https://help.getzep.com/memory-mcp-server?ref=blog.getzep.com) is the place to start.


## FAQ


**Can one person's agent read another person's memory?** No. The user-graph tools take no user or project argument; the target is fixed by the signed token at connection time. A token cannot select another user's graph or switch projects.


**Does this work with the agents we already built on Zep?** Yes. MCP clients and SDK-built agents operate on the same user graph in the same project. Context written through either path is served to both.


**What does an MCP client actually write?** Writes go through the` add_memory` tool, and only when the connection allows writes. The Zep Memory plugin's skill saves durable facts such as preferences, corrections, procedures, and decisions; it skips ephemeral chat. Zep does not auto-ingest every message.


**How do we revoke access?** Disable the connection: new sign-ins are refused immediately and issued tokens stop working within a few minutes. The account-level kill switch stops all MCP writes at once.


**Which clients are supported?** Claude, Claude Cowork, and ChatGPT via the Zep Memory plugin; Cursor and any other MCP client that supports remote servers over HTTP with OAuth, via the endpoint directly.

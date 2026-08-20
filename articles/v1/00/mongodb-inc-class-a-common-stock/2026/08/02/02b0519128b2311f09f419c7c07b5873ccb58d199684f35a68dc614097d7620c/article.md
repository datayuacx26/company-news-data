---
schema_version: "1.0.0"
document_id: "02b0519128b2311f09f419c7c07b5873ccb58d199684f35a68dc614097d7620c"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/mongodb-for-agentic-era-built-for-developers-ai-agents"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T20:14:35.483907+00:00"
fetched_at: "2026-08-13T20:14:38.318971+00:00"
content_hash: "sha256:525fdf0c49fe0eb79a2c786dd671b4e9dd150d3ae5ad685470fe4cdc9cc60104"
---

# MongoDB for the Agentic Era: Built for Developers and AI Agents

How developers work has dramatically changed. Anthropic's 2026 Agentic Coding Trends Report* found that developers now use AI in roughly 60% of their engineering work. Coding agents are no longer just executing decisions; they're helping make them. If you’re building software today, it has to work for both humans and the agents working alongside them.


Today at MongoDB.local Build Fest, we're launching capabilities built for exactly that: the **MongoDB Atlas Managed MCP Server** , **Atlas App Connections, native connectors for frontier AI tools, and an integration with Vercel v0** as part of an expanded partnership with Vercel **.** These capabilities make MongoDB easy to reach from the tools developers are already using, and they let developers control which apps can access their clusters and with what permissions.


## Removing infrastructure overhead


Our existing MCP server already sees more than 30,000 npm installs per week, and starting today, MongoDB can run it for you. The[MongoDB Atlas Managed MCP Server](https://www.mongodb.com/products/tools/mcp-server) is fully hosted by MongoDB, so developers can connect their AI coding tools to their Atlas data without standing up any infrastructure themselves—no connection strings to configure, no service accounts to create, no credentials to rotate, and none of the admin overhead that used to come with standing up a local MCP server.


Connecting to the server is just as simple. Developers grant coding tools access to their Atlas account through a one-click OAuth consent flow, no URL to copy, no secret to paste. The tool then acts on the developer's behalf using their own Atlas permissions.


Once connected, the coding tool has immediate access to a rich set of tools to interact with MongoDB. It can perform read operations such as listing indexes and collections, querying and aggregating data, and inspecting schemas, and, if the developer has permissions, write operations such as creating collections, inserting and updating records, and managing indexes on their behalf.


## Making agents governable


The most common pattern for connecting external APIs and tools to enterprise software is some version of a service account or long-lived credentials, often shared across an entire team, rotated periodically or never. For agents, this is a dangerous setup. An agent acting through a shared identity with broad permissions doesn't pause to check whether an action is appropriate; it just executes.


[Atlas App Connections](https://www.mongodb.com/docs/atlas/app-connections/) **** is a new Atlas feature that addresses this for the interaction between AI tools and MongoDB Atlas. It's also what powers the OAuth consent flow behind the Atlas Managed MCP Server. Built on OAuth 2.1, it replaces the shared service account model with individual user delegation. A developer authorizes an AI tool with a one-click consent flow using their existing Atlas login. The tool then acts on the developer's behalf using their own permissions, scoped to exactly what that developer can do, nothing more. This makes every action attributable to a specific user and gives it the same auditing parity as any other event in Atlas.


With Atlas App Connections, org admins get the guardrails they need without becoming a bottleneck to development. AI client access is disabled by default, so no external tool gets access to your organization without explicit opt-in. Admins can enforce read-only mode across every connected client with a single setting, configure token lifetimes, and revoke access at any time, directly from the Atlas UI or via the Admin API.


## Meeting (human) developers where they are


Gone are the days of manually installing packages and reading through setup guides to get things up and running. If your product is not directly accessible through the major frontier AI marketplaces, developers will simply reach for something that is.


**The MongoDB Atlas Managed MCP Server is now powering native plugins in the AI tools that developers already spend their day in—Claude Code, Codex, Cursor, Grok Build, and Devin AI** . Developers find the connector in the marketplace, authorize with a single click, and access[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) right within their workflow without any context switching or additional setup.


This opens up workflows that weren't practical before: running large-scale data migrations using a coding agent with access to the Atlas Managed MCP Server and[MongoDB Agent Skills](https://www.mongodb.com/docs/agent-skills/) to ensure the agent follows MongoDB’s best practices during the migration, or building workflow automations such as an incident triage agent that monitors a Slack channel for issues, runs queries against Atlas to investigate, and surfaces fixes for human review.


## From prototype to scalable product


[Vercel's v0](https://v0.app/) is an agentic app builder used by more than 3.5 million developers to go from a prompt to a working, deployed application. As that application gains traction and becomes something people rely on, the data layer becomes critical—persistence, backups, replication, and multi-tenancy. This is precisely what MongoDB Atlas is built for.


And now, through MongoDB’s native Vercel Marketplace integration, developers can provision an Atlas cluster directly from within Vercel, with connection details automatically injected into their app's environment.


The division of value is clean: Vercel owns the fastest path from idea to shipped app. MongoDB owns the fastest path from app to a durable, scalable product. Together, they close the gap between prototype and production.


## The intelligent data platform for humans and AI agents


How software is created has changed dramatically over the past few years; more people than ever are building software, and they’re building it in new places


.


It's now built by developers working alongside agents and, increasingly, by agents working on developers’ behalf. What every builder needs is their application data inside the tool they are already working in. Until now, getting it there often meant leaving the tool, standing up a data layer connection by hand, and wiring in credentials. That’s friction a person might tolerate once, but which an agent can’t resolve on its own.


To address the shift in how software is built, we're making MongoDB Atlas accessible from the AI tools developers already use. This means allowing agents to store operational data, embeddings, and memory, ensuring every agent interaction with the cluster and the data therein is auditable, and giving developers a production-grade database that takes their app from a prototype to a scalable product.


Together, they make MongoDB the intelligent data platform built for both the humans creating software today and the AI agents building alongside them.


###### Next Steps


To learn about how MongoDB is closing the divide between agentic promise and impact, read my colleague[Frank Liu's blog post](https://www.mongodb.com/company/blog/product-release-announcements/closing-gap-between-agentic-promise-impact-industry-leading-retrieval) . And for more about all of the capabilities announced at MongoDB.local Build Fest, check out the[What's New Page](https://www.mongodb.com/products/updates) .


*The 2026 Agentic Coding Trends Report can be found at:[https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf)

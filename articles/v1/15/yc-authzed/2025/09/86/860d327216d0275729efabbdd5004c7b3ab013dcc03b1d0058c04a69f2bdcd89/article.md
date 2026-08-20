---
schema_version: "1.0.0"
document_id: "860d327216d0275729efabbdd5004c7b3ab013dcc03b1d0058c04a69f2bdcd89"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/introducing-authzeds-mcp-servers"
published_at: "2025-09-30T10:45:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:55:46.222495+00:00"
content_hash: "sha256:c9a1f658a327ad5685e8b152032afae67d36325b17fdd575bc92fce733de5a15"
---

# Introducing AuthZed's MCP Servers

We're excited to announce the launch of two new MCP servers that bring SpiceDB resources closer to your AI workflow, making it easier to learn and get started using SpiceDB for your application permissions: the **[AuthZed MCP Server](https://authzed.com/docs/mcp/authzed/authzed-mcp-server)** and the **[SpiceDB Dev MCP Server](https://authzed.com/docs/mcp/authzed/spicedb-dev-mcp-server)** .


## Two Servers, Complementary Use Cases


The **AuthZed MCP Server** brings comprehensive documentation and learning resources directly into your AI tools. Whether you're exploring SpiceDB concepts, looking up API references, or searching for schema examples, this server provides instant access to all SpiceDB and AuthZed documentation pages, complete API method definitions, and a curated collection of authorization pattern examples. It's designed to make learning and referencing SpiceDB documentation seamless, right where you're already working.


The **SpiceDB Dev MCP Server** takes things further by integrating directly into your development workflow. It connects to a sandboxed SpiceDB instance, allowing your AI coding assistant to help you learn and experiment with schema development, relationship testing, and permission checking. Need to validate a schema change? Want to test whether a specific permission check will work? Your AI assistant can now interact with SpiceDB on your behalf, making development faster and more intuitive.


Ready to try them out? Head over to[authzed.com/docs/mcp](http://authzed.com/docs/mcp) to get started with both servers.


## Our MCP Journey: From Prototypes to Production


We've been experimenting with MCP since the first specification was published. Back when the term "vibe coding" was just starting to circulate, we built an[early prototype MCP server for SpiceDB](https://github.com/samkim/spicedb-mcp-server) . The results were eye-opening. We were pleasantly surprised by how effectively LLMs could use the tools we provided, and delighted by the potential of being able to "talk" to SpiceDB through natural language.


That initial prototype sparked conversations across the SpiceDB community. We connected with others who were equally excited about the possibilities,[sharing ideas](https://www.youtube.com/watch?v=DG0J_zcNy6M) and[exploring use cases](https://youtu.be/TaCB4oGBtbk?feature=shared&t=1053) together. Those early discussions helped shape our thinking about what MCP servers for SpiceDB could become.


As the MCP specification continued evolving (particularly around enterprise readiness and authorization), we wanted to deeply understand these new capabilities. This led us to build a[reference implementation of a remote MCP server](https://github.com/authzed/mcp-server-reference) using open source solutions. That reference implementation became a testbed for understanding the authorization aspects of the spec and exploring best practices for building production-ready MCP servers.


## Why We Built These Servers


Through our[own experience with AI coding tools](https://authzed.com/blog/coding-with-ai-my-personal-experience) , we've seen firsthand how valuable it is to have the right resources and tools available directly in your AI workflow. Our team's usage of AI assistants has steadily increased, and we know the difference it makes when information and capabilities are just a prompt away.


For AuthZed and SpiceDB users, we wanted to bring learning and development resources closer to where you're already working. Whether you're learning SpiceDB concepts, building a new schema, or debugging permissions logic, having immediate access to documentation, examples, and a sandbox SpiceDB instance can dramatically speed up the development process.


That's why we built both servers: the AuthZed MCP Server puts knowledge at your fingertips, while the SpiceDB Dev MCP Server puts your development environment directly into your AI assistant's toolkit.


## Building Responsibly: Authorization in MCP


We're still actively building and experimenting with MCP. While the specification provides guidance for authorization, there's significant responsibility on MCP server developers to implement appropriate access controls for resources and accurate permissions around tools.


This is particularly important as MCP servers become more powerful and gain access to sensitive systems. We're learning as we build, and we'll be sharing new tools and lessons around building authorization into MCP servers as we discover them. We believe the combination of SpiceDB for MCP permissions and AuthZed for authorization infrastructure is especially well-suited for defining and enforcing the complex permissions that enterprise MCP servers require.


In the meantime, we encourage you to try out our MCP servers. The documentation for each includes detailed use cases and security guidelines to help you use them safely and effectively.


If you're building an enterprise MCP server and would like help integrating permissions and authorization, we'd love to chat.[Book a call](https://authzed.com/call) with our team and let's explore how we can help.


---


Happy coding, and we can't wait to see what you build with these new tools! 🚀


On this page


- Two Servers, Complementary Use Cases
- Our MCP Journey: From Prototypes to Production
- Why We Built These Servers
- Building Responsibly: Authorization in MCP


## Related


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)


[Engineering Introducing the LangChain SpiceDB Integration We're announcing langchain-spicedb, a new library that brings SpiceDB's relationship-based access control into LangChain and LangGraph workflows. Build RAG pipelines that respect what users are allowed to see with post-retrieval filtering, LangGraph auth nodes, and agent permission tools. Mar 9, 2026 · 6 min](https://authzed.com/blog/langchain-spicedb-integration)[Engineering Introducing the LangChain SpiceDB Integration We're announcing langchain-spicedb, a new library that brings SpiceDB's relationship-based access control into LangChain and LangGraph workflows. Build RAG pipelines that respect what users are allowed to see with post-retrieval filtering, LangGraph auth nodes, and agent permission tools. Sohan Maheshwar · Mar 9, 2026 · 6 min](https://authzed.com/blog/langchain-spicedb-integration)


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)

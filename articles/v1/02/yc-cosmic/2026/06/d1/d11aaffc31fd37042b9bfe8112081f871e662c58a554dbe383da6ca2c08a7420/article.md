---
schema_version: "1.0.0"
document_id: "d11aaffc31fd37042b9bfe8112081f871e662c58a554dbe383da6ca2c08a7420"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/mcp-server-complete-guide"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:5828f307d6c188b3668864bcc50dca419a7b2b5d4365fe5764d242e4d67783b3"
---

# MCP Server: The Complete Guide for Developers (2026)

MCP server searches have crossed 60,000 per month and are still climbing. If you're building with AI agents in 2026, you've already hit MCP in Cursor's settings panel, Claude Desktop's config file, or a GitHub README. What you might not have is a clear, practical mental model for what an MCP server actually is, when to use one, and how to build and host your own.


This guide gives you that. We'll cover the protocol itself, the schema definition, the current ecosystem, the 18 tools Cosmic's native MCP server exposes, and a full walkthrough of creating, connecting, and hosting an MCP server. By the end, you'll have a working MCP-powered content workflow and a solid foundation for building custom MCP servers on top of any data source.


## What Is an MCP Server?


MCP stands for Model Context Protocol. An MCP server is a lightweight service that exposes tools, resources, and prompt templates to AI agents in a standardized way.


The key word is *standardized* . Before MCP, every tool integration for an AI agent required custom glue code: a bespoke JSON schema, a one-off API wrapper, a pile of system prompt instructions. Each AI client had its own format. Nothing was portable.


MCP solves that with a common interface that any compliant AI client can speak. You build the server once. Claude, Cursor, Copilot, Codex — any client that supports MCP can connect to it without modification.


At its core, an MCP server does three things:


- Exposes *tools* the agent can call ("fetch this object", "create a new page", "search posts by tag")
- Provides *resources* that give the agent context (a knowledge base, a content schema, a list of available types)
- Accepts *prompt templates* that shape how the agent reasons about the data it receives


The protocol was introduced by Anthropic and has since been adopted broadly across the developer ecosystem. It operates over two transports: HTTP/SSE (for hosted endpoints) and stdio (for local processes). Any language or runtime can implement it.


## Why MCP Is Everywhere Right Now


Three things converged over the last 12 months to push MCP from niche spec to mainstream standard.


### 1. Agentic IDEs needed a universal tool layer


Cursor, Claude Code, and Codex aren't autocomplete tools anymore. They're autonomous coding agents that run multi-step tasks: reading files, editing code, running tests, committing changes. For those agents to act on *your* data — your CMS, your database, your API — they need a standardized way to reach it. MCP servers are that hook.


### 2. Claude's native MCP support changed the calculus


When Anthropic built MCP support directly into Claude Desktop and the Claude API, adoption exploded. Developers who had been wiring up custom tool definitions switched to MCP because the portability payoff was immediate: build the server once, connect any compatible client.


### 3. The skills model is winning


Early agentic tooling required developers to describe every tool in a flat JSON schema, which didn't scale. MCP introduces the concept of discrete, composable capabilities that agents can discover and invoke dynamically. This is a better mental model for production agentic workflows, and it's the direction the ecosystem has converged on.


## The mcp.json Schema: Complete Reference


Every MCP client reads its server configuration from a JSON file. Understanding the schema is essential for setting up any MCP server correctly.


### mcp.json Schema Definition


The top-level structure of an config file looks like this:


```text


```


### mcpServers Key Requirements


The object is the required top-level key. Each child key is an arbitrary name you give the server (e.g. , , ). Within each server definition:


Property Type Required Description


string Yes (stdio) The command to run the MCP server process (e.g. )


string\[\] No Arguments passed to (e.g. )


object No Environment variables injected into the server process


string Yes (HTTP/SSE) Hosted endpoint URL (used instead of for remote servers)


object No HTTP headers sent with every request (used for auth with hosted servers)


For **stdio transport** (local process), use + + .
For **HTTP/SSE transport** (hosted server), use + .


You cannot mix and in the same server definition.


### Where mcp.json Lives Per Client


Client Config file location


Claude Desktop (macOS)


Claude Desktop (Windows)


Cursor (project-scoped) in your project root


Cursor (global)


Windsurf


VS Code (Copilot)


### Complete mcp.json Example (Cosmic, stdio)


```text


```


### Complete mcp.json Example (Cosmic, hosted HTTP/SSE)


```text


```


For read-only access, omit the write key entirely. Write tools return a clear error if invoked without one.


## The MCP Protocol: How It Actually Works


Under the hood, an MCP server runs as a process (local or hosted) that communicates with MCP clients over a defined protocol.


### Transports


MCP supports two transports:


**HTTP/SSE (Server-Sent Events):** The client sends HTTP POST requests to the server's endpoint. The server streams responses back via SSE. This is the standard for hosted MCP servers — it works over any HTTPS connection, requires no local install, and scales like any other web service.


**stdio:** The client spawns the server as a local process and communicates over standard input/output. This is common for developer tools that run inside an IDE (Cursor, Claude Desktop in local mode). Lower latency, but requires the server to be installed locally.


### The Three Primitives


**Tools** are functions the agent can call. Each tool has a name, a description (used by the agent to decide when to call it), and a JSON schema for its input parameters. The agent calls a tool, the server executes the function and returns a result, the agent uses the result in its next step.


**Resources** are data objects the server exposes for context. A resource might be a list of available content types, a brand guideline document, or a schema definition. Resources are read by the agent at the start of a session to build its understanding of what it's working with.


**Prompt templates** are reusable instruction fragments that shape agent behavior for specific tasks. A template might define how the agent should format a content update request, or how it should structure a search query.


### Discovery


When an MCP client first connects to a server, it calls the , , and endpoints. The server returns its full capability manifest. The client caches this and presents the tools to the agent as available functions. No manual configuration required.


## The Current MCP Ecosystem


As of mid-2026, MCP servers exist for most major developer tools and data sources. Some of the most actively used:


**Content and data:**


- Cosmic (content management — read, write, schema, AI generation)
- Notion, Airtable, Linear (structured data and project management)
- GitHub (repositories, issues, PRs)
- Postgres, SQLite (direct database access)


**Development tools:**


- Vercel (deployments, logs, environment variables)
- Sentry (error tracking)
- Datadog (observability)


**Communication:**


- Slack (channels, messages, threads)
- Gmail, Outlook (email)


**Search and web:**


- Brave Search, Exa (web search)
- Firecrawl (web scraping and extraction)


The Anthropic MCP repository maintains an official list of servers:[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) .


## MCP Server vs. Direct API: When to Use Each


MCP is a layer on top of your REST API, optimized for agent consumption.


**Use the REST API directly when:**


- You're writing application code that calls an endpoint on a fixed schedule
- You need low-level control over request parameters
- You're integrating with a non-AI system
- You want typed SDK methods with autocomplete and error handling


**Use an MCP server when:**


- You want an AI agent to discover and call tools dynamically
- You're connecting an AI assistant (Claude, Cursor, Copilot) to your data
- You want portability across multiple AI clients without rewriting integrations
- You're building a workflow where the agent decides *which* tools to call based on context


For Cosmic specifically: use the[TypeScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk) in your application code, and use the MCP server for agent-driven workflows in Claude Desktop, Cursor, or any MCP-compatible client.


## Cosmic's MCP Server: 18 Tools Across 4 Categories


Cosmic ships a native MCP server as a first-class feature. It's built and maintained by the Cosmic team and exposes 18 tools across four categories.


### Objects (5 tools)


- — list or search content objects by type, status, or locale
- — fetch a single object by ID or slug
- — create a new object (blog post, page, product, etc.)
- — edit an existing object's content or metadata
- — permanently delete an object


### Media (4 tools)


- — browse media files, optionally scoped to a folder
- — fetch metadata and the imgix URL for a single asset
- — upload a file from a URL or base64 payload
- — remove a media file


### Object Types (5 tools)


- — list every content model in the bucket
- — fetch the full schema for a single type
- — define a new content model
- — evolve an existing schema
- — drop a content model and all its objects


### AI Generation (4 tools)


- — draft, rewrite, summarize, or translate copy using your bucket's own content as context
- — generate an image and store it in your media library
- — generate a short video clip (Google Veo)
- — generate narration audio from text (13 voices via OpenAI TTS)


## How to Create an MCP Server


Creating an MCP server involves four steps: defining your tools, implementing the protocol endpoints, testing locally, and deploying. Here's the full walkthrough.


### Step 1: Install the MCP SDK


```text


```


### Step 2: Define your tools


Each tool needs a name, a description the agent can reason about, and a JSON Schema definition for its inputs:


```text


```


### Step 3: Implement tool handlers


```text


```


### Step 4: Connect the transport and start the server


For local stdio (dev and IDE use):


```text


```


For HTTP/SSE (hosted production use):


```text


```


### Step 5: Test your server locally


Use the MCP Inspector to verify your server before connecting a real client:


```text


```


The inspector opens a browser UI where you can call tools manually and inspect responses.


## How to Host an MCP Server


Once your server is built and tested locally, there are three main hosting options.


### Option 1: Publish as an npm package (stdio, easiest)


If your users will run the MCP server locally via , publish it to npm. This is how most community MCP servers are distributed, including .


```text


```


Users add it to their with:


```text


```


### Option 2: Deploy as an HTTPS endpoint (HTTP/SSE, scalable)


For a hosted MCP server that requires no local install, deploy as a standard web service. Any Node.js host works: Vercel, Railway, Fly.io, AWS Lambda.


Vercel deployment is straightforward for HTTP/SSE servers:


```text


```


Users connect via in their , passing credentials as .


### Option 3: Use Cosmic's hosted MCP endpoint (zero infrastructure)


If you're managing content, Cosmic's hosted MCP server at eliminates infrastructure entirely. Pass your bucket credentials as headers and your agent has full content access with zero deployment work on your end.


### Agentic Coding Workflows with Hosted MCP Servers


Hosted MCP servers unlock a key pattern for agentic coding workflows in 2026: your CI/CD pipeline, your editor, and your production AI agents all connect to the same server endpoint. No version drift. No local install required in CI. One endpoint, any client.


For teams adopting agentic coding workflows, a hosted MCP server is the most operationally stable choice. The stdio pattern works well for individual developer machines; HTTP/SSE scales to teams and automation pipelines.


## Connecting Cosmic's MCP Server to Claude Desktop


The hosted endpoint is the fastest way to connect. No local install required.


### Step 1: Get your Cosmic credentials


In your Cosmic dashboard, go to your bucket and navigate to *Settings > API Access* . Copy:


- Your bucket slug
- Your read key
- Your write key (only needed if you want the agent to create or update content)


### Step 2: Add to Claude Desktop config


Open your Claude Desktop config file. On macOS it's at .


Add the Cosmic MCP server entry:


```text


```


For read-only access, omit the header.


### Step 3: Restart Claude Desktop


Quit and reopen Claude Desktop. You should see the Cosmic tools appear in the tool picker (the hammer icon in the input bar).


### Step 4: Test with a natural language command


```text


```


Claude will call with type , status , and return the results in a structured format.


## Connecting to Cursor


For Cursor, add the config to in your project root (project-scoped) or (global):


```text


```


Once connected, Cursor's agent can reference your content model when generating code, ensuring that component props, TypeScript types, and API calls match your actual bucket schema.


## Self-Hosted Option (stdio)


If you prefer to run the MCP server locally inside your dev environment, the npm package ships a stdio binary:


```text


```


Or install globally:


```text


```


The stdio binary reads credentials from environment variables:


```text


```


Claude Desktop config for self-hosted stdio:


```text


```


## Using the Cosmic SDK in Your Application Code


For application code, use the TypeScript SDK rather than the MCP server. The SDK gives you full type safety, autocomplete, and direct control over every API parameter.


```text


```


The SDK is the right choice when you control the call site. The MCP server is the right choice when an AI agent controls the call site.


## MCP Server vs. Agent Skills: What's the Difference?


Cosmic ships both an MCP server and Agent Skills. These are complementary but serve different purposes.


MCP Server Agent Skills


Purpose Direct content management Code generation guidance


Use case "List my blog posts" "Build a blog with Cosmic"


How it works AI calls tools to interact with your bucket AI writes code using the SDK


Best for Managing content while developing Building applications


Use both together: Agent Skills helps your AI write application code that uses the Cosmic SDK correctly; the MCP server lets your AI directly manage content in your bucket.


## What to Read Next


Now that you have the full picture on MCP servers, here are the related pieces worth reading:


- [What is an MCP Server? (Quick intro)](https://www.cosmicjs.com/blog/what-is-an-mcp-server) — the short version for sharing with non-technical teammates
- [Cosmic MCP vs. Strapi MCP](https://www.cosmicjs.com/blog/cosmic-mcp-vs-strapi-mcp) — side-by-side comparison of the two MCP implementations
- [MCP vs. Agent Skills](https://www.cosmicjs.com/blog/mcp-vs-skills-ai-coding-assistant-integrations-guide) — when to use each, with concrete examples
- [Build a DevOps Slack Agent with Cosmic](https://www.cosmicjs.com/blog/devops-slack-agent-cosmic) — a practical agent tutorial that uses MCP-powered content access


---


## Get Started with Cosmic's MCP Server


MCP is moving fast. The developers who wire up production-grade MCP workflows now will have a meaningful head start.


Cosmic's hosted MCP endpoint removes the infrastructure work and lets you focus on what the agent actually does.


[Sign up free — no credit card required](https://app.cosmicjs.com/signup)


Already have an account?[Go to MCP settings](https://app.cosmicjs.com/)


Want a walkthrough for your specific stack?[Book 15 minutes with Tony](https://calendly.com/tonyspiro/cosmic-intro)


Full MCP docs:[cosmicjs.com/docs/mcp-server](https://www.cosmicjs.com/docs/mcp-server)


*Cosmic is a YC W19-backed headless CMS built for developers and AI-native teams.*

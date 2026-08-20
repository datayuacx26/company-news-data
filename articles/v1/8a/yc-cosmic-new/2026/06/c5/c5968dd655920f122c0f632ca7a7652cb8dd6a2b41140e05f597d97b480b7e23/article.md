---
schema_version: "1.0.0"
document_id: "c5968dd655920f122c0f632ca7a7652cb8dd6a2b41140e05f597d97b480b7e23"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/what-is-an-mcp-server"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:222f932b0615d03eeb7276f7391f0140a5a25e01948a838c90c36c3ef3eba68c"
---

# What Is an MCP Server? How It Works, and How to Build One (2026)

An MCP server is a process that exposes tools, resources, and prompts to AI clients via a standard protocol. Connect it once, and any compatible AI client — Claude, Cursor, Claude Code — can read data, invoke actions, and interact with your backend without custom glue code.


This guide covers what MCP is, how the protocol works under the hood, how to build a server from scratch in TypeScript, and why Cosmic ships a native MCP server as a first-class feature.


## What Is MCP?


MCP stands for Model Context Protocol. Anthropic introduced it in late 2024 as an open standard for connecting AI models to external tools, data sources, and services. The goal: give any AI assistant a consistent, portable way to reach outside its context window and interact with the real world.


Before MCP, every tool integration required custom glue code. An AI assistant that needed to query a database wrote bespoke function definitions in JSON schema. An assistant that needed to read from an API defined its own tool call format. There was no standard, and every integration was one-off.


MCP solves that by defining a common interface. An MCP server exposes capabilities using a shared protocol that any MCP-compatible AI client can speak. Build the server once, connect any client.


## What Is an MCP Server?


An MCP server is a process that exposes three categories of primitives to AI clients:


**Tools** — Discrete operations the AI can invoke: fetching a content object, creating a database record, searching a knowledge base, running a query. Tools are the action layer.


**Resources** — Contextual data the AI can read: a list of available content types, a schema definition, a knowledge base index. Resources give the AI situational awareness without requiring it to call a tool first.


**Prompts** — Template-driven instructions that shape how the AI reasons about the data it receives. Prompts are optional but useful for steering the model toward specific output formats or reasoning patterns.


An MCP server sits between your AI client (Claude Desktop, Cursor, Claude Code, a custom agent) and whatever backend system you want the AI to interact with. The AI client speaks MCP. The MCP server translates those calls into whatever API, database, or service is behind it.


## MCP vs API: What Is the Difference?


A REST API is designed for applications to consume programmatically. An MCP server is designed for AI agents to consume dynamically.


With a REST API, you write application code that calls specific endpoints with specific parameters. The developer decides in advance which endpoints to call and when.


With an MCP server, the AI agent discovers available tools at runtime and decides which ones to call based on the current task. The agent reads tool descriptions, reasons about which tools are relevant, and invokes them in sequence without pre-programmed routing logic.


The practical difference: REST APIs require a developer to write integration code. MCP servers let the AI figure out the integration itself.


For a full breakdown, see[MCP vs Agent Skills: What Is the Difference and Which Do You Need?](https://www.cosmicjs.com/blog/mcp-vs-agent-skills)


## MCP vs Function Calling: What Is the Difference?


Function calling (also called tool use) is a feature built into AI models like Claude and GPT-4. You define a set of functions in JSON schema, pass them with your prompt, and the model returns structured calls to those functions.


MCP is a layer above function calling. Instead of defining functions inline per request, you define them once in an MCP server. Any compatible client can discover and call them. The server handles execution; the AI handles reasoning.


The key difference: function calling is per-request and per-model. MCP is reusable across clients, models, and sessions. Build the server once; connect it to Claude Desktop, Cursor, and your custom agent without redefining anything.


## Transport Options


MCP supports two transport types:


**Streamable HTTP** (recommended for hosted servers): The server exposes a single HTTP endpoint. The client sends JSON-RPC requests; the server responds with JSON or streams server-sent events (SSE) for long-running operations. This is how Cosmic's hosted MCP endpoint works.


**stdio** : The server runs as a local process. The client spawns it and communicates over standard input/output. Useful for local development or situations where you need the MCP process inside your dev environment.


## How the Protocol Works


MCP runs on JSON-RPC 2.0. The lifecycle looks like this:


**1. Initialization** — The client connects and sends an request. The server responds with its capabilities.


**2. Tool Discovery** — The client calls to discover what the server can do. The server returns tool definitions with names, descriptions, and input schemas.


**3. Tool Invocation** — When the AI decides to call a tool, the client sends a request. The server executes the tool and returns the result into the AI's context. The model decides what to do next.


The entire loop runs inside a single AI session.


## Why MCP Is Everywhere Right Now


Three things converged to push MCP from a niche spec to mainstream standard:


**Agentic IDEs need a universal tool layer.** Cursor, Claude Code, and Codex are no longer autocomplete tools. They run multi-step autonomous tasks. For these agents to act on your data, they need a way to reach it. MCP provides exactly that hook. The[Claude Code vs Codex vs Cursor comparison](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor) covers this in detail.


**Anthropic built MCP support directly into Claude.** When MCP support landed in Claude Desktop and the Claude API, adoption accelerated sharply. Developers who had been wiring up custom function definitions switched to MCP because it is portable: build the server once, connect any compatible client.


**Discoverable tools beat flat JSON schemas.** Early agentic tooling asked developers to define every capability as a flat JSON schema blob. MCP introduced composable, discoverable tool definitions that agents can reason about dynamically.


## What You Can Build With an MCP Server


MCP servers unlock a specific class of use cases that were awkward to build before the standard existed:


**Content management via natural language** — An agent connected to a CMS via MCP can read, create, and update structured content without a human opening a dashboard.


**Autonomous development workflows** — Cursor or Claude Code can pull schema context from your content model, generate code that correctly maps to your data structures, and push changes, all within a single editor session.


**Cross-platform content syndication** — An agent reads a content object from your CMS via MCP and pushes it to multiple channels: Slack, email, a newsletter API, wherever you wire up.


**Grounded Q&A and search** — An AI assistant with MCP access to your knowledge base answers questions based on real content, with citations, rather than generating plausible-sounding hallucinations.


**Automated content pipelines** — Scheduled agents monitor content status, trigger workflows when objects transition from draft to published, or auto-generate metadata like SEO descriptions and alt text.


**Agent-to-agent coordination** — MCP servers can be chained. Agent A calls Agent B's MCP server to fetch context, processes it, and stores the result via a third MCP server. This is how multi-agent pipelines work in production today.


## MCP Server Example: Build One in TypeScript


Here is a minimal but complete MCP server in TypeScript that connects an AI agent to a Cosmic bucket. It exposes two tools: listing content objects and creating new ones.


```text


```


```text


```


Add to your Claude Desktop config ( on macOS):


```text


```


Restart Claude Desktop. The and tools will now be available in every conversation.


## Cosmic's Native MCP Server


Building your own MCP server is educational. For day-to-day work with Cosmic, the hosted MCP server is the faster path. Connect any MCP-compatible client in under five minutes, no code required.


**Hosted Endpoint:**


**Claude Desktop Config:**


```text


```


**Cursor Config** — add to in your project root:


```text


```


## The 18 Tools


Cosmic's MCP server exposes 18 tools across four categories:


**Objects (5 tools)**


- — list or search content by type, status, locale
- — fetch a single object by ID or slug
- — create a new object
- — edit an existing object's content or metadata
- — permanently delete an object


**Media (4 tools)**


- — browse media files, optionally scoped to a folder
- — fetch metadata and imgix URL for an asset
- — upload a file from a URL or base64 payload
- — remove a media file


**Object Types (5 tools)**


- — list every content model in the bucket
- — fetch the full schema for a single type
- — define a new content model
- — evolve an existing schema
- — drop a content model and all its objects


**AI Generation (4 tools)**


- — draft, rewrite, summarize, or translate copy using bucket content as context
- — generate an image and store it in the media library
- — generate a short video clip via Google Veo
- — generate narration audio from text (13 voices via OpenAI TTS)


## Agent Memory: Cosmic as Context Store


MCP is how agents act on your content. But where do agents store what they learn across sessions?


Cosmic Objects are a natural answer. Every agent output — a finding, a drafted piece, a prompt version, a conversation turn — can be stored as a structured, versioned, API-accessible Object. The dashboard becomes a control panel for agent behavior, not just a content editor.


For a full breakdown of how to use Cosmic as agent context and memory storage, see[Cosmic as Agent Memory: Structured, Versioned, and Queryable](https://www.cosmicjs.com/blog/cosmic-agent-memory-structured-versioned-queryable) .


## The Agent Scope


Beyond the standard bucket tools, Cosmic's MCP server ships a separate scope designed for a specific use case: provisioning a brand-new Cosmic project for a user who doesn't have an account yet, entirely inside an AI tool session.


The agent scope exposes three tools:


- — Creates a new Cosmic project and bucket. The user receives an OTP via email to claim the account.
- — Accepts the OTP, verifies the user's email, and lifts restricted mode. The bucket is now fully operational.
- — Checks the claim status of a provisioned bucket.


This enables a complete "sign up through the AI" flow. A user talking to an AI assistant in Cursor or Claude Desktop can go from zero to a live Cosmic bucket without ever opening a browser.


Agent scope endpoint:


## Connecting to Claude, Cursor, and Claude Code


**Claude Code** — Add the hosted endpoint to your Claude Desktop or Claude Code config. From a Claude Code session, you can issue commands like: "Create a new blog post draft with this title and outline" or "List all published content tagged 'AI' from the last 30 days."


For a detailed comparison of Claude Code against other AI coding tools, see[Claude Code vs GitHub Copilot vs Cursor](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026) and[Claude Code vs Codex vs Cursor](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor) .


**Cursor** — Add the Cosmic config to your project's and restart Cursor. Composer can now manage your content schema and content objects mid-session, without switching to a CMS dashboard.


**Claude Desktop** — Config file location: macOS: . Windows: . Add the Cosmic server entry, restart, and the tools appear in every conversation session.


## Using the TypeScript SDK Alongside MCP


Cosmic's TypeScript SDK () and the MCP server are complementary. The SDK powers your application code; the MCP server powers your agent sessions.


```text


```


The agent loop using MCP handles content creation and management. The SDK handles content delivery to your frontend. Separating these concerns keeps your application code clean and your agent workflows autonomous.


## Frequently Asked Questions


**What is an MCP server used for?**
An MCP server gives AI agents a standard way to interact with external tools, data sources, and services. Common uses: content management via natural language, autonomous development workflows, grounded Q&A, and multi-agent coordination pipelines.


**Is MCP free?**
The MCP protocol itself is open source and free. Cosmic's hosted MCP server is included on all plans, including the free tier (1 bucket, 1,000 objects, no credit card required).


**What is the difference between MCP and an API?**
A REST API is consumed by application code with pre-programmed logic. An MCP server is consumed by AI agents that discover available tools at runtime and decide which ones to call based on the current task.


**What is the difference between MCP and function calling?**
Function calling is per-request and model-specific. MCP is reusable across clients, models, and sessions. Define your tools once in an MCP server; connect any compatible client without redefining anything.


**Which AI clients support MCP?**
Claude Desktop, Claude Code, Cursor, Windsurf, and any client built on the MCP SDK. The list is growing rapidly as MCP becomes the de facto standard for agentic tool integration.


**Can I run an MCP server locally?**
Yes. Use the stdio transport for local development. Cosmic also offers a self-hosted stdio option if you prefer the MCP process running inside your dev environment.


**How many tools does Cosmic's MCP server expose?**
18 tools across objects, media, object types, and AI generation. Full list above.


**Is Cosmic's MCP server secure?**
Authentication uses your bucket write key via an Authorization header. Read-only access is available with just the read key. Keys are never logged or stored by the MCP server. For a deeper look at how Cosmic handles[MCP server security and enterprise controls](https://www.cosmicjs.com/blog/mcp-server-security-zero-touch-oauth-enterprise) , including zero-touch OAuth and role-based access, see the full security breakdown.


**What happens if the AI model I use gets deprecated or restricted?**
Cosmic's MCP server is model-agnostic. It works with any MCP-compatible client regardless of the underlying model. Swap models without changing your MCP configuration.


## Why Cosmic for MCP-Powered Workflows?


Several headless CMS platforms exist, but Cosmic is purpose-built for the AI-native era:


- Hosted MCP endpoint: connect any client in under 5 minutes, zero install required
- 18 built-in tools across objects, media, schemas, and AI generation
- Agent scope for zero-browser account provisioning mid-session
- REST API with sub-100ms response times: agents stay fast
- TypeScript SDK with full type safety and tree-shaking support
- Team agents that live in Slack, WhatsApp, and Telegram
- Flexible content modeling: any schema, any type, any relationship
- Free tier includes full MCP access: 1 bucket, 1,000 objects, no credit card
- YC W19-backed: used in production by FINN, Parque Explora, Vuetify, Tripwire Interactive, and others


## Getting Started


1. Create a free Cosmic account at[cosmicjs.com](https://app.cosmicjs.com/signup) . No credit card required. Full MCP access on day one.
2. In your dashboard, go to Settings > API Access. Copy your bucket slug, read key, and write key.
3. Add the hosted endpoint to your Claude Desktop, Cursor, or Claude Code config using the JSON snippets above. Restart your client.
4. Start building. The 18 tools are available from the moment you connect.


## Further Reading


- [MCP Server Security: Zero-Touch OAuth and Enterprise Controls](https://www.cosmicjs.com/blog/mcp-server-security-zero-touch-oauth-enterprise)
- [MCP vs Agent Skills: What Is the Difference and Which Do You Need?](https://www.cosmicjs.com/blog/mcp-vs-agent-skills)
- [Claude Code vs GitHub Copilot vs Cursor: Honest Comparison](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026)
- [Claude Code vs Codex vs Cursor: The Best AI Coding Tool in 2026](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor)
- [Cosmic as Agent Memory: Structured, Versioned, and Queryable](https://www.cosmicjs.com/blog/cosmic-agent-memory-structured-versioned-queryable)
- [Cosmic MCP Server Docs](https://www.cosmicjs.com/docs/mcp-server)


---


*Build your MCP-powered content workflow today. Cosmic's hosted MCP server connects Claude, Cursor, and Claude Code to your content layer in under 5 minutes. Free plan includes full MCP access: 1 bucket, 1,000 objects, 18 built-in tools.*


[Start building free](https://app.cosmicjs.com/signup) |[Book a 30-minute intro with Tony](https://calendly.com/tonyspiro/cosmic-intro) |[Read the MCP docs](https://www.cosmicjs.com/docs/mcp-server)

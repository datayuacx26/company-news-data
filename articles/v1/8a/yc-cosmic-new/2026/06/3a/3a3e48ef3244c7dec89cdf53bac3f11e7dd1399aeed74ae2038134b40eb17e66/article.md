---
schema_version: "1.0.0"
document_id: "3a3e48ef3244c7dec89cdf53bac3f11e7dd1399aeed74ae2038134b40eb17e66"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-agent-memory-structured-versioned-queryable"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:f86a76c1f64c94ab5272001aa8d975db6d83011c709d27aa1a901c172915d2db"
---

# Cosmic as Agent Memory: Structured, Versioned, and Queryable

AI agents get better the more they run. Every conversation turn, every task completed, every prompt refined adds to a growing body of context that shapes the next output. The compounding effect is real: an agent with 100 turns of memory and a versioned prompt history behaves meaningfully differently from one starting cold.


Cosmic is where that context lives. Agent messages, system prompts, findings, and instructions are all stored as structured, versioned, API-accessible Objects. Each new turn adds to the record. Each prompt edit is tracked. The dashboard becomes a control panel for agent behavior, not just a content editor.


This is how we run Cosmic internally. The more you interact with our built-in agents, the better they get. Context compounds. Memory accumulates. And because everything is stored as a Cosmic Object, you can inspect, edit, and version any part of the loop at any time.


## What Agent Memory Actually Needs


The compounding loop only works if the memory layer has the right properties. Most agent frameworks handle working memory well. The gap is episodic and semantic memory: what the agent learned, did, and produced across sessions.


Researchers at Elastic recently published a[breakdown of agent memory tiers](https://www.elastic.co/search-labs/blog/agent-memory-elasticsearch) : working memory (in-context), episodic memory (past interactions), semantic memory (knowledge), and procedural memory (learned behaviors). Good persistent agent memory needs four properties:


- *Structured* : queryable by type, status, date, or custom field, not just full-text search
- *Versioned* : you need to know what the agent wrote at each point in time, not just the latest state
- *API-accessible* : any model, any framework, any language should be able to read and write it
- *Human-reviewable* : agents make mistakes; a human needs to inspect and correct outputs without touching a database


Cosmic satisfies all four out of the box.


## Cosmic Objects as Agent Outputs


Every piece of content in Cosmic is an Object: a typed, structured document with metadata fields you define, a draft/published status, a full revision history, and a REST API endpoint.


When an agent produces output, storing it as a Cosmic Object gives you:


- A queryable record with typed fields (, , , )
- Draft/published workflow so a human can review before promoting to production
- Full audit trail of every change
- REST API access from any runtime
- A dashboard UI where non-technical team members can inspect, edit, or approve agent outputs


Here's a simple research agent that stores its findings as Cosmic Objects:


```text


```


The output is immediately visible in the Cosmic dashboard. A team member can review the summary, edit it, toggle to true, and publish, all without touching code.


## Storing Prompts, Context, and Conversation Memory


Agent outputs are only part of the picture. The other half is what goes *in* to the agent: system prompts, conversation history, and session context. Cosmic handles these just as naturally.


At Cosmic, we use Cosmic itself to store the prompts and context for our own internal agents every day. Here's what that looks like in practice.


### System Prompts as Objects


Instead of hardcoding system prompts in your codebase, store them as Cosmic Objects. This gives you:


- *Version control for prompts* : draft a new version, test it, publish when ready, roll back if behavior degrades
- *Non-engineer editable* : your team can refine prompt wording without a deploy
- *Environment-aware* : different buckets for staging and production mean different prompts per environment with zero code changes


```text


```


When you want to update the prompt, you edit it in the Cosmic dashboard, save a new version, and publish. The agent picks it up on the next run with no deployment required.


### Conversation Context and Message History


For agents that need to maintain state across sessions, store the conversation history as structured Objects:


```text


```


The agent can reconstruct its full conversation history from Cosmic on every run. The history is human-readable in the dashboard, editable when needed, and queryable across sessions.


### The Cosmic Agents Shortcut


If you'd rather skip the infrastructure setup entirely, Cosmic ships[built-in agents](https://www.cosmicjs.com/ai/agents) that maintain their own memory automatically. They learn from every interaction and get better the more you use them. The memory layer described above is already wired in, so you just use the agents.


## Querying Agent Memory


The real power is in retrieval. Because each agent output is a structured Object with typed metafields, you can query across your entire agent history:


```text


```


You are filtering by structured properties, sorting by custom scores, and scoping by review status. The agent's memory is queryable the same way any other content in your system is queryable.


## Versioning: Know What the Agent Said When


Cosmic keeps a full revision history for every Object. Every time an agent updates a finding, or a human edits an agent output, the previous version is preserved.


This matters for auditability. If an agent's output informed a business decision, you need to know exactly what it said at the time of that decision, not just the current state. Cosmic gives you that without any extra infrastructure.


The same applies to prompts. When a prompt change shifts agent behavior, you can trace exactly which version was active and when. That's the kind of audit trail that matters as agents take on more consequential tasks.


## Using Cosmic with the MCP Server


Cosmic ships a native MCP Server, which means any agent running in Claude, Cursor, Windsurf, or any MCP-compatible runtime can read and write Cosmic Objects directly, with no custom API wrapper needed.


The MCP Server exposes all 18 Cosmic tools to your agent: create objects, update objects, query by type, filter by metadata, manage media, and more. An agent with MCP access to your Cosmic bucket can store its findings, retrieve past context, and update its own outputs in a single tool call.


See the[Cosmic MCP Server docs](https://www.cosmicjs.com/docs/api/mcp-server) to connect your agent in under 5 minutes.


## Schema Design for Agent Context and Memory


The key to making this work well is defining clean Object types upfront. Here are three schemas that cover most agent context and memory use cases:


*Object type:*


- (textarea): the agent's output in plain text
- (text): where the information came from
- (number, 0-1): how confident the agent was
- (text): which agent produced this
- (text): groups findings from a single agent run
- (switch): human review gate
- (objects, references tags type): topic classification


*Object type:*


- (select: user / assistant / system): the message sender
- (textarea): the message text
- (text): which agent this belongs to
- (text): groups a full conversation


*Object type:*


- (textarea): the full system prompt
- (number): prompt version number
- (textarea): what changed and why


All three types take about 3 minutes each to set up in the Cosmic dashboard. You can even ask your Cosmic Agent to set it up for you in seconds. Copy and paste this prompt:


```text


```


## What You Get Out of the Box


You could build this with Postgres and a custom schema. Here's what Cosmic includes that you'd otherwise have to build yourself:


- *Dashboard UI* : a human-readable, editable view of every agent output with no custom admin to build
- *Revision history* : built-in, no extra tables
- *REST API* : ready to consume from any runtime without writing endpoints
- *Draft/published workflow* : agent outputs start as drafts; humans promote them
- *Media handling* : if your agent produces images or files, or reads media for context, Cosmic's media library handles storage and CDN delivery
- *Model agnosticism* : Cosmic's REST API works with any model, any agent framework, any language


For most teams, the CMS handles everything a custom database would, faster and with less infrastructure to maintain.


## Getting Started


Three steps:


1. Create a free Cosmic account at[cosmicjs.com](https://app.cosmicjs.com/signup)
2. Use Cosmic to store your context and agent messages, or use our[built-in agents](https://www.cosmicjs.com/ai/agents) that maintain their own memory and learn more and get better the more you interact with them
3. Install the SDK: and start storing agent outputs


For agent workflows that need MCP access, connect your agent to the[Cosmic MCP Server](https://www.cosmicjs.com/docs/api/mcp-server) and your agent can query and update its own memory directly.


Want to talk through your agent architecture?[Book a quick call with Tony](https://calendly.com/tonyspiro/cosmic-intro) . Always happy to help teams think through their content and agent infrastructure.

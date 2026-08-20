---
schema_version: "1.0.0"
document_id: "1a2f3264a201216f9bda46d4268bd4410eb493cc20c3d16b39835e0a39339af1"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/mcp-in-action"
published_at: "2025-04-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:93a08f2e7edf8f04981ab75e89e548599a7c02a6cb297f204771f4ea2857d210"
---

# Model Context Protocol (MCP) in Action: Using Claude to interact with Quickchat AI Agent

> **Update (July 2026):** connecting Claude to a Quickchat AI Agent is now even simpler than in this demo: every agent has a hosted MCP server URL you add to Claude as a custom connector. The[step-by-step tutorial with the current flow is here](https://quickchat.ai/post/expose-ai-agent-as-mcp-server) .


This post is a demonstration of how two different AI systems can work together using **MCP (Model Context Protocol)** . In this example, Claude (from Anthropic) is connected to a Quickchat AI Agent that specializes in answering questions about a specific product: a fictional mobile phone called *Hello Phone* .


The result is a smooth, human-like conversation, even though two AIs are working together behind the scenes.


Watch the entire interaction here:


## What Is MCP?


**Model Context Protocol (MCP)** is a way for one AI agent to connect with another (or external tools). It lets one AI know:


*Here’s another AI that can answer this better than I can.*


When an AI that supports MCP (like Claude in this case) sees a question outside its expertise, it can send a request to another AI that has more specific knowledge. That model can then respond with relevant information, which is returned to the user.


Each AI stays specialized, and they coordinate when needed.


It was a very high-level explanation of MCP for the sake of this article, so I highly recommend reading more about how MCP works exactly in our[deep dive article](https://www.quickchat.ai/post/mcp-explained) .


## The Demo


In this setup, Claude has access to Quickchat AI Agent via MCP. When I message Claude and say:


“I’m looking for a new phone, maybe a Samsung”


Claude understands that my question might be better answered by another AI, and it knows it can use MCP to reach out to one.


It asks for permission to forward the request, and once I approve, it contacts the Quickchat AI Agent trained on Hello Phone data.


What follows is an **AI-to-AI conversation** . Claude explains the context to the Quickchat Agent, asks for product details, and returns a response to me.


You can even see both sides of the exchange:


**Right side:** My chat with Claude.


**Left side:** Claude’s chat with the Quickchat Agent.


When I asked for more product features, Claude again reached out to the Quickchat Agent, but it turns out the Agent’s Knowledge Base was limited. Claude relayed that back to me, apologized, and even passed along feedback when I suggested improving the Knowledge Base.


All of this happened without me needing to know anything about the underlying system.


This kind of architecture is useful in many areas: product support, internal tools, sales conversations, or AI agents that need to query company-specific knowledge.


Rather than building large systems that try to do everything, MCP allows us to build small, accurate systems that work together.


**Quickchat MCP is coming soon**


We’re working on releasing MCP support for Quickchat AI Agents, so you’ll soon be able to connect your own agents to numerous external apps and tools, enabling the kind of collaboration shown in this demo.


If you’d like to learn more about what it unlocks, and you haven’t already read our MCP guide, go here:[How Model Context Protocol works](https://www.quickchat.ai/post/mcp-explained) .

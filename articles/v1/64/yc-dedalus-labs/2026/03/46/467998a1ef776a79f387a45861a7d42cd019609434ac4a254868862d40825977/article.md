---
schema_version: "1.0.0"
document_id: "467998a1ef776a79f387a45861a7d42cd019609434ac4a254868862d40825977"
company_key: "yc-dedalus-labs"
company: "Dedalus Labs"
source_id: "yc-dedalus-labs-news-import-c6fd8a49c5f3"
canonical_url: "https://www.dedaluslabs.ai/blog/what-is-an-agent"
published_at: "2026-03-11T09:00:00+00:00"
first_seen_at: "2026-07-21T15:59:30.110359+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:30d5e441b42ddb83ebb150f5518f5f110197426369736be75b971509ca9364e0"
---

# Everyone is building agents. But what does that actually mean?

*It seems like everyone is building AI agents right now, yet no one really knows what an agent is.*


Picture this: you're on the road to work and need a way to prioritize and respond to your emails before you get to the office. An AI model can easily do this task, but first, you have to copy the email, paste it into the chat and prompt the AI model correctly, revise their draft a few times, send the email, and then repeat for all your other emails. At that point, you might as well just do this manually.


But imagine if there is an agent that can just natively read your emails, learn your voice, obtain all the context needed from your inbox/Slack/Notion, automatically respond to these emails on your behalf (or draft things for your approval), and even create calendar events based on their content. That could save you at least 5 hours a week.


However, in order for an agent like that to work, the underlying AI model(s) needs to have access to the correct tools that give it control over your email inbox, Slack, Notion, other work/productivity apps, and your preferred calendar.


TL;DR: Connecting an AI model to these tools is what transforms it into an AI agent, and Dedalus Labs is building the underlying infrastructure so any AI model can easily connect to any tool without 200 lines of code. Dedalus enables you to build production-ready agents that help you or your users save hours of time each week and get things done.


## How do we define agents?


At Dedalus, we define AI agents using this simple formula:


**Agents := Model(s) + Tool(s)**


An agent is at least one model (the brain) connected to at least one tool (the body). Tools enable models to interact with the real world, and tools can be APIs, MCPs, skills, or a local function call.


This equation is at the core of every agent you build. An agent is a collection of models and tools, which you can use to complete tasks, and a multi-agent architecture is just a combination of agents, aka. combination of models and tools with scoped access and context that is *situational* .


When you integrate an agent to any product or external service, the agent acts as the execution layer for that product. You specify intent and give your agent goals and autonomy across multiple steps while the agent decides how to execute on that intent at every step and what tools to use.


Think back to the email example. Creating an agent here is quite simple. You can connect your AI model to tools like an email client, your calendar, a simple web search tool, and add a speech integration. Then, as you commute to work, you could have a conversation with your voice agent, allowing you to reply to emails, identify high value leads from inbound messages, add events to your calendar, and everything else we previously discussed, all without taking your eyes off the road. The agent decides what sequence of actions it needs to take and the tools it needs to call, freeing you up to do other things.


Each part of the equation is essential. Remove the tools and you have a text generator. Remove the model and you have an automation script. The combination of a persistent goal (intent), a reasoning engine (brain), and the ability to act via concrete tools (body) is what makes something an agent.


Agents are:


- Connected to tools so they can take actions, not just generate text.
- Software systems that use an AI model to decide *what to do next* to achieve a goal.
- Able to observe results and iterate (plan → act → check → adjust).
- Designed to be repeatable and reliable, not just clever once.


Agents aren't:


- A single prompt or a static "chat with your data" experience.
- A deterministic automation or a rigid drag-and-drop workflow with no decision-making or reasoning capabilities.
- A wrapper around an AI model or video model that can only talk to you, not act.
- A Claude skill, which is just a markdown file that tells your AI model how to run a tool.


## How do you give your agent tools?


Now that we've defined what an agent is, the next question is: how do you actually build one?


To give an AI model access to tools, you need a way to tell the model what tools exist and how to use them. The simplest approach is to define tool schemas locally and let the model call functions directly. This works for small demos and weekend projects, but starts to fall apart as you scale. Tools get messy, integrations become hard to maintain, and models are more likely to hallucinate or call things incorrectly.


What's missing is a consistent standard for models to interact with tools *natively* .


That's where the Model Context Protocol (MCP) comes in.


MCP is an open standard that defines how models discover and use tools. Instead of wiring everything together manually, you expose your tools through an MCP server. The model can see what tools are available, send structured requests to use them, and receive structured responses back.


In practice, MCP acts as the communication layer between models and tools. It makes tool use more reliable, easier to scale, and much simpler to manage.[You can read more about MCP servers in our What is an MCP server? blog.](https://www.dedaluslabs.ai/blog/what-is-an-mcp-server)


Note that approaches like skills or CLI-based tools are still widely used and can work well, especially for local development or tightly scoped workflows. However, they become harder to manage when you want your agent to run beyond a single session (after you close your laptop) or share tool access across multiple users and agents. In those cases, tools need to be hosted and reliably accessible over a network. MCP makes this easier by standardizing how tools are exposed and used, and it also creates a natural path to production use cases like shared infrastructure and monetizable endpoints.


```text
flowchart TD
User("🧑 User (Driving)\n'Reply to Acme Corp and schedule a call'")
Agent("🤖 Voice Agent")
MCP("⚙️ MCP Layer\nroutes tool calls via tools/call")
Email("📧 Email Client\nsendEmail()")
Cal("📅 Calendar\ncreateEvent()")
Done("✅ 'Reply sent and Thursday call\nis on your calendar'")


User -->|Voice prompt| Agent
Agent -->|Discovers tools exposed by MCP,\nchooses what to call autonomously| MCP
MCP --> Email
MCP --> Cal
Email & Cal -->|Response routed back\nto agent via MCP| Done
Done -->|Voice response| User


classDef user fill:#1e3a5f,stroke:#3b82f6,stroke-width:2px,color:#93c5fd
classDef agent fill:#3b1f6e,stroke:#8b5cf6,stroke-width:2px,color:#c4b5fd
classDef mcp fill:#1e1b4b,stroke:#6366f1,stroke-width:2px,color:#a5b4fc
classDef tool fill:#14332a,stroke:#10b981,stroke-width:2px,color:#6ee7b7
classDef done fill:#1a2e2a,stroke:#2dd4bf,stroke-width:2px,color:#99f6e4
```


## Marketplace + get started now


Dedalus Labs provides access to hosted MCP servers and agent extensions through our[marketplace](https://www.dedaluslabs.ai/marketplace) , giving your agent ready-made, secure tools for tasks like web search, email, and more. These tools are fully hosted and can be accessed either via their external URL endpoints or by their marketplace slugs when building with our open-source SDK. Because you're passing sensitive data to these servers, we safeguard your secrets behind our own open-source authentication framework DAuth and guarantee that no tool or malicious actor can ever steal your secrets.[Read more about DAuth in this blog.](https://www.dedaluslabs.ai/blog/dedalus-auth-launch)


Ready to build your first agent? Get started in 5 lines of code with our[agent SDK](https://docs.dedaluslabs.ai/sdk/quickstart) or connect tools from our[marketplace](https://www.dedaluslabs.ai/marketplace) to your SDK or platform of choice using[external URLs](https://www.dedaluslabs.ai/blog/external-url-support) .

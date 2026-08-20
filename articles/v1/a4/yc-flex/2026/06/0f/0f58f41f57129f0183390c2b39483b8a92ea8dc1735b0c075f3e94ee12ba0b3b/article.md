---
schema_version: "1.0.0"
document_id: "0f58f41f57129f0183390c2b39483b8a92ea8dc1735b0c075f3e94ee12ba0b3b"
company_key: "yc-flex"
company: "Flex"
source_id: "yc-flex-news-import-797b5d589915"
canonical_url: "https://www.withflex.com/blog/custom-mcp-server-non-technical-merchants"
published_at: "2026-06-23T05:12:00+00:00"
first_seen_at: "2026-07-21T20:25:44.749599+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:666aa88dc0a131d3da5da6db15fd6f84966bf241845a175bf8bcdac6b7243fa8"
---

# MCP Use Cases: How We Built a Custom MCP Server for a Non-Technical Merchant

An **MCP server** is a small program that connects an AI assistant to a specific set of tools and data, using the[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) , the open standard[Anthropic](https://www.anthropic.com/) introduced so models like Claude can take actions, not just answer questions. This post is one concrete MCP use case: how Flex, the HSA/FSA payments platform, built a custom MCP server so a non-technical merchant could manage subscriptions and payments by chatting with an AI, with no API, no dashboard, and no support tickets. In effect, it turns a general AI assistant into an AI agent for customer support and merchant operations: one that can act, not just answer.


At Flex, we work with merchants of every technical background. Some have engineering teams that integrate with our[APIs](https://withflex.com/docs) on day one. Others are small operations (a few people running a[Shopify](https://www.shopify.com/) store) who need the same capabilities but have no developers to self-serve. For that second group, there's a gap: routine operations like updating a subscription or checking a payment status require either an API or a support ticket. We decided to close that gap with a custom MCP server. Here's exactly what we built, what it does, and what we learned.


## The problem: daily support tickets for routine tasks


One of our merchants, a growing brand with a loyal customer base, relies on Flex for their HSA/FSA checkout experience. They're great at what they do, but they're not technical: no developers on staff, no familiarity with REST APIs, no interest in learning one.


The result was a steady stream of near-daily messages that all boiled down to basic subscription and payment management: *"Can you cancel this customer's subscription?" "What's the status of this payment?" "Can you update this product's price?"*


Every one of these was trivial to execute: a few clicks in our admin tools or a quick API call. But the merchant still had to stop what they were doing, write to us, and wait for confirmation. For them it was a dependency. For us it was time spent on work that didn't need human judgment. This was an architecture problem: we were the human middleware between a non-technical user and a set of operations they should have been able to run themselves.


## What is an MCP server, and why it fit


MCP, the Model Context Protocol, is an open standard for connecting AI models to external tools and data sources. If a plain chatbot can describe what to do, an MCP server gives the AI the hands to actually do it: each "tool" the server exposes is a real operation the model can call on the user's behalf.


That mapped perfectly to our problem. The merchant was already communicating in plain English; they were just sending those sentences to us instead of to a system that could act on them. We'd already built internal AI tooling at Flex (see our post on the[AI Investigation Agent](https://withflex.com/blog/the-flex-ai-investigation-agent-for-hsa-fsa-payments) for the internal side). A custom MCP server was the same idea pointed outward, toward the merchant.


## What we built: the MCP server's tools


We built a custom MCP server scoped to exactly the operations this merchant kept asking us to perform. It exposes a focused set of tools (roughly 8–10) rather than our whole API surface:


**Tool**


**What the merchant says**


**What it does**


**Subscription management**


"Cancel the subscription for customer@email.com"


Create, update, cancel, and query subscriptions


**Payment operations**


"Why did jane@example.com's payment fail?"


Check payment status, look up transactions, surface failure reasons


**Product & price management**


"Update this product's price to $39"


Update pricing, toggle availability, read current config


**Customer lookup**


"Show me this customer's history"


Search customers by email, view history and current state


Each tool is scoped tightly. The merchant can only reach their own data; they can't touch system-level settings or other merchants' information. The server enforces the same permission boundaries we'd apply if they were calling our API directly; they just never have to know that's what's happening underneath.


## How it works: managing subscriptions and payments by chat


The merchant connects the MCP server to their preferred AI chat interface (an MCP-compatible client such as Claude). From there it's conversational:


**What the merchant asks**


**What the AI does**


"How many active subscriptions do we have right now?"


Calls the subscription tool, aggregates the data, and returns a clear count and summary.


"Cancel the subscription for john@example.com, effective immediately."


Looks up the customer, finds the active subscription, confirms the details, executes the cancellation, and returns a confirmation.


"What happened with jane@example.com's payment yesterday?"


Pulls the payment history, identifies the transaction, and explains whether it succeeded, failed, or is still processing, with error context if there is one.


There's no learning curve and no documentation to read. The merchant interacts with their Flex integration the way they'd interact with a knowledgeable teammate, except the answer is immediate.


## How to build a custom MCP server: the steps we followed


If you want to reproduce this pattern, here's the sequence we used:


1. **Inventory the operations.** List the handful of tasks the user actually performs today: for us, the 8–10 recurring subscription, payment, product, and customer requests.
2. **Expose each as a scoped tool.** Map every operation to one MCP tool with a tight input and output, rather than re-exposing your whole API.
3. **Enforce the same permission boundaries as direct API access.** The merchant reaches only their own data; system-level settings stay off-limits.
4. **Ship read-only tools first.** Lookups, status checks, and summaries let the user (and you) build trust before anything mutates state.
5. **Add write operations once the guardrails are proven.** Cancellations and updates come online only after the read-only phase holds up.


## What we learned building a custom MCP server


**Scope matters more than breadth.** Our first instinct was to expose everything; we pulled back fast. The merchant doesn't need webhook configuration or API-key management; they need the 8–10 operations they actually perform. A tight tool set means fewer failure modes, clearer responses, and less risk.


**Error handling is the product.** When a merchant types "cancel subscription for john@example" and that customer doesn't exist, the quality of that error response is the experience. We spent real time making failures explain themselves and suggest a next step ("No customer found with that email. Did you mean john@example.com?") instead of returning raw error codes.


**Non-technical users are the best testers.** They don't read docs and they don't follow expected patterns. They type "get rid of John's sub" and expect it to work. That forced us to handle intent in a way that made the whole system more robust.


**Trust is built incrementally.** We started with read-only tools: lookups, status checks, summaries. Only after the merchant was comfortable and we'd validated the guardrails did we enable write operations like cancellations and updates. That phased rollout meant we never had an "the AI did something unexpected" moment.


## Results: from daily tickets to merchant self-service


The merchant went from sending us near-daily messages for routine operations to running them directly. Cancellations, payment-status lookups, price updates, and customer history checks (the four request types that used to land in our inbox) are now self-served through the AI. Our team gets that time back for work that actually benefits from human judgment: complex troubleshooting, proactive merchant success, and building more tools like this one.


The bigger shift was in the relationship. The merchant moved from depending on us for day-to-day operations to genuine ownership of their Flex integration: from dependency to autonomy.


### Current limitations


This is an honest known-state, not a finished product: this deployment is scoped to a single merchant, the tool set is deliberately narrow (no webhook or API-key management exposed), and each new merchant needs their own tools and permission boundaries configured. The infrastructure is reusable; the configuration is per-merchant.


## When does a custom MCP server make sense?


We scoped this server to a single merchant, but the pattern is repeatable. The core infrastructure stays the same; what changes per merchant is the specific set of tools exposed and the permission boundaries applied. A custom MCP server, effectively an AI customer service agent that can act, is a strong fit when three things are true:


- **The user can't easily self-serve** through an API or a complex dashboard. That's common for non-technical users, but it's also true whenever the existing API or dashboard is too cumbersome to bother with.
- **The requests are routine and bounded:** a handful of recurring operations, not open-ended judgment calls.
- **The relationship is high-touch:** they're already messaging a human for these tasks today.


And MCP servers aren't only for non-technical users; technical teams reach for them too, to collapse routine, multi-step API work into a single request. If your team is the human middleware for a handful of routine requests, that's the signal. The tooling has matured to the point where a focused integration server is a matter of days, not months. Start the way we did: inventory the 8–10 operations your users actually ask for, and expose the read-only ones first.


*This is the first in a series on support engineering at Flex, covering how we use AI tooling to scale merchant support without scaling headcount.*

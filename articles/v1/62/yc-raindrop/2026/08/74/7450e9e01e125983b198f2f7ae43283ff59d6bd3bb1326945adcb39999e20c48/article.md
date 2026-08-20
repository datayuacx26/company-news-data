---
schema_version: "1.0.0"
document_id: "7450e9e01e125983b198f2f7ae43283ff59d6bd3bb1326945adcb39999e20c48"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/mcp-design/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-08T03:28:34.827635+00:00"
fetched_at: "2026-08-08T03:28:36.827308+00:00"
content_hash: "sha256:d7745c2fb71e4a4bc61d302b5729e38c0a92d5197c9c2464151d5d939e029b39"
---

# MCPs need to be designed too

Recently, based on feedback from a customer, we gave our MCP server a deeper look.


Without using the Raindrop app, we wanted to see from our MCP server and coding agents if they were capable of putting a conversation together, specifically long, multi-turn conversations. Watching the agent in action, each tool worked fine in isolation, but the conversation still came out fragmented and the context window blew up. The agent bounced between cheap and expensive calls in no particular order, and each call returned part of the picture, but no direction on what tool to use next. This resulted in a brute force solution that lacked any efficiency or predictability.


MCP design thus far has been “throw tools in a basket” but it's important now to think of the agent experience. Here's how we redesigned ours.


## The fix: a tool hierarchy


No amount of markdown scaffolding could fix this. Skills and better descriptions are certainly useful, and we use them to the fullest. But it's hard to stake success on a markdown file. Infrastructure that's built on the same principles, and enforces them, is far more reliable. So we rebuilt the conversation tools as a funnel where tools don't overlap but build on each other.


-


**get_conversation** — the wide view. Slim turns, tool calls reduced to counts.


-


**list_events** /


**get_event** — the middle. Full turn logic.


-


**get_trace** — forensics. Every span and payload, for one event you've already identified.


Each tier provides the minimum context needed to answer the question:


**get_conversation** returns a snapshot of tool use instead of the full payload.


The funnel works because navigating between layers requires data you can only get from the layer above. It enforces direction without adding guardrails that would make these tools rigid.


Finally, our skills and infrastructure can work together. Before, a skill had to hand a tool specific instructions to make it useful for a given job, and without those instructions it wouldn't work. The skills enhance the workflow no question, but chaining tools together is so much easier now because of how simple it is to reach the next one.


## The results


We ran evals on our production traces and found we cut cost by 12% and time to final answer by 27%.


Massive conversations can now be traced. The ones that need it most can be handled within 6 tool calls.

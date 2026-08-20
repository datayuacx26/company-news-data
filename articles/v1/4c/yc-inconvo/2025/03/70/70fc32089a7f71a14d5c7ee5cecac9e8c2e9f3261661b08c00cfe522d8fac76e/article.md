---
schema_version: "1.0.0"
document_id: "70fc32089a7f71a14d5c7ee5cecac9e8c2e9f3261661b08c00cfe522d8fac76e"
company_key: "yc-inconvo"
company: "Inconvo"
source_id: "yc-inconvo-news-import-030a8db3acc0"
canonical_url: "https://inconvo.com/blog/inconvo-langchain/"
published_at: "2025-03-19T22:00:00+00:00"
first_seen_at: "2026-07-25T09:26:00.881998+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:5cf2dfc05d708648f65f9ab9a967a10c4a9322f2c20fc23928a956af093299fc"
---

# Inconvo + LangChain

We use[LangGraph](https://www.langchain.com/langgraph) and[LangSmith](https://www.langchain.com/langsmith) to power Inconvo.


LangGraph powers our analytics agent which we make available through our API and LangSmith powers our Evaluation and Monitoring interfaces.


### LangChain’s Case Study


We were delighted when LangChain reached out to partner with us and undertake a[case study of Inconvo](https://blog.langchain.dev/customers-inconvo/) .


LangChain were particularly interested in how we built **a powerful query processing system** with LangGraph.


Inconvo’s architecture utilizes LangGraph to manage conditional workflows, where different operations can be executed based on the user’s input. This includes selecting tables, executing SQL queries, and returning structured outputs in various formats. By integrating with LangGraph, Inconvo can handle complex queries with multiple steps, ensuring that users receive accurate and relevant results quickly.


### Powerful Engine - Simple Interface


We make our powerful query processing system easily available through our simple API.

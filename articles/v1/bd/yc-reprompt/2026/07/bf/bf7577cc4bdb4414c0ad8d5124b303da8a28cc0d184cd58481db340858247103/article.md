---
schema_version: "1.0.0"
document_id: "bf7577cc4bdb4414c0ad8d5124b303da8a28cc0d184cd58481db340858247103"
company_key: "yc-reprompt"
company: "Reprompt"
source_id: "yc-reprompt-news-import-f6b2fbe9777c"
canonical_url: "https://repromptai.com/blog/search-and-ask-api"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T00:12:22.152508+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:82ae38dadb8d615cdd6339e2a4d12ece91a75a4b83a0ae1da5bfb86a65846fad"
---

# Introducing Search and Ask APIs

Search is probably the most requested feature from Reprompt customers building agents.


The request is usually simple: give the agent access to the web without making us adopt another search payload, scraping system, or research framework.


We built two endpoints because there are two different jobs hiding inside that request.


## Search


` POST /v1/search` returns ranked web results: title, URL, snippet, and position. Reprompt resolves the organization from the authenticated API key.


```text
{
"query": "Where is the visitor entrance to the Empire State Building?"
}


```


```text
{
"query": "Where is the visitor entrance to the Empire State Building?",
"results": [
{
"position": 1,
"title": "FAQs: Tickets, Hours, & Directions",
"url": "https://www.esbnyc.com/visit/faq",
"snippet": "The Observatory entrance is located at 20 West 34th Street."
}
]
}


```


This is the endpoint for agents that already have their own reasoning loop. Search gets the web into a stable shape. The agent can rerank it, filter it, cite it, or pass it to another model.


## Ask


` POST /v1/ask` takes the same input and returns a short answer with inline citations.


```text
{
"query": "Where is the visitor entrance to the Empire State Building?",
"answer": "The visitor entrance is at 20 West 34th Street in Manhattan. [1]",
"sources": [
{
"position": 1,
"title": "FAQs: Tickets, Hours, & Directions",
"url": "https://www.esbnyc.com/visit/faq"
}
]
}


```


Ask performs one search and one fast model call. It is useful when the agent—or the person using it—needs an answer rather than a result set.


This is not deep research. There is no planner, browser loop, or ten-minute report. Ask is meant to be a quick, bounded web primitive.


## Same input, same price


Both endpoints accept one plain query object. Both cost **$5 per 1,000 requests** . Ask has no separate token surcharge.


We use this Search/Ask split internally because it makes tool selection boring. If the agent needs evidence, call Search. If it needs a grounded explanation, call Ask.


Search and Ask are available in Reprompt v1.


[Read the API documentation.](https://docs.repromptai.com/guides/search-and-ask)

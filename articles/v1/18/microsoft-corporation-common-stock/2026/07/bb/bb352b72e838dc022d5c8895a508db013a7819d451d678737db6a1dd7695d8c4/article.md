---
schema_version: "1.0.0"
document_id: "bb352b72e838dc022d5c8895a508db013a7819d451d678737db6a1dd7695d8c4"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/how-to-test-agent-skills-without-hitting-real-apis/"
published_at: "2026-07-17T09:27:50+00:00"
first_seen_at: "2026-07-20T04:34:25.944233+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:d8f4f8e9b84a69e680d5e000f61327bffd3c935ad13f80f0b4e7cbc9a0ffa96e"
---

# How to test agent skills without hitting real APIs

You shipped a skill that calls an API. The agent uses it, user get results. But how do you know the results are *good* ? How do you know your last change didn’t quietly break the happy path, or that switching models won’t regress the scenarios you already got right?


You need to evaluate. Run your skill through a set of scenarios, score the outputs, compare across iterations. Without that, you’re[shipping blind](https://developer.microsoft.com/blog/is-your-agent-extension-actually-working/) and hoping for the best.


## The problem nobody warns you about


The moment you start evaluating a skill that calls an API, things get complicated.


If that API belongs to an external service, every eval run costs money. Say you’re running 50 scenarios across 3 models with 5 repetitions per scenario. That’s *at least* 750 API calls per session. Multiply by every iteration as you tune prompts and try different models. The meter is running, and you’re paying for test traffic that produces no user value.


But even when the API is yours and costs nothing to call, you still have a problem. Once your skill performs writes, PATCH calls mutate state and DELETE calls remove records. Your eval harness is changing live data as a side effect of measurement. You either need to manually reset state between runs or accept that your eval results are contaminated by prior runs altering the data they depend on.


And there’s one more subtler issue. If other systems or people are also writing to that same API, your eval results become non-deterministic. A score drops between two runs not because you changed anything, but because someone in another team updated a record your scenarios depend on. (If this sounds familiar, it’s the same class of problem as[the hidden variables](https://developer.microsoft.com/blog/the-hidden-variables-in-your-agent-eval/) that plague every eval, just expressed through API state instead of model temperature or file ordering.)


## Why most teams skip this entirely


This is exactly why most skill builders don’t evaluate at all. They test manually (“does it work when I ask it?”), ship, and move on. Rigorous evaluation feels like it requires either a dedicated test environment nobody has budget for, or accepting the risks above.


The result? Skills that work on Tuesday break on Friday when[the model updates](https://developer.microsoft.com/blog/not-all-model-upgrades-are-upgrades/) . Skills that look great in a demo fail when the API returns slightly different data. Nobody knows because nobody measured.


## The mock server trap


If you’ve done testing in the past, the obvious answer is to stand up a mock. Build a local server that mimics your API, and point your skill at localhost running your evals in isolation. Deterministic and clean.


In practice, this means writing and maintaining a fake server. You need to handle multiple endpoints, support filtering and pagination, return realistic payloads, respond correctly to PATCH and DELETE semantics. For a simple API, that’s an afternoon. For anything beyond a handful of endpoints, it’s a project that drifts out of sync with the real API the moment someone ships a change.


There’s a deeper problem though. To point your skill at localhost, you need to change the URLs in the skill file. Which means you’re no longer evaluating the skill you’ll actually ship. Every token in the context window influences the model’s reasoning, and swapping` https://api.contoso.com/products` for` http://localhost:3000/products` is a different set of tokens. You’ve introduced a variable into the very thing you’re trying to measure.


## A lighter-weight approach


It turns out, that the bar for “good enough” API emulation is lower than most people assume. You don’t need a full replica of your backend. You need stable URLs and realistic payloads with correct HTTP semantics for the operations your skill actually performs.


[Dev Proxy](https://aka.ms/devproxy) does this with a JSON configuration file. You define the base URL, the endpoints, and some seed data. Dev Proxy intercepts HTTP traffic from your agent and responds locally, without any server to build or maintain. Your skill keeps calling the real URLs and doesn’t know or care that a proxy is answering instead of the real server:


```text
{
"baseUrl": "https://api.contoso.com/products",
"dataFile": "products-data.json",
"actions": [
{ "action": "getAll" },
{ "action": "getOne", "url": "/{product-id}", "query": "$.[?(@.ProductID == {product-id})]" },
{ "action": "merge", "url": "/{product-id}", "query": "$.[?(@.ProductID == {product-id})]" },
{ "action": "delete", "url": "/{product-id}", "query": "$.[?(@.ProductID == {product-id})]" }
]
}


```


The data resets on every run. No production records harmed, no API costs, no “who deleted all the test orders” conversations. When you’re done testing, remove the proxy and your skill works against production without code changes. There’s a[complete sample](https://devproxy.net/samples/skill-api-emulation/) with a product catalog API and skill definitions for GitHub Copilot CLI, Claude Code CLI, and Codex CLI if you want to see the full working setup.


## The point isn’t the tooling


Whatever approach you choose, the principle is the same: you wouldn’t run integration tests against a production database. Why would you run eval scenarios against a production API?


If your skill calls an API, you need a way to evaluate it without incurring costs or mutating live data. Mock servers work but require maintenance. Proxy-based emulation is lighter. Pick whichever fits your situation, but pick *something* , because the alternative is shipping skills you’ve never systematically measured.


And that’s the real problem. Not the tooling, but the fact that most skill builders haven’t internalized that evaluation is part of the job. If you’re ready to start, the building AX evals article covers[how to structure scenarios and criteria that produce real signal](https://developer.microsoft.com/blog/building-ax-evals-that-actually-work/) . Try it and let me know what you think!

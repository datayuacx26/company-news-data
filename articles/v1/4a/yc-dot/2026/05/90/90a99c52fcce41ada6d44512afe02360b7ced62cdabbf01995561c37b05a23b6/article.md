---
schema_version: "1.0.0"
document_id: "90a99c52fcce41ada6d44512afe02360b7ced62cdabbf01995561c37b05a23b6"
company_key: "yc-dot"
company: "Dot"
source_id: "yc-dot-news-import-8b97b36288bf"
canonical_url: "https://www.getdot.ai/blog/preparation-time-vs-query-time"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-21T16:52:13.925276+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:79154fbf26f99bf44fa1e152fca09c669a459f5703713943d530bc6d683c7be8"
---

# Pay at Preparation Time or Pay at Query Time

**There is no free lunch in analytics. Every question your company asks has a cost, and the only real choice is when you pay it.**


Someone, or something, has to know which tables matter, what the metric means, which rows to exclude, and why last quarter's number changed. That work doesn't disappear when an AI starts doing the answering. It moves. And where it moves is one of the most consequential design decisions in your data stack.


## The two extremes


Classic BI paid everything upfront. Weeks of modeling, metric definitions, dashboard building, all before a single question got answered. Call it preparation time. The problem wasn't prepayment itself. It was prepaying blind: you guessed what people would ask and built dashboards for all of it. Every data team knows the graveyard. A hundred dashboards, six with viewers. That's prepaid cost for questions nobody asked.


"Chat with your data" tools swung to the other extreme: pay everything at query time. No setup, point the agent at the warehouse, ask away. It demos beautifully. But now the agent rediscovers your business from scratch on every question. Which revenue column is canonical. Whether trials count as customers. Why EMEA is split differently since the reorg. You pay in latency, you pay in tokens, and you pay in the worst currency available: inconsistency. Ask the same question twice, the reasoning runs twice, and you can get two different answers. Nothing kills trust in an AI analyst faster.


If you want the finance framing: preparation is capex. You invest once, it serves thousands of queries, and it depreciates slowly as the business changes. Query-time work is pure opex. It recurs on every single question, forever, at whatever scale you reach.[Ramp's internal agent answers 1,800 questions a month](https://www.getdot.ai/blog/ramp-research-analyst-agent-slack-dbt) . At that volume, you really don't want to re-derive what "active customer" means 1,800 times.


## Choosing what to prepay


The skill is not prepaying everything. That's the dashboard graveyard again. The skill is finding the fifty definitions, entities, and rules that sit underneath 80% of the real questions in the business process you care about, and prepaying exactly those.


Notice that this requires knowing which business process you care about. The worst failure mode I see isn't technical. It's a team buying an AI analytics tool with no target use case, throwing it over the fence, paying nothing at preparation time, and concluding that AI analytics doesn't work. Technology first, use case never. No payment plan survives not knowing what you're buying.


## Payments that compound


The design question that actually separates AI analytics tools: can query time fund preparation time?


When an agent answers a new question and a human confirms or corrects the result, that work shouldn't evaporate. The definition it used, the join it figured out, the exclusion it learned: each one can become preparation for the next thousand questions. The first time a question gets asked, you pay query-time prices. Every time after that, it's prepaid. This is how we think about training in Dot. Every reviewed answer becomes an asset, and the cost per question falls with use.


That's the test I'd apply when evaluating any AI analytics setup. Not "how good are the answers today" but "do the payments compound." A stack where every question costs what it cost yesterday is a treadmill. A stack where every answer makes the next one cheaper is an asset.


There's still no free lunch. But there's a meal plan that gets cheaper every week, and it's worth choosing on purpose.


*If this excites you, we'd love to hear from you.*[Get in touch](https://www.getdot.ai/) *.*

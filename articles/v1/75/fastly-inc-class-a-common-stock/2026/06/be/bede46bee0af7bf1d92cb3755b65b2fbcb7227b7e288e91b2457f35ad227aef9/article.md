---
schema_version: "1.0.0"
document_id: "bede46bee0af7bf1d92cb3755b65b2fbcb7227b7e288e91b2457f35ad227aef9"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/ai-traffic-grew-6-5x-faster-than-human-traffic-this-year/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c7117f66957658ae9f9103586ddf1edb5b75a9956b103ce7e81c83bf551a1755"
---

# AI Traffic Grew 6.5x Faster Than Human Traffic This Year

Fastly has observed rapid growth of AI traffic on our network. From January through May 2026, AI requests on our platform have grown approximately 30%, **roughly 6.5x faster than human traffic** in the same timeframe. Growth compounds fast, and at internet scale it can translate into billions of additional automated requests.


Diving into the data across our platform, we see **autonomous machine-to-machine traffic approaching half of all internet requests** , including crawlers, bots, agents, API-driven systems, and other machine-initiated interactions. Machines operate differently, both in volume and velocity. They can discover, request, compare, and act in seconds, raising the stakes for visibility, relevance, and long-term competitiveness.


## **The New Reality of AI Traffic**


AI traffic is not a single workload but two very different categories: AI crawlers and AI fetchers.


**AI crawlers** are bulk, schedule-driven crawlers that systematically read the open web to assemble training corpora for large language models. They behave much like traditional search-engine crawlers and sweep across the web continuously.


**AI fetchers** point to an emerging pattern: agents acting on behalf of users. Unlike crawlers, fetchers are typically tied to a more immediate request. They pull information to answer a question, compare options, check availability, summarize content, validate facts, or handle a transaction.


Fetchers are not on a schedule. Their volume tracks agentic workload and AI assistant adoption. The more agents operate on behalf of users, the more traffic is generated. We expect to see more traffic driven by users prompting ChatGPT, Gemini, and Claude. Because fetchers are more targeted and intent-driven than broad crawlers, these different machine traffic models require entirely different business decisions.


Some AI traffic is growing remarkably fast: we are seeing a massive growth in Claude traffic, with **more than 555% growth** over its January baseline.


Overall, in May 2026, AI bot requests on Fastly's network were **85% crawlers and 15% fetchers.**


### **AI Traffic Behaves Differently Than Humans**


Human traffic is episodic. It follows time zones, workdays, weekends, habits, and intent. AI crawler traffic is far less tied to those traditional patterns. Our platform data shows **AI crawler activity remains relatively consistent across the entire 24-hour cycle.** These systems do not sleep, pause, or wait until morning. They continuously gather, process, and synchronize information across geographies and time zones. AI systems can discover, request, compare, and act at machine speed, pulling businesses into a much more real-time economy.


The fetcher pattern is just as important. AI fetchers appear closer to human rhythms because they are often tied to user action: a person asking a question, comparing options, checking availability, or trying to complete a task. That makes them automated but not detached from human intent.


This dynamic shows a new operating layer on top of the internet: **always-on, automated, and increasingly intelligent agents.** It’s sitting beside human activity, creating a second traffic model that behaves, scales, and stresses infrastructure differently.


### **AI Traffic Requires a Smart Delivery Strategy**


AI systems introduce a new pattern. Our global network data based on May 2026 data shows that less than 9% of human requests require a trip back to origin infrastructure. By contrast, **more than 51% of automated AI requests require us to pull directly from origin servers,** rather than just serving cached content. That means on a request-by-request basis, AI workloads interact with origin infrastructure **more than six times as often** as human users.


AI traffic is often looking for what changed: current inventory, live prices, recent articles, fresh product data, updated policies, new availability, revised documentation, and real-time context. **AI systems use the internet as a live data source.**


Together, these patterns show that AI traffic is changing what businesses need from web infrastructure. You need to serve what can be cached efficiently, route what needs fresh data intelligently, and protect origin infrastructure when automated systems request live information at machine speed.


### **The Next Step**


While the data reveals a massive architectural shift, numbers only tell half the story. The real challenge is determining how your business responds to this surge.


[In part two of this series](http://fastly.com/blog/bot-defense-table-stakes-machine-traffic-requires-business-strategy) *, our CMO, Joan Jenkins, breaks down the business implications of this data and maps out the agentic strategy required to turn machine risk into a competitive advantage.*


### **Methodology**


The data analyzed in this report spans Fastly's global infrastructure from January 1 through May 31, 2026. To isolate organic traffic trends from customer onboarding or traffic shifts, network-wide metrics are based on a fixed cohort of customers with selected exclusions to prevent isolated known machine-to-machine traffic ramps from overestimating bot traffic or distorting broader network baselines.


AI traffic was classified using Fastly’s edge detection signals. Time-series growth metrics index daily request volumes against a January 2026 baseline average, utilizing a 30-day trailing moving average to suppress standard weekly cyclicality. Finally, origin-reach calculations measure the proportion of requests routed directly to origin infrastructure by evaluating request volume strictly against the response source dimension.

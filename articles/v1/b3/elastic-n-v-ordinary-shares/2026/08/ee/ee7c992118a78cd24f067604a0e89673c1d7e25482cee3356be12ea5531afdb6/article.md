---
schema_version: "1.0.0"
document_id: "ee7c992118a78cd24f067604a0e89673c1d7e25482cee3356be12ea5531afdb6"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T16:31:09.223823+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:bf97a9946946f282c39a4fb1947e266c10faf372fbbaac6935a4f891545fd0c6"
---

# 15 lines of click tracking code that tell you what search logs can't

Search volume and latency tell you that search is working, not that it's useful. About 15 lines of OpenTelemetry (OTel) instrumentation lets you track clicks on search results and then query click-through rate (CTR), Mean Reciprocal Rank (MRR), and click position distribution with Elasticsearch Query Language (ES|QL) against the same traces index that your search spans already live in. You'll wire click tracking to your existing` search.query_id` and write the queries that show which searches need relevance tuning.


## What you'll discover


In this post, you'll learn how to:


-


Add client-side click tracking that links clicks back to their originating search via` search.query_id` .


-


Calculate CTR; that is, the percentage of searches that produce at least one click.


-


Calculate MRR; that is, how far down the results users click on average.


-


Analyze click position distribution to see the full shape of user engagement.


-


Write ES|QL queries for all three metrics against` traces-generic.otel-default` .


-


Identify which specific queries need relevance tuning.


### What you'll need


-


A working OTel instrumentation setup from Blog 2 (search spans with` search.*` attributes flowing to Elastic via OTel-native ingestion).


-


A front end that can send click events (JavaScript example provided).


-


Familiarity with the` attributes.*` field mapping from Blog 2.


## Why search logs alone can't measure search quality


In the[second blog](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry-esql) of the series, we instrumented search requests and ran six ES|QL queries against the data. We can see what users search for, which queries return nothing, and how fast search is.


But there's a blind spot. A search that returns 15 results looks healthy from the server side. Every metric we have says it worked. But if nobody clicks any of those results, your ranking has a problem, and none of the queries from Blog 2 will tell you.


This is the gap between *results returned* and *results that are useful* . Search volume, zero-results rate, and latency measure the mechanics of search, but they don't measure whether search is actually helping users find what they need.


To answer that question, you need a second instrumentation point: *click tracking* .


If you’re following along with code, the[reference project](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) has click tracking ready to enable. Uncomment the Blog 3 sections in` app.py` and` frontend/app.js` , restart, and then generate traffic with` python generate_traffic.py --blog 3` .


## How click data measures search relevance


Before we write any code, here's what click data lets you measure and why each metric matters:


-


**CTR** answers the most basic engagement question: *What percentage of searches result in at least one click?* If your CTR is low, users are seeing results but not finding them compelling enough to engage. Establish your own baseline once you have data; CTR varies considerably across product category, query type, and industry vertical.


-


**MRR** goes deeper: *When users do click, where in the results are they clicking?* An MRR of 1.0 means every user clicks the top result (a perfect ranking). An MRR of 0.5 means the average click is at position 2. Low MRR with high CTR is particularly telling. It means that users are finding what they need, but your ranking is making them work for it.


-


**Click position distribution** shows the full shape of where users click. A healthy search engine shows most clicks at position 1 with a sharp drop-off. A flat distribution across positions 1–5 means that your ranking isn't differentiating well. Per-query distributions reveal exactly which searches need relevance tuning.


Together, these metrics move you from *Did search work?* (Blog 2) to *Did search work well?* , and they pinpoint exactly where to invest in relevance improvements. Later in the series, we'll show how to turn these metrics into concrete actions: building judgment lists for Learning To Rank (LTR), tuning relevance with tools like[Elasticsearch Relevance Studio](https://elastic.github.io/relevance-studio/#/) , and evaluating changes with the Rank Eval API. But first, you need the data.


All three metrics require just one new instrumentation point: about 15 lines of code.


## Add click tracking


Click tracking captures what happens after the results appear. When a user clicks a search result, we create a new span with attributes describing the interaction, including which document they clicked, where it appeared in the results, and which search produced it.


Here's the code:


```text
# Track which query_ids have already received a click
_clicked_queries: set[str] = set()


@app.post("/api/events")
async def track_event(event: EventRequest):  # reference project uses ClickEvent
with tracer.start_as_current_span("search.result.click") as span:
span.set_attribute("search.action", "click")
span.set_attribute("search.result_click_id", event.object_id)
span.set_attribute("search.result_click_position", event.position)
span.set_attribute("search.result_click_type", event.object_id_type)
span.set_attribute("search.query_id", event.query_id)
span.set_attribute("enduser.pseudo.id", event.client_id)


# First click per search — enables single-query CTR
if event.query_id not in _clicked_queries:
span.set_attribute("search.first_click", True)
_clicked_queries.add(event.query_id)


if event.user_query:
span.set_attribute("search.query", event.user_query)
```


Let's unpack what matters.


### Click spans are separate traces


This is the key architectural difference from Blog 2. Search spans are created synchronously during the API request: The user searches, the span opens, Elasticsearch responds, and the span closes. Click spans are *asynchronous* . The user searches, gets results, browses the page, and might click 30 seconds later (or they might never click).


That means click spans aren't children of the search span's trace. They're independent traces, linked to the originating search through` search.query_id` . This is the same` query_id` we derived from the trace ID in Blog 2, and it now serves as the join key between searches and clicks across` traces-generic.otel-default` .


### Choosing the right OTel signal for clicks


OTel gives you three signal types, and clicks could be modeled as any of them. Each has strengths:


**Signal**


**Index**


**Weight**


**Best for…**


Spans


` traces-generic.otel-default`


Full trace context


Same-index queries with search spans


Logs


` logs-*`


Lighter weight


Log-centric pipelines, high volume


Span events


` logs-generic.otel-default`


Lightest instrumentation


Attaching to existing spans


-


**Spans** land in` traces-generic.otel-default` alongside search spans, are fully queryable in ES|QL, appear in Kibana APM views, and carry timing information. Since our search spans are already in` traces-generic.otel-default` , using spans for clicks means you can query searches and clicks together in a single ES|QL statement, and no cross-index joins are needed.


-


**Log records** are also independently queryable in ES|QL, living in` logs-*` . If you use the same` search.*` attribute names, the analytics queries are almost identical; just change the index pattern. Logs are lighter weight (no trace context overhead) and are a natural fit if your team already has a log-centric observability pipeline. One of Elastic's strengths here is that traces, logs, and metrics all land in the same platform and are all queryable with ES|QL, so choosing logs over spans doesn't mean giving up any query capability.


-


**Span events** are lightweight at instrumentation time (attached to an existing span in the OTel API). In Elastic's OpenTelemetry Protocol (OTLP) ingestion pipeline, span events are written as separate documents to` logs-*` data streams (for example` logs-generic.otel-default` ), and they’re independently queryable in the same way as logs. They’re a good, lightweight option but might require more code changes than logs, which can even pull in logs from legacy code.


In this series, we use *spans* because they keep searches and clicks in the same index with the simplest query path. But if you're at high volume and want to optimize for cost, or if your organization already routes OTel logs to Elasticsearch, the log-based approach works well; the` search.*` attribute schema is the same either way, and ES|QL queries against` logs-*` follow the same patterns you'll see below.


### The` search.first_click` attribute


` search.first_click` is a Boolean set only on the first click for a given` query_id` . It exists for one reason: accurate CTR calculation without post-processing.


CTR is defined as the percentage of searches with at least one click. Without` search.first_click` , you'd need to deduplicate clicks by` query_id` at query time: grouping, counting distinct values, and subquerying. By marking the first click at instrumentation time, the ES|QL query becomes a simple count.


The set above is demo-only. It grows unbounded and breaks with multiple API replicas. The[reference implementation](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) uses a thread-safe time-to-live (TTL) dict with a 30-minute expiry window (` _is_first_click()` in` app.py` ). For production with multiple replicas, use a shared external cache (Redis, Memcached) keyed by` query_id` with a TTL matching your session window.


### Where to track first click: Front end vs. back end


The` search.first_click` deduplication could live in either the front end or the back end. Both are valid, and here are the trade-offs:


-


**Frontend tracking** is simpler to implement. The browser already knows the current query and whether the user has clicked before. No server-side state is required, you don’t have to worry about multiple replicas, and it works without any backend changes. The downside is that browser state is ephemeral; a page refresh, multiple tabs, or an ad blocker can interfere with accurate tracking.


-


**Backend tracking** (our approach) gives you a single source of truth. All click events flow through one place, so the deduplication is consistent regardless of what the client does. It also means that the analytics logic is colocated with the instrumentation code, which simplifies reasoning about data quality. The trade-off is that the back end needs to maintain state: the` _clicked_queries` set. For a single-instance API, this is trivial; for multiple replicas behind a load balancer, you'd use a shared TTL cache (Redis or similar).


We chose backend tracking here because we want the analytics data to be authoritative. This click data will later feed into relevance tuning and judgment lists, where accuracy matters. But if you're starting simple or running a client-side–only setup, frontend tracking is a perfectly reasonable first step. The` search.first_click` attribute works the same way regardless of where you set it.


### Sending click events from the browser


The browser sends click events to the back end when a user clicks a result. It needs three things from the search response: the document ID, the position, and the` query_id` .


```text
// Generate a persistent client ID once per browser (stored in localStorage)
const CLIENT_ID = localStorage.getItem("search_client_id")
|| (() => {
const id = crypto.randomUUID();
localStorage.setItem("search_client_id", id);
return id;
})();


// On result click
fetch('/api/events', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({
object_id: product.id,
position: index + 1,       // 1-indexed
query_id: lastQueryId,     // from the most recent search response
client_id: CLIENT_ID,      // persistent browser identifier → enduser.pseudo.id
user_query: currentQuery,
object_id_type: 'product', // optional; defaults to "product" on the backend
})
});
```


` CLIENT_ID` is generated once and stored in` localStorage` . It survives page reloads and gives you a stable` enduser.pseudo.id` without requiring a login. The back end maps` client_id` →` enduser.pseudo.id` on the span.


Positions are 1-indexed; that is, the first result is position 1, not 0.


### Click tracking OTel attributes and ES|QL field mapping


**Attribute**


**Type**


**Required**


**Purpose**


` search.action`


string


yes


Event type:` "click"`


` search.result_click_id`


string


yes


Document ID clicked


` search.result_click_position`


int


yes


Position in results (1-indexed)


` search.query_id`


string


yes


Links to originating search


` enduser.pseudo.id`


string


yes


Client/device identifier


` search.first_click`


boolean


recommended


` true` if first click for this` query_id`


` search.result_click_type`


string


recommended


Object type:` "product"` ,` "article"`


` search.query`


string


recommended


The search query text (for queryability)


These follow the same` search.*` namespace we established in Blog 2. With OTel-native ingestion, attributes map directly to` attributes.*` fields in ES|QL:


**OTel attribute**


**ES|QL field**


` search.action`


` attributes.search.action`


` search.result_click_id`


` attributes.search.result_click_id`


` search.result_click_position`


` attributes.search.result_click_position`


` search.query_id`


` attributes.search.query_id`


` search.first_click`


` attributes.search.first_click`


With OTel-native ingestion,` search.first_click` is stored as a native Boolean; you query it with` == true` , not` == "true"` , and no string coercion is needed.


### Verify that clicks are arriving


Before calculating metrics, confirm that click spans are flowing to APM:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "click"
| KEEP attributes.search.result_click_id, attributes.search.result_click_position,
attributes.search.query_id, attributes.search.query
| LIMIT 5
```


If this returns rows, you're ready for analytics. If not, check the same things as we looked at in Blog 2: OTLP endpoint, auth token, and span export.


**Note:** Results in this post are illustrative, generated by running` python generate_traffic.py --blog 3 --sessions 50` on the[reference project](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) . Running Blog 3 traffic adds click events *and* additional search sessions on top of the 62 from Blog 2, so cumulative search counts will exceed 62. Your exact numbers will vary based on session count and the random nature of the traffic simulator. The metric calculations and ES|QL patterns are what to focus on.


## CTR


**CTR** is the primary signal for search relevance, answering the question that Blog 2 couldn't: *Are users engaging with the results?*


**CTR = searches with at least one click / total searches * 100**


CTR is a binary per-search metric: Either a search got clicked or it didn't. The maximum is 100%.


### Overall search CTR with ES|QL


```text
FROM traces-generic.otel-default
| WHERE (name == "search" AND attributes.search.query IS NOT NULL)
OR attributes.search.first_click == true
| STATS
searches = COUNT(CASE(name == "search" AND attributes.search.query IS NOT NULL, 1)),
clicked = COUNT(CASE(attributes.search.first_click == true, 1))
| EVAL ctr_pct = ROUND(100.0 * clicked / searches, 1)
```


**Result:** 41 clicked searches out of 146 total. **CTR: 28.1%**


This is a single query that pulls both search spans and first-click spans from` traces-generic.otel-default` . The` OR` in the` WHERE` clause brings both into one result set.` COUNT(CASE(...))` counts each type separately, and` EVAL` does the division.


This works because of` search.first_click` . Without it, you'd be counting raw clicks (a user who clicks three results on one search would inflate the count). The deduplication happened at instrumentation time; the query stays simple.


### CTR by search query: Finding your worst relevance failures


The overall number is useful for dashboards. The per-query breakdown is where you find problems.


```text
FROM traces-generic.otel-default
| WHERE ((name == "search" AND attributes.search.query IS NOT NULL)
OR attributes.search.first_click == true)
AND attributes.search.query IS NOT NULL
| STATS
clicked = COUNT(CASE(attributes.search.first_click == true, 1))
BY attributes.search.query
| EVAL ctr_pct = ROUND(100.0 * clicked / searches, 1)
| SORT searches DESC
| LIMIT 20
```


This shows CTR broken down by query text.


### What CTR tells you (and what it doesn't)


-


**High searches + zero clicks:** These are the worst relevance failures. Fix these first.


-


**High searches + low CTR:** Results appear, but they aren't compelling. Check ranking.


-


**Low CTR + high zero-results rate:** This is a double problem; either no results or bad results.


-


**CTR trend over time:** This measures the impact of relevance changes.


CTR doesn't measure satisfaction. A user who clicks position 1, bounces back, and then clicks position 3 still counts as one clicked search. For a fuller picture, you need to know *where* they're clicking. That's what MRR measures.


### CTR vs. clicks per search


**CTR** (what we just calculated) is capped at 100%. This is the industry-standard definition.


**Clicks per search** is total clicks divided by total searches. It can exceed 1.0. For example, a search where the user clicks three results scores 3.0. It measures engagement depth, which is useful but different. If you need it, count all click spans (not just` first_click` ) divided by search spans.


## MRR


**MRR** tells you *where users* click. It measures how far down the results list users go before finding something worth clicking.


**MRR = average of (1 / click_position) across all clicks**


The reciprocal rank transforms click positions into a 0–to–1 scale, where higher is better:


**Click position**


**Reciprocal rank**


1


1.000


2


0.500


3


0.333


5


0.200


10


0.100


### Overall search MRR with ES|QL


MRR can be calculated two ways, depending on what you want to measure:


-


**All-click MRR:** Averages the reciprocal rank of every click and reflects overall click quality, including repeated interactions.


-


**First-click MRR:** Averages only the first click per search (using` search.first_click == true` ). It’s more comparable to traditional information retrieval (IR) evaluation and aligns with how you computed CTR.


For consistency with CTR and alignment with judgment-list workflows in Blog 5, we prefer first-click MRR:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "click"
AND attributes.search.first_click == true
| EVAL reciprocal_rank = 1.0 / attributes.search.result_click_position
| STATS mrr = ROUND(AVG(reciprocal_rank), 3)
```


**Result:** MRR = **0.495**


This is decent but shows room for improvement. An MRR of 0.495 means the average first click lands around position 2. It isn’t a crisis, but there are queries where ranking can be improved.


### MRR by search query: Finding your worst-ranked results


Like CTR, the per-query breakdown is where the actionable data lives. To surface the worst-ranked queries first, sort ascending.


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "click"
AND attributes.search.first_click == true
AND attributes.search.query IS NOT NULL
| EVAL reciprocal_rank = 1.0 / attributes.search.result_click_position
| STATS
mrr = ROUND(AVG(reciprocal_rank), 3),
clicks = COUNT(*)
BY attributes.search.query
| SORT mrr ASC
| LIMIT 20
```


This reveals which queries have the worst ranking. A query with multiple clicks and low MRR means that the ranking is consistently poor for that search; that is, users find results, but they have to dig for them.


### What is a good MRR score for search?


-


**MRR > 0.8:** This ranking is solid; users usually click position 1–2.


-


**MRR 0.5–0.8:** This is decent, but there’s room for improvement.


-


**MRR < 0.5:** This is a ranking problem, and users are scrolling past top results.


-


**MRR drop after a change:** This is ranking regression that should be investigated immediately.


-


**Low MRR + high CTR:** Users are finding things, but they have to work for it.


That last pattern is particularly interesting. High CTR with low MRR means your results are relevant (users are clicking), but your ranking isn't surfacing the best results first. It's an optimization opportunity, not a crisis.


### MRR limitations: Position bias and multi-click sessions


MRR is heavily influenced by the gap between position 1 and position 2 (1.0 versus 0.5). Positions 5 and beyond barely move the average. This means that MRR is most sensitive to whether your top result is good, which is often exactly what you want to optimize.


MRR also only measures clicks, not satisfaction. With *all-click MRR* , a user who clicks position 1, bounces, and then clicks position 3 contributes two data points, but only the second was useful. The *first-click MRR* queries above avoid this by counting only the first click per search via` search.first_click == true` .


## Click position distribution


Click position distribution shows you the full picture of where in the results users are engaging.


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "click"
| STATS click_count = COUNT(*) BY attributes.search.result_click_position
| SORT attributes.search.result_click_position ASC
```


**Results:**


**Position**


**Clicks**


1


21


2


10


3


7


4


4


5


3


****


This is a reasonable distribution: 21 of 45 clicks (47%) land on position 1, with a tapering tail. If you paste this query into Discover's ES|QL editor, Kibana auto-generates a bar chart that makes the shape immediately visible.


### How to read click position distribution for search relevance


-


**Sharp dropoff after position 1:** The ranking is effective, and the top result is usually right.


-


**Flat across positions 1–5:** The ranking isn't differentiating well, and all positions are equally likely to be clicked.


-


**Spike at position 3+ for specific queries:** Those queries have ranking problems.


-


**No clicks beyond position 5:** Users don't scroll far. Top 5 ranking matters most.


### Click position distribution by search query


To see the shape for specific queries:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "click"
AND attributes.search.query IS NOT NULL
| STATS click_count = COUNT(*)
BY attributes.search.query, attributes.search.result_click_position
| SORT attributes.search.query, attributes.search.result_click_position
```


A query where all clicks land on position 1 has perfect ranking. A query where clicks spread across positions 1–5 needs relevance tuning.


### Position bias and click models


One caveat: Click position distribution is influenced by *position bias* ; that is, users see position 1 first, so it gets clicked more regardless of relevance. A click at position 1 isn't necessarily more relevant, just more visible.


This is a well-studied problem in information retrieval. *Click models* are statistical models that attempt to separate genuine relevance from position bias in click data. The foundational work by[Joachims et al. (2005)](https://www.cs.cornell.edu/people/tj/publications/joachims_etal_05a.pdf) showed that users are significantly biased toward higher-ranked results, and, in proposed methods like skip-above analysis (if a user clicks position 3 but skips positions 1 and 2), those skipped results are likely less relevant for that query.


For the metrics in this post, you don't need to implement a full click model. The key insight is practical: Compare distributions *between queries* rather than treating absolute position counts as ground truth. If query A has 80% of clicks at position 1 and query B has clicks spread across positions 1–5, query B's ranking is worse, even accounting for position bias. Later in the series, when we look at building judgment lists for LTR, position bias correction becomes more important, and the click data you're collecting here is exactly what those models need as input.


## CTR, MRR, and click distribution: Reading search quality metrics together


These three metrics offer three different angles on search result quality:


**Metric**


**What it measures**


**Our value**


**Interpretation**


**CTR**


Do users click at all?


28.1%


Moderate: Roughly a third of searches get engagement, but there’s room to improve.


**MRR**


Where do they click?


0.495


Decent: The average click is around position 2, but ranking can be improved.


**Distribution**


What's the shape?


47% at position 1


Reasonable drop-off: The top result wins most but isn’t dominant.


Together, they tell a coherent story. For our demo data, search is performing adequately: Users are engaging with results and can find what they need, but the ranking has room to improve. The CTR of 28% and MRR of 0.495 are realistic starting points for a new search implementation without tuning.


Where they're most valuable is in combination at the query level. The queries to fix first are those with **high volume + low CTR + low MRR** ; that is, lots of users are searching, few are clicking, and those who do click are scrolling deep. That's where relevance investment has the highest return.


### Using click data for LTR and relevance tuning


These metrics don't just tell you how search is performing; they're the foundation for making it better. The click data you're now collecting feeds directly into relevance improvement workflows:


-


**Judgment lists for LTR:** Click positions and frequencies become graded relevance labels for training machine learning (ML) ranking models. A document clicked at position 1 across many queries is a strong positive signal.


-


**Relevance tuning tools:** Per-query CTR and MRR tell you exactly which queries to focus on in tools like[Relevance Studio](https://elastic.github.io/relevance-studio/#/) or the[Rank Eval API](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-rank-eval.html) , which scores your ranking against expected results.


-


**Query rules and boosting:** Zero-CTR queries with results are candidates for pinning, boosting, or synonym rules.


We'll cover these applications in detail in Blog 5. For now, the important thing is that the instrumentation you've built here is doing double duty: It measures search quality *and* provides the training data to improve it.


## Next in the series: Conversion tracking from search to purchase


We can now measure whether users find results (Blog 2) and whether they engage with them (this post). But a click isn't a conversion. A user who clicks a product and then abandons the page didn't get what they needed.


In the next post, we add *conversion tracking* , the third instrumentation point that closes the loop from search to purchase. It’s the same pattern: Add` search.*` attributes to add-to-cart and checkout spans, query with ES|QL, and answer the question your product manager actually cares about: *Which searches drive revenue?*


## Get started


-


[Reference project:](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) Working code for the entire blog series (clone, configure, and run).


-


[Elastic Distribution of OpenTelemetry Python:](https://github.com/elastic/elastic-otel-python) EDOT Python.


-


[OpenTelemetry with Elastic:](https://www.elastic.co/docs/solutions/observability/apm/opentelemetry) How to send OTel data to Elastic APM.


-


[ES|QL documentation:](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html) Query language reference.


-


[UBI standard:](https://www.ubisearch.dev/) Reference schema for search event structure.


*This is the third post in a*[series on search analytics with OpenTelemetry and Elastic](https://www.elastic.co/search-labs/blog/series/search-analytics-opentelemetry) *. Next up: From clicks to conversions: Conversion tracking, funnel analysis, and revenue attribution.*

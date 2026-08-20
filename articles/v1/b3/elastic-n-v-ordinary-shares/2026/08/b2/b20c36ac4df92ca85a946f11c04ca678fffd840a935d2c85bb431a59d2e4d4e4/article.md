---
schema_version: "1.0.0"
document_id: "b20c36ac4df92ca85a946f11c04ca678fffd840a935d2c85bb431a59d2e4d4e4"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/search-conversion-tracking-opentelemetry"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T02:39:15.888121+00:00"
fetched_at: "2026-08-07T02:39:17.068337+00:00"
content_hash: "sha256:0e53ddbb7523825bbd02eed26acbb6bb19870483f07e7c03097a60523a132c4e"
---

# From search to checkout in 20 lines of code: building a 4-stage conversion funnel with OpenTelemetry

Your product manager wants to know which searches drive the most revenue. You can tell them what users search for ([check out our second blog](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry-esql) ) and what they click ([in our third blog](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ), but not what they buy. Two new span types, add-to-cart and purchase, complete a four-stage funnel from search query to checkout, built on the same search.* attributes and ES|QL queries you've been using since Blog 2. About 20 lines of code, and every relevance decision you make gets a revenue number attached to it.


## What you'll discover


In this post, you'll learn how to:


-


Add conversion tracking (add-to-cart and purchase spans) with` search.*` attributes that tie back to the originating search.


-


Build a full search-to-revenue funnel: search → click → add-to-cart → purchase.


-


Write Elasticsearch Query Language (ES|QL) queries to calculate conversion rates, revenue per query, and average order value.


-


Identify where users drop off and which team should own each drop-off point.


-


Attribute revenue to specific search queries for prioritizing relevance work.


## What you'll need


-


Click tracking from[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) (search + click spans flowing to Elastic via OTel-native ingestion).


-


Backend endpoints for add-to-cart and checkout events (example code provided).


-


Basic understanding of ecommerce conversion funnels.


## Why search revenue attribution matters


Your product manager walks into a meeting and asks, "Which searches are driving the most revenue?"


You can tell them what users search for ([Blog 2](https://www.elastic.co/search-labs/blog/series/search-analytics-opentelemetry) ) and what they click ([Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ). But you can't tell them what they *buy* . The gap between "clicked a result" and "purchased a product" is where the business case for search investment lives, and right now it's invisible.


This post closes the loop. By adding two more span types (add-to-cart and purchase), you get a full funnel from search to revenue, built on the same` search.*` attributes and ES|QL queries you've been using since[Blog 2](https://www.elastic.co/search-labs/blog/series/search-analytics-opentelemetry) .


### What search conversion tracking answers for your team


Here's what conversion tracking lets you answer and why each question matters to different people on your team.


-


**Which searches drive revenue?** This is the product manager's question. When you can attribute dollars to specific queries, you can prioritize relevance work by business impact. A query with mediocre click-through rate (CTR) but high conversion value is more important than a high-CTR query that never leads to a purchase.


-


**Where do users drop off?** The funnel from search to purchase has four stages: search, click, add-to-cart, and purchase. Each drop-off points to a different problem. High click-to-cart drop-off suggests that product pages aren't convincing. High cart-to-purchase drop-off is checkout friction rather than a search problem. Knowing *where* users abandon tells you *which team* should fix it.


-


**Which queries to protect?** Once you know that "laptop bag" generates $12,000/month in attributed revenue, you treat it differently. Any relevance change that touches high-revenue queries gets extra scrutiny. You can set up monitoring (Blog 6, coming soon) to alert when conversion rates drop for your top-earning searches.


Two new instrumentation points total about 20 lines of code, and they follow the same pattern as you’ve used before. You add attributes to spans, and query them with ES|QL.


**Following along with code?** The[reference project](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) has conversion tracking ready to enable. Uncomment the Blog 4 sections in` app.py` and` app.js` , restart, and then generate traffic with` python generate_traffic.py --blog 4` .


## The four-stage search conversion funnel


Before we write any code, here's the shape of what we're building. Each stage is an instrumentation point, and each creates spans in` traces-generic.otel-default` :


```text
search          →  click              →  cart.add         →  checkout.complete
(Blog 2)           (Blog 3)              (this post)         (this post)
query_id=abc       query_id=abc          query_id=abc        query_id=abc
user_query=...     click_position=1      product_id=...      order_total=$149
result_count=15    product_id=...        quantity=1           item_count=2
```


The thread running through the entire chain is` search.query_id`` .` The same identifier you derived from the trace ID in[Blog 2](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry-esql) and used to link clicks to searches in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) now carries through to add-to-cart and purchase events. This is what makes revenue attribution possible: You can trace a purchase back to the search that started the journey.


## Add conversion tracking spans with OpenTelemetry


### Add-to-cart span instrumentation


When a user adds a product to their cart from a search results page (or from a product detail page they reached via search), you create a` cart.add` span. This captures the moment that intent turns into action.


```text
@app.post("/api/cart/add")
async def add_to_cart(event: AddToCartRequest):  # reference project uses CartEvent
with tracer.start_as_current_span("cart.add") as span:
span.set_attribute("search.action", "add_to_cart")
span.set_attribute("search.result_click_id", event.object_id)
span.set_attribute("search.result_click_position", event.position)
span.set_attribute("search.query_id", event.query_id)
span.set_attribute("enduser.pseudo.id", event.client_id)
span.set_attribute("cart.quantity", event.quantity)
if event.price is not None:
span.set_attribute("cart.price", event.price)
if event.user_query:
span.set_attribute("search.query", event.user_query)
```


This follows the same pattern as click tracking in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ; that is, an independent span linked to the originating search via` query_id` . The new attributes are` cart.quantity` and` cart.price` which let you aggregate revenue at query time.


### Purchase span instrumentation


When the user completes checkout, you create a` checkout.complete` span. This is the revenue event, the one that answers the product manager's question.


```text
@app.post("/api/checkout")
async def checkout(event: CheckoutRequest):  # reference project uses CheckoutEvent
with tracer.start_as_current_span("checkout.complete") as span:
span.set_attribute("search.action", "purchase")
span.set_attribute("checkout.order_id", event.order_id)
span.set_attribute("checkout.total_amount", event.total_amount)
span.set_attribute("checkout.item_count", len(event.items))
span.set_attribute("enduser.pseudo.id", event.client_id)
if event.query_id:  # last search in journey
span.set_attribute("search.query_id", event.query_id)
span.set_attribute("search.query", event.user_query)
```


-


**Spans capture errors automatically.** This is a side benefit of using OTel spans for conversion events. If an add-to-cart or checkout call throws an unhandled exception, the span's status is automatically set to` ERROR` and the exception details are recorded. These errors are business-critical (a broken checkout flow means lost revenue), and they show up immediately in Elastic APM's error tracking, service maps, and alerting. You get conversion analytics *and* operational monitoring from the same instrumentation, with no extra code.


-


` **checkout.total_amount**` **is the revenue number.** This is what you'll aggregate in ES|QL to get revenue-by-query. It represents the order total, not the price of a single item.


-


` **search.query_id**` **is conditional.** Not every purchase originates from search. Users browse categories and follow promotional links. Then they return to their cart days later. The` if event.query_id:` guard ensures that you only attribute purchases to search when there's a genuine connection. Purchases without a` query_id` still get recorded; they just don't appear in search attribution queries.


-


` **search.query**` **is set on both cart and purchase spans.** This is a deliberate denormalization. You could join back to searches via` query_id` to get the query text, but while ES|QL does support` LOOKUP JOIN` , it’s likely not the right choice here due to the need to optimize the lookup index. By putting the query text directly on conversion spans, your revenue-by-query and cart-by-query queries are single, straightforward aggregations.


### Span attributes for cart and purchase events


**Add-to-cart attributes:**


**Attribute**


**Type**


**Required**


**Purpose**


` search.action`


string


yes


` "add_to_cart"`


` search.result_click_id`


string


yes


Product document ID


` search.result_click_position`


int


yes


Position in results when added


` search.query_id`


string


yes


Links to originating search


` enduser.pseudo.id`


string


yes


Client/device identifier (OTel Semantic Conventions \[SemConv\])


` cart.quantity`


int


yes


Quantity added


` cart.price`


float


optional


Product price


` search.query`


string


recommended


The search query text (for cart-by-query analysis)


**Purchase attributes:**


**Attribute**


**Type**


**Required**


**Purpose**


` search.action`


string


yes


` "purchase"`


` checkout.order_id`


string


yes


Unique order identifier


` checkout.total_amount`


float


yes


Order total


` checkout.item_count`


int


yes


Number of items purchased


` enduser.pseudo.id`


string


yes


Client/device identifier (OTel SemConv)


` search.query_id`


string


recommended


Links to originating search (last search in journey)


` search.query`


string


recommended


The search query text (for revenue-by-query analysis)


These follow the same` search.*` namespace from Blogs 2 and 3, with new` cart.*` and` checkout.*` prefixes for conversion-specific data. With OTel-native ingestion, all attributes are stored under` attributes.*` with dot notation preserved. That means no more mapping strings to` labels.*` and numbers to` numeric_labels.*` . A` search.query` attribute is queryable as` attributes.search.query` . Likewise,` checkout.total_amount` is queryable as` attributes.checkout.total_amount` .


### User identity: connecting search to purchase across sessions


You'll notice that` enduser.pseudo.id` appears on every span type in this series. It's the *minimum identity level* ; that is, a persistent identifier stored in the browser's localStorage that ties events to a device across sessions.


For conversion tracking, identity becomes more important. You need to connect a search on Monday to a purchase on Tuesday, or correlate cart additions across tabs. Our schema supports three identity levels, aligned with OTel semantic conventions:


**Attribute**


**Persistence**


**Purpose**


` enduser.pseudo.id`


Permanent (localStorage)


Device/browser identifier (OTel SemConv)


` session.id`


Per-visit (sessionStorage)


Groups events within a single visit (OTel SemConv)


` user.id`


Account (auth system)


Authenticated user (OTel SemConv)


For the funnel queries in this post,` enduser.pseudo.id` is sufficient. It links the journey from search to purchase within a browser. If your users authenticate, adding` user.id` enables cross-device attribution (searched on mobile, purchased on desktop) and richer personalization.` session.id` helps disambiguate when the same client has multiple active sessions.


All three are optional on interaction spans. Start with` enduser.pseudo.id` , and add the others when your use case requires them. The important thing is consistency: Use the same identifiers across search, click, and conversion spans so the joins work.


### Frontend integration


The front end needs to propagate` query_id` through the user journey. When the user clicks a search result, you already have` query_id` from the search response ([Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ). The key is carrying it forward.


```text
// CLIENT_ID: persistent browser identifier from localStorage (set up in Blog 3)
// const CLIENT_ID = localStorage.getItem("search_client_id") || ...


// On add-to-cart from a search result page
fetch('/api/cart/add', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({
object_id: product.id,
position: product.resultPosition,  // from search results
query_id: product.queryId,         // from search response
client_id: CLIENT_ID,              // persistent browser identifier → enduser.pseudo.id
quantity: 1,
price: product.price,
})
});


// On checkout completion
fetch('/api/checkout', {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({
order_id: order.id,
total_amount: order.total,
items: order.items,
client_id: CLIENT_ID,              // same identifier as search and click spans
query_id: lastSearchQueryId,       // last search in session
user_query: lastSearchQuery,
})
});
```


The front end sends` client_id` as an HTTP field name; and the back end maps it to the OTel semantic convention` enduser.pseudo.id` when setting span attributes. The` query_id` propagation is the critical piece. Store it alongside the product in the cart data structure so it survives navigation between pages. We'll discuss the design challenges of this in the attribution section below.


**Using the reference project?** The reference app's` frontend/app.js` wires up add-to-cart buttons, but the checkout flow isn’t implemented in the browser UI. It goes through the traffic generator. To simulate conversion events, run:` python generate_traffic.py --blog 4 --sessions 100` . This sends a realistic mix of searches, clicks, cart additions, and purchases to all three backend endpoints.


### Verify that conversion events are arriving


Before building funnel queries, confirm that both span types are flowing to Elastic:


Add-to-cart events:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "add_to_cart"
| KEEP attributes.search.result_click_id, attributes.search.query_id,
attributes.cart.quantity
| LIMIT 5
```


Purchase events:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "purchase"
| KEEP attributes.checkout.order_id, attributes.checkout.total_amount,
attributes.checkout.item_count, attributes.search.query_id,
attributes.search.query
| LIMIT 5
```


If these return rows, you have the full funnel instrumented. If not, check the same things as always: OpenTelemetry Protocol (OTLP) endpoint, auth token, and span export.


## Funnel analysis with ES|QL


### Count search, click, cart and purchase events in a single query


You can count all four funnel stages in a single ES|QL query using the same` COUNT(CASE(...))` pattern from the CTR query in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) :


```text
FROM traces-generic.otel-default
| WHERE (name == "search" AND attributes.search.query IS NOT NULL)
OR attributes.search.first_click == true
OR attributes.search.action IN ("add_to_cart", "purchase")
| STATS
searches  = COUNT(CASE(name == "search" AND attributes.search.query IS NOT NULL, 1)),
clicked   = COUNT(CASE(attributes.search.first_click == true, 1)),
carts     = COUNT(CASE(attributes.search.action == "add_to_cart", 1)),
purchases = COUNT(CASE(attributes.search.action == "purchase", 1))
| EVAL
click_rate    = ROUND(100.0 * clicked   / searches, 1),
cart_rate     = ROUND(100.0 * carts     / searches, 1),
purchase_rate = ROUND(100.0 * purchases / searches, 1)
```


**Example output:**


**searches**


**clicked**


**carts**


**purchases**


**click_rate**


**cart_rate**


**purchase_rate**


146


41


28


12


28.1%


19.2%


8.2%


You get all four counts and three conversion rates from a single query. The` WHERE` clause pulls all four span types into one result set, and` COUNT(CASE(...))` counts each type separately. This is the same technique that made the CTR query in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) so clean.


Notice that we use` search.first_click` for the click stage rather than counting all click events. This gives you the number of searches that received at least one click (the same definition used for CTR in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ). Without this, a search with three clicks would inflate the click count to three while only counting as one search, making the funnel numbers misleading. Each stage now represents a unique progression: how many searches happened, how many of those got clicked, how many led to a cart addition, and how many resulted in a purchase.


You can turn this into a funnel visualization using Kibana Lens. Run each stage count as a separate ES|QL query, save them as dashboard panels, and arrange them as a horizontal bar chart with the four stages on the y-axis and counts on the x-axis. The drop-off at each step becomes immediately visible.


The dashboard above (from[the first blog in the series](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry) ) shows this in practice: The **Conversion Funnel** panel in the lower left uses a horizontal bar chart to visualize the four stages, and the **Top Queries by Revenue** table alongside it shows revenue attribution. You can build these directly from the ES|QL queries in this post.


### What drop-off rates tell you and who owns each bottleneck


Each transition in the funnel tells you something specific:


-


**Search to click (CTR):** You measured this in[Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) . Low CTR means that results aren't compelling. This is a relevance problem.


-


**Click to cart:** The user engaged with a result but didn't add it to their cart. This could mean that the product page isn't persuasive or the price isn't competitive. It could also mean that the item was out of stock. It's often *not* a search problem. It’s possible that the search worked (the user clicked), but something downstream lost them.


-


**Cart to purchase:** The user committed to buying but didn't complete checkout. Complicated forms, unexpected shipping costs, and payment issues cause this *checkout friction* . This is almost never a search problem, but it's useful to know where the funnel leaks so you don't waste time optimizing relevance when checkout is the bottleneck.


The diagnostic pattern is straightforward:


**Drop-off point**


**Likely cause**


**Who owns it**


Search to click


Relevance / ranking


Search team


Click to cart


Product page / pricing / availability


Product / merchandising


Cart to purchase


Checkout UX / payment / shipping


Checkout / growth


This is one of the most valuable things that full-funnel data gives you: the ability to point at the right problem. When the VP asks, "Why aren't searches converting?", you can show whether the bottleneck is relevance, product pages, or checkout.


## Revenue attribution by search query


Here's the query your product manager actually wants:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "purchase"
AND attributes.search.query IS NOT NULL
| STATS
purchase_count = COUNT(*),
total_revenue = SUM(attributes.checkout.total_amount)
BY attributes.search.query
| SORT total_revenue DESC
```


This gives you a ranked list of queries by the revenue they generated. The` search.query` attribute on purchase spans (the denormalization we discussed earlier) makes this a single aggregation query, without joins or subqueries.


### How to act on search revenue data


-


**Protect high-revenue queries.** If "laptop bag" generates the most revenue, any relevance change that affects that query gets extra scrutiny. You might add it to a regression test suite or pin specific results with[query rules](https://www.elastic.co/docs/reference/elasticsearch/rest-apis/searching-with-query-rules) . You might even set up an alert when its conversion rate drops (Blog 6).


-


**Prioritize relevance investment.** The queries at the top of this list are where relevance improvements have the most business impact. A 10% CTR improvement on a query that generates $500/month in revenue is worth more than a 50% improvement on one that generates $20.


-


**Identify missed opportunities.** Cross-reference with the top-queries data from[Blog 2](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry-esql) . A query with high search volume but no purchase attribution is either a browsing query (informational intent) or a conversion gap worth investigating.


### Top revenue queries: where to focus relevance investment


To focus your relevance team's efforts, pull the top revenue-generating queries:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "purchase"
AND attributes.search.query IS NOT NULL
| STATS
purchases = COUNT(*),
revenue = SUM(attributes.checkout.total_amount)
BY attributes.search.query
| SORT revenue DESC
| LIMIT 10
```


Cross-reference this with the per-query CTR from Blog 3. A query with high revenue but low CTR is underperforming; even small relevance improvements have outsized business impact. A query with high CTR but no purchase attribution might be informational (such as users browsing but not buying). The queries at the top of *both* lists deserve the most attention from your relevance team.


### Average order value by search query


Average order value (AOV) tells you how much purchases are worth, in addition to which queries convert to those purchases. Queries with high AOV are your premium-intent searches; ranking improvements there have the biggest per-purchase impact:


```text
FROM traces-generic.otel-default
| WHERE attributes.search.action == "purchase"
AND attributes.search.query IS NOT NULL
| STATS
purchase_count = COUNT(*),
total_revenue = SUM(attributes.checkout.total_amount),
avg_order_value = ROUND(AVG(attributes.checkout.total_amount), 2)
BY attributes.search.query
| SORT avg_order_value DESC
| LIMIT 10
```


A query with high AOV but low volume is a different opportunity than high volume + low AOV. The first means premium intent from a small audience (consider featured results or dedicated landing pages); the second means broad reach with budget buyers (price sensitivity may be limiting conversion more than relevance).


## Search revenue attribution: limitations and workarounds


Revenue attribution from search is valuable, but it's imperfect. Understanding the limitations helps you set appropriate expectations and design around them.


### Last-touch attribution in multi-search journeys


Users rarely search once and buy. A typical journey might look like:


1.


Search "laptop bag": Browse results and click a few.


2.


Search "laptop bag leather": Refine the search.


3.


Search "laptop sleeve 15 inch": Try a different angle.


4.


Add to cart from the third search's results.


5.


Purchase.


With the instrumentation above, this purchase attributes to the third search, the one whose` query_id` was on the cart item. The first two searches contributed to the journey but get no credit.


This is *last-touch attribution* , and it's the simplest model that works within a single` query_id` linkage. It's not perfect, but it's concrete and unambiguous. The alternative, that is,tracking every` query_id` in a user's session and distributing credit, adds significant complexity to both instrumentation and analysis.


For most teams, last-touch is a good starting point. If you need multi-touch attribution later, the raw data is there. You can query all searches and clicks for a given` client_id` within a time window and reconstruct the full journey:


```text
FROM traces-generic.otel-default
| WHERE attributes.enduser.pseudo.id == "client-abc-123"
AND (name == "search"
OR attributes.search.action == "click"
OR attributes.search.action == "add_to_cart"
OR attributes.search.action == "purchase")
| KEEP @timestamp, name, attributes.search.action,
attributes.search.query, attributes.search.query_id,
attributes.search.result_click_id
| SORT @timestamp ASC
```


This reconstructs a user's full search journey in chronological order. It's useful for debugging individual sessions, even if you don't build automated multi-touch attribution.


### Cross-session attribution limits with query_id


A user searches for "wireless headphones" on Monday, clicks a few results, leaves, and comes back on Wednesday to buy. The` query_id` from Monday's search is long gone, since it was a property of that specific search request.


This is a fundamental limitation of` query_id` -based attribution. It works within a session (or more precisely, within the scope where the front end retains the` query_id` ). It doesn't work across sessions.


For cross-session attribution, you'd need a different approach, typically a user-level event store where you associate product views, cart additions, and purchases with a persistent user ID and then look back in time to find the originating search. That's a more complex analytics pipeline and is outside the scope of what we're building here.


The practical impact is that your search-attributed revenue will be an *undercount* . Some purchases that were genuinely influenced by search won't carry a` query_id` . This is fine for relative comparisons (such as, *Which queries generate* more *revenue than others?* ), even if the absolute numbers are conservative.


### How to persist query_id from search to checkout


A few practical decisions affect how far your` query_id` propagation reaches:


-


**Store**` **query_id**` **in the cart.** When a user adds a product to their cart, persist the` query_id` alongside the item. This way, even if the user navigates away and comes back to checkout later (within the same session), the attribution survives.


-


**Don't overwrite**` **query_id**` **on re-search.** If a user adds a product from search A and then searches again and adds another product from search B, each cart item should keep its own` query_id` . The purchase event carries the last search's` query_id` as a summary, but per-item attribution gives you richer data.


-


**Accept the limitations.** Not every purchase will have search attribution. Direct navigation, category browsing, promotional links, and returning customers who go straight to their cart will all produce purchases without a` query_id` . That's correct behavior, not missing data.


### Spans vs. log events for conversion tracking


Conversion events can emit both an OTel span and a UBI-compatible log event, the same dual-signal pattern used for click tracking in Blog 3. The log event is actually richer for conversions. It can contain the full items list with per-item` query_id` attribution, which doesn't map cleanly to flat span attributes.


The span gives you the simple, aggregatable view (total revenue by query), and the log gives you the detailed, per-item view (which specific products from which specific searches). For the funnel queries in this post, spans are sufficient. If you need item-level attribution analysis, the log events in` logs-generic.otel-default` have the detail you need, and ES|QL queries them the same way, just against a different index pattern.


## What's next: turning conversion data into relevance improvements


We now have the complete instrumentation picture, with four span types:` search` ([Blog 2](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry-esql) ),` search.result.click` ([Blog 3](https://www.elastic.co/search-labs/blog/search-click-tracking-opentelemetry-esql) ),` cart.add` , and` checkout.complete` (this post). These capture the full user journey from query to purchase. Every span lives in` traces-generic.otel-default` , and every metric is queryable with ES|QL. The` search.query_id` thread ties the entire funnel together.


But measuring the funnel is only half the story. The real payoff is using this data to make search better.


In a later blog in this series, we take everything we've built and turn it into relevance improvements. Click positions and conversion data become judgment lists for[Learning To Rank](https://www.elastic.co/guide/en/elasticsearch/reference/current/learning-to-rank.html) , and per-query CTR and revenue become rank features for boosting. Plus, high-revenue queries get protective monitoring. And tools like[Elasticsearch Relevance Studio](https://elastic.github.io/relevance-studio/#/) give you a visual interface for tuning the searches that matter most, using exactly the data you're now collecting.


The instrumentation you've built in Blogs 2–4 is a feedback loop, beyond analytics: Measure, improve, and measure again.


## Get started with search conversion tracking


-


[Reference project:](https://github.com/elastic/elasticsearch-labs/tree/main/supporting-blog-content/search-analytics-otel) Working code for the entire blog series; clone, configure, and run.


-


[Elastic Distribution of OpenTelemetry for Python:](https://github.com/elastic/elastic-otel-python) EDOT for Python.


-


[OpenTelemetry with Elastic:](https://www.elastic.co/docs/solutions/observability/apm/opentelemetry) How to send OTel data to Elastic APM.


-


[ES|QL documentation:](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html) Query language reference.


-


[UBI Standard:](https://www.ubisearch.dev/) Reference schema for search event structure.


-


[Query rules:](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-rules.html) Pin, boost, or exclude results for specific queries.

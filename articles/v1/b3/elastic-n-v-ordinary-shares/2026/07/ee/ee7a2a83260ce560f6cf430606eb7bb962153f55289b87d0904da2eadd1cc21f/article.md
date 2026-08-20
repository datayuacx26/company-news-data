---
schema_version: "1.0.0"
document_id: "ee7a2a83260ce560f6cf430606eb7bb962153f55289b87d0904da2eadd1cc21f"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/ai-shopping-agents-context-engineering"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-21T17:41:41.822757+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:74bb6069cd08f217961e692168fbb0c3b74faa42fc751f1858dcf1f4055ca3a1"
---

# AI shopping agents: Why context comes before the query

Agent Builder is available now GA. Get started with an[Elastic Cloud Trial](https://cloud.elastic.co/registration?utm_source=agentic-ai-category&utm_medium=search-labs&utm_campaign=agent-builder) , and check out the documentation for Agent Builder[here](https://www.elastic.co/docs/solutions/search/elastic-agent-builder?utm_source=agentic-ai-category&utm_medium=search-labs&utm_campaign=agent-builder) .


The race is on for retailers to match the evolving expectations of their customers to provide significantly richer online shopping experiences. Customers want to go beyond searching for products; they want interactive, personalized, and proactive guidance powered by AI. The challenge is that, although large language models ([LLMs](https://www.elastic.co/what-is/large-language-models) ) can be extremely powerful, how do you construct a system that acts like a knowledgeable employee of your store in a fast, accurate, and cost-effective way? We’ll discuss the challenges of building these AI shopping assistants as well as emerging[context engineering](https://www.elastic.co/search-labs/blog/context-engineering-overview) approaches to optimize how they work.


AI shopping agents fail not because the model is wrong but because the agent arrives at every conversation not knowing your catalog,vocabulary , or business rules. It has to discover all of this context through tool calls, and that discovery is the cost. Precomputing a structured context layer from signals you already hold (vocabulary, policies, user profiles, session behavior) cuts the exploratory work the agent does before it can answer and makes its behavior governed and predictable.


In the analogousdocument -retrieval case, precomputing context reduced input tokens by up to 75% on a controlledbenchmark ; we expect comparable savings in[ecommerce](https://www.elastic.co/enterprise-search/ecommerce) because the exploration pattern is the same, though the exact figure will vary by catalog andquery mix. The signals are richer in ecommerce than in almost any other domain, and most retailers already have them. The question is whether or not these signals are assembled in a form that the agent can use before it starts reasoning. Elastic’s broad mix of search capabilities, semantic, hybrid, keyword, filtering, and aggregations make it a compelling choice for not only building your core retrieval tools but also as the vital context engine.


## Why AI shopping agents fail in production


Retailers investing in AI shopping assistants are discovering an uncomfortable gap between what the demos promise and what the first few months in production deliver.


The assistant takes four seconds to respond. It confidently recommends a product in a size range that doesn't exist for that item. It tells a returning customer about a coat style they bought 18 months ago and returned. It filters by a category name that doesn't match the internal taxonomy and returns zero results. The customer gives up and abandons the chat altogether.


These aren't model failures. The frontier models powering these agents are capable of extraordinary reasoning when they have the right information in front of them. The problem is that the agent arrives at the conversation knowing nothing about the retailer's catalog, customer, or business rules that govern what should and shouldn't be recommended. It has to learn all of this through the conversation itself, making exploratory tool calls to discover what departments exist, what filter values are valid, and what the brand's policies are on certain product types. Every one of those discovery calls costslatency and tokens before the agent has said a single useful thing to the customer.


Latency matters in ecommerce in a way it doesn't in many other contexts. Shoppers expect response times measured in seconds, not the minutes that enterprise knowledge-base agents routinely take. It’s well established that slower responses reduce engagement and conversion in online retail. An AI shopping assistant that thinks visibly for five seconds before answering a question about gift ideas isn't a feature; it's friction.


The fix isn't a faster model or a bigger context window. **The agent's latency and cost problem is a context problem.** This can be solved by carefully computing context before the agent call, not during it.


### What an AI shopping agent knows before it searches


Imagine you’re the agent. You have a search tool connected to the catalog, and this query arrives:


` "an outfit for an autumn wedding"`


What do you actually do with that?


Start with what you don't know. An outfit for a man or a woman? Is "autumn" a color, a season, a style of fabric, or just when the wedding happens? And who is this shopper, someone who has bought from you for years, or a stranger? Do they buy expensive designer brands or do they always hunt out a bargain on sale? You have none of these answers. So you do what an agent does when it's working blind: You ask loads of follow-up questions, guess, or fire off a series of exploratory searches to find out what departments and filter values even exist, watching the seconds tick by before you've offered the customer anything at all.


Hold onto that feeling of working blind. The rest of this article is about what changes when the agent is handed the answers first.


## Why ecommerce is different from general RAG


Most of the published work on reducing agent costs focuses on document retrieval: question-answering over corpora of articles, reports, or knowledge-base entries. A recent experiment from the Elastic team ([Cutting agent costs with pre-computed context](https://www.elastic.co/search-labs/blog/pre-computed-context-llm-agent-costs) ) demonstrated that pre-extracting structured facts from documents before the agent call reduced inputtoken consumption by up to 75% and improved answer accuracy from 60% to 92% on a hard factual benchmark. That improvement came in stages, with the largest jump driven by feeding the agent's own wrong answers back into the extraction step rather than by precomputing context alone, which is a distinction we'll return to when we discuss governance.


Ecommerce applies the same principle to a fundamentally different structure. A product catalog isn't a documentcorpus . It's a highly structured index of items with strict field semantics, a domain-specific vocabulary of brand names, color codes, and category hierarchies, and a layer of business rules that override pure relevance in specific situations.


The failure modes that result are distinct from document[retrieval augmented generation](https://www.elastic.co/search-labs/blog/retrieval-augmented-generation-rag) (RAG) failures:


- **Vocabulary mismatch:** A customer asks for a "navy jumper." The agent constructs a filter against a field where the canonical value is NAVY and the category is stored as Knitwear & Jumpers. Without a vocabulary mapping, the agent either guesses colors and categories and gets it wrong, or makes multiple exploratory calls to discover what values exist before it can filter correctly.
- **Hallucinated filter values:** Without knowing which filter dimensions are valid for a given query, agents can construct queries against fields that don't exist or with values that return zero results. A filter like category: knitwear looks reasonable;` masterCategoryNames: "Knitwear & Jumpers"` is what the index actually contains. If the agent doesn’t know, it either hallucinates or has to do a separate tool call to find out, causing another LLM loop, which costs time and tokens.
- **Context-free personalization:** The same query from two different customers, one who typically shops in the premium range and dresses for formal occasions, and one who buys primarily casualwear under £40, should return different results. Without profile context, the agent treats every query identically, which is worse than a well-tuned keyword search because it creates the impression of a personal assistant while delivering generic answers.


## The context layer: What signals it needs


The reason ecommerce is particularly well suited to precomputed context is that retailers already hold an unusually rich set of signals. The challenge isn't data availability; it's assembly.


The signals fall into two groups. Two of them, the catalog vocabulary and the business policies, are the genuinely original work and the heart of this approach. The rest, live facet state, user profiles, and session history, are valuable but closer to table stakes, signals that most teams already understand how to fetch. Here's what most mid-to-large retailers have and what each signal prevents.


Signal What it prevents Effort to build


Catalog vocabulary Vocabulary mismatch and hallucinated filter values; the agent guessing at colors, categories, or brand names instead of resolving them to canonical field values One-time engineering effort (full-catalog aggregation); incremental to maintain as new categories and brands are added


Business policies Recommendations that ignore legal or trading requirements, for example, missing age verification on alcohol queries or routing that misses a gluten-free range Human-authored and governed, not automated; ongoing review as new policy types are added


Live facet state Recommending filters that return zero results or out-of-stock options for the current query Runs in parallel with vocabulary and policy lookups; leans on existing catalog and retrieval infrastructure


User profile Making a returning customer restate sizes, budget, or brand preferences they've already given Fastest signal to retrieve, a single document lookup by user ID


Session and purchase history Re-recommending an item the customer already dismissed or bought and returned Most aspirational layer; depends on customer relationship management (CRM) and analytics integration, best added once the agent is already live


The reason we say *semantic metadata* and not just *metadata* is that we’re trying to match the semantic (meaning) of the intent rather than the exact words. If a user is searching for “teal,” we should be able to understand that this is a color and which colors exist in our products that are semantically similar to teal, even if none of them are actually teal. So, if we search for “teal,” we might want to return:


` ProductColours = “aquamarine, turquoise”`


Hopefully, you can see how this semantic metadata is bridging the gap between user intent and the agent’s knowledge of the products.


### Catalog vocabulary: The foundation of context engineering


Catalog vocabulary is the layer that does the most work, and it's the one most worth getting right first.


A vocabulary index maps natural language to the exact field values and category paths used in the product index. It answers questions like: *What does "navy" map to?* *Which categories fall under "knitwear"?* *Is "Autograph" a brand or a range?* *What's the correct spelling of a competitor brand the agent might encounter in a query?*


What makes this more than a synonym list is how it's queried. The interesting thing a shopper says is rarely an exact field value. They say "something cozy for fall," not` colour: NAVY` and` masterCategoryNames: "Knitwear & Jumpers"` . So the vocabulary index needs to resolve fuzzy, natural language intent into precise, exact-match filters, and that requires both kinds of matching at once:[semantic search](https://www.elastic.co/search-labs/blog/introduction-to-vector-search) to understand that "cozy" leans toward knitwear and fleece, and exact keyword matching to pin the result to the canonical values the product index actually stores. A metadata index that supports both on the same documents is, in effect, a translation layer between how customers talk and how the catalog is structured.


This is also where the index earns the description "semantic metadata layer" rather than "lookup table." Each entry is a small natural language description of a facet value or schema concept, so the agent can match against meaning and then read back the exact filter to use. For a typical fashion retailer, this covers hundreds of color values, brand aliases, category synonyms, and size-range conventions. Building the semantic metadata layer from a full-catalog aggregation is a one-time engineering effort; maintaining it is incremental as new categories and brands are added. Without it, an agent encountering an unfamiliar term must either guess or make exploratory tool calls to discover what's there.


### Business policies in the context layer


The second original layer is policy. Some queries carry implicit business requirements that pure relevance cannot handle. A query for "wine gift for a friend" should trigger an age-verification reminder in markets where it's legally required. A query for "gluten-free food gift" should route away from general confectionery toward the specific gluten-free range. A query mentioning "wedding guest outfit" in spring should apply different weighting than the same query in November.


These are policies, and most retail search teams already write them. They just call them boost rules, merchandising overlays, or synonym configurations. The difference in an agentic context is that instead of being applied silently as query modifications, they're surfaced as readable hints the agent can use when deciding how to frame its answer and which products to surface. The agent doesn't have to infer your trading rules from the catalog; it's handed them.


The critical point: These policies encode business intent, not just relevance. A policy that routes alcohol queries through an age-appropriate flow isn't a retrieval optimization; it's a trading requirement. That's why this layer must be human-authored and governed, not generated automatically from traffic patterns, a point we return to in the governance section.


Together, vocabulary and policy are what make the agent behave like it understands your business rather than just your data. The remaining three signals sharpen the experience, but they're more familiar engineering.


### Facet, profile, and session signals in the context layer


- **Live facet state:** Before the agent recommends filters, it should know which filters are available and how many results each returns for this specific query. An agent that suggests "filter by size 8" without knowing that size 8 is out of stock for this query undermines the customer's trust immediately. A facet state query against the product index, run in parallel with the vocabulary and policy lookups, returns the counts, ranges, and available values specific to the current query.
- **User profile:** A persistent profile (sizes, color preferences, budget range, brand affinities) lets a returning customer skip restating what they've already told you. It's typically the fastest signal to retrieve, a single document lookup by user ID.
- **Session and purchase history:** Within a session, the agent should know what the customer has already seen, dismissed, or added to their basket, so it doesn't re-recommend a dismissed item or repeat itself. Longer-term purchase history extends this, and signals like return history are richer still, but using them well depends on data most retailers hold in systems that aren't yet wired into their search path. This is the most aspirational layer and the one best approached last, once the earlier layers are delivering value. There must also be balance when building the initial context not to overinflate the size, which will slow down the first reply and increase token costs. There is, therefore, a careful balance to strike between providing information like purchase history in the initial context or providing it as a tool for the agent to use during conversation, but users will expect that, if they’re logged in, the agent should know what they’ve purchased. The precise optimal context is likely to be specific to each implementation and customer experience and will require careful testing.


## How context engineering works before the LLM call


The pattern that makes this work is simple to describe and moderately involved to implement:


Without this context build, the agent makes the same discoveries on its own, but through LLM-driven tool calls that each cost a full[inference](https://www.elastic.co/docs/explore-analyze/elastic-inference) round trip. As a rough rule of thumb, an exploratory tool (for example, GetFilterValues) call tends to land somewhere in the region of 600ms to 1 second of latency in practice; an agent that discovers the vocabulary, checks policy hints, and retrieves facet state through three separate tool calls before it even begins answering can, therefore, add two to three seconds to the response time, and that's before it performs the actual product search. These are order-of-magnitude estimates, not benchmarked figures, and the real numbers depend heavily on the model, the network path, and how the tools are implemented.


Replacing those discovery calls with a parallel fetch (multiple context-building queries can be done in parallel) that runs before the LLM is invoked removes most of that cost. The context build takes roughly the same wall time as a single LLM tool call, but it replaces three or four of them. The LLM would either need to repeat failed searches or do its own context-building tool calls sequentially to get enough context to be successful. Precomputing context is also deterministic rather than subject to the model's tool-selection choices, which gives the business more control to fine-tune the experience.


The token reduction follows the same logic: Each exploratory tool call returns raw data the model must process. A preassembled context summary replaces that raw data with structured facts the model can consume in one pass. At the scale of usage possible in public-facing retail websites, this token cost saving can be significant. This work on document search ([Cutting agent costs with pre-computed context](https://www.elastic.co/search-labs/blog/pre-computed-context-llm-agent-costs) ) showed up to 75% input token reduction on a controlled benchmark. That benchmark was document retrieval rather than ecommerce, and its authors are explicit that the multiplier isn't a fixed number you can expect everywhere. We expect the direction to hold in ecommerce, because the exploration pattern is the same; it just runs against a structured catalog rather than a document corpus. The magnitude is something each team should measure against its own traffic.


### The AI shopping agent with full context


Remember the query that left you guessing: an outfit for an autumn wedding. Run it again, but this time, before you have to think, you’re handed a precomputed context as a short brief:


- This shopper’s name is Sarah, female, age 32, and she buys womenswear, size 12.
- She typically buys your mid-tier ranges.
- "Autumn" here matches these specific color labels: “rust”, “burgundy”, “forest green”, “camel”.
- Matching departments: “Womenswear”, “Menswear”.
- Matching tags: "occasion dresses”, “trouser suits”, “wedding”.
- She already has a burgundy bag in her basket.
- House rule for wedding-guest looks: Complete the outfit. Show hats and accessories, not just the dress.


Suddenly, you’re not guessing; you’re styling for this customer. And the interesting part is how those facts combine rather than just stack. The season proposes a whole autumn palette; the burgundy bag already in her basket narrows that palette to the few tones that coordinate with it; the house rule tells you to finish the look with a matching fascinator rather than stopping at the dress. Together, these facts let you answer like someone who knows both this customer and this shop, in a single pass, with nothing invented.


That short brief is exactly what the signal stack produces: the shopper's profile, the resolved vocabulary, the live basket, and the business policy. These are assembled in parallel and placed in front of the agent before its first move, so the " *What do I even do with this?* " problem never has to be solved one expensive tool call at a time.


## Governing the context layer without automated drift


One difference between the approach described here and automated knowledge extraction systems is worth addressing directly: In ecommerce, the context index cannot self-update without human review.


The policies that govern how an agent responds to gift queries, alcohol queries, or queries from customers in certain age brackets aren't just relevance configurations; they're trading decisions with potential legal and brand implications. An automated system that generates new policies from traffic patterns, without review, is a compliance risk before it's a technical asset.


This is actually the right constraint for most retail organizations, and it aligns with how search teams already work. Merchandisers write boost rules. Search teams maintain synonym configurations. Content teams approve what language appears in automated recommendations. The context policy layer is the same kind of governed configuration; it just serves a different consumer, namely, the agent's reasoning step rather than the query pipeline.


It's worth noting where this differs from the document-retrieval work referenced earlier. In that experiment, the biggest accuracy gain came from an automated feedback loop that fed the agent's wrong answers straight back into the extractor. That works well for factual question-answering, where "right" and "wrong" are unambiguous. In ecommerce, the equivalent signals still surface automatically, but a human decides what to do with them, because the changes carry trading and compliance weight. The loop is the same shape; the publication step has a person in it.


The governance loop that works in practice has two tiers:


- **Automatic signal surfacing:** Zero-result queries, repeated reformulations on the same topic, and sessions that end without a purchase after an agent interaction are all signals that something in the context layer is missing or wrong. These surface automatically as candidates for improving the experience, for example: a vocabulary term that didn't resolve or a policy that didn't fire on a query type it should have covered. To do this, you need a thorough log of conversations, including the reasoning and tool call trace in a platform like Elastic. This allows you to analyze the performance of the agent using both structured tools, for example, percentage increase in thumbs-down conversations and semantically. You could also run an automated review of conversations about "gifts" around December to characterize the thumbs-up/down ratio across an AB test of two agents.
- **Human authorship and review:** The search or merchandising team reviews candidates and authors the appropriate vocabulary entry or policy. Policies go through approval before publication. This typically mirrors the workflow that already exists for synonym changes or boost rule modifications; the tooling is the only new part.


## How to implement context engineering in phases


- **Phase 1: Vocabulary layer** (highest leverage, bounded engineering task).
- **Phase 2: Facet state and initial policies** (leans on the same catalog and retrieval primitives).
- **Phase 3: User profiles and session signals** (requires CRM and analytics integration; best added when the agent is active).
- **Phase 4: Governed feedback loop** (shifts to organizational alignment; surfaces gaps for merchandising teams).


The full signal stack described above doesn't need to be built at once, and the order isn’t arbitrary. The highest-leverage starting point is also the lowest in implementation complexity: the vocabulary layer. A semantic metadata index built from a full-catalog aggregation (canonical color values, brand aliases, category paths, field names) is a bounded engineering task, and an agent that can resolve "navy jumper" to color: NAVY, masterCategoryNames: "Knitwear & Jumpers" before its first tool call is materially better than one that discovers this through trial and error. If you build nothing else, build this.


Facet state and the first set of policies can follow close behind, often in parallel, because they lean on the same catalog and the same retrieval primitives. The later layers, such as user profiles, session signals, and the governed feedback loop, are where the work shifts from search engineering to organizational alignment. To implement CRM system integration, merchandising workflow changes, and the analytics needed to surface gaps can take a significant amount of work. Those layers are more valuable once the agent is already in regular use and generating the traffic signals that make the governed loop worth running. The important feature is that each layer stands on its own, so a retailer gets real value from phase one without committing to phase six.


## What infrastructure does a context layer need?


Precomputing context at the depth described here places specific requirements on the underlying platform. It's worth being explicit about these, because the temptation in early agent builds is to reach for the simplest available tool for each capability.


- **Semantic search** to match natural language queries against the vocabulary index and surface the right canonical values. Fuzzy keyword matching alone won't resolve ambiguity between similar brand names or color terms.
- **A percolator** to implement the policy layer. A percolator reverses the usual search direction: Instead of matching a query against stored documents, it stores the queries and matches an incoming piece of text (here, the customer's message) against them. That's exactly what policy matching needs, because each policy is essentially a saved pattern that says "When a query looks like this, surface this hint."
- **Real-time aggregations** over the full product catalog to produce accurate facet state at query time. Precomputed facet snapshots go stale quickly in active catalogs; query-time aggregations are the more reliable source.
- **Document retrieval by key** for user profiles: fast, single-document lookups by user ID that must complete within the context build window.
- **Structured and semantic logging and analytics** over query traces and agent interactions, which are the raw material for the governed loop's automatic signal surfacing.


These are standard capabilities of a mature search and analytics platform, rather than six separate systems, and[Elasticsearch](https://elastic.co/elasticsearch) provides all of them in one place. That matters less as a procurement point than as an architectural one: When semantic matching, percolation, aggregations, profile lookups, and analytics all run against the same catalog in the same cluster, the context layer stays consistent with the search layer by construction. Splitting these across a separatevector store and a separate analytics platform is a legitimate choice, but it adds operational surface and introduces a consistency problem between two systems that are reasoning about the same products. The context infrastructure is simplest to run when it lives where the product data already lives and can be updated without a separate extract, transform, load (ETL) step.


## Conclusion: Context engineering is a search team's job to own


The retailers who run effective AI shopping experiences at scale aren't the ones with the largest models or the most generous token budgets. They're the ones who have done the work to make their catalog, vocabulary, and tpolicies legible to an agent before it starts reasoning.


The good news is that most of the work is already done. The vocabulary is implicit in the catalog. The policies exist as merchandising rules and compliance guidelines. The user profiles are in the CRM. The session signals are in the analytics stream. The gap isn't data; it's the assembly layer that turns those signals into a structured context the agent can consume before it starts reasoning.


The search team already owns the vocabulary, policies, and merchandising workflows. The context layer is the right home for work the search team is already doing, in a form that serves the agent as well as the query pipeline. And, because it grows every time a gap is found and filled, it behaves less like a setup cost and more like an asset that compounds.


To begin building a context layer in Elastic, you can start a[trial of Elastic Cloud](https://www.elastic.co/cloud?utm_campaign=G-TXT-EMEA-UK+CA-Core-EN-Lead_Gen-CloudTrials-BR&utm_content=Brand-Cloud&utm_source=google&utm_medium=cpc&device=c&utm_term=elastic%20cloud%20trial&utm_id=701610000005lJVAAY&gad_source=1&gad_campaignid=22979576770&gbraid=0AAAAADrDgoJnVYpNJwbmfVxoTcZSmr4S8&gclid=CjwKCAjwx7LSBhB3EiwAjcodxPrvWRgCciehjf-6cu_sOb7FxbwDEJiS8Dpl95oQo7D2J61zXLJrgRoCtgQQAvD_BwE) or[run locally](https://www.elastic.co/docs/deploy-manage/deploy/self-managed/local-development-installation-quickstart) . You should become familiar with configuring[semantic search](https://www.elastic.co/docs/solutions/search/semantic-search) , and if you’re interested in how to build, store, and match search policies at query time, you will enjoy[this blog](https://www.elastic.co/search-labs/blog/elasticsearch-percolator-search-governance) .


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Operations](https://www.elastic.co/search-labs/blog/category/operations)


July 28, 2026


#### [Your agents have been keeping receipts: turning Elastic Agent Builder's built-in OTel traces into token cost dashboards in Kibana](https://www.elastic.co/search-labs/blog/opentelemetry-tracing-agent-builder)


Your Agent Builder agents already log every LLM call as an OTel trace, and that agent tracing data can power token cost dashboards and budget alerts before one runaway conversation quietly wrecks your month.


MMPM


By:[Meghan Murphy](https://www.elastic.co/search-labs/author/meghan-murphy)


and[Pablo Neves Machado](https://www.elastic.co/search-labs/author/pablo-neves-machado)


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


July 28, 2026


#### [One prompt, a complete workflow: Elastic's AI agent writes your automation for you](https://www.elastic.co/search-labs/blog/ai-workflow-automation-natural-language)


Elastic Workflows takes a plain-text prompt and generates YAML you can inspect, version and run against your Elasticsearch data. Now GA, with human-in-the-loop workflows in Slack, parallel execution, and 10 new connectors.


TESG


By:[Tinsae Erkailo](https://www.elastic.co/search-labs/author/tinsae-erkailo)


and[Shahar Glazner](https://www.elastic.co/search-labs/author/shahar-glazner)


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 27, 2026


#### [56% faster, up to 50% better retrieval performance: What's inside Jina's new 600 million parameter listwise reranker](https://www.elastic.co/search-labs/blog/jina-reranker-35-legal-medical-structured-data)


Jina Reranker 3.5 beats v3 by 50%+ on case law, closes the gap with models 7x its size on legal, medical, and financial benchmarks, and beats them outright on structured data. It's a drop-in replacement for v3, with no API changes.


FWSM


By:[Felix Wang](https://www.elastic.co/search-labs/author/felix-wang)


and[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 16, 2026


#### [A picture is worth 1.5x the words: What we learned benchmarking product search embeddings](https://www.elastic.co/search-labs/blog/multimodal-embeddings-ecommerce-product-search)


We benchmarked two embedding models on 5,000 real products and found that combining image and text beats either alone by up to 50%. Here's the data and the model that won.


SV


By:[Sofia Vasileva](https://www.elastic.co/search-labs/author/sofia-vasileva)


[Analytics](https://www.elastic.co/search-labs/blog/category/analytics)[ES|QL](https://www.elastic.co/search-labs/blog/category/esql) +1


July 8, 2026


#### [How to build search analytics on Elastic using OpenTelemetry, no extra pipeline required](https://www.elastic.co/search-labs/blog/search-analytics-opentelemetry)


How to instrument your search application to use modern Open Telemetry standard to drive insights in to your search and users.


MA


By:[Matthew Adams](https://www.elastic.co/search-labs/author/matt-adams)

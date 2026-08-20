---
schema_version: "1.0.0"
document_id: "0da7c8bd680b2f293a9f0e1f71b5319dd6cdc873716245f7b98bc129cec4c68a"
company_key: "yc-sitefire"
company: "sitefire"
source_id: "yc-sitefire-news-import-97a7181f264f"
canonical_url: "https://sitefire.ai/blog/query-fan-out"
published_at: "2026-03-03T23:00:00+00:00"
first_seen_at: "2026-07-28T19:25:04.800018+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:3ab08d10b346a310d24e99ca3cf8081076dfc07cd3dc600fefb190aa7fd72061"
---

# Query Fan-Out: What AI Search Actually Does With Your Query

There's a heated debate in the search industry right now. One side says GEO is the future and SEO is dead. The other says GEO is just a rebrand of what SEO people have been doing for years. Both sides are wrong, and both are right.


There's a debate in the search industry right now. One side says GEO is the future and SEO is dead. The other says GEO is just a rebrand of what SEO people have been doing for years. Both sides are wrong, and both are right.


I don't care about the label. What I care about is understanding what the machine actually does when someone types a question into ChatGPT, Google AI Mode, or Perplexity. Because once you see the mechanism, the debate resolves itself.


The mechanism is called query fan-out. It is the single most important concept for understanding how AI search differs from traditional search. And yet most articles about GEO either skip over it or explain it at surface level.


Here's what actually happens, step by step, when an AI search engine processes your query.


## How Traditional Search Works


This part is easy. A user types a query, say "best CRM for small business." Google runs that query against its index, ranks the results, and returns ten blue links. One query in, one ranked list out.


Your job as an SEO is to rank on that list. You optimize a page for that keyword, build authority, earn backlinks, and work your way up. The unit of competition is clear: one keyword, one ranking - a model that has worked for 25 years.


A single search step in AI search still works exactly this way. What changed is what happens before and after.


## What Is Query Fan-Out?


Query fan-out is the process where AI search engines decompose a single user query into multiple sub-queries, search each one independently, then merge the results into one synthesized answer.


When a user types "best CRM for small business" into Google AI Mode or ChatGPT, the AI doesn't run one search. It rewrites the query into 5 to 20 sub-queries. Something like:


-


"top CRM software for small business 2026 comparison"


-


"CRM pricing for small teams under 50 employees"


-


"HubSpot vs Salesforce vs Pipedrive small business"


-


"CRM features most important for small business"


-


"small business CRM reviews and ratings"


-


"easy to use CRM for non-technical teams"


-


"CRM with best free tier for startups"


Each sub-query is searched independently against the web index. The results are merged, and the AI synthesizes one comprehensive answer from the combined results.


One query in, many searches out. That's query fan-out.


The numbers vary by platform and query complexity. In Sitefire's analysis of 1.47 million AI answers, we see 4 to 10 sub-queries per prompt: less for ChatGPT, more for Google Gemini and Perplexity. We call this the first stage of the AI citation pipeline. The sub-queries determine which pages even have a chance of being cited. If your content doesn't match what the AI searches for, you're invisible before the ranking even starts.


## Where Do the Sub-Queries Come From?


Sub-queries are generated through systematic intent expansion, where the AI identifies related topics, adjacent intents, and predictable modifiers like "comparison," "best," and "reviews."


If you've ever looked at Google's People Also Ask boxes, you already have a good intuition for what's happening. The AI identifies related intents, adjacent topics, and natural follow-up questions, much like PAA does today. Sitefire's prompt analysis across all major AI models confirms this: the sub-queries map closely to the People Also Ask clusters Google already surfaces.


On top of that, AI models insert predictable modifiers: "comparison," "best," "reviews," "2026," "vs." You can see this happening in ChatGPT's thinking panels when it searches the web. It's not mysterious. It's systematic intent expansion.


The academic foundation is a framework called "Rewrite-Retrieve-Read," introduced by researchers at[Microsoft Research and Shanghai Jiao Tong University](https://arxiv.org/abs/2305.14283) in 2023. Their core insight: there's a gap between what users type and what needs to be searched. The user asks a natural language question. The search engine needs specific, well-formed queries. The LLM bridges that gap by rewriting before searching.


Google has invested heavily in this direction.[Patent US12158907B1](https://patents.google.com/patent/US12158907B1/en) , granted in December 2024, describes a "Thematic Search" system that identifies themes from search results and generates refined sub-queries for each theme. It's not purely parallel. It can go multiple levels deep: themes break into sub-themes, which break into sub-sub-themes. This is architecturally central to how[Google AI Mode](https://blog.google/products/search/google-search-ai-mode-update/) works.


Here's the critical detail: only 27% of fan-out sub-queries remain stable across repeated searches of the same prompt ([Similarweb](https://www.similarweb.com/blog/marketing/geo/geo-keyword-research/) ). Run the same query twice, and the AI generates largely different sub-queries each time. There is no single fixed set of queries to optimize for. The system is inherently dynamic.


## How Are the Results Merged?


Reciprocal Rank Fusion (RRF) is the algorithm that combines ranked lists from multiple sub-queries into a single merged ranking. It scores pages higher when they appear across multiple result lists.


Each sub-query produces its own set of ranked results. But the AI needs to combine all of these into a single pool of sources before synthesizing an answer. This is the part most articles skip.


The standard technique is RRF. Originally published by[Cormack et al. in 2009](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf) , it's now the industry standard for hybrid search.[Microsoft Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking) documents RRF as its merging algorithm. ChatGPT searches via Bing, which is an Azure product. The connection is direct.


In plain terms: if your page shows up in 5 out of 12 sub-query results, it scores dramatically higher than a page that appears in only 1. A[CommonCrawl analysis](https://commoncrawl.org/blog/how-seos-are-using-common-crawls-web-graph-data-for-ai-ranking-signals) of 3,000 ChatGPT citations found over 120 co-citation relationships - pages that consistently appear together across sub-query results. This is RRF in action. Topical breadth creates compounding visibility.


After rank fusion, the AI doesn't read full pages. It extracts content chunks, self-contained passages typically 130 to 170 words, that directly answer specific aspects of the query. Lists, tables, clear paragraphs with concrete data. Those chunks become the raw material for the synthesized answer.


This is what Sitefire measures when we analyze the citation funnel: from query fan-out to sourced pages to cited pages to brand mentions. Each stage has a drop-off. Understanding RRF explains why pages with broad topical coverage survive the merge step while single-keyword pages don't.


## What Actually Changed?


The difference between traditional search and AI search is not the search itself. It is the surface area of queries your content competes on.


Let's be honest. The search step in AI search is still search. The same web index. The same ranking signals: domain authority, relevance, freshness, backlinks. The retrieval doesn't use some exotic new algorithm. It uses the web search infrastructure that already exists.


What's different is what happens before and after:


-


**Before:** The AI rewrites one query into 5 to 20 sub-queries. Each one is a separate search with its own set of competing pages.


-


**After:** Results from all sub-queries are merged via rank fusion and synthesized into one answer. The user never sees a list of links. They see an answer with citations.


Dimension Traditional Search AI Search (Fan-Out)


Queries executed 1 per user input 5-20 sub-queries per prompt


Results format 10 blue links 1 synthesized answer with citations


Merging method None Reciprocal Rank Fusion


Unit of competition 1 keyword, 1 SERP Full cluster of sub-queries


Unit of retrieval Full page Content chunk (130-170 words)


User behavior Click and evaluate Read answer (92-94% zero-click)


Ranking signals DA, relevance, freshness, backlinks Same signals, wider surface area


The zero-click rate in AI Mode is 92 to 94% ([Ekamoira](https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible) ). Users get their answer without visiting a website. Brand visibility in the AI response is the new visibility.


That table tells the whole story. The ranking signals are familiar. The web index is the same. What changed is the surface area. Instead of competing on one keyword per user intent, you're now competing on 5 to 20 sub-queries, most of which you've never explicitly optimized for.


## Why the Data Backs This Up


If query fan-out didn't matter, you'd expect the same pages to win in both traditional search and AI search. They don't.


A[Surfer SEO analysis](https://surferseo.com/blog/query-fan-out-impact/) of 173,902 URLs across 10,000 keywords found that 68% of pages cited in AI Overviews were not in the top 10 organic search results.[Similarweb](https://www.similarweb.com/blog/marketing/geo/geo-keyword-research/) reports that 25% of URLs cited by ChatGPT had zero organic Google visibility. Only 12% of LLM-cited URLs rank in Google's top 10 for the head term.


Why? Because those pages rank for the sub-queries, not the head term. A page about "CRM pricing comparison for teams under 50" might never rank for "best CRM for small business" in traditional search. But when AI Mode generates that as a sub-query, the page gets retrieved, merged into the citation pool via RRF, and ends up in the synthesized answer.


Pages that cover the fan-out query space are 161% more likely to earn AI citations than those ranking only for the head term ([Ekamoira](https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible) ). The Spearman correlation between fan-out coverage and AI Overview citations is 0.77, strong, and much higher than the correlation between traditional rankings and AI citations.


Meanwhile, 88% of AI citation opportunities are missed by traditional SEO-only strategies ([Ekamoira](https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible) ). Sitefire's tracking data confirms this pattern: brands that monitor only their head-term rankings are blind to the sub-query surface where most AI citations actually happen.


## The Bottom Line


GEO is not replacing SEO. It's not a separate discipline. It's what happens when search engines add a query rewriting step before doing the same search they've always done.


The ranking signals are familiar: authority, relevance, freshness. The web index is the same. What changed is the surface area. Call it GEO, call it SEO. The key mechanism is query fan-out.


And the interesting question is whether your content is visible across the full set of queries AI generates from a single prompt, not just the one the user typed.


Now you know what the machine does. In the next part, we'll cover what to do about it.


## Frequently Asked Questions


### How many sub-queries does AI search generate per prompt?


Typically 5 to 20 for standard queries, depending on the platform and complexity. Google AI Mode can generate hundreds via Deep Search. Healthcare and e-commerce queries tend to trigger more sub-queries (22 to 28) than general informational queries (8 to 12). The exact number varies with every search.


### Can I see which sub-queries ChatGPT generates?


Yes. Open your browser's DevTools, go to the Network tab, and search for your chat ID in the response payloads. Look for` search_model_queries` to see the exact sub-queries ChatGPT generated. This is the most direct way to observe query fan-out in action.


### Does traditional SEO ranking still matter for AI visibility?


Yes. The search step in AI search uses the same web index and ranking signals as traditional search. Pages with strong domain authority, fresh content, and relevant backlinks still perform better in sub-query results. But ranking for one head term is no longer sufficient. Your content needs to appear across multiple sub-query results to score well in rank fusion.


### Is query fan-out the same across all AI search engines?


No.[ChatGPT sends sub-queries to Bing](https://help.openai.com/en/articles/9237897-chatgpt-search) . Google AI Mode uses Google's own index. Perplexity maintains its own 200B+ URL index. Each platform generates different sub-queries and applies different merging logic. A page cited by ChatGPT may be invisible to Perplexity, and vice versa. Sitefire tracks AI visibility across all major models for this reason.


### What content format works best for AI citation?


AI systems extract content chunks of 130 to 170 words. Self-contained passages with clear headings, data tables, and specific facts are preferred. Pages with statistics are up to 41% more likely to be cited ([Aggarwal et al., GEO: Generative Engine Optimization](https://arxiv.org/abs/2311.09735) ). Structure and specificity matter more than length.


## Key Takeaways


-


Query fan-out is the core mechanism of AI search: one user query becomes 5 to 20 independent sub-queries before any search results are returned.


-


The search step in AI search uses the same web index and ranking signals as traditional search. What changed is the surface area.


-


Reciprocal Rank Fusion (RRF) merges results from all sub-queries. Pages appearing across multiple result lists score exponentially higher.


-


68% of AI-cited pages are not in the top 10 organic results, because they rank for sub-queries, not the head term.


-


Only 27% of fan-out sub-queries are stable across repeated searches. Broad topical coverage beats single-keyword optimization.


-


Pages covering the fan-out query space are 161% more likely to earn AI citations than those targeting only the head term.


-


GEO is not replacing SEO. It is SEO applied to a wider set of queries that AI generates from a single prompt.


## Sources


-


[Similarweb](https://www.similarweb.com/blog/marketing/geo/query-fan-out/) . "Query Fan-Out in AI Search." 2025.


-


[Similarweb](https://www.similarweb.com/blog/marketing/geo/geo-keyword-research/) . "GEO Keyword Research." 2026.


-


[Ma et al.](https://arxiv.org/abs/2305.14283) "Query Rewriting for Retrieval-Augmented Large Language Models." arXiv:2305.14283. 2023.


-


[Google](https://patents.google.com/patent/US12158907B1/en) . Patent US12158907B1, "Thematic Search." 2024.


-


[Google](https://blog.google/products/search/google-search-ai-mode-update/) . "AI in Search: Going beyond information to intelligence." 2025.


-


[Cormack, Clarke & Buettcher](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf) . "Reciprocal Rank Fusion outperforms Condorcet and individual Rank Learning Methods." SIGIR 2009.


-


[Microsoft Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking) . "Hybrid Search Scoring (RRF)."


-


[CommonCrawl](https://commoncrawl.org/blog/how-seos-are-using-common-crawls-web-graph-data-for-ai-ranking-signals) . "How SEOs Are Using Web Graph Data for AI Ranking Signals." 2025.


-


[Surfer SEO](https://surferseo.com/blog/query-fan-out-impact/) . "Query Fan-Out Impact Study." 2025.


-


[Ekamoira](https://www.ekamoira.com/blog/query-fan-out-original-research-on-how-ai-search-multiplies-every-query-and-why-most-brands-are-invisible) . "Original Research on Query Fan-Out." 2026.


-


[OpenAI](https://help.openai.com/en/articles/9237897-chatgpt-search) . "ChatGPT search." Help Center.


-


[Aggarwal et al.](https://arxiv.org/abs/2311.09735) "GEO: Generative Engine Optimization." KDD 2024.

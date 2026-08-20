---
schema_version: "1.0.0"
document_id: "d253685b149748fd88a9b52128a37df5139764c9b62bd74fa666faa23b66c0ac"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/rapido-sql-rewrite-system/"
published_at: "2025-10-31T08:59:44+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:9ec171cd13873d5fec1a4dd6c1a36a5074f581e7414f44b3c33b8c60e8d4b000"
---

# How We Developed Our AI-Based SQL Rewrite System

For many database professionals I know, having an automatic SQL rewriting system has been something they’ve been aspiring to for many years.


Building such a system has been a challenge, but with the rise of LLMs it became much more feasible.


So we went on a journey and did it..
We also gave it a name that will fit what it does – **Rapido.**


## The Human Approach To Query Rewriting


When humans look at a query for potential rewrites, they:


- Search for anti-patterns in the query based on their experience, and possibly searching for such anti-patterns in the web
- Try different ways to write parts of the query (join to exists, row_number() to cross apply, etc)
- Go over the query plan information trying to understand where the bottleneck is
- Go over the table schemas and index/projections
- Apply hints


In some cases, when they have knowledge about the data in the tables and relations between the tables, they also:


- Check data distribution and adjust the query where possible
- Change the query based on the knowledge they have on the data of their company’s specific data model


They then try a few rewrites for the query until they find a fast enough one, in a process looking about like the below:


But there’s a limit to how much one person/team can do. **Can we use the Generative AI revolution for doing it at scale? Let’s do it!**


So what would such a system at Taboola need?


- Knowledge about general SQL query tuning techniques
- Taboola specific optimizations, relevant to Taboola data models
- Schema information (data types and indexes/projections)
- Data distribution information
- Query plan information
- Automated testing for finding the best rewrite


## Connecting The Pieces


As an AI summarized it, “at its core, Rapido is a pipeline that mimics an expert’s workflow. It starts by gathering context, then generates multiple rewrite ideas in parallel, benchmarks each one, validates the results, and finally takes steps for implementing the rewrite”.


Here’s the flow:


**Let’s explain each step.**


### Get query


Rapido gets queries from a few interfaces:


- UI, when a developer/analyst wants Rapido advice on a query
- Automations pulling queries from production
- Soon


- Git, for verifying queries get to production with good performance


As an example, here’s Rapido UI:


****


Developers and analysts can submit their query for checking through Rapido UI. They select their database type, whether it’s a backend or frontend query, the timeout for the query, and number of executions, for making sure we get consistent results. The user can also choose whether to use the predefined recipes or supply a custom prompt.


### Get relevant recipes


A recipe is something we teach the LLM to look for and guide it which rewrite to perform.


We supply an explanation for what to do, alongside an example query in which this recipe helped improve performance. Prompt engineering was key here. We send the recipe as part of the prompt with very clear instructions as to what to look for and how to apply a fix.


### General recipe examples


**Union to Union All**


“Optimize the Union operations in this query by:


– Converting UNION to UNION ALL where duplicates are impossible


– Optimizing individual queries within the UNION”


**Data Type Mismatches (Implicit Conversion)**


“Fix data type mismatches: Quote numeric values when filtering VARCHAR columns (e.g., *WHERE varchar_col IN (‘123’, ‘456’) not IN (123, 456)* )”


**Preaggregate With CTE**


“Rewrite the SQL query to pre-aggregate data with a Common Table Expression (CTE) to improve performance”.


**Merge CTEs**


“Sometimes people use too many CTEs.


Change the query logic to use less CTEs by merging CTEs that can be merged without changing the query logic.


An example for CTEs that can be merged are CTEs that query the same tables. Make sure the result is a valid Vertica SQL query and that the query logic stays the same”


**Not Exists To Left Join**


“Optimize this query by:


– Converting NOT EXISTS to LEFT JOIN with IS NULL check


– Maintaining correct NULL handling


– Preserving exact semantic equivalence”


## Taboola Specific Recipe Examples


- We guide the LLM to treat tables from certain schemas as fact tables, filter and preaggregate them using a CTE
- We identify a common unneeded historic join and rewrite the query to grab the relevant column from one of the fact tables
- We add a common missed column to a join which we know improves performance


In addition, we have a general prompt called “magic” for general performance guidance, in which the LLM can be more flexible:


“You are a Senior Database Engineer. Rewrite the Vertica query to optimize performance by:


– Converting complex subqueries to efficient JOIN operations


– Optimizing JOIN sequences for better performance


– Simplifying complex WHERE conditions


– Minimizing data movement between nodes where possible


– Improving GROUP BY and aggregation patterns


– Avoiding SELECT * and listing only needed columns


Maintain exact semantic equivalence while focusing only on SQL structure”


### What else do we pass the LLM for more context?


- Database type and version
- Schema information
- Data distribution information
- Query plan (explain)


### Do we pass all recipes for each query?


No we don’t. We parse the queries, and if, for example, the query doesn’t have “union”, we won’t pass the “union to union” all recipe.


### Why do we have a few recipes? Why don’t we use a single prompt and get a single query back?


Because as you might know from your query tuning work, each query is different, and something which works for one query doesn’t necessarily work for the other. So we get a query for each recipe and **let the best recipe win** .


## Test Each Recipe In Production


We get the rewritten query for each recipe, run it and measure performance. We run it a few times in order to verify we get a consistent runtime.


We run the rewritten queries In production, in order to make sure we get production runtimes.


### How do we make sure we don’t burn the kitchen?


We run the queries one at a time, in isolated servers/clusters in order not to hurt any production workload. We also limit the runtime and amount of resources each query can use.


### Logic validation: how do we verify the queries are logically equivalent?


The easy part is when it’s a session option we can control on our side. In this case, we only change the session option in a configuration table. Our automation is then verifying the performance is indeed better and rolls back the change if it isn’t.


If it’s not only a session option but an actual rewrite, we have a few options for verifications. In most cases, we perform a count and an unordered hash calculation over the result set. If they are the same between the original query and the rewritten one, and the performance boost is significant, we pass it to the query owner for human verification and implementing the rewrite in code.


## Results


So how does it look? Let’s look at runtime results for a few queries.


In this case, the original query ran for almost 60 seconds, while the “magic” recipe resulted in a 2 seconds runtime, and the “preaggregate_with_cte” recipe resulted in a 1.2 second runtime.


Here, the “magic” recipe reduced the runtime from 110 seconds to 5 seconds. The other recipes did not improve the query (the “magic” was to turn an IF, which is an external function in Vertica, into a CASE).


In this case, “merge_ctes” is the winner, with a runtime almost 15 times faster than the original query.


Here, the winner is “not_exists_to_left_join”, with a 15 seconds performance improvement.


And in this case, the “preaggregate_with_cte” recipe is the winner with runtime 56% faster than the original query, while the other recipes were actually slower than the original one.


As you can see, each query is different, and having a wide range of recipes increases the possibility for a good rewrite.


The UI also offers a comparison screen between the original query and the best rewrite, where the user can also see an explanation about the nature of the rewrite


## Challenges


We initially worked with the OpenAI GPT-4o model and got back many queries that were “too creative”. Lowering the temperature to 0 helped but not fully. We also got queries that were cut off. Both of the issues were resolved when we moved to the o3-mini model which allows more max_tokens and follows the recipes better without the need for temperature definition


Other things that we did for improving coverage:


- In cases where the LLM returns a query that has a syntax error, we provide the LLM with the error message and let it try to fix it. This improves the number of compiling queries by 30%
- Even though our recipes are descriptive and specific, we do allow some flexibility inside them, and have the general “magic” recipe which allows the LLM more creativity
- We make sure the LLM response doesn’t end prematurely


Another challenge was how to verify we supply a rewrite that is logically equivalent to the original query. As stated, we perform a count and an unordered hash calculation over the result set. However, what should we do in cases where data in the table changes frequently, or in cases where the rewrite generates a result set slightly different while the performance benefit is very big? We’re currently considering exposing such cases while mentioning the difference in percent between the data sets.


## Side Benefit


While developing Rapido, we needed a way to benchmark queries for knowing if a rewrite is better than its original query. We developed such a tool which is now being used by us humans when trying different ways for writing queries and when comparing performance between different database engines.


## Impact


Rapido is relatively new, and one of our current challenges is measuring the impact and understanding whether an optimization suggestion was implemented in production.


What we know so far:


- Up to 40% potential performance boost for specific workloads that were tested
- 15% performance boost by automatic fixes where they were possible
- 3200 human optimization hours were saved


## What’s Next For Rapido?


Rapido keeps being developed. Here are a few things we plan for it:


- More input interfaces:


- CI/CD pipeline, for making sure queries are deployed to production with proper performance
- MCP, for allowing other Taboola application utilize Rapido


- Optimizing queries over more database engines
- Automated comparison and benchmarking between database engines
- An optimization loop – further optimizing an already Rapido-optimized query
- Optimization targets beyond runtime (e.g. memory usage, CPU time)
- More recipes and enhancing the existing ones for better coverage
- More automations where possible


We believe the approach that led us with Rapido can be beneficial for many organizations.


## The Amazing People Behind Rapido


Rapido was developed by Yakir Gibraltar and Illés Solt who are in charge of the Rapido engine, Nati Poliszuk who’s in charge of the UI, and yours truly. It’s been an amazing journey developing it together, and we’ll keep on making it better.

---
schema_version: "1.0.0"
document_id: "d3f60aa62c39c1da237e7069ba4db75ae5e04e00da48b3d5cba72d99cc9cb458"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/vertica-query-optimization/"
published_at: "2025-05-12T15:38:24+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:1caa2f3890ccbd20aaa8ab2fb79810db00f5efdd65e82d9f15051c5ecfd4b70f"
---

# How Changing a Few Lines of Query Reduced the Execution Time by 80%

## The Challenge: A 7-Hour Job on 45 Billion Rows


At Taboola, we run a massive Vertica job that calculates analytics and KPIs through multiple processing stages. The core query scans **45 billion rows** spanning **90 days** , performing complex calculations to generate dynamic KPIs. With a **runtime of 7 hours per day** , this job wasn’t just consuming resources – it was delaying analysis downstream which impacts dev velocity.


Optimizing such a workload required a fundamental shift in our approach, starting with a deep dive into how we process and structure the data.


## Background: Meet Genie


Genie’s our A\\B testing analysis framework, which allows developers to choose a list of dimensions and metrics and get daily aggregated report tables including complex statistical calculations.


The requirement for Genie was to get for each dimension the aggregated value **over the entire time** of the A\\B test, so for example if I asked How many clicks I had per country & platform per day, Genie will create a row per day, country & platform with the **total Clicks over the entire period of the test** (see picture for example)


Sounds simple? We get around 600m new rows per day, but if a test runs for a few months and more dimensions, this could easily force our job to scan 45B (B as 45 billion rows :) rows and aggregate them every day from scratch.


##


## Step 1: Pre-Aggregation—The First Breakthrough


Our first idea was to adopt a cumulative **pre-aggregation** approach


Instead of every day recalculating ALL the data of the test (e.g. 90 days of data), just aggregate the last day, and accumulate it with previous “total-so-far” results (sum of last 89 days + today). This method eliminates the need to recalculate all the data every run, rather simply retrieve a single, lightweight entry.


We create a cumulative job that aggregates all data per day, each day adding only the new day into cumulative table:


Now, our job should scan fewer rows than before: **3B vs 45B** (>90% reduction in rows)


**But wait wait, you said ~600m rows, why 3B and not 600m??**


Good question! Initially, we thought each cumulative day would have the same number of rows as a single day from the raw data: ~600M rows.
However, because the key granularity varies between days (e.g. the same combination of country/platform isn’t present every day), we have more rows per day in the cumulative data.
For example if our dimensions are platform & country, in a specific day we might see only:


- Israel & Chrome, Israel & Safari, UK & **Chrome**


And in next day contains:


- Israel & Chrome, Israel & Safari, UK & **Edge** (yes there are still Edge users out there )


Aggregate of those 2 days = 4 rows overall (not only 3).


Ok so it sounded great on paper, and initial tests showed promise.
But… Reality had other plans.


## Step 2: The Mystery of Faster Queries


### Failures Before Success


When we deployed the cumulative solution to production, we noticed an odd pattern: the first execution would often fail after a **long period of time** , only to succeed in a retry just a **few minutes** later (!) — exactly as we expected it to work.
If the original failure was due to the complexity of the query, wouldn’t we expect the retries to also be slow? So why were the successful retries so fast?!


To make sure there’s no dark magic going on, we tried running the same production query on the same environment under a test job we saw the query is still super fast, the ONLY differences we could think was:


The production query always runs on a **newly created database daily partition** .
In contrast, the retries occur **hours later** and after at least a few queries were **already run on this partition** .


**Same query, same resources, same environment, same load – the only factor that changed? Timing. 🤔**


This raised our suspicion that additional factors were influencing the query’s performance.


### Database statistics


*The Vertica cost-based query optimizer relies on data statistics to produce query plans. If statistics are incomplete or out-of-date, the optimizer is liable to use a sub-optimal plan to execute a query* **. (**[Vertica doc](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/AdministratorsGuide/Statistics/CollectingDatabaseStatistics.htm?tocpath=Administrator%27s%20Guide%7CCollecting%20Database%20Statistics%7C_____0) **)**


When we added an initial step before actually running the query in production to report the[explain plan](https://docs.celonis.com/en/vertica-sql-performance-optimization-guide.html) of our query, we noticed Vertica reporting **missing_stats on the new day partition** which means statistics were still not established on the new partition (not table! The statistics are apparently managed per partition and not per table).


And this also explains why subsequent runs (the retries and our manual tests) were much faster – because the original query ran, Vertica had enough time to gather statistics on the new partition making all subsequent queries run MUCH faster.


### The Solution


By running[ANALYZE_STATISTICS](https://docs.vertica.com/24.1.x/en/sql-reference/functions/performance-analysis-functions/statistics-management-functions/analyze-statistics/) on **1% of the data for the date partition column** before executing the query, we provided Vertica with enough information to optimize its plan. The result? The query runtime dropped by a staggering **90%** !


## Step 3: Rethinking Temporary Tables


With the first step in our flow optimized, we turned our attention to the next step in the DAG. This part of the process relied on **temporary tables** and involved operations like COUNT(DISTINCT)


and GROUP BY


.


Initially, we assumed the COUNT(DISTINCT)


operation was the bottleneck. However, even after removing it, the query runtime remained largely unchanged.


The realization that this query primarily relied on GROUP BY


operations across multiple fields prompted us to explore how Vertica segments and sorts data across its nodes. In Vertica’s terminology, this is referred to as a “Projection.” This directly influences query performance by avoiding the data shuffle between nodes.


### The Revelation: Projections and Temp Tables


Digging into Vertica’s documentation, we found that[temporary tables inherit their projections](https://docs.vertica.com/12.0.x/en/sql-reference/statements/create-statements/create-temporary-table/) from the SELECT


statement used to populate them. Without explicitly defining a projection, the column order in the SELECT


statement could directly impact subsequent query performance.


Confusing? let’s take an example:


- The original query returned a temp table with the columns: **Country** , **Browser** , **Variant**
- The subsequent query running on the temp table grouped by: **Variant** , **Country** , **Browser**
- Because the temp table inherited the projections from its SELECT, even though the subsequent query grouped by the same list of columns, because the **order is different** **it had to shuffle the data across all nodes only to do the same exact group by!**


### The Solution


We aligned the column order in the SELECT


statement with the GROUP BY


operation in the subsequent query. This adjustment reduced the runtime of this step by **80%** —from **134 minutes to 23 minutes** !


the reason is that it avoids shuffle between vertica nodes:


## The Final Result: A Transformed Process


After some iteration, testing, and learning, the transformation was complete.


- Trying a pre-aggregation approach to reduce the overall data scanned.
- Running ANALYZE_STATISTICS


eliminated inconsistencies


and ensured optimal query plans.
- Optimizing the projection of temporary tables further slashed runtimes.


The result? A job that once took **7 hours** now completes its key steps in **under an hour** , freeing up valuable resources and ensuring timely delivery of results.


## Lessons from the Journey


Looking back, this journey offered valuable lessons for anyone optimizing large-scale Vertica queries:


1. **Pre-Aggregate Data for Efficiency**
Pre-aggregated data can make even the most complex queries faster
2. **Analyze Statistics**


Ensuring Vertica starts with the right query plan. Running ANALYZE_STATISTICS


at the right moment guided Vertica to generate better plans **from the start** , improving both reliability and performance.


Especially when you query a new partition – Running ANALYZE_STATISTICS


on the partition column even on 1% of the data before query execution can be a game-changer


1. **Pay Attention to Projections and shuffled data**


Temporary tables inherit their projections from the SELECT


statements used to populate them. Aligning the column order with downstream operations like GROUP BY


can lead to substantial performance improvements.


1. **Embrace Iteration and Discovery**
Optimization is a journey of trial and error. Every failure offers clues that bring you closer to the solution.

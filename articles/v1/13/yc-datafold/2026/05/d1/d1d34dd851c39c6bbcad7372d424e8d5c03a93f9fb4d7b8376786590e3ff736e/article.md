---
schema_version: "1.0.0"
document_id: "d1d34dd851c39c6bbcad7372d424e8d5c03a93f9fb4d7b8376786590e3ff736e"
company_key: "yc-datafold"
company: "Datafold"
source_id: "yc-datafold-news-import-e233b68116ce"
canonical_url: "https://www.datafold.com/blog/9-ways-to-screw-up-data-migration/"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-24T03:54:32.907441+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:caf508b5880dd1f57fbe41288a5a733be5b66634817f42d32a264da2124ea074"
---

# 9 Ways to Screw Up a Data Migration

Who in the data space hasn’t heard of a failed data platform migration that went over timeline and budget and failed to accomplish the stated goals?


Data platform migration is one of the most complex and consequential projects an organization can undertake.


The best way to ensure the success of such a project is to envision and iterate on multiple ways it can fail, and to develop defenses against those failure modes until you would be shocked to see it fail.


## My hard lessons


As a data engineer, I led a petabyte-scale migration of a data platform at Lyft, exceeding both the timeline and budget by 4x. I made big mistakes and learned a lot.


As CEO of Datafold, I work on automating data platform migrations, disrupting the status quo with a fixed-price, guaranteed-timeline, and data-quality offering made possible by full AI automation of the entire process. My hard learnings are now paying off with[happy customers](https://www.datafold.com/case-studies/) .


## #1: Delaying or avoiding the migration


No one wakes up on a good day thinking they suddenly need to migrate. If this is top of mind, there is a good business reason. Whether the legacy stack is so old that it’s approaching end-of-life, prohibitively expensive, fails to scale or meet SLAs, or you are just not happy with your current cloud data warehousing bill and want to diversify, something is not working.


While a migration is no small project and has real costs (more on that later), delaying or avoiding it until it’s too late can be much more expensive.


First, for purely financial reasons: you keep paying for software that doesn’t meet your business’s needs, while also likely paying for something more modern (that’s also not cheap). Not thinking about the renewal cycles is a sure way to get an email or two from your CFO.


Second, opportunity cost: what you COULD be doing with your data and with the money you are spending on the legacy software instead. AI opens so many new opportunities to leverage data and accelerate business operations. You dream up really cool ideas every week. Getting from idea to production is so easy. But not if your data is locked in some obscure on-prem database no one has access to.


**My advice:** at Lyft, we started the migration way too late when our legacy infrastructure was already massively overloaded, which not only delayed the process but crippled our work. Don’t wait till it’s too late!


## #2: Picking the wrong data stack to migrate to


Sounds obvious, right? Well, I spent 1.5 years as a data product manager leading a migration from Redshift to Hive, which was a complete failure. Granted, this was 2017, so migrating to Hive wasn’t completely stupid as it would be now, but it was still the wrong choice. TL;DR: by then, Hive (SQL on top of Hadoop MapReduce), which had terrible performance and even worse UX, had already peaked in adoption, and much better options, including Spark and Trino, were mature enough, not to mention cloud warehouses.


What IS the right stack to migrate to warrants at least a dedicated post, so this is just a reminder for you to ask your AI and human friends to double-check your choice of technology aligns with your business needs. Luckily, there is a lot to choose from, including on the open-source front (though you should pick open source for the right reasons).


## #3: Piece-mealing your migration


Examples of piece-mealing migrations:


*“We want to first deprecate Informatica by moving its logic into SQL Server, and then we will migrate that to Snowflake.”*


Here, you’re effectively doing the work twice, extending the timeline, increasing risk and slowing down adoption of the desired state.


*“We’ll migrate our stored procedures from Oracle to Databricks, and then when we’re done, we will introduce dbt for orchestration of the jobs.”*


In this case, the orchestration layer may seem like an easy migration because the dialect is the same, but at scale it remains a significant undertaking, given that orchestration has its own patterns. Migrating stored procedures to a platform like Databricks as-is can introduce significant inefficiencies — it’s best to refactor them to the target state and align with best practices during the migration.


Data teams trying to stage migrations do so with the best intentions, often seeing moving everything at once as riskier. But in fact, piece-mealing is almost certainly riskier: migrations are hard enough, and in both examples, splitting it into two phases increases the work by 50% to 100%.


**My advice:** migrate and modernize; go straight to the target state. This also gives you a chance to modernize and refactor legacy Frankensteinian patterns, such as Informatica-orchestrated stored procedures in Teradata, into a modern pipeline.


The only thing I would not change during the migration is the data model (the structure of tables, business logic, and definitions). Even though it’s possible to accelerate with AI, changing data model (effectively, interface to your data) requires getting alignment from a broad community of business users, which will slow things down significantly.


## #4: Doing it yourself


Migrations are challenging projects for a number of reasons:


- **Large scale** — need to rewrite millions of lines of code
- **Complexity** — the data platform often spans multiple systems with a lot of technical debt and poorly documented
- **High quality standard** — would your business be happy with the migration if, after you declare success, they see different numbers on the dashboard? I don’t think so. Generally, data users expect all the numbers to match up. Which is very hard, given reasons #1 and #2.


Doesn’t AI make it easy? It sure does! But it doesn’t make it a walk in the park. AI agents can spit out SQL faster than any human, but even frontier LLMs and the most popular agents drown in the scale and complexity of enterprise migrations. What you need is specialized AI, but more on that later.


Pulling off a successful migration using an internal team requires:


- Having a team of qualified data engineers available for the migration project
- Making sure they are fully ramped on agentic AI workflows
- Planning, organizing, and actively managing a complex migration project
- All while meeting your stakeholders’ expectations on maintaining SLAs and building new data products


I led a migration at Lyft (with a high-caliber team), and I’ve seen some of the fastest-growing companies blow their migration timelines by 2-3x, not because of a lack of talent, but because competing priorities make the already challenging project even harder to deliver.


**My advice:** outsource your migration to the right partner. To Datafold, of course :)


## #5: Outsourcing your migration to an IT service provider for billable hours


$100B+ IT Data Services industry has been happily staffing engineers, project managers, and QA specialists for migration projects for decades, charging their customers per-hour rates.


Nothing bad here, except one thing: incentives.


If you delegate a project, such as a data platform migration, to someone who charges by the hour, it is not, strictly speaking, in their best interest to complete the migration as quickly and cheaply as possible.


They will do their best to make you satisfied as their client, sure, but to maximize their profits, they need to charge you for as many hours as possible. You need to minimize migration costs and complete it by a particular date (e.g., to avoid a legacy platform renewal). You see the problem?


**My advice:** always delegate migrations on a fixed-price, guaranteed timeline basis. Many traditional IT Services shops resist that because it increases their risk, but there are players, including Datafold, who will deliver an outcome-based migration on a fixed-price, guaranteed timeline basis.


## #6: Not defining clear acceptance criteria


Defining success in migration is absolutely critical to achieving it. Yet so many teams (and certainly me early in my career) fail to do that until it’s too late in the project.


Let’s think from the first principles:


The output of a data platform is data products, such as tables, views, dashboards, and ML models. These data products need to be delivered and refreshed reliably according to a set schedule.


We want:


- The same data products rebuilt on the new platform, with no dependency on the legacy platform, so we can turn off the old stuff
- Full data parity: identical data values on the target platform across all tables/views/dashboards
- The same or better performance SLA (if the business expects to have the numbers refreshed by 9 AM, we should maintain that)
- Completed on time (e.g., by date X, which gives us a 2-month buffer before legacy contract renewal)


**It’s all about confidence in the data.** User acceptance testing (UAT) ultimately determines the success of the migration. For every migrated asset, someone (usually the data user) needs to sign off. To make an informed decision, that person needs to be confident in the data parity between the legacy and target state. Most drama and missed timelines result from the inability to pass UAT on time.


In my experience, passing UAT has two key ingredients:


1. **Visibility into data parity.** At Datafold, we use Data Diffs — value-level comparison reports across every asset. Data diffs also provide a match score — the percentage of values in a given table/view that match — which makes it easy to quantify and communicate parity.
2. **Explanations for deviations:** you won’t get 100% match score across all datasets in a large-scale migration. Legacy and new platforms may use different source data, operate on different refresh or ingest schedules, or just have non-deterministic code. Business stakeholders will accept not-perfect parity if they are confident in the explanation.


**My advice:** define success and align with your stakeholders early. Invest in the right tools, or partner with someone who has both a process and tooling to achieve user acceptance efficiently.


When Datafold delivers migrations, every data asset’s translation includes a data diff showing value-level data comparisons, along with a detailed explanation of any differences.


## #7: Carrying all technical debt over during the migration


For years, the wise data engineer said, “Always lift and shift.” That means, do the least possible amount of work to complete the migration. Never try to deal with technical debt WHILE doing the migrations. There is truth to it: migrations are hard enough to pull off without trying to reengineer things.


However, lift-and-shift comes with a real price tag in terms of carrying over technical debt.


Consider a popular pattern in legacy data warehouses such as SQL Server, Oracle, or Teradata. Stored procedures often include logic for processing the data, often using batches in loops. Often these sprocs span hundreds to thousands of lines of code. While you can technically make them run on a modern data platform by tweaking the dialect, the resulting performance will likely be quite poor because the code was written for a completely different database architecture. Let alone maintainability.


This is where specialized AI agents can help. LLMs are great at understanding and writing code.


During one migration, our customer, Evri, had to migrate sproc-based pipelines from SAP HANA to Databricks, about 5K LOC each. Each pipeline included incremental reads from a billion-row event source, 20-30 lookups to enrich the data, and upserts into the final dimension table. The pipeline included complex batch-based logic that processed tens of thousands of rows per iteration.


Datafold’s Migration Agent refactored that into Databricks Spark Declarative Pipeline, reducing code volume from 6K to 2K LOC per pipeline with much better performance and much cleaner code.


**My advice:** refactor the pipelines to align with the target system’s best practices. While refactoring, the implementation is key — point #8 explains why you must not touch the interface.


## #8: Trying to deal with too much technical debt during the migration


While modernizing the code to align with the target system’s best practices is generally a good idea, a particularly risky change is to remodel the data and business logic, which leads to a change in the data semantics and breaks downstream data usage.


It may be particularly tempting to change the data model of core tables and fix some bug or improve definitions. This is exactly what I tried to do when managing a migration. Grave mistake.


Changing the data model and business logic requires renegotiating contracts with data consumers, and while you may have perfectly fine reasons and good arguments, this adds friction and work and can delay the project.


**My advice:** keep the interface (data model, business definitions) the same. Modernize the implementation.


## #9: Not asking the target data platform’s account team for migration incentives


Migrations bring new workloads and, therefore, revenue to the target data platform. Wherever you are migrating, the target platform is highly interested in the migration and its success. Data platforms often provide various incentives in the form of credits or can partially or fully fund the migration cost to help you move over. Don’t hesitate to ask, as this may meaningfully change the economics of migration for your team.


## Making migration a success


At Datafold, we believe migrations shouldn’t be a gamble or a painful slog. Thanks to AI, it’s possible to complete migrations in weeks rather than years at a fraction of the cost it would have historically cost. But the success doesn’t just depend on the technology — so much depends on the data leaders making the right decisions at the right time. I hope this gives you at least food for thought, and I am always happy to chat!

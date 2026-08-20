---
schema_version: "1.0.0"
document_id: "29c902ea226411a4e0324605f3f17cf659c2977d3e35e58dc2d8f19b91f5adb7"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-database-optimization/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:ae22513657cd4a24db56da6454e72aef8eb1b75be142fb6d1208c01e1b500dec"
---

# Automatically optimize database queries with Datadog Database Monitoring

Nenad Noveljić


Senior Software Engineer


Alex Weisberger


Senior Software Engineer


Eden Gorevoy


Software Engineer


Jake Martin


Product Designer


Query optimization is a problem every team knows they should address but few have the time or expertise to do well. Identifying slow queries, reasoning about how to fix them, validating that a fix actually helps on your specific schema, and finding the right line of code to change all require deep database knowledge and careful testing. As a result, optimization usually becomes reactive to incidents, when the cost of inaction is already high.


Most database tooling stops at detection or, at best, a heuristic-based recommendation. Engineers are still left to interpret the suggestion, test it against their own environment, locate the code that produced the query, and decide whether the change is safe to ship.


Bits Database Optimization closes that loop by taking each candidate query through the full optimization life cycle. When[Database Monitoring (DBM)](https://www.datadoghq.com/product/database-monitoring/) detects a problematic query, it generates a candidate fix, runs both the original and the rewrite against a simulated copy of your schema, and only surfaces the optimization if the simulation proves it’s more efficient. From there, DBM hands the validated fix to[Bits AI](https://docs.datadoghq.com/bits_ai/) , which locates the code that built the query and opens a pull request with the diff. The PR includes the simulation evidence inline, so reviewers can ship the change without leaving their normal workflow.


In this post, we’ll walk through how Bits Database Optimization:


-


Identifies optimizations and generates rewrites


-


Validates each fix against a simulated copy of your environment, with no write access to your database required


-


Opens a pull request on the line of code that produced the query


-


Helps you verify the improvement on real traffic after merge


## Identify optimization candidates and generate rewrites and index recommendations


Bits Database Optimization scans your database fleet automatically and selects candidates based on signals already collected by DBM. It ranks queries by various criteria, including total execution time and the top queries for each service. For example, total execution time surfaces the queries with the largest cumulative wall-clock impact across your workload. These are the queries where a single rewrite delivers the most latency reduction in aggregate.


Because candidate selection runs against the telemetry DBM already collects, there’s nothing for you to configure, meaning no manual monitor setup, query lists, or tagging. Once a candidate is selected, Bits Database Optimization produces a concrete fix rather than a generic suggestion. Bits Database Optimization also surfaces missing[index recommendations](https://www.datadoghq.com/blog/database-monitoring-index-recommendations/) , but only when adding the index would actually change the plan for the query in question, not as generic suggestions to add covering indexes everywhere.


## Validate every fix against a simulated copy of your environment


Other tools tell you that a rewrite is probably faster. With Bits Database Optimization, you can run both versions and see the evidence through a simulation of your own schema and data shape.


The[Datadog Agent](https://www.datadoghq.com/blog/datadog-agent/) already collects table and index definitions as part of DBM’s schema collection. Bits Database Optimization replays those definitions into a fresh Postgres instance so the tables, indexes, and constraints mirror yours. It then populates the tables with synthetic data whose statistical properties match your production dataset (starting with cardinality and expanding to column-level distributions over time), so the query planner picks the same plans it would on real data. Queries run against synthetic test data covering frequent, rare, and normally distributed parameter values, designed to approximate real-world workloads. This means the simulation reflects the actual query shapes that matter instead of only the normalized form.


Original and rewritten queries run head to head, and Bits Database Optimization collects elapsed time, logical reads, WAL bytes written, and full before/after` EXPLAIN` plans. A rewrite is only surfaced as an optimization if the simulation shows it’s faster on your specific schema and data shape.


This means Bits Database Optimization never needs write access to your database **.** All simulations happen in Datadog’s environment against the reconstructed schema, so there’s no risk to production data and no privileged credentials to manage. Also, no customer data leaves your environment. The simulation runs on synthetic data generated from statistical metadata; your actual rows are never copied or transmitted.


The simulation is also rewrite-agnostic, which means it can validate any rule-based or LLM-generated candidate query. This positions the simulation environment as a general trust layer for query optimization, not just a backend for the rewrites we ship today.


## Open a pull request on the exact line of code


A validated fix is only useful if you can apply it. In practice, finding the line of code that builds a particular SQL query is one of the slowest parts of optimization work, especially in services where queries are constructed dynamically or sit behind an ORM.


Bits Database Optimization closes that last mile automatically. Using the[APM](https://www.datadoghq.com/product/apm/) span associated with the query, it resolves the owning service and the linked GitHub repository, then hands the rewrite to Bits AI, which locates the file and line that builds the SQL and opens a pull request with the minimal diff. The PR body embeds the simulation results (e.g., elapsed time delta, plan diff, affected tables) so that reviewers have the evidence inline and don’t need to click back into Datadog. From the normalized query view in DBM, a direct link takes you to the open PR.


Two prerequisites unlock automatic PRs: APM has to be configured for the service issuing the query, and the service needs a GitHub repository linked in Datadog. With those in place, the PR is created automatically as soon as the optimization is validated.


You can also configure a DBM monitor that fires when a new simulation optimization is available. The Slack notification includes a short description of the rewrite, the projected impact from the simulation, and deep links to both the optimization in DBM and the PR in GitHub, so the team can review and merge without leaving the channel they already work in.


## Verify the improvement on real traffic


After the PR merges and the change deploys, the rewritten query shows up in DBM Query Metrics with its new performance profile. Teams can confirm that the gain measured in the simulation holds up under production load and watch the elapsed time drop on the same dashboard they used to spot the slow query in the first place.


## Continuous query optimization without the overhead


With Bits Database Optimization, slow queries are detected automatically, fixes are validated against your own schema before they ever reach you, and the diff lands in the same review queue as the rest of your code changes. You spend less time hunting for the right query to fix and more time shipping. The database is continuously tuned in the background.


To learn more,[visit our documentation for Database Monitoring](https://docs.datadoghq.com/database_monitoring/) . If you’re new to Datadog,sign up for a free 14-day trial.


-
-
-

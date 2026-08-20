---
schema_version: "1.0.0"
document_id: "17f90b6dac78ac05432b600434326464007a490081d613fb97ba49adbc217a0f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-model-data-for-bi-star-schemas-wide-tables-and-how-much-you-need/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:d7a2381387e51cfdf9555e0af7f941e5f7e3348ded4f3eb0b6fea2d23834c0f7"
---

# How to model data for BI: star schemas, wide tables, and how much you actually need

Most BI dashboards do not break because of the chart library or the tool. They break because the data underneath them is shaped wrong: the grain is ambiguous, a join fans out and doubles a revenue number, or a “total” depends on which table you started from. Data modeling for BI is the work of shaping tables so that questions have one correct answer and a dashboard is hard to read wrong.


The short version: get the grain of each table right, keep your fact tables and your descriptive tables separate enough to join cleanly, and do only as much modeling as the question requires. A two-person startup querying a production database does not need the same model as a 200-person company with a warehouse. This guide explains the patterns (star schema, one big table, wide tables), how to choose between them, and a practical model for how much modeling you actually need.


It is written for founders, analysts, and data leads who are about to build dashboards and want the underlying tables to be trustworthy, not theoretically perfect.


## What does “data modeling for BI” actually mean?


For BI, data modeling is mostly about three decisions:


- **Grain.** What does one row in a table represent? One order? One order line? One daily snapshot per customer? This is the single most important decision, and most modeling mistakes are grain mistakes in disguise.
- **Shape.** How are facts (events and measures you sum or count) separated from dimensions (the descriptive attributes you filter and group by)? This is where star schemas and wide tables come in.
- **Location.** Where does the transformation live: SQL views, a dbt project, materialized tables in a warehouse, or calculations inside the BI tool? That is a separate decision, covered in[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


Modeling for BI is not the same as modeling for an application. An app database (often third normal form) is optimized for writes and consistency: many small, normalized tables, foreign keys everywhere. Analytics is optimized for reads: fewer, wider tables that are cheap to scan and easy to join for reporting. The two goals pull in opposite directions, which is why you usually model an analytics layer rather than charting directly off raw application tables.


## How much data modeling do you actually need?


This is the question most guides skip. The honest answer is that modeling effort should scale with data volume, number of sources, and how many people build their own queries. Here is a practical three-tier model.


**Tier 1: query the source directly.** A small team, one production database, a handful of dashboards. You do not need a warehouse or a star schema. Build a few SQL views to hide joins and name things clearly, and query them. The cost of over-modeling here is real: weeks spent building a dimensional model nobody needs. If you are at this stage, see[how to build a SQL dashboard](https://www.basedash.com/blog/how-to-build-a-sql-dashboard-from-queries-to-a-dashboard-people-trust) .


**Tier 2: a light reporting layer.** Multiple sources (production DB, Stripe, a CRM), more dashboards, and a few non-technical people who want to slice data. Centralize into a warehouse and build a thin set of cleaned, well-named tables: a few fact tables, a few shared dimensions (customers, dates, products). You do not need full Kimball formality, but you do need consistent grain and one canonical customer table.


**Tier 3: a governed dimensional model.** Many sources, many builders, and metrics that must reconcile across teams (finance, board reporting, investor numbers). This is where a real star schema, a date dimension, conformed dimensions, and a[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) earn their cost. The signals that you have outgrown the lighter tiers are the same signals that tell you to[add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) .


The mistake teams make in both directions: startups build Tier 3 models for Tier 1 problems, and scaling teams keep charting off raw tables long after they have Tier 3 complexity. Match the model to the stage.


## Star schema vs one big table: which should you use?


Two patterns dominate analytics modeling. A **star schema** keeps one central fact table (orders, events, payments) surrounded by dimension tables (customers, products, dates) joined by keys. **One big table** (OBT, sometimes called a wide table) pre-joins everything into a single denormalized table where each row already carries its descriptive columns.


Neither is universally correct. They trade flexibility against simplicity.


Attribute Star schema One big table (OBT)


Structure Fact table + separate dimension tables Single pre-joined wide table


Storage Lower (no repeated attributes) Higher (attributes repeat per row)


Query writing Requires joins No joins, simple` GROUP BY`


Non-technical friendliness Lower (must understand joins) Higher (everything in one place)


Reuse across dashboards High (dimensions shared) Low (table is purpose-built)


Risk of fan-out errors Higher if joins are wrong Lower (joins done once, correctly)


Schema changes Localized to one dimension Requires rebuilding the wide table


Best fit Many metrics sharing dimensions, governed reporting A specific dashboard or self-serve audience


A reasonable default for most teams: model a small star schema as the source of truth, then build one or two wide tables on top of it for the dashboards non-technical people touch. The star keeps things correct and reusable; the wide table makes self-serve querying safe because the joins are already done. Columnar warehouses (Snowflake, BigQuery, Redshift, ClickHouse) make wide tables cheap to scan, which is why OBT has become more popular than it was in the on-prem era.


## Get the grain right before anything else


Grain is the definition of what one row means. Decide it explicitly for every fact table, write it down, and never mix grains in the same table.


The classic failure looks like this. You have an` orders` table at one-row-per-order, and an` order_items` table at one-row-per-line. You join them to get product names, then sum` order_total` . Because each order has several line items, the join repeats the order row once per item, and your revenue is now inflated by the number of items per order. This is “fan-out,” and it is the most common source of wrong numbers on dashboards.


Rules that prevent most grain bugs:


- State the grain in the table name or a comment:` fct_orders` is one row per order;` fct_order_items` is one row per line.
- Sum a measure only at the grain it lives in. Order totals live at order grain; quantities live at line grain.
- When you need attributes from a finer table, aggregate them up first (one row per order) before joining, or join and then aggregate with care.
- Use a daily snapshot grain (one row per entity per day) for things like balances, subscriptions, or inventory that you want to trend over time.


If you only fix one thing about your modeling, make every fact table’s grain unambiguous.


## Build a date dimension early


A dedicated date table (one row per calendar day, with columns for week, month, quarter, fiscal period, weekend flag, holiday flag) is the highest-leverage piece of modeling for almost any BI setup. It lets every dashboard agree on what “last quarter” means, supports fiscal calendars that do not match the Gregorian one, and makes period-over-period comparisons trivial. It is cheap to build once and reused by every fact table. Most teams that struggle with inconsistent date filtering never built one.


## Where should the modeling live?


The same logical model can be implemented in several places, and the right choice depends on your stack:


- **SQL views** in the source or warehouse: lowest setup cost, good for Tier 1 and Tier 2, easy to inspect.
- **dbt models** : versioned, tested, documented transformations. The right tool once multiple people build models and you need[version control and tests](https://www.basedash.com/blog/bi-as-code-how-to-version-control-dashboards-metrics-and-semantic-models) . If you already run dbt, let it own the modeling and keep your[BI tool from duplicating those metrics](https://www.basedash.com/blog/bi-tools-for-dbt-how-to-choose-without-duplicating-your-metrics) .
- **Materialized tables / incremental models** : when views get too slow to scan repeatedly, materialize them on a schedule.
- **The BI tool itself** : fine for last-mile calculations (a ratio, a filtered measure), risky as the home for core business logic because it is hard to reuse and govern.


A good rule: keep the shape (joins, grain, cleaning) upstream in views or dbt, and keep only presentation-level calculations in the BI tool. This is the layering described in[the modern BI stack for lean teams](https://www.basedash.com/blog/the-modern-bi-stack-for-lean-teams-what-to-build-and-what-to-skip) .


## Handling change over time without overcomplicating


Dimensions change: a customer moves to a new plan, a sales rep changes territory, a product gets renamed. How you handle that history matters for accuracy.


- **Type 1 (overwrite):** keep only the current value. Simplest, and correct for attributes where history does not matter (a corrected typo in a name).
- **Type 2 (track history):** add a new row with valid-from and valid-to dates each time an attribute changes, so a historical order joins to the plan the customer had *at the time* . Necessary when you report things like “revenue by plan tier” over time.


Most early-stage teams over-build here, adding Type 2 history to dimensions nobody analyzes historically. Start with Type 1 everywhere, and add Type 2 only to the specific attributes where “what was true then” changes a reported number. Slowly changing dimensions are a real technique, not a default setting.


## Common data modeling mistakes that break dashboards


- **Charting straight off normalized app tables.** Many small joins, easy to fan out, slow to scan. Build a reporting layer instead.
- **Mixing grains in one table.** Order-level and line-level columns in the same table guarantee a future double-count.
- **No canonical customer (or user) table.** Three dashboards define “active customer” three ways, and none of the numbers match.
- **Modeling for purity instead of questions.** A perfectly normalized snowflake schema that requires six joins to answer “revenue this month” is worse than a wide table that answers it in one query.
- **Putting core logic in the BI tool.** Metrics defined in one dashboard cannot be reused or tested, and they drift from every other dashboard.
- **No date dimension.** Endless re-derivation of weeks, fiscal months, and “trailing 90 days” in every query.
- **Over-modeling too early.** Building Tier 3 infrastructure for a single-database, five-dashboard reality.


## A practical data modeling checklist


Before you build dashboards on a new model, confirm:


- Every fact table has a single, documented grain.
- Measures are summed only at their native grain (no fan-out joins).
- There is one canonical table each for customers, users, and products.
- A date dimension exists and every fact joins to it.
- Dimensions and facts are named so a non-technical teammate can guess what a column means.
- History (Type 2) is applied only where a reported number depends on past attribute values.
- Core joins and cleaning live upstream (views or dbt), not inside individual dashboards.
- The level of modeling matches your tier; you have not built a warehouse-grade model for a two-table problem.


## Where Basedash fits


Basedash is built for teams at Tier 1 and Tier 2: it connects directly to a production database or warehouse, so you can build dashboards on SQL views and well-named tables without standing up a full dimensional model first. Its AI can write and explain queries against your schema, which is useful when you are still shaping a reporting layer and want non-technical teammates to ask follow-up questions safely. For teams that have grown into a governed dimensional model in dbt or a warehouse, Basedash queries that model directly rather than asking you to redefine it. The point is to model only as much as your stage requires, and let the tool meet the model where it is.


## FAQ


**Do I need a star schema to build dashboards?** No. A star schema pays off when many metrics share dimensions and numbers must reconcile across teams. For a single database and a handful of dashboards, clean SQL views are enough. Add a star schema when reuse and governance justify it.


**Star schema or one big table for analytics?** Use a star schema as the reusable source of truth, and build wide one-big-tables on top for the dashboards non-technical people query. Columnar warehouses make wide tables cheap, so OBT is a reasonable default for purpose-built, self-serve dashboards.


**What is the grain of a fact table?** The grain is what one row represents, for example one order, one order line, or one customer per day. Defining it explicitly and summing measures only at that grain prevents the fan-out joins that inflate totals.


**Should I model data in the warehouse or in the BI tool?** Keep joins, grain, and cleaning upstream in SQL views or dbt so they are reusable and testable. Keep only last-mile presentation calculations in the BI tool. Core business logic in a dashboard cannot be reused and tends to drift.


**What is a slowly changing dimension, and do I need one?** It is a way to track how a descriptive attribute changes over time. Use Type 1 (overwrite) by default, and apply Type 2 (keep history) only to the specific attributes where a reported number depends on past values, such as plan tier in historical revenue.


**How do I avoid double-counting revenue in a dashboard?** Double-counting usually comes from joining a coarser table (orders) to a finer one (order lines) and then summing at the wrong grain. Aggregate the finer table up to the grain you are summing at first, or sum each measure only in the table where it naturally lives.

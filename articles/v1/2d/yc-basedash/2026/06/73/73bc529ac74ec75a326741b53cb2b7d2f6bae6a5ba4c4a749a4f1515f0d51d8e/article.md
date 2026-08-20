---
schema_version: "1.0.0"
document_id: "73bc529ac74ec75a326741b53cb2b7d2f6bae6a5ba4c4a749a4f1515f0d51d8e"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/what-is-a-data-dictionary-and-how-to-build-one-for-analytics/"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:f612ea3bc415af8dc2dea636b5bb8f33ba3d06d1cdf343f82d772263bb1189a1"
---

# What is a data dictionary, and how to build one for analytics

A data dictionary is a structured reference that defines what every field, table, and metric in your data means: its name, type, source, definition, allowed values, and who owns it. It is the answer to the question every analytics team gets ten times a week, “what does this column actually mean?”, written down once so nobody has to guess.


If you have ever seen two dashboards report different numbers for “active users,” or watched a new hire spend a week reverse-engineering what` status = 3` means, that is the gap a data dictionary fills. This guide explains what belongs in one, how to build it without turning it into a documentation project that nobody maintains, and how it differs from a data catalog and a semantic layer. It is written for founders, analysts, and data leads who want their data to be readable, not just queryable.


## What is a data dictionary?


A data dictionary describes the contents of a dataset at the level of individual fields. For each column it records the technical facts (name, data type, nullability), the business meaning (what the field represents in plain language), and the governance facts (who owns it, where it comes from, whether it contains sensitive data).


The key distinction: a data dictionary documents *meaning* , not just structure. A database schema already tells you that` orders.amount` is a` numeric` column. A data dictionary tells you that` amount` is the gross order total in cents before refunds and excluding tax, sourced from Stripe, and that you should use` net_amount` for revenue reporting. That sentence is the difference between a trustworthy dashboard and a support ticket.


Historically a data dictionary was a static document. In practice it now lives anywhere your team will actually read it: a table in your warehouse, a dbt schema file, a wiki page, a column-description field inside your BI tool, or a dedicated catalog. The format matters less than two things: it is correct, and it is close to where people use the data.


## Data dictionary vs data catalog vs semantic layer


These three terms get used interchangeably and they are not the same thing. They sit at different scopes and solve different problems.


Data dictionary Data catalog Semantic layer


**Scope** One database or dataset The whole data estate The metrics layer on top of tables


**Primary job** Define what each field means Discover and index assets across sources Compute consistent metrics from raw tables


**Granularity** Columns, tables, metrics Datasets, lineage, ownership Measures, dimensions, joins


**Who maintains it** Analysts, data owners Data platform team Analysts, analytics engineers


**Typical form** A document, table, or schema file A platform (Alation, Atlan, DataHub) A modeling layer (dbt, LookML, BI tool)


**When you need it** Almost immediately When you have many sources and teams When metric definitions drift


In practice they layer on top of each other. A small team starts with a data dictionary. As the number of sources grows, a[data catalog](https://www.basedash.com/blog/best-data-catalog-tools-compared-2026) automates discovery and lineage across them. A[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) goes one step further and turns the definitions into computed, reusable metrics so that “revenue” means the same thing in every chart. A data dictionary is the foundation the other two assume already exists.


## What should a data dictionary include?


A useful data dictionary records the following for each field. You do not need every attribute for every column, but the first five are close to mandatory for any field that shows up in reporting.


Attribute What it captures Example


**Field name** The technical column name` mrr_cents`


**Type** Data type and nullability` integer` , not null


**Definition** Plain-language meaning, in one sentence Monthly recurring revenue in cents, excluding one-time charges


**Source** Where the value originates Stripe subscriptions, synced hourly


**Owner** Person or team accountable for it Finance / @dana


**Allowed values** Enums, ranges, or units Cents; non-negative


**Sensitivity** PII or restricted-data flag None (no PII)


**Freshness** How current the data is Updated hourly


**Notes / caveats** Gotchas that cause wrong queries Excludes refunds; use` net_mrr` for net


The two attributes that do the most work are **definition** and **notes** . A good definition prevents the wrong column from being used. A good caveat (“this excludes refunds”) prevents the right column from being used the wrong way. If you only have time to write two fields per column, write those.


Beyond individual columns, a complete dictionary also documents the table itself: its grain (what one row represents), its primary key, its update cadence, and how it relates to other tables. Grain is the single most common source of double-counted metrics, so recording it explicitly is worth the effort. For more on why grain matters, see[how to model data for BI](https://www.basedash.com/blog/how-to-model-data-for-bi-star-schemas-wide-tables-and-how-much-you-need) .


## The three layers of a useful data dictionary


Most data dictionaries fail because they try to be a complete technical manual. A more durable approach is to treat the dictionary as three layers, and to be honest about which layer each field actually needs.


**1. The physical layer.** Column names, types, nullability, keys. This layer can be generated automatically from the database schema. Do not hand-write it. Anything a query against` information_schema` can produce should be produced that way.


**2. The business layer.** Definitions, units, caveats, and the mapping from technical names to business concepts (“` mrr_cents` is monthly recurring revenue”). This layer is where humans add value, and it is the part worth protecting. It cannot be auto-generated reliably, because the meaning lives in people’s heads, not in the schema.


**3. The governance layer.** Ownership, sensitivity classification, source, and freshness. This layer matters most for anything that touches customer data, finance, or compliance. It connects directly to access control: if a field is flagged as PII here, your[data governance](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) rules should restrict who can see it.


The practical implication is that you should automate layer one, invest human effort in layer two, and treat layer three as a governance requirement rather than nice-to-have documentation. Teams that try to hand-maintain all three layers usually end up with a stale dictionary that nobody trusts.


## How to build a data dictionary


You do not need a documentation sprint. Build it incrementally, starting with the data people actually query.


1. **Start with reporting tables, not everything.** Inventory the tables and views that feed your dashboards and recurring reports. These are the fields where a wrong definition causes real damage. Ignore the long tail of internal tables nobody queries.
2. **Auto-extract the physical layer.** Pull column names, types, and keys from` information_schema` (or your warehouse’s equivalent) into a table or spreadsheet. This gives you the skeleton in minutes.
3. **Write definitions for the fields that appear in reports.** Go column by column through the reporting tables and add a one-sentence definition plus any caveats. Prioritize fields that have ever caused confusion or a “which number is right?” conversation.
4. **Assign an owner to each table.** Ownership is what keeps a dictionary alive. An unowned definition is a definition nobody will update. Owners are usually the team that produces the data, not the team that consumes it.
5. **Flag sensitive fields.** Mark anything containing personal, financial, or otherwise restricted data. This list should match the fields your masking and access-control policies cover.
6. **Link the dictionary to where data is used.** The definition is most valuable at the moment someone is about to query or chart the field. Putting it next to the column in your BI tool or warehouse beats burying it in a separate wiki.


A realistic first pass covers the 50 to 150 fields that actually appear in dashboards. That is a few hours of work, not a quarter-long initiative, and it captures most of the value.


## Where should a data dictionary live?


The best location is wherever the people who need it already are. A dictionary that requires opening a separate tool is a dictionary that gets ignored.


- **In the warehouse / dbt.** If you use dbt, column and table descriptions live in` schema.yml` files alongside the models, and they are version-controlled with the code. This keeps definitions next to the transformations that produce them. It is the strongest option for teams that already model in dbt. See[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) for how this connects to metric definitions.
- **In the BI tool.** Many modern BI tools let you attach descriptions to tables and columns so the definition shows up exactly where someone is building a chart. This is the highest-leverage place for the business layer, because it reaches non-technical users who never open dbt or the warehouse.
- **In a data catalog.** Once you have many sources and teams, a catalog automates ingestion and lineage and becomes the central index. This is an upgrade from, not a replacement for, good definitions.
- **In a document or spreadsheet.** Perfectly fine for a small team. A shared sheet with the attributes above beats no dictionary at all. The risk is that it drifts from reality faster than schema-embedded options.


The pattern that scales: generate the physical layer automatically, keep the business and governance layers as close to the data as your tooling allows, and avoid maintaining the same definition in three different places.


## How to keep a data dictionary from going stale


A stale data dictionary is worse than none, because people trust it and get burned. Three habits keep it current.


**Tie definitions to schema changes.** When someone adds, renames, or repurposes a column, updating the definition should be part of that change, not a follow-up task. In a dbt workflow this is natural, because the description sits in the same file as the model. Treating documentation as part of the change is the same discipline as[versioning your BI as code](https://www.basedash.com/blog/bi-as-code-how-to-version-control-dashboards-metrics-and-semantic-models) .


**Make ownership real.** Every reporting table should have a named owner who is expected to keep its definitions accurate. Unowned fields rot. A short quarterly review where owners confirm or fix their definitions is enough for most teams.


**Capture caveats when they are discovered.** The most valuable entries are written the moment someone gets burned. When an analyst discovers that a field excludes refunds or resets at month end, that caveat should land in the dictionary immediately, while the lesson is fresh.


## Why data dictionaries matter more with AI-assisted BI


AI-assisted analytics raises the stakes on definitions. When a person writes a natural-language question and a tool generates SQL, the quality of the answer depends heavily on whether the model knows what the columns mean.


An AI assistant pointed at a schema with cryptic names like` flg_2` and` dt_x` will guess, and guessing is how you get confidently wrong numbers. The same assistant pointed at columns annotated with clear definitions (“` net_mrr` is monthly recurring revenue after refunds, in cents”) can map a question like “what was net MRR last month?” to the correct field on the first try. Definitions are no longer just for humans; they are context that AI systems read to generate correct queries.


This is part of why AI-native BI tools treat field and metric descriptions as first-class. Basedash, for example, surfaces table and column context where analysts and AI both work, so that generated SQL and human queries draw on the same definitions rather than diverging. A good data dictionary is the difference between an AI assistant that reflects your team’s real definitions and one that invents its own.


## When you need a data dictionary, and when you do not


You need a data dictionary when:


- More than one person writes queries or builds dashboards, and they need to agree on what fields mean.
- The same metric (“active users,” “revenue,” “churn”) shows up in multiple reports and has to match.
- Column names are cryptic, encoded, or inherited from a system nobody fully remembers.
- You handle sensitive data and need to track which fields are restricted.
- You use AI-assisted querying and want generated SQL to use the right columns.


You can skip a formal one when:


- A single person owns all the data and holds the definitions in their head, and the team is small enough that this is not a risk.
- The schema is small, self-explanatory, and stable.
- You are prototyping and the data is throwaway.


Even then, the moment a second person asks “what does this column mean?”, you have crossed the threshold. The cheapest version of a data dictionary is a shared document with the attributes from the table above; the most durable version lives next to the data itself.


## FAQ


**What is the difference between a data dictionary and a data catalog?** A data dictionary defines the fields, types, and meanings within one dataset or database. A data catalog is a platform that automatically discovers and indexes metadata across your entire data estate, adding lineage, usage stats, and search. A dictionary describes one source in depth; a catalog indexes many. Small teams start with a dictionary and add a catalog when the number of sources grows.


**Is a data dictionary the same as a semantic layer?** No. A data dictionary documents what fields mean. A semantic layer goes further and computes consistent metrics from raw tables, so “revenue” resolves to the same calculation everywhere. The dictionary is documentation; the semantic layer is executable definitions. They complement each other.


**Who should own the data dictionary?** Ownership should sit with the team that produces each dataset, not the team that consumes it. Producers know the source and the caveats. A central data or analytics lead can own the structure and process, but per-field definitions should map to the people accountable for that data.


**How do I create a data dictionary for a PostgreSQL or MySQL database?** Start by querying` information_schema.columns` to extract column names, types, and nullability into a table or spreadsheet. That gives you the physical layer automatically. Then add one-sentence definitions, owners, and caveats for the fields that appear in reports. If you use dbt, store these in` schema.yml` so they stay version-controlled with your models.


**How often should a data dictionary be updated?** Update definitions whenever the schema changes, as part of the change itself rather than as a separate task. Beyond that, a short quarterly review where data owners confirm their definitions is enough for most teams. The fields most likely to drift are calculated metrics and enums, so review those first.

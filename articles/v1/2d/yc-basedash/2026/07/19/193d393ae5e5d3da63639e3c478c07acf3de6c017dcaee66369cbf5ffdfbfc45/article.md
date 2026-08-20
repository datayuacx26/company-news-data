---
schema_version: "1.0.0"
document_id: "193d393ae5e5d3da63639e3c478c07acf3de6c017dcaee66369cbf5ffdfbfc45"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/teaching-our-ai-agent-whats-actually-in-your-database/"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-21T15:31:16.890008+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:23d40fb5b9a1756ce548e832f8773c3bfc7cf96cca59d77596276bb1f84e2025"
---

# Teaching our AI agent what's actually in your database

Most of what our chat agent does is write SQL against a database it has never seen before. A user asks “which customers churned last quarter”, and the agent has to figure out that churn lives in` analytics.subscription_events` , that the status column is an enum with a` canceled` value, and that the` customers` table with 40M rows is the real one while` customers_tmp` is someone’s forgotten experiment.


For a long time our answer to this was: dump the schema into the prompt and hope. That works great on a 12-table Postgres database. It falls apart on a 304-table warehouse, and we watched it fall apart in a specific, measurable way: the agent started running its own` information_schema` queries against customer warehouses to rediscover what we already knew.


In one benchmark run we counted **140 introspection queries against the customer’s warehouse** . After the changes in this post, that number is 2.


## The prompt-side schema, and its compression ladder


Every agent turn gets a` <database>` block: the connected database’s schema rendered as YAML, with tables, columns, types, foreign keys, and enums. The renderer has a character budget (25,000 by default), because schema context competes with conversation history and tool results for the context window.


Big schemas don’t fit, so we run an ordered pipeline of compression candidates and take the first one that fits the budget. Each step gives up a little fidelity:


1. Full schema, everything included.
2. Truncate enum value lists, then drop rarely-referenced enums.
3. Drop profiling metadata (more on that below).
4. Deduplicate structurally identical schemas and tables. Fivetran-style warehouses love having` staging` ,` dev` , and` prod` schemas with identical shapes, so we render one and note “same structure also applies to: staging, dev”.
5. Truncate tables per schema, keeping foreign-key-important ones.
6. Truncate columns per table, then drop column types, and eventually drop column names entirely.


The detail that matters most in that list is what happens to the tables we cut. We don’t silently drop them. The rendered YAML keeps an index line:


```text
# other tables (details omitted — use describe_tables): invoices, refunds, tax_rates, ...
```


An agent that has never heard of a table will guess a name or go introspect the live database. An agent that has seen the name but not the details will ask for details, which is exactly the behavior we want.


## Where the old setup broke down


On large warehouses the compression ladder bottoms out hard. A 304-table warehouse compressed to fit 25k characters ends up as mostly table names, and the agent has to fill the gaps somehow.


Its only option used to be running SQL against the customer’s warehouse:` information_schema.columns` queries,` SELECT * FROM x LIMIT 5` probes, that kind of thing. This is slow (each one is a network round trip to the customer’s database), and on Snowflake or BigQuery it’s also money, since any query can wake a paused warehouse or bill scanned bytes. It’s a bad feeling to know a customer’s compute woke up because our agent forgot what a column was called.


The fix has two halves: give the agent tools to query the schema metadata we already have, and make that metadata rich enough that it rarely needs anything else.


## Half one: catalog tools


We sync schema metadata into our own Postgres whenever a data source connects or refreshes. That means we can answer “what tables mention subscriptions” without touching the customer’s warehouse at all. Two new tools expose that:


**` search_catalog`** does keyword search over table names, column names, descriptions, and enum values. The ranking took a few iterations to get right. Tokens match at identifier-segment boundaries, so` etl` matches` reverse_etl_sync` because it’s a segment of the name, but` voice` no longer matches` invoice` . Exact name matches rank above prefix matches, which rank above segment matches, which rank above description mentions.


A couple of behaviors we added after watching agents flail:


- When a search fully resolves to 3 or fewer tables, we inline their complete metadata into the search result. The agent was going to call describe next anyway, so we saved it the round trip.
- When nothing matches, the result says so, suggests the closest table names by edit distance, and (this one mattered a lot) tells the agent that data values like customer names or environment strings live in rows, not schemas, so it should switch to` run_query` instead of rephrasing the search 4 more times.
- Column matches are capped at 5 per table, because warehouse tables all share columns like` sync_mode` and one table’s boilerplate would otherwise bury every other result.


**` describe_tables`** returns full metadata for up to 10 tables in one call: every column, type, nullability, foreign keys in both directions, enum values, and the profiling stats below. It also resolves sloppy names. An exact` schema.table` match wins, then a unique unqualified table name, then a unique near-miss within a small Levenshtein distance. Agents guess table names slightly wrong constantly, and each failed lookup used to cost a full model turn. Now` describe_tables("public.usres")` just works and tells the agent what it interpreted.


The tool descriptions actively push toward batching: describe all your candidate tables in one call, in parallel with other tool calls. Tool-call round trips are the latency budget in an agent loop, so the discovery phase should be as wide and as shallow as possible.


## Half two: profiling the data, carefully


Names and types only get you so far. The queries agents actually get wrong are the ones that need to know what’s *in* a column: is` status` a lowercase` active` or an uppercase` ACTIVE` ? Is` deleted_at` null for 95% of rows or 5%? Is this table 300 rows of config or 40M rows of events?


So after each schema sync we run a profiling pass that collects, per table:


- An approximate row count.
- The newest row timestamp, from the best available` updated_at` -like column.
- Per column: sample values for low-cardinality text columns, a null fraction, and a distinct count.


The obvious way to do this is` COUNT(*)` and` SELECT DISTINCT` over everything, which would be a great way to torch a customer’s warehouse bill. Almost all of the design here is about keeping the probes cheap and bounded:


- **Row counts come from engine catalogs** , never` COUNT(*)` . Postgres statistics, Redshift’s` svv_table_info` , and so on. (Redshift serializes its numeric row counts as strings like` "12400.000000"` , which is the kind of thing you only learn by running this against real warehouses.)
- **Engine statistics are read first when they’re free.** Postgres’` pg_stats` already knows null fractions and distinct counts for analyzed tables. If the engine says a column has 40,000 distinct values, we skip the sample probe entirely, since we’d find more than 8 values and store nothing.
- **Sample probes are` SELECT DISTINCT ... LIMIT 9` .** If we get 9 rows back, the column is high-cardinality and we store only a “9+” lower bound. If we get 8 or fewer, we know the list is complete, and we store it with that guarantee.
- **Null fractions come from the first 1,000 rows** , 30 columns per query, not a full-table scan. It’s an estimate and it’s labeled as one.
- **Warehouses that bill per query get metadata-only profiling.** Each dialect’s profiler declares whether data probes are allowed. Snowflake and BigQuery profilers only read catalogs and statistics, so profiling can never wake compute or scan billed bytes.
- **Everything runs inside one global deadline** (60 seconds per sync by default) with a 3-worker pool that profiles smallest tables first. Tables over 10M rows skip sampling, tables over 50M rows skip the freshness probe, and a table profiled in the last 24 hours is skipped entirely, so repeated syncs naturally rotate through whatever the last budget didn’t reach.


There’s also a hard policy layer: columns marked as obscured in Basedash are never sampled, and neither is any column whose name looks like an identifier, email, URL, token, or hash. Sample values exist to teach the agent the shape of categorical data, and identifiers teach it nothing anyway.


One subtle bit we got wrong at first: when the budget expires mid-table, you can’t persist half a column’s results. A column whose distinct-sample probe ran but whose null-fraction probe didn’t would overwrite last week’s good null fraction with an unknown. So a snapshot only includes columns whose probes fully completed, and persistence keeps the previous values for the rest.


## What the agent actually sees


All of this renders back into the same YAML the agent already reads, as comments:


```text
subscription_events  :   # ~2.4M rows, newest 2026-07-20
columns  :
-   id   # uuid, PRIMARY KEY
-   status   # text /* all values: active, canceled, past_due, trialing | null fraction: 0 */
-   canceled_reason   # text /* distinct values: 9+ | null fraction: 0.87 */
-   customer_id   # uuid, FK → customers.id
```


The` all values:` label is a promise: the list was exhaustive at profiling time, so the agent can treat it like an enum and write` WHERE status = 'canceled'` without a scouting query first.` distinct values: 9+` is the opposite signal, don’t bother enumerating this. And the row count plus newest-row date answers “is this table real and current” before the agent commits to it.


Freshness gets a staleness guard: if the profile itself is more than 7 days old, we drop the newest-row date from the render, because at that point it misleads more than it helps. Row counts drift much slower, so those stay.


## Measuring it


We benchmark the chat agent on an internal eval suite (Chat Bench) that runs scenarios against a 304-table Postgres warehouse, with graded answers and full SQL traces. Two numbers from the catalog rollout:


Metric (per full bench run) Before After


Introspection queries on the customer’s DB 140 2


Chat Bench score (GPT-5.4) 47.57% 51.53%


The score delta is directional (the runs were on different days and the suite has real variance), but the introspection number is the one we were after, and it’s not subtle. Latency and cost stayed flat: the catalog tools added tool calls, but each one is a local Postgres read instead of a warehouse round trip, and the fused search results removed about as many calls as the new tools added.


The remaining 2 introspection queries are legitimate, for what it’s worth: dialect-specific edge cases the catalog doesn’t model, which is exactly the fallback role we want live introspection to have.


## What we took away


1. **If your agent is querying a live system to learn things you already know, you’re missing a tool.** We had the full schema in our own database the whole time, and the agent simply had no way to ask for it. No amount of prompt tuning fixes that.
2. **Never silently drop context, leave a pointer.** The single highest-value change in the compression pipeline was keeping omitted table names as an index. An agent can recover from “details omitted, ask for them” but not from “this table doesn’t exist as far as you know”.
3. **Design tools around the failure transcripts.** The near-miss name resolution, the “data values live in rows” redirect, and the fused describe results all came directly from watching agents waste turns in eval traces.
4. **Bound every probe you run against systems you don’t own.** Global deadline, per-query limits, catalog-only mode for billed warehouses, and a recency TTL. The profiler’s job is to be invisible on the customer’s side.
5. **Label the reliability of derived data.** “All values” versus “9+ distinct” versus a rounded null fraction are three different confidence levels, and the agent behaves noticeably better when the rendering makes them distinct.


If you like this genre of work, we’ve written up a few adjacent pieces:[how a timestamp was busting our prompt cache](https://www.basedash.com/blog/how-a-timestamp-was-busting-our-prompt-cache) ,[paginating user-written SQL across every dialect we support](https://www.basedash.com/blog/paginating-user-written-sql-across-every-dialect-we-support) , and[what we learned running background jobs on Postgres](https://www.basedash.com/blog/what-we-learned-running-background-jobs-on-postgres) , which is the queue this profiling job runs on.


And if you want to see the agent that reads all this metadata,[Basedash](https://www.basedash.com/) is an AI-native BI platform that turns questions into charts and dashboards on top of your own database.

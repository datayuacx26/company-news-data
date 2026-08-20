---
schema_version: "1.0.0"
document_id: "d87a3f3464857ed8beebdbcbdaafb7e6aa1e24285f2c299acbebce1695773d19"
company_key: "yc-kaelio"
company: "Kaelio"
source_id: "yc-kaelio-news-import-d43a8938b457"
canonical_url: "https://www.kaelio.com/blog/introducing-ktx-open-source-context-layer-for-data-agents"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T01:10:57.591835+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:5484baa57a71068be5073c8e230ddc7fa909d2070a7eac894aba2612598c11e6"
---

# Introducing ktx: the Open-Source Context Layer That Makes Data Agents Reliable

Today we're open-sourcing[ktx](https://github.com/Kaelio/ktx) , an executable context layer that makes agents reliable on your data stack.


If you've tried pointing Claude Code, Codex or your custom-built agent at your data warehouse, you already know the problem: accuracy is the #1 issue. Agents are great at generating valid SQL, but valid SQL is not always correct SQL. It might quietly be using the wrong joins, filters, or metric logic. In the data world, a number that's slightly wrong is still wrong.


## The current state


Most attempts to solve this fall into two buckets:


The first is to give the agent more context through skills or wiki-style Markdown docs. That gives it some guidance through a single recipe, but still makes it guess as soon as the question needs a variation or a combination.


The second is to create and maintain a semantic layer with high coverage. That solves the executable part, but it's a real pain to build and maintain, since those tools were designed for legacy BI, not for agents. And as a standalone tool, a semantic layer lacks all the useful context that lives in unstructured sources like internal docs, dashboards, query history, and Slack threads.


**ktx** combines the best of both worlds: the breadth of a knowledge base and the SQL safety of a semantic layer, optimized for agents to use and for teams to maintain.


## How ktx works


**ktx** has 2 parts:


1. Business context goes in Markdown wiki pages that are auto-ingested and auto-populated.
2. Queryable definitions go into YAML files that define tables, row grain, joins, measures, dimensions, filters, and filter groups.


Both are plain files in git, so you review them in pull requests like any other code. **ktx** ingests them from sources such as BigQuery, Snowflake, Postgres, dbt, MetricFlow, LookML, Looker, Metabase, and Notion, plus corrections from your analysts during agent sessions.


That way, when an agent needs a metric, it asks **ktx** for a measure, dimensions, and filters instead of writing the whole query itself. **ktx** 's planner chooses the join path, uses grain and relationship metadata, catches issues like join fan-out and chasm joins, and compiles the warehouse SQL, all while using the extra unstructured knowledge it has access to.


So instead of querying raw schema, agents query **ktx** , which translates their requests into accurate SQL against your warehouse. The model never got smarter. It just stopped writing the SQL.


## Try it out


**ktx** is open source under Apache 2.0. It works with Claude Code, Codex, or whatever agent you're using, and you don't need any API keys if you're on a Claude Code Pro or Max plan.


Install it manually:


bash


```text
npm   install   -g   @kaelio/ktx
ktx   setup
```


Or give this prompt to your agent:


```text
Run npx skills add Kaelio/ktx --skill ktx and use ktx skill to install and configure ktx
```


The[quickstart](https://docs.kaelio.com/ktx/docs/getting-started/quickstart) walks through connecting your warehouse and context sources. If you'd like help managing context for your data agents, we run a hosted version too:[ktx Cloud](https://www.kaelio.com/products/ktx-cloud) .


Go to the[GitHub repo](https://github.com/Kaelio/ktx) to find out more.

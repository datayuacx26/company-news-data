---
schema_version: "1.0.0"
document_id: "0305d109a21fd548cc0de3844a63dcb1cbb59684c58b3b52c18f6a9e4d516472"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/agents-dont-read-dashboards/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:dd400abfdbfb8d3ac3a84eeecd28d60536e92e125300de44b966f69bcb1db11e"
---

# Agents Don't Read Dashboards

One of your customers runs an ops agent every Monday morning. Its standing instruction: *flag anything that changed materially in the business last week, and figure out why.*


Some of the data it needs comes from your product. The agent never logs into your app to get it. It looks for the data where it already works: it queries the customer’s data warehouse, joins your usage events against billing and support data, and has the answer before anyone is at their desk.


Whether your product is part of that answer comes down to one thing: is your data in the warehouse when the agent runs?


## Agents work where the data is


There’s nothing wrong with your dashboard. It serves the humans who log into your product, and it should keep doing that. An AI agent is simply a different kind of consumer. It doesn’t look at charts; it queries tables. Give it a schema and it will write the SELECT, get structured results back in seconds, and move on to the next step of its run.


The data warehouse is where that loop works best, for a practical reason: it’s where the rest of the customer’s data lives. The Monday-morning question above isn’t answerable from your product’s data alone. “Figure out why” means joining your events against billing, support, and whatever else the customer keeps. Those joins happen in one place.


And increasingly, the agent itself lives there too. Snowflake Cortex, Databricks Genie, and their peers run inside the customer’s data platform. For a warehouse-native agent, the question “what data can you use?” has a one-line answer: whatever’s been synced in.


## When your data isn’t there


Say the data isn’t synced. The agent still has to answer the question, so it falls back to whatever it has access to.


It might pull from your API. Analytical questions need whole tables, and APIs serve records a page at a time, so the agent paginates until it hits your rate limit. Every page it fetches becomes tokens your customer pays for. If the pull gets cut off, the analysis runs on partial data, and nothing in the output says so.


Or it might drive a browser through your product and read the dashboard. This sometimes works, but it’s slow, it breaks when your UI changes, and it costs the customer far more tokens than a SQL query would have.


Either way, your customer spent real money on an answer that’s late, incomplete, or wrong. Teams building agent workflows notice this quickly. Products whose data is in the warehouse get built into more workflows. Products whose data isn’t get left out, and the customer starts looking at competitors that are more agent-native.


## The unglamorous prerequisite


Avoiding all of this takes one thing: your product’s data has to be in the customer’s data warehouse when the agent asks. Getting it there is what data exports do: continuously, reliably, and as a feature of your product rather than a favor from your solutions team.


That’s a bigger deal than it sounds. Synced into the warehouse, your product becomes a first-class dataset in your customer’s agent stack: joinable, queryable, part of every analysis they run. Your customers get to answer the questions they actually have, with your data in them, without waiting on anyone.


Lookups and actions are a different job: *what’s the status of this invoice, pause that campaign.* For those, agents want your MCP server, and you should ship one. That’s the other half of the same story:[one row on MCP, the whole table on exports](https://www.prequel.co/guides/mcp-vs-data-exports) . We’ve written before about why whole tables shouldn’t flow through a context window:[some questions don’t fit in one](https://www.prequel.co/blog/some-questions-dont-fit-in-a-context-window) .


## The roadmap question


So the question for product teams isn’t “dashboard or data warehouse?” The dashboard keeps its job. The question is where the next analytics investment goes, now that a growing share of your customers’ analysis is run by software that works in the warehouse. We wrote up the full framework, including what to make of embedded-analytics vendors’ AI features, in[Embedded Analytics in the Agent Era](https://www.prequel.co/guides/embedded-analytics-in-the-agent-era) , and what the combined surface looks like on[our agent-readiness page](https://www.prequel.co/ai-agents) .


*Disclosure: Prequel sells data export infrastructure, so we have a side in this argument. But the Monday-morning agent is real, it’s running at your customers already, and it’s worth asking what it finds when it goes looking for your data.*

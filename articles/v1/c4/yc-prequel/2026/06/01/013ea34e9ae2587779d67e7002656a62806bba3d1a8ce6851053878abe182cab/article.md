---
schema_version: "1.0.0"
document_id: "013ea34e9ae2587779d67e7002656a62806bba3d1a8ce6851053878abe182cab"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/some-questions-dont-fit-in-a-context-window/"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:51560b4c4832821af0c387edc37dc86ce10da28e050fa114bcb1dc7dd9d204a5"
---

# Some Questions Don't Fit in a Context Window

If you build software right now, one roadmap question keeps coming up: *what would make our product useful to AI agents?*


The first answer is usually: ship an MCP server. That’s a good answer. MCP is one of the more useful things to happen to product integrations in a long time. For the first time, there’s a standard way for an agent to discover what your product can do, look things up, and take actions without everyone hand-rolling their own connector. If you sell software, you should probably ship one.


Then someone asks where data exports fit, and whether the MCP server replaces them. It doesn’t, for the same reason a production database doesn’t replace a warehouse.


## We’ve seen this split before


Nobody runs analytics against the production Postgres. Not because Postgres is bad, but because it’s built for a different workload. Transactional systems are optimized for point reads and writes: fetch this record, update that row, low latency, one entity at a time. Analytical systems are optimized for the opposite: scan a lot of data, aggregate it, join it, compute over whole tables at once. Databases didn’t split into OLTP and OLAP because one side lost an argument. They split because the workloads are materially different.


Agent data access is starting to split along the same line, one layer up.


MCP is terrific for transactional access. *What’s the status of invoice #4521? Pull up this account’s configuration. Create a ticket. Pause that campaign.* One entity, one answer, one action. A tool call returns a small JSON payload into the agent’s context, and the agent can use it immediately. This is where MCP shines.


But watch what happens when the question gets bigger.


## One row or the whole table


Say an agent gets asked: *“Which of our 50,000 users are at churn risk, and how has that trended over the past two quarters?”*


That’s not a lookup. It’s a full-table scan. Run it through tool calls and the constraints show up quickly. Every page of results lands in the model’s context window as serialized JSON, because that’s how[MCP tool results work](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) . Back-of-envelope:


- A user record with usage fields is a few hundred characters of JSON — call it **~100 tokens** , with the field names repeated on every single row.
- 50,000 rows is roughly **5 million tokens** , which doesn’t fit in any frontier model’s context window.
- So the agent paginates: 50+ tool calls, each page accumulating in the conversation, with the model re-reading what it has already fetched on every step.
- At mid-2026 model prices, that’s **tens to hundreds of dollars and several minutes** for a single question — one a data warehouse answers with one SQL query, in seconds, for fractions of a cent.


Cost still isn’t the worst part: the two paths don’t return the same quality of answer. A database running one SQL aggregate is deterministically right. A language model hand-tallying thousands of rows is approximately right on a good day.


This is not a knock on MCP. It’s a routing problem. An MCP server paging 50,000 rows through a context window is the production database running the quarterly BI workload: the tool is fine; the workload is in the wrong place. There’s good evidence for this pattern already. Anthropic’s engineering team[showed](https://www.anthropic.com/engineering/code-execution-with-mcp) a workflow dropping from 150,000 tokens to 2,000 once intermediate data stopped flowing through the model’s context, and the MCP spec itself[tells servers to rate-limit tool calls](https://modelcontextprotocol.io/specification/2025-06-18/server/tools#security-considerations) . That’s OLTP behavior, which is what MCP is good at.


Meanwhile, the churn-risk question already has a better home. Scanning 50,000 rows, joining usage against billing, aggregating by quarter — this is the exact workload the OLAP side of the house was built for. Columnar storage, vectorized scans, query planners that have been optimized for decades: a modern warehouse treats this as routine. The only prerequisite is that your product’s data is already sitting in the customer’s warehouse when the agent asks. Getting it there — continuously, reliably, as a product feature — is precisely what data exports do.


## Agents will ask a lot more of these questions


That’s why the distinction matters now.


Human analysts ask analytical questions occasionally: when someone builds a dashboard, when a quarterly review comes up, when a metric looks off. Agents ask them constantly: on every workflow trigger, every monitoring loop, every “summarize what changed since yesterday.” Automated traffic[passed human traffic on the web this year](https://www.tomshardware.com/tech-industry/artificial-intelligence/bots-have-now-passed-human-traffic-online-cloudflare-boss-laments-says-agentic-traffic-wasnt-expected-to-eclipse-real-people-until-next-year) , and your customers are wiring agents into more of what they run.


That’s a real opportunity for software vendors. The products agents will be good at using are the ones that give each kind of question the right path: lookups and actions over MCP, whole-table questions answered by one SQL query against the product’s data in the customer’s warehouse. Even the warehouse vendors’ own MCP integrations use that architecture: the agent writes the query, the warehouse does the scan, and the data never passes through a context window.


The piece that makes that work is the unglamorous one: your product’s data, synced continuously into the customer’s warehouse. That’s data exports. Not the legacy alternative to MCP, but the other half of the same agent-readiness story.


## Ship both halves


Here’s how we think about things: **if the question is about one row, use MCP; if it’s about the whole table, use exports.**


So ship the MCP server, and keep it doing what it’s great at: lookups and actions, small payloads, fast answers. Pair it with data exports so the rest of the questions are cheap, fast, and correct. The teams that ship both won’t just have an AI story; they’ll have products that are positioned to do well in the agentic era.


We wrote up the deeper framework — the cost math, the spec details, and a decision matrix for product teams — in our guide:[MCP vs. Data Exports: How AI Agents Should Access Your Product’s Data](https://www.prequel.co/guides/mcp-vs-data-exports) . For the export half, start with[what it means to be warehouse-native](https://www.prequel.co/guides/what-it-means-to-be-warehouse-native) and[which tools to use for data export](https://www.prequel.co/guides/data-export-which-tools-to-use) . And if your product answers analytical questions with an in-product dashboard today, see[Embedded Analytics in the Agent Era](https://www.prequel.co/guides/embedded-analytics-in-the-agent-era) .


*Disclosure: Prequel sells data export infrastructure, so we have a side in this argument. That said, the token math is checkable, and we’d encourage you to run it yourself.*

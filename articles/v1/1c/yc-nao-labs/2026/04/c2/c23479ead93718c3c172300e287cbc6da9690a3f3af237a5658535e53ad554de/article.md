---
schema_version: "1.0.0"
document_id: "c23479ead93718c3c172300e287cbc6da9690a3f3af237a5658535e53ad554de"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/agentic-analytics-meetup-paris/"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:d8c7514f104ff00d63bf22f356397517542c4177fabe5bc31ac3ba29e18a1ab4"
---

# Agentic Analytics Meetup Paris 🇫🇷

Last week, we hosted the first meetup dedicated entirely to agentic analytics. Four data teams - from[Gorgias](https://www.gorgias.com/) ,[Malt](https://www.malt.com/) ,[GetAround](https://getaround.com/) , and[Brigad](https://www.brigad.co/) - came to share what they've actually built, what worked, and what didn't. 45 people responded to our pre-event survey. We analyzed the results live during the demo.


Here's what we took away.


[Watch the full recording →](https://www.youtube.com/watch?v=5IPqQ_TX-YI)


## About nao


This meetup was organized by[nao Labs](https://getnao.io/) , the team behind nao - an open-source analytics agent that gives every employee in your company a way to query your data conversationally. nao is built around a[context layer](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) (a file system you construct and version-control like a dbt repo) and a chat UI you deploy to your business users. You can see the full demo, including a live analysis of the meetup survey results, in the recording above.


We're open source. Leave us a star!


## Agentic Analytics survey recap


Before the meetup, we surveyed 45 data professionals: Analytics Engineers, Data Engineers, Data Analysts, Data Managers, and more - to get a ground-level picture of where the industry stands with analytics agents today. Here's what stood out:


- **Experimentation is the norm, scale is rare.** 21 teams are running a POC and 15 are researching options, but only 5 have fully scaled an agent across their company. On average, agents handle ~22% of data requests today.
- **The tooling space is highly fragmented.** 18 different tools were cited across 45 respondents. The most popular are Claude (29), in-house builds (14), Dust (9), nao (8), Snowflake Cortex (7), and Databricks Genie (7) - with no clear winner beyond Claude as a general-purpose fallback.
- **The core problem is trust, not technology.** The top blockers are measuring agent reliability (18), choosing the right tool (17), and hallucinations (15) - all pointing to the same challenge: getting confident enough to ship agents to production.
- **The semantic layer has a clear majority.** 26 out of 45 respondents say it's[mandatory to build analytics agents](https://getnao.io/blog/semantic-layer-impact-analytics-agent) - a stronger consensus than expected.
- **Data professionals aren't scared.** 78% say AI won't kill their job. The prevailing view: the role evolves toward designing and orchestrating agent workflows, not disappearing.


## Our speakers' analytics agents


### [Anaïs Ghelfi](https://www.linkedin.com/in/ana%C3%AFs-ghelfi-50382397) , Data Platform Director at[Malt](https://www.malt.com/)


**Stack:** BigQuery, dbt Core, Airbyte, Looker *([22:45](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=1365) in the recording)*


Malt started with a hackathon in early 2024. A cross-functional team of four - two data analysts, one data platform engineer, one ML engineer - built a prototype in three days on top of Dust, using a Streamlit app and Claude (an older version). It could answer basic questions like "how many freelancers do we have?" but hallucinated frequently on SQL.


Over the next six months, they tried Looker's Vertex AI integration - a Docker-based app that connects to Gemini, uses Looker's semantic layer as context, and produces charts via Looker Explores. After significant prompt engineering, it worked. But users called it "a gadget." It gave a number, made a graph. No insight, no context. Engagement dropped after a week.


The current version, now in beta with ~100 testers, uses Dust as the orchestration layer, with the full LookML file passed as context and a Looker MCP for chart generation. It lives in Slack. The key addition: a **clarification step** before answering. Users ask vague questions; the agent asks back before running any queries. This dramatically improved accuracy and trust. Accuracy is high because they constrain the agent to their semantic layer.


Next step: open it to the full dataset beyond the semantic layer.


### [Yochan Khoi](https://www.linkedin.com/in/yochankhoi) , Staff Context Engineer at[Gorgias](https://www.gorgias.com/)


**Stack:** BigQuery, dbt, Airbyte (no BI tool) *([27:25](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=1645) in the recording)*


November 2024: two engineers connect BigQuery to Slack and start asking questions. It generates SQL but the link between what users ask and what gets returned is unreliable. The missing piece: a context layer.


They went to Coalesce, where they met the[getdot.ai](https://getdot.ai/) team, and tested their solution for a year - structuring a context layer that could explain metrics, business terminology, and table structures to an agent. At the end of 2024, someone on the team put Claude Code on top of this context layer with full autonomy. Results were surprisingly good - but not scalable.


In December 2025, a team of four rebuilt it properly using LangChain and LangGraph, with a custom context layer and full control over context window compression. They deployed in January.


The result: **84% of the company uses it. 1,000 questions per day. 250 of their 380 employees use it multiple times daily.** Average response time: just over a minute.


Their context layer is a structured knowledge graph: hundreds of instructions, each potentially 600 lines, organized hierarchically. The agent uses **progressive disclosure** : it reads a description first, then decides whether to go deeper. No RAG. Pure structured traversal.


One of their key adoption moves: they didn't push it to users. They gave business people from each department direct access to the context layer, put them on GitHub, taught them to edit it, pointed them at Notion to structure their domain knowledge, then had them open PRs. Those people became the internal champions. They went from 0 to 75% company-wide adoption without a single company-wide announcement.


### [Léon Stefani](https://www.linkedin.com/in/l%C3%A9on-stefani-a8451bb2) , Data Engineer at[Brigad](https://www.brigad.co/)


**Stack:** Databricks, Airbyte, dbt *([31:57](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=1917) in the recording)*


Léon came to agentic analytics almost by accident. He's a solo data engineer at a company serving the hospitality and healthcare staffing market. A product manager on his team had built their own context layer on Notion: every table, every column, every metric definition - and was manually pasting it into Claude to generate SQL.


When nao released its analytics agent, Léon tried it. The jump from "PM copy-pasting Notion into Claude" to a structured context repo with version-controlled metadata made the value obvious. He's now deploying nao and training his product team on how to ask better questions - because getting accurate answers from an agent requires knowing what you're actually asking for.


### [Benjamin Rouif](https://www.linkedin.com/in/brouif) , Head of Data Analytics at[GetAround](https://getaround.com/)


**Stack:** Snowflake, dbt, Tableau, Hex *([34:30](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=2070) in the recording)*


GetAround started by exploring two directions in parallel: AI-assisted coding for the data team, or AI-assisted BI for stakeholders. They picked BI.


First attempt: Snowflake Cortex + Snowflake Intelligence. They built a semantic layer in Snowflake, exposed it in Snowflake Intelligence, and showed it to a small group of business stakeholders. The "wow effect" lasted a week. Then came the 100% churn. Too many hallucinations, no trust.


Second attempt: Hex. Their BI tool of choice, Hex, had been investing in AI from early on - first for helping analysts write code, more recently for agentic BI. They reused their existing semantic layer (integrated between Snowflake and Hex), added a master guide explaining the company, key business metrics, and a set of explicit rules ("you must always do X", "you are not allowed to do Y"), plus domain-specific sub-guides.


One week in, with five testers: high accuracy, good retention, no flagrant errors. The only errors appear in long multi-turn conversations (4-5 interactions deep) and are subtle enough that you need an agent to catch them.


Their first big use case: weekly business review prep. Before, it took a pricing analyst three hours to compile numbers from dashboards. Now it takes thirty minutes, and those thirty minutes are spent on actual analysis, not data retrieval.


## Q&A


### What does agentic analytics even mean?


The table aligned quickly on a working definition: **agentic analytics is making data accessible to everyone in the company the way they'd talk to a data analyst: conversationally, with full context, not just a number.**


Anaïs specifically avoided putting "AI" in the definition. AI is the means, not the goal. The goal is data access.


Benjamin extended it: the next frontier isn't just answering questions, it's taking actions: feeding analysis back into the product, modifying code, triggering downstream workflows. "The day that works, I'll go to bed and the company will run itself."


Yochan added **flexibility** : the ability to access data from anywhere, at any moment, in any format. Agentic analytics breaks the constraint of the dashboard - the output can be a chart, a table, a web app, a Slack message, or a structured report.


### Did the project start from the data team or C-levels?


*([39:30](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=2370) in the recording)*


Unanimous: **bottom-up** . All four teams started without executive sponsorship. They built, showed results, then got buy-in.


Yochan: "It's too conceptually fuzzy to get a budget approved upfront. You have to build something that works and show it."


Benjamin needed senior sponsorship to approve a commercial buy (Hex), but the initial exploration was entirely the data team's initiative.


### How do you increase business users adoption?


*([42:45](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=2565) in the recording)*


Yochan's playbook was the most concrete: start with one department, get one person from that department owning the context layer for their domain, and let them become the internal champion. Gorgias went from zero to 75% company-wide adoption almost entirely through word of mouth from those 15 or so domain owners - no all-hands, no top-down mandate.


Benjamin's version: find your use cases first. At GetAround, the business review meeting was the wedge. One obvious, repeated pain point that the agent solved in a visible way.


### How do you measure accuracy?


*([45:05](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=2705) in the recording)*


Nobody has a perfect answer here. The consensus:


- Offline eval is hard on a non-deterministic model with a small, non-representative test set - it gives false confidence.
- The most reliable signal is **deploying to people who know the data well** and watching whether the answers match their intuition.
- Yochan: autonomize domain experts on the context layer. When they start editing it to fix the agent's mistakes, they're doing your accuracy work for you.
- Benjamin: if there's no accuracy, there's no trust. If there's no trust, there's no adoption. The sequence matters.
- Anaïs: users trained on dashboards (deterministic, 100% reliable by assumption) have a hard time accepting any error rate from an agent. That psychological switch is the hardest part of rollout.
- Léon: show up fast when something breaks. Visibility that the team responds in under 10 minutes makes users far more tolerant of occasional errors and far more likely to report issues.


One approach in progress: LLM-as-judge. After each conversation, a second model scores the interaction and explains the score. Alert when quality drops below a threshold. Iterate on the context layer accordingly.


On our side, Christophe walked through[nao's evaluation framework](https://docs.getnao.io/nao-agent/context-engineering/evaluation) : a way to build a test suite of natural language questions with expected SQL outputs, run them against your agent, and get a reliability score before you deploy to business users. The goal is to give data teams a confidence threshold to work toward, rather than deploying and hoping.


### What does it cost?


*([51:40](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=3100) in the recording)*


At Gorgias: **~$0.60 per question, ~$2.10 per full conversation** on average.


The team built on LangChain/LangGraph specifically to control context window compression and prove to leadership they could optimize costs when needed. That optionality was more important than the actual number.


Context window costs scale with what you put in. The antidote: structured retrieval (fetch descriptions first, go deeper only when needed) rather than flooding the prompt with everything.


Model choice matters enormously. Léon noted a ~100x cost difference between Claude and Mistral in early tests. The right choice depends on quality requirements, latency tolerance, and budget.


### How do you do context engineering?


*([53:50](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=3230) in the recording)*


This was the richest technical thread of the evening. A few principles that came up repeatedly:


**Building the context layer**


Think of it like a dbt repo for business knowledge: not just table schemas, but metric definitions, business terminology, data ownership, and event annotations ("we ran a migration on this date, data quality is degraded in this window"). The more precise and structured this is, the less your agent has to guess. We cover the full framework in[How to build a context stack for agentic analytics](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) .


Yochan's team spent a year on this at Gorgias. Their context layer now has ~180 instruction files, each up to 600 lines, organized as a knowledge graph. Domain experts from each department own their section and open PRs to keep it current. They use **progressive disclosure** : each file has a description the agent reads first, and the agent decides whether to go deeper before loading the full content. No RAG. No vector search. Just structured traversal.


The takeaway: RAG is hard to debug. You can't easily explain why one vector is closer to another. Hierarchical descriptions are auditable - you can follow the agent's path through the graph.


**Maintaining the context layer**


It drifts. Treat it like a living codebase, not a configuration file. A few approaches from the panel:


- Yochan's team runs a scheduled process that reads conversation traces, groups common failure patterns, and opens automated PRs against the context layer. The team reviews and merges.
- The agent writes a self-assessment at the end of each conversation: what went wrong, what feedback did the user give? Those signals feed the improvement loop.
- Observability is the prerequisite. You need full traces before you can improve anything.


See our step-by-step guide on[how to improve analytics agent reliability](https://getnao.io/blog/improve-analytics-agent-reliability-steps) for a concrete implementation of these patterns.


**Optimizing context**


The more you can push reasoning local (loading the right context upfront) versus relying on MCP calls during inference, the faster and cheaper your agent runs. MCP calls add latency at every step - use them at the end of a reasoning chain, not throughout it.


We've benchmarked the accuracy/cost tradeoff across different context configurations: table descriptions only, adding previews, adding profiling, layering routing rules - in our[context impact study](https://getnao.io/blog/context-impact-analytics-agent) . Worth reading before you over-engineer. And if you're deciding whether to invest in a semantic layer first,[this analysis](https://getnao.io/blog/semantic-layer-impact-analytics-agent) covers the data.


### How do you make agents faster?


*([56:50](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=3410) in the recording)*


Reduce reasoning steps, not model speed.


If your semantic layer correctly describes a metric and the agent can find it in two steps: look up description, run query - it's fast. If the agent has to reconstruct what "revenue" means from scratch across a dozen tables, it won't be, regardless of the model.


Routing helps: simple factual questions resolve directly from the semantic layer; deep-dive analysis earns the longer chain. Caching helps for frequently-asked questions. Keep MCP calls to the tail of the reasoning chain.


### What's the impact on data jobs?


*([1:00:50](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=3650) in the recording)*


The framing that resonated most came from Yochan: **you've built a castle with a small door. An analytics agent doesn't remove the castle - it widens the door.**


Suddenly, every department wants to know what's in the data warehouse. Requests don't decrease; they explode. The nature of the work changes. Data analysts stop spending time maintaining dashboards and answering one-off questions. They start doing actual analysis. That's the job they were supposed to have.


The data modeling layer becomes *more* important, not less. If 250 people are querying your data daily, bad models get surfaced fast. Documentation quality, naming conventions, metric definitions: everything that was aspirational hygiene becomes a reliability requirement.


Yochan's title change is instructive: from Analytics Engineer to **Staff Context Engineer** . The scope extended, not replaced. The new work is owning the full chain from raw ingestion to the context layer that agents navigate. The old work (modeling, documentation, testing) didn't go away; it got more consequential.


### Will agentic analytics kill BI?


*([1:10:51](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=4251) in the recording)*


The honest answer: probably not kill, but fundamentally transform.


Benjamin's take: in three years, paying large BI tool licenses just to display dashboards makes less sense when your context layer lives in a repo, agents can answer questions on demand, and generating a chart takes seconds. You still need something to render charts. You don't need $50k/year for that.


Yochan's data point: Gorgias migrated from Periscope to Metabase (embedded in their agent for chart generation) in 10 days using their agent. What used to be a months-long migration project became a weekend sprint.


Léon's framing: BI isn't dying, it's becoming a component. A dashboard is now one output format among many, not the primary interface to data.


### What about security and permissions?


*([1:15:40](https://www.youtube.com/watch?v=5IPqQ_TX-YI&t=4540) in the recording)*


Current honest state: most teams aren't handling row-level security yet. They're scoping the agent to metrics and aggregate data that's company-wide accessible.


The gap everyone flagged: impersonation and row-level security across warehouses, BI tools, and LLM pipelines is genuinely unsolved at scale. Yochan: "No one has cracked good impersonation." Data with PII should be excluded from what you send to external LLMs - zero data retention contracts help, but contractual guarantees of not sending PII are stronger.


As agents expand from semantic layers to full data warehouses, this becomes a first-class problem.


## Next events


This was Paris. Next up: **London and Berlin** .


We're planning to bring the Agentic Analytics meetup series to other cities in the coming months. If you're based there and want to co-organize - or if you're somewhere else entirely and think your city is ready for this conversation -[reach out](https://getnao.io/contact) .


In the meantime, if you want to follow along or contribute:[star the nao repo on GitHub](https://github.com/getnao/nao) . We're open source, almost at 1K stars, and every star helps us keep building.

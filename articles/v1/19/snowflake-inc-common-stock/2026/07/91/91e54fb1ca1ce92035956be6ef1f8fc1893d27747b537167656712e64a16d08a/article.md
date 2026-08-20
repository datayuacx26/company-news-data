---
schema_version: "1.0.0"
document_id: "91e54fb1ca1ce92035956be6ef1f8fc1893d27747b537167656712e64a16d08a"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/observability-at-scale-whatnot-snowflake-summit"
published_at: "2026-07-27T16:30:00+00:00"
first_seen_at: "2026-07-27T18:49:15.081564+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:c12711fda585a5d44b319028e0750ed1f7fe6820d39482bc483803feeacf54e9"
---

# Observability at Scale: Whatnot at Snowflake Summit

Have you ever wondered what is *actually* happening inside a massive data platform at any given second?


For many companies, data infrastructure operates like a black box. Millions of data points go in, complex queries run and reports come out. But when things slow down, costs spike or a critical dashboard goes blank, finding the root cause can feel like floundering in the dark.


To shed light on this challenge, the live-shopping platform[Whatnot](https://www.whatnot.com/?srsltid=AfmBOopUWw3FqUqWP6zSjpnGqcIzDQ94_MH9Ek4XaMkxM_i89-_icEol) joined Snowflake on stage at[Snowflake Summit 2026](https://reg.snowflake.com/flow/snowflake/summit26/sessions/page/catalog/session/1768514412651001NaiB) . They outlined a new blueprint for the modern enterprise: one that combines a legendary hyper-growth story with automated AI analysts and crystal-clear platform monitoring. This partnership demonstrates how modern data tools keep customer experiences smooth, reliable and completely visible, even under massive, real-time demand.


## The reality of hyper-growth: billions of events, zero room for error


Whatnot has captured international momentum as one of the fastest-growing marketplaces ever, pacing ahead of the historic trajectories of legacy ecommerce giants such as eBay and Amazon within its first few years. Today, it stands as the premier live-shopping platform across North America, the UK, Australia and Europe.


These metrics underscore the sheer scale of their operation and the immense volume handled by their data infrastructure:


-


**$8 billion generated** in live global gross merchandise volume (GMV) in 2025


-


**20M+ new accounts** added across all markets in 2025


-


**A 285% year-over-year increase** in first-time buyers in 2025


-


**Over 550,000 hours of livestreams** hosted every single week — with active viewers averaging **95+ minutes a day** on the app


-


**Hosted the largest live shopping stream in U.S. history** in 2026 that had **583,000 concurrent viewers** and **555,000 users** entering the same giveaway at the same time


Behind the scenes, auction bids, chat messages and transactions generate billions of data points daily. All that data converges inside Snowflake to power the immediate user experience.


"Data isn't just for historical reports at Whatnot — it drives the live app experience," explains Alice Leach, Engineering Manager at Whatnot. "If a customer buys an item on a livestream, our machine learning algorithms need to recommend related products within minutes, not days. If we experience a data delay, it directly impacts our buyers and sellers."


Originally, Whatnot managed all this information through a single, centralized data team using dbt. But as the company exploded, this setup became a major bottleneck. To fix it, Whatnot shifted to a modular data stack. Using infrastructure as code (IaC), individual business units (such as fraud prevention or vendor analytics) were given the independence to spin up their own dedicated Snowflake warehouses on demand and manage their own pipelines.


Decentralization cleared organizational blockages, but it created a new puzzle: How do you give teams complete freedom to run their own data pipelines while maintaining total visibility, cost control and performance quality across the entire company?


## The AI solution: moving from data requests to conversational analytics


While decentralizing the infrastructure helped the engineering side, it highlighted a human bottleneck: data scientists. As business leaders rushed to make fast, day-to-day decisions, data scientists became trapped in an endless loop of answering ad hoc data questions over Slack.


To move "uncomfortably fast," Whatnot realized it needed to lower the barrier to entry so anyone could access data at the speed of typing.


### The evolution of the virtual analyst


Whatnot’s journey to scale analytics evolved through three phases:


1.


**2024 (the rigid Slackbot):** Whatnot built an AI Slack bot (@databot) to auto-generate SQL queries. While it could handle simple requests, it required heavy maintenance and continuous human verification.


2.


**2025 (decentralized tools):** The company integrated[Snowflake semantic views](https://www.snowflake.com/en/blog/engineering/native-semantic-views-ai-bi/) across multipurpose apps such as Sigma and Glean. By pairing Snowflake with a leading, advanced LLM, text-to-SQL accuracy crossed 90% in Whatnot’s internal testing, but the ecosystem lacked a cohesive "front door."


3.


**2026 (the agentic analytics era):** Whatnot rolled out Hex Threads — a custom data companion powered by[Snowflake Cortex Agents](https://www.snowflake.com/en/developers/guides/getting-started-with-cortex-agents/) . Instead of forcing users to know exactly where a database table lives or how to debug a SQL error, the AI safely scans the data network to act as a conversational assistant.


### The business impact


The transition from clunky, manual data pulls to agentic AI completely transformed the company's internal culture:


-


**Widespread adoption:** Within 90 days of launch, **over 80% of Whatnot’s 1,000+ employees** were actively using the agentic solution.


-


**Universal access:** 17 different company departments reached **100% active utilization** . Teams such as performance marketing, talent acquisition and business operations became entirely self-sufficient.


-


**Deeper strategic analysis:** Rather than just pulling static lists, teams are using conversational AI for complex work — such as tracking international weekly trends, matching messy data fields and building seller churn prediction models.


-


**Rapid iteration on business and product analysis:** What previously required ad hoc data requests planned weeks in advance has been compressed into data available at the speed of typing. This drastically reduces technical friction, allowing product teams to run SQL execution checks, test feed-generation logic and rapidly iterate on predictive models (such as seller churn) without waiting on engineering queues.


**The Data Whatnot Employees Are Asking Agents to Retrieve**


**Business topic**


**Share of total queries**


Seller data


16.0%


Livestreams


11.9%


Orders


11.4%


Data engineering


8.2%


Whatnot is currently bringing this exact capability to the public. Through the Whatnot Seller Hub, livestreamers operate as independent business executives. By embedding Cortex Agents with strict row-level security (designed to ensure data remains private and protected), sellers can naturally text the app for instant updates, such as: " *Show me my top buyers in the last 30 days* ."


## Technical infrastructure: fast, affordable and democratic monitoring


Giving hundreds of internal employees and thousands of external sellers the freedom to query data and spin up warehouses introduces a unique operational challenge: managing the risk of runaway costs and unexpected platform errors.


To help balance the rapid speed of decentralized AI, Whatnot's ongoing goal has been to cultivate a monitoring setup designed to be fast, cost-effective and easy to read.


### Breaking the logging bottleneck


Historically, tracking platform health in real time was a massive headache. Standard utilization logs were delayed by three to four hours — far too slow to catch a live pipeline error. The alternative was running massive diagnostic scans every 15 minutes, which burned through the compute budget and required strict admin security clearances just to view the logs.


To fix this, Snowflake overhauled its native telemetry engine using next-generation[event tables](https://docs.snowflake.com/en/developer-guide/logging-tracing/event-table-setting-up) via[Snowflake Trail](https://www.snowflake.com/en/product/features/snowflake-trail/) . This update made event ingestion 10x faster, removing the financial stress of comprehensive logging.


### Moving from complex code to natural language alerts


Setting up high-quality system alerts used to require data engineers to write over a hundred lines of intricate SQL code. This technical barrier effectively locked business analysts and operational managers out of the loop.


Now, using Snowflake's AI-assisted observability workflows, users can stand up automated infrastructure monitoring simply by typing a conversational request into CoCo, in[Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight) UI, such as:


" *Create an alert that detects when our warehouses experience performance anomalies or sudden cost spikes, and email me the summary daily* ."


Behind the scenes, the built-in AI is designed to evaluate the user's business intent, scan the relevant platform views, build the underlying code logic and provision the notification channel automatically.


## The road ahead: epistemic hygiene and proactive data operations


As Whatnot looks toward the future, the ultimate goal is to transition from diagnosing past problems to actively preventing them.


However, scaling AI analytics has taught the company an important lesson: Dropping technical friction inevitably exposes organizational friction. When data moves at the speed of typing, gaps in data modeling, ownership and metric definitions become very obvious very quickly. Furthermore, decentralized AI systems can suffer from "agent sprawl" if left completely unguided.


To keep its data trustworthy, Whatnot enforces a strict playbook of epistemic hygiene and agent guidance across its AI network:


-


**Probabilistic language** : Agents must use cautious, probabilistic language by default (" *this likely reflects* ," " *the data is consistent with* "), and are explicitly banned from using definitive phrases such as " *this proves* " or " *this clearly shows* ."


-


**Observation vs. interpretation** : The AI must explicitly separate raw data facts from subjective business interpretations.


-


**No blind causation** : Agents are strictly prohibited from declaring causation from simple observational data, defaulting to terms such as " *associated with* " or " *correlates with* ."


By combining these rigorous structural guardrails with Snowflake’s upcoming features — such as Unified Observability Hubs for centralized cost tracking and AI-driven smart alert recommendations — the line between software engineers, data scientists and business managers is intentionally blurring.


By embracing modular data stacks, affordable real-time event logs and conversational AI, companies like Whatnot are turning the infrastructure black box into a major competitive advantage — ensuring data teams spend less time writing custom logging scripts, and live shoppers enjoy a flawless, real-time experience.


*All statistics are provided by Whatnot internal analytics, as of July 27, 2026.*


***Forward Looking Statements***


*This article contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.*

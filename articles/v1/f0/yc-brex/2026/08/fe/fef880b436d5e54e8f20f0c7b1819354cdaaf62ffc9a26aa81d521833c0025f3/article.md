---
schema_version: "1.0.0"
document_id: "fef880b436d5e54e8f20f0c7b1819354cdaaf62ffc9a26aa81d521833c0025f3"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/brex-benchmark-articles/top-25-fastest-growing-startups-of-summer-2026"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-16T12:21:42.567657+00:00"
fetched_at: "2026-08-17T00:00:01.700571+00:00"
content_hash: "sha256:2b8db8233b72258e70352a231439006a42bb3fc6be2b90099f9c15afbc77f15b"
---

# Brex Benchmark: Top 25 Fastest Growing Startups of Summer 2026

Fourteen of the 25 fastest-growing software vendors on Brex this summer — more than half the list — sell infrastructure for building AI products, not AI products themselves. Six more are AI products built on those stacks.


Behind #1 Together AI, which rents access to open models, sit the databases, GPUs, caches, and sandboxes that agent products get built on. The last time a Benchmark list looked like this, the products on top were mobile apps and the infrastructure was AWS. The fastest-growing line items on startup card statements are the plumbing of the agent economy, and how startups buy that plumbing shows what they're building.


The Summer Benchmark goes deep on the two categories driving the list, AI compute and databases, whose purchase patterns answer questions nobody outside the vendors can measure. When do startups start running open models? Does open compute replace Big Lab bills or grow alongside them? And what happens in the 90 days after a founder prompts an app into existence?


## The Top 25 Fastest Growing Startups (Summer 2026)


These rankings capture growth acceleration and are the companies pulling away right now, not ones already large. Vendors that are publicly traded, valued above $30B, or past $1B in ARR graduate off the list, along with their subsidiaries. The data is real[Brex card](https://www.brex.com/product/credit-card) and[Brex bill pay](https://www.brex.com/product/bill-pay) activity from tens of thousands of Brex customers, recency-weighted.


The highlighted in the ranking below marks all fourteen as the building blocks for shipping AI products.


*Ranked by momentum, scaling velocity, and dollar-growth acceleration across 35,000+ Brex customers' card and bill pay activity, recency-weighted. Jan-Jul 2026. Source: Brex data*


## Startups go direct for compute in their first year


Everything required to turn those files into a working product is not, and a whole category of vendors now sells that work pre-assembled.


### What is Together AI actually selling?


An open-weight model is free to download: tens to hundreds of gigabytes of numbers that do nothing on their own. Turning them into a product takes GPUs big enough to hold them, an inference engine like vLLM, and a serving layer that batches web requests into efficient GPU work. A whole category now sells that work pre-assembled. GPU clouds rent the raw machines. Serverless platforms like Modal and RunPod add containers, autoscaling, and per-second billing. Then a the top, Together AI, Deepinfra, Baseten, and Fireworks serve popular open models behind compatible endpoints. Change one line of config and the model becomes DeepSeek or Kimi, at whatever price the hosts are currently competing down to. The newest rung, Thinking Machines Lab's Tinker API, sells managed fine-tuning, where you shape an open model to your data without touching a GPU.


*Every rung sells rented inference with more or less of the stack pre-assembled, from metered API pennies to reserved-machine rent. On Brex the metered layers have by far the most buyers; machine tickets are far larger and far fewer. Vendors shown are summer 2026 Benchmark leaders.Trend 3: A healthcare-interoperability newsletter cracked the top 15.*


### Why rent open compute when you already pay for tokens?


An H100 rents for around $3.40 an hour, down from roughly $8 in 2023, and batched well it produces tokens from mid-sized open models for pennies per million. Closed frontier APIs list at up to $5 per million input tokens and $25 to $30 per million output. For the workloads that dominate a token bill (tagging and routing data, extracting structure from documents, speech-to-text, image and video generation), an open model is usually good enough, and the gap runs 5x to 20x. A whole merchant category grew up to arbitrage GPU-hours against token prices. Scope note: card and bill-pay data sees the self-serve layer, not invoiced enterprise GPU contracts. That's a census of what startups do, and startups are where new stacks get chosen.


### New Startups are moving first to Open Source Models on Compute Platforms


After building a PMF AI app with a Big Lab API, the costs can balloon as usage scales. Moving to an OSM hosted in the cloud can cut costs 80% overnight. Companies are adding their first open-compute vendor within 5 months for their first Big Lab API bill, twice as fast as in 2024.


*Median months from a company's first OpenAl/Anthropic API charge to its first charge at an open-model compute vendor (Together Al, Modal, RunPod, Fal, Groq, Deepinfra, Baseten and peers), by year of first LLM API charge. *2025 cohort has a shorter observation window; its fastest quartile graduated in ~2 months. Brex card + hill pay data, Jan 2021 - Jun 2026.*


*Startups route each workload to the cheapest model that clears its quality bar. Token prices are public list prices, July 2026. Overlap and after-adoption figures: Brex card + bill pay data.*


## Trend 2: The database customer is no longer human


Measured within each company's first 12 months on Brex, Supabase adoption grew roughly 17x between the 2022 and 2025 startup cohorts; nearly 1 in 10 companies that joined in 2025 paid for it in their first year. Vendor telemetry explains the new default: Neon, a Postgres company (part of Databricks, ineligible here), reports that AI agents, not people, create more than 80% of its new databases, and Supabase independently reports more than 60% launched by AI tools. The majority customer of a database company is now software.


### Why do agents need a different kind of database?


A classic cloud database couples compute and storage on one machine: minutes to provision, billed while idle, nerve-wracking to copy. The new generation (Neon is the cleanest example) splits it into disposable compute over durable storage built on a replicated log. A new database becomes a seconds-long metadata operation, an idle one suspends to near zero, and a copy is a pointer, not a transfer.


*Splitting Postgres into disposable compute and durable log-structured storage is what makes instant provisioning, scale-to-zero pricing, and copy-on-write branching possible - the three properties Al agents require. Neon is the cleanest example of the architecture; Supabase, Railway, and peers apply variants of it.*


None of it was designed for agents; it fits them perfectly. Agents work at machine speed, so a minutes-long provision is a blocking failure. Agents are prolific: a coding agent or app builder like Lovable, Bolt, or Replit provisions a database for each app it generates, thousands a day, most of which sit idle. Near-zero idle cost makes that pattern economically boring instead of absurd, and it's already the norm: Replit's agent runs its backend on Neon, and Retool manages more than 300,000 Postgres instances there with a single engineer. And because the training corpus is saturated with Postgres, LLMs speak it fluently, turning the category's decade-old Postgres bet into an advantage nobody planned.


###
What the buying data shows


Our data shows what happens when this machinery meets startups:


- The default flipped. Among first-ever database purchases, Supabase's share went from about 15% in 2023 to 38% in 2026, the most common first pick. Pinecone, the RAG wave's default entry point, went from 22% to under 2% over the same window. Vector search didn't die; it moved inside Postgres. (We can't see inside consolidated AWS bills; this measures which brands startups pick when choosing a database on its own merits.)
- **The compounding isn't one vendor.** The 17x cohort curve that opened this section repeats down the category: Neon's first-year adoption grew about 60x from a tiny base over the same period, and PlanetScale's roughly 5x.
- The vibe-code funnel is real. Among companies whose first prompt-to-app-builder purchase (Lovable, Replit, or Bolt) came in 2025 or 2026, 9.6% added a paid database within 90 days (more than an order of magnitude above the base rate), and 8.2% chose Supabase or Neon. Among companies that add one, the median gap from first app-builder charge to first Supabase or Neon charge is 106 days: just over a quarter from "prompted an app into existence" to "paying for a production backend." App-builder adopters attach databases at a higher rate than users of professional AI coding tools: net-new demand, and a conversion no single vendor can see.


*Share of companies' first-ever database-category purchase captured by each vendor, by year of that first purchase. Category spans 20+ modern database and vector database vendors. *2026 is Jan-Jun. Brex card + bill pay data, merchant history to 2021. Source: Brex data*


*Share of active-spending Brex customers that paid each vendor within 12 months of their first Brex spend, by year of first spend. Cohorts restricted to companies with a full 12-month observation window. Brex card + bill pay data, 2021-2026. Source: Brex data*


Above the entry point sits PlanetScale (#24, the only Postgres company on this summer's list), which brought its locally attached NVMe architecture, Metal, to Postgres last year for the latency-critical case where an agent chains hundreds of queries. It barely shows up in first-year adoption by design: it dropped its free tier years ago and leaves free provisioning to Supabase and Neon. Measurable performance-tier alternatives are flat or shrinking among startups; the rest hides inside consolidated hyperscaler bills, where card data can't see it.


Entry-tier bills are flat or falling: the median is tens of dollars a month (Supabase's Pro plan is $25; Neon meters usage with no minimum), and Railway's median bill halved even as its paying customers grew nearly 8x. The revenue engine is count, not contract size: millions of tiny databases created by software, priced like a utility, growing with every app an agent generates. The database industry spent thirty years selling to database administrators. The companies winning the entry point in 2026 sell to other people's software.


## The infrastructure layer is where the agent race gets funded


Spring's trend was abstraction: value accruing to whoever sits in front of a fragmenting market. Summer's trend is what those abstractions run on. Every voice agent, coding agent, and generative media product on this list resolves into the same two purchases, compute to run models and databases to hold state, and startups now make both in their first year, at small dollar amounts, by the tens of thousands. Those two purchases are the earliest reliable signal for which agent-era companies are becoming real businesses.


## Methodology and privacy


*Sumeet Marwaha is the Head of Data at Brex, supporting Brex in understanding how customers spend, adopt tools, and grow their businesses.*


*All analysis conducted for this report that uses Brex internal customer data is anonymized and aggregated for privacy. To learn more about how we use data in anonymized or aggregated form for these trend reports, email us at privacy@brex.com.*

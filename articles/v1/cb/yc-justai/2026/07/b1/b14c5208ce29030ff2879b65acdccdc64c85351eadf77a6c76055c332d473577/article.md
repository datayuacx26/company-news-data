---
schema_version: "1.0.0"
document_id: "b14c5208ce29030ff2879b65acdccdc64c85351eadf77a6c76055c332d473577"
company_key: "yc-justai"
company: "JustAI"
source_id: "yc-justai-news-import-e4de3da632ca"
canonical_url: "https://www.getjust.ai/blog/from-great-ctrs-to-real-results-justai-evolution-with-thumbtack"
published_at: null
first_seen_at: "2026-07-25T10:24:54.291702+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:2ad2716beb96e2b1557b523e10eb32e92e9962b5b09eed236e0bbc1cfbbd2021"
---

# JustAI’s evolution with Thumbtack

Most case studies airbrush the story. This one doesn’t. It’s about the messy middle, the aha’s, and how a high-bar partner helped us turn early engagement pops into durable business impact.


#### The goal that made everything harder (and better)


Thumbtack kept raising the bar (and we delightfully took the challenge each time because what better way to improve our product). We began with a simple hypothesis: **creative optimization in Push should move statsig CTR** . It did. In the first month we shipped fast:


We saw early engagement wins ( **up to +80% CTR** and **+35% OR** ). And then came the right question:


**“Does this translate to requests and revenue?”** That reframed the work, and the product.


They pushed us to move beyond vanity metrics; they wanted to see if this engagement lift can translate into requests (and well real revenue).


Beyond metrics, the team wanted learnings: which segments actually matter, where personalization pays off, and whether AI can improve over time against human-written creative.


#### The messy middle: regret, learning windows, and “not enough data yet”


**Where we stumbled:**


-


**Regret from weak tests:** Early on, we solely incentivized running as many optimizations as possible. This means we explored many creative variants, sometimes creating more regret than wins (at least in the short term). This diluted wins for our overall Just Words arm.


-


**Single-day/seasonal sends:** Beautiful for volume, terrible for learning - too little time for the bandit to call a request-rate winner. Even though we could track for open and click rates, we couldn’t spotlight winners for long term requests.


-


**Measurement foundations:** We decided to do a look back instead of a forward projection of wins. This means we created a 50-50% holdout, to assess if JW arm can be beat control in a few months. This made it difficult to forecast impact of wins in the future, and we struggled to find a balance between the trade of always-on testing vs shipping wins.


-


**Engagement ≠ requests.** Some variants drove gorgeous CTRs but fizzled post-click. This meant we had to both, observe ‘requests’ but also wait for a while to optimize on the lagging indicator. Interestingly, more often than not, requests would not correlate with proxy metrics like clicks, making it even harder to find winners.


-


**Is personalization working for Thumbtack?** We tried creative variants across users, but didn’t quite answer which user segments matter when it comes to personalizing content and how much.


**What we changed:**


-


**Direct KPI optimization:** We ingested **custom metrics (sessions, requests)** and pointed the bandit there—so decisions are made on **requests** , not proxies.


-


**Segment signal hunt:** We did a post analysis to determine which segments make a difference.


Eg: Recency was a bigger feature (and divergence in creative would significantly impact results) vs Customer stage & Ownership


-


Signals got sharper, too: homeowners leaned into project-focused prompts; apartment dwellers responded to open-ended “spaces.” Personalize where it matters; stay generic where it doesn’t.


-


We expanded to contextual bandits to evaluate behaviors across many segments - intent, home type, recency and more.


#### The turn: requests start to move


Once we optimized to the right metric, and gave it time, request lifts showed up:


-


Request rate for post project reminders up **+1.4%** .


-


Churned users’ request rate up **+3.75%** .


-


After the pivot, ~20% of tests produced request-rate winners at a unique user level. > 90% of tests showed either session or request lift assuming sessions/sends or requests/sends as the core metric


-


In aggregate, the JW arm caught up on statsig click rates and trended up on unique-user request rate, but didn’t hit statsig within a month, most probably due to early exploration dilution and **metric definition differences** (requests/sends vs requests/users).


#### Overall results


**Win rate across all optimizations (rates calculated over sends)**


Overall win rate (at least sessions or requests are statsig overall or for 1 variant)


**92%**


Requests win rate (statsig overall or 1+ variants)


**33.33%**


Sessions win rate (statsig overall or 1+variants)


**83.33%**


Upward trending rate


**100.00%**


-


🚀 **102 variants** tested across **Email and Push across 18 campaigns**


-


🤖 **193 AI-driven decisions** to optimize messaging, Personalized by **tiers**


-


📩 **3.09M messages** powered by Just Words AI


-


**🎯 Optimized on Requests, Sessions and Engagement**


-


**📊 Personalized by Intent, Home Type, City, Request Type, & Lifecycle Stage**


**✨ Top Results**


-


📈 **$366K lift in revenue** through optimization on **Re-engagement pushes**


-


📊 **$108K lift in revenue** on **Churn Push Optimization**


-


🎯 **Up to +80% lift in Click Rates & +35% lift in Open Rates**


#### What we’re building next


-


**Impact-first measurement:** A built-in **1% vs 1%** control/JW **impact holdout** for clean readouts, while the remaining **98%** runs **MAB** optimizations at full speed. With the goal of giving companies a clear impact readout as well as incentivizing rapid testing, we’re building a holdout structure within Just Words where we will cleanly measure impact through one holdout while the other one for MAB optimizations.


-


**KPI window estimator:** An evaluation calculator to predict how long a KPI will take to reach statsig based on variance and baseline, so teams plan tests with realistic horizons.


-


**Definition alignment:** Processes in place to keep metric definitions reconciled across tools (eg: requests/sends vs requests/users).


#### What actually worked (so you can steal it)


-


**Optimize to the outcome you’re paid for:** Use engagement to accelerate learning, but let **KPIs** decide allocation. Do this early, and upfront.


-


**Shrink regret:** Archive fast, replace fast, keep a small holdout.


-


**Personalize where it pays:** Prioritize segments with **proven behavioral separation** (recency, home type, intent).


-


**Measure two ways:** One split for **bandit optimization** , another **impact holdout** for a clean lift read.


#### Kudos to the AI forward Growth team at Thumbtack!


Under[Chris Acton-Maher's](https://www.linkedin.com/in/chris-acton-maher-58411a3/) product leadership, and with a sharp, curious Marketing team led by[Stephanie Wang](https://www.linkedin.com/in/stephwang/) ,[Joshua Mack](https://www.linkedin.com/in/joshuammack/) ,[Alex Goldstein](https://www.linkedin.com/in/alex-goldstein-938167b5/) , Thumbtack pushed us to level up: 100+ creatives tested in a few weeks, smarter personalization, and measurable lifts tied to real requests. The partnership didn’t just produce wins; it evolved the product.

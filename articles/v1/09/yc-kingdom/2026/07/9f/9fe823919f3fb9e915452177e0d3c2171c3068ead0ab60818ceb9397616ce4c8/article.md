---
schema_version: "1.0.0"
document_id: "9fe823919f3fb9e915452177e0d3c2171c3068ead0ab60818ceb9397616ce4c8"
company_key: "yc-kingdom"
company: "Kingdom"
source_id: "yc-kingdom-news-import-0f99577d1401"
canonical_url: "https://www.kingdomsuperculture.com/blog/inside-kingdoms-ai-operating-system"
published_at: "2026-07-13T14:00:00+00:00"
first_seen_at: "2026-07-25T10:51:22.470586+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:2ae8479707587a55eaf1b32180ea1137e72d8acdb1e121a894ad65bed2d91b10"
---

# ‍Inside Kingdom’s AI Operating System

Most Kingdom team members have a small glass jar on their desk. Inside live a few Opae ula ( *Halocaridina rubra* ): small shrimp that can (almost miraculously) live in a closed ecosystem with no intervention.1


They are a fitting mascot for our team. We have spent years learning how to harness natural microbes to make a measurable impact on the health of pets and people. These shrimp, sustaining an entire microcosm for decades, are a daily reminder of the outsized power of biology.


At Kingdom, we're building the world's first AI-native ingredients company, and LLM tools are being woven into every step of how we develop and deliver incredible ingredients. When we began to build our own AI operating system, we looked to these shrimp as the inspiration: ensuring the right conditions, connecting the system, and letting processes compound.


We call the system Colony, and the agent inside of it Shrimpy. It lets any team member (technical or commercial) access and harness our wealth of knowledge, and turn a simple idea into a working tool in minutes.


To be clear, the point is not just using AI for its own sake. Using these tools throughout our processes allows us to (i) make better ingredients and (ii) give our partners stronger support, faster.


Here are a few things building Colony has taught us so far.


‍ **The bottleneck isn’t ideas. It's turning messy context into something that works.**


Most teams (including ours) don’t have a shortage of good ideas. The real problem is taking all of the complex business logic, distilling it into a clear system that works, fixing the bugs and quirks, and making sure it continues to work.


Identifying the right generic tool that can’t adapt, or relying on spreadsheets that have amassed overwhelming complexity, is where many useful ideas quietly die.


Colony helps to collapse that distance. The person closest to a problem can use it to refine and scope a solution, prototype it, test it with the team, and put it into daily use. This happens now in minutes instead of weeks.


Our first win was a simple but meaningful tool. We replaced a paid contract management platform with an internal contract management system that was orders of magnitude cheaper, worked far better with LLM-driven field extraction, and better matched exactly how our team actually works: simply forward a signed contract to Colony and the rest is done and tracked automatically.


The most important part was not the tool itself, but proving that the path from “this off-the-shelf tool is not great” to “we can build something that works perfectly for us” is readily possible.


This compounds over time. Colony does not just serve the team; it improves with the team. Every new workflow can plug into our shared infrastructure that strengthens our entire value chain, across scientific ingredient discovery, clinical validation, supply chain, and partner support.


The more we use it, the more useful it becomes.


‍ **One centralized system or brain, not a pile of tools.**


The initial version of “AI at a company” is a scattered pile of disparate solutions. Each tool can solve a narrow problem, but cannot connect cleanly to all of the others.


We wanted to build something different.


Colony connects to all of our key systems: Notion, Google Drive, Attio (our CRM), Slack, supply chain, quality data, scientific data infrastructure, and much more. It’s one company brain that both our team and agents can use.


*Shrimpy, the agent within Colony*


A couple examples make this more concrete:


- In our supply chain, we capture each lot’s Certificate of Analysis, extract out key result values (of course with a human verification step), compare each result against the relevant specification, and return a clear quality assessment as soon as the results are available. This previously required transcribing each result manually from long PDFs into a massive spreadsheet, and building complex formulas that broke with scale.


- In partner support, when a partner asks about a shipment, an agent can trace the partner and order to the exact lot shipped, then pull the associated quality record in seconds instead of digging through spreadsheets.


Because these workflows run on the same underlying system, a capability built for one use case makes the next one easier. One improvement or captured data stream can benefit the entire team, rather than being trapped within a single tool or automation.


**Every employee is a builder. And so is every agent.**


I initially asked a few team members to request features and tools that could be scoped and built inside Colony.


What surprised me was that the agents also began surfacing their own gaps.


As the agents run, they have access to the entire codebase of Colony, and have the ability to surface feature requests in Linear (where we triage and build out code for the system). For example, a team member created a scheduled workflow that scans our experimental lab notebooks for key cross-functional results in the previous week. Colony can build the workflow and return it for review and scheduled execution. But as the agent runs, it can identify missing capabilities and file improvement requests of its own.


We’ve logged and built out hundreds of feature requests, many of which have been surfaced by the agents themselves. A small but meaningful example: an agent lacked the ability to query experiments by date ranges (and instead took this on in a token-hungry manual manner). It filed a feature request, and built out the API endpoint to do this with code more efficiently in the future.


The volume that we’ve built out is pretty wild in my opinion (the code base is now well over 100k lines of code), and only possible with the recent rapid advancement of LLMs.


‍ **The hard part isn’t building. It’s knowing what to build.**


This was the biggest surprise.


We initially assumed building out this sort of system would be well outside of our means as a small startup. Could current LLM models actually build out production-grade features and workflows that stand the test of daily use?


They largely can, and at this point the vast majority of our development (scoping feature requests, implementing code, QA/QCing database schema and data ingestion, testing frontend UIs) is automated by LLM and coding tools. But a capable tool is not the same thing as a useful system for our business.


We’ve realized that the harder work happens upstream of all of this: defining the business logic, deciding which problems are worth solving, and documenting the judgment that oftentimes lives only in one person’s head.


What is the best format and cadence for reminders on contract expiration? What does strong partner support look like? How should we prioritize between multiple conflicting sources of information?


Getting that context onto paper, clearly enough that both people and agents can use it, is now the core part of the work.


It’s important to note that Colony does not solve or answer everything. There are a vast number of decisions and work behind every Superculture® ingredient that require deep, thoughtful, and careful human judgement.


The point is that Colony can improve the speed and depth of that work.


**An ecosystem, self-maintaining**


Opae ula do not need much. In the right conditions, they can sustain a complex ecosystem that can recycle what it has and keep on going with minimal intervention.


That is the kind of company we are trying to build at Kingdom: a small but mighty team, operating with the leverage of a much larger organization. Every employee can be a builder, and every useful tool can integrate into shared infrastructure.


The goal is not to replace scientific rigor and human expertise. It is to give a lean team more capacity for the work that actually requires judgment, creativity, and scientific expertise.


Over the coming months, we plan to share more on how we’re using AI across the business to deliver superior ingredients faster, and to better serve our partners. And if what you are reading excites you: we are hiring for multiple roles; take a look at our open positions[here](https://jobs.ashbyhq.com/Kingdom) .


**Notes**


1. If you’re interested in the science of this.. Light helps algae to grow through photosynthesis, the shrimp eat the algae, and waste from the shrimp is processed by bacteria that cycle nutrients back into the system. The shrimp have exceptionally very slow metabolism and can live for decades!

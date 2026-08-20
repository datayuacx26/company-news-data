---
schema_version: "1.0.0"
document_id: "8a0b42255502d991508adc06943a7fbb5011b36d2e5897b538177c8daaae8bbc"
company_key: "yc-leadgenius"
company: "LeadGenius"
source_id: "yc-leadgenius-news-import-60e73975c0bf"
canonical_url: "https://www.leadgenius.com/resources/clays-pricing-change-just-proved-the-bear-case"
published_at: "2026-06-01T18:41:20.831+00:00"
first_seen_at: "2026-07-25T11:51:49.719746+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:97bec17884d747a4a3b39cec43cc71d6ce7f8b58a83c12b8df5c82209a5a530d"
---

# Clay's Pricing Change Just Proved the Bear Case

A few months ago, we argued Clay's $3.1B valuation made no sense next to ZoomInfo's $1.15B market cap. A company doing $100M in ARR at a 31× multiple was being priced on a story. A public competitor doing $1.25B at 0.9× was being priced on cash flows. The pushback was loud and predictable.


*"It's not a database, it's a workflow."* *"You're comparing apples to oranges."* *"You don't understand the GTM engineering motion."*


Fair enough. The market is allowed to disagree with us. But the market is also allowed to disagree with **Clay** — and that's exactly what just happened.


## The pricing change that broke the loyalty


On March 11, Clay announced the biggest pricing overhaul in its history. The headline was a data cost reduction of 50–90% and a plan consolidation from three tiers to two. Buried in the announcement was the real news: Clay split its credit system into **Data Credits** (for enrichment data) and **Actions** (for platform usage). Actions now measure platform work — enrichment steps, AI calls, API requests, CRM pushes, data exports.


In other words, Clay introduced a new meter on the one thing builders had previously gotten for free: **orchestration** .


The reaction in the GTM community has been brutal. A LinkedIn post titled *"Did Clay commit harakiri?"* racked up hundreds of reactions in days, with operators from agencies, RevOps shops, and GTM engineering teams all saying the same thing: the pricing didn't get more expensive in absolute terms — it got more **uncertain** . And uncertainty is the one thing Clay's audience cannot price into a client quote.


The customer problem, in their own words


*"Customers can absorb higher prices. What they struggle to absorb is uncertainty."*


## Why this matters for the valuation


Here's the thing nobody wants to say out loud: **Clay's $3.1B valuation was built on the very behavior this pricing change is designed to discourage.**


The bull case for Clay was never really about data enrichment. ZoomInfo, Apollo, Cognism, LeadMagic, Datagma, and a dozen others have been doing enrichment for years — most of them owning the actual underlying data, which Clay does not. The bull case was always about the *workflow layer* : the idea that Clay had built the GTM equivalent of Zapier, and that an entire generation of "GTM engineers" would build their entire stack on top of it.


That bull case required one critical condition: **builders had to feel free to experiment.** Every safety check, every fallback, every waterfall, every retry, every QA column, every dedupe step — all of it was free in the old model (or close to it, if you brought your own API keys). That freedom is what created the cult. That cult is what created the inbound demand that justified a 31× revenue multiple.


The new pricing puts a meter on every one of those steps. As one operator framed it:


> "Should I add this safety check? Should I split this into another column? Should I run this validation step? Should I simplify the workflow even if it makes the output worse?" — A working GTM engineer, on LinkedIn


When the builder starts asking those questions, the product stops being magical. And when the product stops being magical, the 31× multiple stops being defensible.


## The counter-argument actually strengthens the bear case


The most thoughtful response in the LinkedIn thread came from Jordan Crawford, who argued — correctly, we think — that the pricing change wasn't a mistake. It was a deliberate ICP shift:


> "Clay became worse for small GTM shops, but better for enterprises… For those that drove little value to the ecosystem, it got more expensive because Clay decided to stop losing money on us. For the enterprises it got cheaper because they used Clay credits mostly for data enrichment." — Jordan Crawford, Blueprint GTM


This is almost certainly right. Clay looked at its P&L, saw that small agencies and GTM engineers were piggybacking on unlimited actions with their own API keys, and decided to stop subsidizing them. The agencies brought distribution and evangelism; the enterprises bring revenue. Clay chose revenue.


That's a perfectly rational business decision. But it has an awkward implication for the valuation:


The trap


If the enterprise segment is where the money is, then Clay is competing directly with ZoomInfo, Apollo, and the rest of the boring, data-owning, low-multiple incumbents — not against a frontier of unbounded GTM-engineering ambition.


You cannot have it both ways. You cannot be priced like a category-creating workflow platform **and** run the unit economics of an enterprise data vendor. Pick one. The market will eventually pick for you.


## The exodus is real, and it's structural


Scroll through the comments under that LinkedIn post and you'll see the same names appearing over and over:


- **Cargo** API-first, granular pricing, better enterprise support
- **Bitscale & Databar** Clay-like UIs with friendlier economics for builders
- **Oxygen** A CLI/MCP for GTM that lets Claude Code drive the workflows directly
- **Claude Code / Codex** Operators skipping the GTM layer entirely and writing their own orchestration


That last one is the most important, and it's the one nobody at Clay's last board meeting wanted to talk about. The same generation of GTM engineers who learned to build on Clay now has access to Claude Code, Cursor, and a fully agentic stack that can write, run, and debug an enrichment workflow in plain English — for the cost of an API call.


The moat Clay sold to investors was *"we made GTM programmable for non-engineers."* The moat the market is now revealing is *"non-engineers are becoming engineers, and engineers don't need Clay."*


## What the public comps still tell us


Restating the numbers from our original piece, because they have only become more uncomfortable:


Clay vs ZoomInfo · The Full Breakdown


184.6 pt spread


Metric Clay ZoomInfo


Valuation $3.1B private · Series C, Aug 2025


$1.15B public · Nasdaq: GTM


Annual revenue $100M ARR


$1.25B TTM


Revenue multiple 31×


0.9×


Employees ~1,040 ~3,180


Owns underlying data Largely no Yes


12-month valuation change +140%


secondary est.


−44.6%


public market


ZoomInfo trades at 0.9× revenue because the public market has decided that even a profitable, cash-generative, data-owning GTM company in a slowing growth environment is worth less than its trailing revenue. Whatever you think of ZoomInfo's product, that is the market's verdict on the category.


Clay trades at 31× revenue because the private market has decided Clay is *not* in that category.


The pricing change is the first concrete piece of evidence that **Clay's own leadership disagrees with its own valuation.** You don't introduce a new cost meter on your most loyal builders if you believe their experimentation is the engine of your growth story. You introduce one when you've quietly accepted that the growth story has to come from somewhere else — the enterprise — and that the enterprise will not pay a workflow-platform premium for a data-vendor product.


§


## The honest read


We don't think Clay is going to zero. It's a good product with a real customer base and a genuinely talented team. The pricing change is, by most reasonable measures, a sensible business decision. Margins matter. Losing money on agencies who run forty clients off a single seat is not a sustainable position.


But there is a difference between **Clay is a good business** and **Clay is a $3.1B business** .


The first is defensible. The second was always going to require the workflow-platform story to keep compounding — and the workflow-platform story required builders who felt free to over-engineer. That's the constituency Clay just chose to monetize instead of subsidize.


The 31× multiple was the price of that subsidy. When the subsidy ends, the multiple has to find a new floor. The market is starting to look for it. The next funding round, the next secondary, the next benchmark — somewhere in there is going to be a number that looks a lot more like ZoomInfo's 0.9× than Clay's 31×. Maybe not all the way down. But the gap is going to close, and it's not ZoomInfo that's going to do the moving.


You can absorb a price increase. What you cannot absorb is the realization that the thing you were paying a premium for — the freedom to experiment — was the thing being subsidized all along.


Read the original piece: "Clay vs ZoomInfo: A Tale of Two Markets" →

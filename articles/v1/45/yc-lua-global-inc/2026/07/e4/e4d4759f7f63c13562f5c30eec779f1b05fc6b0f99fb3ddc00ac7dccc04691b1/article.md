---
schema_version: "1.0.0"
document_id: "e4d4759f7f63c13562f5c30eec779f1b05fc6b0f99fb3ddc00ac7dccc04691b1"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/renting-intelligence-is-the-new-technical-debt/"
published_at: "2026-07-19T14:11:22+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:abe72803be706535005032feee62985f0bfc091c6989190ec702c18bf3961edc"
---

# Renting Intelligence Is the New Technical Debt

The industry is spending half a trillion dollars building datacenters to fight the last war.


I'll say the quiet part loudly: cloud inference is a transitional technology. It's dial-up. It's the mainframe timeshare. It's a tollbooth built on a road that's about to be rerouted. The people collecting the toll know it, which is why every frontier lab's marketing insists you need a model the size of a small country to summarize your inbox.


You don't. And within a few years, most of the AI you use will run on the machine in front of you. Here's the case. Unlike most hot takes, I'll put dates on mine.


## The tollbooth economics nobody wants to discuss


Per-token pricing is a tax on success. The better your AI works, the more you use it. The more you use it, the more you pay. Your vendor's revenue model *requires* your costs to scale with your outcomes, forever. No other layer of your stack works this way. You don't pay your database per query. You don't pay your team per keystroke. Yet somehow we've normalized renting the most strategic capability in the business at a permanent, escalating markup.


We built Lua on the opposite principle: own your agents, own your outcomes. I've watched the metered model break our customers' economics from the inside. Most agent steps are classification, extraction, routing, tool calls. Routing those through a frontier API is paying Michelin prices for toast.


The labs' incentive is to keep you convinced that intelligence is scarce, centralized, and rented. The data says otherwise.


## Curve one: you already bought the hardware


Gartner: 77.8 million AI PCs shipped in 2025, 31% of the market. 143 million and 55% in 2026. Canalys: 200M+ and up to 70% share by 2028. Gartner also expects essentially 100% of commercial PC purchases to be AI PCs by end of 2026.


AI PC shipments are heading for 70% of the market. Sources: Gartner, Canalys.


That's the largest deployment of idle inference capacity in history: 143 million nodes this year alone, sitting under the noses of the same enterprises writing eight-figure API cheques. We are building $500B of centralized compute while shipping a decentralized supercomputer nobody's using. That's not a strategy. That's a market inefficiency wearing a strategy's clothes.


And here's the delicious irony: the 2026 DRAM shortage, the thing making local hardware pricier right now, is *caused by hyperscaler AI demand* . The incumbents' capex is literally raising the price of your independence. If you think that's an argument for the cloud, ask yourself why they need the memory so badly.


A $1,500-$2,300 AMD Ryzen AI Max+ machine runs 70B-parameter models at 12-15 tokens per second today. A Mac Studio does 20-30. Two years ago that required a server rack.


The consumer memory ceiling has grown 8x in four years. The frontier still doesn't fit, and that's fine.


## Curve two: the frontier is depreciating at 90% a year


The dirtiest secret in AI: frontier capability has a shelf life of about 18 months before it fits in a laptop.


GPT-3.5-class performance needed 175B parameters in 2022. Phi-3.5 delivers it at 3.8B, roughly 98% less compute. Phi-4 at 14B beats 671B models on mathematical reasoning. Fine-tuned open 8B models have surpassed GPT-4-class systems on the Berkeley Function Calling Leaderboard, the benchmark that actually matters for agents.


The same capability, two years later, at roughly 2% of the parameters.


So when a lab charges you frontier prices, understand what you're buying: a head start that evaporates in a hardware refresh cycle. You're not renting intelligence. You're renting *this year's* intelligence at a price set by the assumption you won't notice it going generic.


Practitioners already report that for ~80% of production use cases, a laptop-scale model performs as well as a frontier API at ~95% lower cost. NVIDIA's own researchers, the company with the most to gain from giant models, published the argument formally: small models are the right default for agents. When the shovel-seller tells you to dig smaller holes, listen.


## The hammer: the cloud is now a jurisdictional hostage


Everything above is economics. This part is existential.


In June, a US export-control directive suspended a frontier model globally for nearly three weeks. Every user. Every geography. Mid-workflow. It came back. But the precedent is set: **rented intelligence can be switched off by government directive, and yours just might be.** Weights on your own hardware cannot. If your business runs on an API, you don't have an AI strategy. You have an AI landlord, and the landlord answers to someone who isn't you.


The regulators are closing in from the other side. EU AI Act high-risk enforcement starts August 2, penalties up to 7% of global revenue. Stack GDPR on top and one failure can reach 11% of turnover. GDPR fines hit ~€2.3B in 2025, up 38% in a year. At least 34 countries now restrict where AI processing can happen. And residency won't save you: a Frankfurt datacenter run by a US company is still CLOUD Act territory. Your data's zip code is irrelevant; its jurisdiction is everything.


One incident can now touch two regimes at once. Local isn't the cheap option. It's the compliant one.


The default cloud architecture (ship your data to someone else's API, hope for the best) is drifting toward *illegal* in dozens of jurisdictions. The only inference architecture that fully solves sovereignty is the one where data never leaves the building. Local isn't the cheap option. It's the compliant one.


## The steelman, dispatched quickly


"But the frontier!" Yes. Kimi K3 is 2.8 trillion parameters and needs ~1.4TB of memory. Genuinely hard reasoning will live in datacenters for years. Irrelevant to the thesis. The frontier becomes the escalation path, not the engine: a specialist you consult, not a utility you're plumbed into. The winning architecture is a router: local handles the volume, cloud handles the rare hard step. The question stops being "which model is smartest" and becomes "which model is cheap and private enough to run this loop at scale." That's a different game, and the incumbents aren't structured to win it.


## The router needs a home


If the winning architecture is a router, the obvious question is where the routing should live. Not inside any single vendor's API, for all the reasons above. It lives in the layer where your team actually works.


This is exactly the future[Lua Workspaces](https://blog.heylua.ai/finally-ai-your-team-can-use-together/) sets you up for. Workspaces is model-agnostic by design: your team collaborates in shared threads, each task routes to whichever model fits it, and everything your people teach the system compounds into memory that belongs to your company, not to a lab. Today that router mostly picks between cloud models. Tomorrow it picks between a local model on the laptop and a frontier model for the rare hard step, and nothing about how your team works has to change. The workflows, the agents, the institutional memory: all of it stays put while the inference underneath quietly moves onto hardware you own.


That's the practical difference between renting intelligence and owning it. If your AI layer is welded to one vendor's API, every shift in this market is a migration. If it's a workspace you own, every shift is a price cut.


## Predictions, so you can hold me to them


1. **By end of 2027** , major agent frameworks ship local-first routing as the default, cloud as a fallback lane.
2. **By end of 2027** , at least one major EU enterprise publicly repatriates an AI workload from a US cloud API to on-prem/on-device, citing sovereignty.
3. **By 2028** , the majority of enterprise agent *steps* (not queries, steps) execute on hardware the enterprise owns.
4. **Per-token pricing for routine inference collapses** toward zero as buyers realize the marginal cost of a local token is electricity.


If I'm wrong, this post will age badly and you're welcome to quote it back to me. But I ran engineering at Paystack through enough infrastructure shocks to know the pattern: whenever a critical dependency is rented, jurisdictionally exposed, and priced against your success, it eventually gets owned. Payments went through it. Data went through it. Intelligence is next.


The cloud isn't dying. Its monopoly on inference is. The companies that treat intelligence as something you own and compound, not something you rent by the sip, will run the next decade.


Buy the laptop.

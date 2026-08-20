---
schema_version: "1.0.0"
document_id: "b45cb2c03623e663dbbd488d6656af65a3a8a2af410c2ed40c7c2f96c63f1e11"
company_key: "insight-enterprises-inc-common-stock"
company: "Insight Enterprises Inc."
source_id: "insight-enterprises-inc-common-stock-news-import-ac4ae975e0f2"
canonical_url: "https://www.insight.com/en_US/content-and-resources/blog/the-bill-was-never-the-real-cost.html"
published_at: null
first_seen_at: "2026-08-05T00:28:55.232389+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:caae8db71af63bed8a5d8f80809827ae31cf34c3ccaa2aa4485878609695f8fc"
---

# Blog The Bill Was Never the Real Cost: The Hidden Impact of AI Token Pricing

### Your token bill went up. Your foundation may be washing out.


A few years ago, I had a water leak. Nothing dramatic, no geyser in the front yard or flooded basement. Just a quiet leak in the pipe running from the street to my house, somewhere underground where I couldn’t see it.


I found out the way most people do: The bill showed up — $1,500. I managed to talk the city out of the sewage portion, but I still owed for the water itself. Then I paid a contractor a few thousand more to dig up the line and fix it. And because the leak straddled a billing cycle, the city hit me with another $700 the next month for water that had already drained into the dirt.


Annoying, expensive, over. Or so I thought.


A couple of years later, we discovered that all that water had quietly carved out a cavity under my driveway. Repairing that cost me more than thirty thousand dollars, more than twenty times the water bill that started the whole thing. And I’ll never get that money back.


I’ve been thinking about that leak a lot lately, because it’s a near-perfect picture of what’s happening with AI right now.


For two years, tokens were billed like an unlimited tap. Twenty dollars a month, use as much as you like. So we did. Teams built habits around it. Engineers reached for a frontier model for every little thing. Products, contracts, and roadmaps all got built on the quiet assumption that the tokens were basically free.


Then the frontier labs flipped the model. Caps, quotas, metered pricing, overage charges. And suddenly everyone’s staring at a surprise bill, the same way I stared at mine.


With my leak, the bill was the part I could see, and it turned out to be the smallest cost of the whole episode. AI is in the same spot right now. The invoice everyone is staring at is real, but it’s only the water above ground.


The real cost is underneath, and almost nobody is talking about it.


## Why the tap stopped being free


That subsidy wasn’t an accident; it was the point. The frontier labs were buying a market, and the economics didn’t need to make sense yet. Flat subscriptions, generous limits, capability that cost them far more to deliver than they charged for it. They wanted us to drink from the open tap, and we did.


That era is over. Every major lab has moved from flat-rate subscriptions to metered, token-based pricing, with weekly caps and overage charges now the default. In August 2025, Anthropic capped its Pro tier at roughly 40 to 80 hours of use a week, with overages billed at API rates. OpenAI began rate-limiting Plus and added a $200 Pro tier above it. Google capped its Advanced tier, locking heavy users out mid-session.


The spread between models is staggering. The gap from the cheapest to the most expensive frontier token today runs[about 4,500x](https://www.elvex.com/blog/ai-token-cost-enterprise-budget-control) . The same task, routed two different ways, can differ in cost by orders of magnitude, and most organizations have no idea which tap they’re drinking from.


## The bill is the easy part.


The surprise bills are real, and they’re big. When Uber handed Claude Code to 5,000 engineers in December 2025, it[burned through its entire annual AI budget by April](https://www.forbes.com/sites/janakirammsv/2026/05/17/uber-burns-its-2026-ai-budget-in-four-months-on-claude-code/) — just four months. One healthcare enterprise ran up more than[$6 million annualized unplanned spend](https://www.seekr.com/resource/the-hallucination-tax-why-your-best-models-are-costing-you-the-most/) after consuming around a trillion tokens in six months.


This is the conversation the whole market is having, and it’s a manageable one because it stops at the meter. The bill is visible. You can see it, add caching, route to smaller models, and set quotas to bring spend back under control. It’s painful but recoverable, bounded by what you actually spent.


So pay the bill. Just don’t mistake it for the problem.


## The cavity under the driveway


Here’s what the invoice doesn’t show you. For two years, we spent money on cheap tokens and, more importantly, we made decisions on them. Structural decisions. Those assumptions are now baked into four foundations underneath the organization, where the water has been running the whole time:


1. **Dev teams and culture:** Engineers learned to throw a frontier model at everything: autocomplete, search, refactors, scratch work. That reflex is now a line item, and it doesn’t switch off because pricing changed. The teams that leaned hardest on cheap inference often under-invested in the fundamentals they now need in order to ration it. Velocity benchmarks, and sometimes hiring plans, were calibrated to a world where model help was free and instant. Reset the cost, and the baseline you measured against moves under your feet.
2. **Software and architecture:** We built systems that call models liberally, with chatty agents, redundant retries, and oversized context windows, because nothing in the design needed to push back on consumption. Cost was never a constraint, so nothing reflected it. Adding caching, routing, smaller models, and token budgets after the fact is the contractor digging up the line: necessary, expensive, and disruptive to everything built on top.
3. **Purchasing and vendors:** Seat-based and flat-fee deals were signed assuming generous usage, and the renewals are arriving metered. Every AI-enabled tool we adopted brought its own consumption clock, and the aggregate was never budgeted as a utility. The workflows we hardwired to a single frontier vendor leave us little leverage now that its pricing has moved.
4. **App-dev cycle expectations:** Features were greenlit on unit economics that no longer hold, and some may never pencil out. Delivery commitments made on subsidized speed are now harder and costlier to keep. And if your product passes model cost through to customers, your margins have already moved.


The bill hit all four at once. The damage shows up later, one foundation at a time.


### And it’s not just developers.


This looks like an engineering problem, contained to the people who write the prompts. It runs much wider than that.


Developers are the biggest consumers of tokens today. But the value of AI is reaching an increasingly wide gamut of people, including marketers, analysts, support reps, operations, and finance, and that group is growing far faster than the engineering org. They are also often the least equipped to understand the token implications of what they’re doing. The meter is invisible to them. They may not know that one phrasing of a request can cost a fraction of another, or that the “summarize this whole folder” button just spun up an expensive job.


So the controls can’t live only in engineering. If cost-awareness stops at the dev team, the leak simply moves to where nobody is watching the pipe. These guardrails need to cover developers and everyone else reaching for the tap.


## Agentic AI pours by the gallon.


If you want to see where this gets dangerous fast, look at agents.


Agentic tasks consume exponentially more tokens than a simple chat, and that’s a certainty rather than a guess. A single agentic run can burn[around 1,000x the tokens of one chat prompt](https://arxiv.org/abs/2604.22750) , and the cost can vary by as much as 30x from one run to the next, depending on the path the agent takes. Code that was cheap to run in a demo can be ruinous at production scale.


And here’s the part that might keep you up at night: We’re building agentic systems into the foundation *right now* , at speed, on the same “tokens are cheap” assumption that’s already proven false. We’re pouring more and more water through pipes we already suspect are leaking.


## Why this is the part that hurts


Put the two costs side by side.


The bill is visible and negotiable, painful but bounded by what you actually spent.


The foundation is the opposite, and far harder to reverse: skill gaps, misaligned pricing models, aging systems, and plans that no longer match current conditions. It doesn’t surface on this month’s invoice. It shows up cycles later, when a feature quietly stops making economic sense, and by then the money and the time are gone.


My driveway cost twenty times the water bill that revealed it. Foundational damage almost always dwarfs the invoice that points to it, which is why paying the bill and walking away is the most expensive move you can make.


## Five questions worth asking now


The good news about my leak is that the cavity was findable. But looking sooner would have caught it while it was still damp soil instead of a $30,000 hole. The same approach holds here. Foundational damage is detectable if you ask the uncomfortable questions while a leak is still just a leak.


**Five questions to find the water before it finds your foundation:**


1. If model pricing doubled tomorrow, which of your products or features stop making economic sense?
2. Which of your systems call frontier models with no budget, no cache, and no fallback in place?
3. What did your team stop learning to do because the model always did it for them?
4. Which of your contracts and roadmaps were signed assuming inference was effectively free?
5. Are you passing model cost into your own pricing, and has your margin already moved?


If those questions make you uncomfortable, good. That discomfort is far cheaper than the driveway.


## Before it cracks


Finding the water is the start. From there, here’s the order of operations I’d run.


**Identify the leak.** Instrument token consumption end to end. Most organizations genuinely don’t see where their tokens go, and you can’t manage what you can’t see. Make the flow visible before it pools somewhere expensive.


**Stop the bleed.** This part is recoverable, and it works. FinOps controls, caching, and right-sized routing routinely cut bills while adding the cost back-pressure your architecture never had. The playbook I keep coming back to is roughly 80/20: Send the bulk of inference to open or self-hosted models, and reserve the frontier models for the work that genuinely needs them. A weather lookup doesn’t need your most expensive model; a complex reasoning task might. Plenty of organizations have that ratio backwards right now, and correcting it is the fastest money they’ll save.


**Repair the structure.** This is the step most people skip, and the one that matters most. Rebuild the assumptions underneath: Re-price products against real inference cost, reset velocity expectations for a metered world, and invest in the team skills you’ll need when the tap tightens. None of it holds without a real operating layer underneath, with FinOps discipline, quota and identity controls, and governed data access. Without that layer, spend quietly routes back to unmanaged usage and the new discipline never sticks.


**Lay durable foundations.** Build so the next pricing shift never reaches the foundation. Past roughly a billion tokens a month, owned or hybrid infrastructure starts to beat frontier APIs outright — and owning that layer turns the next lab pricing change into a line item rather than a structural event.


## Pay the bill. Then go check the foundation.


The labs changed the economics. That part is settled, with no going back. The bill on your desk is the easy half of the story: visible, finite, and recoverable.


The worst part of a water leak is never the part you can see. It’s the part you find out about later, once it’s already done its damage. So pay the bill, then go check the foundation and ask the uncomfortable questions while the leak is still just a leak.


None of this is an argument to use less AI. I’m not shutting off the water. The value running through that pipe is real, and the teams that pull back now will fall behind the ones that keep building. The shift that matters is quieter: Stop treating the tap as free, and build on ground you’ve actually inspected. Do that, and you can use AI as hard as you want, on a foundation that holds.


If you’re wondering where the water has been collecting in your own organization, that’s a conversation worth having before the driveway gives out.


### **Keep your AI momentum without the cost chaos.**[Build an infrastructure that thinks ahead with Insight](https://www.insight.com/en_US/what-we-do/expertise/modern-infrastructure.html) .


### About the Authors:


#### Juan Orlandini


Chief Technology Officer, North America and Distinguished Technologist, Insight


Juan is Insight’s chief technology officer, North America, and one of Insight’s distinguished technologists. He is a 30-plus-year veteran of the IT industry and has designed and deployed enterprise computing, storage, data protection, virtualization and hybrid cloud solutions. Juan evaluates next-generation technologies for Insight and works with enterprise clients, assisting them in architecting and selecting strategic roadmaps. In his current role, Juan designates champions of the technology community within Insight and drives events that promote thought leadership, professional development and knowledge sharing.


- [Linked In](https://www.linkedin.com/in/jorlandini)


|


[Read more posts by Juan](https://www.insight.com/en_US/content-and-resources/authors/juan-orlandini.html)

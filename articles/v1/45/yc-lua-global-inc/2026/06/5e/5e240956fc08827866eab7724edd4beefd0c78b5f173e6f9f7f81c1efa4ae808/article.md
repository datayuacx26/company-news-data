---
schema_version: "1.0.0"
document_id: "5e240956fc08827866eab7724edd4beefd0c78b5f173e6f9f7f81c1efa4ae808"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/interthe-99-cent-illusion-the-real-cost-of-renting-your-ai/"
published_at: "2026-06-16T16:18:33+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:45bcdbd8ce8159153aab1c16fbd3870110c04ca5c69055138f6f2a093555be6d"
---

# Fin & The 99-Cent Illusion: The Real Cost of Renting Your AI

*Fin just sold to Salesforce for $3.6 billion. The 99-cent price that got it there looks like a bargain — until you ask what it's anchored to, and what you actually end up owning.*


A mid-size team running 30,000 support tickets a month through Fin, resolving a solid 60% of them, pays roughly $18,000 a month — north of $200,000 a year — in resolution fees alone, on top of the subscription.


And that's one agent doing one job. This is how the whole category is coming to be priced — a separate meter for the support agent, the sales agent, the ops agent — so the model compounds across every function you automate. You're not buying a tool; you're signing up to rent a workforce, one metered seat-equivalent at a time, indefinitely.


Underneath each of those resolutions is a handful of model calls — a little retrieval, a couple of replies, a few checks — that cost cents to run. Measured against raw compute, the markup runs into the thousands of percent. Ordinarily that's the whole argument: the wrapper exposé, the "it's just an API call" takedown. It isn't ours. A real product is worth far more than its compute — the orchestration, the integrations, the reliability, the years of support data are all real, and companies are right to charge for them. Paying up for that used to be an easy call — renting a big engineering org's output was how you got ahead. Margin isn't the scandal, and cost-plus isn't how good software is priced.


The question was never whether there's a margin. It's what the margin is *for* .


## How we got to $0.99


Software pricing has never really been about cost; it's about value. The whole game is choosing the comparison — and AI support vendors chose the most flattering one available: the human being stood in for. A live agent resolves a ticket for somewhere between $5 and $15 of salaried time, so $0.99 doesn't read as a markup on three cents of compute. It reads as a 90% saving on a person. Anchored that way, the price sells itself — and for a couple of years the anchor was real. You genuinely were choosing between the agent and the headcount.


That anchor is quietly slipping. As agents absorb more of the repetitive work — the resolvable tickets, the first drafts, the lookups that never really needed a person — the human baseline becomes a smaller share of how the work actually gets done. The comparison the price leans on is fading, but the price isn't moving with it. It was set when the perceived value was highest, pegged to a person, and it's being held there as the peg dissolves.


Which leaves the number floating. Increasingly, $0.99 isn't anchored to a live alternative you'd weigh today — it's anchored to the memory of one. And a price that's lost its anchor doesn't fall on its own; it has to be competed down, or re-pegged to something real. As we'll see, the companies charging it are the least able to do either.


## Outcome pricing, and the meter you can't read


AI pricing is notoriously hard and model costs scale like labour costs overtime.


The way that $0.99 is charged — per resolved outcome — was itself a stroke of genius, at a specific moment. In 2023 and 2024 nobody trusted LLM output, and pay-per-resolution sold *trust* : we'll absorb the risk of being wrong and only bill you when a customer leaves satisfied. It let sceptical buyers say yes, and everyone copied it:


- Zendesk charges about $1.50 a resolution on commitment.
- Agentforce has charged nearer $2.00 a conversation — billed whether it works or not.
- Newer entrants go as low as $0.29.
- Sierra won't publish a rate at all — every deal is custom, with different outcomes (a resolved ticket, a saved cancellation, an upsell) priced differently.


But once trust is no longer the thing you're buying, the cracks show. As models get better, the arbitrage becomes weaker.


First, success is the line item. The better the agent gets, the more it resolves, the larger your invoice — you're charged in direct proportion to the thing you were trying to achieve, and the bill grows precisely as the product starts working.


Second, you can't optimise it. Anywhere else you operate, unit cost is a lever: you train, route and refine until each outcome gets cheaper, and that compounding efficiency is where real advantage comes from. Here you can feed the knowledge base and little else — model choice, routing, escalation, scoring all sit on the vendor's side. So your cost per outcome falls when their roadmap decides it falls, not when you get good at running it. The one number that should be your moat isn't yours to move.


And whichever way you pay, you're half-blind. You mostly pay one of two ways, and you lose control with each. Pay per token and you fly blind on a unit you can't forecast — ask anyone what a token is and they can't tell you, yet it's what the bill is denominated in; consumer plans like Claude's don't even show the count, just "usage" that resets on rolling windows, so people burn a week's allowance in an afternoon and can't say why. Pay per outcome and the bill is at least legible — but it swings with your traffic and rests on a definition you don't set.


Transparent or predictable: you get to pick one. Either way, your economics stay on the vendor's side of the glass.


## The moat the model keeps eating


So why can't they just fix it — drop the price, hand you the levers, re-peg to something real? Look at the trade you're actually making. Every month you pay to keep a product current against models that keep making it redundant. The work gets redone — and the redone version is the vendor's, not yours. You pay to keep current something you never come to own.


For years, the hardest engineering in AI products went into making unreliable models behave: validation, guardrails, retries, routing, eval harnesses, whole libraries of prompt-craft. Companies spent years and millions on these rails. Then each model generation started doing it natively — structured outputs, reliable tool calls, longer context, fewer hallucinations — and absorbed them. The capability didn't vanish; it moved *down* into the model and became free, for you and your competitors alike. Years of capital expenditure, reduced to a line in someone else's release notes.


You can watch it in fast-forward at Fin, which has rebuilt on a new model three times in three years — wrapping OpenAI, then Anthropic, then training its own. Plenty still compounds along the way; your knowledge base, your tuning, your workflows all accrue. But the part you'd point to as your engineering moat — the rails that made the model behave — is exactly the part the next model tends to swallow.


And those rails aren't only perishable; they're replicable. The moment a capability is absorbed into the model, anyone with an API key has it — including the competitor you were out-engineering. Look how little now separates the best model from the rest: Fin's own model leads the best general-purpose ones by two or three points. That sliver is the entire durable edge a proprietary model buys — and it's rentable within touching distance tomorrow.


The extreme version is already on the record. Jasper rode a thin layer on OpenAI to a $1.5 billion valuation, then watched revenue roughly halve in 2024 once ChatGPT did most of it at the source. The survivors, like Cursor, built something underneath the wrapper a model release couldn't absorb.


The lesson isn't that small beats big. It's that scale is starting to invert. The expensive incumbents were built when a large engineering team was the moat — Valley salaries, massive orgs, the ability to out-hire a problem. But once intelligence is something any team can buy by the token, that same org stops compounding into an advantage and starts compounding into drag: more to maintain, more fixed cost to defend, less room to turn when the ground shifts. The three-person team renting the same models carries none of that weight. So the larger the organisation, the more of its effort goes just to standing still — and the better the models get, the wider the gap opens against it. Size isn't the moat any more. Increasingly, it's ballast.


Which is also why the price won't simply fall. Not because of the years already sunk into those guardrails — that money's gone whether they charge $0.99 or $0.29 — but because of the large standing organisation behind the product, which is expensive to run and slow to turn. A three-person team renting the same models carries almost none of that, and can sustainably charge a fraction of the price. The incumbent can't follow it down without shrinking, and shrinking means gutting the revenue it's valued on today for a payoff that only arrives later. So the price holds — less out of greed than because the scale that was once the moat is now a cost base it has to keep feeding.


## The real question: do you own it, or are you renting it?


Step back, and the through-line is simple. Renting someone else's scale used to be how you got ahead; it isn't any more. Intelligence is commoditised, the engineering on top is perishable, and the premium application layer is still priced as though renting its org is your advantage — when increasingly it's just how you keep pace, on terms you don't set and can't tune, indexed to a baseline that's fading rather than to anything you own.


Winning now asks for different things: adapt fast as the models move, govern and secure your own AI, keep your institutional memory instead of renting it back, control your own unit economics, and never be hostage to a single vendor's roadmap. A seat-and-resolution business isn't built to hand you any of that. That's not a knock on the people who built it — it's just what their cost structure is for.


And notice what the capabilities always sold as "enterprise" have in common — governance, security, audit, institutional memory. They were gated behind enterprise pricing largely because they used to require big teams to build. But they're also the rare parts of an AI stack that *do* compound for the buyer: they're durable, they're yours, and they survive every model change. If big teams are no longer the moat, there's no reason the capabilities that actually build your equity should stay luxury goods reserved for the logo at the top of the pricing tier.


That's what we built Lua to be: somewhere you build agents and actually *own* them — the agent, the data, the institutional memory, the governance, and the levers that set your cost per outcome. When the next model lands, you upgrade and everything you built comes with you. It doesn't reset to zero, and you don't watch a vendor rebuild it and bill you for the privilege. The work compounds, because it's yours — and you're not re-staffing a platform team to get there: the governance, security and memory that used to be a build are things you adopt and own, so the only engineering you keep doing is on your product, not on the plumbing the model keeps absorbing.


- **An open-source governance SDK** that puts guardrails, policy and runtime enforcement into your code — not behind a six-figure enterprise tier. It's also what makes outcome-based pricing honest: an outcome you're billed for should be one you can audit and prove, not one a vendor simply declares.
- **Honeycomb, our institutional-memory layer** , so your organisation's context and decisions compound over time instead of evaporating every time a model or vendor changes underneath you. The memory Fin had to rebuild three times shouldn't be hostage to anyone's roadmap but yours.
- **Enterprise-grade security, priced for everyone** — because being trustworthy from your first customer shouldn't depend on being a Fortune 500.


Governance and security aren't features you bolt on once you're big enough to afford them. They're how you stay trustworthy and move fast from day one. The application layer got expensive renting you scale that no longer makes you win — and keeping the controls on its own side of the glass. We'd rather you owned your leverage outright.


Build and own. Don't rent.

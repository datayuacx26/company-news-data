---
schema_version: "1.0.0"
document_id: "21a098ef93dd33b988ac6fc1cbbd2929fcf39da571e5ce41ed308d068291ebbc"
company_key: "adtran-holdings-inc-common-stock"
company: "ADTRAN Holdings Inc."
source_id: "adtran-holdings-inc-common-stock-rss-2a55922dc4ce"
canonical_url: "https://www.blog.adtran.com/en/network-autonomy-needs-an-open-ai-ecosystem"
published_at: "2026-07-29T08:35:00+00:00"
first_seen_at: "2026-07-29T09:46:18.729020+00:00"
fetched_at: "2026-07-29T09:46:21.014227+00:00"
content_hash: "sha256:a23e2528a8ebd1b9b3df4a7dc582b70cf1a1ad949009c601cc0490d7a0715bdd"
---

# Network autonomy needs an open AI ecosystem

Over six blog posts, we’ve covered a lot of ground together. We’ve walked the journey – starting with[phased AI adoption](https://www.blog.adtran.com/en/meet-adtrans-real-ai-engine-and-start-your-journey-to-phased-ai-adoption) , then from[reactive to responsive service](https://www.blog.adtran.com/en/real-ai-from-reactive-to-responsive) , from[responsive to proactive assurance](https://www.blog.adtran.com/en/real-ai-from-responsive-to-proactive) and from[proactive to autonomous optimization](https://www.blog.adtran.com/en/real-ai-from-proactive-to-autonomous) . The last two took up the questions that journey raised: what the people do (they stop doing the work and start directing it), and how the software gets built (composed in days, not coded in quarters – and safe in both directions).


Both ended pointing at the same unfinished thought. You can direct teams of agents, and you can compose workflows on the fly, only if there’s something open to compose against. A brilliant builder with nothing open to build on builds nothing. Composition is only as wide as the world it can reach.


So, here’s the conclusion I think this whole series has been driving toward: autonomy was never a feature anyone could sell you. It’s a property of the world your agents operate in. This last post is about what that world looks like, what it costs to run and what we’re building to help create it.


**The oldest pattern in computing**


I know how “open” sounds coming from a vendor. Every vendor says open; the word is nearly worn through. So don’t take the argument on our authority – it isn’t ours. It’s the most repeated result in the history of building software.


I’m old enough to remember when “online” meant AOL. That walled garden was polished, curated and capped at exactly what one company could ship. The open web was messier in every way – and it won, not because it was better on day one, but because anyone could build on it, and the building accumulated. Networking itself ran the same experiment a generation earlier. The proprietary stacks – IBM’s SNA, DEC’s DECnet – were rich, well-engineered and backed by serious partner programs, and every one of them lost to TCP/IP, the protocol nobody owned, because that’s where the building accumulated. Open doesn’t win on ideology. It wins because anyone can build on an open foundation, and that building keeps compounding while a closed one only grows as fast as its owner can ship.


That’s the mechanism, in one line: a closed catalog grows at the speed of one vendor’s roadmap; an open ecosystem grows at the speed of everyone building on it. And that gap doesn’t shrink over time – it compounds. None of this is a telecom prediction. Telecom is simply the next domain where the pattern plays out, and agentic AI is the moment it arrives.


**What does the open world look like from your seat?**


We said last time that your workflows are yours – trouble resolution, provisioning, outage escalation – and that they cross the access network, the home, aggregation and transport, the OSS, the BSS and the ticketing system by nature. Composition can only follow those workflows if the tools from all of those systems show up open and agent-ready. This means the open world has a cast, and everyone in it brings something the others can’t.


You sit at the center – it’s your network, your data and the end-to-end workflows that this whole ecosystem exists to serve. Equipment vendors – Adtran among them – bring their domains as agent-ready tools: access, in-home, fiber plant, aggregation and optical. And more than raw reach, they bring intelligence – root cause analysis, service-impact inference – exposed as tools an agent can call. Integrators bring the stitching, composing experiences above the foundation for operators who’d rather not build it themselves. And the BSS/OSS and adjacent partners – billing, ticketing, workforce, mapping – bring their own domains on the same terms, next to everyone else, rather than absorbed into any one’s catalog.


Here’s what that buys you: every participant who exposes their domain as agent-ready tools makes every other participant’s agents smarter. The use cases grow with your operation, not with our roadmap. Picture the kind of use case that creates. Your NOC engineer composes a workflow that watches for outage signals. When one fires, an agent team investigates the root cause – and in parallel, the billing system’s tools tell affected subscribers there’s an outage before they call in. The root cause comes back to a fiber break, so a work order goes to the field with the precise location, pulled from the GIS system’s tools – and while the crew works, frontline support and the subscribers themselves are kept informed of progress.


One workflow, composed by one engineer, crossing four different companies’ systems – and no single vendor ever had it on a roadmap. In a closed world, that workflow waits forever. In an open one, it’s a week of composing. A sealed catalog can’t get there, however many agents it holds, because it only ever sees its own island.


**The view from the other side of the table**


In April, I sat on stage at the Adtran Summit making roughly the case above: every system an operator runs – the OSS, the BSS, the other equipment vendors, the ticketing systems and the mapping systems – needs to provide itself as an agent-ready tool, because the moment they do, you get a force multiplier. What stuck with me wasn’t the nodding. It was that the partners on the stage were already ahead of me.


Adam Ross Hill from GLDS, one of our BSS partners, pointed out that a billing platform can carry more than 7,000 database fields – and that agent-ready tools are exactly how you correlate that down to what the moment actually needs: does the network serve my address, am I in an outage, is this a temporary disconnect or a real problem? Opening their data wasn’t a concession to anyone; it was the fastest path to the real-time dashboards and custom reporting their customers had been asking for all along.


Mike Scardina from Archtop Fiber described the workflow he wants next: a call-center agent that captures a trouble call into actionable fields – not free-text notes – and talks to our network intelligence in real time while the customer is still on the line. Two agents from two different companies, composing over the same open plane, doing something neither could do alone. Build those tools once, he pointed out, and you reuse them for the call center, for proactive outreach and for marketing. His word was “compounding.”


Twin Lakes’ Justin Reagan then supplied the caution that makes the whole thing workable: rank your agents – the mapping agent doesn’t get to touch routing tables – and keep a human between the teams, because AI can’t be held accountable and humans can. Adam put the same point more bluntly: just turning a general-purpose large language model (LLM) loose on your systems could crush your back office and your network. I don’t read any of that as resistance to openness. I read it as the specification for it.


So, openness is a two-sided deal. The operator gets workflows that follow their problem across every system they run. The vendor gets reach and reuse without being consumed into somebody’s bundle. That mutual upside is why an open ecosystem grows – and why a closed one structurally can’t.


> The use cases grow with your operation, not with our roadmap.


**The honest question: What does it cost?**


There’s an anxiety underneath this. We hear it from operators in the field, in almost these words: “Just give me the API – I already paid for my data.” And sharper still is the claim that AI agent layers amount to margin capture disguised as innovation. That worry is real.


But it’s worth being precise about where the cost lives. The tools themselves don’t have to consume tokens – an API or a model context protocol (MCP) tool reading your data is a deterministic call, the same as it’s always been. Tokens are consumed when an LLM reasons – when an agent plans an investigation, weighs evidence and decides what to do next.


The cost of reasoning scales with use, whether or not you can see it. And when a vendor’s agent layer is the only door, every question you ask of your own data runs through someone else’s meter – you can’t inspect it, can’t optimize it and can’t leave it. Bundled today, attached tomorrow, billed by sub-feature the year after. In an open world, the equation flips: the APIs and the MCP tools sit over the same data you already own, the agent layer is an enhancement rather than the only path in, and the token spend is yours to see, govern and optimize.


Don’t get me wrong – this isn’t an argument that AI should be cheap. Agents doing real work consume real resources, ours included. It’s an argument about who controls the spend. Your agents run your workflows, so the tokens they consume should be yours to manage like any other operational resource: see the consumption per workflow, decide which ones earn their cost, control the prompting that drives so much of the usage, set the budgets, choose the models, dial it up or down as the value proves out. That’s not a courtesy for a vendor to extend – it’s a requirement of running an operation. And it’s the question worth putting to anyone selling you agents, us included: is the value growing faster than the cost? Can you see the cost well enough to ask? And – most important – can you build your own agents, so the token spend is yours to control, not theirs to bill?


One more thing an open world has to be: governed. The safety architecture from[my last blog post](https://www.blog.adtran.com/en/agentic-ai-is-a-new-way-to-build-workflows) lives here too – tools that are agent-ready rather than merely exposed, governed deterministically at the tool, fed trustworthy cross-domain context instead of one vendor’s fragment. Open, governed, composable – drop any one and you’re back in the trap. That’s the bar. Here’s what we built to meet it.


**What we’re building toward**


We’re not naming what we’re building here – you’ll hear about it soon – but the shape follows directly from everything above: a governed tool plane where operators, integrators and partners all compose against the same open tools on the same terms. A cross-domain view spanning the home, the access network and the fiber plant, reaching toward aggregation and optical, so agents reason across your whole operation rather than one vendor’s island. Deterministic root cause analysis and intelligence functions exposed as tools – because handing an agent a root cause analysis (RCA) tool makes it a lot smarter all of a sudden. Governance that protects the network and the agents from each other. Knowledge bases, long-term memory and starter kits, so a skilled engineer who knows the operation can compose against it – not only a developer. And open access to your own data, so your token spend stays yours to see and control, never ours to bill.


We’ll have more to share soon. But what we’re proudest of already isn’t anything we’ve built ourselves – it’s what customers have started building on their own: real agent workflows, composed by the people who actually run the network. That’s the whole thesis in miniature. The catalog model measures itself by how many agents are on the shelf. The open model measures itself by what the people who run the network have built for themselves.


**The invitation**


The open world doesn’t arrive because one vendor builds it – not us, not anyone. It arrives when equipment vendors make their domains agent-ready, integrators compose the experiences and BSS/OSS partners show up on the same terms – an open environment built for the operator, where the workflows that get composed are yours. A vendor that claims to be the whole answer is just selling the old trap with new branding.


I said it from the Adtran Summit stage in April, and a customer repeated it back to me before the panel ended: Adtran is part of the solution, but not the whole solution. We’re doing our piece well and keeping it open. Your team, vendors and partners provide the rest.


So that’s where this series lands. The journey from reactive to responsive to proactive to autonomous was never about a switch you flip – and it turns out it was never about a product you buy, either. People directing the work. Software composed, not coded. A world open enough to build in and governed enough to trust. You don’t buy that world. You build it – together. We’re at the table, and the invitation is open.


There’s more to share soon about how we’re putting this into practice.

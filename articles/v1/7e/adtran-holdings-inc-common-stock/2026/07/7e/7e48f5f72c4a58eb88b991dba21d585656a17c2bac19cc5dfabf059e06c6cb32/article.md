---
schema_version: "1.0.0"
document_id: "7e48f5f72c4a58eb88b991dba21d585656a17c2bac19cc5dfabf059e06c6cb32"
company_key: "adtran-holdings-inc-common-stock"
company: "ADTRAN Holdings Inc."
source_id: "adtran-holdings-inc-common-stock-rss-2a55922dc4ce"
canonical_url: "https://www.blog.adtran.com/en/from-doing-the-work-to-directing-the-work-how-ai-agents-are-changing-network-operations"
published_at: "2026-07-15T10:17:00+00:00"
first_seen_at: "2026-07-20T23:22:01.904129+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:46d88c384ce38c4e8f53fc0c56802117323eb33f0991ed87b6ec25bb55dee9d3"
---

# From doing the work to directing the work - how AI agents are changing network operations

Over four blog posts, we’ve walked the journey – starting with[phased AI adoption](https://www.blog.adtran.com/en/meet-adtrans-real-ai-engine-and-start-your-journey-to-phased-ai-adoption?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260715-from-doing-the-work-to-directing-the-work) , then from[reactive to responsive service](https://www.blog.adtran.com/en/real-ai-from-reactive-to-responsive?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260715-from-doing-the-work-to-directing-the-work) , from[responsive to proactive assurance](https://www.blog.adtran.com/en/real-ai-from-responsive-to-proactive?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260715-from-doing-the-work-to-directing-the-work) and from[proactive to autonomous optimization](https://www.blog.adtran.com/en/real-ai-from-proactive-to-autonomous?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260715-from-doing-the-work-to-directing-the-work) . Each described what the network does as it matures, using the same self-driving-car analogy. This time, I want to talk about what we’ve mostly left implicit: what the people do.


Because here’s the first thing an operations team hears when a slide says “autonomous network” or “self-healing network” – fewer jobs, maybe mine. From the NOC to the field to the back office, that’s the reflex. I don’t think we should dance around that. So let me be honest: full autonomy is a real goal, and agentic AI genuinely can move us toward it. I’m not going to call it hype. But it’s a long way off – maybe an asymptote we never quite reach – and we don’t get there by flipping a switch. We slide there along a gradient.


The cars make the point for us. “Full self-driving” didn’t show up overnight; it arrived as a ladder of driver-assist levels, and full, go-anywhere autonomy still isn’t here. And notice where you sit on the way up that ladder – not in some distant control room, but in the driver’s seat. You set the destination; the car drives while you supervise, and when the rain comes down, or the lane lines vanish, it hands control back: this part needs you. You take over, then hand it back as conditions clear. That’s the stretch broadband operations is entering, and I’d argue it’s where the work lives for years.


**You become a manager**


So what changes for you? Not “you get a smarter tool.” You become a manager. Your team just happens to be made of agents.


You still decide what needs doing – the intent, the constraints, the definition of done. Agents don’t decide what matters; you do. And the intent is your own business process, the way your company runs, not a vendor’s script. The team has roles, like any good one: an agent triages, another diagnoses, another remediates, handing off to carry out that intent. They come back for permission where it matters and earn more autonomy as trust grows – the way you’d delegate to a new hire. There’s a speed gain, sure. But the bigger one is leverage: one person’s judgment applied across many workflows at once, the way a good leader multiplies a whole organization, not just themselves.


Picture it in the NOC. A monitoring team – a set of AI agents – watches on your behalf, separates a real outage from noise, and comes to you: “Service-impacting outage, roughly 400 subscribers in this area – open an investigation?” You say go, and an investigation team – another set of agents – digs into topology, history and recent changes, asks which of two likely causes to chase first, then comes back: “Most likely a failed splitter here – permission to roll a truck?” The chain ran end to end, but you made every call that mattered – and never hunted a dashboard or typed a command.


**This isn’t a telecom prediction**


If that still sounds speculative, you don’t have to take it on faith. Look at the knowledge workers who got there first. In software development, the model has already flipped: you describe a task and an agent team – tools like Claude Code, Codex and Copilot – specs it, writes it, tests it, and reviews it before handing you a finished change. Engineers run several of those teams at once now, and the scarce skill isn’t what any one agent can do. It’s how well a person directs many of them. At Google, the CEO says[roughly three-quarters of new code is already generated by AI](https://www.fastcompany.com/91531519/google-ceo-says-75-of-the-companys-code-is-ai-generated) . In finance,[98% of Morgan Stanley’s advisor teams use an assistant](https://openai.com/index/morgan-stanley/) that turns a 30-minute research hunt into a matter of seconds. In support,[Klarna’s assistant did the work of some 700 agents in month one](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/) , then brought humans back for the hard cases. AI took the routine; the role re-formed around judgment.


The pattern is the same in every one of them: the value shifts from producing the work to directing it. Telecom operations isn’t exempt from a pattern this consistent. We’re simply next in line.


Which is why cutting heads to book the savings is the wrong move – a one-time gain that caps you at old work done cheaper. Keep the people, point them at agents, and output multiplies while headcount holds. The winners won’t be the operators who did the same work with fewer people; they’ll be the ones who did far more with the same people.


So come back to the fear we opened with. The autonomous network is a direction, not a date. Every step up the gradient is people directing more, not people removed. From doing the work to directing it. From one pair of hands to many teams at once. From hunting and typing to setting intent and owning the call. The threat was never the agent that does your task; it’s the operator down the road whose people learned to direct teams of agents while yours were still doing the work by hand.


There’s a catch, though – and it’s the thread the rest of this series pulls on. The intent you’re directing is your process – trouble resolution, provisioning, a billing escalation – and your process doesn’t respect vendor boundaries. It runs across the access network, the home, the OSS and the BSS, across whoever’s equipment you run. Agents that see only one vendor’s gear can’t direct that work; they direct the slice that vendor sells.


So directing agents at scale rests on two things we don’t have yet: software that composes around your workflow as fast as you can direct it, not one brittle integration at a time, and agents that reach across every system you operate, whoever built it. One is a new way to build software. The other needs an open ecosystem to build it in. Those are the next two posts, and both land in the same place: agents shaped to your operation, not a vendor’s catalog.

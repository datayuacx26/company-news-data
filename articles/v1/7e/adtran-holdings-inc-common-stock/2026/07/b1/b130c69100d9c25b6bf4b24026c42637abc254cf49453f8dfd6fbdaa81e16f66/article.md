---
schema_version: "1.0.0"
document_id: "b130c69100d9c25b6bf4b24026c42637abc254cf49453f8dfd6fbdaa81e16f66"
company_key: "adtran-holdings-inc-common-stock"
company: "ADTRAN Holdings Inc."
source_id: "adtran-holdings-inc-common-stock-rss-2a55922dc4ce"
canonical_url: "https://www.blog.adtran.com/en/agentic-ai-is-a-new-way-to-build-workflows"
published_at: "2026-07-22T16:07:00+00:00"
first_seen_at: "2026-07-22T08:52:38.262008+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:1e0bfd8ece75cfd16deacf2b9738d4fb569266f35caedc01f9b188c9af6c6f00"
---

# Agentic AI is a new way to build workflows

The[last blog post](https://www.blog.adtran.com/en/from-doing-the-work-to-directing-the-work-how-ai-agents-are-changing-network-operations?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260722-agentic-ai-is-a-new-way-to-build-workflows) made the case that the future of operations is people directing teams of agents – and ended with a catch: directing at scale only works if the software underneath can be composed as fast as you can direct it. This time, I want to get under the hood because the way we’ve always built software can’t keep up.


I’m sure we’ve all lived this story. Somebody has a great idea for a workflow – make the frontline tech’s job easier, connect the ticketing system to what the network is actually saying. And then comes the integration project. A software engineering team spends months on the nuances of each system and the custom business logic that glues it all together – a lot of effort, a lot of time, a lot of cost. And when it finally ships, it’s brittle – broken the day any system on either end changes. But I’d argue the real cost was never the bill. It was all the things we didn’t build. Because the cross-vendor version cost so much, we bent our workflows to fit whatever a vendor had pre-built. The software shaped the operation, rather than the other way around.


The bottleneck was never the idea. It was the integration.


**What if workflows could be composed instead of coded?**


Here’s the exciting part, the part I think gets missed in the hype: the bigger change isn’t agents running your workflows – it’s how the workflows get built. When your systems expose their capabilities as agent-ready tools, the agents take it from there. They understand your intent, they build a plan to meet it, and they work out which tool to call, with which parameters, feeding one tool’s output into the next. You don’t get a single static workflow from this; the logic is dynamic and adaptable. I think prebuilt workflows start turning into custom, intent-driven ones, and I think integration time starts to go down – maybe quite drastically.


And it doesn’t take a development team anymore. A skilled engineer who knows the operation can compose a workflow across systems, and recompose it when the problem changes, in days rather than quarters. We said last time the work is yours; here’s why no vendor could ever hand you the whole of it. Trouble resolution, provisioning, a billing escalation – each crosses the access network, the home, the OSS, the BSS, the ticketing system. Every vendor sees its own slice. Only you see the whole job. When composition gets cheap, the workflow finally gets to follow your problem instead of the vendor’s roadmap.


If this sounds far off, it isn’t. At Block, the company behind Square, some 12,000 employees across 15 job functions compose their own workflows against a hundred-plus internal systems exposed as agent-ready tools, many of them not developers at all. One of our BSS partners, GLDS, put it well at Adtran Summit: model context protocol (MCP) – the protocol underneath, already adopted by OpenAI, Google and Microsoft – is on track to become the gold standard the way REST did after flat files. A software developer at the Summit told me his team is seeing a 50x improvement (I made him repeat it). I’m not promising anybody 50x. But the people who build software for a living have already switched, and I don’t think telecom gets to sit this one out.


**Should we really be building this fast on a live network?**


Don’t get me wrong – I haven’t forgotten the warning from our very first blog post on[phased AI adoption](https://www.blog.adtran.com/en/meet-adtrans-real-ai-engine-and-start-your-journey-to-phased-ai-adoption?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260722-agentic-ai-is-a-new-way-to-build-workflows) – that a legion of agents unleashed on every workflow spells disaster. Cheap, fast composition on a live network is exactly that scenario. But the answer isn’t to build slower. It’s an architecture that’s safe in both directions: protect the network from the agents, and the agents from the network.


Protecting the network starts with one principle: this flexibility requires structure, and the governance belongs in the tools, because the tools are deterministic. An agent can say all day that it wants to reboot that OLT; if the tool can say “no, you’re not” – deterministically – that’s where the rule lives, not in the large language model (LLM) itself. Consent layers, bounded and reversible actions, deterministic governors – the[governed autonomy](https://www.blog.adtran.com/en/real-ai-from-proactive-to-autonomous?utm_source=technically-speaking&utm_medium=blog&utm_campaign=260722-agentic-ai-is-a-new-way-to-build-workflows) from earlier in this series. I’d think of the agent like a really smart entry-level employee: you wouldn’t let it make major network changes unsupervised, but you wouldn’t refuse the help either – you’d put guardrails around it.


Protecting the agents is the quieter half. You don’t want an agent waking up every time a network element throws an alarm – one flapping port can generate thousands of them, and an agent reasoning over raw noise reaches very confident, very wrong conclusions. What you want is a layer of intelligence between the network and the agents: correlating events, filtering the noise, running root cause analysis deterministically – so by the time an agent gets involved, it’s reasoning over one meaningful, well-understood signal instead of an alarm storm. Hand the agent a deterministic root cause analysis (RCA) tool, and it gets a lot smarter all of a sudden.


**So who provides all of this?**


Nobody does – and I'd argue that's the point. Your workflow spans every system you run, so composition only reaches it if all those systems show up as open, agent-ready tools. A sealed catalog of pre-built agents, however many, only ever sees its own island – the old pre-built workflow in new clothing. Open standards don’t win on ideology; composition compounds on open foundations and stalls on closed ones. No single vendor can supply the whole, and that includes us. As one of our customers put it back to me: Adtran is part of the solution, not the solution. Our part is making our systems and intelligence agent-ready. Yours and your other vendors make up the rest.


Integration was the tax on every good idea we ever had. That tax is lifting – and what an open world looks like, and what it takes to run one well, is the next post.


Build fast. Build safe. Build open. Drop any one and we’re right back in the old trap.

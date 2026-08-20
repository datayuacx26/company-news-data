---
schema_version: "1.0.0"
document_id: "7225ff141479deb00c0090a007803977c5140761778f1bae26b7a424d5ede2d2"
company_key: "yc-gumloop"
company: "Gumloop"
source_id: "yc-gumloop-news-import-488d75a2156b"
canonical_url: "https://www.gumloop.com/blog/gumloop-fireworks-ai"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-21T22:19:59.182624+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:053cb65689983e80b3311b8b229756d939ef530d954d96877c34dcdb157fafc8"
---

# Why Gumloop is going all in on open weight models with Fireworks AI

Cheaper runs, faster steps, and more control in how people deploy AI across their orgs.


That question led to a more uncomfortable one.


How much of an agent's work actually needs a frontier model?


The honest answer is not much. Most steps in a workflow are extracting data, routing decisions, and summarizing text. Nobody in AI is in a hurry to tell you that, because most of the industry makes money when you burn more tokens. Tokenmaxxing is the business model.


We are going the other way. We want to shrink your AI bill without giving up any of the productivity gains, and open weight models finally got good enough to make that possible.


So starting today, Gumloop is steering hard in this direction. We optimized our agent harness for open weight models, and we’re partnering with Fireworks AI as our default inference provider to run them.


Here is what that means and why we did it.


## The cost problem nobody talks about


Agents can get expensive to run. Every step in a workflow is a model call, and complex workflows can chain dozens of them. When every one of those calls hits a frontier model, you are paying frontier prices dozens of times per run.


For a while, those who spent the most credits were rewarded for becoming “AI native.” But now, those rising AI bills have people questioning if the productivity gains are actually worth the price.


Open weight models change that math. Models like GLM-5.2 or DeepSeek V4 Pro can now handle the majority of agent work at a fraction of the cost.


And we actually tested this internally. After optimizing our harness for open weight models, we secretly swapped one of our most used internal agents from Opus 4.8 to GLM-5.2, and no one at the company noticed. We are now seeing cost savings of up to 72%.


It’s the same workflow, with the same outputs. But now at a massive discount.


## Every model gets dogfooded first


Swapping the model is the easy part. The hard part is that open weight models behave differently than frontier models, and not every one of them behaves the same inside an agent.


So the Gumloop team dogfoods every open weight model before release. We run it on our own internal agents and workflows first, and it only ships to customers once it performs as well as it should. The Opus to GLM swap mentioned earlier above was exactly that process in action.


The result is that[open weight models](https://www.gumloop.com/blog/open-weight-vs-open-source) inside Gumloop perform like first-class citizens, not a budget fallback.


## Why Fireworks


Running open weight models well requires serious inference infrastructure. Speed and accuracy matter a lot for our agents. We wanted our agents to get the same outputs they were getting from frontier models, just faster and cheaper.


And after evaluating multiple providers,[Fireworks](https://fireworks.ai/) came out ahead on speed and reliability for agent workloads. That is why Fireworks AI is now the default inference provider for open weight models in Gumloop.


> "Gumloop is a fascinating example of how every company will build AI agents tailored to its own workflows and data. The future of AI belongs to specialized intelligence built on open models, and we're proud that Fireworks provides the infrastructure that lets those agents run with the speed, reliability, and efficiency required for production." — Lin Qiao, CEO of Fireworks AI


## What you can do today


You can switch any Gumloop agent or workflow to an open weight model right now. Just select the model you want to use in the Agent Preferences dropdown.


A few places this makes an immediate difference:


- High-volume workflows like scraping, enrichment, and summarization, where cost per run matters most.
- Always-on agents that run in the background all day.
- Teams who want more control over what models they use and how much they’re spending.


If a step in your workflow genuinely needs a frontier model, keep it on one. Gumloop supports both, and the best setups we see mix them. Frontier models at the key decision points, open weight models everywhere else.


We also see many people create their agents and skills with frontier models, but then switch to open weight models to run them after the initial build.


Tokenmaxxing had a good run. But the teams that win with AI from here will be the ones getting more done for less, not the ones spending the most. That is who we are building Gumloop for.


A big thank you to the Fireworks AI team for making this partnership happen. We are just getting started.

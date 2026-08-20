---
schema_version: "1.0.0"
document_id: "5608090fcd2bd1845cd0a63bcb7f4b5e08474295b36de4833200b8d060a454a0"
company_key: "yc-hightouch"
company: "Hightouch"
source_id: "yc-hightouch-news-import-8089bfc075a8"
canonical_url: "https://hightouch.com/blog/composable-context"
published_at: null
first_seen_at: "2026-08-03T22:11:57.222552+00:00"
fetched_at: "2026-08-03T22:52:40.984273+00:00"
content_hash: "sha256:8ab9599590b7d3762ee592ea69a5dcc4e1b2ae09a2fba7582e6b51747e28dec9"
---

# Yes, models should be open. Your context should be too.

AI is having a long-overdue conversation about openness. The[Open Weights and American AI Leadership letter](https://www.microsoft.com/en-us/corporate-responsibility/topics/open-weight/) makes a simple case: companies should be able to access, adapt, and run models without locking themselves into a single provider. We agree.


But open models are only part of an open AI stack. A model is only as useful as the context it can access: your customer data, brand standards, campaign history, creative assets, business rules, and everything your organization learns over time. If that context is trapped inside a proprietary platform, the freedom to switch models does not free you from lock-in. It simply moves the lock-in up the stack.


We learned this lesson with customer data. Hightouch was founded on the belief that companies should not have to copy years of data models, governance, workflows, and business logic into another vendor’s proprietary store. That principle became the Composable CDP: keep your core data and logic in the data warehouse infrastructure you already built, own and govern, and make it useful everywhere.


The same thing is happening now with AI context. We could try to own the context layer. We could ask companies to pour their brand knowledge, campaign memory, creative assets, customer understanding, and business rules into a proprietary Hightouch silo, and charge them to store it. That might be strategically convenient for us, but it would not be right for customers.


We call the alternative Composable Context: keeping your organization’s knowledge and memory in systems you control, while making it useful across every agent, model, and workflow. The winning architecture for agentic marketing pairs the right model with the right context without forcing companies to surrender control of either.


## Context is the next strategic asset in martech


Technology vendors clearly understand how important context is to AI. The word has spread across the martech landscape. Scott Brinker named “context” as the “2026 Martech Word of the Year” in his latest[State of Martech Report](https://newsletter.chiefmartec.com/p/here-s-your-ungated-copy-of-the-state-of-martech-2026-report) .


The word “context” showed up in Scott Brinker’s State of Martech report 194 times – 7 more than “Marketing” itself.


That attention validates a simple point: agents are only as useful as the context they can access. But the context required to do real marketing work is bigger than just structured customer data. Agents also need to understand the company’s brand voice, approved creative, campaign performance, product rules, legal constraints, and the institutional memory of what worked, what failed and why. That knowledge doesn’t live neatly in one place. It’s spread across a company’s data, tool stack, workflows, and teams. So when vendors argue that the path to AI starts with moving all that knowledge into their platform, they risk recreating the same lock-in that martech has spent the last decade unwinding. In the agentic era, companies need context to remain connected to the systems they own, rather than trapped inside any single application.


## What a composable context layer looks like in practice


At Hightouch, we do not want to build another context silo that locks you into the ‘Hightouch Suite’. That would defeat the entire point. A composable context layer should connect to the places where context already lives, make that context governed and queryable, and make it available to any agent that needs it. In practice, that means context comes from three broad places.


### Data warehouses and lakehouses


First, we connect to the data warehouse and lakehouse, where the most important proprietary customer and business data already lives. This includes customers, transactions, web events, inventory, experiments, subscriptions, and performance history. The idea that customer data should live in your own systems has always been core to Hightouch.


What is changing is that warehouses and lakehouses are becoming better at storing and governing unstructured context too, including product catalogs, creative metadata, images, brand guidelines, and other assets agents need to do real marketing work. We expect more marketing context to gravitate there over time, and we are excited to plug into these context stores in the same composable way we have always plugged into customer data. That is why we are continuing to invest heavily in our partnerships with Snowflake, Databricks, BigQuery, and tools like Atlan that sit on top of the modern data stack.


### The tools your marketing team already uses


Second, we connect to the tools where the rest of your marketing context already lives and where marketing work already happens. Not all enterprise marketing context lives in the data warehouse, and it would be unrealistic to pretend otherwise.


Creative assets live in DAMs and Figma. Brand guidelines live in Google Drive, Notion, and increasingly Git. Campaign performance lives across ad platforms, lifecycle tools, BI dashboards, and experimentation systems. Increasingly, marketing work also happens inside AI tools and agents: Claude, ChatGPT, Databricks Genie, Snowflake Cortex, and whatever “coworker” agents your team is already testing in Slack. Live signals come from APIs, scrapers, event streams, and external sources. Some context sits in lakes. Some in object storage. Some in tools we haven’t heard of yet.


A composable context layer needs to connect to that diversity rather than fight it. Our goal is to make context accessible to agents while preserving the systems where it already belongs.


### Memory created by real marketing work


Third, we create new context from the work marketers do in Hightouch. Every interaction with an agent can become memory: feedback on assets, approvals and rejections, edits to campaigns, changes to guardrails, performance outcomes, and the taste a team teaches the system over time.


That memory is one of the most valuable forms of context in agentic marketing because it captures how your business actually operates. It is not just what your brand guidelines say. It is how your team applies them, which ideas get approved, which messages perform, which audiences respond, which constraints matter, and what your team learns from every campaign.


But we do not believe that memory should live only in Hightouch. We write it back to your data warehouse & make all these learnings and memories queryable from the Hightouch MCP so it lives with the rest of your context, owned by you and available to any agent you want to point at it. The more your team uses Hightouch, the richer that memory gets, and because it is written back to your stack, that memory becomes part of your company’s durable intelligence rather than a feature trapped inside one vendor’s product.


### Bridging context between systems & tools


Composable context should not just flow into Hightouch. It should flow back out to the other tools, agents, and models your team uses. If your brand-guidelines skill lives in Claude, Hightouch should be able to use it. If your team is testing a campaign-planning agent in ChatGPT or Claude, it should be able to ask Hightouch what audiences exist, what campaigns have worked before, what creative rules matter, and what the customer data says. If Hightouch learns something from a campaign, that memory should be callable by the next agent your team uses.


That is why our approach to MCP is intentionally open. Many MCP implementations expose a narrow list of predefined actions: create this, fetch that, update this record. We think it should be more than that. Hightouch MCP is designed to make anything Hightouch does or knows callable from any other tool your team uses: the agent, the context, the memory, the workflows, and the learnings created through real marketing work. The goal is not to turn Hightouch into another closed destination. The goal is to make Hightouch part of the connective tissue across your AI stack. Context should move both ways.


## Key principles as you build your AI strategy


Regardless of which vendors you use, you should be wary of letting your marketing context become locked inside a silo owned by someone else. A composable context approach gives agents the information they need to do real marketing work while keeping that context open, governed, and useful across your stack.


### Fluid


Your context should move to where it creates value. It should be accessible to the models, agents, workflows, and teams that need it. The question is not which vendor gets to own the context. The question is whether your business can use that context wherever work happens.


### Open


Your context should not be limited to one vendor’s agents, channels, or workflows. As new agents emerge across lifecycle, paid media, web, sales, service, analytics, and creative workflows, your context should be able to travel with them.


### Compounding


Your context should get better every time real marketing work happens. Every campaign, approval, rejection, test result, product update, performance signal, and agent interaction should become durable memory about how your business actually markets.


## How context will shape the next decade of marketing


The last decade of martech was shaped by the fight over customer data. The next will be shaped by the fight over context. Composable CDP took years to become the obvious architecture for leading teams. With agentic marketing, this shift will happen faster. Brands with fluid, open, compounding context will run AI that reflects their customers, brand, and business. Brands without it will keep buying generic AI tools and wonder why more automation does not create more advantage.


This is the foundation behind the products we are building at Hightouch.[Agentic CDP](https://hightouch.com/blog/the-agentic-cdp) ,[Ad Studio](https://hightouch.com/platform/ad-studio) ,[Lifecycle Studio](https://hightouch.com/platform/lifecycle-studio) , and more specialized marketing solutions still to come are all built around the same idea: context stays where it belongs, agents work on top of it, and learnings get written back to your stack.


When we started Hightouch, it would have been easier to own the data layer. We chose the customer instead. Now we’re making the same bet on context.

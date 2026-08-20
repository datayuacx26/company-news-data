---
schema_version: "1.0.0"
document_id: "2b9876b047623f1e89f71682848358fe3e9ac8f720be4b96666a556a89408de6"
company_key: "yc-aleph"
company: "Aleph"
source_id: "yc-aleph-news-import-c59328786189"
canonical_url: "https://www.getaleph.com/blog/state-of-ai-in-finance"
published_at: "2026-05-26T17:24:00+00:00"
first_seen_at: "2026-07-21T05:43:03.706827+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:8b8ddda78ee7bff32f381f4fef872b1b9b87aef211cd692a4c8c181c2e42fa08"
---

# AI in finance has an 80% problem

The first time you use AI in FP&A, it feels like magic.


Upload a couple CSVs to Claude or ChatGPT, then ask for a BvA, board summary, or dashboard. A minute later, you have something that looks genuinely impressive. You’ve gotten 80% of the way there in a fraction of the time it used to take.


But in finance, that last 20% is everything.


Do the numbers tie? Did the mappings and definitions hold across systems? Can you trust the process will work again next month?


This is the state of AI in FP&A in 2026: the models are powerful, the prototypes are impressive, and the gap to production comes down to an auditable, context-aware data layer.


Here’s what that means.


## Models are getting better, but finance teams need certainty


LLMs are *probabilistic* by nature, meaning they make their best guess at what the user is looking for. That’s a big part of what makes them so powerful. Ask for a meeting summary, and they can read through the transcript, identify key themes, and surface the takeaways you’re most likely to care about. It may not be perfect, but it’s accurate enough to be useful.


“Accurate enough to be usable” is a high bar in finance, though. Something like 99%+. Every number in your board deck needs to be defensible and traceable to its source of truth. In other words, it needs to be *deterministic* —the same inputs need to generate the same answer, every time.


That’s why[data and context](https://www.numeric.io/blog/your-ai-agents-are-starving-for-context) matter so much. In their recent[Agents for financial services announcement](https://www.anthropic.com/news/finance-agents) , Anthropic pointed out that "AI agents are only as good as the data and context they can access."


To produce work you can trust, AI tools need to be rooted in your actuals, forecasts, mappings, definitions, permissions, business logic, and source-system data.


I think LLMs are great with text. They are not great with numbers. There's a lot of things that you need to do in order for them to perform quickly, reliable in a way that is auditable and you can trust. I think part of that will come from, you know, next generation of models, models getting better. But in finance specifically, I do think that has to come as well from the application layer and tools like Aleph helping the models do well at this specific task.


## The trust gap: why finance teams don’t act on AI output


There’s a pattern hiding inside the 80% problem. Most finance teams have tried AI by now. Far fewer actually act on what it produces. The pilot goes well, the summary looks sharp, and then the output sits in a folder while an analyst rebuilds the same analysis by hand—because nobody could defend the AI’s version in a board meeting.


That’s the trust gap, and it’s not a model-quality problem. It’s an explainability problem. A black-box answer that’s right 95% of the time still gets re-checked 100% of the time. And once you’re re-checking everything, the time savings quietly disappear.


Three things close the gap:


- **Traceability to transactions.** Every figure in an AI-generated report needs to drill down to the GL entries and source-system records behind it. If a number can’t be traced, it can’t go in the board deck—no matter how confident the model sounds.
- **Driver attribution.** “Revenue missed by 4%” is a headline, not an answer. The output needs to name which accounts, customers, or cost centers moved and by how much—the way[AI-powered variance detection](https://www.getaleph.com/answers/ai-fpa-software-variance-detection) surfaces the specific drivers behind a miss instead of just flagging it.
- **Human-review gates.** AI drafts, finance approves. A workflow where someone signs off before anything reaches leadership keeps accountability where it belongs—and makes the output something the team will actually stand behind.


This is what[explainable AI in FP&A](https://www.getaleph.com/answers/explainable-ai-fpa) looks like in practice: not a model narrating its reasoning, but outputs grounded in governed data where the lineage already exists. Explainability isn’t a feature you bolt on at the end. It falls out of the architecture—or it doesn’t exist at all.


## How do I give AI models my business context safely and securely?


One new way to give AI models context is MCPs. Instead of exporting CSVs and uploading them to a chat window, MCPs let models query live systems directly.


MCPs are a meaningful step forward—we just[released our own](https://www.getaleph.com/mcp) . They’ve also made the “vibe code your own FP&A tool” path start to look tempting. Just upload your spreadsheet models to Claude, hook it up to the Netsuite MCP, and keep iterating until you have something that works.


For one-off analyses, that might be good enough. For production-grade FP&A, it’s not quite there.


A NetSuite MCP helps Claude access NetSuite. A Salesforce MCP helps Claude access Salesforce. But those systems don’t share mappings, definitions, or business logic. Financial data might live in NetSuite, sales data might live in Salesforce, and both systems might be talking about the same booking, revenue stream, or expense category in different ways.


You end up with multiple versions of the truth, which makes working with cross-system data far more difficult.


I looked at this, and I was like, this is what I've always wanted. Like, I've always wanted all of my data to sit in one place. Because what I found in doing FP&A is you have financials in like, for us, they're in NetSuite or they're in your ERP system, and then you have sales stuff that is sitting in Salesforce or, you know, some other thing. And you need to they all have information talking about the same thing, but it's all labeled differently. The the very first thing that I used Aleph to do was map all of this stuff into a common language. So if I have an account for professional services data in AI in NetSuite, and I'm getting that revenue there. It's called professional you know, it's a called "account 4000: professional services data" or something like that. Then over in Salesforce, I have a a deal that I just booked, and it's called something different like PSDNA or something like they've abbreviated that. I've taken all of those missed, like, nonmatching things, and I've mapped them all into a common language so that whenever you pull something out of NetSuite or out of Salesforce, everyone is speaking the same language about what is this booking, what is this revenue, what is this expense.


Aleph connects the dots across systems, creating a common language that finance teams and AI tools can both rely on.


Without that layer, the scattered data problem finance teams have dealt with for years becomes a scattered MCP problem. The LLM has access to more systems, but it’s still unaware of how those systems map together, which permissions apply, and what logic finance uses to make the numbers meaningful.


Could Claude help reconcile some of that itself? Yes…to a certain extent. The more steps a model has to take to clean, map, normalize, and structure your data, the more room there is for small hallucinations (read: errors) to compound. And because the model doesn’t have its own finance database, that context has to live somewhere else: a markdown file, a set of one-off instructions, or a patchwork of configs someone has to maintain.


Instead of worrying about all of that, finance teams can use their favorite AI tools to build, analyze, and automate on top of live, governed data via Aleph without rebuilding the context layer themselves.


{{quote-1}}


## Your stack should get better as the models get better


AI isn’t going anywhere. And despite decades of predictions to the contrary, neither are spreadsheets. In fact, they’re stronger than ever, with the world’s biggest AI companies[investing billions into spreadsheet add-ins](https://chatgpt.com/apps/spreadsheets/) and integrations.


That’s exciting for us because it complements what we’ve been building. As Claude and ChatGPT get better, Aleph gets better: Claude for Excel gets more useful when it can reference live Aleph data instead of hardcoded exports. Aleph gets more powerful as the models get better at spreadsheet work, analysis, and workflow automation.


The most critical ingredient, though, is **trust** . Any dashboard, forecast, or board summary AI helps create needs to be verifiable. Teams need to be able to drill into every number, trace it back to the source of truth, and know a workflow will reliably produce the same result next month.


That kind of repeatability is what any production-grade FP&A system demands. Aleph gives finance teams a way to harness the latest AI models while keeping the outputs governed, traceable, and repeatable.


Six years ago when we started Aleph, we made a bet, and that bet was the spreadsheets, you know, would still be relevant in the way that finance teams work and the ways that finance teams wanna do FP&A. Like, the relationship, between finance teams and the spreadsheet long predates us. And we made a bet that that was still gonna be the case. And I think seeing all of the investment going in the space, seeing, you know, not only Anthropic and OpenAI investing heavily, but also, you know, ten plus startups that have raised tens of millions going after this horizontal spreadsheet agent makes us even more confident that that bet was right and that spreadsheets not only will be as central as they were, but it will also become more central to finance workflows.


## Build an AI-powered finance function on a solid foundation


It’s an exciting time to be a finance pro. AI is showing up the tools teams already use, making everyday FP&A work faster, smarter, and more flexible. This generation of finance teams will be the most valuable in history.


But the best version of that future is not AI floating above the business with no grounding in the numbers. It’s AI connected to the data, definitions, permissions, and workflows finance already trusts.


That’s what Aleph is built for: the governed finance context underneath the AI tools your team already uses.


Claude, ChatGPT, Gemini, and the next wave of AI tools bring the intelligence. Aleph brings the live data layer, mappings, audit trails, and traceability that make the output usable in real FP&A work. Together, they give finance teams more leverage without giving up trust.


**Maximize the upside of AI in finance while minimizing your risk.**[Schedule a 1:1 demo today.](https://www.getaleph.com/demo)

---
schema_version: "1.0.0"
document_id: "279198000db21d92c911894c523dab9ace527cc9147373f1b385bbb2171a0d2a"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/ai-agents-in-finance"
published_at: "2026-08-12T14:30:45+00:00"
first_seen_at: "2026-08-12T23:13:16.243917+00:00"
fetched_at: "2026-08-12T23:13:22.570171+00:00"
content_hash: "sha256:d44905f45d14682111ba0d6c72e11ade6f8f5a45a4d47c4ed9f09e3f171fdddf"
---

# AI Agents in Finance: What They Actually Do Today

A tempting way to open this post would be with a stat about how many finance leaders say AI will transform their function in the next five years. I'm not going to do that, mostly because I don't trust those surveys, and partly because Lago has a dog in this fight: we're shipping an AI assistant that sits on top of billing and revenue data, so anything I say here should be read knowing that. Read on anyway, I think the actual answer is more interesting than the hype.


Last month someone on our own finance side asked me why a specific customer's invoice total looked off. Not wrong, exactly. Off. The kind of thing where you stare at a dashboard for ten minutes, cross-reference two tabs, and eventually find a proration that fired on the wrong day. That's a two-line answer if you already know where to look. It's a twenty-minute detour if you don't. That gap, between "someone who knows the system" and "someone who has a question," is roughly the entire pitch for AI agents in finance, and it's worth being precise about what that actually means, because most of what's written about it isn't.


## Agent is doing a lot of work in that sentence


Here's the confusion I keep running into. Someone says "AI agent" and means anything from a chatbot with a finance-flavored system prompt to something that autonomously reconciles your books overnight. Those are wildly different things, and conflating them is why so much of the writing on this topic feels like it's describing a trend instead of a tool.


The useful distinction is autonomy over multiple steps, not just fluent answers. A chatbot answers a question. A dashboard shows you a number you asked for by clicking around first. An agent, in the sense that actually matters for finance work, takes a goal like "check whether this month's revenue recognition matches the invoiced amounts," breaks it into steps, pulls the data it needs from more than one place, and either flags what it finds or takes a bounded action. The bar isn't "sounds smart." It's "did something on its own that would've taken a human several steps to do."


## What's actually running in production right now


Strip away the aspirational language and four categories show up again and again, and they map pretty directly onto billing and revenue operations.


The first is natural-language querying over revenue and billing data. Someone asks "what did MRR look like for this segment last quarter, and what drove the change" and gets an answer without opening a BI tool or filing a request with data. This is the least glamorous of the four and also the one shipping fastest, because the underlying data (subscriptions, invoices, usage events) is already structured. It's a retrieval problem wearing an agent costume, and that's fine. Most useful software starts that way.


The second is anomaly detection that runs ahead of a human noticing. A billing anomaly, an unexpected churn spike in one cohort, a proration that didn't apply the way the plan change should have triggered it. This isn't new territory for Lago specifically.[Billing observability](https://getlago.com/blog/billing-observability) has been about surfacing these anomalies with metrics and alerts for a while now. What's different with an agent layered on top is that it doesn't just fire an alert. It's to surface the anomaly before your finance team finds it during a monthly close, and increasingly, to attach a plausible cause so the human doesn't start from zero.


Third: drafted communications. Dunning sequences, collections follow-ups, the "your subscription is about to renew at a different rate" emails nobody wants to write by hand. An agent drafts these based on account context (payment history, plan, prior interactions) and a human approves before anything goes out. This is the category closest to "solved," mostly because the stakes of getting a draft slightly wrong are low and a human is still in the loop.


Fourth, and the one I'd bet is still two years from being boring: scheduled reconciliation and close-related checks. Comparing invoiced amounts against recognized revenue, flagging discrepancies between what a usage-metering system recorded and what got billed, running the kind of cross-checks a junior analyst does every close cycle. This works today for well-defined, rules-based checks. It does not yet handle the judgment calls a senior controller makes when something genuinely ambiguous shows up, and I don't think that's a training data problem you fix by next quarter.


## What it can't do yet, and why that's the more useful thing to say


Every one of the four things above has a version that fails: the query that returns a confidently wrong number because the underlying billing data was ambiguous, the anomaly flag that's actually just a legitimate one-time discount, the reconciliation check that can't tell the difference between "this looks wrong" and "this is unusual but correct." None of the pages currently ranking for this topic mention failure modes at all, which is its own tell. If a piece about AI agents in finance doesn't tell you where they break, it's marketing, not analysis.


The honest version: agents are good at bounded, well-defined tasks where the data model is clean and the cost of a wrong answer is a human catching it, not a customer being billed incorrectly. They're not yet good at the ambiguous 10% that makes finance work require a finance person. That's not a knock on where this is going. It's just an accurate description of where it is.


## Where this connects to billing specifically


This is the part that made us build something, rather than just write about it. Billing and revenue data is structured, high-stakes, and query-heavy in exactly the way that makes natural-language agents useful faster than in messier domains. A subscription, an invoice, a usage event: these are well-defined objects with clear relationships, which is a much better substrate for an agent than "read this contract and tell me if we're compliant." Lago's[AI billing infrastructure](https://getlago.com/blog/ai-billing-infrastructure) work has mostly been about metering and pricing AI products correctly. The Finance AI Assistant is the other side of that: using the same structured billing data to answer the questions a finance or RevOps person actually has, day to day, instead of making them build a report first.


If you're building the kind of agent that monetizes AI usage rather than one that answers questions about your own revenue, that's a related but different problem, and we've written about[how to monetize AI agents](https://getlago.com/blog/ai-agent-monetization) separately. The overlap between "billing an AI agent" and "using an AI agent to understand your billing" is real, but they're not the same post.


## Key takeaways


Agent means autonomy across multiple steps, not just a fluent answer, and most of what's branded "AI agent" in finance content right now doesn't clear that bar. Four things are actually working in production today: natural-language queries over revenue and billing data, anomaly detection ahead of human review, drafted routine communications, and rules-based reconciliation checks. The judgment calls a senior finance person makes on ambiguous cases are still firmly human territory, and any piece that doesn't mention that is skipping the interesting part. Billing and revenue operations specifically are a good fit for this because the underlying data is structured and well-defined, which is a big part of why this is moving faster in finance than in messier domains.

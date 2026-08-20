---
schema_version: "1.0.0"
document_id: "b57e9373187eaf8ff021f38e9eda5a22306ce9ff0a217ca946cd245877f8c51f"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/ai-api-token-based-pricing"
published_at: "2026-07-24T07:33:42+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:c46d0deeeb2976655ed3a1f3535e3e8286c7adc25dad40ddfd325beb3adf8f94"
---

# AI API Billing: Token-Based Pricing for LLM Products

In June 2025, Cursor switched its Pro plan from a flat 500 requests a month to a pool of credits. Twelve days later, "Unlimited" quietly became "Extended" in the plan description. Then a developer posted a $7,225 invoice from a single day of usage. It got 797,000 views in a week. Cursor's CEO published a public apology and refunded every affected user between June 16 and July 4, 2025. Nobody on that team woke up planning to break trust with their most active users. They just priced a flat plan on top of a cost structure that isn't flat.


Lago sells billing infrastructure. This post is about how to price a product built on someone else's tokens, and yes, the answer eventually points at Lago. Read at your own risk.


## The 40x spread


If you're building on top of GPT-4o mini, Claude Opus, or anything in between, your input costs and output costs are not the same number, and they are not close.


GPT-4o mini: $0.15 per million input tokens, $0.60 per million output tokens. Claude Opus 4.8: $5.00 input, $25.00 output. Same task, same prompt shape, a 33x spread on input and a 40x spread on output, depending on which model answers the request.


Claude Haiku 4.5 sits in between at $1.00 input and $5.00 output. GPT-4o lands at $2.50 input and $10.00 output. Claude Sonnet 4.6 sits closer to the middle at $3.00 input and $15.00 output, and Anthropic's pricing has its own cliff: cross a 200K-token input threshold on the equivalent Sonnet 4.5 tier and the whole request jumps to $6/$22.50, effectively doubling cost mid-request. None of these are static either. Anthropic ran introductory pricing on newer models through August 2026 before reverting to standard rates. OpenAI discounts batch processing 50%. Prompt caching cuts input costs another 90% on repeated context.


You are not pricing against one number. You are pricing against a moving range, and the range is wide enough that the same feature can cost you 40 cents or 1 cent depending on which model happens to answer it.


## Four ways to charge for it


Passthrough is the simplest: charge the customer exactly what the model cost, plus a fixed margin on top, per token. It's transparent, it's easy to justify, and it means your revenue moves with your provider's pricing changes, both up and down. It also means your invoice looks like a cost breakdown, which is a strange thing to hand a customer who came to buy a product, not a commodity.


Flat per-request pricing hides the token math entirely. The customer pays $0.05 per API call, per resolved ticket, per generated image, regardless of what happened underneath. Customers like this. It's the model Intercom's Fin uses for ticket resolution and Salesforce Agentforce uses for conversations. It also means you are underwriting the variance. If a request routes to a cheaper model, you keep the spread. If it routes to a pricier one, you eat it.


Blended or bundled pricing sits between the two: a seat price or subscription that includes a usage allowance, with overage billed per unit past the cap. This is the shape most platform companies converge on, because it gives customers a predictable number to budget against while still letting you recover cost at the edges.


Credits are a variant of all three, prepaid instead of postpaid. The customer buys a balance, you draw down against it per token or per request, and you never bill negative. It solves a collections problem more than a pricing problem, and it's worth treating as a payment mechanism layered on top of one of the three models above, not a fourth model of its own.


## The margin cliff


Say you're pricing a support-ticket-resolution feature. Average request: 3,000 input tokens, 1,000 output tokens. On GPT-4o mini that's roughly $0.001 per request. On Claude Opus, roughly $0.04. Same feature, same prompt, a 38x difference in raw cost depending on model.


The reusable version of this math: price = cost / (1 − target margin). At a 70% target margin, the mini-routed request prices at roughly $0.003. The Opus-routed request prices at roughly $0.13. Whatever flat number you actually charge sits somewhere between those two, which means it's exactly right for neither.


Price the feature at a flat $0.05 per resolved ticket and the mini-routed requests carry a 98% margin. The Opus-routed requests carry 20%. That's fine, until a model update changes your default, a customer's usage pattern shifts toward harder tickets that need the bigger model, or you add a smarter routing layer that sends more traffic to Opus because it resolves more tickets on the first try. Your blended margin was never one number. It was an average over a mix you don't control, and the mix moves.


There's a second leak in this math: failed or hallucinated requests still cost tokens whether or not you bill for them. Most teams don't charge customers for a bad response, which means your effective cost per billable request is higher than your raw per-request cost by whatever your failure rate is. A 5% failure rate is a 5% tax on the margin you thought you had.


This is the part flat-rate AI pricing usually skips. The pricing decision isn't "what do we charge." It's "what do we charge, and how do we see the moment the underlying mix breaks the number." We[built a stress test](https://getlago.com/blog/ai-margin-cliff) so you can put your own numbers in and watch where the margin breaks.


## What actually breaks


The math above isn't hard. Spreadsheets do it fine for one feature and one model pair. It stops being a spreadsheet problem the moment you have more than one feature, more than one model in rotation, and a pricing page that changes as often as your product roadmap does, which for most AI products is closer to weekly than quarterly.


At that point you need per-request events that carry which model answered, how many input and output tokens it used, and what that combination cost you, rated against whatever price you're charging the customer for it. That's a metering and rating problem, not a pricing philosophy problem, and it's the same shape whether you're an AI wrapper reselling GPT-4o mini or a platform charging developers to run agents on your infrastructure.


I'll be direct about where this goes: this is the problem Lago's usage-based billing engine is built to sit under. It ingests usage events per token, per model, per request, rates them against whatever pricing shape you've picked using any of the[charge models Lago supports](https://getlago.com/docs/guide/plans/charges/charge-models) , and gives you the margin visibility to see a cost spread before it becomes a quarter-end surprise. It's open source and payment-processor agnostic, so switching from passthrough to blended pricing, or adding a new model to the rotation, doesn't mean re-architecting your billing stack. It's a description of the problem, not a pitch. Which model you pick and which processor you route payments through is still your call. For more on billable metrics and how usage-based charges get structured, the[usage-based charges documentation](https://getlago.com/docs/guide/plans/charges/usage-based-charges) covers the mechanics, and[creating a billable metric](https://getlago.com/docs/guide/billable-metrics/create-billable-metrics) is the first concrete step.


## Three paths, not one


If your product has one model and low request volume, passthrough with a clean markup is honest and easy to build. You don't need infrastructure for this, you need a multiplier and an invoice line item.


If your product has multiple models in rotation and customers who want predictability, blended pricing with a usage allowance and metered overage is the shape that scales, but it requires the event-level tracking described above or your margin visibility disappears the day usage mix shifts.


If your product is agentic and the unit of value isn't a token at all but an outcome (a resolved ticket, a completed workflow), outcome-based pricing is where the market is heading, and it requires the same underlying metering just applied to a different unit. We've written about that shift in more detail in[how AI agents get monetized](https://getlago.com/blog/ai-agent-monetization) and in the broader landscape of[AI pricing models](https://getlago.com/blog/ai-pricing-models) .


None of these paths are wrong. The wrong move is picking one before you know which mix of models and usage patterns your product actually produces, because that mix is what determines whether your margin holds.


For the deeper argument on why tokens make a messy pricing unit in the first place, see[pricing AI as a unit-based problem](https://getlago.com/blog/pricing-ai-a-unit-based-problem) . For the general mechanics of designing a usage-based model from scratch, the[full implementation playbook](https://getlago.com/blog/the-full-playbook-how-to-design-usage-based-pricing-models) is the place to start.


## Key takeaways


Token costs vary 30-40x across models from the same provider, and the spread moves as providers reprice. Flat per-request pricing hides that variance from customers but makes you absorb it, which is fine until your model mix shifts. The fix isn't picking the "right" pricing model once. It's building the metering layer that shows you, per request, what it cost and what you charged, so you catch the margin cliff before it shows up in your P&L. Cursor found this out in public. Cheaper to find it out in a spreadsheet first.

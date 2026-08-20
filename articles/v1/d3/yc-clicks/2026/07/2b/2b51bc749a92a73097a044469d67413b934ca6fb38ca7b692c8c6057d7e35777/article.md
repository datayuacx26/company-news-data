---
schema_version: "1.0.0"
document_id: "2b51bc749a92a73097a044469d67413b934ca6fb38ca7b692c8c6057d7e35777"
company_key: "yc-clicks"
company: "Clicks"
source_id: "yc-clicks-news-import-80b31ada7c32"
canonical_url: "https://www.goclicks.ai/news/vendor-per-seat-not-ai"
published_at: null
first_seen_at: "2026-07-21T13:47:55.095513+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:6b212d54c10cd3f2c9f9b38d90e4caeb6abc108bebc1a8ad3a771255754af01d"
---

# If Your Vendor Charges Per Seat, They're Not an AI Company

There's a question I keep coming back to. What does it actually mean to be an AI native company? Not "we use GPT somewhere in the stack" native. Actually, structurally, can't-exist-without-inference native.


I think the answer is simpler than people make it. And it has nothing to do with technology.


It's about cost structure.


If you're an AI native company, your token spend is the plurality or even the majority of your costs. That's it. That's the whole test. Everything else follows from this one fact.


Think about what made software companies software companies. It was never the code itself. It was the economics. You build the product once, then every additional user costs you basically nothing. Near-zero marginal cost of distribution. That's why per-seat pricing works so beautifully for SaaS. Every new seat is almost pure margin.


AI native companies are the opposite.


Every single time a user does something, you pay. Tokens in, tokens out, money gone. The cost of serving each user is real, significant, and scales linearly with usage. You don't build it once and distribute it for free. You build it once and then pay for every single interaction forever.


This is not a subtle difference. This is a completely different type of business.


And once you see it, you start noticing things.


## The UiPath vs Sierra test


Take UiPath. They sell per-bot licenses. You buy an unattended robot for a few thousand dollars a year and it can run unlimited processes. Doesn't matter if it runs once or a million times, same price. The marginal cost of one more run is essentially zero, so the pricing reflects that. This is classic software economics. There is no AI here. The bot is a script. It costs UiPath nothing extra when you use it more.


Now look at Sierra. They charge per resolved conversation. If their AI agent handles a customer issue end to end, you pay. Every conversation costs Sierra real money in inference. So the pricing matches the cost structure. If they handle more calls, you pay more. This is how AI needs to be priced. Sierra could not charge a flat fee, because that is not how their cost structure works. This is inherently different from UiPath and clearly where the world is going. A nice side effect is that it maximizes alignment between buyer and vendor. If Sierra's AI works, they make more money. UiPath, on the other hand, can lock you into a big contract and never deliver.


## The pricing tells you everything


This is the lens buyers should use. When a vendor charges per seat or per license with unlimited usage, they're telling you that AI is not the core of their product. Their costs don't scale with your usage because the AI isn't doing the heavy lifting. When a vendor charges per outcome or per usage, they're telling you the opposite. Every unit of work costs them tokens, and they're confident enough in the value to let you see that.


At Clicks, we price the same way Sierra does. Our AI agents automate back-office workflows, and we charge based on the work they complete. Every task costs us inference. If we can't deliver enough value per task to justify the tokens, we don't have a business. That constraint is what keeps us honest and what keeps our product good. It's what AI native means in practice.


One more implication. If you're a successful AI native company, your revenue is higher than your token cost per unit of work. If it's not, you don't have enough pricing power to charge for the value you create. Maybe your users don't actually care about the AI part. That's fine, but then you're a software company, not an AI company. Own it.


## Ask how they charge


The next time someone pitches you an "AI solution," don't ask what model they use. Don't ask about their training data.


Ask how they charge.


The answer tells you everything.


PS: Even ChatGPT's pricing proves the point. It looks like a seat-based subscription, but there are usage caps. Hit your limit and you wait, or you upgrade. That's not seat-based pricing. That's usage-based pricing in disguise, with the seat fee as a minimum commitment and the cap as a ceiling. OpenAI knows what every conversation costs them. They just wrapped it in a format that consumers expect.

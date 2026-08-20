---
schema_version: "1.0.0"
document_id: "c0fc7b1d9b2ba47421febf8e7aece0c0c14108035f07868c987db49eb6258271"
company_key: "yc-orange-slice"
company: "Orange Slice"
source_id: "yc-orange-slice-news-import-fc722c545c85"
canonical_url: "https://orangeslice.ai/blog/ai-sales-agent-surprised-us"
published_at: null
first_seen_at: "2026-07-24T07:49:43.182361+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:9b0b0c8e7f579dfd09b829bf4ba1cf82ed05d213f29db0b7bfe146bdcca518be"
---

# We Built an AI Sales Agent. The Way People Use It Completely Surprised Us.

We launched Orange Slice back in January as the world's first agentic sales spreadsheet. This means we can handle complex sales tasks in natural language, right inside the familiar interface of a spreadsheet. Since then, we've processed thousands of user prompts, and we've learned a ton about what users actually expect from a long-running sales agent.


For context, users ask the Orange Slice agent for everything from prospecting and research to CRM enrichment and web scraping. Here are a few classic examples of what we see every day:


> Find 10 law firms in Maryland under 10 employees that handle medical malpractice.


> Find founders / decision makers / CISOs / CMOs / owners at these companies.


> Enrich LinkedIn URL, company name, and employee count.


For the most part, we anticipated this. Our thesis going into YC was that outbound sales is a game of **who** you're talking to, not **what** you say. What really matters is that the person you're reaching out to actually has the problem you claim to solve. So our platform is built to help you identify and enrich proxies for “likelihood to have X problem.”


## What proxies are people asking for?


By far, the most common prospecting filters are geography and headcount. These days, location and company size are table stakes. Tools like Apollo and LinkedIn Sales Navigator have had this for years. Funding stage is probably skewed higher for us since we're a startup selling to other startups, who also sell to startups.


There's not all that much surprising about this chart. It lines up almost exactly with what we've experienced on the ground.


## Industries


Again, you can see our startup bias pushing software to the very top by a large margin. But throughout YC, we noticed that companies selling to brick-and-mortar businesses like healthcare and legal, especially those with a long tail of small companies, desperately need help with prospecting and finding decision makers.


In fact, there are entire massive companies like Definitive Healthcare built just to help healthtech companies get data on medical clinics.


## The most surprising insight: Rapid experimentation vs. static workflows


The most surprising thing I learned from reading through thousands of user sessions was how often users relied on Orange Slice to make **judgment calls** on the fly.


We expected users to treat Orange Slice like a massive, static workflow engine: go build me a list of 10,000 leads and enrich them all.


Instead, they use us for incredibly rapid micro-experimentation. We see a massive volume of questions focused on single accounts or tiny segments:


- Is this specific company a good prospect?
- What should I say to this one person?
- Do they look like they fit our ICP based on their recent posts?
- Are they showing signs of capacity strain right now?


They aren't just using us as a data tool to build tables. They're using us as a GTM context layer and a real-time decision engine.


## Secondary insight: if you give someone a chat, that's all they'll use


When we first launched, it was honestly a bit frustrating. We built a powerful spreadsheet interface alongside the chat, but users just expected the chat box to do everything. At the time, it couldn't.


They didn't want to click “add column” and configure an enrichment. They just typed, “I added an email column, fill it for every row.” They didn't want to navigate a complex UI to set up a recurring job. They typed, “run this every morning and email me.”


But Claude 4.5 Opus changed everything. I vividly remember going home for Thanksgiving, switching our agent over to Opus, and typing a test prompt I had probably run 100 times before. The results were truly amazing. From that point forward, the chat interface became the primary way you can and should use our product.


## On GTM alpha


GTM is a game of alpha. Like financial markets, you constantly have to iterate on your “algorithm” in order to stay ahead of your competitors. That often means experimenting faster and more frequently than they do.


Real GTM doesn't usually look like “find a big list of prospects and run a cold email campaign.” Instead, it looks like this:


- Try cold emailing 1k people. No positive replies.
- Try a smaller, tighter segment. Get one reply.
- Okay, this segment responds the best. Let's find an adjacent segment and try similar messaging.
- Hmm, I went to a conference last weekend. Is anyone on the attendee list in my ICP?
- And so on.


Our goal at Orange Slice is to build the best control plane for you to use AI to grow your business.


You can sign up for our tool here:[orangeslice.ai](https://orangeslice.ai/)
My email is always open for feedback and ideas:kishan@orangeslice.ai

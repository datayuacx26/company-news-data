---
schema_version: "1.0.0"
document_id: "3549197e4aa7de60b853a2a4b886d062ca44681f421dae6fce2557cd485f1e6c"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/we-let-ai-drive-our-product-roadmap-and-it-10x-d-our-activation-rate/"
published_at: "2025-10-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:ff12d823f4fbc5bb20d123bf08034bb3456afaccdf593999de6a94e93f1d8a97"
---

# We let AI drive our product roadmap (and it 10x’d our activation rate)

We’ve been growing really fast (30%+ MoM ARR) at Basedash since launching earlier this year. Most of that growth has been the result of hard work, but we’ve also had a secret weapon: an AI agent that acts as both a data analyst and a PM, working 24/7 to optimize our product’s activation and conversion rates.


For decades, companies have been making product decisions based on intuition and manual data analysis. We wanted to see what would happen if AI could take the wheel completely.


We originally built this AI agent as an internal project in June, and since then, it’s improved our activation rate by 10x. Yes, that’s accurate—we went from 5% activation in June to now being over 50% (chart below generated in Basedash, of course 🙂).


Activation for us involves a company connecting their data to our platform, which can be quite a difficult task (finding database credentials, checking with their security team, and often self-hosting Basedash on their own infrastructure), and then actually building a dashboard or asking our AI a question.


So, how does the AI work? First, we gave it access to all of our business data, from our product database, to Stripe, to Segment, to our internal Slack, and everything in-between. The agent can explore all of this data incredibly efficiently ( **cough** unlike MCP), look up public info online, and reason through anything it finds. After that, we gave it some light guidance around the kinds of things we care about: which metrics to optimize, how our current product funnels work, etc.


Finally, we set up a schedule for the AI to compile its results and share its findings with us every morning over a quick Slack message. Here’s an example of a message we got earlier this week:


As you can see, the AI discovered a clear correlation between organizations that create multi-chart dashboards early on and long-term product usage. It backed its findings up with stats and gave us an actionable product suggestion to tweak our onboarding flow.


We’ve been getting these messages from the AI every day since June, and we’ve simply been implementing any suggestions that seem reasonable. Takes our engineering team a few hours a week and has definitely contributed to our incredible growth.


This feels insane to us. We have an AI agent autonomously—and proactively—finding novel data insights and telling us how to improve our product. Most of the insights we get are things we wouldn’t have even thought to look up, even if we had a dedicated data analyst on the team.


---


So, it’s been working great for our use case, but even better, we built the agent in a way that it can connect to *any* data sources, and optimize for *any* metrics. That means you can hook it up to your own company data and get the same kinds of results we did on anything you’re trying to optimize for—signups, email open rates, feature adoption, you name it.


Here are a few ways that other companies have been using the agent so far:


- Compiling a weekly executive status report on everything that’s happened across the business
- Analyzing social media performance, including the best-performing content, ideal posting times, and audience growth
- Scanning recent signups for anyone notable or in the company’s ICP
- Tracking marketing spend efficiency and finding the optimal areas to increase spend
- Keeping on top of general KPIs (MRR, active users, retention) and alerting when there are any notable spikes


Overall this has been a pretty wild experiment, but it definitely feels like AI will increasingly become more of a collaborator in product teams, not just on the engineering front, but also in driving product roadmap decisions. Our small team doesn’t include any dedicated data analysts or PMs, and I honestly don’t think we’ll need to hire for those roles any time soon. Our AI agent has so much context and can make correlations across all of our data in a way that would be difficult for even the best human data analysts.


---


Our goal at Basedash is to help companies make better decisions. That started with data visualization and dashboards, progressed to a chat-based AI data analyst, and now we’re building a proactive AI agent that works while you’re asleep.


This feature is still in beta, but you can try it out today by[signing up for Basedash](https://charts.basedash.com/signup) , connecting your business data, and creating a Report. We automatically create one for you when you sign up, but you can tweak the prompt or create a new one from scratch.

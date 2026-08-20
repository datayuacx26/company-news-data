---
schema_version: "1.0.0"
document_id: "63d84eff9629b616cc7a61813148c748306609f3f702fc6c0cc2f03710b708eb"
company_key: "yc-the-context-company"
company: "The Context Company"
source_id: "yc-the-context-company-rss-63dbe5673379"
canonical_url: "https://www.thecontextcompany.com/blog/ai-products-generate-more-signal-than-teams-know-how-to-use"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-28T21:01:08.917383+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:21d358573b8d557f68838b83d821cfcbc4c7051aa51919dd8d75529801c779ef"
---

# You have missed revenue hiding in your agent traces

AI products generate more usable signal than any software before them, yet (almost) nobody is actually using it. I think that's a mistake, and a surprisingly expensive one at that.


## How AI changed the shape of user interaction


For the last twenty years, products were built around indirect signals. A user clicked here, viewed that page, hovered over this button but didn't press it. Teams became really good at inferring user intent and behavior from these breadcrumbs! Unless you were running surveys, this was your only data point for understanding users.


But with AI products, users now interact via natural language. This new form of user input is far more useful. We can read exactly what users want to accomplish, and whether or not they're satisfied.


When you pair this with other signals (thumbs up/down votes, online evals, technical metrics, etc), you get a clear relationship between the performance of your agent and revenue.


But it feels like almost nobody is actually putting the data together. The average team has some basic APM software and agent tracing set up. But when it comes to analyzing user intent, churn signals, retention, and customer satisfaction, the data falls relatively short.


## Why aren't people fixing this today?


Two reasons.


The first and largest reason is that data is really fragmented across teams:


- Engineering owns tracing and technical metrics.
- Product owns user feedback and analytics.
- Customer Success owns churn signals and revenue expansion.


Nobody is actually in charge of analyzing these in tandem (perhaps the only roles I've seen do this well are founders or engineers at extremely early startups, since engineers tend to fill all of these roles at early startups). But if you dig deeper, you'll find that all of this data leads to interlinked relationships between agent performance and revenue.


The second reason is that it simply takes a lot of effort to collect and effectively analyze this data. And if you're not careful, it can get expensive fast.


## What this looks like in practice


When you do put this into practice and truly capitalize on these data points, the relationships become a lot more clear:


> It seems like the average user spends 3 turns asking the agent to rewrite the email in the user's tone. We can probably boost adoption if we tell the agent how to write like a user!


> Wow, 35% of users who encounter problem XYZ tend to never come back! We can probably cut churn if we fix that problem.


> Over 430 users have asked the agent to search the internet, but the agent doesn't have search capabilities yet. We should add this to our product offering!


I've started calling this "holistic observability". More than just tracing your agent, you collect a suite of data points:


- agent traces and conversations
- user votes/feedback (simple thumbs up/down data is enough)
- online evals/monitoring over traces
- topic clustering over user prompts
- sentiment analysis on user prompts (frustration, confusion, etc)


Eventually, signals start to show themselves. The result is clear and actionable insights for teams across the organization. Customer success can clearly identify churn signals and opportunities for revenue expansion in existing accounts. Product teams have a clear understanding of use cases, and which ones are underserved. Engineering teams get insight into real agent performance that extends beyond simple error tracking.


My strongest opinion is that in AI startups, agent observability should not remain an engineering-only concern. CEOs, product leaders, and customer-facing teams should be reading this data, too.


We spent the last twenty years trying to infer what users wanted from clicks. Now they're telling us directly.


It would be strange not to listen.

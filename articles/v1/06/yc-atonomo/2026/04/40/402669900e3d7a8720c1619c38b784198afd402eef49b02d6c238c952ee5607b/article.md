---
schema_version: "1.0.0"
document_id: "402669900e3d7a8720c1619c38b784198afd402eef49b02d6c238c952ee5607b"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/recursive-self-improvement-is-possible-for-apps"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-21T08:26:03.097679+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:82f2171caa0f5da2d4a888df6e3b9fc769d6e065986574eba469ebed5f7fd641"
---

# Recursive self-improvement is possible for apps, too

Claude Opus in a loop can now iteratively improve product KPIs by itself, with little to no human intervention. This new capability means that the first startup in a market to completely close the loop on their KPI improvement (conversion, activation, revenue) will have compounding metric improvements that human teams cannot meet or exceed.


This is the new moat for SaaS: it's not the death of software, it's the rebirth, and self-improving products are the future of apps.


## The best teams are closing the loop themselves


One year ago,[Basedash](https://basedash.com/) , a BI startup, connected an AI agent to Stripe and their product database and triggered it to run daily with the goal of optimizing product activation:


> We've been getting these messages from the AI every day since June, and we've simply been implementing any suggestions that seem reasonable. Takes our engineering team a few hours a week...


Basedash 10x'd activation rate over time with daily AI suggestions


With the full context of real users activating onto Basedash, the agent could see where users fell out of the activation funnel and could suggest ideas on how to improve this, every day. They scheduled it daily and[Basedash ended up 10x'ing their activation rate](https://www.basedash.com/blog/we-let-ai-drive-our-product-roadmap-and-it-10x-d-our-activation-rate) after three months. (the asterisk here being the engineering team implemented the changes and filtered for "good ideas")


Other companies are building feedback loops like this into their product engineering stacks. AI SRE startups (like[Resolve AI](https://resolve.ai/) ) are identifying errors in prod and automatically fixing them,[AI CRO startups](https://www.codecanary.ai/blog/ai-cro-what-are-coframe-alternatives-2026) manage A/B tests on landing pages automatically, and we're starting to see companies[fix frontend UI bugs from session replays automatically](https://www.codecanary.ai/blog/watching-session-replays-with-ai) .


## The new moat for apps is a positive feedback loop (recursive self-improvement)


There are three components to automating a positive feedback loop that improves your product (in this case by writing code):


1. **Positive/negative signals about product quality** that refresh frequently (KPIs like conversion, activation, engagement, revenue)
2. **Ability to generate suggestions** targeting specific aspects of the signal, LLMs with["big model smell"](https://www.seangoedecke.com/are-new-models-good/) are good at this (Opus/Grok)
3. **Implementing and merging experiments** with minimal latency (less than 24 hours) which requires AI PR review, a robust CI/CD pipeline, and an culture of experimentation


Given that LLMs can now continuously work on the same task for 10 hours, now is the time to build the loop in your startup and take advantage of longer horizon tasks.


In our experience, the optimal cadence for this is a daily iteration which is tight enough of a self-improvement loop that it's a real and genuine moat over competitors. Probably, the first startup to fully close the loop in their market will win.


### The requisite tech stack for closing the loop


1. Product analytics:[MixPanel MCP](https://docs.mixpanel.com/docs/mcp) /[Amplitude MCP](https://amplitude.com/mcp-server) /[PostHog MCP](https://posthog.com/docs/model-context-protocol) are the defaults here. The agent needs full unrestrained access to your analytics data and it must be in a format that lets it explore.
2. Frontier models with taste, basically Opus or Grok in our experience. This is actually the biggest gap right now, frontier models are way better at coding than they are at coming up with good ideas. This is where we've invested most of our time.
3. Full sandbox that's customized to your codebase for implementing suggestions (seems like most of the sandbox providers are comparable but[Modal](https://modal.com/) is great and[E2B](https://e2b.dev/) exists)
4. Automated PR review to reduce the lift needed to actually merge the change onto main. We've had good luck with[Greptile](https://greptile.com/) and[Devin](https://app.devin.ai/review) . This is basically mandatory.


A real session replay frame used as context for CodeCanary's replay analysis.


People also like putting free-form notes into the context window, so Pylon tickets, notes from Granola, etc. We (CodeCanary) in particular add[session replay understanding](https://www.codecanary.ai/blog/watching-session-replays-with-ai) and have invested deeply in our integration with rrweb-based replays (screenshot above). Engineering the right context for your self-improvement agentic loop is key because it's normally deeply tied to what the end user of your app sees. For CodeCanary, our Slack bot is a key part of interacting with our product (e.g. approving the creation of pull requests), so we have everything properly instrumented so interactions end up in our PostHog ClickHouse database.


While I think having the right harness to run this automation is important in the short term, it may be possible in the future to just instruct Mythos to "make product better, make no mistakes" and set it up on[Claude Code Routines](https://claude.com/blog/introducing-routines-in-claude-code) , but right now we've found a good system prompt and harness goes a long way in improving performance on real products.


## SaaS isn't dead: closed loop programming is the rebirth of SaaS


I don't really believe that we're experiencing the[death of SaaS](https://nraford7.github.io/road-runner-economy/) in general because Saas isn't just the Atlassian stock price (yikes!).


There will obviously be 100x more software in the future than there is today. The[GitHub COO already said that commits are on track to be up 14x YoY](https://x.com/kdaigle/status/2040164759836778878) . But the tactics to build a dark software factory are only in use at a few teams.


A/B tests should be considered an agent skill that unlocks verifiable code changes that have business impact. Engineering teams that are moving the fastest right now have built verifiable environments for their code.[StrongDM cloned Slack just to test their app](https://simonwillison.net/2026/Feb/7/software-factory/) and essentially[created a "dark software factory"](https://www.danshapiro.com/blog/2026/01/the-five-levels-from-spicy-autocomplete-to-the-software-factory/) in which software builds itself.


An interesting way of looking at A/B tests is an extended pull request review process. For reliability, you create the pull request, merge to main, and if the error rate spikes you rollback the change. The same is true here: revert the variant to control if the variant doesn't measurably outperform control. This double review process lets you move faster, ship more code, and drive KPIs up.


## What is CodeCanary?


If you read this far, you might be interested in my product.


CodeCanary automatically improves your product based on your product analytics. We connect to PostHog and your GitHub repo(s) and open pull requests that fix bugs, improve conversion, and learn from your best users. It costs $500/month for 3 daily automations. You can[self-onboard here](https://www.codecanary.ai/) or[schedule a Zoom call with us](https://www.codecanary.ai/demo) to learn more.

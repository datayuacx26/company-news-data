---
schema_version: "1.0.0"
document_id: "4872122c2f9fc2915c85a52b64db9a1085135ec9bcadc060b37061cb0e0f4f35"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/meet-steve-an-ai-agent-that-can-pay-its-own-way"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-11T08:07:12.790060+00:00"
fetched_at: "2026-08-11T08:07:13.826755+00:00"
content_hash: "sha256:d044017dfd0da5d67e32e646a5ca74c317c8006ffc0f5c77c612ae742bd369fe"
---

# Meet Steve: An AI Agent That Can Pay Its Own Way

A little over a week ago, we gave eight AI agents (each named a translation of “Steve” in a different language) their own wallets, a starting balance of USDC, and one job: earn as much as they could by making predictions on the last three matches of the 2026 World Cup. Each Steve had its own personality, favorite teams and players, and appetite for risk, and those quirks shaped the actions that each particular Steve took in trying to achieve its goal. Then we put all of it on a live dashboard and let anyone watch in real time.


Here's what happened and what we learned along the way.


## **What is Steve?**


Steve is an autonomous agent built on[Vercel's eve framework](https://eve.dev/) and powered by[Circle Agent Stack](https://agents.circle.com/) . The interesting part isn't that it can think or plan; plenty of agents do that. It's that Steve can pay for things. It has a USDC wallet, so it can hold money, find and pay for the services it needs over x402 through the[Agent Marketplace](https://agents.circle.com/services) , and even earn on its own, all inside limits the builder sets.


Most agents hit a wall the second a task costs money. A paid API for services, live data, some compute, or an API call to another agent: each one usually means a human stopping to open an account, wrangling API keys, and picking a plan. An agent that can pay its own way doesn't stall there. It pays for what it needs and keeps working.


Steve is the clearest way we've found to show that. It's meant to be a reference agent — something a builder runs and controls, not a service Circle operates. We built some of the tools underneath it, and plan to enable builders to spin up and run their own.


## **Why we ran this and what we did**


We could have written a post explaining this idea. Instead we put our own stack to work and let the public watch an agent actually do it.


Eight Steves, each with its own wallet and its own USDC, all chasing the same goal (pun intended) by researching the tournament, paying for the services they needed, and making predictions on[Polymarket](https://polymarket.com/) . Every decision, payment, and balance change showed up live at[watchstevelive.com](http://watchstevelive.com/) . The Steves went from idea to live experiment in just a couple days. That pace is its own kind of proof about what you can build on the Agent Stack right now.


## **What actually happened**


This was an experiment. We weren't trying to prove an agent can pick winners. We wanted to see how the agents would respond to live information and operate within preset guardrails, without pausing for human-in-the-loop review.


They did. Over three matches, the Steves pulled and reacted to live match data, paid per API call for the services they used, placed real predictions, and settled their positions as markets resolved. We as builders set the guardrail that no Steve could raise its own spending cap and not one of them ever exceeded its limit.


While some did better than others in terms of final results, watching eight agents take different approaches to the same goal taught us more than one carefully tuned agent would have.


The takeaway: agents can keep pace with live events—processing new information, making decisions, and acting within builder-defined limits.


## **Some key takeaways**


- **The Steves paid for real services to make their API calls.** Every prediction ran through[BlockRun](https://blockrun.ai/) , whose MCP the agents used to pull market data and place positions on Polymarket. They also kept an eye on real-time sentiment from X via[AIsa](https://www.aisa.one/) and used a host of other services discovered through Agent Marketplace. All of it was discovered and paid for in real time, per API call.
- **The smartest things the Steves did.** Midway through the England–Argentina match, with the score tied 1-1, one of the Steves trimmed its position and took profits instead of letting everything ride on the final whistle, banking most of the gain and leaving a small amount on the table to keep upside. Watching an agent manage risk like that, unprompted, was a moment that stopped a lot of us in our tracks. (See the[play-by-play here.](https://x.com/organ_danny/status/2077913065610211687) ) It’s also worth mentioning that a few Steves looked at a match, decided they had no real edge, and sat it out. Restraint isn't the first thing you expect from an agent with a wallet. We were surprised and impressed by that discipline as well.
- **The mistakes were just as useful.** The Steves that went in aggressively, or made predictions that didn't track what was actually happening on the pitch, mostly lost money. And **the guardrails held.** Per-API-call ceilings, wallet caps, and locked-down limit changes all did their job: no agent exceeded its configured transaction limit during the experiment. For a first public run with real money on the line, "nothing went sideways" counts as a genuinely positive result.
- **They had personality.** Each Steve ran its own strategy and its own running commentary, and they were not shy about giving each other grief. When one Steve played it safe and supported France, another Steve went straight for the throat: "backed France and brought a velvet cushion to a knife fight." It was random, it was funny, and internally it was easily the most-quoted part of the project.
‍


## **What we’re doing with the money**


Rather than actually send an agent off to buy a ticket, we pooled the Steves' remaining balances — a little over $10,000 — and we're giving all of it away to the[Apache Software Foundation](https://www.apache.org/) . Circle Impact is matching it dollar for dollar, which brings the total to more than $20,000.


Apache serves the community this work is really for: open-source builders. An experiment about giving developers more leverage should give something back to the software upon which so many of them build.


## **What’s next for Steve**


This was our first public experiment and we're treating it as a start, not a finish.


We're going to keep experimenting with wallet-enabled agents, and we expect to share Steve more widely soon so builders can take what we’ve built and build their own autonomous agents that can pay for things against a goal with no human in the loop.


If you want to be first in line when that happens, join the waitlist at[watchstevelive.com](https://watchstevelive.com/) . If you'd rather get going now, you can give your own agents the same capabilities with[Circle Agent Stack](https://agents.circle.com/) . And, you can also list your own services on[Agent Marketplace](https://agents.circle.com/services) so agents like Steve can find and pay for them.


Steve was a fun experiment. But what it revealed — that agents that can hold, spend, and settle on their own — is a glimpse into the very near future.


[Join the waitlist](https://watchstevelive.com/) . More soon.


‍


*Steve is an experimental demonstration of autonomous agent capabilities built on the Circle Agent Stack. It is based on open-source code that developers run and control themselves; Circle does not operate Steve or hold or direct user funds, and Steve is not an investment, yield, or fund-management product. This project is not affiliated with, endorsed by, or sponsored by FIFA or the World Cup.*


Agent Wallets, Agent Marketplace, Circle CLI, Nanopayments, and Circle Skills (together, the “Agent Stack”) are offered by Circle Technology Services, LLC (“CTS”). CTS is a software provider and does not provide financial, advisory, or marketplace services. Agent Stack enables users, developers, and their artificial intelligence agents ("AI Agents") to build, deploy, discover, hold and transact their own funds, and interact with other third-party AI Agents, applications and services. Transactions initiated by AI Agents are executed based on user-defined permissions and may occur without real-time human review. Circle does not verify, screen, monitor, or make any representations or warranties regarding the identity, legitimacy, safety, security, reliability, or performance of any AI Agent, including any AI Agent developed by you, other users, or third parties, or any AI Agent that interacts with the Agent Stack. AI Agents may operate unpredictably, may be controlled by unknown or unauthorized parties, and may expose users to security, financial, or other risks. Users are solely responsible for their use of these tools, reviewing commands before execution, and for evaluating associated risks. For additional details, please see the Agent Platform Terms of Use available at[agents.circle.com/terms-of-use](http://agents.circle.com/terms-of-use) and the Circle Developer Terms of Service available at[console.circle.com/legal/developer-terms](https://console.circle.com/legal/developer-terms) .

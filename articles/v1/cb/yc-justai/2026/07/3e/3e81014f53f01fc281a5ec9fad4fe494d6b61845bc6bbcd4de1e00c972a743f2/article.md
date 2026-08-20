---
schema_version: "1.0.0"
document_id: "3e81014f53f01fc281a5ec9fad4fe494d6b61845bc6bbcd4de1e00c972a743f2"
company_key: "yc-justai"
company: "JustAI"
source_id: "yc-justai-news-import-e4de3da632ca"
canonical_url: "https://www.getjust.ai/blog/how-we-made-general-purpose-ai-useful-for-marketing"
published_at: null
first_seen_at: "2026-07-25T10:24:54.291702+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:54cd628b7642cddde81ed157b816ab5632ec903b14fb4661eb2b62a21be10d22"
---

# How we made general purpose AI useful for marketing

# How we made general purpose AI useful for marketing


The core problem with using AI for marketing isn't that the models are bad at context. It's that general-purpose models (the kind that power most AI marketing tools) are trained on the internet, not on your customers. Reinforcement learning in marketing is how you close that gap.


OpenAI or Claude can write a perfectly coherent onboarding email. It doesn't know that your SaaS users respond better to plain-text than HTML, that your Tuesday sends outperform Thursdays by 30%, or that your highest-converting upgrade message leads with the feature, not the price. That knowledge lives in your data. A general model doesn't have it.


This is what we built at JustAI: a system that starts with a general-purpose LLM and uses our own purpose-built reinforcement learning engine to make it specific. Here's how it works.


## What reinforcement learning in marketing actually is


Reinforcement learning is a feedback loop. A system tries something, observes what happens, and adjusts. Try again. Observe again. Adjust again.


You've already seen it work. Google's ad bidding system, Netflix's recommendation engine, Spotify's Discover Weekly. All of them use RL in some form. The model doesn't follow fixed instructions. It learns from outcomes.


In marketing, the outcomes are your engagement signals: opens, clicks, conversions, downstream revenue. Every time a customer interacts (or doesn't), that's data. RL uses that data to update its model of what works.


The alternative is what most teams still do. Build a campaign, run an A/B test, wait two weeks, pick a winner, move on. That works at a small scale. Once you're running hundreds of campaigns across dozens of segments, the two-week lag means you're always learning from last month's audience.


## RL vs. A/B testing: what actually changes


Capability


A/B testing


Reinforcement learning


When optimization happens


After the campaign ends


Throughout the campaign


Traffic to losing variants


Equal split until significance


Continuously reduced in real time


Personalization depth


Segment-level


Individual-level


Time to actionable results


Two to four weeks


Hours to days


Depends on team size


Yes, scales with headcount


No. Improves as data grows, independent of team size


## The multi-armed bandit problem in marketing


There's a classic framing in RL called the multi-armed bandit problem. You have five slot machines. You don't know which one pays out the most. How do you figure out which one is best while losing as little money as possible in the process?


The naive answer is to test them equally, run the numbers, and commit to the winner. That's A/B testing. The problem is that every pull on a losing machine is money you didn't have to spend.


The smarter answer is to start pulling all five, but gradually shift more pulls toward whatever's performing best. You never stop exploring (you might be wrong about the winner), but you stop betting heavily on the losers.


In marketing terms: you have five subject lines. A bandit algorithm routes more sends toward the better-performing ones in real time, during the campaign, not after it. By the time the campaign ends, you've sent less traffic to the losers. That difference compounds across every campaign you run.


## How JustAI connects RL to general-purpose models


Here's what most explanations of reinforcement learning applications in marketing skip: the creative still has to come from somewhere.


RL tells you which message works better. Writing the messages is a separate problem, and that's where general-purpose LLMs come in.


The architecture we've built connects these two layers. A general-purpose LLM generates creative variants: subject lines, email body, call-to-action copy. The RL layer runs the experiment, learns from the results, and feeds that signal back to the creative layer to generate better variants next time.


The LLM starts general. The RL loop makes it specific. Over time, the system builds a model of what works for your audience, your product, your brand. What comes out of campaign 20 is meaningfully different from campaign one. The system learned.


We call this LLM Auto-Tune. When performance data signals that existing variants are plateauing, the system generates new options guided by what's already working. It identifies patterns in the winners (tone, structure, emotional framing) and extrapolates from them.


## One version for everyone vs. one version per person


The best subject line for a power user isn't the best for someone on day two of their trial. A single campaign-wide winner misses that.


We run the system two ways depending on what you're trying to do. For broad campaigns, it tracks which variants are winning across your whole audience and continuously shifts traffic toward them. For user-level personalization, it builds a separate model for each type of person, so what gets sent to a long-tenured customer is different from what gets sent to someone who just signed up, and both keep improving independently.


The result is personalization built on users that share behaviours, not segments you built manually.[ClickUp ran this across 200+ segments and saw a +89% lift in create view rates.](https://www.getjust.ai/blog/how-clickup-personalized-lifecycle-across-300-segments-with-justai)[Notion tested over 100 content variations autonomously and got a 96% lift in CTRs.](https://www.getjust.ai/blog/justwords-for-ai-marketing)


## The reward signal: where most of the hard work is


The algorithms we use are well-established. We didn't invent them. What we built is the infrastructure that makes them work in a live marketing context.


The hardest part is telling the system what "winning" actually means. Marketing feedback is noisy. An open is a weak signal. A conversion is strong. A click that doesn't convert is somewhere in between. Where you draw those lines shapes everything the system learns. Getting it right across different industries and campaign types took a lot of iteration.


[Outschool saw a +33% lift in purchase membership rate](https://www.getjust.ai/blog/how-outschool-scaled-personalization-with-multi-agent-ai-decisioning) after running long enough for the system to accumulate meaningful signal.[Lemonade hit double-digit CTOR lifts](https://www.getjust.ai/blog/insurance-at-ai-speed-lemonade-s-growth-with-ai-decisioning) in an industry where timing is everything. In both cases, the first few campaigns were the system calibrating. The gains compounded from there.


## What marketing reinforcement learning looks like in practice


You don't interact with any of this directly. You set a goal (activations, upgrades, retention), connect your data, and the system handles the rest.


The shift is in how you think about learning. With A/B testing, learning is something you do at the end. Run the test, pick the winner, apply it. With RL, the system is improving the whole time, on every send, in every campaign.


The first campaign is the system learning who your customers are. The tenth campaign is the system knowing them.


Teams that run long enough to accumulate signal see compounding returns. That's the real argument for reinforcement learning for marketing: it's not that any single campaign performs dramatically better. It's that the gap between month one and month six is large, and it keeps widening.


## Key takeaways


-


**General AI can write. RL teaches it what to write for your audience.** A general model has no idea what converts for your customers until it starts learning from your data.


-


**RL optimizes during campaigns, not after.** Every send is a data point. The system shifts toward winners in real time rather than waiting weeks for a test to finish.


-


**Defining "winning" matters more than the algorithm.** An open is a weak signal. A conversion is strong. Getting that weighting right is where the real work is.


-


**Results compound.** The first campaign is calibration. The gap between month one and month six is where the lift actually lives.


## Frequently asked questions


### What is reinforcement learning in marketing?


Reinforcement learning in marketing is a method where an AI system tries a marketing action (sending a subject line, choosing a send time, picking a CTA), observes the result (open, click, conversion), and adjusts future decisions based on what happened. The system learns continuously from live campaign data rather than from a fixed historical dataset.


### How is reinforcement learning different from A/B testing?


A/B testing splits traffic evenly and waits weeks for statistical significance before declaring a winner. RL routes more traffic toward better-performing variants in real time, during the campaign. The practical difference is that A/B testing optimizes after a campaign runs; RL optimizes throughout it, reducing traffic to underperforming variants as it goes.


### What data does RL need to work in marketing?


At minimum: goals (opens, clicks, conversions), basic customer attributes (plan tier, lifecycle stage), and channel preference data. Most implementations work well with moderate data volumes once you've defined what a "win" actually looks like for your business.


### What is the multi-armed bandit problem in marketing?


The multi-armed bandit problem describes the challenge every marketer faces: you have several message options, you don't know which one works best, and every send to a losing message is an opportunity cost. Bandit algorithms solve this by continuously shifting sends toward whatever's working, rather than waiting weeks for a test to finish.

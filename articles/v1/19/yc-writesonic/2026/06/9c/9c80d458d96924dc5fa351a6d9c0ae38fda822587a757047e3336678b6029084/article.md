---
schema_version: "1.0.0"
document_id: "9c80d458d96924dc5fa351a6d9c0ae38fda822587a757047e3336678b6029084"
company_key: "yc-writesonic"
company: "Writesonic"
source_id: "yc-writesonic-news-import-0d409124f8fc"
canonical_url: "https://writesonic.com/blog/introducing-ai-ads-index"
published_at: "2026-06-21T10:00:00+00:00"
first_seen_at: "2026-07-22T20:38:42.288608+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:9fa050975f362aeeb49861025cd824e8f57973a40ca45db696aaf82ae0573279"
---

# Introducing the AI Ads Index. The First Public Tracker for ChatGPT Ads.

The[AI Ads Index](https://writesonic.com/ai-ads-index) is live. It's the first public daily index of sponsored ads inside ChatGPT answers. 90 days of history, 1,795 advertisers tracked, 8 AI platforms monitored, refreshed every morning. Free.


We moved fast for one reason. The surface isn't waiting.


OpenAI announced ChatGPT's ad beta on February 9, 2026. For the next three and a half months, almost nothing happened. We watched daily across our customers' tracked prompts. Most days, fewer than five ChatGPT responses out of ten thousand carried a paid placement.


Then on May 26, ChatGPT activated the ad surface. The global rate hit 14.12% on day one, with 47.7% inside the US, on the data provider we had at the time (about 10% of ChatGPT answers covered). One day produced 1,318 ads. OpenAI pulled back briefly, then returned. We[published the study](https://writesonic.com/blog/chatgpt-ads-rollout-study-may-2026) and kept tracking.


In mid-June a second provider switched ad detection on, lifting our coverage to roughly 80% of ChatGPT answers. The numbers since are measured close to directly. Yesterday, June 20, the daily rate sat at 5.99%, 300× the prior week, with 1,795 advertisers running. The shape is stable enough to publish openly.


*ChatGPT ad penetration over the past 30 days. The latest reading is 5.99%, a 300× jump versus the prior week. Source:[Writesonic AI Ads Index](https://writesonic.com/ai-ads-index) .*


Here's what's live today.


## What you can see


Five things, refreshed every morning:


1. **How often ChatGPT answers carry an ad.** Daily share across our global sample. 5.99% on the latest reading (June 20). 7-day, 30-day, and 90-day views all live.
2. **Which brands show up most.** A live leaderboard of advertisers by appearance count. Samsung, Nike, Apple, Dove, and Lenovo currently lead.
3. **Which industries are buying.** Business and productivity software (5.4%) is the top buyer. Specialty retail (3.0%) second. Accounting and tax services (1.4%) third.
4. **Where the ads are running.** 93.4% of observed ads ran on US queries. Canada at 5.0%. Australia at 1.4%. Everywhere else is rounding error.
5. **What a sponsored card actually looks like.** A representative example of the native sponsored format ChatGPT inserts into its answers.


No other AI visibility tool is publishing this surface daily.


*The brand leaderboard updates every morning. Samsung leads with 1,672 appearances, followed by Nike (1,161) and Apple (1,091). Source:[Writesonic AI Ads Index](https://writesonic.com/ai-ads-index) .*


*Left: business and productivity software (5.4%) is the top industry buying ChatGPT ads, followed by specialty retail (3.0%) and accounting and tax services (1.4%). Right: 93.4% of observed ads ran on US queries; Canada at 5.0% and Australia at 1.4%. Source:[Writesonic AI Ads Index](https://writesonic.com/ai-ads-index) .*


## Why now


The transition from organic to paid in AI search is the biggest channel shift since Google AdWords. Most marketing teams cannot see it.


Reports so far have been one-off studies (ours included) or numbers buried inside paid dashboards. There was no live daily ground truth. Every brand we work with asks the same two questions:


"Are ads showing up in my category yet?"


"What does normal look like?"


Both need a market-level view, refreshed often, measured the same way every day. That view didn't exist. So we built it. Publishing it is the obvious next step, and it should never have been a paid product.


## How we measure it


A short note on what's in the data and what isn't.


**Sample.** Roughly 400,000 ChatGPT answers per day, pulled from our customers' prompt panel. The panel leans toward brand visibility and competitive comparison queries, which surface ads more often than purely open-ended prompts. Treat exact figures as indicative. Trust the trend.


**Detection.** Three independent signals: the` ads\[\]` array on the response, creative served from` bzrcdn.openai.com` , and destination URLs tagged` utm_source=chatgpt.com&utm_medium=src` . Any one is enough. We log all three for redundancy.


**What we exclude.** ChatGPT's` inlineProducts\[\]` and` shoppingCards\[\]` arrays. Commercial surfaces, but not unambiguously paid. We'll track those separately when volume justifies it.


**Mid-June coverage change.** Until mid-June, one of our data providers (about 10% of ChatGPT answer coverage) returned ad data. From mid-June, a second provider switched ad detection on, lifting coverage to roughly 80% of ChatGPT answers. The mid-June jump on the chart is partly the coverage change and partly a real expansion in advertiser supply. Both effects are flagged inline on the page.


## What's next


The index tracks ChatGPT today. We monitor eight AI platforms on the same panel: ChatGPT, Perplexity, Claude, Gemini, Microsoft Copilot, Grok, DeepSeek, and Meta AI. Sponsored ads are also live in Google AI Overviews. As ads roll out on any of these surfaces, they'll show up here automatically. Google AI Overviews data lands next.


We're also building a separate index for general AI search visibility, not just paid placements. That one is the next launch after this.


## For brands tracking your own surface


This index is the market view. It shows you the whole category in aggregate.


The brand-specific view lives inside our[AI Ads Tracker](https://writesonic.com/ai-visibility-tracker) , part of Writesonic's AI Visibility platform. That's where you can see your own ad coverage, your competitors' coverage, paid versus organic placements in your category, and the prompts that trigger sponsored slots. We run this for US customers today. The moment OpenAI flips the EU or any other market on, the same tracking carries over.


If you want to see what's showing up in your specific category,[book a demo](https://writesonic.com/?demo=open) .


## A prediction


ChatGPT ads went from a rounding error to 5.99% of all sampled answers in under a month of public rollout, with 1,795 advertisers running. By the end of Q3 2026, I expect the daily rate to settle between 15% and 30% inside the US on commercial categories, and the EU surface to be live and adding another 100,000 commercial queries a day to the pool.


Start watching this surface this quarter and you'll see the auction form in real time. Leave it for next year, and you'll be reading about the new CPCs in someone else's 2027 write-up.


We'll keep publishing.


[See the AI Ads Index →](https://writesonic.com/ai-ads-index)


If you want to track and optimize for both AI search and AI ads inside your own category,[book a call with our team](https://writesonic.com/?demo=open) . We'll walk you through your prompts, your category's paid-vs-organic split, and where your brand can win citations and ad placements before your competitors do.


[Samanyou Garg](https://writesonic.com/blog/author/samanyou-garg)


Founder @ Writesonic


Samanyou is the founder of Writesonic, a platform that helps you track & boost your brand’s visibility in AI search. Two years before the launch of ChatGPT, Writesonic was already at the forefront, helping organizations automate their entire marketing workflow through specialized AI agents for SEO and content. Samanyou is a Forbes 30 Under 30 awardee and a winner of the 2019 Global Undergraduate Awards, often referred to as the junior Nobel Prize.


[Tarsh Swarnkar](https://writesonic.com/blog/author/tarsh-swarnkar)


Data & Infrastructure @ Writesonic


Tarsh builds the data pipelines behind Writesonic's AI visibility research, including the AI Ads Index. His work focuses on real-time measurement of how AI platforms cite, mention, and rank brands.


[Ramish Jamal](https://writesonic.com/blog/author/ramish-jamal)


Engineering @ Writesonic


Ramish builds the infrastructure behind Writesonic's AI visibility platform, including the data systems that power the AI Ads Index. His work focuses on real-time tracking of paid and organic placements across AI answer surfaces.

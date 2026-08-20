---
schema_version: "1.0.0"
document_id: "e8928245924d884b33ffb181c9ab76f0a4052fa9debb2d48b49a4ff913760bd5"
company_key: "yc-gauge"
company: "Gauge"
source_id: "yc-gauge-news-import-b2f5831b5413"
canonical_url: "https://www.withgauge.com/blog/how-posthog-uses-gauge-to-win-aeo/"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-21T21:19:22.794183+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:cb994eb832bad02f54566cb00ad082edc2ae6033ab9d3f7546285b068c7a00bf"
---

# How PostHog uses Gauge to win AEO

*With Natalia Amorim, Content Marketing Manager*


PostHog is the all-in-one developer platform that engineering teams actually reach for, covering everything from product analytics and session replay to feature flags, error tracking, data warehouse, and LLM observability. Their customers are developers and product engineers who don't read whitepapers, don't respond to cold outreach, and definitely don't click banner ads. They ask AI.


PostHog's buyers are already using AI assistants to research and evaluate developer tools. So when Natalia Amorim was asked to lead the company's AEO efforts about a year ago, the strategic case wasn't hard to make. The harder question was: how do you actually do it?


PostHog had 11 products. AI models knew them well for product analytics, but the rest of the suite rarely made it into the conversation.


‍


## **Figuring it out in public**


Natalia had a strong background in SEO, having previously worked at Turo. When PostHog asked her to take on AEO, she first said yes, and then, by her own admission, started figuring out what that actually meant.


She wasn't alone.[Answer Engine Optimization](https://www.withgauge.com/aeo) is very new, and there is no established playbook. A lot of vendors making claims that were hard to verify. Her instinct was to be skeptical of everything, including her own results, and build from there.


That meant starting with the basics: content structure, internal linking, writing that actually answers questions instead of dancing around them. A lot of the foundational work wasn't far from good SEO practice. The meaningful shift was that AEO rewards content that can be lifted and cited by a model mid-answer, not just pages that rank well as a whole.


Once that foundation was in place, the more interesting question became: where specifically were the gaps? Which queries are PostHog missing? Which of their products isn’t being recommended when it should be? That kind of systematic analysis is hard to do manually, and that's where Gauge came in.


## **Why Gauge**


Natalia's criteria for picking a tool were pretty simple. She wanted something that fit her workflow, didn't cost more than the problem warranted, and most importantly, a team she could actually work with.


> *"I went with Gauge for one reason that mattered to me more than anything else: their team builds with us, not at us. When you're early in your journey and still figuring out what you need like I was, a team that actually ships your feature requests (instead of lobbing them into a ticket backlog where dreams go to die) beats a tool with fifty shiny functions you didn't ask for (or don't need yet). I wanted to be treated as a partner, not just an account number on a spreadsheet."*


Since then, Natalia has become an expert user of Gauge, from the core prompt tracking all the way to advanced integrations, its marketing agent, and more.


## **What PostHog built**


With Gauge tracking[prompt visibility](https://www.withgauge.com/features/prompt-tracking) across PostHog's full product suite, Natalia can now see which queries they show up for, which ones they don’t, and what[competitor content](https://www.withgauge.com/features/competitor-analysis) is filling the gap. Gauge’s analysis surfaced who was being cited alongside which specific URLs AI models were pulling from, which gave her data to better prioritize content work. Limited capacity meant every piece written had to be backed by data.


She also built a first-party prompt collection loop that feeds directly into Gauge, and it turned out to be one of the most useful inputs in her entire stack. PostHog added a question to their onboarding flow: when a new user said they'd heard about PostHog through AI, they were asked which prompt they used. To her surprise, people started providing this data.


Three months in, and she’s already collected 6,814 real prompts from real users. Every morning a Slack automation drops the previous day's batch (50 to 100 prompts) and Natalia uses PostHog's own AI tooling to find patterns and feed them back into strategy. It cost almost nothing to set up and has become one of the most grounding data sources from real users. Gauge can deliver its own alerts into Slack the same way; see our docs for[setting up the Slack integration](https://docs.withgauge.com/help/slack-integration) .


On the reporting side, Gauge's[MCP integration](https://www.withgauge.com/blog/gauge-mcp) lets her pull prompt visibility data alongside clicks and impressions from Google Search Console and traffic and conversion data from PostHog itself.


## **Posthog’s results**


PostHog's LLM-referred traffic has grown 41x over 23 months and 7.4x year-over-year. Every quarter since Q2 2024 has been a new record. More importantly, it converts well, better than almost any other channel they run. Someone who arrived via an LLM recommendation came with a specific question already answered. The intent is high.


PostHog now shows up in over 43% of all relevant AI answers tracked, leading their entire industry. They are the most cited domain in the space, ahead of both YouTube and G2. On top of that, they also hold the single most cited article across all tracked queries.


The multi-product visibility gap is still being worked on. Some of PostHog's newer products are getting more traction in AI answers now, while others are still underrepresented. There’s ongoing work, but the infrastructure to see the gaps clearly and close them systematically is in place.


## **Measuring something that doesn't have clean measurement yet**


One thing Natalia is clear-eyed about: AEO isn’t about just a tidy dashboard. Referrer traffic tells a small part of the picture. Self-reported attribution catches more. Prompt visibility tools tell you what models are saying, but only for the prompts you thought to track. Nothing fully reconciles.


Her framing: AEO reporting is a quilt, not a blanket. You stitch together what you have, you're honest about what's data versus inference, and you don't wait for perfect measurement before investing in the channel. The teams that do will just be late.


She's also right about something broader. AI search as a channel is young enough that it's easy to make numbers look good if you're willing to be loose with what you're measuring. Her advice is to be the most honest person in the room, especially when the data is flattering.


## **Where things stand**


A year in, PostHog has a cutting edge AEO operation: a content strategy grounded in real gap data, a feedback loop built on first-party prompt collection, and reporting that pulls from multiple sources into a coherent picture. It didn't start that way. It started with Natalia saying yes to something she hadn't done before and building the muscle from scratch.


Gauge sits at the center of it all. Not as a magic bullet, but as the tool that made the analysis tractable and the reporting real.


## **Key takeaways**


**Partial visibility is a real growth problem.** Being known for some of your products while others are invisible to AI models creates a ceiling in entire verticals. You need to know where the gaps are before you can close them.


**First-party prompt data is underrated.** Asking users which prompt led them to you is cheap to set up and produces more useful signal than most third-party sources. PostHog collected nearly 7,000 real prompts in three months just by asking.


**The right tool fits how you actually work.** In a channel this new, a vendor that ships your feature requests and treats you like a partner matters more than one that arrives with the most features.


**Honest measurement is part of the strategy.** AEO is full of easy ways to make results look better than they are. Teams that stay rigorous about what they're actually measuring build on something solid.


**The channel rewards showing up consistently.** LLM recommendations compound. Every mention reinforces the association between PostHog and the categories they compete in, and for a developer audience already living in these tools, that adds up.


*Talk to Gauge*


*Thank you to Natalia for sharing PostHog's story.*


‍

---
schema_version: "1.0.0"
document_id: "5b325a73a0c6584b8c1e1337109cc73ef28a80df7d561c8c2fc527b2c17a6c99"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/watching-session-replays-with-ai"
published_at: "2026-01-24T00:00:00+00:00"
first_seen_at: "2026-07-21T08:26:03.097679+00:00"
fetched_at: "2026-07-28T22:23:12.089184+00:00"
content_hash: "sha256:58129e9766483a02bbf9dab9a07b41e51a8ba2be254c993597246f2d02184f4a"
---

# Watching session replays with AI / LLMs

While building[CodeCanary](https://codecanary.ai/) I've come to understand both the pains of watching session replays, converting those hours into positive ROI, and ultimately **how we automated watching session replays using LLMs** . In this article I'm going to cover everything.


Session recordings are a good way to watch how users use your app, what their goals are, and where they struggle. PMs often **obsess over session replays of users that churn** from the funnel to see where their mouse hovers occur, what they're looking at, consider how long they dwell at a CTA or even see what they type into search or AI fields and see what results they get out.


## Two pains of watching session replays


I watched probably around 50+ hours of session replays while building previous products to learn these things about my users. By doing so, I learned that:


1.


Watching replays is monotonous and requires attention to detail — when a user asks an AI a technical question and gets an answer, a good PM would consider *how good* the answer was and if the user should be satisfied. If the user leaves after reading the answer, it's a **red flag** that answer quality is **negatively impacting retention** .


2.


High volumes of session replays lead to **filtering what replays to watch** and **sampling how many replays to save** . This requires PMs to know *which replays to watch* and results in many *churned users' recordings* going uncollected.


I[started CodeCanary](https://www.codecanary.ai/blog/codecanary-is-a-ycombinator-funded-company) not because this was painful — instead, I realized that using product analytics data to[automatically improve products is a feedback loop](https://www.codecanary.ai/blog/recursive-self-improvement-is-possible-for-apps) any PM or founder would dream of.


(We actually[use CodeCanary to improve itself](https://www.codecanary.ai/blog/codecanary-uses-codecanary-to-improve-itself) , which is really cool!)


### Can LLMs actually watch session replays?


You can use[rrvideo (the MP4 renderer for rrweb )](https://github.com/rrweb-io/rrvideo) to convert the JSON` rrweb` data, which represents the DOM over time and different user-triggered events in linear timestamps, into a video that a couple of LLMs support (Gemini traditionally being regarded as having the best video processing and understanding).


For[Claude you need to pass individual frames](https://platform.claude.com/docs/en/build-with-claude/vision) , and you need to[do the same with OpenAI's models](https://platform.openai.com/docs/api-reference/chat) . So basically you need to use Gemini.


This is the simplest way to ingest session replays into AI/LLMs: render the user session into a video and include it in the context window. Then, you can ask questions like:


- Are there friction points in the user's journey?
- What parts of this page is the user most focused on?


> One fun tip is to use the[Google AI Studio](https://aistudio.google.com/) to do this, which is free.


The more intelligent and balanced the LLM is, the better the results tend to be here. Among the latest generation of SOTA AI models (Claude Opus 4.5, GPT 5.2, and Gemini 3 Pro as of January 2026), I would argue that Gemini has the best pretraining ("well-roundedness") while OpenAI and Anthropic have the best critical thinking and math skills ("IQ").


### Is "AI watching session replays" just AI slop?


There's a *lot* of slop that LLMs generate when you ask them to review session replays. They love to explain what a user is doing, or point out the website's text, or which page the user navigates to.


The best way to create value from AIs watching session replays is by:


1. De-duplicate insights across the entire application
2. Ground each insight in statistics backed by the product analytics stack
3. Sort insights by their potential impact on important business metrics


In a sense, *this* is what[CodeCanary](https://codecanary.ai/) is doing.


## Grounding session recordings in product analytics stats


CodeCanary builds a statistical model that mimics your application, from your top of funnel to checkout and onwards through retention. Based on the trailing six months of analytics data, we calculate how often users of various cohorts (e.g. American iPhones) pass through your application.


With this statistical model — which is basically a "digital twin" of your application — we can answer specific questions like "How many users drop out of the checkout funnel after clicking in the header or footer?" and even consider counterfactuals.


For example, one consideration PMs sometimes forget is the relationship between **your cookie banner and conversion** . Many high converting websites choose to hide the cookie banner in key parts of the flow.


When CodeCanary sees a potential issue like that in a session replay, it first asks: what percentage of users haven't interacted with the cookie banner before checkout? Then, what is the difference in conversion rate between those who interact and those who don't?


This gives us an upper bound on the potential lift associated with hiding the cookie banner in this part of the funnel. CodeCanary only raises insights where the potential lift is significant enough to move an important KPI.


## Which startups are using AI to watch session replays?


In general, there aren't a lot of companies that are trying to do this.


There's an interesting open source project called[Providence](https://providence-replay.github.io/) that may be doing something similar:


> Providence enhances the value of session replay through AI while significantly mitigating the time investment needed to draw meaningful conclusions from user sessions.


One YC startup,[Decipher AI](https://getdecipher.com/) uses LLMs to watch session replays for QA/QC:


> Decipher AI offers an AI agent that uses advanced vision LMs to watch thousands of hours of session replays for you so you can discover customer issues in realtime, understand feature usage, and get product questions answered.


Another YCombinator-backed startup,[PathPilot](https://www.producthunt.com/products/pathpilot) launched a product like this in 2024:


> PathPilot is an AI tool that transforms user session replays into "highlight reels," helping you spot critical moments from thousands of hours of recordings. Save time, uncover insights, and take data-driven action to boost customer retention.


Beyond these, I haven't really found any startups taking this approach very seriously.


The larger session replay incumbents (Clarity, FullStory) only have a couple of public reviews on Reddit:


> **u/fukofukofuko** (2y ago)
>
>
> MS Clarity does that, I haven't used the AI feature enough to say whether it's good or not though.


> **u/Unlikely-Ad210** (2y ago)
>
>
> I use a tool called FullStory that kind of does something like this. We can easily see issues without having to watch every session. However we can dig in to each session if we wanted to.
>
>
> Again, it doesn't summarize each session but it's very good at telling us when cohorts of users are experiencing the same issue.


> **u/foolishtactician** (2y ago)
>
>
> I use Descript to generate transcripts of my user interviews. It doesn't summarize them, but it's much easier to look back at a transcript than to rewatch the entire recording.

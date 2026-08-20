---
schema_version: "1.0.0"
document_id: "49a5b7d28400584c89588f29b5d261a5f79ea544655e579e5402b23a138a3ec9"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/ai-taking-from-publishers-heres-how-they-can-fix-it/"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:a41f8ed4875985dab018fe611cbca82c758b88aac5fc287048c072c259c475d7"
---

# AI is Taking From Publishers! Here’s How They Can Fix It

Digital publishers face a threat unlike the ones before it. Search disruption, social media fragmentation, the collapse of programmatic advertising - those were battles over attention. AI, and the consequences it brings, is a battle over something deeper: the ownership of value.


AI systems can ingest an article, summarize it, and answer user questions directly - no click, no visit, no ad or relationship formed. The content does the work, and in almost all cases, the publisher sees none of the return.


The industry has spent years adapting. Changing revenue models, changing platforms, and the addition of rich media, just to name a few. What's needed now isn't adaptation - it's a new architecture. One that gives publishers real control over how their content is accessed, protected, monetized, and measured.


## Why Is the Traditional Publisher-Search Engine Agreement Breaking?


For decades, search engines operated on an implicit bargain. Crawlers ingested content, indexed it, and sent users back to the source. Publishers tolerated the scraping because the increased traffic and exposure justified it. It wasn't a perfect deal, but there were mutual benefits.


Today, AI breaks that.


The new generation of AI crawlers doesn't index content to drive discovery - they ingest to generate answers. When a user asks an AI-powered app a news-related question and gets a direct response, the publisher's article did the work. The publisher saw none of the return. No visit. No impression. No subscription prompt, and often no citation of the source. In short, no relationship was formed.


The damage compounds in three directions:


1.


**Content is scraped without consent or compensation.** Unlike traditional search bots operating under established conventions like robots.txt, many AI crawlers operate in far murkier territory - harvesting content at a significant scale with no licensing agreement, no attribution requirement, and no traffic in return.


2.


**AI answers are eroding referral traffic and audience ownership.** When a search engine or AI assistant answers a query directly, the incentive to click disappears. For publishers whose audience relationships - and advertiser value - depend on owned traffic, this is an existential pressure, not a temporary dip.


3.


**Bot traffic inflates infrastructure costs without producing revenue.** AI crawlers generate substantial server load. They consume bandwidth, trigger CDN costs, and strain origin infrastructure. But unlike a human visitor, they generate no pageviews, ad impressions, or subscription conversions.


Publishers are, in effect, subsidizing the training and operation of systems that compete with them - paying the infrastructure bill for their own disruption. The numbers tell a fairly unforgiving story: higher costs, lower yield, and a sharply worsening curve.


## How Can Publishers Control Content Access and Monetization in the Age of AI?


[Blocking every bot](https://www.fastly.com/products/bot-management) is not the answer. Some AI visibility may matter - being cited by a large language model or surfaced in an AI-powered search experience could carry real brand value, particularly for publishers whose authority depends on being recognized as a primary source. The problem isn't, in principle, AI access. The problem is AI access without visibility, compensation, or choice. Publishers don't need a kill switch. They need a control model.


This is where new approaches get more sophisticated. Rather than simply blocking unverifiable or malicious bots, publishers can deceive them at scale - impacting attacker economies while mitigating the damage they cause. Fastly Deception's capabilities are patent-pending and continue to grow. The first use case targets account takeover:[Next-Gen WAF](https://www.fastly.com/products/web-application-api-protection) customers can choose the Deception for ATO action, which automatically swaps attacker-submitted credentials with randomized values, invalidating attempts and wasting attackers' time and money while limiting their ability to retool.


Much like spam nearly broke email because it was so cheap to send hundreds of thousands of messages, the economics don't initially seem to favor publishers. After all, it's much cheaper to scrape content than it is to produce and serve it.


Some of our customers are flipping that equation by introducing proof-of-work interstitials that burn through computing resources. Trivial for individual users but ruinously expensive for bad actors trawling thousands of sites.


Others generate infinite content mazes or produce plausible but nonsensical data - effectively poisoning the well and ruining the training data. The clever thing about this particular technique is that it won't be discovered until they've finished their model and found that it's full of garbage.


The result is a frustrated attacker who burned resources on an attack that was never going to succeed - and a secure experience for legitimate users. Applied to content extraction, the same logic holds: serve suspicious crawlers degraded or misleading responses instead of blocking them outright, wasting their resources without signaling detection. It's a more durable deterrent than a hard block.


Deception is one tool. But a complete control model requires knowing which crawlers deserve access, which should be stopped, and which represent a commercial opportunity worth capturing.


**Allow trusted bots** : Not all[crawlers](https://www.fastly.com/learning/bots/what-is-a-web-crawler) are adversarial. Search indexers, licensed aggregators, and credentialed AI partners operating under clear terms can and should be permitted access. The key word is trusted - verified identity, known purpose, agreed terms.


**Block abusive bots:** Crawlers that harvest content at scale without authorization, ignore robots.txt conventions, or mask their identity to avoid detection should be stopped at the edge before they consume resources or extract value.


**Charge commercial**[AI crawlers](https://www.fastly.com/learning/what-are-ai-crawlers) **:** If an AI system is ingesting publisher content for commercial purposes - training a model, powering a product, generating answers that replace visits - that use has economic value. Fastly partners with companies such as[TollBit](https://tollbit.com/) , which has built infrastructure to make that exchange real: a metering and payment layer that allows publishers to set terms for commercial crawlers and collect compensation when those terms are met. Content access becomes a revenue event, not just a cost center.


**Measure AI citation and content usage:** Publishers can't manage what they can't see.[ScalePost](https://www.scalepost.ai/) , also a Fastly partner, provides the visibility layer - tracking when and how publisher content is cited, surfaced, or used by AI systems, and giving publishers the data to understand their footprint across the AI ecosystem. That intelligence informs every other decision in the framework. You can't set fair terms for access you can't see, and you can't enforce limits you're not tracking.


## Why Edge Infrastructure Is the Key to Publisher Control


The publishers who successfully navigate AI won't be the ones who lock everything down. They'll be the ones who built the infrastructure to make informed, real-time decisions about access - who gets in, on what terms, at what cost, and with what visibility.


That means treating the edge not just as a delivery layer, but as an enforcement layer. Bot management,[deception](https://www.fastly.com/blog/make-attackers-cry-outsmart-them-with-deception) , licensing infrastructure, and content tracking aren't separate products solving separate problems. They're pieces of a single answer to a single question: who actually controls the value in what you publish?


Right now, for most publishers, the answer isn't them. That doesn't have to be permanent.


The shift to AI doesn't have to mean losing control of your content. Discover how Fastly gives you the edge you need to protect, monetize, and measure your value in the age of AI.[Explore our Digital Publishing solutions](https://www.fastly.com/solutions/digital-publishing) .

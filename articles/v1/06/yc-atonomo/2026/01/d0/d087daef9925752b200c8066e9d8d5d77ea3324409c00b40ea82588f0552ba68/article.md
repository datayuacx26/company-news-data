---
schema_version: "1.0.0"
document_id: "d087daef9925752b200c8066e9d8d5d77ea3324409c00b40ea82588f0552ba68"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/poorly-configured-exposure-criteria-with-posthog-experiments"
published_at: "2026-01-23T00:00:00+00:00"
first_seen_at: "2026-07-24T08:30:33.811787+00:00"
fetched_at: "2026-07-28T22:23:13.149633+00:00"
content_hash: "sha256:afeb54abe4b58d467b591aada1b01cd64e70a6290e55fd5741ba10b4181e3b54"
---

# Poorly configured exposure criteria with server-side PostHog experiments

There are two main ways to implement an A/B test: server-side and client-side. Most engineering teams opt to implement experiments server-side because it:


1. eliminates any[content flashes ("FOUC")](https://posthog.com/tutorials/prevent-fouc-ab-tests) or delays in displaying content, which hurts page speed scores like the First Contentful Paint
2. doesn't require any client-side JavaScript to run the experiment


The main advantage of client-side experiments is that they can be implemented without code (especially via PostHog's new[no-code web experiments](https://posthog.com/docs/experiments/no-code-web-experiments) , which are still in beta), meaning marketing teams can bypass the engineering team and execute experiments themselves.


### Server-side experiments should have custom exposure criteria


While CodeCanary recommends server-side experiments generally when possible ([CodeCanary delivers A/B tests with server-side experiments in pull requests](https://codecanary.ai/) ), server-side experiments generally require you to manually set the exposure criteria to avoid including bots in your experiment.


This is especially true if your primary endpoint is measured on the client side, with the[JS SDK](https://posthog.com/docs/references/posthog-js) . When you count all exposures, you're including crawlers and bots in your experiment. This dilutes the effect size because crawlers and bots are far less likely to "convert", especially if the conversion event is measured on the client side.


### What this looks like in a real-world A/B test


This recently impacted one of the experiments we were running for a customer. Because we hadn't[customized the exposure settings](https://posthog.com/docs/experiments/exposures) , the experiment was diluted by 80% crawlers and bots, and only 20% real traffic.


This inflated the time to statistical significance and reduced the actual win probability. Once we amended the exposure criteria, a 23% uplift was revealed in a core part of their funnel.


When you dilute your actual experiment effect by 4-5x by including bots, the statistical significance calculation is off because it brings the conversion rates of all of the arms closer together (and closer to 0%). To get statistical significance faster, the goal is to maximize the difference between the arms.


We ended up using the` $pageview` event as the exposure, conditioned on the path name and the feature flag property being either assigned to` control` or` variant` .


### Downside of client-side exposure criteria


The main downside of using the PostHog JS SDK to set exposure is that it doesn't account for fast bounces. If your server-side experiment arms can impact the bounce rate before the JS SDK fully loads and sends the first` $pageview` event, you're losing that information.


As[an engineer implementing A/B tests](https://mikebifulco.com/posts/ab-testing-with-posthog-to-fix-conversions) , you need to consider this potential problem before deciding to reset the exposure criteria to use the server-side or client-side exposure events.


One great example of the delicateness here is[Alexey Komissarouk's article about an Opendoor A/B test](https://alexeymk.com/2020/07/14/lies-damn-list-and-front-end-tracking) , where they improved load performance, and their A/B test metrics actually indicated performance was worse! This was simply because the frontend SDK ended up loading *faster* , meaning they stopped underreporting the bounce rate:


> When the homepage loads, the front-end tracking code records a 'page view' event. If the 'page view' event was recorded, but then nothing else happens, analytics will consider that user to have "bounced".


> It turned out that the old site was so slow that many folks left before their 'page view' ever got recorded. In other words, the old site was dramatically under-reporting bounces.

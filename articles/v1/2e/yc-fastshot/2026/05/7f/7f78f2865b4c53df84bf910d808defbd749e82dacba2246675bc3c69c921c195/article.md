---
schema_version: "1.0.0"
document_id: "7f78f2865b4c53df84bf910d808defbd749e82dacba2246675bc3c69c921c195"
company_key: "yc-fastshot"
company: "Fastshot"
source_id: "yc-fastshot-news-import-c7e54c9ff724"
canonical_url: "https://fastshot.ai/blog/b2b-saas-founder-ships-mobile-app-in-3-hours"
published_at: "2026-05-16T00:00:00+00:00"
first_seen_at: "2026-07-24T07:48:17.937433+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:f4a363a8949284b994bed496c98502a6853d0ff2889ed927e06bd3d00860e327"
---

# How a B2B SaaS Founder Shipped a Consumer Mobile App in 3 Hours

[SKS Nutrition](https://www.linkedin.com/in/ke-wang-b8084b105/) is a B2B AI SaaS platform serving the food, beverage, and ingredients industry — helping brands accelerate product innovation through data and AI. The kind of company that talks to formulation scientists and brand managers, not end consumers. Until April 12, 2026, SKS had no consumer-facing mobile experience. By the end of that afternoon, they did — and it had taken 2nd place at the[Stanford × DeepMind Hackathon](https://luma.com/ag8lks10) .


The build window was three hours. The product was a working consumer-facing iOS and Android app called **SmartCart** . The hackathon ran two parallel tracks — Google AI Studio's Gemini codegen for web prototypes and Fastshot for native mobile apps — and the top 6 teams across both tracks pitched live to a judging panel of 35+ VCs from NFX Bio, Felicis, SOSV, Threshold Ventures, Insight Partners, Mighty Capital, and others, plus Stanford faculty and Google DeepMind product leadership. SmartCart placed second out of 150+ prototypes built during the same afternoon.


For[Ke Wang](https://www.linkedin.com/in/ke-wang-b8084b105/) , founder of SKS Nutrition, it was her first hackathon.


[🛒 Try SKS SmartCart live →](https://fastshot.ai/gallery/45930a1e-a481-4570-bd44-b37bb660c99c)


[▶️ Watch Ke's demo →](https://youtube.com/shorts/Nt7A-7GUAy4)


## What SmartCart is — and why a B2B company built it


Ke's pitch in her own words:


> *"SKS AI addresses consumer 'decision fatigue' in grocery stores by providing a data-driven, personalized shopping assistant. The app, SmartCart, uses Computer Vision and Geofencing to align every purchase with the user's specific health and wellness goals."*


The mechanism: open the app in a grocery store, and SmartCart's geofencing recognizes which store you're in. Use the camera to scan products on shelves, and computer vision identifies them. Behind both inputs sits SKS Nutrition's existing food and ingredient intelligence — the same data and models the company already licenses to food and beverage brands. SmartCart turns that B2B intelligence into a direct consumer experience that helps shoppers buy in line with their health goals.


Strategically, this is one of the more interesting moves an AI SaaS company can make. SKS has been collecting and modeling food data for the B2B side of the business. That data has value to consumers too — anyone trying to make smarter grocery decisions could benefit from it. The bottleneck has never been the data. The bottleneck has been the channel: how do you put B2B intelligence in front of consumers without building a consumer software team, hiring mobile developers, or putting a 6-month engineering project on the roadmap?


SmartCart is the answer. A consumer mobile app on top of an existing B2B data platform. Same intelligence, new channel, no engineering team required.


## The business math, plainly


Adding a consumer-facing mobile channel to an existing B2B SaaS platform is a real strategic decision that most companies in this position avoid — not because they don't want the channel, but because the cost of opening it is too high. The traditional path: hire a mobile engineering team, design a consumer experience from scratch, build the iOS and Android apps in parallel, navigate App Store and Play Store submission, then operate the new channel ongoing. The first version is typically a quarterly initiative at minimum, often a half-year commitment. Most B2B SaaS founders look at the math and decide to wait.


In Ke's case the math collapsed:


Resource Traditional path What actually happened


Time to first usable version 3-6 months 3 hours


Engineering team required 2-4 mobile engineers Zero new hires


Cash spent before validation $50k-$200k $0


Number of consumer-facing iterations possible per day <1 ~20


Risk profile if the channel doesn't work Significant write-down Negligible


That's not a marginal improvement. That's a different decision. When the cost of opening a new channel falls by two orders of magnitude, founders stop asking "should we invest in this?" and start asking "why wouldn't we try it?" — which is approximately what happened at the hackathon. SKS went from "no consumer mobile" to "winning 2nd place in a consumer mobile app competition judged by 35+ VCs" in the space of an afternoon.


> *"1st in Semis, 2nd in Finals — not bad for our first hackathon! Today was a whirlwind at the Stanford University x Google DeepMind Hackathon. I'm so proud of team SKS Nutrition for our performance and for the chance to present our new features on the big stage."*
>
>
> — Ke Wang ([on LinkedIn](https://www.linkedin.com/in/ke-wang-b8084b105/) , April 13, 2026)


## What changed in the underlying tooling


The mechanism that made this possible:[Fastshot](https://fastshot.ai/) is an[AI mobile app builder](https://fastshot.ai/best-ai-app-builder) that generates production-grade React Native code from natural-language descriptions. You describe what you want the app to do, the AI produces a working version, you preview it on a real device, you iterate by chatting. The output is real native source code that compiles to iOS and Android binaries — not a vendor runtime, not a web app wrapper. Source code that an engineering team could continue to develop independently if SKS chose to bring on dedicated mobile staff later.


For an established business, the source-ownership detail matters more than it does for a hackathon project. If SmartCart works and SKS wants to take it to market seriously, the codebase from the 3-hour build is the foundation they'd continue building on. There's no rebuild step, no migration off a proprietary platform, no vendor lock-in. The app that won 2nd place at Stanford is the app that ships to consumers — if and when SKS decides to push it that direction.


That's the underlying shift that makes Ke's story relevant to other B2B SaaS founders looking at a similar opportunity. It's not just that the hackathon prototype came together fast. It's that the prototype is real software, with a real path to production, owned by SKS rather than rented from a vendor.


## What this means for other B2B SaaS companies (and DTC brands)


Every B2B SaaS company in 2026 should be asking itself: do we have data, intelligence, or expertise that consumers would value, that we could deliver through a mobile channel? For most, the honest answer is yes — and the channel hasn't been opened because the engineering cost was prohibitive. That cost has changed.


The pattern is especially relevant for[DTC and ecommerce-adjacent companies](https://fastshot.ai/ecommerce) , where the consumer mobile surface has direct revenue implications. A nutrition data company can build SmartCart in three hours; a Shopify store can build a[retention and loyalty mobile app in similar time](https://fastshot.ai/shopping-app) ; a B2B logistics company can build a customer-facing tracking app over a long lunch. The decision math that used to land at "we'll build a mobile app in 2027" now lands at "let's prototype this on Wednesday."


For DTC brands specifically, the framing flips from "should we hire Tapcart or build something custom?" to "what does our own AI-built mobile app look like?" The same intelligence that lives in a B2B platform's data model lives in a DTC brand's customer data — and the mobile channel for that data has gotten cheap enough that not opening it requires explanation.


## The same hackathon, the same tool, a different kind of winner


SKS wasn't the only winner using this approach at the hackathon. The 1st place app — PinkedIn — came from a solo game designer with no engineering background.[That story is here →](https://fastshot.ai/blog/game-designer-wins-stanford-deepmind-hackathon-no-code)


The 6th place team (LY-Links / MoodFeed) also built on Fastshot. Three of the top six placements on the same tool, used by three completely different kinds of builder: a solo designer, an established SaaS founder, and a two-person team with a new social platform concept. Same tool, same time budget, three very different starting points — all converging on working mobile apps shipped in 3 hours.


## Build your consumer channel before your next board meeting


If you've been sitting on a mobile channel for your business, the experiment is no longer expensive. Open[SKS SmartCart](https://fastshot.ai/gallery/45930a1e-a481-4570-bd44-b37bb660c99c) and see what 3 hours of focused work produces. Then describe what your own version would look like.


The new question isn't "can we afford to try this?" The new question is "what's the smallest meaningful version we could ship by Friday?"


→[Start building with Fastshot](https://fastshot.ai/)


For the deeper picture —[the 10 AI app builders compared by engineers](https://fastshot.ai/best-ai-app-builder) ,[the full how-to guide for AI mobile app building](https://fastshot.ai/builder-mobile-apps) , or[the technical breakdown for engineering teams evaluating the category](https://fastshot.ai/react-native-builder) — the linked deep-dives walk through the workflow in more depth.

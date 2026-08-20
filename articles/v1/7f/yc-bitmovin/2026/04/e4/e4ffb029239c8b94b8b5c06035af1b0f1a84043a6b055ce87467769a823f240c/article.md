---
schema_version: "1.0.0"
document_id: "e4ffb029239c8b94b8b5c06035af1b0f1a84043a6b055ce87467769a823f240c"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-1c765b973c81"
canonical_url: "https://bitmovin.com/blog/ai-ad-placement-score-scene-level-streaming/"
published_at: "2026-04-16T17:03:44+00:00"
first_seen_at: "2026-08-18T01:28:54.087447+00:00"
fetched_at: "2026-08-18T01:28:35.749916+00:00"
content_hash: "sha256:0cb993c6ca77ebfd4ba9d63e3f6d9c09e3ea3035e318c540a5fc077ed9745c0f"
---

# Improving ad placement with AI for better viewer experiences

## TL;DR


- Most ad placement today is built around availability: a scene boundary exists, so an ad goes there, with no regard for whether it’s actually a good moment.
- Bitmovin’s AI Scene Analysis (AISA) adds scene-level context including sentiment, topic classification, pacing, and tension signals (called Dynamics) to every scene in a piece of content.
- A new Ad Opportunity Score evaluates each scene boundary, considering both the current and following scene’s context, pacing, and tension, and outputs a 0-1 score plus human-readable reasoning explaining why a boundary is or isn’t suitable.
- This enables a semi-automated ad scheduling workflow pre-integrated with Bitmovin’s VOD Encoder, where cue points and keyframes are automatically placed at scene boundaries, and teams use the score and reasoning to apply editorial judgment at scale.


---


## Table of Contents


You’re watching a key moment in a show. The tension is building, the scene is about to peak, and just as it does, the video cuts to an ad. It’s a familiar experience, and one that immediately breaks immersion, not because the ad exists, but because of where it appears.


As ad-supported streaming continues to grow, across AVOD, FAST, and hybrid subscription models with ad tiers, expectations have shifted. It’s no longer enough to maximize fill rates, impressions need to perform. The challenge is that most ad placement logic today is built around availability, not suitability or context. A scene boundary exists, so an ad can go there, but “can” and “should” are very different things.


Closing that gap requires a deeper understanding of what is happening within each scene, not just where boundaries exist. In this blog, we explore how AI Scene Analysis (AISA) helps address this by providing deep, structured metadata for every scene in a piece of content, including scene boundaries, topic classification, sentiment, and IAB 3.0 categories. We also look at how new signals, such as **Dynamics** and an **Ad Opportunity Score** , build on this foundation to enable more informed ad placement decisions.


## **What’s New**


To move beyond simple scene boundaries, it’s not enough to know where a scene changes, you need to understand how it behaves. Every scene in AISA now includes a dynamics object that captures the pacing and tension of the scene:


> “dynamics”: {
>
>
> “tension”: “HIGH”,
>
>
> “pacing”: “MEASURED”
>
>
> }


These signals are difficult to quantify at scale, but they directly influence how ad breaks are perceived within a stream. Dropping an ad into a high-tension, fast-paced sequence feels jarring, while the same placement at a natural transition is far less disruptive.


Building on these dynamics signals, each scene boundary is also assigned an Ad Opportunity Score:


> “adOpportunityInformation”: {
>
>
> “reason”: “Location and topic shift, but tension remains moderate to high.”,
>
>
> “score”: 0.4
>
>
> }


The score considers both the current scene and the one that follows, evaluating their contextual relationship alongside pacing and tension to determine whether a boundary is suitable for an ad break. Critically, it also surfaces the reasoning behind the score, giving teams visibility into why a boundary is suitable, not just how it is scored.


## **Semi-Automated Ad Scheduling Workflow**


The Ad Opportunity Score is designed to work within existing encoding workflows. With the pre-integration with Bitmovin’s VOD Encoder, cue points and keyframes can be added at every scene boundary. Combined with the Ad Opportunity Score and its reasoning, this gives streaming platforms a new semi-automated ad scheduling workflow with seamless transitions during playback.


**Ad opportunity placement score visual**


Rather than manually reviewing content to decide where breaks should go, teams can use the Ad Opportunity Score to surface the best candidate boundaries, review the reasoning to apply their own editorial judgment, and rely on the existing cue points and keyframes to handle the technical execution, giving them both the contextual data and the delivery mechanism within the same workflow.


## **Where We’re Heading**


Our goal is to enable fully automated ad placement, where a publisher can define the number of ad breaks in a piece of content and have the system identify the best positions for them, based on scene boundaries, contextual metadata such as sentiment and dynamics, and Ad Opportunity Scores.


This moves ad scheduling from a manual, time-intensive process to something that can scale across a huge content library, consistently and reliably, without requiring teams to review every piece of content they publish. However, automated placement decisions need to reflect how publishers and ad operations teams approach this problem in practice. This varies depending on:


- Genre
- Format
- Audience
- Platform


Sports content has different decisions compared to a drama series, for example. It also depends on the intended ad experience and when subscriber data is taken into account, platforms can differentiate ad placement and pacing between free and premium viewers.


## **Get Involved**


We’re working closely with streaming platforms to refine both the scoring model and the reasoning behind it, building towards a fully automated ad placement workflow that reflects the nuance experienced teams bring to these decisions today.


If you want to improve your VOD workflow and enhance viewer experiences,[Get access with 10 free hours every month](https://bit.ly/Ad_opportunity_blog) to explore how AISA works within your environment on Bitmovin’s VOD Encoder.


---


## FAQs


### What is Bitmovin’s AI Scene Analysis (AISA)?


AISA is Bitmovin’s AI-powered content analysis product that generates deep, structured metadata for every scene in a piece of video content. This includes scene boundaries, topic classification, sentiment, IAB 3.0 categories, and now two new signals — Dynamics (pacing and tension) and an Ad Opportunity Score — that together enable more context-aware ad placement decisions.


### What are Dynamics signals in AISA?


Dynamics is a new per-scene metadata object that captures two attributes: tension (e.g., HIGH, LOW) and pacing (e.g., FAST, MEASURED). These signals quantify how a scene feels in motion, information that is very difficult to produce at scale manually but directly affects how disruptive an ad break will feel to a viewer in that moment.


### What is the Ad Opportunity Score in AISA?


The Ad Opportunity Score is a 0-1 numerical rating assigned to each scene boundary that indicates how suitable that moment is for an ad break. It evaluates both the current scene and the one that follows, weighing their contextual relationship alongside pacing and tension. Crucially, it also returns a plain-language explanation of why a boundary scored the way it did, giving editorial teams the context they need to apply their own judgment.


### How can I try AISA for my VOD workflow?


Bitmovin offers 10 free hours per month to explore AI Scene Analysis within your own environment on Bitmovin’s VOD Encoder. You can also[contact the Bitmovin team](https://bitmovin.com/contact-bitmovin/) to discuss your specific use case.

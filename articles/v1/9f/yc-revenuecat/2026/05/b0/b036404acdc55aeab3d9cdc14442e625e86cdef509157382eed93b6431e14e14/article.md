---
schema_version: "1.0.0"
document_id: "b036404acdc55aeab3d9cdc14442e625e86cdef509157382eed93b6431e14e14"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/growth/ad-generated-ads-ugc"
published_at: "2026-05-06T13:02:26+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:722ecdf155d83c28f45a323c0fe19d5c7409757d29331dcf5406e1d35af913f5"
---

# AI-generated ads: balancing attention and trust in user-generated content

User-generated videos are no longer exclusively *user* generated.


With the rise of AI, advertisers can now generate creator-style ads in a matter of hours using synthetic avatars, automated voiceovers, and video tools. What used to require weeks of coordination and creator involvement can now be executed in an afternoon.


This shift is not just technological, it reflects a deeper change in how advertising operates — the pace of modern platforms has outgrown traditional content production.


But this raises a fundamental question. If content no longer needs to be created by real people to perform, what actually drives trust, and where does artificial intelligence start to break it?


Based on what we’ve tested at Mojo, the answer is more nuanced than the current hype suggests. But there’s a few key learnings:


- Replication beats interpretation
- The cost of failure has collapsed
- AI knows what looks real, but humans know what feels real
- You are no longer building an ad; you are building a system


## **The real driver: ad fatigue**


The rise of AI-generated ads is not driven by novelty alone. While it is undeniably fueled by the fact that technology has finally crossed a critical threshold of fidelity, the primary catalyst for its adoption is a structural constraint: ad fatigue


On platforms like TikTok and Meta, videos are consumed at a relentless pace. A highly-effective ad can lose its impact within days. For subscription-based products, this rapid decay directly increases the cost of acquiring a new customer.


Today, those stakes couldn’t be higher — with RevenueCat data showing[the top 25% of subscription apps grew 80% year-over-year in 2026](https://www.revenuecat.com/state-of-subscription-apps/) , while the bottom 25% shrank by 33%. In a winner-take-more market, efficient acquisition isn’t just about optimization, it’s about your app surviving at all.


The natural response is to produce more videos. But this is where the traditional system breaks:standard production is slow and requires a lot of resources. Even highly efficient teams operate on multi-week cycles. Meanwhile, the algorithms shift in real time.


This creates a fundamental mismatch: **the algorithm evolves faster than your production process** . AI removes that constraint. Instead of asking what the next ad should be, teams can now ask how many variations they can test today. The primary advantage of AI is not that it produces better ads. It’s that it allows you to discover better ads faster.


## Case study: Scaling the Mojo ‘Auto Edit’ winner


At Mojo, one of our highest-performing ads was a simple 30-second split-screen video featuring a speaker explaining the product on top and a screen recording of the app on the bottom. This video converted trial users to paid at 23%.


The secret was the person. Our creator was our Product Manager. When he explained the product, he was walking through something he deeply understood. His conviction came from ownership. That kind of authenticity is extremely difficult to synthesize from scratch.


*Baseline: Our top-performing human UGC. Notice the genuine conviction of our PM talking about the feature he built.*


### **Phase 1: why replication outperformed native creators**


To scale this ad internationally, we initially recruited local creators in several countries to recreate the video. The result was disappointing — despite strong production quality, performance dropped across markets.


The issue came down to execution: each creator interpreted the script slightly differently. Pauses were shortened, pacing changed, and gestures were misaligned. Individually, these differences seemed minor, but collectively, they killed performance.


There was also a constraint regarding legal rights. We often did not have the rights to extend a creator’s content into other languages. This highlighted a critical point: **the best-performing creator is the one you actually own.**


So we changed strategy. Instead of adapting the ad, we replicated it: we used video translation tools like HeyGen to dub the original video, while strictly preserving the timing, the pauses, the gestures, and the exact energy. In Brazil, the dubbed version achieved a 40% lower acquisition cost compared to native creators.


The lesson here is that execution mechanics matter just as much as the message. AI does not reinterpret; it preserves.


*AI Dubbing: 95% perfect lip-sync preserving our PM’s original high energy.*


### **Phase 2: AI avatars (the 80% failure rate)**


Encouraged by these results, we moved to fully AI-generated avatars. We expected the speed of production to compensate for a slight dip in quality. Instead, the outcome was binary: most of the profiles we tested failed almost immediately.


The generic avatars we initially generated did not work, and the reasons went beyond mere technical fidelity.


First, we encountered a fundamental **perspective and staging issue** . As you can see in the variations above, the avatars were placed in hyper-polished environments — studio microphones, cinematic lighting, and stylized backgrounds. Some were even positioned at slight three-quarter angles rather than looking directly into the lens. This immediately broke the native, informal visual code of UGC. The user’s brain categorized the content as a commercial within the first second.


Second, there was a major **casting issue** . The avatars simply did not match our Product Manager’s original profile or conversational energy. We were trying to scale a specific type of raw, founder-led credibility, but we were using avatars that looked like generic stock models or polished influencers. The disconnect between the message and the messenger was jarring.


These flaws, combined with the expected rigid expressions, caused rapid drop-offs. One avatar had a mere 0.2-second lip-sync delay on a key word, which resulted in a 68% drop in click-through rate compared to the human baseline. Users couldn’t articulate why they did not trust it, but the data was brutal.


**But one avatar worked.**


Instead of using a generic, polished profile, we used HeyGen to create a custom digital twin based strictly on our original Product Manager. We stripped away the studio mics and cinematic lighting. We matched his raw facecam perspective, his specific look, and his exact baseline energy.


Because the source material already had natural presence and inherent credibility, the output felt believable. That version reached **87% of the original human conversion rate.**


The economics changed dramatically:


- Traditional creator: **$500**
- Generating the custom AI avatar: **$20**


**RESULT: 31% lower cost per acquisition.**


*The $20 Winner: Natural head tilts and accurate micro-expressions driving a 31% lower CPA.*


### **Phase 3: the double AI stack**


Once we identified a[winning creative](https://www.revenuecat.com/blog/growth/informed-empathy-user-interviews-ad-creatives/) , we introduced a two-step process: generating the base video using the custom avatar, then instantly translating it into multiple new languages using[ElevenLabs](https://www.revenuecat.com/blog/growth/jack-tanmay-elevenlabs-sub-club-podcast-2026/) for localized, natural-sounding voice cloning.


**The results (Brazil localized metrics):**


- **CPA:** $8 (31% lower than the human control)
- **CTR:** 4.5%
- **Conversion Rate:** 3.1%
- **ROAS:** 2.1x


**The cultural limit:** This ‘double layer’ stack (a synthetic avatar that is then dubbed into another language) worked flawlessly in Brazil and Spanish-speaking markets, where users are highly receptive to direct-response UGC formats. However, when we pushed this exact same dubbed avatar to Europe (France, Germany), it failed. The European audience was far more sensitive to the uncanny valley, and the double layer of AI broke their trust.


This highlighted to us that sensitivity to artificial media is not just technical. It is geographic and culturally dependent. You can’t assume universal adoption.


## **Key takeaways: how AI-generated UGC impacts ad performance and trust**


Ads are our business, so we had the luxury of being able to test AI-generated ads and finesse the output. Many apps won’t be able to take this same risk and time, so here’s my top learnings from our creative experiments with AI[user-generated content](https://www.revenuecat.com/blog/growth/ugc-ads-apps/) .


## **Why natural execution beats polished authenticity**


There is a common assumption that videos made by users perform well because they’re authentic. In practice, authenticity is only part of the story. These videos work because — even when users are following a script — they *feel* natural.


The best-performing ads rely on familiar patterns like direct-to-camera delivery, informal conversational tones, and simple visuals. AI is highly effective at replicating these patterns. By mimicking the structure of successful content, AI-generated ads can improve early metrics like views and watch time.


## **Balancing algorithmic attention vs. user trust**


One of the most important insights from our experiments is that these ads operate across two distinct layers:


1. The algorithm rewards **attention**
2. The user decides based on **trust**


AI helps you win the first, but it doesn’t guarantee the second.


In our tests, fully synthetic avatars struggled in formats that rely on credibility, especially testimonials or personal stories. People are sensitive to anything that feels artificial in emotional contexts. We subconsciously scan for micro-expressions, subtle changes in eye contact, and the natural hesitation that signals genuine human experience. When an algorithm tries to simulate a heartfelt story, it misses these imperceptible cues. The result is a subconscious rejection by the viewer.


Conversely, AI thrives in structured, utility-driven formats. For product demonstrations, tutorials, and feature breakdowns, clarity and pacing matter far more than emotional authenticity. The rule is simple: **use AI to explain, and use humans to convince** . This principle was at the heart of our strategy when we decided to scale our own top-performing content at Mojo.


## **The economics of failing fast**


The real advantage of AI was not just cost. It was speed.


With traditional methods,[creative testing](https://www.revenuecat.com/blog/growth/subscription-app-creative-testing/) five human creators takes $2,500 and three weeks of production to find one potential winner. Testing five AI variations takes $100 and just two hours.


The downside of[testing creatives](https://www.revenuecat.com/blog/growth/overanalyze-creative-analysis-paid-ads/) has collapsed. You’re no longer optimizing for perfect execution upfront, you’re optimizing for iteration speed. You are no longer buying videos, you’re buying iterations — and iteration compounds.


## **From production to decision-making**


Before, production was the bottleneck. Now, production is instant. But a new bottleneck emerges: decision-making.


When you only have five videos a month, you can rely on intuition. When you can generate 50 variations a day, intuition fails. Teams no longer need to figure out how to produce content. They need to figure out what to test and what to cut.


AI did not remove the need for creativity. It made taste and data analysis the new competitive advantages. Bad decisions now scale just as fast as good ones.


## **The hidden risks**


Like any new approach to[ad creatives](https://www.revenuecat.com/blog/growth/creative-fatigue-mobile-apps-roas/) , there are payoffs you have to consider:


### Cookie-cutter ads


We’re already seeing a specific aesthetic emerge among AI-generated ads: perfect lighting, lack of breathing pauses, and synthetic enthusiasm. If every brand starts using similar avatars, these videos will become the new fatigued format.[Ad fatigue](https://www.revenuecat.com/blog/growth/detect-ad-fatigue-mobile-apps/) won’t disappear; it just shifts from the individual video to the format itself.


### Navigating the legal maze


There’s also a significant legal challenge. Building your acquisition engine on a digital copy of a real person is a liability maze. Who owns the likeness if that employee leaves the company?


With external creators, the problem is worse. If you don’t secure rights to use their digital likeness, you’re essentially renting your growth. **Owned content scales, rented content does not.** Brands need to rethink their talent contracts entirely. You need specific likeness agreements that outline exactly how, where, and for how long a synthetic avatar can be used, including clauses that allow the company to run the digital copy for six to twelve months after an employee departs.


### Audience backlash and reputational risk


The risk of audience backlash is a reality that cannot be ignored. Audiences are becoming increasingly sophisticated at identifying synthetic media, and for subscription-based apps, the stakes are uniquely high. Because the business model relies on an ongoing relationship, being perceived as using deceptive or low-quality AI[creative at volume](https://www.revenuecat.com/blog/growth/creative-volume-meta-ad/) can be devastating.


It creates what is essentially a ‘trust tax’, if a user feels tricked by an advertisement, they might install the app, but will inevitably churn. It doesn’t just hurt immediate acquisition; it erodes the fundamental trust that drives long-term retention. Every app audience is different, so it’s vital to consider your specific consumer group’s sentiment. The challenge for brands moving forward is finding the balance between being transparent, efficient, and maintaining a sincere human connection.


## **Best practices for scaling AI-generated ads**


Before you generate new AI content, you need to answer a few fundamental questions:


- Do you own the source material and likeness rights?
- Is the format utility-driven rather than an emotional story?
- Can you kill the campaign in 24 hours if the data shows it is failing?
- Have you tested your specific market’s tolerance for synthetic media?


Most importantly, you have to **break the perfection** . Perfect delivery feels like a commercial. Here’s what we’ve found works:


- Lower the resolution slightly so it does not look like a studio shoot
- Let the audio retain a bit of background room tone.


Artificial intelligence naturally gravitates toward a flawless output, so you have to actively force it to be messy.


### Checklist: when to use AI or real users for ads


If you’re deciding between a human creator and a synthetic avatar, use this quick reference guide based on our testing:


**Opt for AI-generated ads when:**


- **The goal is utility:** product walkthroughs, feature demos, or screen-recording tutorials
- **You need scale:** testing 10+ hook variations or localizing an ad into five different languages
- **Speed is critical:** you need to respond to a creative trend within 24 hours
- **Cost is the bottleneck:** you have a winning script but a limited budget for multiple creators


**Opt for real humans when:**


- **The goal is trust:** personal testimonials, ‘storytime’ formats, or founder-led brand intros
- **Emotional nuance is key:** content that requires subtle micro-expressions or genuine empathy
- **High-value markets:** targeting regions (like Europe) with high sensitivity to ‘uncanny valley’ media
- **Originality:** creating the source material that will eventually be replicated by AI


## **The new UGC**


We’re entering an era of infinite content where the true bottleneck is no longer creation, but judgment. While the technological barrier to generating an ad has effectively dropped to zero, **the psychological barrier to earning consumer trust has never been higher** .


Algorithms are exceptionally good at replicating familiar patterns to buy attention, compress feedback loops, and scale distribution. What they cannot do, however, is synthesize genuine conviction. The top growth teams of the future won’t merely be those who test the fastest. Instead, they will be the ones who understand exactly when to automate a system for efficiency, and when to rely on a real human heartbeat for persuasion.Ultimately, AI didn’t make advertising smarter — it simply revealed the clear boundary between what machines can execute and what they cannot. **Speed and automation will always buy you attention, but it takes a human to earn trust** .


## Eleven Labs on the Sub Club podcast


Hear the story behind Eleven Labs, the AI tool Anthony and the Mojo team used for their winning creative


[YouTube](https://www.youtube.com/watch?v=3r8pr9w_lDQ)[Spotify](https://open.spotify.com/episode/3gC8n6pbe52ppPB0BWLyTa?si=feaba85b2af14556)[Apple Podcasts](https://podcasts.apple.com/gb/podcast/how-elevenlabs-turns-feature-launches-into-a-growth/id1538057974?i=1000753972691)

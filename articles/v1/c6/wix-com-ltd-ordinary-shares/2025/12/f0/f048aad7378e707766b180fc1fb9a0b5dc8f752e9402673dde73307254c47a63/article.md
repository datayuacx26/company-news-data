---
schema_version: "1.0.0"
document_id: "f048aad7378e707766b180fc1fb9a0b5dc8f752e9402673dde73307254c47a63"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/the-art-behind-better-ai-how-we-achieved-a-46-speed-boost-and-23-cost-reduction"
published_at: "2025-12-09T08:38:23+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:25:04.749708+00:00"
content_hash: "sha256:568d011058ed1f4daa0c29591bc2992c271e1f4cbbd1c7ce9949a5e42d0b3fc4"
---

# The Art Behind Better AI: How We Achieved a 46% Speed Boost and 23× Cost Reduction

###


Intro: The Limits of Prompt Engineering


In modern AI agent development, many teams, including ours at


**Wix** , start with an intense focus on


**prompt engineering** , carefully crafting instructions for the LLM. We iterated endlessly on system messages and instruction sets to solve a core problem: our AI scheduling assistant was struggling with vague requests, such as


*"Schedule a meeting with Jake."*


This simple sentence triggers a surprisingly complex challenge. The system must identify


*which* person best matches the user’s intent, based on everything it knows: names, past meetings, recency, and more.


Despite our best efforts with prompt tuning, the model often hallucinated contacts, asked useless clarifying questions, or picked the wrong person because it was forced to process a massive amount of unoptimized,


**"raw context"** .


###


**The Baseline: Slow, Costly, and Inaccurate**


All tests were conducted using the internal


**Wix Evaluation Center** platform, ensuring a standardized and repeatable environment. Each test run processed


**150 unique requests** using


**mock data** to simulate complex, real-world user scenarios and track key performance metrics.


Our initial attempts, relying on a baseline of sending all raw, unoptimized data, were poor and expensive. The LLM had to do the heavy lifting of sorting and reasoning through mountains of irrelevant tokens.


This raw context approach was slow, expensive, and unreliable. It confirmed that the true differentiator is not how you ask the question (the prompt), but


**what data you send with it (the context)** .


We quickly pivoted our focus to


**Context Engineering** .


###


What is Context Engineering?


Context engineering is the discipline of collecting, normalizing, structuring, and optimizing data


*before* it ever reaches the LLM. It moves the heavy lifting from the slow, expensive LLM to a fast, deterministic preprocessing layer.


The goal is to take a raw, complex dataset and present the LLM with a


**compact, sorted, token-efficient list of likely matches** . This allows the LLM to make quick, confident decisions using a faster, cheaper model.


###


**The ROI: Flash Models, Not Pro Models**


By investing in sophisticated context engineering, we were able to use


**Gemini Flash 2.0** , a fast, cost-effective model, instead of requiring slower, more expensive models. The heavy lifting happens in our preprocessing layer, allowing the LLM to just validate the top recommendation or select from a clearly ranked list.


**The trade-off** : More engineering complexity upfront -> Faster responses and lower costs.


###


The 4 Best Practices for Context Engineering


Our context pipeline follows a structure designed for accuracy, cost efficiency, and reliability: Data Collection -> Normalization -> Relevance Scoring -> Formatting -> LLM.


####


1. Rank Candidates with a Utility Function


LLMs make more accurate choices when they receive


**ordered** context, they tend to prioritize what appears at the


**top** of the prompt.


We compute a


**utility score** for each candidate contact based on weighted signals:


-


**Name Matching Signals (Highest Weight):** Includes exact, fuzzy, and phonetic matches (e.g., Catherine <->Katherine).


-


**Interaction History Signals (Medium Weight):** Includes meeting frequency and recency of interactions (using a decay function).


-


**Contextual Signals (Lower Weight):** Includes shared projects or department overlap.


This ranking process transforms a potentially overwhelming list into a


**sorted shortlist** that reflects real-world relevance, dramatically improving LLM accuracy.


####


2. Use Slugs Instead of Opaque IDs


LLMs understand natural language, not database identifiers. We assign every contact a


**semantic, human-readable slug** (e.g.,


"john smith"


or


"


jake@example.com "


).


-


**Token Efficiency:** Slugs are short, meaningful, and waste fewer tokens than long, opaque IDs (UUIDs or internal numerics).


-


**Safe Matching:** The LLM's output is an intended key (the slug), which is then mapped back to the internal ID by our system. This creates a secure separation of concerns and prevents the model from hallucinating or manipulating real database IDs.


####


3. Optimize Context for Tokens


In the


**Raw Context** , our LLM was receiving massive, nested JSON payloads for both meeting history and contact lists and more. These raw data, though accurate, were highly inefficient. This preprocessing step transforms the raw, noisy data package into a readable, token-optimized line for the LLM:


Contact: John Smith, Slug: john smith, Details: 3 recent meetings, strong name match


This single-line format yielded a


**~96% token reduction** over our verbose structure, improving clarity and keeping the prompt focused on the core decision.


####


4. Minimize LLM Output for Speed and Cost


While optimizing the input context is critical, the single most effective way to reduce overall latency is by strictly optimizing the


**LLM's output** . Since output tokens are generated sequentially (autoregressively), they are the primary driver of response time and cost.


The goal is to design the LLM's task to return a


**minimal, structured data payload** rather than generating large amounts of prose. We achieve this by compelling the model to output a strict JSON object containing only the necessary, final decision.


For example,


{"chosen_slug": "john-smith"}


. This strict output instruction is critical for achieving the


**45.8% reduction in average duration** by minimizing the expensive, sequential token generation step.


###


The ROI of Context Engineering: The Data Speaks


By implementing these context engineering strategies, rigorously tested through 150 requests per run in the Evaluation Center, we achieved massive performance gains.


###


**Cost and Token Reduction: Baseline vs. Optimized**


The move from the


**Raw Baseline** to the


**Optimized Context** was transformational. The cost reduction validates the trade-off of "More engineering complexity upfront -> Faster responses and lower costs forever".


The Avg cost was reduced by


**~23.2X Cheaper.**


###


**Model Comparison: The Advantage of Flash**


By optimizing context, we enabled the use of


**Gemini Flash 2** , a fast and cost-effective model, even when compared to the highly capable GPT-4.1 model.


When comparing the models using the


**Optimized Context** :


-


**Speed:** Gemini Flash 2 achieved an average duration of


**1.786s** per request, while the GPT-4.1 model averaged


**2.589s** . This makes


**Gemini Flash 2 approximately 1.45X faster** .


-


**Cost:** While we cannot share the specific pricing figures for proprietary reasons, the relative performance showed that


**Gemini Flash 2 is approximately ~4.8X cheaper** on a per-call basis than the GPT-4.1 model.


**Conclusion from Comparison:** When context is optimized, both models perform significantly better than the raw baseline, but


**Gemini Flash 2** offers an optimal balance of cost, speed, and accuracy for this specific task, being almost five times cheaper than the alternative on a per-call basis.


###


Conclusion


Context engineering is the invisible layer that makes AI systems feel intelligent. By moving the heavy lifting from the LLM to a deterministic preprocessing layer, we were able to achieve a


**~23X reduction in cost** and a


**~46% speed-up** compared to our initial raw context baseline.


At Wix, this robust methodology, supported by our Platform Team's Evaluation Center, proved that success in modern AI agents stems from controlling the input data, not just tuning the prompt instructions. Invest in context engineering, and everything else, your models, your user experience, and your budget, becomes dramatically more effective and affordable.


This post was written by


**Idan Dagan**


And Co-authored by


**Yonatan Nadel,**


**Yoav Jacobsen** , and


**Guy Sopher**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS)

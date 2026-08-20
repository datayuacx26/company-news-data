---
schema_version: "1.0.0"
document_id: "454a2b9dd40b767426e4d95c7fbd74c2f75d574208de76749acff7a47c722135"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/what-makes-an-llm-generative-ai-copyright-law-and-the-schrodingers-cat"
published_at: "2025-02-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:e5846e92b380fcbe8c95f96ededd376cf239d5adff0f5138bbe46a9c6df10ee8"
---

# What makes an LLM generative? AI copyright law and the Schrödinger's cat

## Generative models 101


**Generative models** are a relatively new concept. All generative models (text, image, sound, video, financial transactions) have been trained in essentially the same way: *here is what’s happened so far, try to guess what happens next* .


The magic happens when you ***sample* from them repeatedly** . You ask the model *“what happens next?”* , *“what happens next?”* , *“what happens next?”* until you’ve seen enough. That’s how[ChatGPT](https://chatgpt.com/) ,[Midjourney](https://www.midjourney.com/) ,[ElevenLabs](https://elevenlabs.io/) ,[Sora](https://openai.com/sora/) , etc. work.


As is usually the case, brilliant ideas are extremely simple at their core. The success of generative models is not the fact that it’s fun to keep asking *“what happens next?”* . It’s because it’s **so easy to obtain training data** for them. Take *anything* (e.g. text, image or video), hide a part of it and get the model to guess what happens next. If it gets it wrong, show them what actually happened, correct course and repeat.


There. I just summarized the Deep Learning revolution.


## Thomson Reuters vs Ross Intelligence


The above story works great as an intuitive definition of what a generative model is. However, the recent major AI copyright case shed a new light on the subject.


[Thomson Reuters sued](https://www.reuters.com/legal/thomson-reuters-wins-ai-copyright-fair-use-ruling-against-one-time-competitor-2025-02-11/) Ross Intelligence for repurposing law case data purchased via its platform[Westlaw](https://legal.thomsonreuters.com/en/westlaw) to train a language model. The model was turned into a product, Westlaw’s direct competitor. Thomson Reuters won the case as per[judge’s February 11th decision](https://storage.courtlistener.com/recap/gov.uscourts.ded.72109/gov.uscourts.ded.72109.770.0.pdf) , a revision of their 2023 summary judgment.


## Non-generative model?


Interestingly, the judge calls Ross’s model **non-generative** because it *spits out relevant judicial opinions* as opposed to *writing new content* .


> *It is undisputed that Ross’s AI is not generative AI (AI that writes new content itself). Rather, when a user enters a legal question, Ross spits back relevant judicial opinions that have already been written.*


So that there is no doubt, the judge later adds that:


> *Because the AI landscape is changing rapidly, I note for readers that only non-generative AI is before me today.*


## Model temperature


When we ask a generative model *“what happens next?”* it doesn’t tell us straight. Aware of its limitations, the model hedges its bets and gives us a **list of probabilities** over words, parts of images or video frames. It is up to whoever wrapped the model in a **product** to decide what to make of the probabilities.


There is a parameter called **temperature** you may have heard of. Temperature defines how we go from the probabilities produced by the model to deciding what to show to the user. If we sample with a very low temperature, we **always** take the highest probability word, part of image or video frame. The higher the temperature, the more likely we are to sometimes have the model say a word that (according to the model itself!) isn’t the most probable.


We associate higher temperature with a higher creativity and a higher risk of hallucinations. At very low temperatures (temperature 0.0), the model is less creative, **sticks very closely to its training data** .


## Both generative and non-generative?


When the Ross Intelligence model is given a question and asked *“what happens next?”* it *spits back relevant judicial opinions* which, according to the judge, makes it **non-generative** . The model is likely run with a very low temperature (near 0.0). Undoubtedly, with temperature set to 0.7 or higher, they would be more likely to say that it *writes new content* .


That reasoning takes us to the Schrödinger’s cat. **Until I’ve decided what temperature to use** , my model might be abiding by or violating copyright law. After the decision is made, it’s still the same model and the probabilities it spits out are the same. What has changed is how the probabilities are **translated into what gets shown to the user** .


## *Generative* redefined


Thomson Reuters versus Ross Intelligence is just one of many AI copyright cases to watch. As they hit the news cycle and the mainstream, we might see the term *generative* redefined.


Generative products can be see as ones that **act as if they knew things** rather than **tell you those things directly** . Which it is, is only known once it’s decided at what temperature and[how people shall interact with the model](https://www.quickchat.ai/post/generative-search-optimization#toc-how-does-chatgpt-work?) . Until then, it’s a Schrödinger’s cat.


*This analysis was originally published as a*[LinkedInpost](https://www.linkedin.com/feed/update/urn:li:activity:7295698275800637440/) *on February 14th.*

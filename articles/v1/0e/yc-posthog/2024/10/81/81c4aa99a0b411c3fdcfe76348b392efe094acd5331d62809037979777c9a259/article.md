---
schema_version: "1.0.0"
document_id: "81c4aa99a0b411c3fdcfe76348b392efe094acd5331d62809037979777c9a259"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/ai-community-answers"
published_at: "2024-10-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:27aca1e4fe86c162cf67af7c6657ad2f55218cf62919b8d45e954e3cf99bd32f"
---

# Did you know AI is answering our community questions?

# Did you know AI is answering our community questions?


- [Cory Watilo](https://posthog.com/community/profiles/30200)


Oct 03, 2024


#### Contents


-
-
-
-
-
-
-
-


AI. You may have heard of it.


Sure, ChatGPT is pretty cool, but when it comes to AI chatbots that try to replace a human in a product support context, I've generally been underwhelmed. The experience generally isn't great, and I'm often left with more frustration than before I skeptically asked the question in the first place.


But it's not all the fault of LLMs. It also has to do with the available information about a product that an LLM consumes. And for a technical product like PostHog, the documentation has to be extensive enough to answer very nuanced questions. (This is tricky as information is constantly changing and there's always room for improvement!)


As the product owner *totally official* "webmaster" of PostHog.com, I've contemplated how we can make use of AI to help answer questions... and until now, I've lacked a good solution.


##


Why AI chatbots have a bad rap


There are a lot of *"wrong"* ways to do AI chatbots – and there are many examples of this even before we collectively started using true AI.


How many times have you tried to create a support ticket but then:


- been first prompted to review a list of "related questions" that may solve your issue
- been *blocked* from submitting until you explicitly confirm that none of the suggested answers are helpful


And now in a world of *actual* AI, we see a new set of issues – largely centered around LLMs not understanding the question, the context, or straight up hallucinating.


In no world would we want to subject our very technical audience to these types of dreaded experiences.


This isn't to say *all* AI chatbots are bad. A couple of exceptions come top of mind mainly Intercom and Shopify. But the status quo that the industry has to overcome is the negative connotation of historically bad chatbot interactions.


##


Our approach


The reason we've been so cautious to introduce AI is that we're very intentional about earning (and not losing) the trust of our audience. We do this in a handful of ways:


1. We don't use fancy words that don't add actual value
2. We try to be relatable and show a personality
3. We traded in the[value-based sales model](https://posthog.com/sales)


for a no-BS approach (self-service, fair, usage-based pricing)
4. We avoid gimmicky marketing tactics (like popups and jamming CTAs into every available whitespace)


So for an AI solution to mesh well with our brand's vibe (yes, we're about the vibes), it needs to be accurate and succinct in its replies, and never make up a false answer.


After all, if you interact with an AI chatbot and don't get the answer you're looking for, you're likely to write it off entirely. And that's exactly what I want to avoid.


##


Where we decided to implement AI


The most[natural place to include an AI chatbot](https://posthog.com/newsletter/building-ai-features)


would be in our[docs](https://posthog.com/docs)


. But I was hesitant to start here, as the conversational nature of AI meant we'd lack control over the quality and accuracy of the responses. So I decided to try something different.


Our[community questions](https://posthog.com/questions)


section is where users can ask questions directly in the docs. And we currently have... a lot of open (unanswered) questions. But because they're posted in a forum-style format, there's no expectation of a live chat experience. And this presented an opportunity...


Today customers may wait hours (or even longer) for a response from a human. **So what if AI could answer *only* the questions it was highly confident it could answer, then leave the rest for a human?**


Using this approach, AI can *add* value by instantly responding if it has a high degree of confidence, but it won't *detract* from the experience if it doesn't have the solution, simply leaving those questions open as they normally would.


##


Introducing Inkeep


This whole idea came about from an[Inkeep](https://inkeep.com/?utm_source=posthog)


ad on LinkedIn – which is incredible to say because I'm generally allergic to ads. But Inkeep's ad spoke to me.


I took the bait, and they quickly set me up with[a custom sandbox](https://share.inkeep.com/posthog/3003538eb7fd)


to try them out. They're indexing our docs, tutorials, blog posts, handbook, existing community answers, and even GitHub repos. (This also means it's aware of bug reports, feature requests, and open pull requests - *huge!* )


They even themed the sandbox to match our brand. <3


To vet the quality of the answers, I started copy/pasting unanswered community questions into the sandbox and seeing what Inkeep could produce. And to say I wasn't let down should tell you something. It was good enough to where I was willing to try it out on our real questions.


##


How it works


> Inkeep's pre-built library of UI components works great for most companies. But out of the kindness of their hearts, they went the extra mile for us.


**Here's how our setup works:**


1.


When a new question is posted, we send it to Inkeep's LLM.


2.


Typically within ~20 seconds, we get a response. During this time, we show that we're searching for an answer.


3.


In the response, we get a` confidence` score.


```text
const KnownAnswerConfidence = z.enum([      "very_confident",      "somewhat_confident",      "not_confident",      "no_sources",      "other",    ]);
```


**If the confidence score is high enough,** we show the answer to the user.


**If Inkeep *isn't* confident in the answer,** we show a message that says we couldn't find an answer, and that a human will appear as soon as possible.


When an AI answer is presented, we show feedback buttons so the original poster can tell us if the answer was helpful. We use this information to improve the quality of the answers.


##


Demo


Here's what it looks like to submit a question and receive an AI answer from Inkeep.


##


How we're tracking feedback


As you can imagine, every time a customer rates a response, we're sending the information into PostHog as a custom event. Along with the event, it sends properties like the sentiment ("helpful" or "not helpful") and the category of the question. (This will help us identify if certain areas of our documentation needs to improve.)


Now that this is live, we're monitoring feedback in Slack. PostHog's recently introduced support for[realtime destinations](https://posthog.com/changelog?id=2069)


in data pipelines.


This allowed me to configure notifications when we get feedback about an AI answer.


##


So how's it working?


Overall, it's working pretty well! In fact, it worked so well[in one case](https://posthog.com/questions/autocapture-event-bubbling)


that the original poster replied to share his delight.


In other examples, we were receiving feedback that the answer *wasn't* helpful. But after reviewing the response, the best way to accurately summarize the bulk of feedback is: "The answer is *correct,* but I didn't *like* the answer." (This is usually the case when a feature isn't yet supported and it's not the answer the customer was hoping for.)


This resulted in a lower overall success rate. So we modified the response options to better understand the different reasons why a response might not meet the mark.


With these additional feedback options, we're able to use it to inform areas for improvement. For example, if someone says, *"I think this is a bug",* we'll take a closer look. Or if they choose the *"Answer is wrong"* option, we'll take a look into what might be missing from the documentation.


It's also won over some internal support from developers on the PostHog team:


---


We're still early in our days of exploring AI – and don't worry, AI *is* coming to the PostHog app when we feel it's ready.


But I couldn't give you a better rationale for a slow rollout than how a colleague explained it to me:


Hopefully the next time you[ask a question](https://posthog.com/questions)


in our community, you'll get the answer you're looking for. And even better – if you get it even faster than a human could answer.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions

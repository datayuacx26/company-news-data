---
schema_version: "1.0.0"
document_id: "2ce1c7c0c12ed8d696a21beaa7082685024f6174b16ba18db6700adec062cf00"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/why-we-killed-our-ai-product-assistant"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:25:11.092082+00:00"
content_hash: "sha256:2ab4248144b5536007e5848a7a7235448314951dd53f008137261c268a454383"
---

# Why we killed our AI product assistant

# Why we killed our AI product assistant


- [Cleo Lant](https://posthog.com/community/profiles/36864)


Nov 19, 2025


- [PostHog news](https://posthog.com/blog/posthog-news)


,
- [Inside PostHog](https://posthog.com/blog/inside-posthog)


#### Contents


-
-
-
-
-


*jk, we didn't really do that.*


Like everyone else, our AI journey started with a chatbot. From the first “you're absolutely right!” it crawled, then walked, then became a lanky teenager with confidently wrong opinions.


Our beloved hedgehog, Max, was the face that greeted you at every AI interaction. It kept learning, and eventually grew into a very capable product assistant. We learned a lot about[building AI-powered features](https://posthog.com/newsletter/building-ai-features)


along the way.


**The problem:** Even as we added AI capabilities across the platform, people still thought of Max AI as "the chat bot." The broader vision (AI woven through every product) wasn’t landing.


We didn't want to be another company with[hand-wavy marketing](https://posthog.com/newsletter/marketing-for-devs)


like “it's not just a chat bot, it's… \[insert amazing AI capability here\]”, which meant we needed to rethink how we positioned our AI capabilities in the minds of our users.


##


The assistant becomes the architect


Lots of beta users loved Max, but a comparison that kept coming up internally was Clippy; a paperclip animated assistant from Microsoft Office in the late 90’s, known for its intrusive, and unhelpful pop-ups (It was widely disliked and was removed in Office 2007).


Clippy was ahead of its time. Prove me wrong.


Clippy was the Kool-Aid Man busting through your wall; Max AI was opt-in and useful, but still it's an important lession in perception. Product assistants don’t work when they feel like a novelty UI. Whether users see your AI as a gimmick or a genuine tool is something you have to own, especially if you're building an AI-native product.


Deciding how to address this was a hot topic at PostHog. A single Slack thread blew up with over 90 comments! The discussion was pretty intense, hitting on all the key angles from engineering to marketing. Did the mascot add to the user experience, or did it doom perception of PostHog AI as[‘just a chat bot'](https://posthog.com/blog/ai-community-answers)


.


In the end, we decided to axe Max. Not because we hate joy (we like it so much we[risked tanking our conversion rate](https://posthog.com/blog/why-os)


). We did it because the mascot was a limiting factor.


Max AI became PostHog AI. Not just a character that lives in one corner, but the underlying architecture of PostHog product intelligence (sorry for sneaking a ‘not just’ statement in there).


Don’t worry, we asked Max if he was ok with it. He said it was our call


##


Why our AI got noticeably better since beta


Long before this pivot, and as it was happening, the real work of improving PostHog AI was happening deep within PostHog. The posthog-ai


was building a shared infrastructure with the same philosophy as[PostHog SQL](https://posthog.com/docs/sql)


: a single system for each small team to extend.


**Without it:**


- fragmented experience
- duplicated effort
- maintenance nightmare
- Teams stuck building simple features from scratch instead of advanced stuff by default


**With it:**


- shared agent system any product can extend
- reusable components that work everywhere
- consistent UX patterns
- platform-level improvements that benefit everyone automatically


Overhauling the guts was a good move. It enables deterministic tool‑calling, tighter context packing, and fine-tuned reasoning.


Translation: fewer “sorry I can’t do that yet” moments, faster replies, cheaper inference.


Key differences from older architectures:


- **Less hallucination** : The agent always checks your data before giving an answer
- **Full visibility** : All tool calls are visible to the agent throughout the conversation
- **Maintained context** : The agent remembers every decision it made and can build on them
- **Explainable** : Chain-of-thought is visible and easy for you to investigate


What PostHog AI can do now:


- **Search and filter** : Find insights, filter session recordings, search documentation
- **Create and modify** : Build dashboards, create insights, set up surveys
- **Write SQL** : Generate and debug PostHog SQL queries for custom analysis
- **Configure and Test** Set up and validate feature flags and experiments
- **Teach you PostHog** : Explain how features work, recommend best practices, and clarify terms


*Interested in a technical deep-dive? Peep our[AI platform architecture](https://posthog.com/handbook/engineering/ai/architecture)*


##


How this changes who can use PostHog


As a company, we’re well known for our[ICP discipline](https://posthog.com/newsletter/ideal-customer-profile-framework)


. Knowing who you’re building for is important — it keeps teams focused, docs clean, and marketing honest. But as PostHog AI matures, we need to keep an eye on the expanding circle.


Tools like Cursor, Replit, and automation platforms such as Make or n8n are built with technical users in mind. Yet their intuitive interfaces also draw in less technical users — people who might not code every day but still have real[problems to solve](https://posthog.com/newsletter/how-to-uncover-your-users-real-problems)


and[workflows to automate](https://posthog.com/blog/workflows-alpha)


.


That’s not exactly our goal with PostHog AI.


We’re doubling down on technical users. PostHog AI helps them move faster by automating the tedious parts, connecting insights across products, and turning analysis into “what should I work on next?”


**A nice side effect** : it also makes PostHog easier for everyone around them. PMs can self-serve data, marketers can ask smarter questions, and analysts can spend less time as human middleware.


A common piece of feedback we hear during user interviews is that PostHog AI helps with adoption of PostHog across the org.


##


Build systems, not assistants


Assistants are what you build when intelligence sits *next* to your product. Systems are what you build when intelligence becomes *part of it* .


For us, that meant letting go of Max as a mascot in order to shape the perception of PostHog AI as an architecture that connects everything together. It reasons across data, automates work, and makes each part of the product a little smarter.


Some folks miss the personality:


But most people won’t care, so long as the tool makes their workflow better:


##


Try it


[Enable PostHog AI](https://posthog.com/docs/posthog-ai/allow-access)


and ask it a question you actually care about this week. Need some inspiration? Check out our[example prompts](https://posthog.com/docs/posthog-ai/example-prompts)


.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions

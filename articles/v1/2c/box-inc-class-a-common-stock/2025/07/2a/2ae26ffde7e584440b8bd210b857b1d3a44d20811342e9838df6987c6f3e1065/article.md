---
schema_version: "1.0.0"
document_id: "2ae26ffde7e584440b8bd210b857b1d3a44d20811342e9838df6987c6f3e1065"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/building-your-first-ai-product-a-practical-guide-9323f8ab9469"
published_at: "2025-07-01T14:52:37+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:4d840fe76ed8017c4feab6b970da10e1a132a18fa9ffd221f04befe12080bd06"
---

# Building your first AI product: A practical guide

# Building your first AI product: A practical guide


[Shubhro Roy](https://medium.com/@shubhrojyotir?source=post_page---byline--9323f8ab9469---------------------------------------)


9 min read


·


Jul 1, 2025


--


Press enter or click to view image in full size


Illustrated by Navied Mahdavian / Art directed by Erin Ruvalcaba Grogan


AI is no longer a moonshot. Every company, small or large is trying to figure out how to create value for their customers using AI. They must do this to adapt to the tectonic shift that is occurring in how consumers and enterprises operate. At the same time new startups are coming up building tools and products that leverage AI to solve existing problems or simplify workflows. But too many companies make the same mistakes when building their first AI product: oversized teams, unclear roadmap / product vision, complex architectures and not clear way to evaluate progress.


In this article, I will share some hard-earned lessons from my experience as a senior engineering leader within Box’s AI organization, where I helped build Box AI — our flagship generative AI product — from the ground up. If you’re embarking on building your first AI product or assembling your initial AI team, these insights are for you.


## Anchor to a Hero Usecase


Pick a painful real world problem that can be meaningfully solved using AI. It is important to spend time finding the first hero usecase coz that sets the AI engine in motion. Success of this first use-case determines a lot of things including further business investment in this area, expansion of team and roadmap and most importantly team morale. I have come across many instances where everything was done right to solve the wrong usecase leading to low adoption / lack of impact on business metrics. Here are some strategies that worked for us when identifying the first usecase:


- **Conduct customer interviews** to understand how they are already using AI or where are the painpoints currently that AI can help with
- **Run internal surveys within your company** ( works better for mid-size / large companies ) to understand internal workflows that can be simplified with AI
- **Perform market survey** to understand how similar products or companies are innovating using AI. This can either help to find the right product-market fit or avoid building a redundant product.


Once you have identified a use-case, find an early adopter or design partner. This can be a bullish customer or an internal team that is willing to dogfood. Treat them as collaborators and not testers only. This speeds up real customer feedback cycles allowing you to quickly discard approaches that are unfavorable than waiting till alpha / beta stages. For Box AI we had both and it was immensely helpful. Now we are doing the same with AI Agents.


Lastly it is critical to connect any new product (especially AI products ) to relevant business metrics. For startups / entirely new product lines this could be simply WAU / MAU. But in some other cases AI products could improve operational efficiency. For example if you are building a customer success AI chatbot for your product you should measure the % reduction of actual customer tickets filed or mean time to resolution for filed tickets etc. Every AI product is different and the business metric should correctly reflect the value proposition of applying AI to the problem space. Incorrect business metrics or lack thereof can lead to late stage product failures or issues with justifying further investment or scope expansion.


## Start with a small team


When we started building out Box AI few years back, we kicked off with 4 borrowed engineers from other teams and a product manager ( part time from another team ). Thats all we needed to build out the first MVP. This requires a few things:


- Identify a single hero usecase
- Clearly define the MVP requirements both on the product and engineering side
- Reuse existing infra where possible
- Keep it simple


In our case we picked single document question answering as our first hero usecase and scoped it down to only small documents that fit within the context window of gpt3.5 ( the SOTA model at that time ). It was limited but sufficient to prove out the value proposition to our design partners.


Having a small laser focussed team make it easy to navigate changing priorities initially and optimize for speed and alignment. Dont worry about headcount till you have had your first customer Beta atleast. The smallest AI tiger team needs only the following roles:


- 1 product minded tech lead / eng manager
- 1–2 10x engineers with ML exposure ( no you dont need to hire an ML engineer yet )
- 1 full-time / part time product manager with background in AI / ML / Search / Recommendations or related fields
- 1 UX Designer


At a small startup some of these roles can also be merged or played by the co-founders.


The most important thing to ensure with such tiger teams is maintaining execution velocity. We achieved this with a culture of weekly demos: build, demo, gather feedback and iterate and measure constantly. This brings me to my next advice.


## Define Metrics and build evaluation sets early


Evaluation is the key to building any AI / ML product. When you use an LLM to generate a response / action, you need to measure if it was done correctly to the satisfy the users informational need. There are two types of evaluation:


- **Offline Evaluation:** where you validate your approach against ground truth data typically generated / validated by humans offline.
- **Online Evaluation:** where you validate the performance of approach on unseen data where ground truth is not available. Typically this is done using LLM-assisted grading approaches with some sampled human-in-the-loop validations.


As you start building out your product you will first need an offline evaluation dataset. Leverage your early adopters to build this. Initial evaluation sets do not need to be extensive and definitely dont need LLM-assisted approaches. Rather I would suggest staying away from AI assisted dataset generation for offline evaluation initially as this can bake in hallucinations early on. A few hundred examples with human judgements goes a long way initially than thousands of AI generated examples that are hard to validate for humans. For Box AI we started with gathering our initial evaluation dataset using a Google form and a few teams within the company that had strong examples for our initial hero usecase. Depending on the size of your company, crowdsourcing the initial dataset can be very effective.


Press enter or click to view image in full size


Fig 1: AI Evaluation Flow


**Word of Caution:** Public datasets are available for common AI tasks such as summarization and QA. But the data may not be reflective of what your customers have. For example Box customers use AI on enterprise documents: contracts, company manuals, medical images etc. Hence if we tried to evaluate using Wikipedia based QA datasets it would be ineffective. Hence in most cases having your own representative dataset can be instrumental.


Once you have started gathering your dataset, define your evaluation strategy and metrics. Evaluation metrics can be of three types:


- **Standard ML metrics:** Precision, Recall, F1-score, AUC/ROC etc
- **Generative AI metrics:** Perplexity,[ROGUE](https://en.wikipedia.org/wiki/ROUGE_(metric)) ,[BLEU](https://en.wikipedia.org/wiki/BLEU) etc
- **Human Evaluation metrics:** Correctness, coherence, conciseness etc


Depending on the task you are applying AI to, one or more of these metrics can be used to measure how your approach is working. Using standard industry metrics ensures that you can benchmark your scores against others and share this data publicly or with your customers to build confidence and trust in your product.


Lastly establish a continuous evaluation strategy. Evaluate against your benchmark dataset every time:


- You modify your system prompt for the LLM
- Add / modify additional processing steps in your pipeline such as RAG, tool use etc
- Change the LLM model as new SOTA models are released or choose to fine-tune an existing one.


## Start Simple and add complexity only when the metrics justify it


Building your AI architecture does not need to be really complex for your first product, atleast not initially. Based on your hero usecase and product requirements for your MVP figure out what the simplest engineering architecture.


For Box AI we initially started with 2 services:


- **Intelligence:** This service exposed external APIs for the Front End to integrate with and performed permissions checks to ensure the user had access to the content they were using for their AI needs.
- **llm-gateway:** This service served as the interface with LLM providers like OpenAI, Anthropic etc. This service also managed the prompts for our initial use-case and performed online grading of answers returned by the models using an LLM based grader.


This initial setup allowed us to quickly iterate and gather feedback from customers. As we added additional features likes RAG, conversation history, citations etc. we revisited this architecture and started splitting up the core services where it made sense and added additional components like vector stores and indexing pipeline.


Press enter or click to view image in full size


Fig 2: Simple AI Architecture


Below table provides a quick guide to to common aspects of AI Architecture and when to consider them in your journey to build the first AI product:


Press enter or click to view image in full size


Table 1: AI Architecture Decision Table


Once you have a baseline architecture, add complexity incrementally. After each addition:


- Run **offline evaluation** (against ground truth)
- Monitor **online metrics** (user feedback, satisfaction, usage drop-offs)
- Estimate **engineering + ops cost**


If a new approach doesn’t clearly outperform the last — go back or stay where you are.


## Stay Model/Provider Agnostic


One of the most strategic decisions you can make early on is to avoid locking your AI stack into a single model provider. The landscape is evolving rapidly — new foundation models are being released every few weeks, each with different strengths in reasoning, speed, latency, cost, or compliance. If your architecture is tightly coupled to a specific provider’s API, switching later can be time-consuming, expensive, and risky.


Instead, design your platform to be modular and provider-agnostic. Abstract model interactions behind a service layer or gateway so that your application code isn’t directly dependent on any one API schema or response format. This allows you to experiment with multiple models and switch when performance, cost, or capabilities justify it — without rewriting your core logic. It also enables A/B testing across providers, fallback strategies when one provider fails, and custom routing based on task type.


To stay truly provider-agnostic, your architecture should include:


- **LLM Integration Layer** : Supports major providers like OpenAI, Anthropic, Google Gemini, Mistral, or AWS Bedrock via APIs or SDKs.
- **Prompt Translation Layer** : Lets you manage prompts per provider or model family — either through prompt templates or translation logic.
- **Model Router** : Dynamically selects the model + prompt based on use case, user tier, performance, or cost constraints.


This flexibility provides major benefits:


- **Easier Upgrades:** When newer, more powerful models become available, you can integrate them seamlessly.
- **Cost Efficiency:** You can take advantage of pricing differences across providers by routing requests dynamically based on cost or performance.
- **High Availability:** Avoid long downtimes when a single provider has an outage ( we have seen our fair share of these from major SOTA model providers )
- **Future-Proofing:** Your product remains adaptable and resilient against rapid shifts in the AI ecosystem.


## Final Thoughts


Once the first usecase is delivered to customers in Beta or GA state, its time to focus on operational aspects:


- **Scalability:** identify bottlenecks in the architecture that impact availability. This could involve adding adding autoscaling capabilities to deployed LLMs, caching repeated computations such as embedding / RAG responses to reduce overall load on downstream systems or simply adding rate limiters on externally exposed APIs. Focussing on observability in the initial days will make this easier
- **OnCall:** Establish team rituals around operational aspects such as on-call rotation to keep track of customer impact. Write runbooks for commonly encountered issue to bring down recovery time etc
- **Customer Feedback:** Monitor quality metrics and customer feedback to understand cases where the MVP approach doesnt work. Now is the time to start the quality hill climb from the MVP.


This is also when the next use case often emerges — sometimes organically. At Box, our first AI use case focused on document Q&A. But customer usage revealed a new need: metadata extraction. Our existing solution didn’t fit. So we kicked off the AI Extract Agent — a purpose-built system leveraging named-entity recognition and tool use. It built upon the platform primitives we had already invested in, like our provider-agnostic llm-gateway, but pushed us further in architectural complexity and product scope.


That single MVP, built by a tiger team of four, eventually became a multi-team AI Platform organization. What began as two services grew into indexing and query pipelines, with dedicated services, vector stores, and specialized teams focused on agents, RAG, foundation models, and prompt engineering.


And the foundational decisions we made early — to stay provider-agnostic, to treat prompts as code, to build a robust evaluation loop — unlocked speed and agility as the landscape evolved. They made it easy to adopt the latest state-of-the-art models without major investment in time or resources.


So if you’re at the beginning of your journey: start simple, but build smart. Focus on solving one real problem well. Lay the groundwork — with evaluation, modularity, and observability — for what comes next.

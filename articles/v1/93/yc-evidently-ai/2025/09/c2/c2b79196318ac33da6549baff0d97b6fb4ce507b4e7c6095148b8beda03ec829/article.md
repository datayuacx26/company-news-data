---
schema_version: "1.0.0"
document_id: "c2b79196318ac33da6549baff0d97b6fb4ce507b4e7c6095148b8beda03ec829"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/gen-ai-use-cases"
published_at: "2025-09-05T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:999405ab1c9536e7d64d44d7c59fd986ee9774575fd22cee824e05f28f16db70"
---

# Gen AI use cases in 2025: learnings from 650+ real-world examples

In 2023, we created a[database](https://www.evidentlyai.com/ml-system-design) of real-world AI and ML use cases, and we’ve been steadily expanding it since. Back then, it was mostly “classic ML” use cases focused on solving predictive problems like classification, regression or recommendations.


Fast forward to today: the collection includes 650+ examples, and a growing share are GenAI applications.


*Use cases by technology*


> 👉 You can check the full database and slice it by categories[here](https://www.evidentlyai.com/ml-system-design) .


Of course, the list of examples is skewed towards companies that actively share how they build things publicly – which means you’ll see plenty of tech firms and platform players.


Here is how we grouped all use cases by application type:


*Use cases by type, across all technologies (predictive ML and Gen AI).*


It’s not a perfect taxonomy, but even with that caveat, some clear patterns stand out.


Let’s walk through some of them.


**A lot of attention goes to the process automation behind the scenes.**


“Ops” is our catch-all tag for backend improvements and automations. These applications range widely, but the spirit is the same: optimize some high-volume workflow, reduce manual effort, and save costs through targeted predictions. For example:


- [Mercado Libre](https://medium.com/mercadolibre-tech/predicting-package-dimensions-based-on-a-similarity-model-at-mercado-libre-d64a9dd4351d) predicts package dimensions for delivery.
- [Datto](https://datto.engineering/post/predicting-hard-drive-failure-with-machine-learning) predicts hard drive failures.
- [Microsoft](https://medium.com/data-science-at-microsoft/ml-and-customer-support-part-2-leveraging-topic-modeling-to-identify-the-top-investment-areas-in-f0348382c251) clusters support issues.


**Search, personalization, and recommender systems are the “bread and butter” of AI applications.**


The earliest wave of ML at scale came from e-commerce and consumer platforms. They often employ ML to show the right content or goods to the right person at the right time – by improving search or through better targeting and personalization of offers.


Think[Pinterest’s content ranking](https://medium.com/pinterest-engineering/modernizing-home-feed-pre-ranking-stage-e636c9cdc36b) or[Uber’s personalization features](https://www.uber.com/en-IN/blog/enhancing-personalized-crm/) .


**Many of the same themes persist even as we move from ML to GenAI.**


What’s striking is how much the same application types continue with GenAI. We’re still often talking about Ops, personalization, search – but with new capabilities layered in.


*Use cases by types, across Gen AI applications.*


**GenAI for Ops now covers even more sophisticated optimizations.**


Automation is still king – just applied to more complex flows. For example:


- [Uber](https://www.uber.com/en-GB/blog/fixrleak-fixing-java-resource-leaks-with-genai/) uses GenAI to fix Java resource leaks.
- [Intuit](https://medium.com/intuit-engineering/revolutionizing-knowledge-discovery-with-genai-to-transform-document-management-0cdf4385c11c) improves document management and knowledge discovery.


**Agents is a category of their own (sort of).**


We singled out “agents” when companies explicitly used the term, though many overlap with Ops. For example:


- [Delivery Hero](https://tech.deliveryhero.com/blog/how-delivery-hero-uses-agentic-ai-for-building-a-product-knowledge-base/) runs agentic AI for product attribute extraction.
- [Uber](https://www.uber.com/en-IN/blog/unlocking-financial-insights-with-finch/) built Finch, a conversational agent for faster access to financial data.


> 💻 More AI agent examples[here](https://www.evidentlyai.com/blog/ai-agents-examples) .


**RecSys and search are reimagined with GenAI**


Search and recommendations are still a core theme, with LLMs adding even better semantic understanding and quality of results. For example:


- [LinkedIn](https://www.linkedin.com/blog/engineering/ai/building-the-next-generation-of-job-search-at-linkedin) implements next-gen job search that goes beyond keywords.
- [Yelp](https://engineeringblog.yelp.com/2025/02/search-query-understanding-with-LLMs.html) also used LLMs to power better search query understanding.
- [Netflix](https://netflixtechblog.com/foundation-model-for-personalized-recommendation-1a0bd8e02d39) created a foundation model for personalized recommendations.


**RAG is one of the most popular newcomer use cases.**


[Retrieval-Augmented Generation (RAG)](https://www.evidentlyai.com/llm-guide/rag-evaluation) has become its own category, with customer support being the most common application. For example:


- [DoorDash](https://careersatdoordash.com/blog/large-language-modules-based-dasher-support-automation/?utm_source=chatgpt.com) created a delivery support chatbot.
- [LinkedIn](https://arxiv.org/pdf/2404.17723?utm_source=chatgpt.com) created RAG with knowledge graph for customer support.


> 🔍 More RAG examples[here](https://www.evidentlyai.com/blog/rag-examples) .


**Honorable mention: AI quality and LLM evaluation**


More and more Gen AI and LLM use cases highlight the importance of[LLM evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) and share the details of how teams safeguard new features and products to ensure AI safety and quality. For example:


- [Klaviyo](https://klaviyo.tech/how-do-i-trust-genai-fd98c314cdf2) uses LLM-as-a-Judge to evaluate LLM-powered features.
- [Instacart](https://tech.instacart.com/turbocharging-customer-support-chatbot-development-with-llm-based-automated-evaluation-6a269aae56b2) developed an LLM-assisted framework to auto-evaluate chatbot interactions.
- [Gusto](https://engineering.gusto.com/tackling-ai-hallucinations-in-llm-apps-6d46692f8cac) uses token log-probabilities to tackle AI hallucinations.


## **Summing up**


So, what do 650+ AI/ML use cases tell us?


- The “classic” ML core themes remain: search, personalization, ops automation.
- GenAI adds new flavors (agents, RAG) but builds on those foundations.
- Ops, in particular, remains a dominant category – automation always pays off.


We’ll keep tracking and expanding this database as the field evolves. In the meantime, explore the full list[here](https://www.evidentlyai.com/ml-system-design) .

---
schema_version: "1.0.0"
document_id: "5b63e3d703ab448fd007f6de8eea9a7b16f1d593a68a5f90f0416de73eaeafcc"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/gen-ai-applications"
published_at: "2025-12-22T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:a210c5026b6d4ee4c29818351bc3302941fb62032bd45a1d5bb249027aaf2ec8"
---

# Learnings from 800+ GenAI and ML use cases

Since 2023, we’ve been continuously curating and updating a database of real-world AI and ML use cases. Today, it includes 800+ production GenAI and ML applications from 185 companies, spanning 2017 to 2025.


These are not demos or experiments – they are AI systems running in production, powering products, operations, and decision-making workflows.


With this breadth of examples, we stepped back to ask a broader question: How are companies actually using AI in production, and how has that evolved over time?


Below, we highlight some of the key patterns we found.


> 💻 You can check the full database[here](https://www.evidentlyai.com/ml-system-design) .


## Database composition


The database contains 805 GenAI and ML use cases, sourced from blog posts, papers, and articles that describe in-house AI systems. While the coverage spans 2017-2025, the majority of examples come from 2023-2025, reflecting the recent acceleration in production AI adoption.


*Use cases by year*


We deliberately focused on systems built internally by companies – not solutions sold or implemented by vendors. This dataset does not include any “case studies” published by vendors or promotional announcements that discuss the existence of a new AI feature – we focused on technology blogs, conference videos, and occasional papers that outline the implementation details.


In total, the dataset covers use cases from 185 companies across a wide range of industries:


*Use cases by industry*


## Use case taxonomy


We grouped use cases by underlying technology. Predictive ML (together with “classic” computer vision and NLP applications) still accounts for about 60% of all examples. However, various GenAI systems already represent 40% of production use cases, despite being only a few years old as a mainstream production technology.


For convenience, we singled out AI agents and Retrieval-Augmented Generation (RAG) systems as a separate category tag, reflecting their growing visibility in real-world applications.


*Use cases by technology*


In parallel, we grouped use cases by recurring application types, such as demand forecasting, fraud detection, content personalization, and others.


> **Note:** Since the[previous update](https://www.evidentlyai.com/blog/gen-ai-use-cases) , we have refined the taxonomy to make application types more specific. For example, data analytics, code generation, and software operations were split out from the broader “ops” category.


*Use cases by application type, across all technologies*


While no taxonomy is perfect, this structure reveals several clear and consistent patterns. Let’s walk through them.


## Most popular use cases


**User-facing AI leads the way.**


Applications where AI powers a specific user-facing feature are labeled as “product feature.” This category spans everything from grammar correction and outfit generation to coding assistance. In all these cases, AI is built directly into the core product the company develops, as opposed to supporting some internal business process.


Creating customer “aha moments” has always been a priority – but the democratization of GenAI and LLMs has made it significantly easier to design and deploy compelling AI-powered features directly into products.


Examples:


- [Grammarly’s](https://www.grammarly.com/blog/engineering/efficient-on-device-writing-assistance/) on-device writing assistant.
- [​​Doordash](https://careersatdoordash.com/blog/doordash-ai-menu-descriptions/) automatically generates menu item descriptions for restaurants.
- [The New York Times'](https://open.nytimes.com/experimenting-with-handwriting-recognition-for-new-york-times-crossword-a78e08fec08f) handwriting recognition for its crossword puzzles.


**A lot of AI value is created behind the scenes.**


Still, companies continue to invest heavily in optimizing high-volume internal workflows. Here, AI often powers use cases such as data analytics and software testing (grouped under “ops” in our taxonomy). While the applications vary depending on the specifics of the business, the objective remains consistent: to reduce costs and effort associated with repetitive processes.


For example:


- [Agoda](https://medium.com/agoda-engineering/improving-security-incident-response-at-agoda-with-large-language-models-78b1f33151e0) uses GenAI to resolve security incidents faster.
- [GoDaddy](https://www.godaddy.com/resources/news/harnessing-ai-to-navigate-millions-of-customer-conversations-at-godaddy) analyzes customer support transcripts.
- [Plaid](https://plaid.com/blog/ai-agents-june-2025/) automates data labeling.


**Search and recommender systems remain core drivers.**


Search and RecSys together account for roughly a quarter of all analyzed use cases. E-commerce and consumer platforms were early adopters, using AI to surface the right products or content at the right time and help with product discovery. Improved search, better targeting, and increasingly sophisticated personalization remain central themes even in the GenAI world:


- [Delivery Hero](https://tech.deliveryhero.com/blog/how-we-improved-multilingual-search-with-few-shot-llm-translations/) builds multilingual search with few-shot LLM translations.
- [Target](https://tech.target.com/blog/accessory-recommendations-with-llms) uses LLMs to improve recommendations for related accessories.
- [Wayfair](https://www.aboutwayfair.com/careers/tech-blog/teaching-wayfairs-catalog-to-see-style-an-llm-powered-style-compatibility-labeling-pipeline-on-google-cloud) uses vision LLMs to identify stylistic compatibility between recommended products.


## Predictive ML vs GenAI use cases


One striking observation is how consistent the application *types* remain even as the underlying technology shifts from predictive ML to GenAI. These use cases we just discussed – search, recommendations, ops, and customer-facing product features continue to dominate – now enhanced by generative capabilities and LLMs’ stronger semantic understanding.


*Use cases by application type, Predictive ML vs. Generative AI and LLM.*


## How use cases evolve over time


We also tracked how use case popularity has changed over time. Below are the top seven use cases across 2021–2025.


*Top seven use cases across 2021–2025, all technologies*


**Search and recommendations remain evergreen.**


Across every year and every technology wave, search and recommender systems remain among the top three AI applications.


**Code generation and data analytics are the new defaults**


With the rise of LLMs, data analytics – such as Text-to-SQL or automated analytical reporting – quickly became a common first use case. Code generation has emerged as another primary application by 2025. Customer support, powered mainly by[RAG-based](https://www.evidentlyai.com/llm-guide/rag-evaluation) chatbots, has also grown significantly.


By contrast, more “classical” predictive ML applications like demand forecasting, fraud detection, or spam moderation still exist – but companies write about them far less frequently today.


Examples of code generation and analytics use cases include:


- [Delivery Hero](https://tech.deliveryhero.com/blog/introducing-the-ai-data-analyst-queryanswerbird-part-1-utilization-of-rag-and-text-to-sql/) helps extract and visualize data with no code.
- [Grab](https://engineering.grab.com/transforming-the-analytics-landscape-with-RAG-powered-LLM?utm_source=substack&utm_medium=email) generates analytical reports using LLMs.
- [Intuit](https://medium.com/intuit-engineering/a-platform-centric-approach-to-ai-assisted-code-generation-at-intuit-03984a85558e) launches coding assistant for developer productivity.
- [ASOS](https://medium.com/asos-techblog/introducing-test-driven-vibe-development-0effe6430691) adopts AI-powered vibe coding.


## AI agents and RAG


We analyzed AI agents and RAG systems as distinct technologies under the GenAI umbrella. Together, they account for roughly 15% of all documented use cases.


The most common agentic applications include various workflow optimizations, such as customer support, analytical tasks, and coding tasks, as well as complex search. For example:


- Uber[uses](https://www.uber.com/en-GB/blog/enhanced-agentic-rag/) an enhanced Agentic RAG (EAg-RAG) to improve the quality of answers provided by its on-call copilot, Genie.
- [Cubic](https://www.cubic.dev/blog/learnings-from-building-ai-agents) created an AI code review agent.
- [Booking’s](https://booking.ai/building-a-genai-agent-for-partner-guest-messaging-f54afb72e6cf) AI agent suggests relevant responses to customer inquiries.
- Ramp built an[agentic data analyst](https://engineering.ramp.com/post/meet-ramp-research) .


For RAG systems, customer support is the dominant use case. Examples:


- [Doordash](https://careers.doordash.com/blog/large-language-modules-based-dasher-support-automation/) built an LLM-based support chatbot.
- Thomson Reuters[helps](https://medium.com/tr-labs-ml-engineering-blog/better-customer-support-using-retrieval-augmented-generation-rag-at-thomson-reuters-4d140a6044c3) customer support executives quickly access the most relevant information.
- Coinbase builds a conversational[support chatbot](https://www.coinbase.com/en-ar/blog/behind-the-scenes-of-the-conversational-coinbase-chatbot) .


A large number of companies also build general-purpose AI assistant chatbots to improve knowledge access for employees: see examples from[Amplitude](https://www.youtube.com/watch?v=9Q9Yrj2RTkg) or[Grab](https://engineering.grab.com/the-birth-of-grab-gpt) .


*Use cases, AI agents*


> 💻 More[agentic AI](https://www.evidentlyai.com/blog/agentic-ai-examples) and[RAG](https://www.evidentlyai.com/blog/rag-examples) examples.


## Summing up


Across 800+ production use cases, a few clear patterns emerge:


- AI is firmly embedded in both user-facing features and backend operations.
- GenAI is rapidly scaling alongside predictive ML, often powering the same applications with new capabilities layered in.
- Search and recommender systems remain the most “evergreen” AI application.
- RAG and AI agents are gaining traction in support, analytics, and complex workflows.

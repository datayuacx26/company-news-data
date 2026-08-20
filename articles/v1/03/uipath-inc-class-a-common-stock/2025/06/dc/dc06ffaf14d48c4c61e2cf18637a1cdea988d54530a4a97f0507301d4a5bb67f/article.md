---
schema_version: "1.0.0"
document_id: "dc06ffaf14d48c4c61e2cf18637a1cdea988d54530a4a97f0507301d4a5bb67f"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/docsai-reshaping-uipath-support-with-intelligent-real-time-documentation-assistance-c1d1a645f6bc"
published_at: "2025-06-30T06:19:18+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:fb78e81c89da31a0f5aaed577fcb4ff4c22e8ce692b7acba66aa7231d6efc3d5"
---

# Smart Search: Reshaping UiPath Support with Generative AI-based Intelligent, Real-Time…

# Smart Search: Reshaping UiPath Support with Generative AI-based Intelligent, Real-Time Documentation Assistance


[Avichal Srivastava](https://medium.com/@avichal_srivastava24?source=post_page---byline--c1d1a645f6bc---------------------------------------)


4 min read


·


Jun 30, 2025


--


## What is Smart Search?


We call it Smart Search — the generative AI-powered documentation search is a retrieval augmented generation[(RAG)](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) -based AI assistant seamlessly integrated into the UiPath ecosystem. At its core, it’s designed to fetch, process, and deliver precise answers to user queries by leveraging the vast UiPath knowledge base, including:


- [UiPath Documentation Portal](https://docs.uipath.com/)
- Knowledge Base (KB) articles


Whether you’re a developer looking for technical guidance, a customer trying to solve a configuration issue, or a support agent aiming to reduce response times — Smart Search serves as your go-to source of truth.


## How it works: behind the scenes


### Simplified workflow


- *Query Submission:* A user inputs a question.
- *Vector Search:* The system queries a vector database using vector similarity search to retrieve the most relevant documents.
- *LLM gateway Interaction:* These documents, combined with the user query and a system prompt, are passed to LLM gateway, powered by GPT-4.
- *Answer Generation:* The LLM returns a rich, context-aware response, complete with source links for users to explore further.


Press enter or click to view image in full size


Smart Search Architecture Press enter or click to view image in full size


Smart Search Runtime Sequence Diagram


### Smart filtering for targeted accuracy


While working across multiple UiPath offerings, the answer to a question can differ based on the specific version and deployment mode of the product, as different configurations or updates may lead to variations in how the system processes and responds. Therefore, implementing filtering mechanisms is essential to ensure that the information provided remains accurate and consistent across different environments and versions.


- *Product-Based Filters:* Whether you’re using UiPath Orchestrator, Automation Suite, or another UiPath product, Smart Search tailors its response accordingly.
- *Deployment-Based Filters:* Whether you’re using UiPath on UiPath Automation Cloud™, on-premises, or in a hybrid setup, Smart Search factors in the deployment context to serve environment-specific information that makes sense for your infrastructure.
- *Version-Based Filters:* Documentation can vary across product versions. Smart Search ensures that answers are relevant to the exact version you’re working with.


### Always up-to-date: real-time sync with Docs andKB


Smart Search is accurate and relevant (real-time relevance):


- Documentation and KB articles are crawled and re-indexed daily.
- Any change or update is reflected in the search within 24 hours.
- Users can rest assured they’re receiving the latest, most accurate information every time.


## Get Avichal Srivastava’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


This ensures not only accurate and contextual answers, but also promotes transparency by highlighting exactly where the information comes from.


## What makes Smart Search stand out?


1. **Contextual answers:** Smart Search is designed to provide responses that are contextually relevant rather than keyword-matched. This drastically improves the quality of assistance, especially when users are navigating complex queries.
2. **Human-like interaction:** By leveraging a conversational interface, Smart Search mimics a human-like interaction model. You ask a question in natural language, and it responds just like a knowledgeable colleague.
3. **Dynamic updates:** The service continually evolves by incorporating new documents, user feedback, and refinements to its AI models — ensuring that responses stay relevant and trustworthy over time.
4. **Fast, reliable, and always available:** Performance is key to a great user experience. Smart Search aims for excellence with *P90 Latency <* seven *seconds* and *P95 Latency <* eight *seconds.*


## Outcomes


### Improving the support landscape


One of the key impact of Smart Search is its integration with the UiPath *Customer Portal’s ticket creation flow.*


- **Ticket deflection rate:** Smart Search currently deflects around *17%* of support tickets. This means users are finding what they need through the GenAI-powered documentation search without raising a ticket.
- **Significant cost savings** : Each ticket has a significant cost attached to it in its process of resolution. With UiPath receiving thousands of tickets annually, this translates to potential savings in the millions.
- **Instant help, seamless experience** : Customer Portal users get their answers in seconds, improving satisfaction and decreasing wait times for those who do require person assistance.


Press enter or click to view image in full size


Smart Search Usage in Ticket Creation Flow


### Availability across the UiPath ecosystem


Smart Search is already available across multiple touch-points:


- UiPath Automation Cloud
- Customer Portal
- UiPath Studio
- Slack integration (Smart Search Slack Bot)
- UiPath Assistant (via UiPath Autopilot™ for Everyone)


Considering its growing footprint, plans are already underway to expand Smart Search across all UiPath products, ensuring universal support coverage.


## Conclusion


Smart Search provides a shift in how support is delivered and experienced. With its AI-first approach, intelligent retrieval, and transparent, source-backed answers, Smart Search empowers users to solve problems faster, easier, and more independently. Whether you’re building automations, debugging errors, or exploring new capabilities, Smart Search is there to support your journey — smartly, efficiently, and instantly.

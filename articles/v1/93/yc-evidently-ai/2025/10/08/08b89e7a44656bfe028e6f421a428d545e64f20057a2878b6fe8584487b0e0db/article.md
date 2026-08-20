---
schema_version: "1.0.0"
document_id: "08b89e7a44656bfe028e6f421a428d545e64f20057a2878b6fe8584487b0e0db"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/agentic-ai-examples"
published_at: "2025-10-30T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:e50218617eeb75915039f5989cb9a2a4f22d0918e75cd13addb8fb5d59ac2eb8"
---

# 7 agentic AI examples and use cases

Agentic AI marks a shift from simpler prompt-response systems to active autonomous collaborators. These systems can reason over multiple steps, plan actions, call external tools or APIs, and adapt based on feedback. They can handle complex, multi-step tasks with minimal human input.


Top companies across various industries are already deploying agentic AI for real impact. Financial institutions use AI agents to automate transaction analysis and compliance checks, e-commerce companies employ them for dynamic recommendations, and engineering teams rely on multi-agent systems for code review and test generation. In this blog, we will explore seven agentic AI examples and use cases in the real world.


## 🚛 Delivery Hero’s data analyst


Delivery Hero built QueryAnswerBird (QAB), an AI-powered data analyst assistant, to enable employees to query, visualize, and discover business data without code. The solution consists of two components:


- The[Text-to-SQL](https://deliveryhero.jobs/blog/introducing-the-ai-data-analyst-queryanswerbird-part-1-utilization-of-rag-and-text-to-sql/) feature is designed to speed data access. It combines an LLM with retrieval-augmented generation (RAG) to access the company’s internal metadata, documentation, and SQL schemas. Key features include natural-language to SQL conversion, query syntax validation, and table/column usage guides.
- The[Data Discovery](https://deliveryhero.jobs/blog/introducing-the-ai-data-analyst-queryanswerbird-part-2-data-discovery/) feature helps users explore data to derive business insights. It utilizes a vector store to manage data from the company’s Data Discovery Platform and the Log Checker, which manages user and event data from apps and websites at the log level. The feature includes information acquisition functions that help improve employees’ data literacy by explaining queries and tables and providing guidance on utilizing log data.


Here’s how it works: a user asks a question in Slack, QAB retrieves relevant internal data definitions and examples via vector search, crafts a compliant SQL query tailored to business logic, validates it, and returns the result.


*Architecture of QueryAnswerBird (QAB). Credit:*[Introducing the AI Data Analyst “QueryAnswerBird” – Part 1. Utilization of RAG and Text-to-SQL](https://deliveryhero.jobs/blog/introducing-the-ai-data-analyst-queryanswerbird-part-1-utilization-of-rag-and-text-to-sql/)


## 🛍 eBay’s agentic platform for RecSys


[Mercury](https://www.linkedin.com/pulse/mercury-agentic-ai-platform-llm-powered-experiences-ebay-chowdhury-kka8e/) is eBay’s internal agentic AI platform. It powers LLM-driven recommendation experiences on the marketplace and lets teams efficiently build and scale autonomous, goal-oriented AI workflows:


- The platform integrates RAG to combine LLM’s outputs with real-time, domain-specific data, ensuring recommendations stay accurate and current.
- The Listing Matching Engine bridges the gap between LLM text outputs and eBay’s two-billion-item inventory by converting textual suggestions into relevant live listings.


Additionally, the platform includes internal models to detect and prevent[prompt injection](https://www.evidentlyai.com/llm-guide/prompt-injection-llm) attempts by malicious actors.


*Different agent structures in Mercury. Credit:*[Mercury: Agentic AI Platform for LLM Powered Recommendation Experiences at eBay](https://www.linkedin.com/pulse/mercury-agentic-ai-platform-llm-powered-experiences-ebay-chowdhury-kka8e/)


## 🚘 Uber’s enhanced Agentic RAG


Uber[uses](https://www.uber.com/en-GB/blog/enhanced-agentic-rag/) enhanced Agentic RAG (EAg-RAG) to improve the answer quality of its on-call copilot, Genie. To ensure near-human answer precision of the copilot, Uber adds AI agents at multiple stages:


- Query Optimizer reformulates or breaks down ambiguous queries.
- Source Identifier narrows down the document set.
- Post-Processor Agent deduplicates and orders the retrieved context.


As a result, the share of acceptable answers increased by 27%, and incorrect advice was reduced by 60% compared to traditional RAG architecture.


*End-to-end workflow of EAg-RAG architecture for Uber’s on-call copilot. Credit:*[Enhanced Agentic-RAG: What If Chatbots Could Deliver Near-Human Precision?](https://www.uber.com/en-GB/blog/enhanced-agentic-rag/)


## 🎙 Salesforce’s AI agent for event management


Salesforce[built](https://www.salesforce.com/blog/build-an-ai-agent/) the Ask Astro agent, which is integrated into the Salesforce Events mobile app. The event agent enables attendees to ask natural-language questions, get session recommendations, and manage their personal schedules.


​​Ask Astro ingests structured event data, like session schedules, and unstructured FAQs, indexed using a hybrid search architecture to improve accuracy. For example, when a user asks, “Which sessions is Marc Benioff doing on Tuesday?” the agent identifies the topic, retrieves sessions, filters by time and speaker, and returns grounded answers.


*Data flow for the Ask Astro agent. Credit:*[How We Built the First Dreamforce Event Agent in 5 Days with Agentforce](https://www.salesforce.com/blog/build-an-ai-agent/)


## 💻 Google’s coding agent


Google[developed](https://www.youtube.com/watch?v=X4BwOu0GWb8) Jules, a massively parallel asynchronous AI coding agent. It assists developers by autonomously performing common coding tasks – for example, it can fix bugs, write tests, and update dependencies. Jules runs tasks in the background: it creates a plan, executes it, runs tests, and reports back with completed pull requests.


It can run multiple variations of tasks in parallel (e.g., testing different frameworks) and allows developers to review or merge the best outcomes.


*Example use case for Jules. Credit:*[Jules documentation](https://jules.google/docs/changelog)


## 💼 LinkedIn’s agentic Hiring Assistant


LinkedIn’s[Hiring Assistant](https://www.linkedin.com/blog/engineering/ai/how-we-engineered-linkedins-hiring-assistant) is an AI agent designed to automate and support recruiters' hiring workflows. It helps with tasks such as drafting outreach messages, generating screening questions, and sourcing candidates by leveraging LinkedIn’s extensive recruitment data ecosystem.


It is a modular multi-agent system where one “supervisory agent” orchestrates a set of sub-agents, each handling a discrete function (e.g., drafting job description, sourcing candidates, messaging, ranking). The orchestrator coordinates the workflow by decomposing recruiter requests into tasks, invoking the appropriate sub-agent, aggregating their outputs, and presenting the results to the recruiter.


Hiring Assistant is modeled as separate agent instances, each tied to its own recruiter.


*High-level cognitive architecture of Hiring Assistant. Credit:*[Building the agentic future of recruiting: how we engineered LinkedIn’s Hiring Assistant](https://www.linkedin.com/blog/engineering/ai/how-we-engineered-linkedins-hiring-assistant)


## 💬 Booking’s agent for messaging


Booking’s[AI agent](https://booking.ai/building-a-genai-agent-for-partner-guest-messaging-f54afb72e6cf) is designed to streamline communication between accommodation partners and guests by automatically suggesting responses to guest inquiries. It handles incoming messages, such as questions about check-in times, parking, or special requests.


In response to an incoming message, the system can either:


- Return a predefined partner-template answer,
- Generate a free-text custom response when no template fits, or
- Opt to not respond automatically if the system lacks sufficient context or the message is restricted.


The system employs a tool-calling architecture that embeds guest messages, retrieves relevant templates via vector search, pulls contextual data such as reservation and property information, and then reasons using an LLM to determine the best action. The agent runs as a scalable microservice and supports multilingual scenarios.


*Booking’s AI agent design overview. Credit:*[Building a GenAI Agent for Partner-Guest Messaging](https://booking.ai/building-a-genai-agent-for-partner-guest-messaging-f54afb72e6cf)


## Evaluate AI agents with Evidently


The examples covered here show that agentic AI no longer exists only as demos, but as production deployments delivering measurable business value.


If you are building complex systems like AI agents, you need[evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) to make sure they work as expected – both during development and in production. That’s why we built[Evidently](https://www.evidentlyai.com/) . Our open-source library, with over 30 million downloads, makes it easy to test and evaluate LLM-powered applications, including AI agents.


We also provide[Evidently Cloud](https://www.evidentlyai.com/register) , a no-code workspace for teams to collaborate on AI quality, testing, and monitoring, and run complex evaluation workflows. You can generate synthetic data, create evaluation scenarios, run adversarial tests, and track performance – all in one place.


Ready to test your AI agent?[Sign up for free](https://www.evidentlyai.com/register) or[schedule a demo](https://www.evidentlyai.com/get-demo) to see Evidently Cloud in action. We're here to help you build with confidence!

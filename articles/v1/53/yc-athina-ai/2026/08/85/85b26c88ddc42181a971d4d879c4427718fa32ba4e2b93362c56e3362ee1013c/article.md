---
schema_version: "1.0.0"
document_id: "85b26c88ddc42181a971d4d879c4427718fa32ba4e2b93362c56e3362ee1013c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-e-com-giant-meesho-accelerated-llm-development-using-athina-ai"
published_at: "2026-08-20T00:55:52.946+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:50c331f53186a7c3213968f27b8e623e5518199e720d406b6f28113d168aeae4"
---

# How E-com Giant Meesho Accelerated LLM Development Using Athina AI

Do not index


Original Paper


Blog URL


**[Meesho](https://meesho.com/)** **,** a leading e-commerce platform, serves over 120 million customers and is valued at over $5 billion.


As a fast-growing company, Meesho needed to scale its customer support operations while maintaining high customer satisfaction (CSAT) scores.


To do so, they explored the potential of LLM agents to handle high volumes of customer queries with speed and consistency. However, deploying these agents at scale introduced new challenges, especially around reliability and observability.


🔥


####


Highlights


- *With Athina, Meesho reduced its per-chat evaluation costs by* ***80-85%***


- *On average Meesho was able to achieve a* ***14-15% cost optimization*** *on each chat.*


- Meesho was able to upgrade their existing models to GPT-4o mini and rolled out to **Millions** of users in **just 3 days**


###


Why Use LLM Agents?


Customer support is essential to Meesho’s user experience, particularly during peak seasons when the number of customer queries surges. Maintaining high CSAT scores during these times means managing wait times while providing accurate and effective support. However, relying solely on human agents posed significant challenges:


- **Training time:** Agents require extensive training on SOPs (Standard Operating Procedures) across languages and products.


- **Scalability:** Fluctuating demand is hard to match with a fixed number of agents, leading to delays during busy periods.


- **Attrition rates:** High turnover increases costs and training time.


Using LLM agents allowed Meesho to handle more customer interactions autonomously. These agents could:


- Triage queries, allowing human agents to focus on more complex issues.


- Answer repetitive queries, reducing the load on support teams.


- Scale flexibly according to demand, especially during peak seasons.


###


Why Athina?


While LLMs offered several benefits, ensuring their effectiveness and accuracy was critical. Meesho’s support team observed that LLM agents faced unique challenges, such as:


- Responding in a language different from the customer’s original query.


- Requesting PII or sensitive information.


- Using complex language that some customers found confusing.


Meesho needed a solution to evaluate LLM agents on custom criteria, mirroring the way human agents are assessed (e.g., response times, CSAT scores) while addressing these specific challenges.


###


The Search for an Automated Solution


Manual audits of chat history helped them identify some issues, but this approach was time-consuming, error-prone, and lacked scalability.


They needed a way to automatically evaluate LLM performance across thousands of interactions and quickly address potential issues.


###


Solution


####


1. Production Observability and Automatic Evaluations


With Athina, Meesho implemented an automated system for evaluating LLM responses. This allowed them to:


- **Automate evaluations** : Instead of manually reviewing chats, Meesho could automatically flag responses that failed to meet their criteria, allowing their team to focus on debugging rather than detection.


- **Set custom evaluation criteria** : Athina helped them ensure LLM responses met specific standards, such as language consistency, proper adherence to SOPs, and user-friendly language.


With Athina’s observability and evaluation platform, Meesho gained new insights into their LLM systems’ performance. Some of the key impacts included:


**1. Faster Debugging**


Before Athina, reviewing even a sample of 1,000 chats across ten criteria was labor-intensive. Now, Athina **automatically flags responses that don’t meet the criteria** , enabling Meesho’s team to review only flagged cases, which significantly sped up their debugging process.


**2. Trend Analysis**


Rather than focusing on individual metrics, Meesho began using trends to monitor LLM system health. Athina’s dashboards allowed them to spot issues quickly, often triggered by model updates, edge case additions, or knowledge base changes. By tracking these trends, they could swiftly identify the root cause and make adjustments.


####


2. Beyond Observability: Experimentation with Athina IDE


Meesho wanted to experiment with different prompts, models, and retrieval systems to improve performance. However, running these tests in a live environment was risky.


Athina IDE provided the perfect platform for running rapid, data-driven experiments. Using real-user data from production logs, Meesho could evaluate various setups, iterate quickly, and only deploy the best-performing configurations in production.


####


Pre-Athina vs. Post-Athina


Before adopting Athina, Meesho randomly sampled 1-2% of chat logs to assess adherence to SOPs.


They struggled to identify why some interactions didn’t align with SOPs, whether due to insufficient SOPs, model limitations, or other factors.


Athina’s evaluation framework allowed them to identify these issues with data-driven insights. By benchmarking various scenarios (e.g., order delays, model types, and user intents), Meesho could better understand LLM performance across different contexts.


####


Rapid Rollout of GPT-4o mini


When GPT-4o mini launched, Meesho wanted to upgrade their systems to leverage its capabilities for improved user experience and cost savings.


However, deploying this model across millions of users required careful evaluation. Using Athina, they:


1. **Ran comparative experiments** on existing datasets to assess GPT-4o mini’s performance against their current models.


1. **Conducted phased rollouts** : After successful testing, they deployed GPT-4o mini to a small user group, tracked metrics, and fine-tuned prompts.


1. **Achieved full deployment in three days** : The Athina platform allowed them to iterate quickly, addressing any issues in real time and expanding the rollout to their full user base.


###


Let us Help you Ship LLMs to Production


[Athina](https://athina.ai/) helps AI teams during their entire LLM lifecycle. Whether you’re looking for a platform to rapidly iterate over prompts, models or retrieval systems or looking for production observability, we’re here to help.


We work with leading AI teams, including **Perplexity, Meesho, and Doximity** , and understand what it takes to build production-grade LLM applications.


Let’s chat to see how we can help -[Schedule a Call](https://cal.com/shiv-athina/30min)

---
schema_version: "1.0.0"
document_id: "16351b5804ec9e1de64792a0e9ede370db9dfc6bbaecc7ad72903d06cf08e6a4"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/exploring-llm-based-agents-for-root-cause-analysis"
published_at: "2024-03-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:82016d0a8875d476768eff936a57a9aed2ddda18032ba2f1f9f1f60a73644575"
---

# Exploring LLM-based Agents for Root Cause Analysis

Do not index


Original Paper


Blog URL


**Original Paper:**[https://arxiv.org/abs/2403.04123](https://arxiv.org/abs/2403.04123)


**By:**[Devjeet Roy](https://arxiv.org/search/cs?searchtype=author&query=Roy%2C%20D) ,[Xuchao Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20X) ,[Rashi Bhave](https://arxiv.org/search/cs?searchtype=author&query=Bhave%2C%20R) ,[Chetan Bansal](https://arxiv.org/search/cs?searchtype=author&query=Bansal%2C%20C) ,[Pedro Las-Casas](https://arxiv.org/search/cs?searchtype=author&query=Las-Casas%2C%20P) ,[Rodrigo Fonseca](https://arxiv.org/search/cs?searchtype=author&query=Fonseca%2C%20R) ,[Saravan Rajmohan](https://arxiv.org/search/cs?searchtype=author&query=Rajmohan%2C%20S)


**Abstract:**


> The growing complexity of cloud based software systems has resulted in incident management becoming an integral part of the software development lifecycle. Root cause analysis (RCA), a critical part of the incident management process, is a demanding task for on-call engineers, requiring deep domain knowledge and extensive experience with a team's specific services. Automation of RCA can result in significant savings of time, and ease the burden of incident management on on-call engineers. Recently, researchers have utilized Large Language Models (LLMs) to perform RCA, and have demonstrated promising results. However, these approaches are not able to dynamically collect additional diagnostic information such as incident related logs, metrics or databases, severely restricting their ability to diagnose root causes. In this work, we explore the use of LLM based agents for RCA to address this limitation. We present a thorough empirical evaluation of a ReAct agent equipped with retrieval tools, on an out-of-distribution dataset of production incidents collected at Microsoft. Results show that ReAct performs competitively with strong retrieval and reasoning baselines, but with highly increased factual accuracy. We then extend this evaluation by incorporating discussions associated with incident reports as additional inputs for the models, which surprisingly does not yield significant performance improvements. Lastly, we conduct a case study with a team at Microsoft to equip the ReAct agent with tools that give it access to external diagnostic services that are used by the team for manual RCA. Our results show how agents can overcome the limitations of prior work, and practical considerations for implementing such a system in practice.


---


###


Summary Notes


####


Automating Root Cause Analysis with AI: A Closer Look


In the fast-paced realm of cloud software, incident management is crucial. Root Cause Analysis (RCA) is a key part of this, but it's becoming harder as systems grow more complex.


Enter Large Language Models (LLMs) - a promising tool for automating RCA, yet not without hurdles, especially in gathering extra diagnostic data. This post explores how LLMs are changing the game for AI Engineers in big companies.


###


Understanding the Basics


Complex software systems often face issues, making effective incident management and RCA critical. While traditional RCA methods have used machine learning and anomaly detection, LLMs are now on the scene. They’re particularly good at RCA, thanks to their ability to reason and interact, though they still need access to dynamic data.


###


The Role of LLM-Based Agents in RCA


The ReAct framework is a standout in RCA, known for its reasoning, planning, and interaction abilities. Our evaluation focused on ReAct in a zero-shot setting - meaning it used a fixed dataset without special diagnostic tools. This helped us understand its strengths and limits.


####


Key Research Questions


We looked into:


- How well do LLM-based agents perform in RCA with basic tools?


- Do discussion comments affect their performance?


- Can these agents handle real-world scenarios with specific tools?


####


Our Approach to Evaluation


We used 107,000 incident reports and the OpenAI GPT4-8k model for our study. Our goal was to test the agents without any prior training (zero-shot) and to qualitatively assess where they succeeded and where they didn’t.


###


What We Found


Our evaluation showed:


- ReAct agents are quite good at pinpointing root causes with general tools, though not perfectly accurate.


- Adding discussion comments didn’t really improve performance, suggesting we need better strategies.


- A real-world test with the Azure Fundamental Team highlighted the importance of Knowledge Base Articles (KBAs), tool integration, and the irreplaceable value of human oversight in complex situations.


###


Key Takeaways for Practitioners


From this study, several points stand out for AI Engineers:


- **Knowledge Base Articles (KBAs)** are vital for embedding domain know-how and aiding RCA.


- **Tool Integration** needs thoughtful design to be effective.


- **Learning from Experience and Multiple Attempts** might boost agent performance in intricate cases.


- **Human Oversight** is essential for trust and safety, especially in critical operations.


####


Wrapping Up


Our deep dive into LLM-based agents for RCA unveils their potential to automate complex decision-making in cloud incident management.


The results are promising but also highlight challenges and future research directions, like creating a simulated RCA environment and further exploring LLMs’ practical uses in RCA. As technology advances, the synergy between AI and human expertise remains key to evolving incident management practices.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

---
schema_version: "1.0.0"
document_id: "79b1e4a70b6bb8f6704c222da14e0a110d49a5aaf31bc55def35da4de486625e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/from-prompt-injections-to-sql-injection-attacks-how-protected-is-your-llm-integrated-web-application"
published_at: "2023-08-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:42eb271fa9f1a9c86b118512994e4e00a971853e58d602afc1c67dd6c48db1c5"
---

# From Prompt Injections to SQL Injection Attacks: How Protected is Your LLM-Integrated Web Application?

Do not index


Original Paper


[https://arxiv.org/abs/2308.01990](https://arxiv.org/abs/2308.01990)


Blog URL


[blog.athina.ai /from-pr...lication](https://blog.athina.ai/from-prompt-injections-to-sql-injection-attacks-how-protected-is-your-llm-integrated-web-application)


**Original Paper:**[https://arxiv.org/abs/2308.01990](https://arxiv.org/abs/2308.01990)


**By:**[Rodrigo Pedro](https://arxiv.org/search/cs?searchtype=author&query=Pedro%2C%20R) ,[Daniel Castro](https://arxiv.org/search/cs?searchtype=author&query=Castro%2C%20D) ,[Paulo Carreira](https://arxiv.org/search/cs?searchtype=author&query=Carreira%2C%20P) ,[Nuno Santos](https://arxiv.org/search/cs?searchtype=author&query=Santos%2C%20N)


**Abstract:**


> Large Language Models (LLMs) have found widespread applications in various domains, including web applications, where they facilitate human interaction via chatbots with natural language interfaces. Internally, aided by an LLM-integration middleware such as Langchain, user prompts are translated into SQL queries used by the LLM to provide meaningful responses to users. However, unsanitized user prompts can lead to SQL injection attacks, potentially compromising the security of the database. Despite the growing interest in prompt injection vulnerabilities targeting LLMs, the specific risks of generating SQL injection attacks through prompt injections have not been extensively studied. In this paper, we present a comprehensive examination of prompt-to-SQL (P2SQL) injections targeting web applications based on the Langchain framework. Using Langchain as our case study, we characterize P2SQL injections, exploring their variants and impact on application security through multiple concrete examples. Furthermore, we evaluate 7 state-of-the-art LLMs, demonstrating the pervasiveness of P2SQL attacks across language models. Our findings indicate that LLM-integrated applications based on Langchain are highly susceptible to P2SQL injection attacks, warranting the adoption of robust defenses. To counter these attacks, we propose four effective defense techniques that can be integrated as extensions to the Langchain framework. We validate the defenses through an experimental evaluation with a real-world use case application.


---


###


Summary Notes


##


Protecting Your LLM-Integrated Web Application from SQL Injection Attacks


As conversational AI and Large Language Models (LLMs) like GPT-4 and Llama 2 become integral to web applications, understanding and mitigating security risks is crucial for AI Engineers.


Recent research by Rodrigo Pedro and colleagues highlights the vulnerability of LLM-integrated applications to SQL injection attacks through prompt injections. This post aims to equip technical professionals with the knowledge to strengthen their applications against such threats.


###


Understanding the Threat


LLMs have transformed user interactions with web applications through natural language interfaces. However, this progress also introduces risks, notably SQL injection attacks via prompt injections.


The research focuses on Langchain middleware, which translates user prompts into SQL queries, demonstrating a security challenge if user inputs aren't properly checked.


###


Exploring the Vulnerabilities


The study addresses three main questions:


- **RQ1** : Identifies feasible P2SQL (prompt-to-SQL) injection attacks and their impacts.


- **RQ2** : Examines the vulnerability of various LLMs to P2SQL attacks.


- **RQ3** : Discusses effective defense mechanisms.


It reveals that LLMs can mistakenly turn malicious prompts into dangerous SQL queries, compromising database security.


###


Key Findings


- **Types of Attacks** : SQL injections are classified into direct and indirect, where attackers manipulate user inputs or the database to run harmful queries.


- **LLM Vulnerability** : All tested LLMs were susceptible to P2SQL attacks, indicating a widespread issue.


- **Defensive Strategies** : The study suggests several defenses, including database permission restrictions and SQL query rewriting, but emphasizes the need for more comprehensive solutions.


###


Enhancing LLM Integration Security


To better protect LLM-integrated applications, consider the following strategies:


- **Database Permission Hardening** : Restrict database permissions to essential operations only.


- **SQL Query Rewriting** : Use middleware to rewrite or validate SQL queries against safe queries or patterns.


- **Auxiliary LLM Validation** : Deploy an additional LLM to check user inputs or SQL queries for malicious intent.


- **In-Prompt Data Preloading** : Minimize direct database queries by preloading data within prompts, reducing attack vectors.


###


Conclusion


While LLM integration offers significant benefits for user interaction, it also introduces notable security risks, particularly P2SQL injection vulnerabilities.


The research emphasizes the urgent need for better security measures and practices. By implementing layered defenses and promoting security awareness, companies can address the challenges posed by these advanced technologies.


###


Contributions to the Field


This research provides critical insights into P2SQL injection vulnerabilities, contributing significantly to the understanding and mitigation of risks in LLM-integrated applications. It lays the foundation for future research and defense mechanisms, aiming for safer human-machine interactions.


As AI and cybersecurity landscapes evolve, staying vigilant, innovative, and collaborative is key to ensuring the safe deployment of these technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

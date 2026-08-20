---
schema_version: "1.0.0"
document_id: "aa7c65d617f96679adcaee004685a8fa4ccdff0177d66b985d661372e044291c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-injection-attack-against-llm-integrated-applications"
published_at: "2024-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:125b7c219d696d37db068180be6b7dc9e50aa73e66f8d3c471e3820311b03329"
---

# Prompt Injection attack against LLM-integrated Applications

Do not index


Original Paper


[https://arxiv.org/abs/2306.05499](https://arxiv.org/abs/2306.05499)


Blog URL


[blog.athina.ai /prompt-...ications](https://blog.athina.ai/prompt-injection-attack-against-llm-integrated-applications)


**Original Paper:**[https://arxiv.org/abs/2306.05499](https://arxiv.org/abs/2306.05499)


**By:**[Yi Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Y) ,[Gelei Deng](https://arxiv.org/search/cs?searchtype=author&query=Deng%2C%20G) ,[Yuekang Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Y) ,[Kailong Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20K) ,[Zihao Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Xiaofeng Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20X) ,[Tianwei Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20T) ,[Yepang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Y) ,[Haoyu Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20H) ,[Yan Zheng](https://arxiv.org/search/cs?searchtype=author&query=Zheng%2C%20Y) ,[Yang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Y)


**Abstract:**


> Large Language Models (LLMs), renowned for their superior proficiency in language comprehension and generation, stimulate a vibrant ecosystem of applications around them. However, their extensive assimilation into various services introduces significant security risks. This study deconstructs the complexities and implications of prompt injection attacks on actual LLM-integrated applications. Initially, we conduct an exploratory analysis on ten commercial applications, highlighting the constraints of current attack strategies in practice. Prompted by these limitations, we subsequently formulate HouYi, a novel black-box prompt injection attack technique, which draws inspiration from traditional web injection attacks. HouYi is compartmentalized into three crucial elements: a seamlessly-incorporated pre-constructed prompt, an injection prompt inducing context partition, and a malicious payload designed to fulfill the attack objectives. Leveraging HouYi, we unveil previously unknown and severe attack outcomes, such as unrestricted arbitrary LLM usage and uncomplicated application prompt theft. We deploy HouYi on 36 actual LLM-integrated applications and discern 31 applications susceptible to prompt injection. 10 vendors have validated our discoveries, including Notion, which has the potential to impact millions of users. Our investigation illuminates both the possible risks of prompt injection attacks and the possible tactics for mitigation.


---


###


Summary Notes


####


The HOUYI Method Against Prompt Injection Attacks


With Large Language Models (LLMs) like GPT-4 and PaLM2 transforming business operations, the shift to AI-driven applications is in full swing.


These technologies are revolutionizing data processing, content creation, and user interaction. However, this integration introduces new security risks, notably prompt injection attacks, which pose a significant threat to application integrity and security.


This post explores the dangers of prompt injection attacks on LLM-integrated applications and introduces HOUYI, a cutting-edge defense strategy.


####


Understanding the Threat: Prompt Injection Attacks


Prompt injection attacks manipulate LLM outputs by altering or inserting prompts, leading to unauthorized actions or data breaches. These attacks come in various forms:


- **Direct Injection:** Malicious inputs directly fed into the LLM.


- **Escape Characters:** Using special characters to change how prompts are processed.


- **Context Ignoring:** Inputs designed to make the LLM overlook the intended context.


Attackers leveraging these methods aim to manipulate outputs without accessing the application's internals.


####


The Vulnerability Spotlight


A study on ten commercial applications with LLM integration uncovered a high risk of prompt injection attacks. This vulnerability stems from the wide range of prompt usage and the unique designs of these applications, challenging existing defense strategies.


####


HOUYI: A Tailored Defense Mechanism


Inspired by defenses against SQL and XSS attacks, HOUYI addresses the prompt injection challenge through a three-phase process:


1. **Context Inference:** Determines the LLM's operational context.


1. **Payload Generation:** Creates a query seen as legitimate by the LLM.


1. **Feedback:** Uses application responses to refine defenses or attacks.


HOUYI aims to distinguish between malicious commands and legitimate context, thus blocking unwanted actions.


####


How HOUYI Shields Your Application


HOUYI integrates into applications with three main components:


- **Framework Component:** Monitors and analyzes prompts in real-time.


- **Separator Component:** Shifts LLM focus from current context to secure commands.


- **Disruptor Component:** Actively neutralizes malicious intents.


This strategy's adaptability, based on application feedback, is key to its effectiveness.


####


Proven Effectiveness


In real-world tests across 36 applications, HOUYI successfully mitigated prompt injection attacks with an 86.1% success rate. This not only enhances security but also addresses potential financial risks, such as unauthorized resource use and data theft.


####


Final Thoughts


The integration of LLMs into business applications ushers in a new era of innovation but also introduces complex security challenges like prompt injection attacks. Implementing defense measures like HOUYI is essential for maintaining application integrity and security.


As AI becomes increasingly embedded in enterprise solutions, the need for dynamic, robust security measures becomes paramount. Protecting against prompt injection attacks is crucial for preserving trust and reliability in AI applications.


####


Dive Deeper


This blog synthesizes findings from extensive research on prompt injection vulnerabilities and the development of the HOUYI defense strategy. For more detailed insights, refer to the original research.


####


Visuals and Data


- **Figures** illustrate user interactions with AI applications, workflow examples, and comparisons between SQL and prompt injections.


- **Tables** summarize attack effectiveness, detail HOUYI components, and list disruptor components for various scenarios.


This combination of theory and practical application provides AI engineers and security professionals with the tools needed to secure the AI-driven future.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

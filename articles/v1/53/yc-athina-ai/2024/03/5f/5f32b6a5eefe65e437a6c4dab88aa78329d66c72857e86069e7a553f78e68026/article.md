---
schema_version: "1.0.0"
document_id: "5f32b6a5eefe65e437a6c4dab88aa78329d66c72857e86069e7a553f78e68026"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/benchmarking-and-defending-against-indirect-prompt-injection-attacks-on-large-language-models"
published_at: "2024-03-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:c5c50516d37dc257bb78d60908bbacceb4bd8ac61af17c8b34d8f45789c47744"
---

# Benchmarking and Defending Against Indirect Prompt Injection Attacks on Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2312.14197](https://arxiv.org/abs/2312.14197)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2312.14197](https://arxiv.org/abs/2312.14197)


**By:**[Jingwei Yi](https://arxiv.org/search/cs?searchtype=author&query=Yi%2C%20J) ,[Yueqi Xie](https://arxiv.org/search/cs?searchtype=author&query=Xie%2C%20Y) ,[Bin Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20B) ,[Emre Kiciman](https://arxiv.org/search/cs?searchtype=author&query=Kiciman%2C%20E) ,[Guangzhong Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20G) ,[Xing Xie](https://arxiv.org/search/cs?searchtype=author&query=Xie%2C%20X) ,[Fangzhao Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20F)


**Abstract:**


> The integration of large language models (LLMs) with external content has enabled more up-to-date and wide-ranging applications of LLMs, such as Microsoft Copilot. However, this integration has also exposed LLMs to the risk of indirect prompt injection attacks, where an attacker can embed malicious instructions within external content, compromising LLM output and causing responses to deviate from user expectations. To investigate this important but underexplored issue, we introduce the first benchmark for indirect prompt injection attacks, named BIPIA, to evaluate the risk of such attacks. Based on the evaluation, our work makes a key analysis of the underlying reason for the success of the attack, namely the inability of LLMs to distinguish between instructions and external content and the absence of LLMs' awareness to not execute instructions within external content. Building upon this analysis, we develop two black-box methods based on prompt learning and a white-box defense method based on fine-tuning with adversarial training accordingly. Experimental results demonstrate that black-box defenses are highly effective in mitigating these attacks, while the white-box defense reduces the attack success rate to near-zero levels. Overall, our work systematically investigates indirect prompt injection attacks by introducing a benchmark, analyzing the underlying reason for the success of the attack, and developing an initial set of defenses.


---


###


Summary Notes


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models


Introducing BIPIA and developing tailored defense strategies marks a step forward in securing LLM applications against indirect prompt injection attacks.


This comprehensive framework for evaluation and mitigation ensures LLMs can be used safely and effectively in diverse real-world applications.


####


Conclusion


While current defenses offer significant protection, the evolving nature of cybersecurity threats means ongoing adjustments and validations are necessary.


Future research will focus on refining these defenses and expanding BIPIA to encompass new attack vectors.


####


Looking Ahead


Implementing these defense strategies significantly lowered the Attack Success Rate (ASR) across tested LLMs, especially with white-box defenses, which nearly eliminated ASR. These methods also preserved the models' performance on benign inputs, ensuring their utility wasn't compromised for security.


####


Evaluating Defense Effectiveness


- **Embedding Layer Modification** : Adjusts the model's embedding layer to better distinguish between legitimate and malicious inputs.


- **Adversarial Training** : Involves training models with examples of indirect prompt injections to improve their resistance.


**White-box Defenses**


- **In-Context Learning** : Teaches the model to identify and ignore malicious content through input context examples.


- **Multi-turn Dialogue** : Engages the model in clarifying dialogues for ambiguous or harmful instructions.


**Black-box Defenses**


The proposed defense strategies are categorized into black-box and white-box approaches:


####


Strengthening Defenses: Strategies and Mechanisms


LLMs struggle to differentiate between legitimate and maliciously crafted content due to their design and operation principles.


This issue, combined with their reliance on external content, amplifies their susceptibility to indirect prompt injections.


####


Identifying Weaknesses


Testing 25 LLMs, the research highlights a widespread vulnerability to these attacks.


- Code QA


- Summarization


- Table QA


- Web QA


- Email QA


The Benchmark for Indirect Prompt Injection Attacks (BIPIA) evaluates LLM vulnerability across various scenarios:


####


The BIPIA Benchmark: Assessing LLM Vulnerability


Indirect prompt injection attacks subtly embed malicious commands in external content that LLMs might use, aiming to manipulate the model's outputs.


Unlike direct attacks that tamper with input directly, these indirect methods are stealthier, potentially leading to misinformation spread or content filter bypass.


####


What are Indirect Prompt Injection Attacks?


As artificial intelligence (AI) continues to grow, large language models (LLMs) are becoming increasingly central in various applications like email responses and code generation.


Yet, their integration with external content exposes them to new security threats, notably indirect prompt injection attacks. This blog post explores these attacks, their impact on LLMs, and presents effective defense strategies, based on insights from Jingwei Yi et al.'s recent research.


####


Benchmarking and Defending Against Indirect Prompt Injection Attacks on Large Language Models

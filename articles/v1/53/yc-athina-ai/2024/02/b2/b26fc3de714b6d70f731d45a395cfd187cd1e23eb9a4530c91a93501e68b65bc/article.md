---
schema_version: "1.0.0"
document_id: "b26fc3de714b6d70f731d45a395cfd187cd1e23eb9a4530c91a93501e68b65bc"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/tree-of-attacks-jailbreaking-black-box-llms-automatically"
published_at: "2024-02-21T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:0def4e5b38405189a2bbcd3fbda0fcbe033d1c4462c3c94d30a791f5cd052b20"
---

# Tree of Attacks: Jailbreaking Black-Box LLMs Automatically

Do not index


Original Paper


[https://arxiv.org/abs/2312.02119](https://arxiv.org/abs/2312.02119)


Blog URL


[blog.athina.ai /tree-of...atically](https://blog.athina.ai/tree-of-attacks-jailbreaking-black-box-llms-automatically)


**Original Paper:**[https://arxiv.org/abs/2312.02119](https://arxiv.org/abs/2312.02119)


**By:**[Anay Mehrotra](https://arxiv.org/search/cs?searchtype=author&query=Mehrotra%2C%20A) ,[Manolis Zampetakis](https://arxiv.org/search/cs?searchtype=author&query=Zampetakis%2C%20M) ,[Paul Kassianik](https://arxiv.org/search/cs?searchtype=author&query=Kassianik%2C%20P) ,[Blaine Nelson](https://arxiv.org/search/cs?searchtype=author&query=Nelson%2C%20B) ,[Hyrum Anderson](https://arxiv.org/search/cs?searchtype=author&query=Anderson%2C%20H) ,[Yaron Singer](https://arxiv.org/search/cs?searchtype=author&query=Singer%2C%20Y) ,[Amin Karbasi](https://arxiv.org/search/cs?searchtype=author&query=Karbasi%2C%20A)


**Abstract:**


> While Large Language Models (LLMs) display versatile functionality, they continue to generate harmful, biased, and toxic content, as demonstrated by the prevalence of human-designed jailbreaks. In this work, we present Tree of Attacks with Pruning (TAP), an automated method for generating jailbreaks that only requires black-box access to the target LLM. TAP utilizes an LLM to iteratively refine candidate (attack) prompts using tree-of-thought reasoning until one of the generated prompts jailbreaks the target. Crucially, before sending prompts to the target, TAP assesses them and prunes the ones unlikely to result in jailbreaks. Using tree-of-thought reasoning allows TAP to navigate a large search space of prompts and pruning reduces the total number of queries sent to the target. In empirical evaluations, we observe that TAP generates prompts that jailbreak state-of-the-art LLMs (including GPT4 and GPT4-Turbo) for more than 80% of the prompts using only a small number of queries. Interestingly, TAP is also capable of jailbreaking LLMs protected by state-of-the-art guardrails, e.g., LlamaGuard. This significantly improves upon the previous state-of-the-art black-box method for generating jailbreaks.


---


###


Summary Notes


##


Strengthening AI Security with the Tree of Attacks with Pruning (TAP) Method


In the fast-paced world of artificial intelligence (AI), Large Language Models (LLMs) such as GPT-4 play a crucial role in various applications, including chatbots and data analysis.


However, as these models become more integrated into our digital lives, they also face increased risks from adversarial attacks.


A new study introduces a groundbreaking approach to combat these threats—the Tree of Attacks with Pruning (TAP) method, offering a new layer of security for AI.


###


Key Challenges in AI Security


The advancement of AI brings about a critical balance between innovation and security.


LLMs, despite their capabilities, are prone to biases and vulnerabilities, leading to "jailbreaks" where the model is manipulated into producing undesirable outcomes.


The study focuses on developing an automated system for detecting and neutralizing these threats, ensuring LLMs adhere to their safety and ethical standards.


####


What are Jailbreaks?


Jailbreaks expose the weaknesses in LLMs when they are tricked into generating content they're designed to avoid, such as biased or unsafe information.


This highlights the urgent need for a system that can automatically identify and fix these issues, keeping LLMs within their operational guidelines.


###


The Tree of Attacks with Pruning (TAP) Approach


TAP marks a significant advancement in protecting LLMs from adversarial attacks. This method uses a strategic process involving multiple LLMs—an attacker, an evaluator, and the target model—to find and eliminate potential jailbreak scenarios efficiently.


####


How TAP Functions


- **Iterative Refinement:** TAP iteratively creates and refines prompts to pinpoint those that could lead to jailbreaks.


- **Pruning Irrelevant Prompts:** It weeds out prompts that are less likely to pose a risk, focusing on those with a higher threat potential.


- **Evaluator's Role:** An evaluator LLM assesses the remaining prompts to see if they can breach the target LLM's safety measures.


This streamlined approach has proven to be highly efficient, achieving over an 80% success rate in identifying vulnerabilities in models like GPT-4 and its Turbo variant with fewer queries.


###


Proven Effectiveness of TAP


Empirical testing shows TAP's effectiveness across different LLM setups, highlighting its adaptability and potential as a universal AI security tool.


The study also explores how various TAP components, such as pruning and the evaluator's role, contribute to its success, offering insights into further optimizations.


###


Advancing Towards a More Secure AI


The development of the TAP method is a crucial step in securing AI systems from adversarial threats. By providing an efficient and scalable solution, TAP addresses a significant security challenge, ensuring the safer deployment of LLMs in critical areas.


This progress not only enhances AI security but also sets the stage for future research aimed at bolstering LLM resistance against attacks.


####


Looking Ahead


The study concludes that TAP outperforms existing methods and opens up avenues for further research to improve the resilience of LLMs against adversarial tactics.


This continuous effort to strengthen AI systems is vital for realizing the full potential of LLMs in a safe and society-beneficial manner.


In summary, the Tree of Attacks with Pruning method signifies a notable advancement in AI security, offering a practical and scalable solution to a pressing challenge.


As AI continues to evolve, research like this ensures that enhancing security remains a top priority, leading to technology that is not only powerful but also secure and trustworthy.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

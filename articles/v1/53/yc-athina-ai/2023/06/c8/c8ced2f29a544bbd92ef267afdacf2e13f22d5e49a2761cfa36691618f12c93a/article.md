---
schema_version: "1.0.0"
document_id: "c8ced2f29a544bbd92ef267afdacf2e13f22d5e49a2761cfa36691618f12c93a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-can-be-easily-distracted-by-irrelevant-context"
published_at: "2023-06-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:bf6be2de66ab0dc6671d95ddf944dcd54eab6bc7b27f489ce66a3676a7e949d5"
---

# Large Language Models Can Be Easily Distracted by Irrelevant Context

Do not index


Original Paper


[https://arxiv.org/abs/2302.00093](https://arxiv.org/abs/2302.00093)


Blog URL


[blog.athina.ai /large-l...-context](https://blog.athina.ai/large-language-models-can-be-easily-distracted-by-irrelevant-context)


**Original Paper:**[https://arxiv.org/abs/2302.00093](https://arxiv.org/abs/2302.00093)


**By:**[Freda Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20F) ,[Xinyun Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20X) ,[Kanishka Misra](https://arxiv.org/search/cs?searchtype=author&query=Misra%2C%20K) ,[Nathan Scales](https://arxiv.org/search/cs?searchtype=author&query=Scales%2C%20N) ,[David Dohan](https://arxiv.org/search/cs?searchtype=author&query=Dohan%2C%20D) ,[Ed Chi](https://arxiv.org/search/cs?searchtype=author&query=Chi%2C%20E) ,[Nathanael Schärli](https://arxiv.org/search/cs?searchtype=author&query=Sch%C3%A4rli%2C%20N) ,[Denny Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20D)


**Abstract:**


> Large language models have achieved impressive performance on various natural language processing tasks. However, so far they have been evaluated primarily on benchmarks where all information in the input context is relevant for solving the task. In this work, we investigate the distractibility of large language models, i.e., how the model problem-solving accuracy can be influenced by irrelevant context. In particular, we introduce Grade-School Math with Irrelevant Context (GSM-IC), an arithmetic reasoning dataset with irrelevant information in the problem description. We use this benchmark to measure the distractibility of cutting-edge prompting techniques for large language models, and find that the model performance is dramatically decreased when irrelevant information is included. We also identify several approaches for mitigating this deficiency, such as decoding with self-consistency and adding to the prompt an instruction that tells the language model to ignore the irrelevant information.


---


###


Summary Notes


####


Enhancing LLMs Amidst Irrelevant Information


Large language models (LLMs) have transformed how we interact with technology, offering human-like responses and understanding.


They're instrumental across various sectors, from automating customer service to powering research tools.


However, their efficacy in handling irrelevant information remains a challenge.


This post, inspired by Freda Shi and colleagues' study, delves into this issue, presenting the Grade-School Math with Irrelevant Context (GSM-IC) dataset and outlining strategies to improve LLMs, especially for AI engineers in enterprise companies.


####


The Challenge of Irrelevant Information


LLMs are known for their context understanding capabilities, but their performance can falter when faced with unrelated or distracting content. This issue is critical in precision-demanding tasks like technical support or data extraction, where errors can be costly.


####


The GSM-IC Dataset


To better understand LLMs' performance with distractions, the GSM-IC dataset was introduced. It enhances the GSM8K dataset by adding irrelevant sentences to math problems, challenging models to ignore these distractions to solve the problems accurately.


The dataset evaluates the impact of various distraction types on model performance.


####


Strategies for Better Model Performance


Improving LLMs' resilience to irrelevant information involves several strategies:


- **Chain-of-Thought Prompting (COT):** Guides the model through logical steps towards the solution, focusing on relevant details.


- **Zero-Shot Chain-of-Thought Prompting (0-COT):** A COT variation that doesn't rely on prior examples for training, suitable for limited data scenarios.


- **Least-to-Most Prompting (LTM):** Breaks problems into smaller parts, simplifying complexity and minimizing distraction effects.


- **Prompting with Programs (PROGRAM):** Uses program-like structured prompts to improve systematic processing of information.


- **Self-Consistency Methods:** Employs multiple model outputs to find the most consistent solution, enhancing accuracy in noisy environments.


####


Practical Implementation Tips


For AI engineers aiming to incorporate these strategies:


- **Explore Various Prompting Techniques:** Different models and tasks might require unique approaches. Testing various techniques is key to finding what works best.


- **Prioritize Data Quality:** Ensuring high-quality, relevant training data is crucial. Efforts should be made to clean and structure data to reduce irrelevant information.


- **Continuously Iterate:** The AI landscape is constantly changing. Regularly updating models and strategies is essential to address new challenges.


####


Conclusion


Tackling the issue of irrelevant information is vital for enhancing LLM performance. By utilizing the GSM-IC dataset and adopting prompting strategies and self-consistency methods,


AI engineers can improve their models' accuracy and reliability. Continuous research and adaptation are necessary to overcome these challenges and unlock LLMs' full potential in complex informational environments.


This approach marks a significant step towards creating more resilient and effective language models.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

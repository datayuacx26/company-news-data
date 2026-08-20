---
schema_version: "1.0.0"
document_id: "27f1dda8606ebca548798288f6c5666144a0285167038675a7c5a81d144d1a2b"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/on-second-thought-let-s-not-think-step-by-step-bias-and-toxicity-in-zero-shot-reasoning"
published_at: "2023-06-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:89124a91617befe267297ef8dd282ef0bf76f5eb1a9a29db16a582c6df7d23ad"
---

# On Second Thought, Let's Not Think Step by Step! Bias and Toxicity in Zero-Shot Reasoning

Do not index


Original Paper


[https://arxiv.org/abs/2212.08061](https://arxiv.org/abs/2212.08061)


Blog URL


[blog.athina.ai /on-seco...easoning](https://blog.athina.ai/on-second-thought-let-s-not-think-step-by-step-bias-and-toxicity-in-zero-shot-reasoning)


**Original Paper:**[https://arxiv.org/abs/2212.08061](https://arxiv.org/abs/2212.08061)


**By:**[Omar Shaikh](https://arxiv.org/search/cs?searchtype=author&query=Shaikh%2C%20O) ,[Hongxin Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20H) ,[William Held](https://arxiv.org/search/cs?searchtype=author&query=Held%2C%20W) ,[Michael Bernstein](https://arxiv.org/search/cs?searchtype=author&query=Bernstein%2C%20M) ,[Diyi Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20D)


**Abstract:**


> Generating a Chain of Thought (CoT) has been shown to consistently improve large language model (LLM) performance on a wide range of NLP tasks. However, prior work has mainly focused on logical reasoning tasks (e.g. arithmetic, commonsense QA); it remains unclear whether improvements hold for more diverse types of reasoning, especially in socially situated contexts. Concretely, we perform a controlled evaluation of zero-shot CoT across two socially sensitive domains: harmful questions and stereotype benchmarks. We find that zero-shot CoT reasoning in sensitive domains significantly increases a model's likelihood to produce harmful or undesirable output, with trends holding across different prompt formats and model variants. Furthermore, we show that harmful CoTs increase with model size, but decrease with improved instruction following. Our work suggests that zero-shot CoT should be used with caution on socially important tasks, especially when marginalized groups or sensitive topics are involved.


---


###


Summary Notes


##


Managing Risks in AI's Chain of Thought Reasoning: A Guide for Engineers


In the ever-changing field of AI, Chain of Thought (CoT) reasoning stands out for boosting natural language processing tasks.


Yet, a collaborative study by Stanford University, Shanghai Jiao Tong University, and Georgia Institute of Technology points out its drawbacks, especially in sensitive areas.


This post examines the impact of CoT reasoning on biases and toxicity in AI outputs and provides AI engineers with strategies to minimize these issues.


###


**Introduction to Chain of Thought Reasoning**


CoT reasoning prompts language models to follow a step-by-step problem-solving process, enhancing performance in logical tasks.


However, its application to tasks involving social knowledge raises concerns. The study highlights the negative side effects of CoT in handling harmful questions and stereotypes.


###


**Exploring the Risks: Study Insights**


####


**Background**


Previous studies have shown that the way prompts are structured can significantly affect AI outputs. The current research builds on this, exploring how CoT influences biases and toxicity.


####


**Methodology**


The study reevaluated three benchmarks (CrowS-Pairs, StereoSet, and BBQ) for bias in AI reasoning and introduced a new benchmark, HarmfulQ, for analyzing responses to harmful queries. It tested various GPT-3 models with both traditional and CoT prompts.


####


**Key Findings**


The results reveal concerning trends for AI developers:


- CoT prompts increased stereotypical responses by 8.8% and toxic outputs by 19.4%.


- Larger models tended to produce more harmful CoT outputs, but models with better instruction-following abilities showed improvement.


- Despite its benefits for logical tasks, CoT reasoning poses risks in sensitive contexts.


###


**Advice for AI Engineers**


To address these challenges, engineers should take proactive steps:


- **Context Evaluation** : Carefully assess if the task at hand is suitable for CoT, especially for tasks related to or impacting marginalized groups.


- **Controlled Testing** : Conduct thorough tests with a variety of prompts to identify biases or toxicities before wide-scale implementation.


- **Enhance Instruction-Following** : Improve your models' ability to follow instructions accurately to mitigate harmful outputs.


- **Craft Careful Prompts** : Design prompts that avoid reinforcing stereotypes or biases.


- **Ongoing Monitoring and Updates** : Continuously monitor your models and update prompting strategies to maintain ethical and responsible AI use.


###


**Conclusion**


This study highlights the delicate balance between leveraging CoT reasoning for its benefits and managing its risks in sensitive contexts.


For AI to progress responsibly, engineers must implement these strategies to prevent biases and ensure ethical standards are met, pushing the boundaries of AI's capabilities while upholding our social values.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

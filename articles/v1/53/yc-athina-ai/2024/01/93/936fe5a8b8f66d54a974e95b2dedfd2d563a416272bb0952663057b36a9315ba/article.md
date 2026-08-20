---
schema_version: "1.0.0"
document_id: "936fe5a8b8f66d54a974e95b2dedfd2d563a416272bb0952663057b36a9315ba"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/jatmo-prompt-injection-defense-by-task-specific-finetuning"
published_at: "2024-01-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:7abd5097f051d512384acca9d180f90de3e0107294107be8c7063f0cf9309e4b"
---

# Jatmo: Prompt Injection Defense by Task-Specific Finetuning

Do not index


Original Paper


[https://arxiv.org/abs/2312.17673](https://arxiv.org/abs/2312.17673)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2312.17673](https://arxiv.org/abs/2312.17673)


**By:**[Julien Piet](https://arxiv.org/search/cs?searchtype=author&query=Piet%2C%20J) ,[Maha Alrashed](https://arxiv.org/search/cs?searchtype=author&query=Alrashed%2C%20M) ,[Chawin Sitawarin](https://arxiv.org/search/cs?searchtype=author&query=Sitawarin%2C%20C) ,[Sizhe Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20S) ,[Zeming Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20Z) ,[Elizabeth Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20E) ,[Basel Alomair](https://arxiv.org/search/cs?searchtype=author&query=Alomair%2C%20B) ,[David Wagner](https://arxiv.org/search/cs?searchtype=author&query=Wagner%2C%20D)


**Abstract:**


> Large Language Models (LLMs) are attracting significant research attention due to their instruction-following abilities, allowing users and developers to leverage LLMs for a variety of tasks. However, LLMs are vulnerable to prompt-injection attacks: a class of attacks that hijack the model's instruction-following abilities, changing responses to prompts to undesired, possibly malicious ones. In this work, we introduce Jatmo, a method for generating task-specific models resilient to prompt-injection attacks. Jatmo leverages the fact that LLMs can only follow instructions once they have undergone instruction tuning. It harnesses a teacher instruction-tuned model to generate a task-specific dataset, which is then used to fine-tune a base model (i.e., a non-instruction-tuned model). Jatmo only needs a task prompt and a dataset of inputs for the task: it uses the teacher model to generate outputs. For situations with no pre-existing datasets, Jatmo can use a single example, or in some cases none at all, to produce a fully synthetic dataset. Our experiments on seven tasks show that Jatmo models provide similar quality of outputs on their specific task as standard LLMs, while being resilient to prompt injections. The best attacks succeeded in less than 0.5% of cases against our models, versus 87% success rate against GPT-3.5-Turbo. We release Jatmo at
>
>
> [this https URL](https://github.com/wagner-group/prompt-injection-defense)


---


###


Summary Notes


##


Simplifying Jatmo: A New Strategy to Protect AI from Prompt Injection Attacks


In the fast-evolving field of artificial intelligence, Large Language Models (LLMs) like GPT-3.5-Turbo have changed the game in generating text that closely resembles human writing. However, these advancements come with their own set of challenges, notably prompt injection attacks.


These are deceptive tactics where attackers feed harmful inputs to manipulate the AI's output. **Jatmo** is a cutting-edge solution crafted by researchers from top-tier institutions such as UC Berkeley and Peking University, aimed at tackling these threats through a method known as task-specific finetuning.


####


The Threat of Prompt Injection Attacks


Prompt injection attacks trick the AI into trusting harmful inputs, leading to manipulated and potentially dangerous outputs.


This highlights the urgent need for more secure AI systems, particularly for tasks that handle sensitive information.


####


How Jatmo Works


Jatmo introduces a unique way to defend against prompt injection attacks by focusing on:


- **Creating Task-Specific Data** : It starts by forming a dataset specific to a task, where a teacher model helps generate the required input-output pairs.


- **Fine-Tuning the AI Model** : With this specialized dataset, the AI model is fine-tuned to be more resistant to malicious prompts.


- **Overcoming Data Limitations** : Jatmo can also create synthetic data to train models in situations where real data is scarce.


####


Testing Jatmo's Effectiveness


Jatmo was put to the test across seven different tasks, and the results were impressive. Models with Jatmo showed a massive reduction in the success rate of prompt injection attacks—down to less than 0.5% from the alarming 87% seen in traditional models like GPT-3.5-Turbo.


####


Looking Ahead: The Potential of Jatmo


While Jatmo marks a significant step in making LLMs safer, it's not without its challenges, such as the need for task-specific models which might limit versatility.


However, the potential to expand and adapt Jatmo for other AI security threats opens up exciting avenues for future research.


####


Wrapping Up: Jatmo's Role in Enhancing AI Security


Jatmo presents a robust solution to the critical issue of prompt injection attacks in LLMs. By integrating task-specific safeguards directly into AI models, Jatmo not only boosts security but also ensures the reliability of AI outputs. This makes Jatmo a valuable tool for AI engineers at companies focused on secure and precise AI applications.


####


Explore More About Jatmo


For a deeper dive into Jatmo or to start implementing its techniques, check out the[Jatmo GitHub Repository](https://github.com/wagner-group/prompt-injection-defense) .


As AI continues to advance, protecting LLMs from threats like prompt injection attacks is crucial. Jatmo's innovative approach, through task-specific finetuning, represents a significant leap towards more secure and trustworthy AI technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

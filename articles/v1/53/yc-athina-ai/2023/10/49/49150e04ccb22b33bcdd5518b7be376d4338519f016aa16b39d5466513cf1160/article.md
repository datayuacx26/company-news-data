---
schema_version: "1.0.0"
document_id: "49150e04ccb22b33bcdd5518b7be376d4338519f016aa16b39d5466513cf1160"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/an-llm-can-fool-itself-a-prompt-based-adversarial-attack"
published_at: "2023-10-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:c9ccf3d47ea6e4c94109679dd4ae6f575031c546cc48cd6324f3c48bf9a1857f"
---

# An LLM can Fool Itself: A Prompt-Based Adversarial Attack

Do not index


Original Paper


[https://arxiv.org/abs/2310.13345](https://arxiv.org/abs/2310.13345)


Blog URL


[blog.athina.ai /an-llm-...l-attack](https://blog.athina.ai/an-llm-can-fool-itself-a-prompt-based-adversarial-attack)


**Original Paper:**[https://arxiv.org/abs/2310.13345](https://arxiv.org/abs/2310.13345)


**By:**[Xilie Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20X) ,[Keyi Kong](https://arxiv.org/search/cs?searchtype=author&query=Kong%2C%20K) ,[Ning Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20N) ,[Lizhen Cui](https://arxiv.org/search/cs?searchtype=author&query=Cui%2C%20L) ,[Di Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20D) ,[Jingfeng Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20J) ,[Mohan Kankanhalli](https://arxiv.org/search/cs?searchtype=author&query=Kankanhalli%2C%20M)


**Abstract:**


> The wide-ranging applications of large language models (LLMs), especially in safety-critical domains, necessitate the proper evaluation of the LLM's adversarial robustness. This paper proposes an efficient tool to audit the LLM's adversarial robustness via a prompt-based adversarial attack (PromptAttack). PromptAttack converts adversarial textual attacks into an attack prompt that can cause the victim LLM to output the adversarial sample to fool itself. The attack prompt is composed of three important components: (1) original input (OI) including the original sample and its ground-truth label, (2) attack objective (AO) illustrating a task description of generating a new sample that can fool itself without changing the semantic meaning, and (3) attack guidance (AG) containing the perturbation instructions to guide the LLM on how to complete the task by perturbing the original sample at character, word, and sentence levels, respectively. Besides, we use a fidelity filter to ensure that PromptAttack maintains the original semantic meanings of the adversarial examples. Further, we enhance the attack power of PromptAttack by ensembling adversarial examples at different perturbation levels. Comprehensive empirical results using Llama2 and GPT-3.5 validate that PromptAttack consistently yields a much higher attack success rate compared to AdvGLUE and AdvGLUE++. Interesting findings include that a simple emoji can easily mislead GPT-3.5 to make wrong predictions.


---


###


Summary Notes


####


Unpacking PromptAttack: Elevating LLM Adversarial Robustness Testing


Large Language Models (LLMs) are at the forefront of advancements in Artificial Intelligence, particularly in Natural Language Processing (NLP). As these models become integral to critical applications, evaluating their resistance to sophisticated attacks becomes imperative.


Traditional testing methods often fall short in simulating complex real-world attacks. This is where **PromptAttack** comes in, offering a novel approach to testing LLMs by using adversarial prompts, providing a more realistic assessment of their robustness.


####


Why PromptAttack is a Game-Changer


LLMs' application in sensitive areas brings to light the need for reliable security measures. Current robustness assessments mainly tweak inputs to expose vulnerabilities, which might not cover all potential real-life exploitation strategies.


PromptAttack introduces a novel concept of manipulating the prompts given to LLMs, reflecting more accurately the techniques adversaries might use, and providing a more resource-efficient way to uncover model weaknesses.


####


How PromptAttack Works


PromptAttack is built around three fundamental elements:


- **Original Input (OI)** : The starting sample with its label.


- **Attack Objective (AO)** : The target change in the model's output, ensuring the content's meaning stays intact.


- **Attack Guidance (AG)** : Directions for modifying the prompt.


It also employs a fidelity filter to guarantee that the adversarial examples remain semantically coherent, addressing common critiques of adversarial attack methods.


####


Practical Implementation and Its Efficiency


PromptAttack operates by combining the OI, AO, and AG to create prompts that lead LLMs to produce specific outputs.


This method has been proven to efficiently breach model defenses with fewer required queries, demonstrating its effectiveness.


####


Real-World Effectiveness


Tests on prominent models like GPT-3.5 have shown PromptAttack to outperform previous methods, revealing vulnerabilities through simple adjustments (e.g., adding an emoji to change sentiment classification).


This highlights the importance of thorough model evaluation before deployment.


####


Implications and Future Directions


PromptAttack's findings signal a crucial wake-up call for AI engineers and developers, especially those in high-stake industries.


This method's ability to identify vulnerabilities quickly and efficiently provides a new perspective on LLM security.


####


Conclusion


PromptAttack represents a significant step forward in understanding and enhancing LLM adversarial robustness. Its innovative prompt-based attack mechanism gives developers and researchers vital insights into securing models against sophisticated threats.


As LLMs continue to integrate into various sectors, the role of robust security measures like PromptAttack becomes increasingly critical in ensuring the safe deployment of these technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

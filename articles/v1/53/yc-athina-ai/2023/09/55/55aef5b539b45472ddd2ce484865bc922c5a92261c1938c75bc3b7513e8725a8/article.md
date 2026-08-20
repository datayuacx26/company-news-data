---
schema_version: "1.0.0"
document_id: "55aef5b539b45472ddd2ce484865bc922c5a92261c1938c75bc3b7513e8725a8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/enhancing-zero-shot-chain-of-thought-reasoning-in-large-language-models-through-logic"
published_at: "2023-09-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:d8ba5ab4830c6f25e0e796aabaee66c70dd2a7635b272e1307caec494f5b2a37"
---

# Enhancing Zero-Shot Chain-of-Thought Reasoning in Large Language Models through Logic

Do not index


Original Paper


[https://arxiv.org/abs/2309.13339](https://arxiv.org/abs/2309.13339)


Blog URL


[blog.athina.ai /enhanci...gh-logic](https://blog.athina.ai/enhancing-zero-shot-chain-of-thought-reasoning-in-large-language-models-through-logic)


**Original Paper:**[https://arxiv.org/abs/2309.13339](https://arxiv.org/abs/2309.13339)


**By:**[Xufeng Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20X) ,[Mengdi Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20M) ,[Wenhao Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20W) ,[Cornelius Weber](https://arxiv.org/search/cs?searchtype=author&query=Weber%2C%20C) ,[Jae Hee Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C%20J%20H) ,[Kun Chu](https://arxiv.org/search/cs?searchtype=author&query=Chu%2C%20K) ,[Stefan Wermter](https://arxiv.org/search/cs?searchtype=author&query=Wermter%2C%20S)


**Abstract:**


> Recent advancements in large language models have showcased their remarkable generalizability across various domains. However, their reasoning abilities still have significant room for improvement, especially when confronted with scenarios requiring multi-step reasoning. Although large language models possess extensive knowledge, their reasoning often fails to effectively utilize this knowledge to establish a coherent thinking paradigm. These models sometimes show hallucinations as their reasoning procedures are unconstrained by logical principles. Aiming at improving the zero-shot chain-of-thought reasoning ability of large language models, we propose LoT (Logical Thoughts), a self-improvement prompting framework that leverages principles rooted in symbolic logic, particularly Reductio ad Absurdum, to systematically verify and rectify the reasoning processes step by step. Experimental evaluations conducted on language tasks in diverse domains, including arithmetic, commonsense, symbolic, causal inference, and social problems, demonstrate the efficacy of enhanced reasoning by logic. The implementation code for LoT can be accessed at:
>
>
> [this https URL](https://github.com/xf-zhao/LoT)


---


###


Summary Notes


Incorporating logic into the reasoning processes of LLMs through LoT is a promising way to tackle the challenges of zero-shot reasoning. This research not only shows a path to more accurate and self-correcting AI but also sets the stage for further advancements in making AI think more logically and effectively.


####


Conclusion:


- Incorporating specific knowledge into prompts for more targeted learning.


- Exploring ways to tune LLMs for better spontaneous logic use and verification methods.


- Teaching LLMs to improve themselves through feedback and reinforcement learning.


- Delving into more logical principles for even sharper reasoning.


These promising results open new research paths, including:


####


Looking Ahead:


- **Learning from Success and Failure** : Analyzing both hits and misses showcases LoT's potential in refining AI reasoning skills.


- **Revision Importance** : Larger models making more revisions points to a natural ability for self-improvement, vital for complex reasoning.


- **Performance Boost** : LoT consistently enhances reasoning accuracy, proving its effectiveness.


####


Key Takeaways:


The tests showed that LoT significantly boosts reasoning across various tasks and model sizes, with larger models showing more improvement. The logical checks and revisions made the reasoning chains not only shorter but also more accurate.


####


What Was Found:


This method was tested on a wide range of reasoning tasks using various LLMs, like Vicuna models, GPT-3.5-turbo, and GPT-4, without prior training on these tasks. Tests included different types of reasoning, from arithmetic to commonsense and social interaction.


####


Testing the Approach:


- **Neurosymbolic Models** : Combines neural networks with symbolic logic for clearer and more accurate reasoning.


- **Variational Prompting** : Adds strategies for better accuracy and reliability, including making sure the reasoning is relevant and diverse.


- **LoT Prompting** : Uses logic (like ensuring steps are valid and logical) to check and fix the model's thought process.


- **Chain-of-Thought Prompting (CoT)** : Simplifies complex tasks by breaking them down into smaller steps for easier handling by LLMs.


Improving zero-shot reasoning in LLMs involves a few critical strategies:


####


How It Works:


Large Language Models (LLMs) are key to advancements in AI, handling everything from simple queries to complex problems. Yet, they often stumble when it comes to multi-step reasoning, especially in tasks they haven't seen before, known as zero-shot settings. To combat this, researchers are working on ways to improve how LLMs think through problems step-by-step, focusing on a logic-based method called Logical Thoughts (LoT).


####


Enhancing Zero-Shot Reasoning in AI with Logic


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

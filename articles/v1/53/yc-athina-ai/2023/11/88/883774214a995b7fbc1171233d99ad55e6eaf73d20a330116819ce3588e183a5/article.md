---
schema_version: "1.0.0"
document_id: "883774214a995b7fbc1171233d99ad55e6eaf73d20a330116819ce3588e183a5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/promptcare-prompt-copyright-protection-by-watermark-injection-and-verification"
published_at: "2023-11-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:5e37305b77906543ecbdb5d3625d3622e6b981d0677df15b9a72aa49cb7c6bfc"
---

# PromptCARE: Prompt Copyright Protection by Watermark Injection and Verification

Do not index


Original Paper


[https://arxiv.org/abs/2308.02816](https://arxiv.org/abs/2308.02816)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2308.02816](https://arxiv.org/abs/2308.02816)


**By:**[Hongwei Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20H) ,[Jian Lou](https://arxiv.org/search/cs?searchtype=author&query=Lou%2C%20J) ,[Kui Ren](https://arxiv.org/search/cs?searchtype=author&query=Ren%2C%20K) ,[Zhan Qin](https://arxiv.org/search/cs?searchtype=author&query=Qin%2C%20Z)


**Abstract:**


> Large language models (LLMs) have witnessed a meteoric rise in popularity among the general public users over the past few months, facilitating diverse downstream tasks with human-level accuracy and proficiency. Prompts play an essential role in this success, which efficiently adapt pre-trained LLMs to task-specific applications by simply prepending a sequence of tokens to the query texts. However, designing and selecting an optimal prompt can be both expensive and demanding, leading to the emergence of Prompt-as-a-Service providers who profit by providing well-designed prompts for authorized use. With the growing popularity of prompts and their indispensable role in LLM-based services, there is an urgent need to protect the copyright of prompts against unauthorized use.
>
>
> In this paper, we propose PromptCARE, the first framework for prompt copyright protection through watermark injection and verification. Prompt watermarking presents unique challenges that render existing watermarking techniques developed for model and dataset copyright verification ineffective. PromptCARE overcomes these hurdles by proposing watermark injection and verification schemes tailor-made for prompts and NLP characteristics. Extensive experiments on six well-known benchmark datasets, using three prevalent pre-trained LLMs (BERT, RoBERTa, and Facebook OPT-1.3b), demonstrate the effectiveness, harmlessness, robustness, and stealthiness of PromptCARE.


---


###


Summary Notes


####


Safeguarding Copyrighted AI Prompts with PromptCARE


In the rapidly advancing field of Artificial Intelligence (AI), Large Language Models (LLMs) such as BERT, RoBERTa, and GPT have revolutionized the way we use cloud services, making tasks that seemed like science fiction a decade ago, a reality today.


Prompt-as-a-Service (PraaS) has become a crucial way to utilize these models for specific tasks.


However, this brings up the challenge of protecting the intellectual property of the prompts—complex instructions created through significant research and creativity. That's where PromptCARE, a novel framework, comes into the picture.


####


The Need for Copyright Protection


Prompts are vulnerable to unauthorized use, making the protection of their intellectual value a priority. PromptCARE is introduced to address this issue head-on.


####


What is PromptCARE?


PromptCARE is a cutting-edge solution for copyright protection of prompts, designed to ensure their intellectual property is safeguarded without hindering their functionality. Here’s how it works:


- **Watermark Injection and Verification** : It stealthily embeds a watermark into the prompt, marking it without affecting its use.


- **Bi-level Optimization** : This strategy ensures the watermark is subtly injected while keeping the prompt's guiding capability intact.


####


Testing and Outcomes


PromptCARE was tested on various LLMs, including BERT, RoBERTa, and Facebook OPT-1.3b, along with commercial models like LLaMA. The results showed:


- **Preserved Accuracy and Functionality** : The prompts maintained their high functionality, proving the watermarking did not compromise their effectiveness.


- **Durable Watermarks** : The watermarks were resilient against removal attempts, demonstrating strong protection.


####


Achieving the Right Balance


PromptCARE effectively balances robust protection with the prompt's utility, a crucial aspect for its practical use across various scenarios.


####


Future Directions


The development of PromptCARE is ongoing, with future efforts focusing on improving watermark transferability across different LLM architectures and enhancing defense against sophisticated attacks.


####


Conclusion


PromptCARE sets a new benchmark for copyright protection in AI, ensuring the safe usage of copyrighted prompts without compromising on LLM performance.


This development is a leap forward in protecting intellectual property in the AI domain, fostering innovation in a secure manner.


For AI engineers in enterprise environments, implementing PromptCARE means not just protecting assets but also nurturing a culture of innovation in a secure ecosystem.


As we continue to explore AI's capabilities, PromptCARE will be at the forefront of ensuring creativity is safely harnessed and protected.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

---
schema_version: "1.0.0"
document_id: "8b538be85683330c0748962280880baa4c0b09b850cca192fd3ca7a8449e05cf"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/last-one-standing-a-comparative-analysis-of-security-and-privacy-of-soft-prompt-tuning-lora-and-in-context-learning"
published_at: "2023-10-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:2ae4d667ee06d76707b462403313a66534be0c502619d6d61ef0e5e67bb45bcf"
---

# Last One Standing: A Comparative Analysis of Security and Privacy of Soft Prompt Tuning, LoRA, and In-Context Learning

Do not index


Original Paper


[https://arxiv.org/abs/2310.11397](https://arxiv.org/abs/2310.11397)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.11397](https://arxiv.org/abs/2310.11397)


**By:**[Rui Wen](https://arxiv.org/search/cs?searchtype=author&query=Wen%2C%20R) ,[Tianhao Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20T) ,[Michael Backes](https://arxiv.org/search/cs?searchtype=author&query=Backes%2C%20M) ,[Yang Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Y) ,[Ahmed Salem](https://arxiv.org/search/cs?searchtype=author&query=Salem%2C%20A)


**Abstract:**


> Large Language Models (LLMs) are powerful tools for natural language processing, enabling novel applications and user experiences. However, to achieve optimal performance, LLMs often require adaptation with private data, which poses privacy and security challenges. Several techniques have been proposed to adapt LLMs with private data, such as Low-Rank Adaptation (LoRA), Soft Prompt Tuning (SPT), and In-Context Learning (ICL), but their comparative privacy and security properties have not been systematically investigated. In this work, we fill this gap by evaluating the robustness of LoRA, SPT, and ICL against three types of well-established attacks: membership inference, which exposes data leakage (privacy); backdoor, which injects malicious behavior (security); and model stealing, which can violate intellectual property (privacy and security). Our results show that there is no silver bullet for privacy and security in LLM adaptation and each technique has different strengths and weaknesses.


---


###


Summary Notes


####


Evaluating Security and Privacy in AI Adaptation Methods


AI Engineers and researchers are always on the lookout to enhance Large Language Models (LLMs) for various uses. This blog simplifies the security and privacy aspects of three key adaptation methods: Soft Prompt Tuning (SPT), Low-Rank Adaptation (LoRA), and In-Context Learning (ICL).


####


Introduction


LLMs are crucial for AI applications due to their adaptability and strength. But as we enhance these models using techniques like SPT, LoRA, and ICL, we must also focus on their security and privacy. This post will discuss how these methods stack up against security threats.


####


Overview of Adaptation Techniques


- **Soft Prompt Tuning (SPT):** Tweaks prompts to guide the model without changing its core parameters.


- **Low-Rank Adaptation (LoRA):** Updates the model by adding low-rank matrices for efficient parameter adjustments.


- **In-Context Learning (ICL):** Uses examples within context to inform the model's responses.


####


Security and Privacy Concerns


####


Membership Inference Attacks (MIA)


MIAs try to figure out if a particular input was in the model's training data, which is a privacy issue.


- **Findings:**


####


Model Stealing


These attacks try to copy the model's functionality based on its outputs, affecting security and privacy.


- **Observations:**


####


Backdoor Attacks


Attackers poison the dataset to trigger specific outputs, threatening the model's security.


- **Results:**


####


Study Methodology


The study tested each adaptation method against these attacks using different settings and datasets. It measured success using various metrics like True Positive Rate (TPR) and False Positive Rate (FPR) for MIAs, and accuracy for model stealing and backdoor attacks.


####


Discussion and Limitations


This analysis is a starting point for understanding the vulnerabilities of LLM adaptation methods. It shows the importance of future research to develop stronger defenses, especially for methods like ICL with higher privacy risks.


####


Conclusion


Comparing SPT, LoRA, and ICL shows distinct differences in their ability to handle security and privacy issues. Although no method is perfect in all areas, this research is crucial for AI Engineers to make informed decisions on implementing LLMs safely and effectively.


As the AI field grows, understanding and addressing these potential vulnerabilities will be crucial for the safe use of these powerful models. AI Engineers must keep up with security developments and integrate strong protections into their models to push the field forward responsibly.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

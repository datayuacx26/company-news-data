---
schema_version: "1.0.0"
document_id: "3bc03cd7175ebc7f7962e8040e8690d185e34809110f9623e3cc6f62b9c4de62"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/automatic-prompt-augmentation-and-selection-with-chain-of-thought-from-labeled-data"
published_at: "2023-02-24T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:5ea736542f37bebcfac939e6a620502f7a432f39a99154a4ea0dda8a3d24926b"
---

# Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data

Do not index


Original Paper


[https://arxiv.org/abs/2302.12822](https://arxiv.org/abs/2302.12822)


Blog URL


[blog.athina.ai /automat...led-data](https://blog.athina.ai/automatic-prompt-augmentation-and-selection-with-chain-of-thought-from-labeled-data)


**Original Paper:**[https://arxiv.org/abs/2302.12822](https://arxiv.org/abs/2302.12822)


**By:**[KaShun Shum](https://arxiv.org/search/cs?searchtype=author&query=Shum%2C%20K) ,[Shizhe Diao](https://arxiv.org/search/cs?searchtype=author&query=Diao%2C%20S) ,[Tong Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20T)


**Abstract:**


> Chain-of-thought (CoT) advances the reasoning abilities of large language models (LLMs) and achieves superior performance in complex reasoning tasks. However, most CoT studies rely on carefully designed human-annotated rational chains to prompt LLMs, posing challenges for real-world applications where labeled data is available without rational chains. This paper proposes a new strategy, Automate-CoT (Automatic Prompt Augmentation and Selection with Chain-of-Thought), that can bypass human engineering of CoT by automatically augmenting rational chains from a small labeled dataset, and then pruning low-quality chains to construct a candidate pool of machine-generated rationale chains based on the labels. Finally, it selects the optimal combination of several rationale chains from the pool for CoT prompting by employing a variance-reduced policy gradient strategy to estimate the significance of each example. Automate-CoT enables a quick adaptation of the CoT technique to different tasks. Experimental results demonstrate the effectiveness of our method, where competitive results are achieved on arithmetic reasoning (+2.7%), commonsense reasoning (+3.4%), symbolic reasoning (+3.2%), and non-reasoning tasks (+2.5%). The code is available at
>
>
> [this https URL](https://github.com/SHUMKASHUN/Automate-CoT)


---


###


Summary Notes


####


Enhancing LLM Reasoning with Automate-CoT: Insights from the Paper


The realm of Artificial Intelligence (AI) and machine learning has seen significant strides, particularly with Large Language Models (LLMs).


Yet, complex reasoning tasks remain a challenge for LLMs. The paper **"Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data"** introduces Automate-CoT, a method aiming to improve LLMs' reasoning abilities.


This post summarizes the key insights from the paper, aimed at AI Engineers in enterprise contexts, offering a peek into the future of LLM enhancement.


####


Why Automate-CoT?


- **Problem:** Traditional Chain-of-Thought (CoT) studies rely on human-annotated rational chains, which are labor-intensive and hard to scale. CoT prompting is affected by order, complexity, diversity, and style sensitivity.


- **Solution:** Automate-CoT proposes an automatic, task-agnostic approach to adapt CoT exemplars for any task, overcoming limitations of human prompt engineering.


####


How Does Automate-CoT Work?


Automate-CoT involves three main steps:


- **Augment:** Automatically generate multiple pseudo-reasoning chains for questions using a language model.


- **Prune:** Remove low-quality chains by comparing generated answers with ground truths.


- **Select:** Use a variance-reduced policy gradient strategy to pick the best combination of rationale chains for CoT prompting.


Benefits include quicker task adaptation, reduced sensitivity issues, and enhanced performance with minimal human input.


####


Testing Automate-CoT


- **Datasets:** The method was tested on eleven datasets, covering arithmetic, commonsense, symbolic reasoning, and non-reasoning tasks.


- **Evaluation:** Exact match accuracy was the primary metric. Comparisons were made with manual CoT, self-consistency, and Auto-CoT methods.


- **Findings:** Automate-CoT showed improvements across tasks, notably in arithmetic reasoning (+2.7%), commonsense reasoning (+3.4%), symbolic reasoning (+3.2%), and non-reasoning tasks (+2.5%). The selection algorithm, chain complexity, and robustness to training example selection were highlighted as effective elements.


####


Limitations and Future Directions


Despite promising results, the study has limitations:


- It doesn't compare Automate-CoT with fine-tuning LLMs due to its prompt-based nature and resource constraints.


- It lacks a detailed analysis of good vs. bad linguistic styles in prompts, pointing to areas for future research.


####


Context and Implications


Automate-CoT is positioned within the broader landscape of prompt-based learning and CoT prompting. It draws from advancements in discrete and continuous prompting methods, offering a scalable, efficient way to boost LLM performance across diverse tasks. For AI Engineers, Automate-CoT's methodology could be crucial in developing advanced AI systems.


####


Conclusion


Automate-CoT marks a significant advancement in AI, particularly in enhancing LLM reasoning capabilities. By automating the augmentation, pruning, and selection processes for CoT prompting, it offers a scalable solution to improving LLM task performance.


While acknowledging its limitations, the paper paves the way for further research and practical applications in AI development, contributing to the evolution of more intelligent, adaptable, and efficient AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

---
schema_version: "1.0.0"
document_id: "7558258c1aa18e1bcb0ab9038d478acc89c4a8b39bb4bcf94a532dcf3808d8e6"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/empowering-multi-step-reasoning-across-languages-via-tree-of-thoughts"
published_at: "2024-04-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:1fb7eeb4457f4306d6059cbeb3a3eb65d81050a5795f14e70c0522afc82d9a1f"
---

# Empowering Multi-step Reasoning across Languages via Tree-of-Thoughts

Do not index


Original Paper


[https://arxiv.org/abs/2311.08097](https://arxiv.org/abs/2311.08097)


Blog URL


[blog.athina.ai /empower...thoughts](https://blog.athina.ai/empowering-multi-step-reasoning-across-languages-via-tree-of-thoughts)


**Original Paper:**[https://arxiv.org/abs/2311.08097](https://arxiv.org/abs/2311.08097)


**By:**[Leonardo Ranaldi](https://arxiv.org/search/cs?searchtype=author&query=Ranaldi%2C%20L) ,[Giulia Pucci](https://arxiv.org/search/cs?searchtype=author&query=Pucci%2C%20G) ,[Federico Ranaldi](https://arxiv.org/search/cs?searchtype=author&query=Ranaldi%2C%20F) ,[Elena Sofia Ruzzetti](https://arxiv.org/search/cs?searchtype=author&query=Ruzzetti%2C%20E%20S) ,[Fabio Massimo Zanzotto](https://arxiv.org/search/cs?searchtype=author&query=Zanzotto%2C%20F%20M)


**Abstract:**


> Reasoning methods, best exemplified by the well-known Chain-of-Thought (CoT), empower the reasoning abilities of Large Language Models (LLMs) by eliciting them to solve complex tasks in a step-by-step manner. Although they are achieving significant success, the ability to deliver multi-step reasoning remains limited to English because of the imbalance in the distribution of pre-training data, which makes other languages a barrier. In this paper, we propose Cross-lingual Tree-of-Thoughts (Cross-ToT), a method for aligning Cross-lingual CoT reasoning across languages. The proposed method, through a self-consistent cross-lingual prompting mechanism inspired by the Tree-of-Thoughts approach, provides multi-step reasoning paths in different languages that, during the steps, lead to the final solution. Experimental evaluations show that our method significantly outperforms existing prompting methods by reducing the number of interactions and achieving state-of-the-art performance.


---


###


Summary Notes


##


Unlocking AI Reasoning in All Languages: The Cross-Lingual Tree-of-Thought Approach


In the fast-paced world of artificial intelligence (AI), the quest to enable complex reasoning in multiple languages is ongoing.


While Chain-of-Thought (CoT) prompting has enhanced problem-solving in English for Large Language Models like GPT-3.5, other languages have been left behind.


This post explores the innovative Cross-lingual Tree-of-Thoughts (Cross-ToT) method, which aims to make AI systems inclusive and effective across different languages.


###


The Challenge with Current CoT Prompting


CoT prompting has revolutionized how AI models tackle complex problems by breaking them down into simpler steps. However, this technique primarily benefits English due to the uneven distribution of data across languages, limiting its global applicability.


###


What is Cross-Lingual Tree-of-Thoughts (Cross-ToT)?


####


The Idea


Cross-ToT is a fresh approach that builds on the Tree-of-Thoughts concept but introduces a cross-lingual prompting mechanism. This allows AI models to process and solve problems in various languages, breaking down language barriers in AI functionalities.


####


How It Works


The heart of Cross-ToT is the Cross-lingual Alignment prompt, enabling the model to merge reasoning paths from different languages into a unified solution.


This method not only boosts the model's reasoning skills but also captures the unique aspects of each language, leading to more versatile problem-solving.


###


Key Findings from Experiments


####


Experiment Design


We tested the GPT-3.5 model in tasks like arithmetic, language comprehension, and commonsense reasoning, using datasets such as Multilingual Grade School Math (MGSM) and Cross-lingual Natural Language Inference (XNLI).


####


Major Outcomes


The results were impressive:


- **Fewer Prompting Steps:** Cross-ToT aligns more with the ideal one-shot approach, reducing the need for repeated prompts.


- **Better Performance in Languages with Fewer Resources:** It leverages knowledge from well-resourced languages to enhance performance in those with less data.


###


Looking Forward


The scalability of Cross-ToT is promising. Future work will aim to apply this method to more models, languages, and tasks, and to improve the interaction between reasoning paths from different languages.


###


Final Thoughts


Cross-ToT represents a significant advance in multilingual reasoning for AI, making it more inclusive and effective across languages.


By fostering better cross-lingual alignment and interaction, it opens the door to an AI future where no language is left behind.


In summary, Cross-ToT is more than just a methodology—it's a step towards an AI future where everyone, regardless of language, can benefit from advanced problem-solving capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

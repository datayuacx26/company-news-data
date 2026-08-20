---
schema_version: "1.0.0"
document_id: "4e64368ae8542ff957b7c20f31610204eacc212faf1463e957b381b3d601de4c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/token-level-adversarial-prompt-detection-based-on-perplexity-measures-and-contextual-information"
published_at: "2024-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:ebfc43383a749ba3acd19fe3c13cde821afb44453f708e70ff28eec12773450f"
---

# Token-Level Adversarial Prompt Detection Based on Perplexity Measures and Contextual Information

Do not index


Original Paper


[https://arxiv.org/abs/2311.11509](https://arxiv.org/abs/2311.11509)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2311.11509](https://arxiv.org/abs/2311.11509)


**By:**[Zhengmian Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20Z) ,[Gang Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20G) ,[Saayan Mitra](https://arxiv.org/search/cs?searchtype=author&query=Mitra%2C%20S) ,[Ruiyi Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20R) ,[Tong Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20T) ,[Heng Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20H) ,[Viswanathan Swaminathan](https://arxiv.org/search/cs?searchtype=author&query=Swaminathan%2C%20V)


**Abstract:**


> In recent years, Large Language Models (LLM) have emerged as pivotal tools in various applications. However, these models are susceptible to adversarial prompt attacks, where attackers can carefully curate input strings that mislead LLMs into generating incorrect or undesired outputs. Previous work has revealed that with relatively simple yet effective attacks based on discrete optimization, it is possible to generate adversarial prompts that bypass moderation and alignment of the models. This vulnerability to adversarial prompts underscores a significant concern regarding the robustness and reliability of LLMs. Our work aims to address this concern by introducing a novel approach to detecting adversarial prompts at a token level, leveraging the LLM's capability to predict the next token's probability. We measure the degree of the model's perplexity, where tokens predicted with high probability are considered normal, and those exhibiting high perplexity are flagged as adversarial. Additionaly, our method also integrates context understanding by incorporating neighboring token information to encourage the detection of contiguous adversarial prompt sequences. To this end, we design two algorithms for adversarial prompt detection: one based on optimization techniques and another on Probabilistic Graphical Models (PGM). Both methods are equipped with efficient solving methods, ensuring efficient adversarial prompt detection. Our token-level detection result can be visualized as heatmap overlays on the text sequence, allowing for a clearer and more intuitive representation of which part of the text may contain adversarial prompts.


---


###


Summary Notes


##


Boosting Large Language Models' Defense Against Adversarial Prompts


Large Language Models (LLMs) like GPT-3 are vital in the AI field, providing advanced capabilities in understanding and generating human-like text. However, they face challenges with adversarial prompts—inputs designed to trick the model.


Our research introduces a new method focused on token-level analysis to better detect and defend against these adversarial prompts, enhancing the safety and reliability of LLM applications.


####


Our Strategy


We've developed a dual approach to identify potentially malicious inputs by examining:


- **Perplexity of Tokens** : Checking if a token's occurrence is unexpected within a sequence.


- **Adversarial Detection Techniques** :


####


Detection Workflow


1. **Pre-processing** : Calculate perplexity for each token to see how well it fits expected patterns.


1. **Applying Detection Algorithms** :


1. **Post-processing** : Uses heatmaps for visualizing the risk level of tokens, making the analysis clearer.


####


Testing and Outcomes


We tested our methods on a mix of adversarial and natural prompts, measuring their success with precision, recall, F1-Score, and Intersection over Union (IoU) across models like GPT-2 and Llama2. The results show our approach is highly effective in spotting adversarial inputs at both token and sequence levels, offering insights through intuitive heatmap visualizations.


####


Our Contributions


Our research enhances LLM security by:


- Introducing token-level adversarial prompt detection methods.


- Combining perplexity and contextual analysis for accurate detection.


- Showing that even smaller LLMs can effectively identify adversarial prompts, making this solution broadly accessible.


####


Looking Ahead


We plan to further refine our detection algorithms to keep up with evolving adversarial strategies and extend our methods to counter more complex attacks, ensuring LLMs' safety and dependability.


####


Conclusion


Our work presents a significant step forward in protecting LLMs from adversarial prompts through token-level scrutiny, combining perplexity and contextual insights. This approach not only improves LLMs' defense capabilities but also supports their continued safe use in various AI applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

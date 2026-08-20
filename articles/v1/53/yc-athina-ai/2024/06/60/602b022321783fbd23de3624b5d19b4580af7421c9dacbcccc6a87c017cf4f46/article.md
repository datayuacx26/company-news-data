---
schema_version: "1.0.0"
document_id: "602b022321783fbd23de3624b5d19b4580af7421c9dacbcccc6a87c017cf4f46"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/soft-prompt-tuning-for-augmenting-dense-retrieval-with-large-language-models"
published_at: "2024-06-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.029782+00:00"
content_hash: "sha256:dfebf7bdf9a637e5d8ce3c7ee8691eb2881466238acf9fb53da393491fb15192"
---

# Soft Prompt Tuning for Augmenting Dense Retrieval with Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2307.08303](https://arxiv.org/abs/2307.08303)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2307.08303](https://arxiv.org/abs/2307.08303)


**By:**[Zhiyuan Peng](https://arxiv.org/search/cs?searchtype=author&query=Peng%2C%20Z) ,[Xuyang Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20X) ,[Qifan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Q) ,[Yi Fang](https://arxiv.org/search/cs?searchtype=author&query=Fang%2C%20Y)


**Abstract:**


> Dense retrieval (DR) converts queries and documents into dense embeddings and measures the similarity between queries and documents in vector space. One of the challenges in DR is the lack of domain-specific training data. While DR models can learn from large-scale public datasets like MS MARCO through transfer learning, evidence shows that not all DR models and domains can benefit from transfer learning equally. Recently, some researchers have resorted to large language models (LLMs) to improve the zero-shot and few-shot DR models. However, the hard prompts or human-written prompts utilized in these works cannot guarantee the good quality of generated weak queries. To tackle this, we propose soft prompt tuning for augmenting DR (SPTAR): For each task, we leverage soft prompt-tuning to optimize a task-specific soft prompt on limited ground truth data and then prompt the LLMs to tag unlabeled documents with weak queries, yielding enough weak document-query pairs to train task-specific dense retrievers. We design a filter to select high-quality example document-query pairs in the prompt to further improve the quality of weak tagged queries. To the best of our knowledge, there is no prior work utilizing soft prompt tuning to augment DR models. The experiments demonstrate that SPTAR outperforms the unsupervised baselines BM25 and the recently proposed LLMs-based augmentation method for DR.


---


###


Summary Notes


####


Enhancing Dense Retrieval with Soft Prompt Tuning: A Revolution for AI Engineers


The shift from traditional information retrieval (IR) methods to Dense Retrieval (DR) signifies a major advancement, especially for AI engineers in large companies. DR's success hinges on having access to ample, domain-specific training data, a hurdle that's hard to overcome.


This blog introduces **Soft Prompt Tuning for Augmenting Dense Retrieval (SPTAR)** , a groundbreaking approach that uses Large Language Models (LLMs) to create quality training data, tackling the data scarcity issue head-on.


####


Understanding Dense Retrieval


Before DR, IR systems relied on token-level similarity metrics like TF-IDF and BM25, which often missed the subtleties of language.


DR improved upon this by using dense vector encodings for queries and documents, capturing semantic meanings more effectively.


Yet, DR's efficiency is limited by the availability of specialized training data.


####


Introducing SPTAR


The rise of LLMs, such as GPT-3, has opened new doors for learning with minimal examples. SPTAR capitalizes on this, using soft prompt tuning to craft weak queries for documents without labels.


This innovative method not only solves the data shortage problem but also upgrades the training data quality for DR models.


####


**What is Soft Prompt Tuning?**


Soft Prompt Tuning is a game-changer because it uses adaptable vectors (soft prompts) rather than fixed ones. This adaptability means the model can fine-tune its responses for specific tasks, leading to more accurate outcomes. The key steps are:


- Optimizing a task-specific soft prompt with a small amount of accurate data.


- Creating weak queries for unlabeled documents with this prompt.


- Using a filtering mechanism to pick the most suitable document-query pairs, enhancing the training set quality.


####


Benefits of Soft Prompts in DR


Incorporating soft prompts into DR brings several advantages:


- **Customization** : Tailoring prompts for specific tasks or domains is straightforward.


- **Enhanced Quality** : The method produces more relevant queries, which improves retrieval accuracy.


- **Efficiency** : Generating training data from existing documents eliminates the need for extensive manual data collection.


####


Testing SPTAR


Our experiments, which were conducted with open-source models across various datasets and DR models, demonstrated SPTAR's superiority over traditional methods and other data augmentation techniques.


A crucial element of its success was the soft prompt filter, which significantly improved the selection of quality training examples.


####


Findings and Future Directions


The data clearly shows that SPTAR significantly boosts DR model performance. The ability of soft prompts to generate relevant queries and the positive impact of the soft prompt filter on data quality were particularly notable.


Looking forward, SPTAR's methodology presents exciting possibilities for AI engineers working on search and recommendation systems, offering a scalable way to enhance DR models with quality data.


Future research might explore multi-task learning and more sophisticated prompt tuning methods.


####


Conclusion


Soft Prompt Tuning for Augmenting Dense Retrieval marks a significant advancement in IR technology. SPTAR provides a viable solution for the common issue of lacking specialized training data, promising to revolutionize efficiency and effectiveness in DR models.


As LLMs and prompt tuning evolve, we anticipate even more groundbreaking developments in IR systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

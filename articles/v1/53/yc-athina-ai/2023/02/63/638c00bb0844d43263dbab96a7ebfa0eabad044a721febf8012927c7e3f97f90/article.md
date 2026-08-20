---
schema_version: "1.0.0"
document_id: "638c00bb0844d43263dbab96a7ebfa0eabad044a721febf8012927c7e3f97f90"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/a-la-carte-prompt-tuning-apt-combining-distinct-data-via-composable-prompting"
published_at: "2023-02-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:49379436ae0c51f6a9bdb7c11b820c9ef136f96aa2477aae05326e6e10d974f7"
---

# A-la-carte Prompt Tuning (APT): Combining Distinct Data Via Composable Prompting

Do not index


Original Paper


[https://arxiv.org/abs/2302.07994](https://arxiv.org/abs/2302.07994)


Blog URL


[blog.athina.ai /a-la-ca...rompting](https://blog.athina.ai/a-la-carte-prompt-tuning-apt-combining-distinct-data-via-composable-prompting)


**Original Paper:**[https://arxiv.org/abs/2302.07994](https://arxiv.org/abs/2302.07994)


**By:**[Benjamin Bowman](https://arxiv.org/search/cs?searchtype=author&query=Bowman%2C%20B) ,[Alessandro Achille](https://arxiv.org/search/cs?searchtype=author&query=Achille%2C%20A) ,[Luca Zancato](https://arxiv.org/search/cs?searchtype=author&query=Zancato%2C%20L) ,[Matthew Trager](https://arxiv.org/search/cs?searchtype=author&query=Trager%2C%20M) ,[Pramuditha Perera](https://arxiv.org/search/cs?searchtype=author&query=Perera%2C%20P) ,[Giovanni Paolini](https://arxiv.org/search/cs?searchtype=author&query=Paolini%2C%20G) ,[Stefano Soatto](https://arxiv.org/search/cs?searchtype=author&query=Soatto%2C%20S)


**Abstract:**


> We introduce À-la-carte Prompt Tuning (APT), a transformer-based scheme to tune prompts on distinct data so that they can be arbitrarily composed at inference time. The individual prompts can be trained in isolation, possibly on different devices, at different times, and on different distributions or domains. Furthermore each prompt only contains information about the subset of data it was exposed to during training. During inference, models can be assembled based on arbitrary selections of data sources, which we call "à-la-carte learning". À-la-carte learning enables constructing bespoke models specific to each user's individual access rights and preferences. We can add or remove information from the model by simply adding or removing the corresponding prompts without retraining from scratch. We demonstrate that à-la-carte built models achieve accuracy within 5% of models trained on the union of the respective sources, with comparable cost in terms of training and inference time. For the continual learning benchmarks Split CIFAR-100 and CORe50, we achieve state-of-the-art performance.


---


###


Summary Notes


####


A-la-carte Prompt Tuning: Tailoring AI Models on the Fly


In the dynamic field of artificial intelligence (AI), the ability to customize AI models for specific datasets and preferences at the time of use (inference time) is a game-changer.


A-la-carte Prompt Tuning (APT) emerges as an innovative solution, enabling prompt customization on various datasets without retraining the entire model.


This approach streamlines model updates, respects data ownership, enhances customization, and brings unparalleled flexibility and efficiency to model deployment.


####


Overcoming Traditional Challenges


Updating traditional AI models for new data is often a resource-heavy process, requiring lots of computational power and time.


Additionally, data ownership and privacy concerns can restrict data availability for model training. APT addresses these issues by facilitating:


- Compartmentalization


- Model customization


- Scalability


This makes the model updating process more dynamic and efficient.


####


The Essence of A-la-carte Learning


APT is based on A-la-carte Learning, which selects specific data sources at inference time. Unlike traditional models that need pre-training for each data subset or separate models for each source, A-la-carte Learning simplifies and economizes the process.


####


How APT Works


APT revolutionizes model-data interaction:


- **Prompt Pool Creation** : Each dataset is turned into a learned prompt.


- **Prompt Retrieval** : At inference time, relevant prompts are selected based on user-defined subsets.


- **Enhanced Input Processing** : Prompts are added to the input, and a modified attention mechanism ensures prompts do not interfere with each other, preserving data integrity while boosting model performance and efficiency.


####


Structured Attention Mechanism


A key innovation in APT is the structured attention mechanism, which prevents prompt interference, maintaining data integrity and ensuring high performance efficiently.


####


Performance Insights


APT showcases remarkable performance, coming close to the "paragon model" (a model trained on all data) with significantly lower computational and storage costs. It has also set new standards in continual learning benchmarks, proving its effectiveness and efficiency.


####


Broad Applications


APT's versatility extends to decentralized learning, model versioning, data forgetting, and continual learning, among other applications.


It supports training on decentralized data, efficient data removal, and incremental learning, offering a flexible solution to complex AI model management challenges.


####


Building on Previous Work


APT extends the principles of prompt tuning, primarily from natural language processing, to vision transformers and incorporates ideas from continual learning and model adaptation, providing a holistic approach to A-la-carte Learning.


####


Looking Ahead


APT marks a significant advancement in AI, tackling real-world issues like model updating, data privacy, and customization.


While still in early stages, the potential for further exploration and development is vast. Future opportunities include integration with Federated Learning and optimizations in prompt-based learning models.


APT is not just a current solution but a step towards a more adaptable, efficient, and user-aligned AI future.


It opens new possibilities for customizing, managing, and deploying AI models, promising an era of AI that is more responsive to our needs and challenges.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

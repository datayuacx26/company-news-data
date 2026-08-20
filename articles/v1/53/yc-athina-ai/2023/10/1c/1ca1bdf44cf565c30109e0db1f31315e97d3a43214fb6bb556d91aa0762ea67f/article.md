---
schema_version: "1.0.0"
document_id: "1ca1bdf44cf565c30109e0db1f31315e97d3a43214fb6bb556d91aa0762ea67f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/efficient-federated-prompt-tuning-for-black-box-large-pre-trained-models"
published_at: "2023-10-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:0d7c70d8cf004e64486dfc8cee81dd4c11e55ecadd818048dfb8d4c15b11e336"
---

# Efficient Federated Prompt Tuning for Black-box Large Pre-trained Models

Do not index


Original Paper


[https://arxiv.org/abs/2310.03123](https://arxiv.org/abs/2310.03123)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.03123](https://arxiv.org/abs/2310.03123)


**By:**[Zihao Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20Z) ,[Yan Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20Y) ,[Yifan Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20Y) ,[Xueqian Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20X) ,[Lifu Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20L) ,[Li Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20L) ,[Dacheng Tao](https://arxiv.org/search/cs?searchtype=author&query=Tao%2C%20D)


**Abstract:**


> With the blowout development of pre-trained models (PTMs), the efficient tuning of these models for diverse downstream applications has emerged as a pivotal research concern. Although recent investigations into prompt tuning have provided promising avenues, three salient challenges persist: (1) memory constraint: the continuous growth in the size of open-source PTMs renders fine-tuning, even a fraction of their parameters, challenging for many practitioners. (2) model privacy: existing PTMs often function as public API services, with their parameters inaccessible for effective or tailored fine-tuning. (3) data privacy: the fine-tuning of PTMs necessitates high-quality datasets, which are typically localized and not shared to public. To optimally harness each local dataset while navigating memory constraints and preserving privacy, we propose Federated Black-Box Prompt Tuning (Fed-BBPT). This innovative approach eschews reliance on parameter architectures and private dataset access, instead capitalizing on a central server that aids local users in collaboratively training a prompt generator through regular aggregation. Local users leverage API-driven learning via a zero-order optimizer, obviating the need for PTM deployment. Relative to extensive fine-tuning, Fed-BBPT proficiently sidesteps memory challenges tied to PTM storage and fine-tuning on local machines, tapping into comprehensive, high-quality, yet private training datasets. A thorough evaluation across 40 datasets spanning CV and NLP tasks underscores the robustness of our proposed model.


---


###


Summary Notes


####


Efficient Federated Prompt Tuning for Large Models


In the dynamic field of Artificial Intelligence (AI), large pre-trained models (PTMs) like GPT-3 and LLaMA are revolutionizing various sectors.


Despite their capabilities, deploying these models for specific tasks presents significant challenges, such as memory limitations and privacy concerns. The Federated Black-Box Prompt Tuning (Fed-BBPT) framework offers a solution, particularly for AI Engineers in enterprise environments facing these hurdles.


###


Challenges with Large PTMs


Large PTMs bring unmatched benefits but also face considerable obstacles:


- **Memory Constraints** : Their enormous size demands extensive memory and computational power, making fine-tuning difficult.


- **Model Privacy** : Often provided as proprietary black-box APIs, these models cannot be directly fine-tuned.


- **Data Privacy** : The lack of publicly available, high-quality datasets, especially in sensitive areas, limits personalized training opportunities.


###


Fed-BBPT Framework Overview


Fed-BBPT combines federated learning with black-box prompt tuning to address these issues, allowing for collaborative prompt generator training across multiple users without direct model access or data sharing.


####


Key Features


- **Eliminates Local Deployment** : Avoids the need for local PTM deployment, reducing memory and computational requirements.


- **Boosts Privacy** : Safeguards model and data privacy during training.


- **Lowers Resource Needs** : Reduces client-side resource demands, ideal for enterprises.


###


Methodology


####


System Architecture


At its core, the Fed-BBPT has a central server managing local prompt models' training, which interact with a PTM in the cloud via API calls.


This setup supports collaborative learning without sharing sensitive data.


####


Training Process


1. **Prompt Generation and API Calls** : Users create prompts with their data, sent to the PTM through an API.


1. **Local Prompt Model Updates** : PTM responses help update local models using zero-order optimization.


1. **Model Aggregation** : The central server periodically combines these models to improve training.


###


Experiments and Results


Tested over 40 different datasets in computer vision and natural language processing, Fed-BBPT showed its effectiveness, matching or outperforming existing methods in efficiency, performance, and privacy.


###


Conclusion and Future Directions


Fed-BBPT represents a significant step forward in applying large PTMs practically, addressing key challenges without sacrificing performance.


It shows promise for scenarios with strict privacy needs.


**Future Work** : Opportunities include exploring more complex datasets, comparing additional methods, and incorporating advanced federated learning techniques. Fed-BBPT sets the stage for future AI advancements, enabling efficient and secure use of large PTMs.


---


**Illustrations** :


- **Figure 1** : Shows the Fed-BBPT framework's interaction between users, the central server, and the PTM.


- **Table 1** : Compares various methods, emphasizing Fed-BBPT's strengths in efficiency and privacy.


In summary, Fed-BBPT offers a groundbreaking approach to overcoming the challenges of large PTMs, paving the way for innovative, private, and collaborative AI development.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

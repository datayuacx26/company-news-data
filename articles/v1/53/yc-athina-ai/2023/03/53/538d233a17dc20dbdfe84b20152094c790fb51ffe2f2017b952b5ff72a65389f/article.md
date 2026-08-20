---
schema_version: "1.0.0"
document_id: "538d233a17dc20dbdfe84b20152094c790fb51ffe2f2017b952b5ff72a65389f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/visual-prompt-based-personalized-federated-learning"
published_at: "2023-03-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:410f05b5d7010149d06d77dce255c385e82f3a5ab18f60941b6aec83e6b5f377"
---

# Visual Prompt Based Personalized Federated Learning

Do not index


Original Paper


[https://arxiv.org/abs/2303.08678](https://arxiv.org/abs/2303.08678)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2303.08678](https://arxiv.org/abs/2303.08678)


**By:**[Guanghao Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20G) ,[Wansen Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20W) ,[Yan Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20Y) ,[Li Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20L) ,[Baoyuan Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20B) ,[Dacheng Tao](https://arxiv.org/search/cs?searchtype=author&query=Tao%2C%20D)


**Abstract:**


> As a popular paradigm of distributed learning, personalized federated learning (PFL) allows personalized models to improve generalization ability and robustness by utilizing knowledge from all distributed clients. Most existing PFL algorithms tackle personalization in a model-centric way, such as personalized layer partition, model regularization, and model interpolation, which all fail to take into account the data characteristics of distributed clients. In this paper, we propose a novel PFL framework for image classification tasks, dubbed pFedPT, that leverages personalized visual prompts to implicitly represent local data distribution information of clients and provides that information to the aggregation model to help with classification tasks. Specifically, in each round of pFedPT training, each client generates a local personalized prompt related to local data distribution. Then, the local model is trained on the input composed of raw data and a visual prompt to learn the distribution information contained in the prompt. During model testing, the aggregated model obtains prior knowledge of the data distributions based on the prompts, which can be seen as an adaptive fine-tuning of the aggregation model to improve model performances on different clients. Furthermore, the visual prompt can be added as an orthogonal method to implement personalization on the client for existing FL methods to boost their performance. Experiments on the CIFAR10 and CIFAR100 datasets show that pFedPT outperforms several state-of-the-art (SOTA) PFL algorithms by a large margin in various settings.


---


###


Summary Notes


####


Revolutionizing AI with Visual Prompt Based Personalized Federated Learning


The world of Artificial Intelligence (AI) is constantly evolving, and **Personalized Federated Learning (PFL)** is at the forefront of this transformation, especially for AI engineers tackling data diversity challenges.


Traditional Federated Learning methods struggle with the unique nature of data spread across different clients.


Enter the innovative **pFedPT framework** , which uses **visual prompts** to significantly improve the personalization and effectiveness of models in image classification tasks.


####


Understanding Personalized Federated Learning (PFL)


PFL aims to customize models to fit the specific data of each client while also drawing on the collective knowledge of a global model.


Although previous PFL methods made some advances, they did not fully address the need for models to adapt to the distinct data traits of each client. The pFedPT framework is designed to fill this gap.


####


The Role of Visual Prompts in PFL


**Visual prompts** are a sophisticated technique borrowed from the successes in Natural Language Processing (NLP) but applied in Computer Vision (CV).


They are adaptable parameters that help pre-trained models adjust to specific tasks more effectively. With the pFedPT framework, every client gets a **Prompt Generator** that produces personalized visual cues from their data.


These cues help ensure that the global model learns both general features and the unique characteristics of each client's data.


####


How pFedPT Functions


- **Prompt Generation:** A special Prompt Generator for each client creates visual cues from local data.


- **Alternating Updates:** The model training alternates between updating the Prompt Generator and the global model, allowing both global and local features to be learned efficiently.


- **Optimization:** The framework uses a specialized loss function to improve the accuracy of personalized predictions on local datasets.


####


Experimental Validation


Tests using the CIFAR10 and CIFAR100 datasets prove that pFedPT outperforms existing PFL methods, showing better test accuracy and handling of non-IID settings.


These results confirm the effectiveness of visual prompts in enhancing model personalization and generalization.


####


Significance for AI Engineers


For AI engineers in large companies, the pFedPT framework offers a new way to overcome data diversity issues. Using visual prompts for personalized federated learning could lead to stronger, more accurate, and tailored AI applications.


The framework's adaptability and performance benefits highlight its potential for broader adoption.


####


Conclusion: A New Chapter in Federated Learning


The introduction of visual prompts in federated learning through the pFedPT framework marks a significant advancement towards more personalized and efficient AI models.


It addresses the shortcomings of previous PFL methods and utilizes the unique data features of each client to improve model personalization.


For AI engineers in the corporate world, adopting this innovative approach could unlock new levels of performance and personalization in AI projects.


The exploration of visual prompt-based personalized federated learning opens up endless possibilities for creating more customized, efficient, and robust AI models.


It's an exciting time for AI engineers to delve into this new field and leverage the full potential of this technology.


####


Visuals & Data Insights


- Comparisons highlighting the improved performance of models using the pFedPT framework.


- Visual demonstrations of how visual prompts affect model learning and focus.


- Performance metrics of the framework in different federated learning scenarios, compared to standard models.


**Keywords:** Personalized Federated Learning, Visual Prompts, Image Classification, Data Diversity, Model Customization.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

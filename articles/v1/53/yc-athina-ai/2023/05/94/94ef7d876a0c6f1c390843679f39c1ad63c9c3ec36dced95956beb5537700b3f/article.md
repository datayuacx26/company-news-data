---
schema_version: "1.0.0"
document_id: "94ef7d876a0c6f1c390843679f39c1ad63c9c3ec36dced95956beb5537700b3f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/the-web-can-be-your-oyster-for-improving-large-language-models"
published_at: "2023-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:e08f6e085aae23f2dcd564386d836be849bdaa140e57ca1edc9a4ba412e68c43"
---

# The Web Can Be Your Oyster for Improving Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2305.10998](https://arxiv.org/abs/2305.10998)


Blog URL


[blog.athina.ai /the-web...e-models](https://blog.athina.ai/the-web-can-be-your-oyster-for-improving-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2305.10998](https://arxiv.org/abs/2305.10998)


**By:**[Junyi Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20J) ,[Tianyi Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20T) ,[Wayne Xin Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20W%20X) ,[Jingyuan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20J) ,[Jian-Yun Nie](https://arxiv.org/search/cs?searchtype=author&query=Nie%2C%20J) ,[Ji-Rong Wen](https://arxiv.org/search/cs?searchtype=author&query=Wen%2C%20J)


**Abstract:**


> Large language models (LLMs) encode a large amount of world knowledge. However, as such knowledge is frozen at the time of model training, the models become static and limited by the training data at that time. In order to further improve the capacity of LLMs for knowledge-intensive tasks, we consider augmenting LLMs with the large-scale web using search engine. Unlike previous augmentation sources (e.g., Wikipedia data dump), the web provides broader, more comprehensive and constantly updated information. In this paper, we present a web-augmented LLM UNIWEB, which is trained over 16 knowledge-intensive tasks in a unified text-to-text format. Instead of simply using the retrieved contents from web, our approach has made two major improvements. Firstly, we propose an adaptive search engine assisted learning method that can self-evaluate the confidence level of LLM's predictions, and adaptively determine when to refer to the web for more data, which can avoid useless or noisy augmentation from web. Secondly, we design a pretraining task, i.e., continual knowledge learning, based on salient spans prediction, to reduce the discrepancy between the encoded and retrieved knowledge. Experiments on a wide range of knowledge-intensive tasks show that our model significantly outperforms previous retrieval-augmented methods.


---


###


Summary Notes


##


UNIWEB: The Key to Enhancing Large Language Models with the Web


In the fast-paced digital world, the demand for smarter, more adaptable AI systems is skyrocketing. Large Language Models (LLMs) are crucial in this search, capable of processing vast amounts of data. However, their potential is often limited by the static data they're trained on.


UNIWEB introduces a revolutionary method that uses the web's endless, constantly updated information to improve LLMs.


###


Why Traditional LLMs Aren't Enough


Traditional LLMs are like snapshots of knowledge, capturing information only until their last update. This makes them less effective for tasks that need the latest data, as they can't adapt to new information.


###


Introducing UNIWEB


UNIWEB stands out by using the web to keep LLMs updated with the latest information. This approach enhances LLMs by:


- Expanding their knowledge base beyond traditional datasets.


- Ensuring they have access to the most current data.


####


Related Work Insights


- **Retrieval-Augmented LLMs:** Previous models tried to address the issue of outdated data by accessing external databases, but often struggled with the limited scope and freshness of the data.


- **UNIWEB's Advantage:** UNIWEB uses a dynamic search engine to fetch the latest web data, providing broader and more up-to-date information.


###


How UNIWEB Works


UNIWEB introduces two main innovations:


- **Adaptive Search Engine-Assisted Learning:** It detects when it needs more information and proactively searches the web to stay current.


- **Continual Knowledge Learning:** It integrates new data with existing knowledge, improving its ability to learn and adapt.


These features keep UNIWEB at the forefront of knowledge, making it highly effective for knowledge-intensive tasks.


###


UNIWEB in Action


Testing UNIWEB across 16 tasks, from fact-checking to reasoning, showed:


- **Improved Performance:** It surpassed traditional models, proving its effectiveness in using web information.


- **Versatility:** It excelled in various tasks, showcasing its adaptability and strength.


###


The Future of UNIWEB


UNIWEB's success paves the way for further development of web-augmented LLMs. Future efforts will focus on enhancing its web retrieval capabilities and exploring new applications.


###


Acknowledgments


This breakthrough was supported by notable institutions and grants, including the National Natural Science Foundation of China and the Beijing Natural Science Foundation, highlighting the significance of UNIWEB in AI advancement.


###


Main Takeaway


UNIWEB shows that the web is an invaluable resource for improving LLMs. By integrating real-time web data, LLMs can become more adaptable and effective, keeping pace with the ever-changing world. This approach is key to unlocking AI's full potential, ensuring AI systems remain as informed as the world around us.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

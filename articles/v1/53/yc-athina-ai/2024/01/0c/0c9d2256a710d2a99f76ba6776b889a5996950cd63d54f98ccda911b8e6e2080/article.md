---
schema_version: "1.0.0"
document_id: "0c9d2256a710d2a99f76ba6776b889a5996950cd63d54f98ccda911b8e6e2080"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/text2mdt-extracting-medical-decision-trees-from-medical-texts"
published_at: "2024-01-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:5ad242d5fc9b1b40592c2c2c08ecfabae03ea7776da1e318591fd562498f0fd1"
---

# Text2MDT: Extracting Medical Decision Trees from Medical Texts

Do not index


Original Paper


[https://arxiv.org/abs/2401.02034](https://arxiv.org/abs/2401.02034)


Blog URL


[blog.athina.ai /text2md...al-texts](https://blog.athina.ai/text2mdt-extracting-medical-decision-trees-from-medical-texts)


**Original Paper:**[https://arxiv.org/abs/2401.02034](https://arxiv.org/abs/2401.02034)


**By:**[Wei Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20W) ,[Wenfeng Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20W) ,[Xing Tian](https://arxiv.org/search/cs?searchtype=author&query=Tian%2C%20X) ,[Pengfei Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20P) ,[Xiaoling Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20X) ,[Jin Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20J) ,[Yuanbin Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Y) ,[Yuan Ni](https://arxiv.org/search/cs?searchtype=author&query=Ni%2C%20Y) ,[Guotong Xie](https://arxiv.org/search/cs?searchtype=author&query=Xie%2C%20G)


**Abstract:**


> Knowledge of the medical decision process, which can be modeled as medical decision trees (MDTs), is critical to build clinical decision support systems. However, the current MDT construction methods rely heavily on time-consuming and laborious manual annotation. In this work, we propose a novel task, Text2MDT, to explore the automatic extraction of MDTs from medical texts such as medical guidelines and textbooks. We normalize the form of the MDT and create an annotated Text-to-MDT dataset in Chinese with the participation of medical experts. We investigate two different methods for the Text2MDT tasks: (a) an end-to-end framework which only relies on a GPT style large language models (LLM) instruction tuning to generate all the node information and tree structures. (b) The pipeline framework which decomposes the Text2MDT task to three subtasks. Experiments on our Text2MDT dataset demonstrate that: (a) the end-to-end method basd on LLMs (7B parameters or larger) show promising results, and successfully outperform the pipeline methods. (b) The chain-of-thought (COT) prompting method \\cite{Wei2022ChainOT} can improve the performance of the fine-tuned LLMs on the Text2MDT test set. (c) the lightweight pipelined method based on encoder-based pretrained models can perform comparably with LLMs with model complexity two magnititudes smaller. Our Text2MDT dataset is open-sourced at \\url{
>
>
> [this https URL](https://tianchi.aliyun.com/dataset/95414)
>
>
> [this https URL](https://github.com/michael-wzhu/text2dt)


---


###


Summary Notes


##


Enhancing CDSS with Automated Medical Decision Tree Construction


In healthcare, the way clinical decisions are made can greatly influence patient results. Clinical Decision Support Systems (CDSS) are crucial for improving these decisions, often using Medical Decision Trees (MDTs) to provide structured advice.


Yet, building MDTs manually is slow, requires a lot of work, and struggles to keep up with new medical findings, which limits the quick updating of CDSS.


This post explores a groundbreaking approach to overcome this challenge: automating the creation of MDTs from medical texts through a process known as Text2MDT, a significant advance in medical natural language processing (NLP).


####


The Problem with Manual MDT Construction


Building MDTs by hand is a detailed task that involves pulling out important medical information from a large amount of literature and clinical guidelines.


This not only takes a lot of time and expertise but also has trouble scaling up, making it hard to keep CDSS current with the newest medical guidelines and research.


####


The Promise of Text2MDT


Text2MDT marks a major step forward in automating the creation of MDTs from medical texts.


By using the latest in NLP and machine learning, especially large language models (LLMs), Text2MDT aims to automatically turn structured medical knowledge into MDTs, which could transform how CDSS are kept up to date.


####


Key Features of the Text2MDT Dataset


A crucial part of this process is the development of the Text2MDT dataset, which is annotated by medical professionals and includes information from clinical guidelines and medical textbooks on various medical conditions and treatments.


This dataset is essential for training and testing models designed to automatically create MDTs from text.


####


Methods for Automating MDT Extraction


There are two main strategies for automating MDT extraction:


- **Pipeline Approach** : This method divides the task into smaller steps, such as extracting information triplets, grouping nodes, and building trees, each handled by different models or methods.


- **End-to-end Approach** : This uses generative models to directly generate MDTs from texts in a single step, aiming for a smoother process.


These approaches are judged using metrics like precision, recall, F1 scores for information extraction, and tree Levenshtein ratio for overall MDT quality.


####


Experimental Results and Observations


Tests using the Text2MDT dataset have shown promising outcomes. LLMs, particularly with chain-of-thought (COT) prompting, excel in generating MDTs directly. However, the pipeline approach has also proven effective, offering a viable option for models with less computational power.


####


Implications for Clinical Decision Support


Automating MDT extraction could greatly improve healthcare by allowing CDSS to quickly incorporate the latest medical knowledge.


Making the Text2MDT dataset and code publicly available also opens up new research and development opportunities at the intersection of medicine and AI.


####


Final Thoughts


Introducing Text2MDT and developing a specialized dataset are key achievements toward automating the creation of Medical Decision Trees from texts.


As AI further integrates into healthcare, automating clinical decision-making processes like Text2MDT could lead to CDSS that are more dynamic, precise, and up-to-date with medical advancements.


####


Get Involved


AI engineers and researchers are encouraged to use the publicly available dataset and code to contribute to the ongoing improvement of automatic MDT extraction models. Collaboration between tech and medical professionals is essential for unleashing AI's full potential in enhancing CDSS.


####


[Dataset and Code Availability](https://www.notion.so/athina-ai/8de4278ac56b4e6d832fb56ee93d9958?v=8d3afbcf5af247b0a867d1dfd34e1b7f&p=4266280957654a42875d2631edda4941&pm=s#)


For those interested in diving deeper into the Text2MDT project, the dataset and code are publicly accessible. Join this pioneering effort to automate the construction of medical decision trees, a key element in advancing clinical decision support.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

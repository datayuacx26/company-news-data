---
schema_version: "1.0.0"
document_id: "f0c24230ed31046fea827b1e832d96cacc230a1f00209f94cfa7cee0f001dbed"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/anomalyclip-object-agnostic-prompt-learning-for-zero-shot-anomaly-detection"
published_at: "2024-03-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:07.231499+00:00"
content_hash: "sha256:261c7fd78bf0aff4d0b0ba551c3d8b2c83dc754c3a7560f4e6240d92fbdb5bb6"
---

# AnomalyCLIP: Object-agnostic Prompt Learning for Zero-shot Anomaly Detection

Do not index


Original Paper


[https://arxiv.org/abs/2310.18961](https://arxiv.org/abs/2310.18961)


Blog URL


[blog.athina.ai /anomaly...etection](https://blog.athina.ai/anomalyclip-object-agnostic-prompt-learning-for-zero-shot-anomaly-detection)


**Original Paper:**[https://arxiv.org/abs/2310.18961](https://arxiv.org/abs/2310.18961)


**By:**[Qihang Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20Q) ,[Guansong Pang](https://arxiv.org/search/cs?searchtype=author&query=Pang%2C%20G) ,[Yu Tian](https://arxiv.org/search/cs?searchtype=author&query=Tian%2C%20Y) ,[Shibo He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20S) ,[Jiming Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20J)


**Abstract:**


> Zero-shot anomaly detection (ZSAD) requires detection models trained using auxiliary data to detect anomalies without any training sample in a target dataset. It is a crucial task when training data is not accessible due to various concerns, eg, data privacy, yet it is challenging since the models need to generalize to anomalies across different domains where the appearance of foreground objects, abnormal regions, and background features, such as defects/tumors on different products/organs, can vary significantly. Recently large pre-trained vision-language models (VLMs), such as CLIP, have demonstrated strong zero-shot recognition ability in various vision tasks, including anomaly detection. However, their ZSAD performance is weak since the VLMs focus more on modeling the class semantics of the foreground objects rather than the abnormality/normality in the images. In this paper we introduce a novel approach, namely AnomalyCLIP, to adapt CLIP for accurate ZSAD across different domains. The key insight of AnomalyCLIP is to learn object-agnostic text prompts that capture generic normality and abnormality in an image regardless of its foreground objects. This allows our model to focus on the abnormal image regions rather than the object semantics, enabling generalized normality and abnormality recognition on diverse types of objects. Large-scale experiments on 17 real-world anomaly detection datasets show that AnomalyCLIP achieves superior zero-shot performance of detecting and segmenting anomalies in datasets of highly diverse class semantics from various defect inspection and medical imaging domains. Code will be made available at
>
>
> [this https URL](https://github.com/zqhang/AnomalyCLIP)


---


###


Summary Notes


####


Revolutionizing Anomaly Detection with AnomalyCLIP


Anomaly detection is a crucial task in fields like industrial inspection, cybersecurity, and medical diagnostics. Traditional methods rely heavily on large amounts of domain-specific training data, which limits their flexibility.


**Zero-Shot Anomaly Detection (ZSAD)** offers a promising alternative by removing the need for target domain training data but faces challenges in generalizing across varied anomalies.


####


AnomalyCLIP: A Game-Changer


AnomalyCLIP represents a breakthrough by using pre-trained vision-language models (VLMs), like CLIP, for ZSAD with a novel approach called object-agnostic prompt learning. This technique significantly enhances the model's versatility and effectiveness in detecting anomalies across different domains without specific training.


####


Key Features of AnomalyCLIP:


- **Object-Agnostic Prompt Learning:**


- **Prompt Template Design:**


- **Optimizing Global and Local Context:**


- **Training and Inference:**


####


Empirical Validation


Tested across 17 diverse datasets, AnomalyCLIP has proven superior in detecting and segmenting anomalies, showcasing remarkable adaptability without domain-specific training.


This performance highlights AnomalyCLIP's potential to revolutionize anomaly detection across various applications.


####


Conclusion: Pioneering Anomaly Detection


AnomalyCLIP marks a significant step forward in zero-shot anomaly detection. By leveraging a generic approach to detect abnormalities, it offers a scalable, efficient, and adaptable solution for the ever-changing needs of anomaly detection across multiple industries.


####


Accessing AnomalyCLIP


The code for AnomalyCLIP is available on GitHub for AI engineers and researchers. This project has received support from the NSFC and the Singapore Ministry of Education, reflecting the collaborative effort behind its development and its potential to impact the future of anomaly detection.


In sum, AnomalyCLIP demonstrates the transformative potential of object-agnostic prompt learning in reshaping the future of anomaly detection, promising greater efficiency and adaptability across a broad range of domains.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

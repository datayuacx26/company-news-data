---
schema_version: "1.0.0"
document_id: "29422da0314245a34aa7505180da85441b93fa4cb67e37e2ec584da088e949e8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/lmpt-prompt-tuning-with-class-specific-embedding-loss-for-long-tailed-multi-label-visual-recognition"
published_at: "2024-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:9f8f2d1c7a6b4f32c3c4167be63f3b4abf024e9f1258ca285e26b12b0d88da84"
---

# LMPT: Prompt Tuning with Class-Specific Embedding Loss for Long-tailed Multi-Label Visual Recognition

Do not index


Original Paper


[https://arxiv.org/abs/2305.04536](https://arxiv.org/abs/2305.04536)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.04536](https://arxiv.org/abs/2305.04536)


**By:**[Peng Xia](https://arxiv.org/search/cs?searchtype=author&query=Xia%2C%20P) ,[Di Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20D) ,[Ming Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20M) ,[Lie Ju](https://arxiv.org/search/cs?searchtype=author&query=Ju%2C%20L) ,[Zongyuan Ge](https://arxiv.org/search/cs?searchtype=author&query=Ge%2C%20Z)


**Abstract:**


> Long-tailed multi-label visual recognition (LTML) task is a highly challenging task due to the label co-occurrence and imbalanced data distribution. In this work, we propose a unified framework for LTML, namely prompt tuning with class-specific embedding loss (LMPT), capturing the semantic feature interactions between categories by combining text and image modality data and improving the performance synchronously on both head and tail classes. Specifically, LMPT introduces the embedding loss function with class-aware soft margin and re-weighting to learn class-specific contexts with the benefit of textual descriptions (captions), which could help establish semantic relationships between classes, especially between the head and tail classes. Furthermore, taking into account the class imbalance, the distribution-balanced loss is adopted as the classification loss function to further improve the performance on the tail classes without compromising head classes. Extensive experiments are conducted on VOC-LT and COCO-LT datasets, which demonstrates that our method significantly surpasses the previous state-of-the-art methods and zero-shot CLIP in LTML. Our codes are fully public at
>
>
> [this https URL](https://github.com/richard-peng-xia/LMPT)


---


###


Summary Notes


##


Revolutionizing AI Visual Recognition with LMPT


In the field of artificial intelligence, engineers at big companies are constantly searching for better ways to solve complex problems.


One tough challenge is Long-tailed Multi-label Visual Recognition (LTML), which involves identifying multiple objects within an image that appear in varying frequencies. This is important for real-world uses like surveillance, self-driving cars, and moderating online content.


###


Understanding LTML Challenges


LTML is tricky because of the uneven distribution of objects in images and the complex ways these objects can be related.


Traditional methods often don't consider how different categories relate to each other and mainly focus on adjusting individual samples.


Although recent breakthroughs, especially in combining vision and language models, have made some progress, there's still a significant need to improve how well models recognize less common objects.


###


Introducing LMPT: A New Solution


A research paper titled "LMPT: Prompt Tuning with Class-Specific Embedding Loss for Long-Tailed Multi-Label Visual Recognition" by Peng Xia and team proposes a new method called LMPT to address LTML challenges.


LMPT combines text descriptions with images and a unique way to handle the imbalance of object frequencies.


####


How LMPT Works


- **Prompt Tuning:** Uses adjustable prompts with text and image data, keeping the main model the same.


- **Class-Specific Embedding Loss:** A new loss function that improves the model by focusing on the differences between class embeddings, with strategies to deal with the imbalance.


- **Multi-Label Classification Loss:** Enhances learning from imbalanced data by combining with the class-specific embedding loss.


####


Testing LMPT


LMPT was tested on widely-used benchmarks like the VOC-LT and COCO-LT datasets, where it performed better than current leading methods, especially for less common objects.


This shows LMPT's potential in improving LTML tasks.


###


Tips for AI Engineers


AI engineers interested in LMPT can consider the following:


- **Use pre-trained models:** Starting with these can save time and resources.


- **Enhance semantic understanding:** Use text descriptions to deepen the model's understanding of different classes.


- **Ensure balanced performance:** Apply class-specific embedding loss and other strategies to balance model performance across common and rare classes.


###


Conclusion


LMPT offers an innovative solution for the LTML challenge by improving semantic understanding and balancing class performance.


Its success sets new standards and provides a practical framework for engineers tackling LTML in various applications.


The LMPT code is available on GitHub ([https://github.com/richard-peng-xia/LMPT](https://github.com/richard-peng-xia/LMPT) ) for those interested in exploring this approach further.


As AI continues to evolve, adopting methodologies like LMPT is key for advancing visual recognition technology and its applications in the real world.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

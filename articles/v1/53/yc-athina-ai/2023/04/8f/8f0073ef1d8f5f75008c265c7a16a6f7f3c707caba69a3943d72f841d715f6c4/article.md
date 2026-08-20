---
schema_version: "1.0.0"
document_id: "8f0073ef1d8f5f75008c265c7a16a6f7f3c707caba69a3943d72f841d715f6c4"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/sam-on-medical-images-a-comprehensive-study-on-three-prompt-modes"
published_at: "2023-04-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:56d9bc96728380490cc8ccbedd35b6200ecfb43c2d0fa4d5c0f1a9376169d2d7"
---

# SAM on Medical Images: A Comprehensive Study on Three Prompt Modes

Do not index


Original Paper


[https://arxiv.org/abs/2305.00035](https://arxiv.org/abs/2305.00035)


Blog URL


[blog.athina.ai /sam-on-...pt-modes](https://blog.athina.ai/sam-on-medical-images-a-comprehensive-study-on-three-prompt-modes)


**Original Paper:**[https://arxiv.org/abs/2305.00035](https://arxiv.org/abs/2305.00035)


**By:**[Dongjie Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng%2C%20D) ,[Ziyuan Qin](https://arxiv.org/search/cs?searchtype=author&query=Qin%2C%20Z) ,[Zekun Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20Z) ,[Shaoting Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20S) ,[Qicheng Lao](https://arxiv.org/search/cs?searchtype=author&query=Lao%2C%20Q) ,[Kang Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20K)


**Abstract:**


> The Segment Anything Model (SAM) made an eye-catching debut recently and inspired many researchers to explore its potential and limitation in terms of zero-shot generalization capability. As the first promptable foundation model for segmentation tasks, it was trained on a large dataset with an unprecedented number of images and annotations. This large-scale dataset and its promptable nature endow the model with strong zero-shot generalization. Although the SAM has shown competitive performance on several datasets, we still want to investigate its zero-shot generalization on medical images. As we know, the acquisition of medical image annotation usually requires a lot of effort from professional practitioners. Therefore, if there exists a foundation model that can give high-quality mask prediction simply based on a few point prompts, this model will undoubtedly become the game changer for medical image analysis. To evaluate whether SAM has the potential to become the foundation model for medical image segmentation tasks, we collected more than 12 public medical image datasets that cover various organs and modalities. We also explore what kind of prompt can lead to the best zero-shot performance with different modalities. Furthermore, we find that a pattern shows that the perturbation of the box size will significantly change the prediction accuracy. Finally, Extensive experiments show that the predicted mask quality varied a lot among different datasets. And providing proper prompts, such as bounding boxes, to the SAM will significantly increase its performance.


---


###


Summary Notes


####


Exploring the Potential and Challenges of SAM in Medical Imaging


The introduction of Segment Anything Model (SAM) into the realm of medical imaging marks a significant advancement, offering a glimmer of hope for overcoming the longstanding challenge of acquiring large, annotated datasets.


SAM's unique ability to adjust to new tasks with minimal guidance makes it a potentially game-changing tool in medical image analysis.


####


Key Takeaways from Related Research


SAM's impact is not just limited to AI and computer programming; it's making waves across various disciplines, including medicine.


Since its debut, there's been a surge in research, particularly focusing on its application in digital pathology and brain MRI.


Results so far have been mixed, suggesting that SAM's effectiveness varies across different medical imaging modalities, warranting further investigation.


####


Study Design: Dataset and Methods


Our research employed 12 public medical image datasets encompassing CT, MRI, and X-ray images.


We experimented with SAM in three distinct modes: auto-prompt, point-prompt, and box-prompt, aiming to identify which mode offers the best zero-shot performance for segmenting medical images.


####


What We Discovered


Our findings revealed:


- **Auto-prompt mode** struggled against baseline models, underlining the difficulty of applying zero-shot learning in medical imaging without specific instructions.


- **Box-prompt mode** stood out, indicating that precise bounding boxes can significantly boost SAM's performance.


- **Point-prompt mode** became more effective with an increased number of points, showcasing its potential given sufficient input.


- However, SAM didn't surpass fully supervised methods, highlighting areas for improvement.


####


Discussion: Insights and Implications


The study suggests that while SAM shows promise in medical imaging, its success greatly depends on the quality of prompts and the diverse nature of medical imaging tasks.


The superior performance of the box-prompt mode points towards the importance of clearly defining areas of interest, suggesting a pathway for future research.


Yet, the varied results across different datasets underscore the necessity for SAM to better adapt to the unique aspects of medical images.


####


Looking Ahead: Future Directions


Improving prompt generation and refining the model are crucial next steps to enhance SAM's utility in medical imaging.


Exploring hybrid approaches that combine zero-shot learning with minimal supervised tuning could lead to more robust and effective solutions, potentially transforming clinical practices.


####


Conclusion: Unleashing SAM's Full Potential


SAM is poised to redefine the approach to medical image segmentation, with the potential to lessen the dependency on extensive annotated datasets.


However, its success is heavily reliant on prompt quality and type.


Continual research is crucial to address these limitations and fully leverage SAM's capabilities to improve patient care.


This study lays a solid foundation for future exploration into SAM's application in medical image analysis, signaling a promising horizon for the field with ongoing advancements and applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

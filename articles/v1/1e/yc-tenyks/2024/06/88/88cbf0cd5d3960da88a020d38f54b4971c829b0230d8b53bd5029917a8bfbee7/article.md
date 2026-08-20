---
schema_version: "1.0.0"
document_id: "88cbf0cd5d3960da88a020d38f54b4971c829b0230d8b53bd5029917a8bfbee7"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more"
published_at: "2024-06-14T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:f346369d619cc439c1844f441875be892a4057b0a3bfe0f129c58c53402307cb"
---

# CVPR 2024: Image and Video Search & Understanding (RAG, Multimodal, Embeddings, and more)

⭐️ *CVPR 2024 is just around the corner! We have a lot of exciting insights to share that can enhance your research and company projects. Don’t miss the chance to chat with the Tenyks team — Make sure to add a visit to our booth to your CVPR 2024 schedule!*


‍


In this article, we highlight four top papers that showcase the latest advancements in the field of image and video search. These works explore new techniques such as Retrieval-Augmented Generation (RAG), multimodal approaches, and advanced retrieval methods.


‍


## 1. Visual search for multimodal intelligence


‍


**Title** : *V*: Guided Visual Search as a Core Mechanism in Multimodal LLMs*


‍


**Paper** :[https://vstar-seal.github.io/](https://vstar-seal.github.io/)


‍


Figure 1. In this example, the VQA LLM could not immediately answer the question, thus initiating V*, an LLM-guided visual search process that utilizes common sense and contextual cues to search for the required details


‍


**Problem to solve:** Current multimodal language models struggle with **high-resolution images** due to their reliance on low-resolution vision encoders (e.g., CLIP), and cannot identify or request missing visual information.


‍


**Novelty** : This work proposes a novel visual search mechanism called V*, guided by a large language model. This V* component is integrated into an existing multimodal system, creating a new meta-architecture called SEAL. By combining language understanding with efficient visual search, SEAL can **better process high-resolution** , complex images and focus on important visual details compared to current multimodal systems.


‍


**Performance Evaluation** : The researchers created a new benchmark called V *Bench to evaluate the ability of multimodal language models (MLLMs) to process high-resolution images with abundant and complex information, where the required visual details might not be easily found. V* Bench is based on 191 high-resolution images from the SAM dataset, with an average resolution of 2246 × 1582 pixels. The benchmark consists of two sub-tasks:


- Attribute Recognition: This task has 115 samples that require the model to recognize certain types of attributes (e.g., color, material) of an object in the image.
- Spatial Relationship Reasoning: This task has 76 samples that ask the model to determine the relative spatial relationship between two objects in the image.


‍


**Applications** : V* could improve various computer vision tasks that require precise understanding of visual details, especially in high-resolution and visually complex scenes. Industries like robotics, augmented reality, and medical imaging could benefit from enhanced visual reasoning capabilities.


‍


**Foundation Models/LLMs/MLLMs based on:**[LLaVA](https://github.com/haotian-liu/LLaVA) ,[LISA](https://github.com/dvlab-research/LISA) .


‍


**Code** :[https://github.com/penghao-wu/vstar?tab=readme-ov-file](https://github.com/penghao-wu/vstar?tab=readme-ov-file)


‍


## 2. Using LLMs as orchestrators to boost multimodal search


‍


**Title** : *Leveraging Large Language Models for Multimodal Search*


‍


**Paper:**[https://arxiv.org/pdf/2404.15790](https://arxiv.org/pdf/2404.15790)


‍


Figure 2. The method extracts visual features from the reference image using a Vision Transformer, specifically a pretrained CLIP model with frozen weights. These features, taken before the projection layer, are processed by a Querying Transformer (Q-Former) through cross-attention with learned queries. The output from the Q-Former is combined with embeddings from the modifying text. Finally, all this information is fed into a T5 model, an encoder-decoder LLM.


‍


**Problem to solve:** Multimodal search involves using both images and text to express search queries. However, existing systems often struggle with simple queries (i.e., unreliable performance) and have difficulty understanding natural language text (i.e., handling variability in text queries), which can be ambiguous or contain irrelevant information. This makes it hard for users to get accurate search results.


‍


**Novelty:** The paper proposes 1) a method that combines foundation models in language and vision for multimodal retrieval that achieves a new performance milestone on the Fashion 200K dataset; and (2) a search interface that uses Large Language Models (LLMs) helps users by engaging in a conversational manner, considering their previous searches, and efficiently directing their queries to the appropriate search systems.


‍


**Performance Evaluation:** This new approach, evaluated on the Fashion 200K dataset, achieves a Recall@10 score of 71.4 and a Recall@50 score of 91.6, with an average performance score of 81.5. These results are considerably higher than those of existing methods.


‍


**Applications** : 1) Medical professionals can use the system to search for medical images that match specific textual descriptions of symptoms or conditions, aiding in diagnosis and treatment planning. 2) Platforms can utilize the system to detect and filter out inappropriate content by combining text descriptions with image analysis.


‍


## 3. RAG for copyright protection of your images


‍


**Title** : *Retrieval Augmented Generation for Copyright Protection*


‍


**Paper** :[https://arxiv.org/pdf/2403.18920](https://arxiv.org/pdf/2403.18920)


‍


Figure 3. Two examples: Images generated without CPR bear close resemblance to the retrieved images, whereas CPR generated images differ from the retrieved images while still capturing the underlying concept in the prompt (e.g., an astronaut on the moon, a more textured Big Ben with a different design). \[2\]


‍


**Problem to solve** : Existing Retrieval Augmented Generation (RAG) techniques for image generation may lead to parts of the retrieved samples being copied in the model’s output, risking leakage of private information contained in the retrieved set.


‍


**Novelty** : This work introduces a new method called “Copy-Protected generation with Retrieval (CPR)” for RAG. CPR allows conditioning the output of diffusion models on a set of retrieved images while guaranteeing that unique identifiable information about those examples is not exposed in the generated outputs. It does so by sampling from a mixture of public (safe) distribution and private (user) distribution by merging their diffusion scores at inference.


‍


**Performance Evaluation** : The researchers evaluated their Copy-Protected Retrieval (CPR) method using pre-trained Stable Diffusion models and a private dataset from MSCOCO. They measured text-image alignment with the TIFA metric. Results showed that retrieving images improved alignment, and applying CPR further enhanced alignment while providing copyright protection.


‍


**Applications** : This method could be particularly useful in creative industries where text-to-image generation and visual content creation are essential tasks. For instance, CPR enhances the text-image alignment of diffusion models, meaning the generated images better match the provided text descriptions. This could allow e-commerce platforms to generate high-quality product visuals from text prompts, enabling better product visualization.


‍


## 4. Using GenAI captions (instead of images) to answer questions about content


‍


**Title** : *Enhancing Visual Question Answering through Question-Driven Image Captions as Prompts*


‍


**Paper:**[https://arxiv.org/pdf/2404.08589](https://arxiv.org/pdf/2404.08589)


‍


Figure 4. VQA pipeline exploiting general and the proposed question-driven (QD) image captioning as an intermediate step


‍


**Problem to solve:** The work aims to address the ongoing challenge of zero-shot visual question answering (VQA). Zero-shot VQA requires advanced generalization and reasoning skills, making it a difficult task for current neural architectures.


‍


**Novelty:** The key novelty proposed in this paper is the incorporation of image captioning as an intermediary process within the VQA pipeline. Specifically, the paper explores the use of image captions instead of images themselves and leverages large language models (LLMs) to establish a zero-shot setting for VQA.


‍


**Performance Evaluation** : The paper evaluated zero-shot image captioning models for VQA by comparing general-purpose and question-driven captions across different question types. Results showed that using question-driven captions in the VQA process led to better overall performance, outperforming the state-of-the-art BLIP-2 model.


‍


**Applications** : Two of the potential use cases are 1) robust visual question answering capabilities could be beneficial for robotic systems operating in complex environments, enabling them to understand and reason about their surroundings more effectively; 2) enhancing the learning experience in educational settings by providing natural language explanations and answers to questions about visual content, such as diagrams, illustrations, or educational videos.


‍


**Code** :[https://github.com/ovguyo/captions-in-VQA](https://github.com/ovguyo/captions-in-VQA)


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


‍


*If you’d like to know more about* ***Tenyks*** *, explore*[sandbox](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*


‍

---
schema_version: "1.0.0"
document_id: "5644e7a5cedb141d3f0aeb3dd1f35db525fa99ad493de360828f0999c8bc1bdd"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/scalable-video-search-cascading-foundation-models----part-1"
published_at: "2024-09-13T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:6528ac6c6b7e475e78fb8d91f9e6e74c98e68a40cda0cd80f2b86d0ce8c827fc"
---

# Scalable Video Search: Cascading Foundation Models — Part 1

Video has become the *lingua franca* of the digital age, but its ubiquity presents a unique challenge: *how do we efficiently extract meaningful information from this ocean of visual data? 🌊*


‍


In Part 1 of this series, we navigate the rapidly evolving landscape of video search technology. From the early days of simple metadata tagging to the current AI-driven approaches using deep learning and multimodal models, we’ll examine the tools and techniques that are revolutionizing how we interact with video content.


‍


Learn about the cutting edge of[multimodality](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) and foundation models in our CVPR 2024 series:


- [Image and Video Search & Understanding](https://medium.com/@tenyks_blogger/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more-59dad7568b80) (RAG, Multimodal, Embeddings, and more).
- [Top Highlights You Must Know](https://medium.com/@tenyks_blogger/top-4-highlights-of-cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding-a808d4bb331e) — Embodied AI, GenAI, Foundation Models, and Video Understanding.


‍


⭐️ Don’t miss our Segment Anything Model (SAM 2) series:


- SAM 2 + GPT-4o Cascading Foundation Models via Visual Prompting:[Part 1](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) and[Part 2](https://medium.com/@tenyks_blogger/sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-part-2-b592b5cd8d4a) .


‍


### Table of Contents


1. The ubiquity of video and the challenge of extracting valuable information
2. Video search: From metadata to deep learning
3. Video search: Multimodal Models, Vector DBs and Foundation Models
4. Next steps


‍


## 1. The ubiquity of video and the high-stakes challenge of extracting valuable information


### 1.1 Video is everywhere


The exponential growth in video content creation and consumption is undeniable (Figure 1). The proliferation of affordable video recording devices, from smartphones to drones, has made it easier than ever for individuals and organizations to capture visual data.


‍


Figure 1. Video has become more ubiquitous than ever, playing an increasingly integral role in our daily lives.


‍


As Figure 2 illustrates, this trend is further amplified by the rise of video-centric platforms and streaming services, such as YouTube, TikTok, and Netflix, which have transformed how we consume and interact with multimedia content.


Figure 2. **Video-centric** industries experiencing **exponential growth** and transformation.


‍


The applications of video span a wide range of industries, from security and surveillance to entertainment and e-commerce. The widespread adoption of CCTV cameras, for instance, has led to a staggering 1 billion surveillance cameras worldwide, with a significant concentration in China.


The integration of video with emerging technologies, such as artificial intelligence (AI) and augmented reality, has opened up new frontiers for innovation:


- Real-time video analytics (Figure 3) powered by AI can enable enhanced security monitoring, traffic management, and even personalized entertainment experiences.
- The video game industry has also witnessed a surge, with the global market projected to reach over $580 billion by 2030, driven by the increasing prevalence of video-based content and interactivity.


Figure 3. Counting cars: real-time **video analytics** powered by computer vision and **deep learning** technologies.


‍


### 1.2 Challenges in unlocking video insights


Despite the ubiquity of video, **extracting meaningful insights** from this data modality remains a significant challenge, involving both technical limitations and practical considerations:


- **1. Volume and Complexity of Data** : The sheer volume of video data generated every day presents significant storage and processing challenges. **High-resolution videos** , especially those captured at **4K** or **8K** , require enormous amounts of storage and computational power for processing.
- **2. Unstructured Nature of Video** : Unlike text, video is an inherently unstructured data format, comprising millions of frames that contain visual, auditory, and sometimes textual information. Extracting relevant information requires sophisticated techniques, including computer vision, natural language processing, and audio analysis, often **in tandem** .
- **3. Real-Time Processing Requirements:** In applications like surveillance and autonomous vehicles, real-time video analysis is critical. However, achieving low-latency processing while maintaining accuracy is challenging, particularly when dealing with high-resolution feeds or complex scenes.


‍


In the next sections we first deep dive on the current state of video, then we explore how AI can redefine and *unlock* 🔐 the market of video search and understanding.


‍


## 2. Video search: From metadata to deep learning


The journey of video search technology mirrors the evolution of digital video itself (see Figure 4). Since the advent of digital video storage, the need to efficiently search through vast amounts of footage has been a persistent challenge for content creators, archivists, and viewers alike.


Figure 4. The **evolution** of **video search** technology from the 1990s


In its infancy, video search relied heavily on metadata — the descriptive information associated with each video or frame.


‍


### 2.1 Metadata-based search


- Videos or individual frames were **tagged with basic information** such as timestamp, duration, or **camera type** .
- This data was stored in **traditional relational databases** , allowing for simple query-based searches.
- However, this method faced two major hurdles: a) **Manual labelling** : To achieve more detailed search capabilities, extensive hand-labelling of metadata was required. This process was both time-consuming and expensive, making it impractical for large-scale video libraries. b) **Limited scope** : Relying solely on basic metadata restricted the depth and accuracy of search results, often failing to capture the rich visual content within the videos themselves.


‍


As technology progressed, the field of video search experienced a significant leap forward with the introduction of deep learning techniques.


‍


### 2.2 Deep learning-based search


This approach leveraged advanced AI models (see Figure 5) to detect and identify elements within video frames automatically.


Figure 5. AI system (based on YOLOv8) used for real-time object detection


‍


Users could now perform more sophisticated queries, such as *“find all frames containing a person”* .


‍


However, early implementations still faced challenges: a) **custom models** : These deep learning systems often required custom-built models, and b) **task-specific training** : Models needed to be explicitly trained for specific detection tasks (i.e., fine-tuning), limiting their flexibility and scalability.


‍


From 2012 (i.e.,[the ImageNet moment](https://www.ben-evans.com/benedictevans/2022/12/14/ChatGPT-imagenet) ) the pace of development in the AI landscape accelerated:


1. **CNNs for image classification in video (2012–2015)**


- Convolutional Neural Networks (CNNs) \[1\] applied to frames of video for object detection and classification.


2. **Two-stream CNNs for action recognition (2014)**


- Introduced to model both appearance and motion in video, facilitating better action detection in video search.


3. **RNNs for sequence modeling in video (2016)**


- Recurrent Neural Networks (RNNs) \[2\] used to model the temporal aspects of video, tracking objects or activities over time.


4. **3D CNNs for spatiotemporal analysis (2016–2017)**


- Enabled analysis of both spatial and temporal dimensions in video, improving search capabilities for actions and events.


**5. Transformers for video understanding (2020s)**


- Transformer \[3\] models adapted for video tasks, offering enhanced ability to capture long-range dependencies and understand complex scenes for video search.


‍


This evolution from simple *metadata* search to *AI-powered* content analysis represents a significant step forward in video search technology. However, the fact that custom training was needed for every task still represents a significant barrier, especially in development and maintenance costs.


‍


### 2.3 The ugly truth — of fine-tuning a video search model 😧


Consider a seemingly simple request: “Find me all people in this video.” To fulfill this task, an organization would need to undertake a complex and resource-intensive process:


1. **Data Collection** : Gather vast amounts of unlabeled video data, often requiring days of footage to ensure diverse scenarios.
2. **Data Selection** : Carefully curate a subset of this data, ensuring a balanced representation of scenes with and without people.
3. **Data Labelling** : Manually annotate the selected frames, marking the presence and location of people. This step is crucial and requires high-quality, consistent labelling.
4. **Model Training** : Utilize the labelled data to train a deep learning model, often requiring multiple iterations and fine-tuning.
5. **Model Deployment** : Integrate the trained model into the existing video search infrastructure.
6. **Monitoring and Maintenance** : Continuously monitor the model’s performance and retrain as necessary to maintain accuracy.


‍


This process is not only time-consuming but also demands significant human resources. Typically, it requires a dedicated machine learning team of several skilled professionals.


‍


Let’s do some *back-of-the-envelop* calculations:


Figure 6. **Fine-tuning** a **video search** model for one single use-case **is expensive** .


‍


In other words, the cost of producing a single actionable result or finding using machine learning can range from $22,500 to $68,333 USD.


‍


However, the challenge doesn’t end there. The *true limitation* becomes apparent when new search requirements arise.


‍


### 2.4 The Hamster Wheel of Model Development


Imagine you’re a video streaming company that just spent two months and $50,000 USD developing a cutting-edge “celebrity detector” for your platform. Your users love it! They can now easily find their favourite stars in any video.


‍


You’re feeling pretty good about yourself until the next morning when your boss bursts into your office:


‍


“Great job on the celebrity detector! Now, can we get one that finds all the luxury cars?”


‍


Your heart breaks 💔. You realize you’re about to embark on the same arduous journey all over again:


1. Crawling the internet for thousands of luxury car images
2. Hiring an army of interns to label Bentleys and Ferraris
3. Feeding your hungry AI model gigabytes of data
4. Crossing your fingers that it can tell a Porsche from a Prius


‍


And just when you think you’re done, marketing chimes in: *“We need to detect designer handbags next!”.*


‍


Sure, you’ll eventually get there, but at what cost? 🤔


‍


## 3. Video search: Multimodal Models, Vector DBs and Foundation Models


The landscape of video search has been dramatically transformed by recent breakthroughs in artificial intelligence and data storage technologies (e.g., Vector databases).


‍


These advancements have addressed many of the limitations we discussed earlier, offering more flexible, efficient, and powerful solutions for video analysis and retrieval.


‍


Let’s explore three key innovations that are reshaping the field.


‍


### 3.1 Multimodal Language Models (MLLMs) and Foundation Models


The advent of[Multimodal Large Language Models](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) (MLLMs) and[Foundation Models](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) has marked a significant leap forward in video search capabilities.


Figure 7. Out of the box YOLO-World model when prompted with “a person with a red hat”


These models, such as[SAM 2](https://medium.com/@tenyks_blogger/sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-part-2-b592b5cd8d4a) (Segment Anything Model 2), OWLViT (Open-World Localization with Vision and Language Transformers),[YOLO-World](https://medium.com/@tenyks_blogger/zero-shot-ai-the-end-of-fine-tuning-as-we-know-it-1e9f0215967e) \[4\], and Gemini, have introduced a paradigm shift in how we approach video analysis. Here are some of their key advantages:


- **Minimal to no training required:** Unlike traditional models that needed extensive fine-tuning for specific tasks, these models can often be used out-of-the-box.
- **Open vocabulary:** They can understand and respond to a wide range of natural language queries without being explicitly trained on every possible object or concept.
- **Flexibility:** Users can simply specify their search criteria using text prompts, and the model will analyze the video accordingly.


‍


For example, with a model like YOLO-World, you could search for “a person with a red hat” (Figure 7) across your video database without ever having to train a specific “red hat detector”.


‍


### 3.2 Multimodal Models


Multimodal models, with CLIP (Contrastive Language-Image Pre-training) \[5\] being a prominent example, see Figure 8, have bridged the gap between text and visual data. These models have revolutionized how we[search for visual content](https://medium.com/@tenyks_blogger/how-to-build-an-image-to-image-search-tool-using-clip-pinecone-b7b70c44faac) using natural language queries.


‍


CLIP’s strengths:


- **Unified embedding space:** These models map both images and text into the same high-dimensional space, allowing for direct comparisons between the two modalities.
- **Text-to-image search:** Users can find relevant images or video frames using text descriptions, even for concepts the model wasn’t explicitly trained on.
- **Cross-modal understanding:** The models can grasp semantic relationships between visual and textual data, enabling more nuanced and context-aware searches.


Figure 8. *CLIP* efficiently learns visual concepts from natural language supervision.


Imagine being able to search your video library for “ *a serene sunset over a bustling city”* and having the system understand and retrieve relevant clips, even if those exact words were never used in any metadata.


‍


### 3.3 Vector Databases


The final piece of the puzzle in this new era of video search is the emergence of efficient[vector databases](https://medium.com/@tenyks_blogger/vector-databases-unlock-the-potential-of-your-data-5bd4950bd72) , such as Milvus, Chroma or Pinecone.


‍


These databases are specifically designed to store and query high-dimensional vector data, which is crucial for working with the[embeddings](https://medium.com/@tenyks_blogger/multi-modal-image-search-with-embeddings-vector-dbs-cee61c70a88a) produced by modern AI models:


- **Efficient storage** : Vector DBs can store the complex embeddings generated by AI models for each video frame or segment.
- **Fast similarity search** : They enable rapid retrieval of the most similar vectors \[6\] to a given query, which translates to finding the most relevant video content quickly.
- **Scalability** : These databases can handle massive amounts of data, making them suitable for large video libraries.


‍


With a vector database, you could store embeddings for millions of video frames and perform similarity searches in milliseconds, allowing for real-time video search across vast archives.


‍


The combination of these three technologies — foundation models, multimodal models, and vector databases — has created a powerful new paradigm for video search.


‍


## 4. Next steps


While the advancements in video search technology are undoubtedly exciting, they also raise a series of critical questions that demand further exploration:


- **Accuracy:** How reliable are these cutting-edge models in real-world scenarios? Understanding their precision across diverse video content is crucial for practical implementation.
- **Cost-effectiveness:** What are the computational and financial implications of deploying these advanced models at scale? Balancing performance with resource utilization is a key consideration for organizations.
- **Speed:** In an age where real-time results are often expected, how do these models perform in terms of processing speed? The trade-offs between accuracy and speed need careful evaluation.


‍


These questions form the foundation for Part 2 of this series, where we’ll explore state-of-the-art approaches to solve this problem.


‍


## References


‍


\[[1](https://arxiv.org/abs/1511.08458) \] An introduction to Convolutional Neural Networks


‍


\[[2](https://arxiv.org/pdf/1808.03314) \] Fundamentals of Recurrent Neural Network (RNN) and Long Short-Term Memory (LSTM) Network


‍


\[[3](https://arxiv.org/pdf/1706.03762) \] Attention is all you need


‍


\[[4](https://arxiv.org/abs/2401.17270) \] YOLO-World: Real-Time Open-Vocabulary Object Detection


‍


\[[5](https://arxiv.org/abs/2103.00020) \] CLIP


‍


\[[6](https://en.wikipedia.org/wiki/Cosine_similarity) \] Cosine similarity


‍


**Authors** : Dmitry Kazhdan, Jose Gabriel Islas Montero


*If you’d like to know more about* ***Tenyks*** *, try*[sandbox](https://tenyks.ai/?utm_source=tenyks_blog&utm_medium=article&utm_campaign=scalable_video_search_part_1) *.*

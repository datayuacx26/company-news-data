---
schema_version: "1.0.0"
document_id: "fd5c7aad0be562115c086395a9579971a3c795e6bed1cd26e50327db98a44839"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/zero-shot-ai-the-end-of-fine-tuning-as-we-know-it"
published_at: "2024-08-30T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:9ae1dcfb8dcdd01570f19c721acfb569087936f8a894e4c36842ec1277333df4"
---

# Zero-Shot AI: The End of Fine-Tuning as We Know It?

Models like[SAM 2](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) ,[LLaVA](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) or ChatGPT can do tasks without special training. This has people wondering if the old way (i.e., fine-tuning) of training AI is becoming outdated.


‍


In this article, we compare two models: YOLOv8 (fine-tuning) and YOLO-World (zero-shot). By looking at how well each one works, we’ll try to answer a big question: **Is fine-tuning becoming a thing of the past, or do we still need both ways of training AI? 🤔**


‍


TL;DR: As shown below, the answer to the above question is: it depends!


Figure 1. When to use fine-tuning over zero-shot and vice versa?


‍


🔥 Learn about the cutting edge of[multimodality](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) and foundation models in our CVPR 2024 series:


- [Image and Video Search & Understanding](https://medium.com/@tenyks_blogger/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more-59dad7568b80) (RAG, Multimodal, Embeddings, and more).
- [Top Highlights You Must Know](https://medium.com/@tenyks_blogger/top-4-highlights-of-cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding-a808d4bb331e) — Embodied AI, GenAI, Foundation Models, and Video Understanding.


‍


⭐️ Don’t miss our Segment Anything Model (SAM 2) series:


- SAM 2 + GPT-4o Cascading Foundation Models via Visual Prompting:[Part 1](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) and[Part 2](https://medium.com/@tenyks_blogger/sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-part-2-b592b5cd8d4a) .


‍


### Table of Contents


1. Fine-tuning VS zero-shot
2. YOLOv8 vs YOLO-World
3. So, when to use fine-tuning over Zero-shot and vice versa?
4. What’s next?


‍


## 1. Fine-tuning VS zero-shot


Traditionally, in object detection — a crucial task in computer vision — we relied heavily on **fine-tuning** \[1\]. **This process involves taking a pre-trained model and adjusting its parameters using a smaller, task-specific dataset.**


‍


While effective, fine-tuning has its drawbacks:


- 1. Time-consuming: It often requires hours or even days of training.
- 2. Data-hungry: A substantial amount of labelled data is needed for each new class.
- 3. Computationally expensive: It demands significant processing power and energy.
- 4. Lack of flexibility: Models need retraining for each new object class.


‍


Enter **zero-shot** \[2\], a game-changing approach that addresses these limitations. Zero-shot learning is not really new, but it **enables a model to recognize object classes it has never been explicitly trained on** .


‍


At a high-level, here’s how zero-shot works:


- 1. The model learns to associate visual features with semantic concepts (i.e., semantic understanding).
- 2. It leverages this understanding to identify new, unseen objects (i.e., knowledge transfer).
- 3. The model uses contextual cues to make educated guesses about unfamiliar objects (i.e., contextual inference).


‍


This lays the ground for the key question of this post: **given the rising popularity of zero-shot models** (like those powering ChatGPT for language tasks), **is the era of fine-tuning coming to an end in object detection?**


‍


## 2. YOLOv8 vs YOLO-World


### 2.1 Datasets


The two object detection datasets we used to run our experiments consist of pictures of cars and of graffiti/art as shown in Figure 2.


Figure 2. Object detection datasets (cars and vandalism) we used to compare YOLO v8 vs YOLO-World


‍


Let’s briefly mention the models used in our experiment.


‍


### 2.2 YOLOv8 vs YOLO-World


**YOLOv8** \[3\] has three main components that make it unique: the backbone network, the neck architecture, and the YOLO head.


- The **backbone** , CSPDarknet53, is a deep neural network that progressively down-samples the input image to extract features. It is divided into four parts, each responsible for feature extraction at different levels, all connected to the PAN, which serves as the neck of YOLOv8.
- The **neck** refines the features captured by the backbone. One of the main improvements in YOLOv8 is in the neck, where the PAN successfully combines features from different levels or scales to capture more details in the data. This makes the extracted features easier for the head to interpret, thereby improving the quality of predictions.
- The **head** , connected to the neck, is responsible for making the final predictions. YOLOv8 features a single head, unlike YOLOv5, which had three. This design simplifies the model by focusing on predicting the centre of an object, reducing complexity.


‍


**YOLO-World** \[4\], on the other hand, has a different architecture, consisting of a YOLO Detector, a Text Encoder, and a Re-parameterizable Vision-Language Path Aggregation Network (RepVL-PAN).


‍


### 2.3 Performance


**YOLOv8 — Cars dataset**


We introduce a simple task — detect all cars in a picture. We trained a YOLOv8 model on 3,123 images for 80 epochs. We start our analysis by looking at the confusion matrix (Figure 3).


Figure 3. The confusion matrix of YOLOv8 (train set) shows a high accuracy for the “car” class


‍


We summarize the three main takeaways:


- 1. The model achieved excellent performance on the training dataset, with a mAP of 0.9123 and mAR of 0.8912.
- 2. Analysis of false positives revealed that the model was detecting unlabelled cars in the background. **To improve performance, it’s recommended to label these background car instances.**
- 3. The model demonstrated strong generalization capabilities, performing exceptionally well on an unseen test set with mAP and mAR scores of 0.9011 and 0.8722 respectively.


‍


We processed the predictions through the[Tenyks platform](http://dashboard.tenyks.ai/) , generating the following confusion matrix (Figure 4):


Figure 4. As expected, the confusion matrix of YOLOv8 (test set) was also very accurate


‍


Examining the errors, we found:


- 1. False Positives were primarily due to the model detecting unlabelled background cars, showcasing its ability to outperform human labelers in thoroughness (see Figure 5).
- 2. A labelling inconsistency was identified and easily rectified by updating the JSON file and re-uploading via Tenyks_SDK. While this didn’t significantly impact overall performance, it highlighted the importance of data quality checks.


Figure 5. The sample where the model (i.e., YOLOv8) did not find the car


‍


At this stage, further improvements could be pursued, but considering the time investment (approximately 8 hours for training, testing, and troubleshooting, excluding setup and learning curve), the model’s performance is highly satisfactory.


‍


**YOLO-World — Cars dataset**


For zero-shot it was much simpler. It took around **10 minutes to get a model** , understand how it works and produce results. But how much better is fine-tuning than zero-shot and was the extra time worth it? 🤔


‍


Here is the confusion matrix of YOLO-World for the test set (Figure 6):


Figure 6. YOLO-World results on the test set for cars were not as good as for YOLOv8


‍


Here are the main takeaways:


- **1. Model performance:**
— Training dataset: mAP of 0.49 and mAR of 0.63.
— Testing dataset: mAP of 0.44 and mAR of 0.55 (for images with at least one prediction).
- **2. Error analysis:**
— YOLO-World’s main weakness: difficulty in identifying cars occupying small areas within 640x640 pixel images (see Figure 7).
— Most False Positives were actually unannotated cars, indicating a significant labelling issue in the dataset.


Figure 7. False Negative predictions based on how big the object is


- **3. Zero-shot vs. fine-tuned model comparison:**
— The zero-shot model underperformed compared to the fine-tuned model in terms of confusion matrices and missed detections.
— However, the zero-shot model showed a crucial advantage: **resilience to mislabelling in the training dataset** .


‍


This labelling issue, though seemingly trivial, is time-consuming to fix. Andrew Ng’s quote is apt:


*“In machine learning, 80% of your time is spent preparing and cleaning the data, and only 20% of the time is spent on the actual model building.”*


‍


Considering the time invested in fine-tuning, this 80% represents a substantial effort.


‍


In **conclusion** , fine-tuning outperformed zero-shot, but the time and resources required for training may not always justify the marginal improvement. For simple tasks, zero-shot models offer a cost-effective starting point, with fine-tuning as a fallback if necessary.


‍


Figure 8. Unsurprisingly, YOLO-World is not as good as the fine-tuned model


‍


▶️ The question remains: how would these methods compare when tasked with identifying more complex objects?


‍


**YOLOv8 — Vandalism dataset**


We ran a similar setup for this dataset using YOLOv8, here are the main takeaways:


- **1. Model Performance Discrepancy:** The model performed better on the training set (mAP 84%, mAR 88%) than on the test set, indicating potential overfitting or difficulty generalizing to new data.
- **2. Class Imbalance:** There was a significant imbalance in the dataset, with only 9.7% of images containing instances of non-vandalism. This led to better performance on vandalism detection compared to non-vandalism detection.
- **3. Generalization Issues:** The model struggled to maintain its performance on unseen data, particularly with non-vandalism detection, suggesting that the concept is challenging to learn and generalize.
- **4. Specific Detection Challenges:** The model had difficulty distinguishing between actual vandalism and normal signs or text in different languages, often misclassifying regular signage as vandalism.


‍


**YOLO-World — Vandalism dataset**


For YOLO-World, here are the three main takeaways:


- **1. Model Struggles with Distinguishing Vandalism:** The model failed to differentiate between vandalism and non-vandalism, likely due to ambiguous wording in the data. Even when focusing solely on “graffiti,” the model’s predictions remain unchanged, leading to poor performance metrics.
- **2. Poor Test Set Performance:** On the test set, the model only made predictions on two images, and both were incorrect.
- **3. Need for Fine-Tuning?:** While the fine-tuned model performed poorly, it still produced some results. When you are trying to find something a five year old might not have heard and definitely will not recognize, you better teach the child what it is. With the models, it is the same: zero-shot understands the elementary objects and performs well on them, but **the moment you need something a bit more specific, you better be ready to fine-tune a model** .


‍


## 3. So, when to use fine-tuning over zero-shot and vice versa?


What is the answer to our main question? The answer is: **it depends** (see Figure 9).


Figure 9. Comparing fine-tuning and zero-shot learning


‍


**If your sole focus is accuracy** , with no other considerations, **then zero-shot learning may not be the best choice for you** . It will likely perform worse than a fine-tuned model in terms of pure precision.


‍


However, **in the real world, accuracy might not always be the most critical metric** . Other factors such as **time** , **resources** , and **scalability should also be taken into account** . Given the vast diversity of applications and use cases, it’s safe to say that the world of AI is large enough to accommodate multiple approaches: **there are simply too many scenarios to declare one method universally superior.**


‍


Think of a **zero-shot model as akin to a curious five-year-old child** . If your task is something you’d be comfortable **asking a child to do** — for instance, identifying specific objects in an image without prior training — then zero-shot learning could be an excellent fit. It will perform similarly to how a child might approach the task: **with general knowledge but without specific expertise.**


‍


However, **if you believe the task is too complex for a child to handle accurately** , or if the potential inaccuracies would be unacceptable for your application, **then you should be prepared** to invest the time and resources necessary **to** **fine-tune** a model for your specific needs.


‍


## 4. What’s next?


We are preparing a Jupyter Notebook that demonstrates how to use the[Tenyks Python SDK](https://docs.tenyks.ai/docs/sdk-reference) to analyze and debug your models.


‍


Specifically, a dataset shown in this post, you will find:


1. An introduction to the Tenyks SDK and its benefits
2. Instructions on how to install the Tenyks SDK
3. Steps for preparing a workspace
4. Guidance on creating a dataset and uploading model predictions
5. Methods to search and display images
6. Techniques for using natural language search filters (e.g., searching for “white car”, “school bus”, “Ford pickup”, etc.)


‍


## References


\[[1](https://arxiv.org/abs/2403.14608) \] Parameter-Efficient Fine-Tuning for Large Models: A Comprehensive Survey


\[[2](https://proceedings.mlr.press/v37/romera-paredes15.pdf) \] An embarrassingly simple approach to zero-shot learning


\[[3](https://arxiv.org/abs/2305.09972) \] Real-Time Flying Object Detection with YOLOv8


\[[4](https://arxiv.org/abs/2401.17270) \] YOLO-World: Real-Time Open-Vocabulary Object Detection


‍


**Authors** : Victor Sirakov, Dmitry Kazhdan, Jose Gabriel Islas Montero


‍


*If you’d like to know more about* ***Tenyks*** *, try*[sandbox](https://sandbox.tenyks.ai/?utm_source=medium&utm_medium=article&utm_campaign=cascading_foundation_models_part_2) *.*


‍

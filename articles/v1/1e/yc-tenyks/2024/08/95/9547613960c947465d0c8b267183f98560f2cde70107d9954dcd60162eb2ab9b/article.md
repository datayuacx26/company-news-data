---
schema_version: "1.0.0"
document_id: "9547613960c947465d0c8b267183f98560f2cde70107d9954dcd60162eb2ab9b"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-rss-b52d09f64a8c"
canonical_url: "https://medium.com/@tenyks_blogger/sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-part-2-b592b5cd8d4a"
published_at: "2024-08-16T19:53:44+00:00"
first_seen_at: "2026-07-27T06:27:15.777070+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:16f64ff9f612acfead0b6db641c0152d49f4f481790a6bd638987f8ac23e5214"
---

# SAM 2 + GPT-4o — Cascading Foundation Models via Visual Prompting — Part 2

Artificial Intelligence


Computer Vision


Data Science


Machine Learning


AI


# SAM 2 + GPT-4o — Cascading Foundation Models via Visual Prompting — Part 2


## We show you how foundation models can be connected together using visual prompting!


[The Tenyks Blogger](https://medium.com/@tenyks_blogger?source=post_page---byline--b592b5cd8d4a---------------------------------------)


7 min read


·


Aug 16, 2024


--


Press enter or click to view image in full size


Tennis players segmentation masks created with Segment Anything Model 2 (SAM 2)


In Part 2 of our[Segment Anything Model 2 (SAM 2) Series](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) , we show how foundation models (e.g., GPT-4o, Claude Sonnet 3.5 and YOLO-World) can be used to generate visual inputs (e.g., bounding boxes) for SAM 2. Learn how this approach is likely to disrupt the field of Computer Vision as we know it.


In case you missed Part 1:


- [Part 1](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) — SAM 2 + GPT-4o Cascading Foundation Models via Visual Prompting.


🔥 Learn more about the cutting edge of[multimodality](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) and foundation models in our CVPR 2024 series:


- [Image and Video Search & Understanding](https://medium.com/@tenyks_blogger/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more-59dad7568b80) (RAG, Multimodal, Embeddings, and more).
- [Top Highlights You Must Know](https://medium.com/@tenyks_blogger/top-4-highlights-of-cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding-a808d4bb331e) — Embodied AI, GenAI, Foundation Models, and Video Understanding.


### Table of contents


1. Cascading Foundation Models
2. Foundation Models as Visual Prompting tools
3. GPT-4o (or Claude Sonnet 3.5) + SAM 2
4. YOLO-World + SAM 2
5. Conclusions


## 1. Cascading Foundation Models


### 1.1 Recap from Part 1


In[Part 1](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) , we introduced Segment Anything Model 2 (SAM 2) \[1\], an object segmentation model for images and video:


- SAM 2 treats images as single-frame videos, using memory to process both images and videos uniformly.
- It allows for promptable segmentation in videos, making predictions and refining them across frames based on user input.
- SAM 2 is trained on the large SA-V dataset, leading to state-of-the-art performance in video segmentation.


Press enter or click to view image in full size


Figure 1. SAM 2 applied to a custom video in[Part 1](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4)


Figure 1 demonstrates a practical application of SAM 2. Our[Jupyter Notebook](https://drive.google.com/file/d/1KJZOWyU0W5afGADSsiGwRhWerATd1VNt/view?usp=sharing) provides step-by-step instructions for setting up and running SAM 2 on your own machine.


### 1.2 What do we mean by cascading foundation models?


Cascading foundation models simply means assembling a pipeline where you use the outputs of one model as inputs for a subsequent model.


You may ask, “ **But what’s novel about this approach?** ” 🤔 The answer lies in the Zero-Shot \[2\] nature of foundation models. Models such as GPT-4o or SAM 2 are called zero-shot, which means they can perform inference without a previous training step. As a result, these models can be connected in a systems perspective, as illustrated in Figure 2.


Press enter or click to view image in full size


Figure 2. A pipeline of zero-shot foundation models interconnected


In fact, some research approaches such as CaFO \[3\] combine multiple pre-trained foundation models (CLIP, DINO, DALL-E, GPT-3) to enhance few-shot visual recognition by leveraging diverse pre-training knowledge and generating synthetic data.


## 2. Foundation Models as Visual Prompting tools


### 2.1 Computer Vision Pipeline 2.0: a new paradigm


We have explored before[what is visual prompting](https://medium.com/@tenyks_blogger/cvpr-2024-foundation-models-visual-prompting-are-about-to-disrupt-computer-vision-026f2c1c3a2f) . It refers to using visual information (like images, bounding boxes, or points) as inputs or “prompts” for foundation models that can process both visual and textual information.


One of the key strengths at[Tenyks](https://www.tenyks.ai/) is visual search. We process tens of thousands of daily queries using visual prompting. For instance, Figure 3 shows how you can select a bounding box of an object to search for fine-grained details in your data.


Press enter or click to view image in full size


Figure 3. Using **visual prompting** to search for objects (e.g., school buses) even if no class for this object exist


As we have previously argued, at Tenyks, we believe that the traditional pipeline in the field of vision is at the beginning of a transition where many stages in the pipeline (e.g., labeling, training) will be replaced by modules containing[foundation models](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) that will form a so-called[Computer Vision Pipeline 2.0](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) .


### 2.2 Challenges of Visual Prompting as a glue for Foundation Models


Now, when you start to connect foundation models together, you immediately notice that building a robust system based on this paradigm is not the same as building a prototype over the weekend.


Here are some of the main challenges you can expect to encounter as soon as you get started:


1. **Performance and Scalability** 🚀📈


- Ensuring the system can handle high volumes of data and requests in real-time.
- Maintaining accuracy and speed as the scale of operations increases.


**2. Integration and Compatibility** 🔗🛠️


- Seamlessly incorporating the multi-model system into *existing* infrastructure.
- Ensuring *interoperability* with various data formats, APIs, and legacy systems.


**3. Reliability and Error Handling** 🔄🛡️


- Developing robust error detection and correction mechanisms.
- Implementing redundancies to maintain operational continuity.


## 3. GPT-4o (or Claude Sonnet 3.5) + SAM 2


### 3.1 A visual prompting pipeline


Press enter or click to view image in full size


Figure 4. Our setup: leverage GPT-4o to extract visual information, which will be used as input for SAM 2


Figure 4 shows a simple pipeline consisting of two steps. The assumption is that GPT-4o or Claude Sonnet 3.5 are powerful enough for a prompt such as:


> “For the given image, please provide three (x,y) coordinates of the gymnast”
>
>
> “For the given image, please provide the bounding box coordinates of the gymnast”


Figure 5 and 6 show the results for GPT-4o and Claude Sonnet 3.5 respectively.


Press enter or click to view image in full size


Figure 5. The results from **GPT-4o** , when queried for (x,y) coordinates, were quite **inaccurate** Press enter or click to view image in full size


Figure 6. **Claude sonnet 3.5** was unable to provide visual understanding as we expected


While GPT-4o, most of the time, provides **incorrect coordinates** , Claude Sonnet 3.5 simply refuses to provide either (x,y) coordinates or bounding boxes.


Figure 7 shows how this behaviour was consistent across 200 API requests for GPT-4o.


Press enter or click to view image in full size


Figure 7. Out of 200 attempts to identify (x,y) coordinates using GPT-4 for visual understanding, only 5 were accurate. None of the bounding box results were correct.


So, can’t we really use **any** decent **foundation model** (in **2024** ) as **input** for a **second** foundation model? **** 😱


## 4. YOLO World + SAM 2


### 4.1 Zero-shot for Computer Vision: YOLO-World


Despite our hopes being crushed by realizing that neither GPT-4o nor Claude Sonnet 3.5 is good enough to provide visual answers from an image, we found a specialized model that made the cut: YOLO-World \[4\].


Figure 7 shows how, given a text input (i.e., classes), this model accurately predicts the bounding boxes for each of the given inputs! 🍾


YOLO-World’s vocabulary even includes the word: *gymnast* ! (see 0.93 mAP on the right hand side of Figure 8).


Press enter or click to view image in full size


Figure 8. YOLO-World predictions given some text inputs (i.e., classes)


- 🔥 **Spoiler alert:** We’ll be discussing more about YOLO-World (Zero-Shot) vs YOLO v8 (Fine-tuning) in an upcoming post!


For starters, YOLO-World is a zero-shot model for object detection that can detect and localize objects in images without requiring prior training on specific object classes.


We used YOLO-World to provide bounding boxes to SAM 2 as shown in Figure 9.


Press enter or click to view image in full size


Figure 9. Final pipeline that includes **YOLO-World** and SAM 2 connected together


The *only input* we were required to provide to the whole system was the class definition for the YOLO-World vocabulary, in this case, “gymnast.” This word is enough for YOLO-World to provide the bounding box coordinates to SAM 2.


Check this[Jupyter Notebook](https://drive.google.com/file/d/1ajEJ7KTIG-cBeeGO8VdyOy08D6wQ0N3Z/view?usp=sharing) for details on the implementation. The final result is shown in Figure 8.


## 5. Conclusions


In this series about SAM 2, we described and set up the Segment Anything Model 2 (SAM 2). Then, we cascaded two foundation models (i.e., we used the outputs of model A as the inputs for model B) using visual prompting.


We discovered that the two leading[MLLMs](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) in the market, GPT-4o and Claude Sonnet 3.5, are quite inaccurate at providing coordinates or bounding boxes of objects for a given image. Instead, we found that specialized models (e.g., YOLO-World) are better suited for this job.


[Building a prototype](https://medium.com/@tenyks_blogger/how-to-build-an-image-to-image-search-tool-using-clip-pinecone-b7b70c44faac) is one thing, but in reality, there are *challenges* that even the best ML teams struggle with (e.g., integration, reliability, and adaptability when chaining together foundation models).


How can you address all these challenges for your system while keeping **maintenance costs low** ? 💸 ↘️ . Well,[Tenyks](https://tenyks.ai/) has taken the time to build visual prompting for interconnected foundation models so that you don’t have to.[Try it out if you’re curious.](https://sandbox.tenyks.ai/?utm_source=medium&utm_medium=article&utm_campaign=cascading_foundation_models_part_2)


As we have argued before, a new paradigm in computer vision is here: some traditional stages in a vision pipeline are likely to be replaced by (zero-shot) foundation models that will continually improve over time.


*How long do we have to wait to get precise bounding box coordinates from the upcoming GPT member of the family?* Probably not long.


🔥 Learn about *Multimodal Large Language Models* **(MLLM):**


- 🆕[Multimodal Large Language Models](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) (MLLMs) transforming Computer Vision


## References


\[[1\]](https://ai.meta.com/blog/segment-anything-2/) Segment Anything Model 2


\[[2](https://proceedings.mlr.press/v37/romera-paredes15.pdf) \] An embarrassingly simple approach to zero-shot learning


\[[3](https://arxiv.org/pdf/2303.02151) \] Prompt, Generate, then Cache: Cascade of Foundation Models makes Strong Few-shot Learners


\[[4](https://arxiv.org/pdf/2401.17270) \] YOLO-World: Real-Time Open-Vocabulary Object Detection


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan


*If you’d like to know more about* ***Tenyks*** *, try*[sandbox](https://sandbox.tenyks.ai/?utm_source=medium&utm_medium=article&utm_campaign=cascading_foundation_models_part_2) *.*

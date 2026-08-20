---
schema_version: "1.0.0"
document_id: "9dd30785a7e9cba7427fe1da4187fdc7cfa69c81dc55779995bbf683ad0b645e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/promise-prompt-driven-3d-medical-image-segmentation-using-pretrained-image-foundation-models"
published_at: "2023-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:19ca5ed8f5574f9203577709a40d7f5b9de06f911ca4678cba97495e6b0d84b8"
---

# Promise: Prompt-driven 3D Medical Image Segmentation Using Pretrained Image Foundation Models

Do not index


Original Paper


[https://arxiv.org/abs/2310.19721](https://arxiv.org/abs/2310.19721)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.19721](https://arxiv.org/abs/2310.19721)


**By:**[Hao Li](https://arxiv.org/search/eess?searchtype=author&query=Li%2C%20H) ,[Han Liu](https://arxiv.org/search/eess?searchtype=author&query=Liu%2C%20H) ,[Dewei Hu](https://arxiv.org/search/eess?searchtype=author&query=Hu%2C%20D) ,[Jiacheng Wang](https://arxiv.org/search/eess?searchtype=author&query=Wang%2C%20J) ,[Ipek Oguz](https://arxiv.org/search/eess?searchtype=author&query=Oguz%2C%20I)


**Abstract:**


> To address prevalent issues in medical imaging, such as data acquisition challenges and label availability, transfer learning from natural to medical image domains serves as a viable strategy to produce reliable segmentation results. However, several existing barriers between domains need to be broken down, including addressing contrast discrepancies, managing anatomical variability, and adapting 2D pretrained models for 3D segmentation tasks. In this paper, we propose ProMISe,a prompt-driven 3D medical image segmentation model using only a single point prompt to leverage knowledge from a pretrained 2D image foundation model. In particular, we use the pretrained vision transformer from the Segment Anything Model (SAM) and integrate lightweight adapters to extract depth-related (3D) spatial context without updating the pretrained weights. For robust results, a hybrid network with complementary encoders is designed, and a boundary-aware loss is proposed to achieve precise boundaries. We evaluate our model on two public datasets for colon and pancreas tumor segmentations, respectively. Compared to the state-of-the-art segmentation methods with and without prompt engineering, our proposed method achieves superior performance. The code is publicly available at
>
>
> [this https URL](https://github.com/MedICL-VU/ProMISe)


---


###


Summary Notes


##


Enhancing 3D Medical Image Segmentation with ProMISe: A Cutting-Edge Technique


Medical imaging plays a crucial role in identifying health issues, such as tumors, by distinguishing between healthy tissues and abnormalities. However, traditional methods often fall short due to the complexity and variability of medical images.


Enter ProMISe, an innovative solution set to transform 3D medical image segmentation by harnessing pretrained image models and prompt engineering. This blog explores ProMISe's approach and its promising implications for the future of medical imaging.


###


Introduction: Overcoming Challenges in Medical Imaging


Image segmentation models have significantly advanced various fields by adapting to different tasks with extensive training. Prompt engineering further tailors these models to specific requirements.


Nonetheless, the medical imaging sector faces unique obstacles, such as limited datasets and the complex nature of medical images, making direct transfer learning challenging.


###


ProMISe: The Next Generation of Medical Image Segmentation


####


ProMISe Explained


ProMISe introduces a hybrid approach that combines a pretrained vision transformer (ViT) with convolutional neural network (CNN) encoders to adeptly handle 3D medical images. It incorporates lightweight adapters and a new prompt encoder while keeping pretrained weights mostly unchanged for efficient adaptability.


####


Core Components


- **Adapters** : Target depth-related information, vital for medical image segmentation.


- **CNN Encoder** : Captures detailed image features, essential for identifying unclear boundaries in tumors and other anomalies.


- **Prompt Encoder** : Enhances model adaptability with visual prompts, improving its segmentation capability.


####


Loss Function Innovation


ProMISe introduces a boundary-aware loss function, designed to enhance edge segmentation—a crucial aspect in medical imaging for accurate diagnosis.


###


Experiments and Results: Proving ProMISe’s Effectiveness


####


Dataset Evaluation


Tested on two public datasets focusing on colon and pancreas tumor segmentation, ProMISe outperformed existing methods, showcasing its superior capability.


####


Implementation Details


The model was fine-tuned with preprocessing and augmentation techniques, utilizing a pretrained ViT-B and optimized with the AdamW optimizer for best performance.


###


The Future of Medical Image Segmentation with ProMISe


####


Impacts and Prospects


ProMISe signifies a major advancement in using pretrained image models for 3D medical segmentation, offering efficient training and high performance. It paves the way for future research and practical applications in medical imaging.


####


Forward-Looking


Further research will aim at boosting efficiency, possibly through knowledge distillation, while its open-source availability facilitates continuous improvement and innovation.


###


Ethical Standards and Accessibility


ProMISe was developed and tested ethically using publicly available data, ensuring responsible information use.


###


Conclusion


ProMISe brings a novel approach to 3D medical image segmentation, combining pretrained models and prompt engineering to overcome traditional challenges.


Its innovative features and effective methodology promise to revolutionize medical imaging, improving diagnostic accuracy and patient care. With ongoing advancements, ProMISe stands to significantly impact the medical imaging field.


###


References


A comprehensive reference list is available for those interested in the technical foundations and recent advancements that have contributed to ProMISe's development, highlighting its importance in the evolution of medical imaging technology.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models

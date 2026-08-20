---
schema_version: "1.0.0"
document_id: "c632a5b1db0f6d480ef13dcedfc73f0bf6f2a16d5eeb3df3427151d3b4d5c7ef"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-5c25eadc4d38"
canonical_url: "https://blog.roboflow.com/roboflow-100/"
published_at: "2022-11-28T14:03:00+00:00"
first_seen_at: "2026-07-25T22:08:10.223085+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:03afbc213b8ca52a0a39de62fd7e63e8de99ef2567abcfc97966edc3111d335b"
---

# 📸 Roboflow 100: A Multi-Domain Object Detection Benchmark

[Francesco](https://blog.roboflow.com/author/francesco/) ,[Jacob Solawetz](https://blog.roboflow.com/author/jacob/) ,[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published Nov 28, 2022 • 6 min read


SUMMARY


**Roboflow 100 (RF100) is an open-source object detection benchmark built to address the limited domain scope of COCO and Pascal VOC. It spans 100 datasets, 7 imagery domains, 224,714 images, and 829 class labels accumulated across more than 11,000 labeling hours, covering areas like aerial imagery, medical imaging, and satellite data that standard benchmarks omit. Initial experiments evaluating YOLOv5, YOLOv7, and GLIP on RF100 show that models trained on COCO perform well on semantically similar domains but degrade significantly on dissimilar ones, making cross-domain evaluation a better proxy for real-world generalization.**


*Advancing the state-of-the-art in object recognition with a new way to benchmark computer vision models across domains and task targets.*


📑


Read the[Roboflow 100 Paper on Arxiv](https://arxiv.org/abs/2211.13523?ref=blog.roboflow.com) .


## What is Roboflow 100?


[Roboflow 100 (RF100)](https://arxiv.org/abs/2211.13523?ref=blog.roboflow.com) is a crowdsourced, open source object detection benchmark. It consists of 100 datasets, 7 imagery domains, 224,714 images, and 829 class labels with over 11,170 labeling hours.


RF100 aims to create an accessible, transparent and open-sourced benchmark for machine learning[object detection models](https://roboflow.com/model-task-type/object-detection?ref=blog.roboflow.com) and assert generality using in the wild crawled datasets. RF100 is an Intel sponsored initiative.


You can download RF100 by following the instruction on the[GitHub repo](https://github.com/roboflow-ai/roboflow-100-benchmark?ref=blog.roboflow.com) or in[Roboflow Universe](https://universe.roboflow.com/roboflow-100?ref=blog.roboflow.com) by clicking the export button in each dataset.


You can explore and visualize the data by playing with the visualization tool below. Similar images are clustered close to each other. To interact with the full visualization, check out the[Roboflow 100 website](https://www.rf100.org/visual.html?ref=blog.roboflow.com) .


## Goal and Motivation


[Microsoft COCO](https://arxiv.org/abs/1405.0312?ref=blog.roboflow.com) and[Pascal VOC](http://host.robots.ox.ac.uk/pascal/VOC/?ref=blog.roboflow.com#:~:text=The%20PASCAL%20VOC%20project%3A,and%20comparison%20of%20different%20methods) are well-known datasets used to train and evaluate computer vision models. Pioneering computer vision research is regularly benchmarked against COCO, for example, to give researchers and engineers a common set of statistics with which to understand model performance.


COCO and VOC datasets have a limited domain scope due to the nature of the data. Models pre-trained on COCO or VOC are usually used for domain-specific tasks, and evaluating narrow domain datasets may not be the best proxy of a model's performance in the wild.


For example, COCO does not contain medical images. As a result, it is difficult to be sure how a given model will perform in the medical domain when benchmarked against COCO.


We made the Roboflow 100 to help address the limited domain problem, providing engineers and researchers with a collection of 224,714 images across 829 classes against which models can be benchmarked. RF100 is a complement – not replacement – for datasets of common objects in context.


Furthermore, we observed researchers, including the Microsoft Research teams behind[Florence](https://arxiv.org/pdf/2111.11432?ref=blog.roboflow.com) and[GLIP](https://arxiv.org/pdf/2112.03857.pdf?ref=blog.roboflow.com) , were organically citing community-created Roboflow Universe datasets to assess the robustness of models beyond COCO. Thus, we saw an opportunity to make this process of assessing model capabilities beyond common objects in context, and RF100 is the first result.


RF100 is composed of images annotated in the wild and represents real data people used to train a model. By open-sourcing RF100, we hope to help researchers test a model's ability to generalize across domains. The RF100 dataset is inspired by the robust[Objects365](https://www.objects365.org/overview.html?ref=blog.roboflow.com) and[Object Detection in The Wild](https://eval.ai/web/challenges/challenge-page/1839/overview?ref=blog.roboflow.com) benchmarking projects.


Overview of per-category metadata, including number of datasets, number of images, and number of classes across categories. Additionally, we record the average mAP@.50 value for the YOLOv5 and YOLOv7 models and the mAP@.50:.95 for the GLIP model for each category


## Data Collection


RF100 is comprised of 100 datasets selected from[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) , an online repository of open-source images. Roboflow Universe contains over 100,000 datasets and images were selected using the following criteria:


- **Effort:** The user spent substantial labeling hours working on the task;
- **Diversity:** The user was working on a novel task;
- **Quality:** The user annotated with high fidelity to the task;
- **Substance:** The user assembled a substantial dataset with nuance and;
- **Feasibility:** The user was attempting a learnable task.


Before including a dataset in the Roboflow 100 benchmark, we conducted a series of processing steps. These were:


1. Resizing all images to` 640x640` pixels;
2. Eliminating class ambiguity;
3. Splitting the data in train, validation and test and;
4. Suppressing under-representing classes (less than` 0.5%` ).


Examples of annotated images in the RF100 benchmark. The data-set names are derived from the object of interest of each collection.


## Data Overview


The data in RF100 is definitionally informed by the areas computer vision is being used in practice. When assessing an initial set of novel domains, we observed clusters to categorize datasets.


We selected seven different semantic categories to achieve comprehensive coverage of different domains:


- **Aerial** : Space, static cameras, and drones
- **Video games:** First-person shooting, multiplayer online battle arena, and robot fighting
- **Microscopic:** Human, plants, and inanimate
- **Underwater:** Aquarium and sea
- **Documents:** Social media and structured
- **Electromagnetic:** Night-vision, x-ray, thermal, and MRI
- **Real World:** Indoor **,** damage **,** vehicles, safety, electronics, animals, plant, geology, and various human activities


The following image shows a curated example of images *per category:*


Image samples from different categories. *Real World* was sampled more due to its larger size.


You can find all the datasets in the RF100 benchmark on[Roboflow Universe](https://universe.roboflow.com/roboflow-100?ref=blog.roboflow.com) .


## Dataset Statistics


In our research, we investigated different statistics. These included dataset size, bounding boxes (bbox) area, and number of categories. This helped us better understand the representation of each category of data in our dataset. The following image groups statistics by category:


This figure shows different statistics for datasets grouped by category. Notice some categories (aerial, video games) have generally smaller bounding boxes compared to others, like documents. The number of classes is low, averaging 10.


Notably, *Aerial* , *Microscopic* and *Electromagnetic* have smaller bounding boxes compared to the rest. The average number of classes across different categories is only ten, meaning in practice the use case is to identify a small set of objects.


To further visualize the data, we encoded each image with[CLIP](https://openai.com/blog/clip/?ref=blog.roboflow.com) and plotted the reduced embeddings via[TSNE](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding?ref=blog.roboflow.com) .


Scatter plot visualization of Roboflow-100 datasets CLIP vectors reduced to two dimensions via TSNE. The graph legend on the top right corner shows what category of dataset each colour denotes. This visualization helps us determine the clustering degree of our collected datasets.


A[high-performance web application](https://francescosaveriozuppichini.github.io/roboflow-100-3d-website/?ref=blog.roboflow.com) is available to help you visualize and explore the datasets.


## Experiments


We evaluated[YOLOv5](https://github.com/ultralytics/yolov5?ref=blog.roboflow.com) and[YOLOv7](https://arxiv.org/abs/2207.02696?ref=blog.roboflow.com) , two popular object detection model architectures, and[GLIP](https://arxiv.org/abs/2112.03857?ref=blog.roboflow.com) , in a zero-shot fashion, on RF100 and report the results.


Since the goal was to compare intra-model performance, we trained only one instance per dataset. We used **YOLOv5** small, **YOLOv7** base both pre-trained on[COCO](https://arxiv.org/abs/1405.0312?ref=blog.roboflow.com) and **GLIP-T** pre-trained **** on ****[O365](https://www.objects365.org/overview.html?ref=blog.roboflow.com) ,[GoldG](https://arxiv.org/abs/2104.12763?ref=blog.roboflow.com) ,[CC3M](https://research.google/pubs/pub47380/?ref=blog.roboflow.com) ,[SBU](https://papers.nips.cc/paper/2011/hash/5dd9db5e033da9c6fb5ba83c7a7ebea9-Abstract.html?ref=blog.roboflow.com) . Both **YOLOv5** and **YOLOv7** have comparable performance.


Experiments results on RF100. We recorded the average mAP@.50 value for the YOLOv5 and YOLOv7 models and the mAP@.50:.95 for the GLIP model for each category.


Supervised trained models perform well on datasets somewhat similar to COCO, while performing worst on different semantically different datasets, e.g. on *Aerial* .


Meaning,[model evaluation](https://docs.roboflow.com/train/evaluate-trained-models?ref=blog.roboflow.com) on semantically different datasets is a helpful proxy for downstream model performance, especially in domain settings that differ from the web imagery in COCO.


> [Arxiv](https://arxiv.org/abs/2211.13523?ref=blog.roboflow.com)


## Conclusion


We introduce the RF100 object detection benchmark of 100 datasets to encourage the evaluation of object detection model performance to test model generalizability across a wider array of imagery domains.


Our initial evaluation shows that the new RF100 benchmark will provide valuable insights into how new object detection models will perform in the wild on tasks previously unassessed.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Francesco](https://blog.roboflow.com/author/francesco/) ,[Jacob Solawetz](https://blog.roboflow.com/author/jacob/) ,[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (Nov 28, 2022). 📸 Roboflow 100: A Multi-Domain Object Detection Benchmark. Roboflow Blog: https://blog.roboflow.com/roboflow-100/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Francesco


Making Neural Networks go brum brum in the day, fighting Siths in the night


[View more posts](https://blog.roboflow.com/author/francesco/)


Jacob Solawetz


Founding Engineer @ Roboflow - ascending the 1/loss


[View more posts](https://blog.roboflow.com/author/jacob/)


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Object Detection](https://blog.roboflow.com/tag/object-detection/)

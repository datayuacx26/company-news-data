---
schema_version: "1.0.0"
document_id: "de888adf3a33c664c7c746fa70be178bf33ab20854f28da02d019d8d8dbbeaf7"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance---part-1-of-3"
published_at: "2024-01-25T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:156273e81b693c0f4e375b0c1888a8b526306af1003da621f0739eaa796c529a"
---

# NVIDIA TAO Toolkit: How to Build a Data-Centric Pipeline to Improve Model Performance - Part 1 of 3

In this series, we’ll build a Data-Centric pipeline using Tenyks to debug and fix a model trained with the NVIDIA TAO Toolkit.


‍


Part 1.[We demystify the NVIDIA ecosystem and define a Data-Centric pipeline tailored for a model trained with the NVIDIA TAO framework.](http://www.tenyks.ai/blog/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance---part-1-of-3)


‍


Part 2.[Using the Tenyks API, we show you how to upload a dataset and a model to the Tenyks platform.](http://www.tenyks.ai/blog/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance---part-2-of-3)


‍


Part 3.[We identify failures in our model due to data issues, fix these failures and improve our model’s performance with the fixed dataset.](http://www.tenyks.ai/blog/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance----part-3-of-3)


‍


🚨Spoiler alert! By the end of this series you will learn how Tenyks enabled us to increase performance in the worst performing class, from ~0.27 mAP to ~0.81 mAP as illustrated on Figure 1.


‍


Figure 1. Left (baseline model), Right (model with fixed dataset).


‍


## Table of Contents


1. Our task and the challenges ahead
2. Detour: The NVIDIA Ecosystem 101
3. Tenyks API
4. Defining a Data-Centric Pipeline
5. Training with the NVIDIA TAO Toolkit
6. What’s next


‍


## 1. Our task and the challenges ahead


By the time you finish this series, you will be able to build your own Data-Centric pipeline (see Figure 2) using state-of-the-art tools developed by Tenyks to debug and fix a computer vision model trained with the NVIDIA TAO framework.


Figure 2. Overview of the Tenyks Data-Centric pipeline we will build in this Series


‍


### 1.1 Task: Detecting cars, traffic lights, crosswalks


We will be working with an autonomous vehicles dataset. Imagine you are tasked with building a system capable of detecting various types of objects on the road (see Figure 3).


‍


Figure 3. Autonomous vehicles dataset


‍


Please feel free to download a copy of the dataset in the COCO format[here](https://drive.google.com/drive/folders/1TyMz5Lsgp-goA4cgxfxGVbxU1dWjyv9E?usp=sharing) .


‍


### 1.2 Challenges


> Data-Centric, as explained in more detail in[other articles](https://medium.com/@tenyks_blogger/where-is-my-model-failing-a-data-centric-approach-cd70ad2811f6) , was proposed by ML researcher and entrepreneur Andrew Ng. You can find plenty of resources, including formal[courses](https://dcai.csail.mit.edu/) on Data-Centric.


1. The first challenge arises when translating theory into practice. Imagine you are given an ML system that isn’t performing as expected, and your task is to improve its performance. Where do you start? What do you do first? What are the common pitfalls you should avoid?
2. Putting together a bunch of scripts in a Jupyter notebook is one thing; building a repeatable process that can help your team solve similar problems in the future is another.
3. Additionally, what if you’re dealing with a model trained with the NVIDIA TAO framework? We love NVIDIA, but sometimes their documentation isn’t easy to follow.


‍


Since we’ll be using the NVIDIA TAO framework, let’s start by coming to terms with some of the concepts of the NVIDIA ecosystem that are relevant to keep in mind when working with the NVIDIA TAO Toolkit.


‍


## 2. Detour: The NVIDIA Ecosystem 101


### 2.1 NVIDIA TAO Toolkit


Figure 4. NVIDIA TAO Toolkit workflow diagram


At the center stage is the[NVIDIA TAO Toolkit](https://developer.nvidia.com/tao-toolkit) (see Figure 4), a CLI and Jupyter Notebook-based solution that abstracts away the complexity of AI and deep learning frameworks.


‍


- **What can I do with this?** You can fine-tune[NVIDIA pre-trained models](https://developer.nvidia.com/ai-models) with your own data.
- **What’s the output of the NVIDIA TAO Toolkit?** A trained model that can be deployed in[Deepstream](https://developer.nvidia.com/deepstream-sdk) ,[Riva](https://www.nvidia.com/en-us/ai-data-science/products/riva/) , or[Triton](https://developer.nvidia.com/triton-inference-server) (more on these terms below).
- **Why might I want to use NVIDIA TAO?** 1) Robust workflows for fine-tuning, 2) Optimization of models for edge devices, 3) Scalability across multiple GPUs, and 4) Integration with CUDA, cuDNN, and[TensorRT](https://developer.nvidia.com/tensorrt) .
- **What system requirements do I need to use the NVIDIA TAO Toolkit?** A list of all the requirements you need can be found[here](https://docs.nvidia.com/tao/tao-toolkit/text/tao_toolkit_quick_start_guide.html#requirements) ! Hint: Here’s a[list of AWS images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) that have everything you need!
- **How can I interact with the NVIDIA TAO Toolkit?** The easiest way is to use a Jupyter Notebook that will contain all the code you need to train a model. These notebooks are contained in the NGC catalog (see below).
- **What are some good resources to learn more about the NVIDIA TAO Toolkit?** (1) The NVIDIA TAO Toolkit official documentation, (2) This website contains some of the best tutorials on the NVIDIA ecosystem, and (3) This video contains a clear guide to install the NVIDIA TAO Toolkit.


‍


### 2.2 The NGC Catalog


The[NGC Catalog](https://catalog.ngc.nvidia.com/?filters=&orderBy=weightPopularDESC&query=) is a GPU-optimized hub with performance-optimized frameworks, SDKs, and models to build Computer Vision and Speech AI applications.


> To run an NVIDIA TAO pipeline, you need to create an account on the NGC Catalog. After that, you will be provided with an API key to download resources (e.g. Jupyter Notebooks) from the NGC Docker registry.


‍


### 2.3 Deepstream, Triton and TensorRT


[Deepstream](https://developer.nvidia.com/deepstream-sdk) is a streaming analytics toolkit for building end-to-end AI-powered solutions. It takes streaming data as input and uses AI and computer vision to generate insights from pixels.


> **NVIDIA TAO** can be used to train and adapt models that can be integrated into applications built with DeepStream for real-time video analysis.


‍


[Triton Inference Server](https://developer.nvidia.com/triton-inference-server) provides a scalable and efficient way to serve models in production environments. It’s akin to a backend where you can run your models and process HTTP requests with images.


> The combination of NVIDIA TAO and Triton Inference Server provides an end-to-end workflow: **NVIDIA TAO** handles the training and adaptation, while Triton manages the serving and scaling of models in production.


‍


[TensorRT](https://developer.nvidia.com/tensorrt) is a high-performance deep learning inference library with the goal of optimizing models for efficient inference on NVIDIA GPUs. Some optimizations include: precision calibration, layer fusion, kernel auto-tuning, dynamic tensor memory, and support for[INT8 quantization](https://developer.nvidia.com/blog/achieving-fp32-accuracy-for-int8-inference-using-quantization-aware-training-with-tensorrt/) .


> After a model has been trained with **NVIDIA TAO** , TensorRT can be employed to optimize the model for efficient inference.


‍


### 2.4 NVIDIA Jetson


Jetson is a platform for AI at the edge. Once you have trained your model with NVIDIA TAO, you can make use of these power-efficient production modules and developer kits that offer an AI software stack for high-performance acceleration to power AI at the edge.


‍


Many folks get confused here, hence let’s break down the NVIDIA Jetson platform.


- [Jetson Developer Kits](https://developer.nvidia.com/embedded/jetson-developer-kits) are development platforms that include a Jetson module along with additional components such as carrier boards, power supplies, and peripherals. These kits are intended for *developers* to prototype, test, and develop applications before deploying them on end-user products. There are three types of Jetson Developer Kits (ordered from more to less performant):[Jetson AGX Orin Developer Kit](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) (up to 275 TOPS),[Jetson Orin Nano Developer Kit](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) (40 TOPS),[Jetson Nano Developer Kit](https://developer.nvidia.com/embedded/jetson-nano-developer-kit) .
- [Jetson Modules](https://developer.nvidia.com/embedded/jetson-modules) are compact, power-efficient compute modules that integrate the CPU, GPU, and other essential components into a single package. These modules are designed to be integrated into end-user products or systems to enable AI capabilities.


‍


🤔 What does **TOPS** stand for? In the context of NVIDIA’s computing performance measurements, TOPS stands for “ **trillions of operations per second”.**


‍


## 3. Tenyks API


### 3.1 Create automated workflows to debug your data


Data is at the core of what we do at Tenyks: 👩🔬 we are pioneering the way humans interact with AI. To achieve that goal, we focus on creating optimal tooling for ML teams to debug AI systems at scale using a Data-Centric approach.


‍


We’ve previously detailed how some of these tools operate in our earlier posts. For instance, you can use[multi-modal search](https://medium.com/@tenyks_blogger/multi-modal-image-search-with-embeddings-vector-dbs-cee61c70a88a) to identify edge cases or swiftly[uncover all the mispredictions and undetected objects](https://medium.com/@tenyks_blogger/multiclass-confusion-matrix-for-object-detection-6fc4b0135de6) where your model is failing. Now, we’ll turn our attention to the[Tenyks API](https://docs.tenyks.ai/reference) , a set of endpoints that can assist you in building automated workflows to debug computer vision models.


‍


We offer you a sneak peek into the endpoint for creating a dataset. In the upcoming posts of this series, we will take a closer look at the rest of them.


‍


### 3.2 Create a dataset


The code below demonstrates how to make a POST request to the URL[https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets](https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets) to create a dataset on your Tenyks account.


‍


You can specify: i) the type of computer vision task (e.g., object_detection) you are interested in, ii) the location of your AWS bucket where your data will be stored, and iii) the name of your dataset.


‍


```text


curl --request POST \
--url https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets \
--header 'accept: application/json' \
--header 'content-type: application/json' \
--data '
{
"task_type": "object_detection",
"images_location": {
"type": "aws_s3",
"credentials": {
"aws_access_key_id": "aws_access_key_id",
"aws_secret_access_key": "aws_secret_access_key",
"region_name": "eu-central-1"
},
"s3_uri": "http://s3.amazonaws.com/[bucket_name]/"
},
"key": "face_detection",
"display_name": "face_detection"
}'


```


‍


## 4. Defining a Data-Centric pipeline


Figure 5. Tenyks Data-Centric pipeline in detail


Figure 5 describes the Data-Centric Pipeline we will build.


- **Data ingestion.** In this stage, we use the Tenyks API to ingest our dataset on the Tenyks platform.
- **mAP analysis.** Once your data is on the platform, we conduct a performance analysis to explore the best and worst-performing classes.
- **Data imbalance.** We identify potential classes with imbalance issues.
- **Class selection.** Based on the previous analysis, we select the classes with the most room for improvent (i.e., often the less performing classes).
- **Failure and error analysis.** We use Tenyks to identify errors, biases and failures in the selected classes.
- **Data slice analysis.** Once a failure is found, we conduct a data slice analysis to identify the potential root cause of the failure.
- **Fixing dataset.** We use one of several methods to fix the data failures we found.
- **Model performance comparison.** We systematically compare how our efforts to build a fixed dataset impact model performance 🏁.


‍


In the rest of the series, we will walk you through all the steps required to build this end-to-end pipeline for your NVIDIA TAO model.


‍


## 5. Training with the NVIDIA TAO Toolkit


This step assumes you already have a trained model with the NVIDIA TAO Toolkit.


‍


Setting up the NVIDIA TAO Toolkit used to be a nightmare! We even wrote a[post detailing how to bypass some common roadblocks](https://medium.com/me/stats/post/d95d08eb46d1) during the TAO installation (e.g., broken docker images, incompatibility of dependencies).


‍


It turns out that since late 2023, you can now run the NVIDIA TAO Toolkit in a[Colab notebook](https://colab.research.google.com/github/NVIDIA-AI-IOT/nvidia-tao/blob/main/tensorflow/yolo_v4/yolo_v4.ipynb) 😉


‍


Please, if you don’t have a model trained with the NVIDIA TAO Toolkit, run the notebook linked above. After you have a trained model, please download the following:


- Dataset: images and annotations (in COCO format).
- Model: training set and test set predictions (in COCO format).


‍


💡 Hint:[This short overview](https://docs.tenyks.ai/docs/coco-format) can help you get up to speed with the COCO format.


‍


## 6. What’s next


We have defined an ambitious goal for this series: build a Data-Centric pipeline to debug and fix a model trained with the NVIDIA TAO Toolkit.


‍


In this first post, we laid the ground for our task and discussed the challenges we’ll face. We introduced the dataset we’ll use during the series. We demystified some of the many moving parts of the NVIDIA ecosystem, and clarified how they relate to the NVIDIA TAO Toolkit. At a high level, we established the kind of Data-Centric pipeline we’ll build with the help of the Tenyks platform. We used the NVIDIA TAO Toolkit to train a model that we’ll utilize in the following posts of this series.


‍


Stay tuned for Part 2! 💙


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


‍


*If you would like to know more about* ***Tenyks*** *, sign up for a*[sandbox account](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*


‍


‍

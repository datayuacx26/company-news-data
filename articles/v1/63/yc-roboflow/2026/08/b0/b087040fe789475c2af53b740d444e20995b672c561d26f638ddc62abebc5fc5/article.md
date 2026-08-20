---
schema_version: "1.0.0"
document_id: "b087040fe789475c2af53b740d444e20995b672c561d26f638ddc62abebc5fc5"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/how-to-build-a-defect-detection-system/"
published_at: "2026-08-12T17:33:00+00:00"
first_seen_at: "2026-08-18T22:20:39.292204+00:00"
fetched_at: "2026-08-18T22:20:41.629330+00:00"
content_hash: "sha256:33bd30eca6602e13840307e80499c3057e53c1d9660307a15c55d48c6c73c73b"
---

# Build a Defect Detection System

[James Gallagher](https://blog.roboflow.com/author/james/) ,[Aarnav Shah](https://blog.roboflow.com/author/aarnavshah/)


Published Aug 12, 2026 • 8 min read


SUMMARY


**Building an automated defect detection system with computer vision follows a repeatable pipeline: collect images of the target product, annotate defects, train a model, and deploy it to trigger automated actions like removing a part from an assembly line. This guide walks through each step using ceramic tile defect data sourced from Roboflow Universe, with annotation, training, and testing handled inside Roboflow. The same approach extends to any manufactured product where visual quality checks currently depend on manual inspection.**


The earlier you can catch defects in product[manufacturing](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) pipelines, the better: faster detection aids in diagnosing any potential issues that caused the defect, and ensuring that the defect does not make its way further through a manufacturing process.


Adding an automated step to screen for defects increases the chance errors and defects are caught before advancing through a manufacturing pipeline. This automated step can be implemented using computer vision.


Today, I'll walk you through building a defect detection system using vision AI.


## How Can Computer Vision Help with Defect Detection?


With computer vision, you can detect any defects visible to a[camera](https://ai1.roboflow.com/?ref=blog.roboflow.com) . When a defect is detected, you can trigger automated systems to remove a product from an assembly line, for example. Or, if your defect rate reaches a defined incidence level, you can alert workers that there may be an issue on the assembly line.


In this guide, we are going to talk through how to build an automated defect detection system with Roboflow. We will show:


1. How to use computer vision models
2. Collecting data for a computer vision model
3. Labeling and preparing data for model training
4. [Training](https://roboflow.com/train?ref=blog.roboflow.com) and testing a model
5. [Deploying](https://roboflow.com/deploy?ref=blog.roboflow.com) a model


We will build a computer vision model to identify defects in ceramic tiles, although the example could extend to any defect. Here is an example output from the system we are going to build:


Without further ado, let’s get started!


## What Is Defect Detection?


[Defect detection](https://roboflow.com/solutions/defect-detection?ref=blog.roboflow.com) refers to methods of identifying issues in a product. Defect detection can refer to a vast range of methods of assuring the quality of a product, including measuring the size of a product, checking for the presence or absence of components, and checking for damage in components.


Computer vision systems can monitor assembly lines in real time and check for defects. Consider a scenario where you are manufacturing ceramic tiles. You want to make sure no scratches have been made in a tile before the material moves to the next stage of production. Computer vision can inspect every tile automatically, raising an alert when a scratch is found.


Using computer vision, businesses can proactively detect defects in products and rectify issues as they happen. Further, you can check for the presence or absence of components as part of[automated quality assurance (QA) processes](https://roboflow.com/ai/quality-control?ref=blog.roboflow.com) .


Over time, this data can be used to better understand points of concern in a manufacturing process. For instance, if a particular defect is identified more when a product or part goes through a particular machine, it may be indicative that there is an issue with that specific machine.


## How to Build a Defect Detection System


To build an automated defect detection system, you will need:


1. A camera that you can connect to a computer. Edge deployment cameras such as the Luxonis OAK, or webcams attached to an NVIDIA Jetson, and WiFi-enabled cameras that can use the[Roboflow hosted API](https://docs.roboflow.com/deployment/roboflow-cloud/serverless-api?ref=blog.roboflow.com) are common choices.
2. A consistent source of power to the camera and computer.
3. A computer vision model that identifies a defect, or multiple types of defect.


Below, we will show how to identify ceramic tile defects with a computer vision model. This model will be able to identify edge chipping, holes in the ceramic, and lines that should not be present on the tile.


### Step 1: Collect Data


*If you already have data, skip to the next step!*


First, we need to collect data relevant to our use case. Gathering data representative of your use case, and the environment in which your model will be deployed, is key to achieving high model performance.


You can also search for data from[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) , a community with over 750,000 computer vision datasets across different use cases.


For this guide, we are going to use a[ceramic tile dataset](https://universe.roboflow.com/datn-6f5ux/ceramic-tile-defects?ref=blog.roboflow.com) from Roboflow Universe. This dataset contains annotated images of ceramic tiles, such as this one:


To download the dataset for this guide, click “Download Dataset” on the Universe project page.


Then click the checkbox to download the dataset as a ZIP file.


Choose the COCO annotation format for the dataset download.Unzip this file, as we will use the data inside in the next step.


With our data ready, we can upload it to Roboflow.


First, create a Roboflow account, then click “Create New Project” on the dashboard:


Choose a project name and specify “Object Detection” as the project type to use.[Object detection](https://blog.roboflow.com/object-detection/) allows you to identify the location of different objects in an image or video.


Next, we need to[upload data to Roboflow](https://docs.roboflow.com/datasets/create-and-upload/adding-data?ref=blog.roboflow.com) . To do so, drag and drop all images and annotations you want to use in training your model:


This step will take a few moments depending on how many images you want to upload. Split the dataset into a 70/20/10 split:


With our images uploaded, we can move to the next step: annotating any extra images.


### Step 2: Annotate Images


[Roboflow Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) provides an interactive web interface through which you can annotate images. The images we uploaded previously had annotations already made to them. To add annotations to new images, first upload some into the project, and then click “Annotate” in the sidebar of your project in the Roboflow dashboard, then click an image to start annotating. This will open the image in an annotation view.


To annotate the image, click the bounding box tool in the right sidebar, then drag your cursor around each object of interest you want to annotate. After you drag a box around an annotation, you will be asked to choose a class to add to the annotation. You can choose from existing classes you have created, or create a new one. Then, press the Enter key on your keyboard to save your annotation.


There are a few tools you can use to speed up annotation in Roboflow, including[Smart Select](https://docs.roboflow.com/datasets/annotate/annotate/ai-labeling/smart-select?ref=blog.roboflow.com) . Smart Select allows you to annotate images with polygons. Polygon annotations allow you to achieve better model performance, but used to take more time to create than boxes.


To use Smart Select, click the magic wand icon in the right sidebar. Once you have configured Smart Select following the on-screen instructions, you can hover over any object in the image. Smart Select will recommend an annotation.


Once you have annotated all of your images, you are ready to create a dataset version.


### Step 3: Create a Dataset Version


Before you can train a model, you need to create a dataset version. Dataset versions are frozen-in-time, allowing you to maintain the state of a dataset at a point in time.


To create a dataset version, click “Versions” in the Roboflow sidebar. A page will appear where you can configure data preprocessing and augmentations. For first versions of a model, we recommend setting no pre-processing or augmentation steps. This allows you to measure baseline performance with your annotated data.


Let’s leave the preprocessing and augmentation values blank for now. At the bottom of the page, click “Create” to create a version of your dataset.


Generating a dataset version can take a few minutes depending on how large your dataset is. Once you have created a dataset version, you can train the first version of your defect detection model. We’re one step closer to identifying ceramic tile defects using computer vision.


### Step 4: Train a Computer Vision Model


You can train[computer vision models](https://playground.roboflow.com/models?ref=blog.roboflow.com) on Roboflow in a few clicks with[Roboflow Train](https://roboflow.com/train?ref=blog.roboflow.com) . Click “Train Model'' on the dataset version page to which you were taken after creating a dataset version.


You will be asked to select a training architecture. Choose “Roboflow RF-DETR Small”:


After following all on-screen instructions and clicking "Start Training", a cloud-hosted computer will be assigned your training job. The amount of time it takes to train a model depends on how many images you have used. As your model trains, a graph on the page will update showing model performance for each training step.


You will receive an email when your defect detection model is ready to use.


### Step 5: Test the Defect Detection Model


When your model has been trained, test it out by clicking “Deploy” in the Roboflow sidebar. A box will appear in which you can run inference on images from your test set (images not used for training) to see how your model performs on unseen data. You can also upload new images to test.


Above, our model successfully identified various defects present in a ceramic tile.


### Step 6: Deploy Model to Production


With a model ready, there is one big question left to answer: how can you deploy the model for use in production? Roboflow provides a range of SDKs and tools for use in deploying your model. You can deploy your model to the following devices:


1. [NVIDIA Jetson](https://docs.roboflow.com/deployment/self-hosted/self-hosted?ref=blog.roboflow.com)
2. [Raspberry Pi](https://docs.roboflow.com/deployment/self-hosted/inference-server/install/raspberry-pi?ref=blog.roboflow.com)
3. [Luxonis OAK](https://docs.roboflow.com/deployment/self-hosted/sdks/luxonis-oak?ref=blog.roboflow.com)
4. [Web (via roboflow.js)](https://docs.roboflow.com/deployment/self-hosted/sdks/web-browser?ref=blog.roboflow.com)
5. [iOS](https://docs.roboflow.com/deployment/self-hosted/sdks/ios-sdk?ref=blog.roboflow.com)
6. [CPU devices (via Docker)](https://docs.roboflow.com/deployment/self-hosted/inference-server/install/linux?ref=blog.roboflow.com)


To learn more about each deployment option available for your model, refer to the deployment compatibility matrix published in the Roboflow documentation.


Once you have chosen a deployment option, you will need to configure the device(s) on which you want to use your model. The list above links to our guides for each deployment option.


Whatever deployment device you choose, you can develop logic that meets your business requirements. For example, if you need to visually inspect for the presence of certain objects (i.e. four screws present in each corner of an image), you can build that logic using the results from your model.


## Conclusion


In this guide, we have demonstrated how to build an automated visual inspection and defect detection system with computer vision.


**Further reading:**


- [Part Inspection with Computer Vision](https://blog.roboflow.com/part-inspection-using-computer-vision/)
- [Vision Inspection Systems](https://blog.roboflow.com/vision-inspection-systems/)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[James Gallagher](https://blog.roboflow.com/author/james/) ,[Aarnav Shah](https://blog.roboflow.com/author/aarnavshah/) . (Aug 12, 2026). Build a Defect Detection System. Roboflow Blog: https://blog.roboflow.com/how-to-build-a-defect-detection-system/*


### Written by


James Gallagher


James is a technical writer at Roboflow, with experience writing documentation on how to train and use state-of-the-art computer vision models.


[View more posts](https://blog.roboflow.com/author/james/)


Aarnav Shah


Growth and ML intern at Roboflow and previously a blog contributor with 50+ articles demonstrating how to build, train, and deploy computer vision models for real-world use cases.


[View more posts](https://blog.roboflow.com/author/aarnavshah/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [RF-DETR](https://blog.roboflow.com/tag/rf-detr/)
- [Manufacturing](https://blog.roboflow.com/tag/manufacturing/)

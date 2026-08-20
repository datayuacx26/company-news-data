---
schema_version: "1.0.0"
document_id: "37931fd3b5b3dc6801cbcfe0757a68802a4010ac5c932a4a08e6eb6b6d96d875"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/bottle-cap-inspection/"
published_at: "2026-08-06T09:33:00+00:00"
first_seen_at: "2026-08-19T20:55:31.297395+00:00"
fetched_at: "2026-08-19T20:55:33.470348+00:00"
content_hash: "sha256:d9ef50db5252c992fb4c6f06d0af7541a98af81ba823cd6347b939d4a76d37ea"
---

# Bottle Cap Inspection with Computer Vision

[James Gallagher](https://blog.roboflow.com/author/james/) ,[Aarnav Shah](https://blog.roboflow.com/author/aarnavshah/)


Published Aug 6, 2026 • 7 min read


SUMMARY


**A computer vision system can automatically classify bottle caps as properly sealed, loose, or missing before bottles reach packaging, reducing the manual inspection burden on assembly lines. This tutorial walks through collecting and labeling bottle cap images using Roboflow Annotate, training an object detection model, and deploying it on your own hardware with Roboflow Inference. The resulting system flags non-conforming caps in real time and its output can feed downstream rejection logic or defect-rate tracking.**


Before bottles can be packaged for distribution, inspections must be run to assure the integrity of the bottle cap. If a bottle cap is not properly sealed, it must be rejected before proceeding to packaging and distribution. Computer vision can be used to for[bottle cap inspection](https://roboflow.com/ai/cap-and-closure-inspection?ref=blog.roboflow.com) to identify whether a cap is or is not properly sealed, and check whether there is or is not a cap present on a bottle.


## Bottle Cap Inspection with AI


In this guide, we are going to walk through how to build a bottle cap inspection system that verifies the integrity of a bottle cap. By the end of this guide, we will have a system that can automatically identify whether a cap is sealed. Here is an example of the system running:


Without further ado, let’s get started.


### Step 1: Create a Project


First,[create a free Roboflow account](https://roboflow.com/?ref=blog.roboflow.com) . Then, go to your Roboflow dashboard. Click the “Create a Project” page to create a project. You will be taken to a page where you will be asked to configure your project.


Choose a name for your project. Select “Object Detection” as your project type. Once you have filled out the required fields, click “Create Project”.


### Step 2: Upload Bottle Cap Data


To build a computer vision system, you need to label images with objects of interest. We need to train our system to identify whether a bottle cap is or is not properly sealed. For that, we will need images that show a bottle cap that is both sealed, not properly sealed, and missing. For optimal performance, we recommend gathering images of bottle caps from your assembly line.


Here are a few example images of bottles with different caps:


**Left: A bottle with an unsealed cap. Right: A bottle with a properly sealed cap.**


We recommend collecting 20-50 images that show all types of defects you want to identify for your first model. For this example, we have collected images that show:


- A sealed cap;
- A loose cap, and;
- A missing cap.


Once you have created a project, you will be taken to a page on which you can upload your data. To upload data, drag images onto the upload data page. Your images will be processed in your browser.[Use the images from this dataset if you don't want to collect your own](https://universe.roboflow.com/capjamesg/bottle-cap-integrity?ref=blog.roboflow.com) .


Then, click “Save and Continue” to upload your images.


### Step 3: Label Bottle Cap Images


The images we uploaded from this[dataset](https://universe.roboflow.com/capjamesg/bottle-cap-integrity?ref=blog.roboflow.com) had annotations already made to them. To annotate new images, drop them into your project, open[Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) from the left sidebar, and select them. This process is also known as labeling.


To create a label, first press “b” on your keyboard or click the box tool in the right sidebar. This will toggle the bounding box tool for use in drawing object detection labels.


Click where you want to start drawing your label on the image then drag your cursor around the object of interest. When you have surrounded the region of interest – a full or loose cap – with a box, stop clicking. You can then select a class to assign to your label.


Label all bottle caps in the images you uploaded to Roboflow.


### Step 4: Generate a Dataset Version


With all of the images labeled, you can generate a dataset version. A dataset version is a snapshot of your labeled images. You can apply[preprocessing and augmentation steps](https://blog.roboflow.com/why-preprocess-augment/) to prepare your model for training and improve model performance, respectively.


To generate a dataset, click “Generate” in the sidebar. You will be taken to a page on which you can configure your dataset version.


For your first dataset version, we recommend leaving the preprocessing steps as the default. We recommend applying the following augmentations for your first version:


1. Greyscale
2. Noise (up to 1% of pixels)


You should only apply a greyscale augmentation if bottle cap color does not matter. If you need to verify the color of a bottle cap, do not apply a greyscale augmentation.


Click the “Create” button at the bottom of the page to create your dataset version.


### Step 5: Train a Bottle Cap Inspection Model


With a dataset ready, you can[train a model](https://roboflow.com/train?ref=blog.roboflow.com) to inspect bottle caps.


To start training a model, click the "Train Model" button on your dataset page.


A pop up will appear in which you can configure your model training job. From this pop up, select “[RF-DETR small](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) ” architecture.


Once you have configured your training job, an estimate will appear that shows roughly how long it should take to train your model.


You can monitor your training job in real time from your dataset page. Once your training job has been allocated to a machine, a graph will appear that shows the performance of your model as it trains.


You will receive an email once your model has trained.


When your model has finished training, you can test it from the "Test" tab available in the sidebar of your project.


Here is an example of our model running to identify a no cap:


Our model successfully identified that a bottle cap isn't visible.


### Step 6: Create a Workflow


In order to automate this task of inspecting bottle caps, assemble a custom[workflow](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiUXZ0MVc1d2pTUjFyTHlKNHl4Y3ciLCJ3b3Jrc3BhY2VJZCI6ImVHM1R4bXRjTUlOSFNiTXhOQVgwNUxKTEtreDEiLCJ1c2VySWQiOiJlRzNUeG10Y01JTkhTYk14TkFYMDVMSkxLa3gxIiwiaWF0IjoxNzg3MTY0MjE5fQ.prhqNsT-wRvr2CPWtzT2fc4X0MD5ir6gzTdWitSqQJY?ref=blog.roboflow.com) using[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) :


Create a workflow by going to the "Workflows" tab on the left and clicking "Create Workflow":


Add an Object Detection Model block. Set the block name to model and set the model ID to your model ID (ex. bottle-cap-integrity-gb3ey/2). This block processes your input image and outputs bounding box coordinates with class predictions.


Add a Bounding Box Visualization block below the model node. Connect the workflow input image and model.predictions into it. This step renders detection boxes on the original image frame.


Add a Label Visualization block below the bounding box node. Pass the image output from bounding_box_visualization into this block to overlay class names and confidence scores on top of the boxes.


Finally, specify the workflows outputs:


### Step 7: Deploy the Bottle Cap Inspection Model


With a workflow ready, you can deploy it on your own hardware using[Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com) . Roboflow Inference is high-performance software that lets you run[computer vision models](https://playground.roboflow.com/models?ref=blog.roboflow.com) on images, videos, live camera streams, and RTSP streams.


For this guide, we will test our model on an image. Refer to the Inference documentation to learn how to deploy a model on a camera stream in real time.


To get started, we need to install Inference. We also need to install[supervision](https://supervision.roboflow.com/latest/?ref=blog.roboflow.com) , a Python package with utilities for working with computer vision models.


To install the required dependencies, run the following command:


```text
pip install --user inference-sdk python-dotenv
```


Next, you need to export your Roboflow API key into your environment. To do so, set an environment variable called` RF_API_KEY` :


```text
RF_API_KEY="insert_your_api_key_here"
```


[Learn how to retrieve your Roboflow API key.](https://docs.roboflow.com/reference/authentication/authentication/find-your-roboflow-api-key?ref=blog.roboflow.com)


Next, create a Python file and add the following code:


```text
# 1. Import the library
from inference_sdk import InferenceHTTPClient
import os
import base64
from dotenv import load_dotenv


load_dotenv()


IMAGE_PATH = "bottle.png"


# 2. Connect to your workflow
client = InferenceHTTPClient(
api_url="https://serverless.roboflow.com",
api_key=os.environ.get("RF_API_KEY")
)


# 3. Run your workflow on an image
result = client.run_workflow(
workspace_name="aarnavs-space",
workflow_id="bottle-cap-annotated",
images={
"image": IMAGE_PATH # Path to your image file
},
use_cache=False # The cache can serve a stale workflow after edits; set True once the workflow is stable
)


# 4. Get your results
predictions = result[0]["predictions"]["predictions"]
print(predictions)


# 5. The workflow draws the boxes in the cloud so you can just save the returned image
with open("annotated.png", "wb") as f:
f.write(base64.b64decode(result[0]["annotated_image"]))
print("Saved annotated.png")


```


Above, replace` bottle-cap-annotated` with your workflow ID. Replace bottle.png with the name of the image on which you want to run inference.


Let’s run the code on an image where a bottle cap isn't visible. Here are the results:


Our model successfully identified that the bottle cap wasn't there.


You can integrate this system into your business logic. For example, you can automatically reject bottles whose caps are not properly sealed. You can count the number of bottles whose caps were not properly sealed or were missing each day to monitor defect rates over time.


## Bottle Cap Inspection with AI


In this guide, we built a bottle cap inspection system with computer vision. Our system was trained to identify properly sealed bottle caps, bottle caps that are not properly sealed, and bottles that are missing caps. To do this, we gathered images showing these bottle cap statuses, labeled them, then trained a vision model.


We then deployed our system onto our own hardware using[Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com) .


Learn more about building industrial inspection systems with computer vision. Contact the[Roboflow sales team](https://roboflow.com/sales?ref=blog.roboflow.com) to talk to an AI expert about architecting a unique solution to identify defects on assembly lines.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[James Gallagher](https://blog.roboflow.com/author/james/) ,[Aarnav Shah](https://blog.roboflow.com/author/aarnavshah/) . (Aug 6, 2026). Bottle Cap Inspection with Computer Vision. Roboflow Blog: https://blog.roboflow.com/bottle-cap-inspection/*


### Written by


James Gallagher


James is a technical writer at Roboflow, with experience writing documentation on how to train and use state-of-the-art computer vision models.


[View more posts](https://blog.roboflow.com/author/james/)


Aarnav Shah


Growth and ML intern at Roboflow and previously a blog contributor with 50+ articles demonstrating how to build, train, and deploy computer vision models for real-world use cases.


[View more posts](https://blog.roboflow.com/author/aarnavshah/)


### Topics


- [Manufacturing](https://blog.roboflow.com/tag/manufacturing/)
- [Object Detection](https://blog.roboflow.com/tag/object-detection/)
- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [RF-DETR](https://blog.roboflow.com/tag/rf-detr/)

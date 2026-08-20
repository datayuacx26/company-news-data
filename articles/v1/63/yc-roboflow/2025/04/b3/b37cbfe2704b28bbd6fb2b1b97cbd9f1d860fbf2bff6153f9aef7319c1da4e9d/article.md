---
schema_version: "1.0.0"
document_id: "b37cbfe2704b28bbd6fb2b1b97cbd9f1d860fbf2bff6153f9aef7319c1da4e9d"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/redzone-monitoring-computer-vision/"
published_at: "2025-04-10T17:34:00+00:00"
first_seen_at: "2026-07-28T00:08:32.096830+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:77b8d9e9b042f0bcecb57880197c60aee06bb338a7a1ac09cb51d4f556563dab"
---

# How to Monitor Red Zones with Computer Vision

[James Gallagher](https://blog.roboflow.com/author/james/)


Published Apr 10, 2025 • 6 min read


SUMMARY


This tutorial builds a red zone monitoring system using Roboflow Workflows that tracks people and vehicles entering a designated area in an industrial facility and logs how long they spend in the zone. The Workflow uses an RF-DETR model pretrained on the Microsoft COCO dataset for detection, a ByteTracker block for object tracking, and a Time in Zone block to define and monitor the restricted area. Once built, the Workflow runs on live camera feeds via Roboflow Inference and can be extended to log entry and exit events to external systems.


When you are managing an industrial facility, you may want to monitor specific "red zones" to ensure that people only go in and out when it is safe to do so.


In this guide, we are going to walk through how to build a red zone monitoring solution with[Roboflow Workflows](https://blog.roboflow.com/workflows/) , a web-based computer vision appliaction builder.


We will create a tool that tracks people and vehicles entering a designated zone. This can then be run on camera feeds of your industrial facility. Here is an example of the system in action:


0:00


/ 0:12


This guide also has an accompaying YouTube video:


Without further ado, let's get started!


## Step #1: Create a Workflow


First, create a free Roboflow account. Then, click Workflows in the left sidebar of your Roboflow dashboard. Click "Create a Workflow".


You will be taken to a blank Workflow editor in which you can start building your application:


## Step #2: Add a Model


We need a model that can detect people and vehicles. For this guide, we are going to use a model trained on the Microsoft COCO dataset. This dataset can identify people, cars, trucks, and more.[View a full list of the classes that are in the Microsoft COCO dataset.](https://blog.roboflow.com/microsoft-coco-classes/)


Click "Add a Model", then choose "Object Detection Model":


A window will open in which you can configure the model to use.


Click "Public Models" and select the[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) model:


This will use a fast COCO model that can identify vehicles, people, and more.


You can use any model you train on Roboflow, too. This is useful if you want to identify specific objects that aren't people or vehicles, like a rig or a pipe.


If you already have a model trained, you can select it from the Your Models tab. Otherwise, refer to our[Getting Started guide](https://blog.roboflow.com/getting-started-with-roboflow/) to learn how to train a model to identify custom objects.


## Step #3: Enable Object Tracking


Before we define a red zone to monitor, we need to enable object tracking using the predictions from the model we set up in the last step.


Search for the "Byte Tracker" block and add it to your Workflow:


We are now ready to set a red zone to monitor.


## Step #4: Create a Red Zone


Next, we need to define the "red zone" that we want to monitor. For this, we can use the Time in Zone block. This block tracks for how long an object is in a specified zone.


Click the add icon in Workflows to add a block, then search for "Time in Zone":


A configuration panel will open on the right side of your screen from which you can configure your red zone. Click the "Set Polygon Zone" button in the configuration panel:


A window will appear in which you can define a red zone.


For[red zone monitoring](https://blog.roboflow.com/real-time-zone-monitoring-with-computer-vision/) to work, your camera should be at a static angle. Upload a frame from your camera feed – or a video, as the Workflows application will automatically take the first frame.


Then, click on the frame in Workflows to draw a zone. When you have drawn your final point, press Enter to create a complete polygon:


Above, we have drawn a polygon with four edges that maps to a red zone.


## Step #5: Add Annotators


Right now, our Workflow can track objects in a zone. But, there is no visual output: the results from our Workflow will be returned as JSON. While JSON is ideal for integration into another system, a visual will help us confirm our system is working.


To visualise the results of our[red zone monitoring](https://blog.roboflow.com/real-time-zone-monitoring-with-computer-vision/) system, we can use the annotation features in Workflows.


Add three annotators to your Workflow:


1. Bounding Box Visualization
2. Polygon Zone Visualization
3. Label Visualization


When you set up the Polygon Zone Visualization, copy the coordinates from the Polygon Zone you set in the Time in Zone block:


Paste them into the Zone field in the Polygon Zone Visualization:


When you set up your Label Visualization, set the Text to display as` Time in Zone` :


Our Workflow will now return a video feed showing the bounding boxes from our model – the location of people and vehicles – as well as annotations that show our polygon zone and the amount of time each object spends in the zone.


## Step #6: Test the Workflow


To run your Workflow, you will need either:


- A Dedicated Deployment, or;
- Roboflow Inference, our open source computer vision inference server, running on your local machine.


You can learn how to set up a dedicated deployment in our[Dedicated Deployment guide](https://docs.roboflow.com/deploy/dedicated-deployments/how-to-create-a-dedicated-deployment-roboflow-app?ref=blog.roboflow.com) . If you want to run your Workflow on your own hardware, follow the instructions in the "Deploy" button in the Roboflow app.


For this guide, let's run the Workflow on our own hardware. Click the "Deploy" button. Then, run the Inference installation command on your computer:


```text
pip install inference
```


Next, copy the code snippet from the Deploy window and paste it into a new Python file. The code snippet will look like this:


```text
# Import the InferencePipeline object
from inference import InferencePipeline
import cv2


def my_sink(result, video_frame):
if result.get("label_visualization"): # Display an image from the workflow response
cv2.imshow("Workflow Image", result["label_visualization"].numpy_image)
cv2.waitKey(1)
print(result) # do something with the predictions of each frame


# initialize a pipeline object
pipeline = InferencePipeline.init_with_workflow(
api_key="your-api-key",
workspace_name="your-workspace",
workflow_id="custom-workflow",
video_reference=0, # Path to video, device id (int, usually 0 for built in webcams), or RTSP stream url
max_fps=30,
on_prediction=my_sink
)
pipeline.start() #start the pipeline
pipeline.join() #wait for the pipeline thread to finish


```


The` video_reference` is the input for your Workflow. It can be a video file, a webcam ID (which is usually` 0` for your default device webcam), or an RTSP stream URL.


💡


The default code snippet looks for an image called` output_image` . Replace this value with` label_visualization` , which is the visualization that our Workflow returns.


Now we are ready to run our Workflow.


When you first run the Workflow, the Workflow configuration and any model weights used in the Workflow will be downloaded to your computer. This may take several seconds. Then, you should see the output of your Workflow, like this:


0:00


/ 0:12


Our system tracks the time vehicles and people spend in the designated zone.


## Conclusion


By following these steps, you can create a robust red zone detection model that enhances worker safety by identifying dangerous areas.


This guide has walked you through the process of setting up a model using Roboflow, defining red zones, and integrating the detection system with real-time video feeds.


We built our monitoring system in[Roboflow Workflows](https://docs.roboflow.com/workflows/what-is-workflows?ref=blog.roboflow.com) , a web-based appliaction builder for computer vision, then deployed it on our own hardware with Inference.


You can integrate this Workflow with your own systems. For example, you could log entry and exit times to a CSV file. To learn more about the possibilities of Workflows, check out our[Workflows guide](https://blog.roboflow.com/workflows/) .


### **Cite this Post**


Use the following entry to cite this post in your research:


*[James Gallagher](https://blog.roboflow.com/author/james/) . (Apr 10, 2025). How to Monitor Red Zones with Computer Vision. Roboflow Blog: https://blog.roboflow.com/redzone-monitoring-computer-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


James Gallagher


James is a technical writer at Roboflow, with experience writing documentation on how to train and use state-of-the-art computer vision models.


[View more posts](https://blog.roboflow.com/author/james/)


### Topics


- [Case Studies](https://blog.roboflow.com/tag/case-studies/)
- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Labeling](https://blog.roboflow.com/tag/labeling/)
- [Model Training](https://blog.roboflow.com/tag/model-training/)
- [Model Deployment](https://blog.roboflow.com/tag/model-deployment/)

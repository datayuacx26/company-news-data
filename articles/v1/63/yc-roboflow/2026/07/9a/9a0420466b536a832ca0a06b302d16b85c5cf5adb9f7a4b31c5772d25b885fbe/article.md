---
schema_version: "1.0.0"
document_id: "9a0420466b536a832ca0a06b302d16b85c5cf5adb9f7a4b31c5772d25b885fbe"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/predictive-maintenance-with-vision/"
published_at: "2026-07-07T00:07:38+00:00"
first_seen_at: "2026-07-22T12:07:01.391993+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:7435469578b6a5976fff0d7733bbef2bd7282b968f0012ca8a9e0c2ddd476bf1"
---

# Predictive Maintenance with Vision AI

[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/)


Published Jul 7, 2026 • 10 min read


SUMMARY


**Automate predictive maintenance with vision AI. Combine RF-DETR and Gemini 2.5 Pro in Roboflow to detect equipment defects before failure.**


Predictive maintenance with vision uses cameras and trained computer vision models to catch the visible early-warning signs of equipment failure, surface defects, wear, corrosion, and contamination, before they escalate into unplanned downtime. Instead of waiting for a component to fail or relying on periodic manual spot checks, a vision model continuously watches for the conditions that precede failure and flags them while there is still time to act.


Bearings are a textbook case. They are critical components in rotating machinery such as electric motors, pumps, conveyors, compressors, and industrial fans, and even minor surface defects can increase friction, vibration, and wear, eventually leading to equipment failure, costly downtime, and unplanned maintenance.


Industry studies[estimate](https://reliamag.com/guides/bearing-failure-cause-statistics/?ref=blog.roboflow.com) that 36% of premature bearing failures are caused by lubrication issues, 34% by fatigue, 16% by improper mounting and handling, and 14% by contamination, which means many early failures stem from preventable conditions a camera can catch. Detecting visible defects before they propagate lets maintenance teams identify deteriorating bearings and schedule repairs before unexpected failure occurs.


In this tutorial, we build an automated bearing inspection system, a concrete example of predictive maintenance with vision, using Roboflow. We train an[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) model to classify bearing conditions from inspection images and integrate it into[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) .


The resulting workflow combines computer vision with[Gemini 2.5 Pro](https://playground.roboflow.com/models/google/gemini-2-5-pro?ref=blog.roboflow.com) to transform visual inspections into actionable maintenance insights.[Here's the workflow we'll build.](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiUGw3N2Z4WFlieEZZV0dTUDJWaEEiLCJ3b3Jrc3BhY2VJZCI6Im5JRk5DOGRjbU5OOXZ4d29ybWpoWTdCNjdQZTIiLCJ1c2VySWQiOiJuSUZOQzhkY21OTjl2eHdvcm1qaFk3QjY3UGUyIiwiaWF0IjoxNzgyOTE1MTAzfQ.FOO8pEgICneEQrrRuXUKo6P6bKCNxKPoLUsaQ1-wLH4?ref=blog.roboflow.com)


## What Is Predictive Maintenance with Vision?


Maintenance strategies fall on a spectrum. Reactive maintenance fixes equipment after it breaks, which is the most expensive option because failure is unplanned. Preventive maintenance services equipment on a fixed schedule whether it needs it or not, which wastes labor and parts. Predictive maintenance acts on the actual observed condition of a machine, servicing it only when the data shows it is starting to degrade. It is the most efficient of the three because it catches problems early without over-servicing.


Predictive maintenance with vision is the approach that uses cameras and computer vision models as the sensing layer for that condition data. Many failure modes are visible before they are catastrophic: surface wear, corrosion, cracks, contamination, misalignment, leaks, and loose or missing components all show up in an image long before the machine stops. A trained vision model inspects those components continuously and objectively, flagging the early-warning signs a periodic manual check would miss.


### Where Vision-Based Predictive Maintenance Is Used


The same detect and assess pattern extends well beyond bearings:


- **Corrosion and rust** on structures, tanks, pipework, and fasteners.
- **Belt and roller wear** or misalignment on conveyors and drive systems.
- **Leaks and lubrication issues** around seals, joints, and gearboxes.
- **Surface cracks and fatigue** on castings, welds, and rotating parts.
- **Thermal anomalies** using infrared cameras to catch overheating bearings, motors, and electrical connections.
- **Loose or missing components** such as bolts, guards, and pins.


In each case, the value is the same: catch the visible condition early, quantify how serious it is, and schedule the fix before an unplanned failure takes the line down. The bearing inspection system below is a concrete, buildable version of that pattern.


### Step 1: Prepare the Dataset


For this tutorial, we use the bearing defect[dataset](https://universe.roboflow.com/devdutts-workspace/bearing-defect-qc?ref=blog.roboflow.com) from[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) . The dataset contains over 1,100 bearing images annotated for object detection, enabling the model to identify different bearing conditions during visual inspection.


The dataset contains four primary inspection classes:


- good
- scratch
- rust
- grease


The good class represents healthy bearings with no visible defects, while the remaining classes correspond to common bearing conditions encountered during quality inspection. The scratch class identifies surface scratches caused by wear, contamination, or improper handling. The rust class highlights corrosion developing on the bearing surface, which may indicate moisture exposure or inadequate storage conditions. The grease class identifies excessive or visible grease around the bearing that may indicate lubrication-related conditions requiring inspection.


Here are examples of the detection targets:


To begin, fork the dataset into your Roboflow workspace. Then navigate to the Train tab and select Custom Training. From the available architectures, choose RF-DETR and set the model size to Small.


Once the model architecture is selected, generate a new dataset version before starting training. Configure a 70/15/15 split for training, validation, and testing.


Enable the following preprocessing steps:


- Auto-orientation
- Resize (512 × 512)


Next, configure the dataset augmentations to improve the model's ability to generalize to unseen inspection conditions. For this tutorial, enable the following augmentations:


- Horizontal and vertical flip
- Random rotation (±15°)
- Random crop (0–20%)
- Random noise
- Motion blur


These preprocessing steps normalize images captured under different conditions while providing a consistent input size for RF-DETR training.


### Step 2: Train the RF-DETR Model


Once training begins, RF-DETR learns to detect bearings and classify their visual condition directly from the annotated examples. Rather than simply determining whether an image contains a defective bearing, the model locates each bearing using a bounding box and assigns one of the predefined condition labels, such as good, scratch, rust, or grease.


Unlike defect localization datasets that annotate the precise location of damage, this dataset labels the entire bearing according to its overall condition. As a result, the model predicts the bearing's health state rather than identifying the exact location of individual defects.


This approach is still valuable for predictive maintenance because it enables rapid screening of bearings during visual inspections. Maintenance personnel can quickly distinguish healthy bearings from those exhibiting signs of corrosion, surface scratches, or excessive grease, allowing potentially faulty components to be flagged for further inspection or replacement.


After training completes, Roboflow provides evaluation metrics that measure how accurately the model classifies each bearing condition. These metrics help determine whether the model is ready for deployment within Roboflow Workflows, where the predictions can be further analyzed by Gemini 2.5 Pro to generate maintenance observations, explain the likely causes of the detected condition, estimate its severity, and recommend appropriate maintenance actions.


### Step 3: Evaluate Metrics


Once training completes, review the model's performance on the test set to evaluate its ability to classify different bearing conditions. Our RF-DETR Small model achieved excellent results across all evaluation metrics.


**Image by author**


The model achieved 99.3% mAP@50, 99.0% precision, 98.2% recall, and an F1 score of 98.6%. These results indicate that the model can reliably distinguish between good, scratch, rust, and grease-bearing conditions across the test dataset.


The high precision score means that most predicted bearing conditions are correct, minimizing false alarms during inspection. Likewise, the strong recall ensures that defective bearings are rarely missed, making the model well-suited as the first stage of an automated predictive maintenance workflow.


Because the dataset was collected under relatively consistent imaging conditions, real-world performance may vary depending on lighting, camera placement, and bearing types.


These metrics evaluate only the RF-DETR detection model. In the next step, we will integrate Gemini 2.5 Pro to analyze the detected bearing condition, generate maintenance observations, explain the likely causes of the detected defect, estimate its severity, and recommend appropriate maintenance actions.


### Step 4: Deploy to Workflows


After training, deploy the model in Roboflow Workflows to build the automated bearing inspection pipeline. The workflow accepts an input image, runs the trained RF-DETR model to classify the bearing condition, visualizes the prediction using bounding boxes and class labels, sends the annotated image to Gemini 2.5 Pro for maintenance analysis, overlays the generated inspection report onto the final image, and records the inspection using Roboflow Vision Events.


To create the workflow, open your trained model and click Deploy Model. In the deployment dialog, select Customize With Logic. This opens the Workflow editor with your trained RF-DETR model already added, allowing you to build the remainder of the inspection pipeline.


### Step 5: Configure the Detection Pipeline


Start with the trained RF-DETR model and connect it to a Bounding Box Visualization block, followed by a Label Visualization block. The model classifies each detected bearing as good, scratch, rust, or grease, while the visualization blocks draw a bounding box and display the predicted class label. This produces a clean, easy-to-interpret visualization and provides Gemini with the same annotated image that users see in the final output.


### Step 6: Configure the Gemini Inspection


Add a Google Gemini block after the Label Visualization block and configure it with the following settings:


- Image: label_visualization.image
- Model: Gemini 2.5 Pro
- Task Type: Open Prompt


Use the following prompt:


```text
You are an industrial predictive maintenance specialist.


Analyze the detected bearing condition together with the inspection image. The image includes RF-DETR bounding boxes and class labels for one of these bearing-condition classes: good, scratch, rust, or grease.


Base your analysis only on the visible evidence in the image. Do not infer defects that are not visually present.


For each section below, write exactly one sentence with a maximum of 12 words.


Return the response using the following format:


Detected Condition:


<State the detected bearing condition.>


Observation:


<Briefly describe the visible bearing condition.>


Possible Causes:


<State the most likely cause of the detected condition.>


Severity:


<Low, Moderate, or High, followed by a brief justification.>


Recommended Action:


<Recommend the next maintenance step.>
```


Gemini receives the annotated image instead of the original image, allowing it to focus on the detected bearing condition and its visual appearance. Rather than acting as an object detector, Gemini interprets the model prediction, generates maintenance observations, estimates the apparent severity, and recommends the next maintenance action. This combination leverages RF-DETR for bearing condition classification and Gemini for maintenance reasoning.


Next, add a Text Display block. Use the labeled image as the base image and the Gemini response as the overlay text. Position the text in the bottom-left corner using white text on a semi-transparent black background. This produces a clean output that combines the model prediction with a concise maintenance assessment.


Finally, add a Roboflow Vision Events block to record each inspection as a structured vision event. Configure it to log the original input image, the annotated output image, the RF-DETR predictions, and custom workflow metadata. This provides a persistent record of each inspection for auditing, traceability, and future maintenance analysis.


The completed workflow returns a single annotated image containing the detected bearing condition together with Gemini's maintenance assessment.


### Step 7: Test the Workflow


Click Run in the top-right corner of the Workflow editor and upload a bearing image. The final output contains:


- The detected bearing condition (good, scratch, rust, or grease)
- Bounding box and class label
- A Gemini-generated maintenance assessment, including observations, possible causes, severity, and recommended actions


The combination of RF-DETR and Gemini 2.5 Pro enables rapid visual inspection while transforming model predictions into actionable maintenance insights, making the workflow well-suited for predictive maintenance applications.


### Extending the Solution


This workflow can be extended beyond standalone image inspection by integrating it with existing industrial systems. For example, it can connect to a Manufacturing Execution System (MES) to automatically record inspection results for each production batch or to a Computerized Maintenance Management System (CMMS) to create maintenance work orders whenever a defective bearing is detected.


It can also be integrated with Enterprise Resource Planning (ERP) systems to associate inspection results with specific assets, serial numbers, or production orders. In addition, Roboflow Vision Events provide a structured inspection history that can be used for auditing, traceability, and trend analysis.


For even greater reliability, the workflow can be combined with condition-monitoring data such as vibration, temperature, or acoustic measurements. While RF-DETR identifies the bearing condition and Gemini generates maintenance recommendations, these additional sensor inputs can help validate the diagnosis and improve maintenance decision-making. Together, they create a more comprehensive predictive maintenance solution that reduces unexpected equipment failures and downtime.


## Predictive Maintenance with Vision AI using Roboflow Agent


If you'd rather not add each block by hand, use[Roboflow Agent](https://app.roboflow.com/solutions/chat/new?ref=blog.roboflow.com) . Instead of configuring blocks one at a time, you describe the pipeline you want in plain text and the Agent builds it for you. Here's an example:


0:00


/ 1:04


## Predictive Maintenance with Vision Conclusion


In this tutorial, we built an end-to-end predictive maintenance workflow using RF-DETR, Roboflow Workflows, and Gemini 2.5 Pro. The solution automatically classifies bearing conditions, generates AI-assisted maintenance assessments, and records each inspection as a structured event.


By combining computer vision with multimodal reasoning, the workflow transforms visual inspections into actionable maintenance insights, providing a practical foundation for building intelligent industrial inspection systems that can be further integrated into real-world maintenance and manufacturing environments.


**Further reading**


- [Defect Inspection AI: Automating Quality Control](https://blog.roboflow.com/defect-inspection/)
- [How to Train RF-DETR on a Custom Dataset](https://blog.roboflow.com/train-rf-detr-on-a-custom-dataset/)
- [How to Detect Small Defects with Computer Vision](https://blog.roboflow.com/detect-subtle-defects/)


## Predictive Maintenance with Vision FAQ


### **How does computer vision enable predictive maintenance?**


A camera captures images or video of a component, a trained model classifies its condition and locates any defect, and downstream logic assesses severity and triggers an action such as an alert or a work order. Because a vision model inspects every unit continuously and objectively, it catches gradual wear that periodic manual checks miss and produces consistent data for maintenance planning.


### What is the difference between predictive, preventive, and reactive maintenance?


Reactive maintenance fixes equipment after it fails. Preventive maintenance services it on a fixed schedule regardless of condition. Predictive maintenance uses real condition data, including visual inspection, to service equipment only when it shows early signs of degrading, which reduces both unplanned downtime and unnecessary servicing.


### What equipment problems can vision-based predictive maintenance detect?


Any failure mode with a visible signature: bearing scratches and rust, grease and contamination, surface cracks and fatigue, corrosion, belt and roller wear, leaks, loose or missing fasteners, and, with infrared cameras, thermal anomalies like overheating motors and electrical connections.


### Can predictive maintenance vision models run on the edge without the cloud?


Yes. Models like RF-DETR can be deployed on edge hardware such as an NVIDIA Jetson or an industrial PC and run inference locally with no internet connection, which suits plant-floor and remote-site monitoring where continuous inspection and low latency matter.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/) . (Jul 7, 2026). Predictive Maintenance with Vision AI. Roboflow Blog: https://blog.roboflow.com/predictive-maintenance-with-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Mostafa Ibrahim


[View more posts](https://blog.roboflow.com/author/mostafa/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [RF-DETR](https://blog.roboflow.com/tag/rf-detr/)

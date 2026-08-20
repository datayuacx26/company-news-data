---
schema_version: "1.0.0"
document_id: "b00081c4838550ec05e234daefbc3ad14eedb7ac4c439ea8828322403c703099"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/segment-anything-with-text/"
published_at: "2026-07-14T17:49:30+00:00"
first_seen_at: "2026-07-23T23:14:51.571653+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:acbd1d68a309562f70094cfab02c94c969cabae7dd4e9dff711c6725709f30aa"
---

# Segment Anything with Text Prompts Using SAM 3

[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/)


Published Jul 14, 2026 • 10 min read


Summary


**You can segment anything with text using SAM 3, which takes a short prompt like "helmet" or "safety vest" and returns pixel-level masks for every matching object, zero-shot, with no training or clicks. This tutorial builds a Roboflow Workflow that uses SAM 3 to inspect construction site PPE and generate an AI safety summary from the annotated image.**


Segment anything with text means giving a segmentation model a plain-language prompt, like "helmet" or "safety vest", and getting back pixel-accurate masks for every matching object in the image. No training data, no drawn boxes, no click prompts.[SAM 3](https://roboflow.com/segment-anything-with-concepts?ref=blog.roboflow.com) does this natively: Meta calls the capability promptable concept segmentation, and unlike earlier Segment Anything models, SAM 3 finds every instance of the concept you describe, not one object per prompt.


In this tutorial, we'll use it for construction site PPE inspection, a task where text-prompt segmentation earns its keep. Construction accounted for nearly one-fifth of all U.S. workplace deaths in 2023, according to CPWR's April 2025[Data Bulletin](https://www.cpwr.com/wp-content/uploads/DataBulletin-April2025.pdf?ref=blog.roboflow.com) , and PPE checks are one visible part of reducing that risk. But jobsite scenes change constantly: workers move, machines block the view, and helmets and vests appear small, folded, or partially hidden. The traditional approach means collecting images, labeling every class, and training a custom model before you see a single result.


With SAM 3, we skip straight to results. We'll build a[Roboflow Workflow](https://roboflow.com/workflows/build?ref=blog.roboflow.com) that segments PPE from text prompts such as person, helmet, safety vest, excavator, and traffic cone, visualizes the masks, and generates a short AI-powered safety summary from the annotated image.


## SAM 3 vs. Grounded SAM and lang-segment-anything


Text-prompt segmentation isn't new as an idea; what's new is doing it in one model. The original SAM and[SAM 2](https://blog.roboflow.com/what-is-segment-anything-2/) only accepted visual prompts (clicks, boxes, or masks), so the community bridged the gap by chaining models:[Grounding DINO](https://blog.roboflow.com/grounding-dino-zero-shot-object-detection/) converts a text prompt into bounding boxes, then SAM converts those boxes into masks. Grounded SAM and lang-segment-anything are both packages of that two-model pipeline.


The chained approach works, but it inherits two models' worth of weaknesses. Errors compound: if Grounding DINO misses the box, SAM never sees the object. It returns one region per detected box rather than reasoning about the concept across the scene. And running it means hosting two models, aligning their dependencies, and paying two inference costs per image.


SAM 3 collapses the pipeline into a single model trained for promptable concept segmentation. You provide a short noun phrase (or an image exemplar), and it detects and segments every instance of that concept in the image or video directly. For this tutorial that difference is practical: one prompt such as helmet masks every helmet in a crowded jobsite scene in one pass, and running it in a Roboflow Workflow means there's no pipeline to host at all.


## Run SAM 3 with a Text Prompt in Python


If you want the raw capability before building the full workflow, Roboflow's serverless API runs SAM 3 on hot GPU instances, so there's nothing to download or host. One request with a text prompt returns segmentation masks:


```text
import requests


response = requests.post(
"https://serverless.roboflow.com/sam3/concept_segment?api_key=<YOUR_ROBOFLOW_API_KEY>",
headers={"Content-Type": "application/json"},
json={
"format": "polygon",
"image": {
"type": "url",
"value": "https://example.com/construction-site.jpg"
},
"prompts": [
{"text": "helmet"},
{"text": "safety vest"},
{"text": "person"}
]
}
)


for prompt_result in response.json()["prompt_results"]:
print(prompt_result["echo"]["text"], len(prompt_result["predictions"]), "matches")
```


Each prompt returns every matching instance with its mask polygon and confidence. You can also run SAM 3 locally with the` inference` package (` pip install inference-gpu\[sam3\]` ) if you have your own GPU; see the[SAM 3 Inference docs](https://inference.roboflow.com/foundation/sam3/?ref=blog.roboflow.com) for both paths. The rest of this tutorial builds the same capability into a no-code Workflow with visualization and reporting on top.


## Segment Anything with Text Prompts Using SAM 3


### Step 1: Create our Dataset


For this tutorial, we will use the Construction Site Safety Computer Vision[dataset](https://universe.roboflow.com/roboflow-universe-projects/construction-site-safety?ref=blog.roboflow.com) from[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) as the source of test images. The dataset contains construction site scenes with workers, safety gear, vehicles, and jobsite equipment. It includes 717 images and classes such as Person, Helmet, Safety Vest, Excavator, vehicle, Ladder, and Gloves.


We are not going to train a model on this dataset. Instead, we will use its images to test how well SAM 3 can segment construction safety concepts from text prompts. This keeps the tutorial focused on Segment Anything with Text, rather than turning it into a traditional object detection training workflow.


The dataset is a good fit because the images contain common PPE and construction objects that can be described with simple prompts. For example:


```text
person
helmet
safety vest
excavator
vehicle
ladder
gloves
```


These prompts give SAM 3 clear concepts to search for in each image. A prompt like person can identify visible workers, while a helmet and safety vest can highlight PPE items. Equipment prompts, such as an excavator or a vehicle, can also be added when the scene contains machinery.


To begin, open the dataset in Roboflow Universe and use a few example images from the Images tab. These images will be passed into the Roboflow Workflow as test inputs for SAM 3-based segmentation.


Here are examples from the dataset:


### Step 2: Create a Roboflow Workflow


Next, create a new Workflow in Roboflow. This Workflow will take a construction site image, run SAM 3 with PPE-related text prompts, and return an annotated result.[Here's the workflow we'll build](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiTXpOZlpybXBDWkxRZ3cyaFY0dmciLCJ3b3Jrc3BhY2VJZCI6Im5JRk5DOGRjbU5OOXZ4d29ybWpoWTdCNjdQZTIiLCJ1c2VySWQiOiJuSUZOQzhkY21OTjl2eHdvcm1qaFk3QjY3UGUyIiwiaWF0IjoxNzgzMzY2NDkxfQ.0CAo08csnz0oe7NT9C04TIyg-130uUwy1QeoeVH2SEA?ref=blog.roboflow.com) .


The workflow has three main parts. First, SAM 3 segments objects such as workers, helmets, and safety vests from text prompts. Then, visualization blocks draw masks, boxes, and labels over the matching regions. Finally, Gemini reads the annotated image and generates a short PPE safety summary, which is displayed on the final output.


The workflow follows this structure:


Start by opening Roboflow Workflows and creating a new workflow. Add an image input block as the first step. This input will receive the construction site image that you want to inspect.


### Step 3: Configure SAM 3 Text Prompts


The SAM 3 block is the core of this workflow. Instead of drawing boxes, placing clicks, or manually creating masks, we provide text prompts that describe the objects we want to segment.


For the first version of the workflow, use a small prompt set:


- person
- helmet
- safety vest


These prompts cover the main PPE inspection targets. Person helps identify visible workers, while helmet and safety vest focus the workflow on the most common protective equipment visible in construction site images.


SAM 3 uses these prompts to search the image for matching concepts and return segmentation masks. Each mask represents the shape of an object that matched one of the prompts. This is different from a standard object detector, which usually returns only bounding boxes.


After the basic workflow is working, you can expand the prompt list with additional construction-related objects:


- helmet
- person
- safety vest
- vehicle
- gloves


Prompt wording matters. For example, helmet, hard hat, and construction helmet may produce slightly different results depending on the image. Start with simple prompts, test the output, and then adjust the wording based on what SAM 3 segments correctly.


### Step 4: Visualize the Segmentation Results


SAM 3 returns segmentation predictions, but raw predictions are hard to review on their own. To make the output useful, add visualization blocks after SAM 3.


The first visualization step draws segmentation masks over the objects found by SAM 3. These masks make it easier to see the exact regions that matched each prompt. This is especially useful for PPE because helmets and vests can be small, partially hidden, or close to other objects in the scene.


Next, add bounding boxes and labels. The boxes give a quick view of where each object is located, while the labels show which prompt matched each segmented region.


Masks are the most important part of this step. A bounding box can show that a helmet is somewhere near a worker’s head, but a mask shows the actual helmet shape. That extra detail is useful when the scene is crowded or when PPE is only partly visible.


### Step 5: Generate a PPE Safety Summary


After the image has been annotated, send it to a vision-language block such as Google Gemini. The goal is not to use Gemini as the detector. SAM 3 has already done the segmentation. Gemini receives the annotated image and writes a short safety summary based on what is visible.


Use this prompt:


```text
Inspect the annotated construction site image.
Write one short PPE safety summary.
Mention whether visible workers appear to have helmets and safety vests.
Keep the response under 20 words.
```


This keeps the response short enough to display on the final image. It also keeps Gemini focused on the PPE inspection task instead of producing a long description of the full scene.


The summary should be treated as an inspection aid, not a final safety decision. The useful part is that the workflow produces both visual evidence and a short written observation in one output.


### Step 6: Display the Final Output


The final step is to overlay the PPE safety summary on the annotated image. Add a Text Display block and use the labeled image as the base image. Then use the Gemini response as the text that appears on the output.


This creates a single result that is easy to review. The image shows the SAM 3 segmentation masks, object labels, and the short safety summary together.


After that, add a Roboflow Vision Events block to log each PPE review as a safety event. Use the original input image as the input image, the Text Display output as the output image, and the SAM 3 predictions as the event predictions. Set the event type to Safety Alert and use a PPE-related alert name, such as ppe_safety_review.


### Step 7: Test the Workflow


After building the workflow, test it on several images from the construction safety dataset. Do not rely on only one clean example. Text-prompt segmentation can behave differently depending on distance, lighting, occlusion, and how clearly the PPE appears in the frame.


Results using the current workflow:


For each test image, review the input image, the prompts used, the SAM 3 segmentation output, and the generated safety summary. The goal is to understand where the workflow performs well and where prompt wording needs adjustment.


This step is important because SAM 3 is being used zero-shot. We are not training it on the dataset, so the test images help us evaluate how well the text prompts transfer to real construction scenes.


By the end of testing, you should have a clear sense of which prompts work best for this dataset and which scene types are more difficult for the workflow.


## Extending the Workflow


Once the workflow is working on a few examples, the next step is to look at the failure cases. Some images will be easy: workers are close to the camera, PPE is visible, and the prompts return clean masks. Other images will be harder because helmets are small, vests are partly hidden, or equipment overlaps with workers.


Use those cases to refine the workflow. Try alternate wording when a class is missed. For example, if helmet is inconsistent, compare it with hard hat or construction helmet. If safety vest misses examples, test high visibility vest or reflective vest. Keep the wording that gives the cleanest masks across several images, not just one result.


This workflow is also useful before building a trained model. SAM 3 can generate first-pass masks for many construction images. After reviewing and correcting those masks in Roboflow Annotate, the cleaned labels can become a training dataset for a dedicated PPE detection or segmentation model.


That dedicated model may be a better fit when you need consistent results for a known camera position, a specific jobsite, or a fixed list of PPE classes. A trained model is usually the better next step when the system needs repeatable performance at a production scale.


## Segment Anything with Text with Roboflow Agent


If you'd rather not assemble the workflow yourself, describe it to Roboflow Agent instead: a prompt like "segment helmets and safety vests in construction images with SAM 3 and summarize PPE compliance" and it builds, configures, and tests the pipeline for you. Text prompts drive the segmentation and plain English drives the build, so the entire system, from masks to safety summary, comes from describing what you want.


0:00


/ 0:39


**What is**[promptable concept segmentation](https://blog.roboflow.com/what-is-promptable-concept-segmentation-pcs/) **?**
It's SAM 3's core capability: you describe a concept with a short noun phrase, like "helmet" or "red car", and the model detects, segments, and tracks all instances of that concept in an image or video, rather than segmenting one object per prompt.


**Do I need to train SAM 3 before using it?**
No. SAM 3 works zero-shot from text prompts with no training data or fine-tuning. Training a dedicated model becomes worthwhile later, when you need consistent, repeatable performance on a fixed class list, and SAM 3 masks can bootstrap that dataset.


**How is text-prompt segmentation different from object detection?**
An object detector returns bounding boxes for classes it was trained on. Text-prompt segmentation returns pixel-level masks for anything you can describe, without training. Masks capture exact object shapes, which matters when objects are small, overlapping, or partially hidden.


## Segment Anything with Text Conclusion


In this tutorial, we built a prompt-based PPE segmentation workflow using SAM 3 in Roboflow. Instead of training a custom model, we used text prompts such as person, helmet, and safety vest to segment construction site safety equipment directly from images.


The workflow shows how Segment Anything with Text can support fast visual prototyping,[PPE inspection](https://blog.roboflow.com/ppe-detection/) , and dataset pre-labeling. For production use, the same process can be extended by reviewing SAM 3-generated masks, creating a labeled dataset, and training a dedicated PPE model for deployment.


**Further reading:**


- [SAM 3: Segment Anything with Concepts](https://roboflow.com/segment-anything-with-concepts?ref=blog.roboflow.com)
- [What is Segment Anything 2 (SAM 2)?](https://blog.roboflow.com/what-is-segment-anything-2/)
- [Grounding DINO: Zero-Shot Object Detection](https://blog.roboflow.com/grounding-dino-zero-shot-object-detection/)
- [Auto Label: Fast, Automatic Vision Labeling](https://roboflow.com/auto-label?ref=blog.roboflow.com)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/) . (Jul 14, 2026). Segment Anything with Text. Roboflow Blog: https://blog.roboflow.com/segment-anything-with-text/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Mostafa Ibrahim


[View more posts](https://blog.roboflow.com/author/mostafa/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)

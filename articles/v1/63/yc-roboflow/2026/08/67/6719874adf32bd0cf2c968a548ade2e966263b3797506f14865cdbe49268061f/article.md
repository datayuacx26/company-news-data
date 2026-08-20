---
schema_version: "1.0.0"
document_id: "6719874adf32bd0cf2c968a548ade2e966263b3797506f14865cdbe49268061f"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/how-to-detect-small-objects-in-drone-imagery/"
published_at: "2026-08-04T20:38:56+00:00"
first_seen_at: "2026-08-04T20:49:17.876217+00:00"
fetched_at: "2026-08-04T21:21:09.879682+00:00"
content_hash: "sha256:be829d94ab494d1d1e612465f1f7cb1cabcba8035c1e1848fe6f8d03f91fa4a3"
---

# How to Detect Small Objects in Drone Imagery

[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/)


Published Aug 4, 2026 • 10 min read


SUMMARY


**Small objects in drone imagery are detectable with a custom-trained model: this tutorial trains RF-DETR on roughly 7,000 aerial images with 120,000+ person annotations to find nine classes of people and vehicles that occupy only a few pixels each. The finished Roboflow Workflow draws the detections, counts them by class in Python, and has Gemini 2.5 Pro add a one-sentence visual inspection of the scene.**


Drones capture large areas from above, supporting applications such as traffic monitoring, disaster assessment, infrastructure inspection, wildlife monitoring, and public-space analysis. The Federal Aviation Administration reports more than 837,000 registered drones and over 481,000 certificated remote pilots in the United States, highlighting the growing need to analyze aerial imagery efficiently.


Computer vision can turn these images into structured information by locating people, vehicles, and other objects. However, aerial detection is challenging, which is why detecting small objects in drone imagery is its own discipline. Higher flight altitudes make objects smaller, while resizing, crowded scenes, occlusion, shadows, motion blur, and similar vehicle shapes can further reduce detection reliability.


Successful small-object detection, therefore, depends not only on the model but also on annotation consistency, image quality, preprocessing, training resolution, and the inference workflow.


In this tutorial, we train[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) to detect people and vehicles that appear small in aerial imagery. We then deploy it in a[Roboflow workflow](https://roboflow.com/workflows/build?ref=blog.roboflow.com) that detects objects, counts them by class, and uses[Gemini 2.5 Pro](https://playground.roboflow.com/models/google/gemini-2-5-pro?ref=blog.roboflow.com) to generate a concise visual inspection.


## Techniques for Detecting Small Objects in Drone Imagery


Small-object detection comes down to four levers, and every working system uses some combination of them.


Training resolution comes first. A person in a drone frame might occupy 15 pixels before preprocessing; resize that frame to a small training resolution and the person becomes 4 pixels the model has almost nothing to learn from. Higher training resolution preserves the pixels small objects live in, at the cost of memory and training time. The right setting depends on your altitude, your camera, and the smallest object you need to catch.


Inference-time slicing is often the biggest single win. Instead of shrinking a high-resolution frame to fit the model, split it into overlapping tiles, run detection on each tile at full detail, and merge the results. This is the[SAHI](https://blog.roboflow.com/how-to-use-sahi-to-detect-small-objects/) technique, available in Roboflow Workflows as the[Image Slicer block](https://inference.roboflow.com/workflows/blocks/image_slicer/?ref=blog.roboflow.com) . It costs multiple inference passes per frame, which matters for real-time video, and it recovers objects that resizing would erase.


Model architecture is the third lever. Anchor-based detectors match objects against preset box sizes, and very small objects fit those presets poorly. Transformer detectors like[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) predict objects directly, without anchors or a separate NMS stage, which helps in exactly the conditions drone imagery creates: dense scenes, overlapping objects, and extreme scale variation within one frame.


The fourth lever, annotation quality, is the cheapest to improve and the easiest to get wrong. A bounding box that is loose by three pixels doesn't matter on a truck; on a 12-pixel pedestrian it's a 25% error the model learns as truth. Tight boxes on small objects, consistent rules for occluded and partial objects, and deliberate handling of ambiguous regions (this dataset's "ignored regions" class exists for that reason) set the ceiling on everything downstream.


The tutorial below applies these levers with a deliberate tradeoff: RF-DETR for the model, careful class cleanup for annotation quality, and a cost-conscious 512 × 512 training resolution, with slicing covered in Extending the Workflow as the production upgrade.


## How to Detect Small Objects in Drone Imagery


The project contains two connected layers.


### Layer 1: Object detection


An RF-DETR model identifies individual objects in the aerial image. It detects nine classes. The class structure follows the[VisDrone benchmark](https://github.com/VisDrone/VisDrone-Dataset?ref=blog.roboflow.com) :


- awning-tricycle
- bicycle
- bus
- car
- motor
- person
- tricycle
- truck
- van


For each object, the model returns a bounding box and class label. Roboflow Workflow visualization blocks then draw the detections over the image.


### Layer 2: Scene analysis


A Custom Python block receives the predictions and counts objects by class. It also calculates the total number of people, the total number of vehicles, and the most common vehicle class.


Gemini then reviews the annotated image and produces a concise visual inspection. Python remains responsible for the numerical results, while Gemini describes visible patterns such as traffic distribution, clustering, occlusion, and image-quality issues.


### Step 1: Prepare the Aerial Dataset


We use the Aerial Person Detection Computer Vision[Dataset](https://universe.roboflow.com/aerial-person-detection/aerial-person-detection?ref=blog.roboflow.com) from[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) . It contains approximately 7,000 aerial images showing roads, intersections, buildings, pedestrians, and several vehicle types from overhead perspectives.


The dataset is suitable for small-object detection because it includes objects at different scales. People, bicycles, and motorcycles often occupy small areas, while cars, buses, trucks, and vans provide larger examples. Many images also contain crowded scenes, overlapping vehicles, shadows, vegetation, and varied camera angles.


Examples from the dataset:


Fork the dataset into your Roboflow workspace. Then open the Classes & Tags tab to review its class structure.


Before training, remove these classes:


- ignored regions
- others


The first represents ambiguous or excluded areas rather than a physical object, while the second does not describe one consistent visual category.


Next, merge people and pedestrian:


1. Open Classes & Tags.
2. Select both classes.
3. Choose the rename option.
4. Rename both to person.
5. Apply the changes.


Assigning both classes the same name merges their annotations. The resulting person class contains 120,302 annotations.


This is the final dataset:


### Step 2: Generate a Dataset Version


After modifying the classes, open the Train tab, select Custom Training, choose RF-DETR, and set the model size to Small.


Configure a 70/15/15 split for training, validation, and testing.


Enable:


- Auto-orientation
- Resize to 512 × 512


We train at 512 × 512 because this is a tutorial and that resolution keeps training fast and cheap. Be aware of what it costs: resizing a large aerial frame down to 512 pixels shrinks a pedestrian who was already small into a handful of pixels, which is the exact problem this article is about.


For production small-object work, do one or both of the following: raise the training resolution (more memory and training time, more detail preserved), or keep the model as-is and add an[Image Slicer block](https://inference.roboflow.com/workflows/blocks/image_slicer/?ref=blog.roboflow.com) before inference, which splits each high-resolution frame into overlapping tiles so small objects reach the model at full detail. The[SAHI approach](https://blog.roboflow.com/how-to-use-sahi-to-detect-small-objects/) behind that block is covered in depth in our small-object detection guide, and the Extending the Workflow section below shows where slicing fits in this pipeline.


### Step 3: Train RF-DETR


Object detection is required because the system must locate every visible person and vehicle in the aerial image. Image classification could report that a car or person is present, but it could not identify multiple instances, draw a box around each object, or show where they are located.


During training, RF-DETR learns how people and vehicles appear from above, how similar vehicle classes differ, how object appearance changes with scale, and how to place an accurate bounding box around each detected instance. It also learns to separate foreground objects from roads, buildings, shadows, and other background details.


After training, we will use the RF-DETR model in a Roboflow Workflow to detect and label people and vehicles in aerial images, then pass those detections to the counting and scene-inspection steps.


### Step 4: Deploy to Roboflow Workflows


After training the model, deploy it in Roboflow Workflows to build the aerial scene inspection pipeline.[Here's the workflow we'll build.](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiSjBHazcyYk82U1BjVGxtb2JJbW4iLCJ3b3Jrc3BhY2VJZCI6Im5JRk5DOGRjbU5OOXZ4d29ybWpoWTdCNjdQZTIiLCJ1c2VySWQiOiJuSUZOQzhkY21OTjl2eHdvcm1qaFk3QjY3UGUyIiwiaWF0IjoxNzg1Njc3ODg4fQ.0dFxIJ5X51BR1gF5MCkYQrsEKDzUlDY2PEUCwfNhtYg?ref=blog.roboflow.com)


The workflow runs the trained object detection model on an input image, draws the predicted bounding boxes and class labels, counts the detected objects, and uses Gemini to produce a short visual inspection.


To create the workflow, open the trained model and click Deploy Model. Select Customize With Logic to open the Workflow editor with the object detection model already connected.


The completed workflow follows this structure:


### Step 5: Visualize Detections and Configure the Aerial Summary


The input image first passes through the trained RF-DETR model, which returns the detected objects and their class names.


Add a Bounding Box Visualization block after the model. Connect the original image and the model predictions to this block. It draws a box around every detected person or vehicle.


Next, add a Label Visualization block. Use the image returned by the Bounding Box Visualization block and connect the same model predictions. This block adds the detected class name to each bounding box.


The labeled image now contains both the object locations and their class names.


Add a Custom Python block after the Label Visualization block. Connect the labeled image and the object detection predictions to the block.


Use the following code:


```text
def run(self, image, predictions):
classes = ['awning-tricycle','bicycle','bus','car','motor','person','tricycle','truck','van']
vehicles = ['awning-tricycle','bicycle','bus','car','motor','tricycle','truck','van']
names = list(predictions.data.get('class_name', []))
counts = {c: names.count(c) for c in classes}
people = counts['person']
vehicle_total = sum(counts[c] for c in vehicles)
common = max(vehicles, key=lambda c: counts[c]) if vehicle_total else 'none'
summary = {'counts_by_class': counts, 'total_people': people, 'total_vehicles': vehicle_total, 'most_common_vehicle_class': common}
summary_text = f"People: {people} | Vehicles: {vehicle_total} | Most common vehicle: {common}"
arr = image.numpy_image.copy()
lines = [summary_text, ', '.join([f'{k}: {v}' for k, v in counts.items()])]
y = 24
for line in lines:
cv2.rectangle(arr, (6, y-18), (min(arr.shape[1]-1, 16 + 9*len(line)), y+7), (0,0,0), -1)
cv2.putText(arr, line, (10, y), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,255), 1, cv2.LINE_AA)
y += 24
out = WorkflowImageData.copy_and_replace(origin_image_data=image, numpy_image=arr)
return {'image': out, 'summary': summary, 'summary_text': summary_text}
```


The block counts the detections for each supported class. It then calculates the total number of people, the total number of vehicles, and the most common detected vehicle class.


Python remains the source of truth for all numerical results. Gemini will inspect the annotated image in the next step, but it will not recount or modify these values.


### Step 6: Configure Gemini


Connect the image returned by the Aerial Summary block to a Google Gemini block.


**Use this prompt:**


```text
Review the annotated aerial image and write exactly one sentence of approximately 14 words.


Mention only the most important visible condition, such as clustering, congestion, occlusion, shadows, or blur.


Do not recount objects, list categories, use headings, use bullet points, or add explanations.


Return only the sentence.
```


Gemini does not calculate or verify the detection counts. Those results have already been produced by the Custom Python block. Gemini uses the annotated image only to describe the most important visible condition in the scene.


Add a Text Display block using aerial_summary.image as its base image. Connect the Gemini output to the text field.


### Step 7: Test the Workflow


Click Run and upload an aerial image containing people or vehicles.


The workflow should detect the visible objects, draw their boxes and labels, and display the numerical summary at the top of the image. Gemini’s short visual inspection should appear near the bottom.


Test the workflow with different types of aerial scenes, such as:


- A crowded parking area
- A busy road
- A sparse street
- A scene with overlapping vehicles
- An image containing shadows or blur


Check that the numerical results match the model predictions and that Gemini returns only one short sentence. Gemini should describe the scene without recounting objects or changing the Python-generated totals.


## Extending the Workflow


The workflow can be extended to process aerial video instead of individual images. A tracking block could assign persistent IDs to vehicles and people across frames, making it possible to estimate movement patterns or count objects crossing a defined area.


For large drone images, image tiling could be added before inference. Splitting a high-resolution image into smaller overlapping sections can preserve more detail for objects that would otherwise become difficult to detect after resizing.


The workflow could also store annotated results for later review or send selected outputs to another application. For example, a downstream system could save scenes with unusually high traffic or images affected by heavy occlusion.


These extensions can be combined into a broader aerial monitoring pipeline that tracks objects, processes large images, stores annotated results, and routes selected outputs to downstream applications.


## Use Roboflow Agent


Roboflow Agent can assemble this Workflow from a prompt instead of block-by-block wiring. Describe the pipeline in plain language ("run my aerial detection model, draw boxes and labels on the image, count detections by class in a Python block, then have Gemini write a one-sentence inspection of the annotated frame") and Agent generates the connected blocks for you to review in the editor. It handles the parts that are easy to miswire by hand, like feeding Gemini the annotated image rather than the original and keeping the Python counts separate from Gemini's text.


It's also useful after the build: ask it to add an Image Slicer block for higher-resolution frames, tighten the confidence threshold, or swap in a retrained model, and it edits the Workflow in place.


0:00


/ 0:56


## Detecting Small Objects in Drone Imagery


This workflow combines RF-DETR object detection, Python-based counting, and Gemini scene inspection. The model detects and labels people and vehicles, while the Custom Python block calculates totals and the most common vehicle class. Gemini then adds a short visual summary, and the workflow returns one annotated aerial image containing detections, counts, and scene observations.


**Further reading**


- [How to Use SAHI to Detect Small Objects](https://blog.roboflow.com/how-to-use-sahi-to-detect-small-objects/)
- [RF-DETR Delivers Best-in-Class Speed and Accuracy for Aerial Imagery](https://blog.roboflow.com/ai-for-aerial-imagery/)
- [Detect Small Objects with Roboflow Workflows](https://blog.roboflow.com/detect-small-objects/)
- [How to Train Computer Vision Models on Aerial Imagery](https://blog.roboflow.com/how-to-use-roboflow-with-aerial-imagery/)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Mostafa Ibrahim](https://blog.roboflow.com/author/mostafa/) . (Aug 4, 2026). How to Detect Small Objects in Drone Imagery. Roboflow Blog: https://blog.roboflow.com/how-to-detect-small-objects-in-drone-imagery/*


### Written by


Mostafa Ibrahim


[View more posts](https://blog.roboflow.com/author/mostafa/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [RF-DETR](https://blog.roboflow.com/tag/rf-detr/)
- [Aerial Imagery](https://blog.roboflow.com/tag/aerial-imagery/)

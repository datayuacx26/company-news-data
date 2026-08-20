---
schema_version: "1.0.0"
document_id: "52244e65c362b3c64db83a0f33b884b90e1b5dcc8ac70c1ad377e52e19839351"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/zero-shot-vs-fine-tuned-models/"
published_at: "2026-08-04T14:54:15+00:00"
first_seen_at: "2026-08-04T16:25:22.407914+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:2a2158a28e24ec7104476b872ac927dbd0a9e9c0b63d9e89756f5461b8af6628"
---

# Zero-Shot vs. Fine-Tuned Models: When to Train Your Own

[Dikshant Shah](https://blog.roboflow.com/author/dikshant/)


Published Aug 4, 2026 • 27 min read


SUMMARY


**Zero-shot models enable rapid prototyping without labeled data or training, while fine-tuned models provide higher accuracy, faster inference, and production reliability through additional labeling and training. This guide shows how to combine both approaches using Roboflow.**


Zero-shot models can detect and segment objects without requiring you to collect or label training data, making them ideal for quickly validating computer vision use cases.


Fine-tuned models require labeled training data, but they can deliver higher accuracy, lower inference costs, faster runtimes, and more reliable performance in production.


For most production applications, the best approach is not to choose one over the other. Instead, use zero-shot models to rapidly prototype your computer vision application and generate training data, then fine-tune a specialized model for deployment.


In this guide, we will compare zero-shot and fine-tuned models and explore how to use both throughout the computer vision development lifecycle.


You will learn how to:


- Prototype a computer vision workflow using a zero-shot model
- Accelerate data labeling with zero-shot model-assisted annotation
- Determine when fine-tuning is the better approach
- Fine-tune a model for your specific use case using[Roboflow Train](https://roboflow.com/train?ref=blog.roboflow.com)
- Build a computer vision workflow around a fine-tuned model
- Deploy your fine-tuned model using[Roboflow Deploy](https://roboflow.com/deploy?ref=blog.roboflow.com)


## What Are Zero-Shot Models?


A[zero-shot model](https://playground.roboflow.com/models/feature/zero-shot-detection?ref=blog.roboflow.com) is an AI model that can perform a task or recognize objects it has never been specifically trained on, using its general knowledge and understanding instead of task-specific labeled examples.


In computer vision, this means you can ask the model to detect or segment an object without first collecting and labeling a dataset for that object.


Instead of learning only from a fixed set of labeled classes (like "cat," "person," and "car"), zero-shot models are trained on massive amounts of diverse data.


This gives them a broad understanding of visual concepts and language, allowing them to generalize to new tasks.


For example:


- You upload an image of a store shelf.
- You ask the model to detect bottles.
- Even if the model has not been trained on your specific bottle dataset, it can often identify and locate the bottles based on its general understanding of the concept.


Zero-Shot Object Segmentation with SAM 3


Zero-shot models include[SAM 3](https://blog.roboflow.com/what-is-sam3/) , Florence-2, and YOLO-World, among many others. You can experiment with these models directly in the[Roboflow Playground.](https://playground.roboflow.com/models/feature/zero-shot-detection?ref=blog.roboflow.com)


### What Do Zero-Shot Models Do Well?


Zero-shot models enable quick experimentation by allowing computer vision tasks without custom training data. Key benefits include:


- **No labeled training data required:** Start detecting or segmenting objects immediately without creating a custom dataset.
- **Rapid prototyping:** Quickly validate whether a computer vision solution is feasible before investing in data collection and model training.
- **Recognizes a wide variety of objects:** Since zero-shot models are trained on diverse data, they can often identify thousands of object categories beyond a fixed label set.
- **Lower upfront cost:** Avoid the expense and effort of annotating images until you're confident the project is worth pursuing.
- **Flexible for changing requirements:** If your application needs to detect new objects, you can simply change the prompt instead of retraining a model.
- **Useful for bootstrapping datasets:** Zero-shot predictions can generate initial annotations that humans review and correct, significantly speeding up dataset creation for future model training.


For example, in the industrial automation domain, we can use SAM 3 to detect complex hairline cracks across various components without labeling any training data.


### Where Do Zero-Shot Models Fall Short?


Zero-shot models are powerful for experimentation but have limitations when accuracy, consistency, and production performance are critical. Key limitations include:


- **Less reliable on domain-specific data:** Images from industries like manufacturing, healthcare, or agriculture may contain objects or visual patterns that general-purpose models struggle to recognize.
- **May struggle with visually similar objects:** Distinguishing between fine-grained categories, such as different product variants or machine components, can be challenging.
- **Lower accuracy than fine-tuned models:** Models trained specifically on your data typically perform better on your target objects and environment.
- **Inconsistent predictions:** Performance can vary depending on image quality, lighting, occlusion, or the wording of the prompt.
- **Higher inference cost:** Large foundation models often require more compute and memory than specialized models, making them more expensive to run at scale.
- **Not ideal for high-volume production:** Applications that require predictable accuracy, low latency, or millions of inferences typically benefit from a fine-tuned model optimized for the specific task.


For example, in the industrial automation domain, the same SAM 3 model that successfully detected hairline cracks was unable to reliably identify other defects, including bad welds, excess reinforcement, good welds, porosity, and spatter.


A fine-tuned[RF-DETR model](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) was therefore used to address these challenges and reliably detect cracks along with a broader range of welding defects.


## What Are Fine-Tuned Models?


A fine-tuned model is an AI model that starts with a pre-trained foundation model such as[YOLO26](https://playground.roboflow.com/models/ultralytics/yolo26?ref=blog.roboflow.com) , RF-DETR, or SAM 3 (acts as both zero-shot and trainable foundational) and is then trained further on a specific dataset to perform a particular task more accurately.


Instead of learning from scratch, the model builds on its existing knowledge and adapts to recognize the objects, patterns, or behaviors that matter for your application.


For example, if you want to detect American Sign Language (ASL) signs:


- Collect a dataset of images containing hands performing different ASL signs.
- Label the hands with the appropriate sign class, such as “A,” “B,” “Hello,” or “Thank You.”
- Train or fine-tune a pre-trained foundational computer vision model using this labeled dataset.
- The model learns the hand shapes, positions, movements, and visual patterns associated with each sign in your data.
- Once training is complete, the model can detect and classify ASL signs with greater accuracy and consistency than a general-purpose zero-shot model.


You can find various such fine-tuned models on[Roboflow Universe.](https://universe.roboflow.com/?ref=blog.roboflow.com)


### What Can Fine-Tuning Do for You?


Fine-tuned models typically require more upfront effort because you need labeled training data and a consistent evaluation process to measure and improve model performance.


However, this additional effort can provide several benefits, including:


- **Higher accuracy for your specific use case:** The model learns the exact objects, visual conditions, and patterns that matter to your application.
- **More consistent performance:** Training on data that reflects your real-world environment helps the model perform more reliably across different lighting conditions, camera angles, backgrounds, and object appearances.
- **Faster inference:** Fine-tuned models are often optimized for a specific task, allowing them to run faster than larger general-purpose models.
- **Suitable for edge deployment:** Smaller, specialized models can run on devices such as cameras, smartphones, embedded systems, and edge computers without requiring cloud-based inference.
- **Custom object classes:** Train the model to detect, classify, or segment the exact classes needed for your application, including specialized objects that may not be recognized reliably by zero-shot models.
- **Continuous improvement:** You can collect difficult or incorrect predictions from production, add them to your dataset, and retrain the model to improve performance over time.
- **Lower long-term inference costs:** Efficient, task-specific models may require less computing power, reducing the cost of running the application at scale.


## How to Choose Between Zero-Shot and Fine-Tuned Models?


The table below compares the two approaches across common computer vision scenarios to help you determine which option is best for your use case.


Scenario Zero-shot models Fine-tuned models


You are just getting started Yes


Validate your idea without collecting training data.


No


Not necessary yet.


You don't have labeled data Yes


Best choice. Works from text prompts or general knowledge.


No


Requires labeled training data.


Your objects are unique or custom Depends


May struggle with uncommon or highly specific objects.


Yes


Learns your exact objects from labeled examples.


You need results quickly Yes


Deploy in minutes.


No


Requires data collection and training first.


You need the highest accuracy Depends


Good for general tasks, but accuracy varies.


Yes


Best option for domain-specific objects and production use.


Your environment is consistent Depends


Generalizes broadly but is not optimized for your setup.


Yes


Optimized for your cameras, lighting, and viewpoints.


You need predictable performance Depends


Performance can vary across scenes.


Yes


More consistent results in production.


You have strict latency requirements No


Not ideal when strict low latency is required.


Yes


Best for real-time applications.


You want to minimize inference costs No


Foundation models are larger and cost more to run.


Yes


Smaller specialized models are cheaper at scale.


You need fast inference No


Usually slower because the models are larger.


Yes


Faster inference from optimized task-specific models.


Your deployment target is the edge Depends


Best suited to cloud prototyping and experimentation.


Yes


Built for edge and resource-constrained devices.


Your inference volume is high Depends


Suited to low-volume workloads.


Yes


Better for high-volume production where efficiency matters.


Your dataset changes frequently Yes


Adapts immediately by changing prompts.


Depends


May need retraining when the data changes significantly.


You are deploying at production scale Depends


Useful for prototyping or fallback detection.


Yes


Best choice for long-term production deployments.


Recommended approach Start with a zero-shot model to validate the use case and generate initial annotations. Fine-tune once you have labeled data and need higher accuracy, lower cost, and reliable production performance.


## How to Use Zero-Shot Models in Roboflow Workflows


[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) is a visual, low-code platform for building computer vision pipelines by connecting AI models, image-processing operations, and custom logic through a drag-and-drop interface.


Instead of building complex computer vision pipelines from scratch, you can assemble them visually and deploy them in just a few clicks.


The video below demonstrates the workflow we will build. We will use this workflow to detect hands as the first step toward creating an **American Sign Language (ASL) detection system.** You can try or fork this workflow[here.](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiT3BuZ1poR3dVNVdCc0pjN2FURHUiLCJ3b3Jrc3BhY2VJZCI6ImNlOWpQdXZRcFFoVXRiYkFXQWZ2UTdDZ3diTDIiLCJ1c2VySWQiOiJjZTlqUHV2UXBRaFV0YmJBV0FmdlE3Q2d3YkwyIiwiaWF0IjoxNzg1NDM1NTYzfQ.mjinjPd9xphqTrbYBGo2xPAtHRNeCXcik4Hj2Y-A3qg?ref=blog.roboflow.com)


0:00


/ 0:10


You can also use Roboflow Agent available in your workspace, to build this workflow directly using natural language prompt.


Roboflow Agent is a conversational interface that uses prompts to generate workflows. You can then review, edit, and customize the generated workflow in the Workflow Editor.


### Step 1: Create a Workflow


To get started, create a free[Roboflow account](https://app.roboflow.com/?ref=blog.roboflow.com) and sign in. Next, create a workspace, navigate to Workflows from the left sidebar, and click Create Workflow.


You will be taken to the Workflows editor, where you can build and configure your workflow.


By default, the editor contains three key elements:


- An **Inputs** block that accepts an image as input
- An **Outputs** block with no configured outputs
- A[Roboflow Agent](https://docs.roboflow.com/agents/agents/roboflow-agent?ref=blog.roboflow.com) chat panel that lets you modify your workflow using natural-language prompts


For this tutorial, we will build the workflow manually, so you can hide the Roboflow Agent chat panel. You can also rename your workflow by clicking the ⚙️ icon.


### Step 2: Add a Zero-Shot Segmentation Model


Because we want to quickly prototype a hand-detection workflow for an American Sign Language (ASL) detection system without first collecting and labeling a dataset, we can use a zero-shot segmentation model.


For this guide, we will use **SAM 3** . SAM 3 is a zero-shot model from Meta AI designed to detect, segment, and track objects in images and videos using text prompts. It is available as a block in Roboflow Workflows, allowing you to use the model without having to deploy or host it yourself.


Click the + button in the upper-left corner of the Workflows editor. Then, search for SAM 3 and add the SAM 3 block to your workflow.


After adding the block, the Workflows editor should automatically connect the blocks in the following order:


**Inputs → SAM 3 → Outputs**


If these connections are not created automatically, connect the blocks manually, as shown below.


Connections pass outputs from one block to another, allowing downstream blocks to use the results produced by upstream blocks.


If the connections make your workflow difficult to follow, click **Auto Layout** , located beside the zoom controls, to automatically arrange the blocks into a cleaner layout.


Next, configure the SAM 3 block as shown below.


The SAM 3 block uses the values provided in the **Class Names** field to determine what objects to detect. The model searches for objects that match the supplied class names.


For this workflow, we use the class name **Hand** . SAM 3 cannot reliably recognize specific ASL hand signs or letters using prompts such as **ASL F** or **American Sign Language F** . However, it can detect hands, which allows us to quickly prototype the first stage of an ASL detection system.


In a production ASL detection workflow, you could later replace SAM 3 with a fine-tuned model trained to recognize specific ASL signs.


One advantage of Roboflow Workflows is that you can run your workflow after adding each block. This allows you to inspect intermediate outputs and verify that each step is working as expected before continuing.


You can also experiment with different class names to determine which prompts produce the best detection results for your use case.


Make sure to save your workflow after adding each block to keep your changes.


### Step 3: Visualize the Segmentation Mask


SAM 3 returns segmentation predictions that identify the pixels belonging to each detected object. You can visualize these predictions by adding a **Mask Visualization** block to your workflow.


Click the + button in the upper-left corner of the Workflows editor, search for Mask Visualization, and add the block to your workflow.


**SAM 3 → Mask Visualization → Outputs**


Next, configure the Mask Visualization block as shown below.


The block overlays the segmentation masks generated by SAM 3 onto the input image, making it easier to see the exact regions detected by the model.


### Step 4: Visualize the Bounding Boxes


In addition to segmentation masks, you can visualize bounding boxes around the objects detected by SAM 3.


To add this capability, add a **Bounding Box Visualization** block to your workflow.


**Mask Visualization → Bounding Box Visualization → Outputs**


Next, configure the Bounding Box Visualization block as shown below. Set the Input Image to the image produced by the Mask Visualization block. Then, use the predictions generated by the SAM 3 block as the Predictions input.


This configuration draws bounding boxes around the detected hands while preserving the segmentation masks from the previous step.


### Step 5: Visualize the Labels


You can also add labels to the detected objects. To do this, add a Label Visualization block to your workflow.


**Bounding Box Visualization → Label Visualization → Outputs**


Next, configure the **Label Visualization** block as shown below. Set the **Input Image** to the image produced by the Bounding Box Visualization block. This ensures that the labels are drawn on top of the bounding boxes and segmentation masks.


For the **Text** setting, select **Class and Confidence** . This displays both the predicted class name and the model's confidence score.


Finally, configure the Outputs block as shown below so that the label-visualized image is returned as the workflow output.


### Step 6: Run the Workflow


Your workflow is now complete. Click the Run (▷) button in the Workflows editor. A menu will open where you can upload an image and run the workflow.


After running the workflow, you should see a JSON output containing the label-visualized image, as shown below.


The image below shows an example output generated by the workflow. SAM 3 detects the hand and displays its segmentation mask, bounding box, label, and confidence score.


### Limitations of SAM 3 for ASL Detection


Although SAM 3 can detect common objects such as hands, it may not reliably recognize specific American Sign Language signs.


For example, you can experiment with prompts such as **G** , **ASL G** , **G American Sign Language** , and **Sign Language** .


To accurately recognize individual ASL letters or signs, you will likely need to collect and label a dataset and fine-tune a model for ASL detection.


Even with this limitation, SAM 3 is useful for rapidly prototyping computer vision applications. In this example, it allows us to build and test the hand-detection stage of an ASL detection system without first creating a custom dataset.


Once you have collected and labeled ASL training data, you can replace the SAM 3 block with a fine-tuned model trained to recognize specific ASL signs.


## How to Auto-Label Images and Build Datasets with Zero-Shot Models


SAM 3 was unable to recognize specific American Sign Language (ASL) signs in the previous workflow. To build a system that can recognize individual hand signs, we need to fine-tune a pre-trained model using a labeled dataset of segmented hands with the correct ASL sign labels.


Although SAM 3 cannot identify specific ASL signs, it can still detect and segment hands. We can use this capability in Roboflow to automate parts of the image annotation process and scale the creation of labeled datasets.


Instead of manually drawing a segmentation annotation around every hand, we can use SAM 3 to automatically detect and segment the hands in our images. We can then review the generated annotations and assign the correct ASL labels.


By automating the repetitive parts of annotation, we can process larger image collections more efficiently and reduce the time required to build a labeled dataset.


Without auto-labeling, you would need to manually annotate every hand in each image, as shown below.


0:00


/ 0:16


With[Roboflow’s Auto Label](https://roboflow.com/auto-label?ref=blog.roboflow.com) feature, we can use zero-shot models such as SAM 3 to automate hand detection and segmentation. This allows us to generate annotations more quickly, review and correct the results, and assign the appropriate ASL labels.


To auto-label your images using SAM 3, follow the steps below.


### Step 1: Create a Roboflow Project


Before you begin labeling images with Roboflow, you first need to create a Roboflow project. Projects store your images, annotations, dataset versions, and trained models within your workspace.


To create a project,[sign in to your Roboflow account.](https://app.roboflow.com/?ref=blog.roboflow.com) From your workspace, select **Projects** in the left sidebar, then click **+ New Project** .


A page will appear where you can enter a project name and select the computer vision task you want to perform.


For this guide, select **Instance Segmentation** . Enter a project name, then click **Create Project** .


Once your project has been created, you can begin uploading images to build your training dataset.


### Step 2: Upload Images to Label


Begin by dragging and dropping one or more images into your Roboflow project.


After selecting your images, click **Save and Continue** to begin uploading them.


Once your images have been uploaded,[Roboflow Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) provides several options for creating annotations:


- **Label Myself:** Annotate the images manually.
- **Label With My Team:** Collaborate with teammates on the annotation process.
- **Hire Outsourced Labelers:** Send your images to professional annotation teams.
- **Auto-Label and Review:** Automatically generate annotations using zero-shot models such as SAM 3.


Select **Auto-Label and Review** because we will use SAM 3 to automatically generate annotations for our images.


### Step 3: Select SAM 3 for Auto-Labeling


After selecting Auto-Label and Review, you should see the auto-labeling configuration page. SAM 3 may already be selected as the model used to generate annotations.


If SAM 3 is not selected, click the model dropdown and select SAM 3.


Roboflow Auto Label also allows you to use models that you have previously trained to automatically label new images. This can be useful when you want to expand an existing dataset using a fine-tuned model.


Next, enter the object you want SAM 3 to detect in the **Classes** field.


Because hand detection is an important first step in building an ASL detection system, enter **Hand** as the detection class.


SAM 3 uses the class name as a text prompt to determine which objects to detect and annotate. In this example, the model will search for hands and generate segmentation annotations around them.


### Step 4: Auto-Label Images with SAM 3


After entering **Hand** as the detection class, click **Generate Test Results** .


Roboflow will run SAM 3 on a sample of your uploaded images and display the generated annotations.


Review the test results to determine whether SAM 3 is detecting the intended objects accurately. You can also adjust the confidence threshold to control which predictions are included during auto-labeling.


Once you are satisfied with the results, click **Auto-Label with This Model** , then click **Start Auto-Label** .


Roboflow will begin generating annotations for the images you uploaded.


### Step 5: Review and Add Annotations to Your Dataset


Once the auto-labeling job is complete, you can review the generated annotations by clicking the Review job shown below.


You can use **Approve All** or **Reject All** to approve or reject an entire batch of annotations.


For this example, we will review the annotations individually. Although SAM 3 correctly detects the hands, it assigns the same **Hand** class to every detection. Our final model needs to recognize specific ASL signs, so we must update each annotation with the correct ASL class.


To review an annotation, click an image.


The image will open in Roboflow Annotate. From here, assign the correct class to each detected hand. For example, you can change the labels to **V** , **R** , or **Q** , depending on the ASL sign shown in the image.


After assigning the correct class, approve the annotation.


0:00


/ 0:30


Once you have corrected and approved the annotations, they will be added to your dataset.


### Optional: Speed Up Image Labeling with SAM 3 Smart Select


As an alternative to Auto-Label, you can select **Label Myself** from the available annotation options.


This opens your images in Roboflow Annotate, where you can use[Smart Select](https://docs.roboflow.com/datasets/annotate/annotate/ai-labeling/smart-select?ref=blog.roboflow.com) to generate annotations with SAM 3.


0:00


/ 0:19


Smart Select allows you to quickly create segmentation annotations while maintaining control over the labeling process.


You can also use Smart Select alongside Auto-Label. For example, if Auto-Label fails to detect an object, you can use Smart Select to create an annotation for the missed object manually.


By combining Auto Label, manual review, and Smart Select, you can use zero-shot models to create a labeled dataset more efficiently while maintaining high-quality annotations for model training.


## How to Fine-Tune a Pretrained Model with Roboflow Train


Earlier, we used SAM 3 as a zero-shot model in a workflow to detect and segment hands. However, although SAM 3 could identify hands, it could not reliably recognize specific ASL signs. For example, it could not consistently distinguish between signs such as **V** , **R** , and **Q** using text prompts alone.


To recognize specialized classes, such as individual ASL signs, we need to fine-tune a pre-trained model using a labeled dataset. Fine-tuning allows the model to learn the visual characteristics of each sign, making it better suited to this specific use case.


Now, we will use Roboflow Train to fine-tune an RF-DETR instance segmentation model on an ASL dataset. This will enable the model to detect and recognize individual ASL signs more reliably.


### Step 1: Build a Labeled Dataset


To fine-tune a model effectively, you need a sufficiently large and diverse labeled dataset. You can continue collecting images and use Roboflow’s SAM 3 auto-labeling feature, covered in the previous section, to speed up the annotation process.


Or, you can use an existing labeled dataset from Roboflow Universe. For this guide, we will use an[American Sign Language (ASL) instance segmentation dataset](https://universe.roboflow.com/team-roboflow/american-sign-language-poly?ref=blog.roboflow.com) available on Roboflow Universe.


To download the dataset, open the **Dataset** menu, then click **Download Dataset** .


Alternatively, you can fork the dataset directly into your Roboflow workspace. In this guide, we will download the dataset so that we can add it to our existing project.


Next, select Download as ZIP.


Select **COCO JSON** as the annotation format, then click **Continue** . The dataset download should begin automatically.


Roboflow supports multiple annotation formats, including COCO JSON, Pascal VOC XML, TensorFlow TFRecord, YOLO formats, and other supported annotation formats.


After downloading the ZIP file, extract its contents.


Next, upload the extracted dataset to your Roboflow project. Once the upload is complete, Roboflow will take you to the dataset version creation stage.


0:00


/ 0:14


You can find the uploaded images and annotations under Annotate in the left sidebar.


### Step 2: Create a Dataset Version


Roboflow Train requires you to create a dataset version before training.


A dataset version is a snapshot of your dataset at a specific point in time. It preserves your images, annotations, dataset splits, preprocessing settings, and augmentation settings. This allows you to train models using a reproducible version of your data.


Dataset versions make it easier to:


- Track changes as your dataset grows
- Reproduce previous training results
- Compare model performance across different dataset iterations
- Return to an earlier dataset version when needed


To create a dataset version, open the **Versions** tab under the **Data** section in the left sidebar of your Roboflow project.


When creating a dataset version, you can configure the training, validation, and test splits. You can also apply preprocessing and augmentation steps.


Start by entering a name for your dataset version. Because the uploaded dataset already contains predefined training, validation, and test splits, Roboflow preserves those splits by default. You can modify the splits by clicking **Edit** , as shown below.


Next, configure the preprocessing steps for your dataset.


For this guide, apply the following preprocessing operations:


- **Auto-Orient**
- **Resize:** 432 × 432 pixels


Preprocessing applies consistent transformations to every image in your dataset. Available preprocessing operations include resizing images, automatically correcting image orientation, converting images to grayscale, applying dynamic crops, and other image transformations.


You can also apply data augmentations to your dataset.


Data augmentation creates modified versions of training images. This helps the model learn from a wider range of visual conditions and can improve its ability to generalize to new images.


Available augmentation options include horizontal and vertical flips, image rotation, brightness adjustments, blur, noise, cropping, and other image transformations.


For this guide, apply the following augmentations:


- Crop: 0% minimum zoom and 20% maximum zoom
- Rotation: -15° to +15°


To learn more about these options, refer to the Roboflow guides on[image preprocessing](https://docs.roboflow.com/datasets/dataset-versions/image-preprocessing?ref=blog.roboflow.com) and[augmentation.](https://docs.roboflow.com/datasets/dataset-versions/image-augmentation?ref=blog.roboflow.com)


Next, optionally add a Version Note, then click Create to generate your dataset version.


Roboflow will process the dataset and apply the selected preprocessing and augmentation settings.


Once generation is complete, you should see the new dataset version, as shown below. The dataset version will also be available for download if needed.


### Step 3: Select a Training Engine


With your dataset version ready, you can begin fine-tuning your model using Roboflow Train.


[Roboflow Train](https://roboflow.com/train?ref=blog.roboflow.com) is a model training platform that allows you to train computer vision models on custom datasets without managing complex training infrastructure. It provides automated training pipelines, optimized model configurations, and evaluation tools to help you build accurate and deployable computer vision models.


To begin, click **Train** from the left sidebar. This opens the Roboflow Train interface, where you can choose from multiple training engines:


- [Neural Architecture Search (NAS)](https://blog.roboflow.com/neural-architecture-search/) automatically trains and evaluates multiple model architectures on your dataset. It then presents models with different speed and accuracy tradeoffs.
- **Custom Training** gives you more control over the training process. You can select a model architecture, choose a pretrained checkpoint, and configure training settings manually.


For this guide, select Custom Training so that we can configure the model architecture and training settings.


### Step 4: Select a Model Architecture


After selecting **Custom Training** , Roboflow displays the model architectures available for your project.


The available model architectures depend on the computer vision task selected for your project. For an instance segmentation project, available models may include RF-DETR, YOLO26, Roboflow 3.0, YOLOv11, and SAM 3.


Select[RF-DETR](https://blog.roboflow.com/rf-detr-segmentation/) , then choose the model size that best fits your application.


Smaller models generally provide faster inference and require fewer computational resources. Larger models typically provide higher accuracy but require more computational resources and may have longer inference times.


Roboflow Train also allows you to initialize training from an existing checkpoint. Because this is our first training run, select the pretrained RF-DETR weights.


You can also configure hyperparameters such as the number of training epochs. For this guide, you can use the default value of 100 epochs.


### Step 5: Select the Dataset Version


Next, select the dataset version you created earlier. The selected dataset version provides the images, annotations, dataset splits, preprocessing settings, and augmentation settings that Roboflow Train will use to fine-tune the model.


### Step 6: Begin Training


Before training begins, Roboflow Train displays a summary of your selected configuration, including the model architecture, model size, training settings, dataset version, preprocessing and augmentation settings, estimated training time, and expected credit usage.


Review these settings to ensure that everything is configured correctly. Once you are satisfied with the configuration, click Start Training to begin fine-tuning your model.


While your model is training, Roboflow Train provides real-time progress updates and performance metrics.


You can monitor information such as the current training epoch, overall training progress, estimated time remaining, training and validation metrics, and model performance throughout training.


You can access these details by opening **Versions** in the left sidebar and selecting **View Training** .


You can also cancel the training job at any time. If the model has already converged and additional training is unlikely to improve performance, stopping the job early can help save time and credits.


### Step 7: Run the Fine-Tuned Model


Once training is complete, Roboflow sends an email notification summarizing the training results.


The results may include evaluation metrics such as[Mean Average Precision (mAP)](https://blog.roboflow.com/mean-average-precision/) , precision, and recall.


The trained model is also added to your Roboflow workspace, where it is available for testing, inference, and deployment.


To test the model, navigate to **Models** in your project. You should see the model you trained, as shown below.


Select the model to open its testing interface. From this interface, you can upload an image and run inference using your fine-tuned model.


After uploading an image, the model generates predictions based on the ASL classes it learned during fine-tuning.


Unlike the zero-shot SAM 3 workflow, the fine-tuned RF-DETR model can recognize the specific ASL signs included in the training dataset. This makes fine-tuning more suitable for applications that require reliable recognition of specialized objects or domain-specific classes.


## How to Use Fine-Tuned Models in Roboflow Workflows


Once your model has been fine-tuned, you can add it to a Roboflow Workflow as a **Project Model** block, similar to how we added SAM 3 in the earlier workflow.


Using the fine-tuned model allows the workflow to recognize the specific ASL signs it was trained on, rather than detecting only a general object such as a hand.


To add the fine-tuned model to the workflow we created earlier, click the **+** button in the upper-left corner of the Workflows editor. Then, search for **Project Model** and select the block.


After selecting Project Model, a configuration window should appear, as shown below. Select the fine-tuned model you trained in the previous section.


Next, delete the SAM 3 block and connect the new Project Model block to the existing workflow.


Configure the workflow connections in the following order:


**Inputs → Project Model → Mask Visualization**


Next, configure the Mask Visualization block to use the predictions generated by the Project Model block, as shown below.


Now, run the workflow to test the fine-tuned model. Click the Run (▷) button, then upload an image to see the model's predictions. You can try or fork this workflow with the fine-tuned model[here.](https://app.roboflow.com/workflows/embed/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3JrZmxvd0lkIjoiUVBJUlczU3RDR2hRazdBWWhuWDkiLCJ3b3Jrc3BhY2VJZCI6ImNlOWpQdXZRcFFoVXRiYkFXQWZ2UTdDZ3diTDIiLCJ1c2VySWQiOiJjZTlqUHV2UXBRaFV0YmJBV0FmdlE3Q2d3YkwyIiwiaWF0IjoxNzg1NDQ4NzkxfQ.xLZquGz7J37yKPNOHYAt0RmjsxhaS-HnieeDFYm9Wtc?ref=blog.roboflow.com)


When you run the workflow, the output image should display the correct ASL label for the detected hand.


You can also use **Roboflow Agent** to replace SAM 3 without manually adding and connecting the **Project Model** block.


Enter the following prompt in the Agent chat panel:


***“Replace SAM 3 with the "sign-language-segmentation-wnciv-1-rfdetr-seg-small-t1" instance segmentation model you trained.”***


Roboflow Agent should update the workflow by replacing the SAM 3 block with your fine-tuned model.


Using Roboflow Agent can speed up workflow development by allowing you to modify workflows with natural-language prompts instead of manually adding, configuring, and connecting blocks.


## Deploying the Fine-Tuned Model Workflow with Roboflow Deploy


Once your workflow is ready, click **Deploy** in the Workflows editor. A deployment window will open with deployment instructions and several options for running your workflow in production.


[Roboflow Deploy](https://roboflow.com/deploy?ref=blog.roboflow.com) generates ready-to-use code that you can use to integrate your workflow into an application.


You can also provide the generated code to AI coding assistants such as Codex, Claude, Cursor, or ChatGPT to help integrate the workflow into an existing project.


Roboflow Deploy supports both local and cloud deployments, allowing you to run your workflow on:


- Images
- Video files
- Live webcam streams
- RTSP streams


Alternatively, if you only need to run inference with your fine-tuned RF-DETR model and do not require the additional logic in your workflow, you can deploy the model directly from your Roboflow project.


To do this, navigate to **Deploy** in the left sidebar of your project, then select **Deployments** . The **Deployments** page provides two options:


1. **Deploy My API:** Generates a Python script that runs a basic workflow. The workflow accepts an input image and returns predictions from your fine-tuned model.
2. **Customize with Logic:** Opens the same basic workflow in the Workflows editor, where you can use Roboflow Agent or manually add additional processing and custom logic.


The Deployments page also provides additional deployment options. For example, you can download your trained model weights or deploy the model directly to supported edge devices, such as NVIDIA Jetson hardware.


With Roboflow Deploy, you can move from a fine-tuned model to a production-ready application without building the entire inference and deployment pipeline from scratch.


## Conclusion: Zero-Shot vs. Fine-Tuned Models


Zero-shot and fine-tuned models are not competing approaches. Instead, they serve different roles across the computer vision development lifecycle.


Zero-shot models are ideal for quickly validating ideas, prototyping workflows, and detecting objects without the need to collect and label large datasets.


As your computer vision application moves from experimentation to production, requirements such as higher accuracy, faster inference, lower operating costs, and consistent performance in a specific environment often become more important.


In these cases, fine-tuning a model on data that represents your target use case can fulfill these requirements.


The most effective workflow is often a combination of both approaches: start with a zero-shot model to explore your idea and label your dataset, then fine-tune a model once you have enough labeled data and need production-level performance.


With tools like Roboflow Workflows, Annotate, Train, and Deploy, you can move from initial experimentation to a fully deployed computer vision application without building the entire pipeline from scratch.[Try Roboflow for free.](https://app.roboflow.com/?ref=blog.roboflow.com)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Dikshant Shah](https://blog.roboflow.com/author/dikshant/) . (Aug 4, 2026). Zero-Shot vs. Fine-Tuned Models: When to Train Your Own. Roboflow Blog: https://blog.roboflow.com/zero-shot-vs-fine-tuned-models/*


### Written by


Dikshant Shah


I develop end-to-end computer vision pipelines by integrating multiple machine learning models, such as SAM 3 and RF-DETR, to solve diverse real world use cases.


[View more posts](https://blog.roboflow.com/author/dikshant/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Model Training](https://blog.roboflow.com/tag/model-training/)
- [Model Deployment](https://blog.roboflow.com/tag/model-deployment/)
- [Roboflow Deploy](https://blog.roboflow.com/tag/roboflow-deploy/)

---
schema_version: "1.0.0"
document_id: "5865b3d12147d9aa7023a43678aaf7b90c76ef6a57c57a2a29b2f4abe799fa88"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/cancer-research-computer-vision/"
published_at: "2026-06-03T13:50:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:a72ab0342a1c662389d5dc0a587bb3f87548184311a2badeb53b4128937d765e"
---

# Improving Cancer Research with Computer Vision

[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published Jun 3, 2026 • 3 min read


Summary


**Researcher Mateo Sokac trained an object detection model in Roboflow to count neutrophils in dry microscopy, hitting 0.70 mAP with image tiling and augmentation.**


Computer vision is improving life sciences. From the early[identification of cancer](https://venturebeat.com/2020/01/01/google-healths-ai-identifies-breast-cancer-in-mammogram-imagery-with-fewer-false-positives/?ref=blog.roboflow.com) to improving plant health,[machine vision](https://roboflow.com/industry/machine-vision?ref=blog.roboflow.com) is enabling us to create more accurate diagnoses, cures, and research methods.


Mateo Sokac is advancing progress in cancer research with[object detection](https://blog.roboflow.com/object-detection/) . Mateo is a Danish-based researcher at Aarhus University concurrently working at the University hospital (Aarhus Universitetshospital). His work builds on his undergraduate studies at the Rochester Institute of Technology, a leader in image recognition techniques.


## Identifying Neutrophils with Computer Vision


Neutrophils are a type of white blood cell that play a key role in animal immune systems. In humans, neutrophils help heal injuries and fight infections. Identifying and measuring their presence in microscopy is an essential step in assessing the efficacy of given experiments.


Individually labeled neutrophils from Mateo's dataset. (Credit: Mateo Sokac)


Mateo notes there are two techniques his labs leverage to identify neutrophils: immersion and dry microscopy. Immersion is generally slower, more expensive, and more precise. On the other hand, dry microscopy is faster, cheaper, and of lower quality.


Mateo's team is currently running experiments to identify which areas of a cell population are enriched in animal populations. In time, these same technique will be applied to humans to advance cancer research.


In order to increase his lab's throughout, Mateo sought to[train a model](https://roboflow.com/train?ref=blog.roboflow.com) to more accurately identify neutrophils in dry microscopy as accurately as one can in immersion. Mateo trained a model on images labeled from immersion and immersion pictures yet validated from image on dry microscopy alone. Mateo initially had 127 images, augmented with Roboflow to 451, and saw a[mean average precision](https://blog.roboflow.com/mean-average-precision/) of 0.70: a 10 percent increase compared to his un-augmented results.


Mateo easily attempted training multiple architectures via the[Roboflow Playground](https://playground.roboflow.com/?ref=blog.roboflow.com) , including[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) ,[EfficientDet](https://playground.roboflow.com/models/google/efficientdet?ref=blog.roboflow.com) ,[MobileNetSSD2](https://playground.roboflow.com/models/google/mobilenet-ssd-v2?ref=blog.roboflow.com) , and more.


### Using Image Tiling to Improve Performance


Moreover, due to the large (` 27000 x 27000` pixels) input images, Mateo's team leverages tiling to improve model performance. Tiling is the practice of breaking a large input image into smaller input images so that a model may train and performance inference on higher pixel density images. For example, if an input image is 1600 x 1600 yet a model may only be able to perform on, say, 800 x 800 input images, breaking an image into four pieces enables the input image to retain its original image quality. Roboflow supports easy tiling as a preprocessing step.


In Mateo's case, an individually labeled example appeared like the following:


One tile of labeled neutrophils. (Credit: Mateo Sokac)


After an image is tiled, it is run through the model to count neutrophils, and the team then reconstructs the individual tiles to create a full image output.


A prediction for one of these individual tiles appears like the following:


Predictions for neutrophil identification on an outputted image tile. The numbers refer to the model's predicted confidence of each label. (Credit: Matro Sokac)


### Resulting Images


Once each individual tile has inference conducted, the tiles are reconstructed into their` 27000 x 27000` pixel final form. The results enable Mateo's team to move more quickly and accurately in determining experiment efficacy.


The output images are truly stunning:


A fully reconstructed image from Mateo's lab. Each individual green spec is a labeled outputted neutrophil the model has identified.


Upon seeing the fully sized image, is it clear why tiling is requisite to capture the detailed granularity necessary for Mateo's team to achieve accurate results!


Read more[Roboflow customer stories here](https://roboflow.com/customer-stories?ref=blog.roboflow.com) .


Published: October 4, 2020
Updated: June 3, 2026


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (Jun 3, 2026). Improving Cancer Research with Computer Vision. Roboflow Blog: https://blog.roboflow.com/cancer-research-computer-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)

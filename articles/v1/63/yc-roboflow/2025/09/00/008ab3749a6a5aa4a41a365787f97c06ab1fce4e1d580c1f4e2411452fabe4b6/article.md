---
schema_version: "1.0.0"
document_id: "008ab3749a6a5aa4a41a365787f97c06ab1fce4e1d580c1f4e2411452fabe4b6"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/searching-for-jetskis-finding-roboflow/"
published_at: "2025-09-19T13:47:45+00:00"
first_seen_at: "2026-07-28T00:08:32.096830+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:53c0d5032d22ad60082ba2125dbaf49555d96b58f1bbb64f33e5d9415c51199d"
---

# Searching for Jetskis, Finding Roboflow

[Dimitri Kosenko](https://blog.roboflow.com/author/dimitri/)


Published Sep 19, 2025 • 5 min read


SUMMARY


**A developer building a marine perimeter intrusion system on Axis cameras with onboard DPUs ran into the usual obstacles: annotation management, dataset balancing, model quantization to int8, and cross-compilation for a rigid embedded SDK. Discovering Roboflow cut through most of that overhead, enabling faster dataset assembly with augmentations and a training loop in Google Colab before exporting a YOLOv5 model in the ONNX and TFLite formats that Axis's Larod inference engine requires. This post tells that first-person story and reflects on what makes rapid iteration in computer vision possible when the tooling gets out of the way.**


When I first started building a computer vision project to detect boats on the water, I had no idea how much time I’d spend wrangling datasets, retraining models, and debugging deployment.


The concept was simple — use an Axis camera with an onboard DPU (Deep Learning Processing Unit) to spot different types of vessels — but the reality was messy. Managing annotations, balancing datasets, and navigating a rigid embedded SDK turned out to be harder than producing the detections themselves.


So why even bother with such a project? Working with Axis cameras for 12 years enabled me to see their transformation from CCTV security to sensors with built-in intelligence and an impact on almost every vertical in the industry. Some clients had complex enterprise needs wrapped in cybersecurity and compliance frameworks, while others simply wanted reliable, cost-effective solutions that could scale.


Living in the Sunshine State, where boats and jet skis are part of everyday life, my business partner and I saw a clear need: perimeter intrusion detection for marine vessels entering restricted zones. In South Florida, with its islands and high-end waterfront properties, the water is often the weakest point of a secure perimeter. Traditional pixel-based video analytics fall apart here — false positives from rippling water, false negatives from changing light, wildlife, or clutter. More advanced analytics came with a hefty license fee and a steep hardware requirement. A reliable, scalable computer vision solution was the only way forward.


That vision led us to build a camera-native app that could detect and classify vessels directly on the device’s DPU — even tracking them with a PTZ for more context. But we quickly hit a wall: dataset management, performance bottlenecks, and the lack of flexible developer tooling.


Packaging the app meant cross-compiling, quantizing the model into int8, converting it to TensorFlow Lite, juggling Docker repositories, and debugging dependencies like OpenCV and VDO just to render overlays. The computer vision part was only half the battle — packaging and deployment became just as time-consuming.


That’s when I discovered Roboflow.


## Finding Roboflow


At first, I was piecing things together manually: creating environments with Anaconda, manually labeling images in Label Studio, exporting[YOLOv5](https://roboflow.com/model/yolov5?ref=blog.roboflow.com) datasets, training and quantizing in Google Colab. To measure performance, I had to run custom Python scripts after each training cycle to calculate mAP, precision, and recall. Any dataset adjustment — rebalancing classes or adding images — meant repeating this cycle from scratch, with no visibility until the very end.


Roboflow changed that completely. I first stumbled onto it while looking for more training images — something easy to do in South Florida, but time-consuming to manage. In[Roboflow Universe](https://universe.roboflow.com/?ref=blog.roboflow.com) , I found ready-made marine datasets that I could merge with my own. With a few clicks, I trained a small bootstrap model and unlocked auto-annotation, turning hours of hand-labeling into minutes.


The dataset augmentation tools were another game-changer. I could instantly simulate variations in tilt, blur, and lighting that would naturally affect real-world cameras; dataset enrichment that can be difficult and time-consuming to accomplish manually. Roboflow’s real-time metrics dashboard showed me precision, recall, and class balance without extra scripts — making it clear, for example, that rare classes like “Kayak” needed oversampling compared to more common ones like “Speedboat.” Lastly, live video content that I gathered could be easily transformed into as many images as I would like to make the dataset even more robust.


## Integrating my model with Axis


With Roboflow, I wasn’t just training models faster — I was making smarter decisions about how to structure my datasets, and I had visibility into performance at every step. That efficiency turned what was once a frustrating cycle of guesswork into a streamlined workflow, letting me focus more on building the marine detector application itself, including additional functionality like API support for external events, MQTT, and cosmetic improvements to the UX.


Axis, being an open company that encourages 3rd party development on its platform, provides helpful resources for external ACAP development, including a device API library, Native SDK documentation pages, and example Github content:


1. [Axis VAPIX API Library](https://developer.axis.com/vapix/?ref=blog.roboflow.com)
2. [Axis Native SDK 12 Documentation](https://developer.axis.com/acap/?ref=blog.roboflow.com)
3. [Github Repositories (Examples of .c files, Dockerfiles, Manifest)](https://github.com/AxisCommunications/acap-native-sdk-examples?ref=blog.roboflow.com)
4. [COCO on ARTPEC8 Processors with DPU](https://github.com/pandosme/DetectX?ref=blog.roboflow.com)
5. [YOLOv5, Axis Patch + Quantization Guide](https://github.com/pandosme/DetectX/blob/main/Train-Build.md?ref=blog.roboflow.com)


Under the hood, we leveraged the Deep Learning Processing Unit (DPU) inside the camera, and relied on Roboflow heavily to build and balance our dataset for robustness. Since this was an edge platform with unique constraints, we couldn’t run[Roboflow Inference](https://inference.roboflow.com/?ref=blog.roboflow.com) directly on it (as you could on a Jetson, for example).


Instead, we had to rely on Axis’s own Larod runtime, the camera’s inference engine. Roboflow allowed us to enrich our dataset with augmentations, then export and train a[YOLOv5](https://roboflow.com/model/yolov5?ref=blog.roboflow.com) model in Google Colab.


To make the model fully compatible with Axis hardware, it needed to be trained in[YOLOv5](https://roboflow.com/model/yolov5?ref=blog.roboflow.com) , quantized to int8, and converted to ONNX/TFLite — the formats Larod supports for running efficiently on the embedded DPU.This project started as a personal experiment — trying to solve a real-world problem we inherited using some Axis cameras and a bit of scrappy model training.


## Building, experimenting, shipping


Along the way, I got a taste of what makes Roboflow exciting: it strips away the noise and redundancy so you can focus on the fun parts — building, experimenting, and shipping.


I’ve gone from “searching for jetskis” to building a working marine detector, quantized and running on an embedded DPU, powered by datasets I could assemble and iterate on in record time. The spirit of tinkering is what drew me to Roboflow, and it’s the same energy I’ve found here: ship fast, learn by doing, and share openly so others can build on top of it.


I’m excited to contribute, keep learning, and see what we build at Roboflow!


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Dimitri Kosenko](https://blog.roboflow.com/author/dimitri/) . (Sep 19, 2025). Searching for Jetskis, Finding Roboflow. Roboflow Blog: https://blog.roboflow.com/searching-for-jetskis-finding-roboflow/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Dimitri Kosenko


Implementation Engineer @ Roboflow


[View more posts](https://blog.roboflow.com/author/dimitri/)


### Topics


- [Case Studies](https://blog.roboflow.com/tag/case-studies/)

---
schema_version: "1.0.0"
document_id: "55611f822e8f153a7ab5bfd878102e5191d1869a2b6fdd6fa9695c9363a84cf8"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/launch-rf-detr-keypoint-in-roboflow/"
published_at: "2026-06-22T19:15:43+00:00"
first_seen_at: "2026-07-22T12:07:01.391993+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:c5c68389a8b9b92224bba54784441e2ec70cc6cb1d307f66c6dddd8066ff2d1e"
---

# Launch: RF-DETR Keypoint in Roboflow

[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/)


Published Jun 22, 2026 • 4 min read


SUMMARY


**RF-DETR Keypoint is a real-time, end-to-end pose model that beats YOLO26-pose on both accuracy and speed, learns calibrated per-keypoint uncertainty from your data, and ships under Apache 2.0. You can label, train, and deploy it in Roboflow today.**


Today we are launching RF-DETR Keypoint support in Roboflow. RF-DETR Keypoint is a real-time, end-to-end keypoint detection model that extends the[RF-DETR](https://roboflow.com/model/rf-detr?ref=blog.roboflow.com) family from boxes and masks. You can use RF-DETR Keypoints across the Roboflow platform: label your skeletons in[Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) , train on your own data with[Train](https://roboflow.com/train?ref=blog.roboflow.com) , and deploy with[Inference](https://inference.roboflow.com/?ref=blog.roboflow.com) and[Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) .


RF-DETR Keypoint is releasing as a preview, the same way our[instance segmentation](https://blog.roboflow.com/rf-detr-segmentation-preview/) model did, because we want real-world usage to shape the final family of checkpoints. You can start building with it now.


For the architecture, the self-calibrating loss, and the full benchmark methodology, read the[RF-DETR Keypoint technical deep dive](https://blog.roboflow.com/real-time-keypoint-detection-with-rf-detr/) .


0:00


/ 0:09


[Source](https://x.com/skalskip92/status/2067637677852119357?s=20&ref=blog.roboflow.com)


## Label, Train, and Deploy in Roboflow


RF-DETR Keypoint runs through the full Roboflow workflow:


Label your skeletons. Define the keypoints and the skeleton for your object in[Annotate](https://roboflow.com/annotate?ref=blog.roboflow.com) , whether that is a 17-point person or 20 points on a basketball court. There is no requirement to match COCO.


Train on your data. Fine-tuning RF-DETR Keypoint works the same way as it does for[detection and segmentation](https://blog.roboflow.com/train-deploy-rf-detr/) .


Deploy where you run. Serve the model with[Inference](https://inference.roboflow.com/?ref=blog.roboflow.com) on the cloud or the edge, and chain it with logic and other models in[Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) to turn poses into applications: rep counting, ergonomic checks, sports analytics, robot guidance, gauge reading.


## What is RF-DETR Keypoint?


RF-DETR Keypoint builds a keypoint head directly into the detection transformer. For every object it detects, it predicts a structured set of keypoints in a single forward pass: no NMS, no heatmaps, no post-hoc grouping of points into instances. Detection and pose are learned jointly, so the keypoints even feed back to make the detections better.


[Source](https://x.com/skalskip92/status/2066922118231503102?s=20&ref=blog.roboflow.com)


It is not limited to people. The COCO checkpoint covers the 17-keypoint human skeleton because that is the benchmark everyone can check, but the architecture supports arbitrary skeletons: any number of keypoints, on any object class, with multiple keypointed classes in one model. License-plate corners, surgical instrument tips, robot-arm joints, and gauge needles are the point.


## How RF-DETR Keypoint Is Different


Three things set RF-DETR Keypoint apart from other pose models.


It beats the newest YOLO pose models on accuracy and speed. On COCO Keypoints, measured end-to-end on an NVIDIA T4 with TensorRT FP16, the preview checkpoint at 576x576 reaches 71.8 AP at 9.8 ms, ahead of YOLO26x-pose (the largest model in the newest YOLO pose family) at 71.0 AP and 10.6 ms. Scaled up to 888x888 it reaches 74.2 AP, within 0.6 AP of the state-of-the-art academic model GroupPose Swin-L while running roughly 13 times faster.


[Source](https://x.com/skalskip92/status/2068006030919762026?s=20&ref=blog.roboflow.com)


It learns its own keypoint uncertainty, and hands it to you. Most pose models depend on per-keypoint tolerance constants that were hand-measured on COCO and quietly fall back to a uniform guess the moment your skeleton is not COCO's.


RF-DETR Keypoint instead predicts a full distribution for each keypoint and calibrates that spread from your data, so the loss that produces the benchmark numbers is the same loss you fine-tune with. Each keypoint comes back with a confidence ellipse and a usable 2D covariance, a drop-in observation model for a tracker, a Kalman filter, or a skeleton fit, plus separate signals for whether a keypoint is findable at all versus merely occluded.


One checkpoint runs at every speed. Like the rest of the RF-DETR family, it is trained with weight-sharing neural architecture search, so a single set of weights runs across resolutions from about 4.5 ms to 26 ms with no retraining. Train once, get a family of options.


## RF-DETR is Apache 2.0: Ship It Anywhere


RF-DETR Keypoint Preview is released under the Apache 2.0 license, code and weights, free for commercial use, with no copyleft obligations and deployable inside closed-source products.


[Source](https://x.com/mirrash7/status/2067274726674674022?ref=blog.roboflow.com)


The YOLO family are AGPL-3.0, which in practice means open-sourcing the application you build around the model or buying an enterprise license, even for many internal commercial uses. If licensing has been the thing standing between your team and shipping a pose model, it is not anymore.


## Conclusion


RF-DETR Keypoint is launching as a preview. The architecture and training recipe are performing well in our evaluations, but real-world usage surfaces things benchmarks do not, and that feedback will shape what comes next: checkpoints across the full accuracy-latency curve, NAS-enabled training on the platform, and purpose-built models for popular keypoint datasets.


[Start with RF-DETR Keypoint](https://app.roboflow.com/?ref=blog.roboflow.com) or read the[technical deep dive](https://blog.roboflow.com/real-time-keypoint-detection-with-rf-detr/) .


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Contributing Writer](https://blog.roboflow.com/author/contributing-writer/) . (Jun 22, 2026). Launch: RF-DETR Keypoint in Roboflow. Roboflow Blog: https://blog.roboflow.com/launch-rf-detr-keypoint-in-roboflow/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Contributing Writer


[View more posts](https://blog.roboflow.com/author/contributing-writer/)


### Topics


- [Product Updates](https://blog.roboflow.com/tag/product-updates/)
- [Model Training](https://blog.roboflow.com/tag/model-training/)
- [Model Deployment](https://blog.roboflow.com/tag/model-deployment/)

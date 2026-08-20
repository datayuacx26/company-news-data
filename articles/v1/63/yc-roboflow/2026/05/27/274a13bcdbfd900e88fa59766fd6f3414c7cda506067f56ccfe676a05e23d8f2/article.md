---
schema_version: "1.0.0"
document_id: "274a13bcdbfd900e88fa59766fd6f3414c7cda506067f56ccfe676a05e23d8f2"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-news-import-01e8e48f5676"
canonical_url: "https://blog.roboflow.com/how-coco-parks-accelerated-their-development-with-roboflow/"
published_at: "2026-05-01T13:41:00+00:00"
first_seen_at: "2026-07-25T22:08:11.425113+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:8b7a325d8ce64b90c290fa68ca48c14811af9f3c486546e12b8d19638921bc4e"
---

# How Cocoparks Accelerated Their Development with Roboflow

[Joseph Nelson](https://blog.roboflow.com/author/joseph/)


Published May 1, 2026 • 2 min read


SUMMARY


**Cocoparks, a Paris-based startup building parking and traffic-flow intelligence for French cities, shipped a working computer vision model in 30 days by using Roboflow to handle data preparation, augmentation, and format conversion.**


Cocoparks, a Paris based startup working on improving traffic flows across French cities, launched their service months faster with Roboflow.


"Before Google Maps, you could find routes to your destination using a map. There are many routes to try. But with Google Maps, it becomes seamless. This is what using Roboflow was like for us – **it made our development faster, more accurate, and easier."** - Raphael, Cocoparks Founder, CEO


> "Roboflow is the 'Google Maps' of computer vision development."


## How Roboflow's Workflow Helped Go from Zero to Minimum Viable Product in One Month


Raphael and his team found Roboflow improved their development in a few key ways. It saved the team weeks of engineering efforts, acting like another machine learning engineer without needing to hire one.


### Data Preparation


Cocoparks more quickly created training datasets, mixing and matching image examples into what best worked for their problem context.


"We were able to immediately visualize label issues, removing poorly labeled images. It enabled us to quickly find easy ways to improve our datasets."


### Data Augmentation


Data augmentation enables teams to create more representative datasets without collecting additional data. For the Cocoparks team, limited data – especially from unique perspectives required for their problem – plagued improved model performance.


"Upon including even a few basic augmentations, we saw head-to-head model improvements of 2 percent without changing anything else."


Ultimately, the model achieved 93%[mAP](https://blog.roboflow.com/mean-average-precision/) on the task at hand.


### Model Prototyping


The Cocoparks team found themselves spending too much time wrangling various data formats, converting between model frameworks, and not focusing on their task at hand.


"We were able to convert from[PASCAL VOC to TFRecord](https://roboflow.com/convert/pascal-voc-xml-to-tensorflow-tfrecord?ref=blog.roboflow.com) or[COCO JSON](https://roboflow.com/formats/coco-json?ref=blog.roboflow.com) seamlessly so we could try more models, faster. It saved at least one week of engineering time."


The team tried models from the[Roboflow Model Library](https://playground.roboflow.com/models?ref=blog.roboflow.com) , finding[MobileNetSSDv2](https://playground.roboflow.com/models/google/mobilenet-ssd-v2?ref=blog.roboflow.com) to be most apt for their use case.


Ultimately, the Cocoparks team shipped a working model for their application in only 30 days, enabling the team to go to market faster, validate their idea, and win more customers.


Read more[Roboflow customer stories here](https://roboflow.com/customer-stories?ref=blog.roboflow.com) .


Published: August 29, 2020
Updated: May 1, 2025


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Joseph Nelson](https://blog.roboflow.com/author/joseph/) . (May 1, 2026). How Cocoparks Accelerated Their Development with Roboflow. Roboflow Blog: https://blog.roboflow.com/how-coco-parks-accelerated-their-development-with-roboflow/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Joseph Nelson


Roboflow cofounder and CEO. On a mission to transform every industry by democratizing computer vision. Previously founded and sold a machine learning company.


[View more posts](https://blog.roboflow.com/author/joseph/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)

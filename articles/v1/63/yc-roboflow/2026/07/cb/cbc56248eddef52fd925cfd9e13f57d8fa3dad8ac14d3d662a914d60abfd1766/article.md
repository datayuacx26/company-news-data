---
schema_version: "1.0.0"
document_id: "cbc56248eddef52fd925cfd9e13f57d8fa3dad8ac14d3d662a914d60abfd1766"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/scale-a-computer-vision-pilot-to-production/"
published_at: "2026-07-15T15:40:50+00:00"
first_seen_at: "2026-07-25T21:41:10.504044+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:63e3da52b3d585a5352c7f0b700425dc263d351c34393e502fa3fa30d2e559e8"
---

# How to Scale a Computer Vision Pilot to Production in Manufacturing

[Erik Kokalj](https://blog.roboflow.com/author/erik/)


Published Jul 15, 2026 • 4 min read


SUMMARY


**Most computer vision pilots in manufacturing stall in the integration, not the model. Scale by modeling vision data as reusable so it lands in your data lake, running detection at the edge with a compact model like RF-DETR, and handing the platform to the plants to run themselves.**


Around 77% of vision AI implementations in[manufacturing](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) never make it past the pilot phase. The striking part is that this is rarely a technology failure. The models pick up the signals and generate the alerts.


What breaks is everything around the model: the disconnected systems, the teams that do not share data, and the assumption that a vision project is a software project instead of an operational change. Scaling a computer vision pilot to production is a people-and-process problem long before it is a modeling problem.


That is the throughline of a recent episode of the[AI in Business Podcast](https://open.spotify.com/episode/6jMqBDWIBMpBHDo2mbDd1f?ref=blog.roboflow.com) , where Jeff Witt, Digital Transformation Leader at a Fortune 500 global leader in building materials and fiberglass composites, who has deployed[computer vision in manufacturing](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) across a large plant network. Jeff has watched pilots stall and pushed them through to production at scale, and his account of what actually changed is concrete enough to act on.


This article walks through what scaling a computer vision pilot to production really takes, why so many stall, and what the episode shows about getting past it.


## What scaling a computer vision pilot to production means


A pilot proves a model can spot something on one line, in one plant, under conditions someone set up for it. Production means that same capability runs on its own, feeds its output into the systems people already use, and repeats across dozens or hundreds of sites without a specialist babysitting each one.


The gap between those two states is mostly integration. In Jeff's experience the camera systems sit on the manufacturing IT network, separated from the enterprise data pipelines and BI systems. A pilot can ignore that separation. Production cannot, because the value only shows up when the vision data lands next to the quality, manufacturing, and ERP data where analysts and operators can act on it. Get that architecture right once and the recipe becomes repeatable, deployable remotely, and scalable across a hundred sites. Leave it as an afterthought and every plant becomes its own custom project.


## Why so many pilots stall


The failure pattern is consistent. Teams treat the model as the deliverable, ship a working demo, and then discover the demo has nowhere to send its results. Data is the limiting factor, but blaming messy data is a description, not a diagnosis. The real issue is that the vision output is trapped in a silo.


Jeff's fix was to model the vision data as reusable, so it travels with the rest of the manufacturing data into the data lake rather than becoming yet another disconnected application. That is what turns a one-plant pilot into a program. It is also why he is blunt about waiting for perfection: none of his models are perfect, and holding out for a completely clean data environment before deploying is, in his words, a recipe for never deploying. You ship with the data you have and improve from there.


## Why vision AI is ready for the plant floor


Two things make this work now. The first is that the technology meets the plant where it is. Most facilities already have process cameras pointed at the parts of the operation worth watching. Layering[automated visual inspection](https://roboflow.com/industry/vision-inspection-systems?ref=blog.roboflow.com) on top of that existing infrastructure unlocks value without ripping anything out, and the value shows up fast. Five years ago an operator had to sit in a control room watching screens. Now the model watches every frame and surfaces the anomaly when it happens, in near real time.


The second is deployment. Safety monitoring, proximity alerting, and chemical bin-label verification cannot wait on a round trip to the cloud, and some run at criticality levels where a wrong call dumps a day of product or worse. Running models at the edge with[Roboflow Inference](https://roboflow.com/deploy?ref=blog.roboflow.com) keeps decisions local and low-latency, and a compact detector like[RF-DETR](https://roboflow.com/model/rf-detr?ref=blog.roboflow.com) is small enough to run on on-site hardware while staying fast enough for real-time monitoring. Building the pipeline as a set of reusable blocks in[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) is what makes the same recipe deployable across sites instead of rebuilt per plant.


Jeff keeps a human in the loop for now. An alert goes to a person before any action is taken. He is clear that the system could stop a pump on a wrong-chemical detection and save a million dollars in one call, and that the industry will get there, but only once people trust the systems. That trust is earned in production, not in the lab.


## What the episode covers


The conversation is worth a full listen because it is a practitioner walking through what actually moved the needle. A few of the payoffs:


Jeff explains why the visual nature of[quality control and vision inspection](https://roboflow.com/ai/quality-control?ref=blog.roboflow.com) is a change-management advantage. Unlike a black-box server rack that a handful of people ever log into, you can pull the video out of the system and show a fork truck getting too close to an operator or a defect getting caught. That visibility drives adoption in a way dashboards never do.


He also lands the structural move that unlocked scale: taking the platform out of IT-only hands and making it business-led, so the plants define their own use cases, build their own models, and scale what works in New York to fourteen similar lines across the country. And he frames the whole thing the way any team evaluating this should hear it: it is a platform, not a point solution. Safety, quality, label verification, production metrics, all from the same toolset, trained to do just about anything the operation needs.


If you have a Vision AI pilot that has been stuck for six months, Jeff's Monday-morning answer is direct: put the tools in the hands of the people in the plants and let the business run with it.


## Listen to the episode


Hear the full conversation with Jeff Witt on Emerj's AI in Business Podcast:[Scaling Vision AI in Manufacturing](https://open.spotify.com/episode/6jMqBDWIBMpBHDo2mbDd1f?ref=blog.roboflow.com) .


When you are ready to build the pipeline behind it, start in[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) and deploy it to the edge with Roboflow Inference.


For the operating model that turns one-off pilots into a capability your own teams own and run, get[Roboflow's Vision AI Center of Excellence Blueprint](https://roboflow.com/ebooks/center-of-excellence?ref=blog.roboflow.com) . It includes a catalog framework for deploying proven solutions in weeks, a maturity model that shows where your program stands, and a 10-question diagnostic that tells you whether you are on track or still stuck in pilot mode.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Erik Kokalj](https://blog.roboflow.com/author/erik/) . (Jul 15, 2026). How to Scale a Computer Vision Pilot to Production in Manufacturing. Roboflow Blog: https://blog.roboflow.com/scale-a-computer-vision-pilot-to-production/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Erik Kokalj


Developer Experience @ Roboflow


[View more posts](https://blog.roboflow.com/author/erik/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Manufacturing](https://blog.roboflow.com/tag/manufacturing/)

---
schema_version: "1.0.0"
document_id: "f942149ff5746b8be99b827681d1a822347e1010d113eb66058bd8f0a243d3ce"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/gemini-3-6-flash-for-vision/"
published_at: "2026-07-22T15:13:55+00:00"
first_seen_at: "2026-07-25T21:41:10.504044+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:4ebf63f24c245ed5b8006f73c928b85364e8aec813e5c0f99ff7a25db1238202"
---

# Gemini 3.6 Flash for Vision: Evaluation and Benchmarks

[Erik Kokalj](https://blog.roboflow.com/author/erik/)


Published Jul 22, 2026 • 4 min read


Google released[Gemini 3.6 Flash](https://playground.roboflow.com/models/google/gemini-3-6-flash?ref=blog.roboflow.com) on[July 21, 2026](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-6-flash-3-5-flash-lite-3-5-flash-cyber/?ref=blog.roboflow.com) , together with Gemini 3.5 Flash-Lite. Google calls 3.6 Flash its " *workhorse model.* " It is faster and cheaper than Gemini 3.5 Flash, and it uses about 17% fewer output tokens for the same task.


We ran both new models through our[Roboflow Vision Evals](https://playground.roboflow.com/evals?ref=blog.roboflow.com) to see how they do on real images and videos, not just text benchmarks. These evals are private. The majority of test images and answers are held back and never published, so no model can be tuned (benchmaxxed) on them.


****TL;DR**** : Gemini 3.6 Flash is a good deal. It matches or leads Gemini 3.5 Flash on most image tasks. It is the best model we have tested on video, and it costs less to run. The one real step back is object detection, where it gets lazy and drops far below Gemini 3.5 Flash.


## Gemini 3.6 Flash for Object Detection


[Object detection](https://blog.roboflow.com/object-detection/) is the weak spot. On mAP@50 (a standard detection score, higher is better), Gemini 3.6 Flash falls to the bottom half of the pack, well behind Gemini 3.5 Flash and even behind the cheaper Flash-Lite.


It seems as though 3.6 Flash got lazy. On many images it returns one large, loose box instead of several tight ones. It also writes malformed JSON often enough that some responses fail to parse, so you lose results even when the model clearly saw the objects.


Other users have reported the same single-box behavior since launch. The examples below show this happening: in each one it draws a handful of boxes for a scene that holds dozens.


## Gemini 3.6 Flash for Vision


Outside detection, Gemini 3.6 Flash sits at or near the front.


Object detection and counting.


Counting is a surprise: Gemini 3.6 Flash leads it, just ahead of Gemini 3.5 Flash, even though it sits near the bottom on detection. So the model did not get worse at seeing objects. It just got lazy about drawing a box around each one.


Data extraction and reasoning.


Gemini 3.6 Flash ties for the top on data extraction and lands mid-pack on reasoning. The new Gemini 3.5 Flash-Lite is the exception: cheap and fast, but last of every model on reasoning. So it misses questions that need a step of thought, like a total or a price difference.


## Where Gemini 3.6 Flash Wins: Video


Gemini 3.6 Flash took the top spot on our video leaderboard, ahead of Gemini 3.5 Flash. It was the best model we tested at tracking what happens in a scene over time.


For video we scored the models on a subset of VantageBench and VideoNet, two public video-understanding benchmarks. We are also building our own video evals, since there aren't many strong public ones.


Video leaderboard, best overall.


## Pricing and Price-Performance of Gemini 3.6 Flash


Gemini 3.6 Flash is also cheaper to run than the model it follows. In our tests it cost less per image, ran faster, and used fewer tokens than Gemini 3.5 Flash. You get similar image quality, and better video, for less.


Model Input Output Notes


Gemini 3.5 Flash $1.50 / MTok $9 / MTok Best on object detection


Gemini 3.6 Flash $1.50 / MTok $7.50 / MTok Cheaper, faster, best on video


Gemini 3.5 Flash-Lite $0.30 / MTok $2.50 / MTok Fastest, weak at reasoning


## When to Use Gemini 3.6 Flash for Vision


Gemini 3.6 Flash is a good default when you need general image understanding, data extraction, counting, or video understanding at a lower cost. For most of those jobs it matches or beats Gemini 3.5 Flash while costing less.


The one place to skip it, or any general vision model, is object detection when you need tight, reliable boxes. For that, train a fine-tuned[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) model on your own data and run it in[Roboflow Workflows](https://roboflow.com/workflows/build?ref=blog.roboflow.com) . It beats frontier vision models on detection accuracy, at a fraction of the cost and latency, and it returns clean, structured output every time.


Compare Gemini 3.6 Flash against every model we have benchmarked, and test it on your own images, on[Roboflow Playground Evals](https://playground.roboflow.com/evals?ref=blog.roboflow.com) .


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Erik Kokalj](https://blog.roboflow.com/author/erik/) . (Jul 22, 2026). Gemini 3.6 Flash for Vision: Evaluation and Benchmarks. Roboflow Blog: https://blog.roboflow.com/gemini-3-6-flash-for-vision/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Erik Kokalj


Developer Experience @ Roboflow


[View more posts](https://blog.roboflow.com/author/erik/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Model Evaluation](https://blog.roboflow.com/tag/model-evaluation/)

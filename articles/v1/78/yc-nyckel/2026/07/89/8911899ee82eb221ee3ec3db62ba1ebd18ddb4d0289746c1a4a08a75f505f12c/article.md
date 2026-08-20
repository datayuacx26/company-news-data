---
schema_version: "1.0.0"
document_id: "8911899ee82eb221ee3ec3db62ba1ebd18ddb4d0289746c1a4a08a75f505f12c"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/dinov2-benchmarks/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:ac68861bba68d1a8e11e7e3d3d001323c5b0d9f59f9d08fcf4f9560e94e6f9f3"
---

# DINOv2: Benchmarking Performance Across 117 Datasets

Picking the right ML model architecture is essential for achieving target accuracy. With so many options available, the process can feel overwhelming, but performance benchmarks provide a practical way to narrow down choices and focus on what delivers results.


In this article, we spotlight one model in particular:[DINOv2](https://dinov2.metademolab.com/) . By benchmarking its performance across 117[image classification](https://www.nyckel.com/blog/image-classification/) datasets, we evaluate how it compares to other leading models.


## What is DINOv2


Released in Spring 2023, Meta AI’s DINOv2 builds on the Vision Transformer architecture to power self-supervised learning. It’s trained on a pool of 142 million images and enables use cases like image classification, depth estimation, and semantic segmentation—all without the need for task-specific fine-tuning.


DINOv2 includes a whole suite of models. In this post we use the base VIsualTransformer model with the extra registers:` DINOv2-vitb14-reg` .


## Who is Nyckel


[Nyckel](https://www.nyckel.com/) is an[AutoML platform](https://www.nyckel.com/blog/nyckel-redefining-automl/) that makes it easy for anyone to build custom ML models without needing a PhD. The Nyckel platform auto-tests your dataset against hundreds of model architectures - including DINOv2 - ensuring your ML model delivers the highest accuracy possible.


Nyckel publishes these[benchmarks](https://www.nyckel.com/blog/image-classification-benchmark/) to help others understand the efficacy of popular models, which can be useful for prioritizing models to test.


## Methodology


We ran the benchmark on 117 image classification datasets from our production database. We used transfer learning, where the models were used as feature extractors on top of which we then trained and evaluted logistic regression classifiers.


In addition to the DINOv2 model, we used three[CLIP](https://github.com/openai/CLIP) models: Their basic ViT model:` CLIP-ViT-B/32` , a larger ResNet based convolutional models:` CLIP-RN50x4` , and the same ResNet model but modified for larger resolution inputs:` CLIP-RN50x4-highres` . The results below highlight how often DINOv2 “won” over these models, and, when it did win, the incremental accuracy improvement.


When we say a model ‘won,’ we mean it achieved the highest accuracy among all the architectures tested. Incremental improvements are expressed as absolute percentage point increases; for example, a 5% increase means accuracy improved from 80% to 85%.


## Results


Across the 117 datasets, DINOv2 was the best model in roughly 20% of cases. Of the remaining, we had:


- CLIP-RN50x4-highres: 46%
- CLIP-RN50x4: 27%
- CLIP-ViT-B/32: 7%


Analyzing DINOv2’s percentage improvements in successful cases revealed:


- 10 out of 23 datasets showed 0%-1% improvement over the next-best model
- 7 saw 1%-5% gains
- 6 saw over 5% gains


## What the results tell us


These numbers highlight that DINOv2 can deliver meaningful improvements (5% of all datasets saw a 5%+ accuracy increase), although it probably shouldn’t be the first model you test. Still, we recommend adding it to your shortlist for image classification testing.


Additionally, when we manually reviewed the images in the models where DINOv2 won, we saw no clear patterns. There was no obvious reason why DINOv2 performed better for those datasets. This underscores why[AutoML](https://www.nyckel.com/blog/automl-machine-learning/) is a powerful tool versus intuition-based model selection. The more models you test, the higher likelihood you find the architecture that can deliver the best accuracy.


## How to find the right model for your data


We highly recommend using an AutoML tool to build your ML model. By testing hundreds of models at once, you’ll ensure you’re using the right model architecture for any given dataset.


If you’d like to hear more about Nyckel - who can help you reach your target accuracy in just minutes - please reach out to us[here](https://www.nyckel.com/contact/) .

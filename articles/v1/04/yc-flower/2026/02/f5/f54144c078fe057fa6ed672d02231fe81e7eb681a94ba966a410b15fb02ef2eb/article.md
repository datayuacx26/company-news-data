---
schema_version: "1.0.0"
document_id: "f54144c078fe057fa6ed672d02231fe81e7eb681a94ba966a410b15fb02ef2eb"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-02-09-announcing-flower-datasets-0.6.0-release"
published_at: "2026-02-09T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T22:21:23.355424+00:00"
content_hash: "sha256:e2825eaf671cc11f9d4fb93b2b50879f8ea42bc051bbdd8e5b96ab0c1e95cc5c"
---

# Announcing Flower Datasets 0.6.0

The Flower Team is excited to announce the release of Flower Datasets 0.6.0!


Flower Datasets ( flwr-datasets


) is a library designed to quickly and easily create datasets for federated learning, federated evaluation, and federated analytics.


## Thanks to our contributors


We would like to give our special thanks to all the contributors who made this new version of Flower Datasets possible (in git shortlog


order):


Adam Tupper


, Alireza Ghasemi


, Daniel Hinjos García


, Haoran Jie


, Heng Pan


, Iason Ofeidis


, Javier


, Yan Gao


## What's new?


-


**Introduce Flower Datasets CLI** ([#6514](https://github.com/flwrlabs/flower/pull/6514) ,[#6520](https://github.com/flwrlabs/flower/pull/6520) )


Run flwr-datasets


in your terminal to access a new CLI. The first command, create


, lets you quickly generate federated datasets from Hugging Face datasets and save them to disk. This is perfect for creating demo data to test Flower SuperNodes


.[Learn more](https://flower.ai/docs/datasets/how-to-generate-demo-data-for-deployment.html) .


-


**Add ContinuousPartitioner** ([#5235](https://github.com/flwrlabs/flower/pull/5235) )


ContinuousPartitioner


enables non-IID partitioning based on real-valued dataset properties with adjustable strictness. It interpolates between IID and non-IID partitioning using a strictness parameter that blends standardized property vectors with Gaussian noise.


-


**New Recommended Datasets** ([#6483](https://github.com/flwrlabs/flower/pull/6483) ,[#4966](https://github.com/flwrlabs/flower/pull/4966) ,[#4853](https://github.com/flwrlabs/flower/pull/4853) )


- Add FedJam


multimodal federated dataset: A 36k sample dataset featuring spectrogram images and time-series KPIs for wireless jamming classification
- Add CareQA


medical benchmark: A dataset of 5,621 QA pairs from official Spanish healthcare exams (2020–2024) for medical LLM evaluation
- Add two ChEMBL


molecular datasets to the recommended FL datasets list


-


**Bug Fixes** ([#5340](https://github.com/flwrlabs/flower/pull/5340) )


- Fix NaturalIdPartitioner


and DirichletPartitioner


producing inconsistent partition assignments across dataset splits by sorting unique IDs/classes before mapping. This issue occurred when partitioning different splits (e.g., train/test) or datasets with different orderings.


-


**Bump datasets dependency** ([#6126](https://github.com/flwrlabs/flower/pull/6126) )


The datasets


dependency now supports versions 4.x


(previously only up to 3.1.0


).


-


**Improved Documentation** ([#5948](https://github.com/flwrlabs/flower/pull/5948) ,[#6127](https://github.com/flwrlabs/flower/pull/6127) )


-


**General Improvements** ([#6126](https://github.com/flwrlabs/flower/pull/6126) ,[#6132](https://github.com/flwrlabs/flower/pull/6132) ,[#6131](https://github.com/flwrlabs/flower/pull/6131) ,[#4854](https://github.com/flwrlabs/flower/pull/4854) ,[#5201](https://github.com/flwrlabs/flower/pull/5201) ,[#5989](https://github.com/flwrlabs/flower/pull/5989) ,[#5248](https://github.com/flwrlabs/flower/pull/5248) ,[#6543](https://github.com/flwrlabs/flower/pull/5653) )

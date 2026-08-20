---
schema_version: "1.0.0"
document_id: "b675255e5a9e89feba66aaadd8c5156ac06a1c13b0b5c2f81e76ed9470da59c7"
company_key: "yc-lancedb"
company: "LanceDB"
source_id: "yc-lancedb-news-import-bc01535eeacf"
canonical_url: "https://www.lancedb.com/blog/volcano-engine-autonomous-driving-data-lake-solution"
published_at: null
first_seen_at: "2026-07-24T14:00:34.339937+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:af51bfdb1b30344e3257b25480214691b126e5963040ac1dda9c183008c9d85e"
---

# Volcano Engine LAS's Lance-Based PB-Scale Autonomous Driving Data Lake Solution

> **💡 Contributed by Bytedance**
> This case study was contributed by Bytedance Volcano Engine LAS (Lake for AI Service) Team


As autonomous driving technology proliferates, unstructured data like camera-captured images, LiDAR-generated point clouds, and microphone-collected audio surges. This massive, diverse, and real-time data demands underlying storage and processing technologies with high efficiency and speed.


Volcano Engine’s multimodal data lake solution is an intelligent data infrastructure for the AI era, comprehensively covering lake computing, storage, management, and analytics. The LAS (Lake for AI Service) enables unified, fine-grained management of unstructured data assets (text, images, audio/video) while providing end-to-end intelligent data services for model pre-training, post-training, and AI application development.


Recently, Volcano Engine LAS has been applied and implemented in autonomous driving scenarios. This article focuses on the "auto driving" scenario, detailing how LAS’s core storage format—[Lance](https://www.lance.org/) , the open lakehouse format for multimodal AI—rapidly constructs a next-gen AI data lake to efficiently store, manage, and process multimodal data (text, images, audio/video).


## Background


**Client A** is a leading Chinese automotive enterprise specializing in Intelligent Connected Vehicle (ICV) scenarios. This solution addresses their challenges in managing and processing massive multimodal data (text/images/point clouds) by leveraging the Lance-based AI data lake. Breakthroughs are achieved through three core technologies:


1. **Zero-Cost Data Evolution:** Adding new data columns for dynamic labeling without rewriting historical datasets, reducing storage costs by 30%.
2. **Transparent Compression:** ZSTD encoding achieves 70% compression for point cloud data, minimizing network bandwidth pressure.
3. **Point Query Optimization:** Column projection and lightweight shuffle boost training efficiency, reaching 96% GPU utilization.


Deployed at an automotive client, the solution improved EB-scale data processing efficiency by 3× and accelerated model training delivery by 40%. Below are technical details.


## Challenges


**Client A faced:**


- **Data Explosion:** Real-time multimodal data collection (cameras, LiDAR) generates TBs/day per test vehicle, scaling to EB-level with mass production. Unstructured data (e.g., driving videos) require conversion to structured insights (object detection, path planning).
- **Core Issues:**


- **Storage:** Reduce costs without compromising point/range query performance.
- **Compute:** Efficiently scale from single-node experiments to production engineering.
- **Retrieval:** Quickly extract business value from massive unstructured data.
- **Management:** Track data pipelines for continuous optimization.


## Solution Architecture: Lance-Driven Upgrade


### Advantage 1: Data Mining & Management


- **Pain Point:** LMDB required full dataset rewrite for new label columns, causing storage bloat and GPU waste.
- **Lance Solution:** Unified metadata management supports incremental updates.


### Advantage 2: Model Training Optimization


- **Pain Point:** Traditional methods suffered I/O amplification and memory bloat, limiting GPU utilization to 60%.
- **Lance Solution:** Point queries enable lightweight data shuffle and column projection, reading only essential fields.


## Lance Core Advantages


### Zero-Cost Data Evolution


In intelligent driving scenarios, data annotation precision determines the upper limit of the model. Lance offers **zero-cost data evolution** that robustly supports dynamic annotation scenarios:


- **Automatic traffic element annotation** : Traffic lights, road signs, etc.
- **Dynamic actor annotation** : Pedestrian and vehicle trajectories.
- **Environmental condition annotation** : Lighting, precipitation, visibility.


When fine-tuning models using datasets corresponding to specific scenarios, it's necessary to filter datasets for particular scenarios based on certain labels. This requires label data, such as whether an image depicts cloudy weather or includes pedestrians.


The automatic labeling process for these tags essentially involves adding columns to the dataset.


Traditional methods (such as LMDB or Pickle) require rewriting the entire dataset when adding new columns, consuming significant resources. Lance, however, supports rapid schema evolution through Manifest metadata. It's very light and high-performance.


**Client Results:**


- 50% inference throughput gain (8×A100 GPU: 60% → 90%).
- 3× end-to-end efficiency (10PB label processing: 4 days → 1 day).


### Transparent Compression


Lance uses ZSTD encoding for high compression ratios on point clouds/labels, reducing storage/bandwidth transparently.


Moreover, Lance's compression is defined in the schema, making it transparent and imperceptible during data writes or reads, thus significantly improving usability.


Data Type Raw Volume Lance Compressed Bandwidth Saved


Camera (images) 10 PB 9.5 PB 5%


LiDAR (point clouds) 1 PB 300 TB 70%


Annotation metadata 1 TB 600 GB 40%


## **Point Query for AI Training**


Lance optimizes training bottlenecks:


Dimension Legacy (LMDB) Lance


Data Shuffle Manual index construction Light weight shuffle via row index


Column Pruning I/O amplification (Pickle + LMDB) Column projection; no I/O waste


Stability Memory bloat from file handles No memory bloat, no handle management


## Conclusion


Lance achieves breakthroughs in auto driving data management, training efficiency, and cost optimization. With Zero-Cost Data Evolution, transparent compression, and point queries, clients gain 3× PB-scale data processing efficiency and >90% GPU utilization.


**Join the Lance community to build next-gen AI data infrastructure!**


- Documentation:[www.lance.org](http://www.lance.org/)
- Discord:[discord.gg/lance](http://discord.gg/lance)

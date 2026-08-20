---
schema_version: "1.0.0"
document_id: "5e122a4616160cace63c5c482eacc0f0630a9f40158d6b3188a365e25c29c265"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/core-ml-performance-2022"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:bcf9d3c4cbd12049d29610e29574e5c699f0c3b498d49a06a704a38f21fc322c"
---

# Core ML performance benchmark iPhone 14 (2022)

Today is new-iPhone day! At Photoroom, this means today is CoreML-benchmark day. Every year we cannot wait to discover what the improved computing power of the new hardware means for on-device machine learning.


CoreML allows iOS developers to ship and execute machine learning models, allowing us to build complex interactive experiences and building an image editor that allows users to think about objects, not pixels. In recent years, the possibility to run our models on the Apple Neural Engine (ANE) has dramatically sped up CoreML.


This year, we decided to spice things up a bit by analyzing performances not only across iPhone models but also across iOS versions.


## Setup


This year we ran a benchmark analyzing the performance of our guided cutout pipeline illustrated above.


We ran this benchmark on the following devices across several OSes:


-


iPhone 12 Pro *A14 Bionic* (iOS 15 + iOS 16)


-


iPhone 13 Pro *A15 Bionic* (iOS 15 + iOS 16)


-


iPhone 14 Pro *A16 Bionic* (iOS 16)


-


iPad Pro 2021 *M1* (iOS 15 + iOS 16)


-


MacBook Pro 2021 *M1 Pro* (macOS 12)


---


For each device, we gathered the average execution time (excluding model-loading time) depending on the CoreML compute units configuration (` MLComputeUnits` ):


-


` cpuOnly` (the model exclusively runs on the CPU)


-


` cpuAndGPU` (the model runs on the GPU as much as possible, and defaults to the CPU when a layer cannot run on the GPU)


-


` cpuAndNeuralEngine` (iOS 16 only, the model runs on the ANE as much as possible, and defaults to the CPU when a layer cannot run on the ANE)


-


` all` (the model runs on the ANE, GPU or CPU depending on each unit's ability)


Each device, OS version and compute unit configuration was measured 40 times and averaged, with some cooldown time in between to avoid the effects of thermal throttling on the SoC.


## Results / Analysis


The results above reveal a few interesting takeaways:


-


The consistent speed increase going from CPU to GPU to ANE showcases that most of our model is able to run on both the GPU and the ANE


-


That being said, the (nearly) consistent increase in inference time when going (on iOS 16) from` all` to` cpuAndNeuralEngine` showcases that a few number of layers in our model are not able to run on the ANE, but instead default to the GPU.


-


The OS version seems to have a negligible impact on overall performance. Do not expect a speed bump by upgrading your OS.


-


Across the A-series chips (A14 Bionic - A15 Bionic - A16 Bionic) we can see a slow and steady improvement in performances in all configurations. In fact, Apple touted 17 TFlops in its new A16 Bionic chip, up 7.5% from the 15.8 TFlops of the A15 Bionic, which is consistent with the` all` configuration going from 45ms to 41ms in between the iPhone 13 Pro and the iPhone 14 Pro.


-


Despite being based on the A14 cores, the M1 chip of the iPad pro is neck and neck on CPU and ANE compared to the new A16 Bionic, and blows it away on the GPU. Of course, this can be explained by both the M1 chip having more GPU cores than the A16 Bionic, and possibly the iPad Pro having a much larger thermal envelope.


-


Interestingly, the M1 Pro chip in the MacBook Pro does not fare much better than the M1 chip in the iPad Pro, and even performs significantly worse in the ANE. We would love to hear a compelling explanation as to why this is the case.


## Conclusion


So far, Apple is unparalleled as far as giving mobile developers tools for on-device machine learning goes. While today's new hardware release is not a ground-breaking innovation, it is the latest data-point in a series of consistent improvements year-over-year. At 41ms per inference on the new iPhone 14 Pro, we are finally over the 24 FPS mark, the standard framerate of cinema (for high-quality, user-guided segmentation!). But if you are looking for the best performance available on models unable to run on the ANE, the latest M1 iPad Pro is still your most trusted ally.

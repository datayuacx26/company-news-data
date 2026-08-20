---
schema_version: "1.0.0"
document_id: "2b14dfd872513567c78e1530d54486473a676df78bfe70b1772f125a240aacb3"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/core-ml-performance-benchmark-2023-edition"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:0466138fd7840a6ceb1ba75ac0614986c114603642071b97ff3d38a4ac204f22"
---

# Core ML performance benchmark iPhone 15 (2023)

Last friday was new-iPhone day! Here at Photoroom, it's a tradition to kick off the iPhone year with our CoreML benchmark. Every year, we eagerly look forward to discovering how the increased computational prowess of the latest iPhone hardware will impact on-device machine learning.


CoreML has been a game-changer for iOS developers, allowing us to seamlessly incorporate and run machine learning models. This results in the creation of intricate interactive experiences, such as[our image editor](https://apps.apple.com/fr/app/photoroom-effaceur-de-fond/id1455009060) , which makes it easy for users to work with entire objects instead of getting caught up in the details of pixels. The evolution of the Apple Neural Engine (ANE) has been paramount in significantly boosting CoreML's performance.


*An example of how one can leverage powerful AI to reinvent image editing*


## Setup


This year, we are again running an object-cutout benchmark, analyzing the performances of one of our core AI model (guided cutout) across some of the most recent Pro-rated Apple devices, showcasing the evolution of Apple’s SoCs:


-


iPhone 12 Pro: *A14 Bionic*


-


iPhone 13 Pro: *A15 Bionic*


-


iPhone 14 Pro: *A16 Bionic*


-


iPhone 15 Pro: *A17 Pro*


-


iPad Pro 2021: *M1*


-


MacBook Pro 2021: *M1 Pro*


-


MacBook Pro 2023: *M2 Max*


[Last year](https://www.photoroom.com/inside-photoroom/core-ml-performance-2022) we concluded that the iOS version has little impact on inference speed, so to keep the setup simple this year, we ran all tests under iOS 17.


---


For each device, we gathered the average inference time (excluding model-loading time) depending on the CoreML compute units configuration (` MLComputeUnits` ) or our pipeline:


-


` cpuOnly` (the model exclusively runs on the CPU)


-


` cpuAndGPU` (the model runs on the GPU as much as possible, and defaults to the CPU when a layer cannot run on the GPU)


-


` all` (the model runs on the ANE, GPU or CPU depending on each unit's ability)


This setup allows us to measure the evolution in performance of different parts of the SoC.


Each device was measured 40 times and averaged, with some cooldown time in between to avoid the effects of thermal throttling on the SoC. The benchmark is of course compiled in release mode with all optimizations enabled.


## Results / Analysis


Here are the raw numbers:


And a few visual representations:


From those, we can glean a few interesting observations:


-


The CPU and GPU improvements in inference time between the iPhone 14 Pro and the 15 Pro are consistent with Apple’s claims of a 10% and 20% (respectively) performance boost. If your model is not running on the ANE, you can expect a solid incremental enhancement.


-


Similarly, the new Apple Neural Engine marks a good step forward in on-device machine learning capabilities, with a 16% improvement in our benchmark compared to its predecessor. The claims of the new ANE in the A17 chip being able to handle 35 TFlops (compared to 17 TFlops of the A16) seem -if accurate- to not directly translate into the leap forward in on-device machine learning that one would expect. That being said, incremental updates do compound with time: in just a few years, the iPhone has more than doubled its capabilities for on-device machine learning, with our specific model showcased here running jumping from 12 FPS on the iPhone 12 Pro to 27 FPS on the iPhone 15 Pro.


-


As with last year, the results have made one thing abundantly clear: the` all` configuration consistently outperforms the rest, on all devices. Given this, it's imperative for developers to optimize their machine learning models to run on the Apple Neural Engine (ANE) as much as possible,[as show here](https://www.youtube.com/watch?v=SSMXnzLWcJM) . Leveraging the capabilities of the ANE can translate to significant performance gains, streamlining user experiences and making applications more efficient.


## Conclusion


In conclusion, the iPhone 15 Pro signifies a solid incremental enhancement in the arena of on-device machine learning, courtesy of its improved ANE. While it may not be a colossal leap, it undoubtedly marks a steady and commendable progression. We keenly anticipate these advancements extending to other Apple offerings such as the iPad and the M-series Macs. Moreover, the potential of these enhancements reaching the non-Pro iPhone models augments the excitement.

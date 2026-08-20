---
schema_version: "1.0.0"
document_id: "48b3532e684ad95e299183919679dbbe73b0fcf14835f08b6b9bab27d1235fb1"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/articles/nvidia-b200-gpu-price-guide"
published_at: "2025-08-11T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:09cea4d6437fced1d70e914b044ac3012b79ca4b1c971541b023796a6b054d35"
---

# NVIDIA B200 GPU Price Guide

The NVIDIA[B200 GPU](https://www.nvidia.com/en-us/data-center/dgx-b200/) , announced on March 18th, 2024, is NVIDIA's first processor to make use of their new[Blackwell architecture](https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/) .


According to the latest[MLPerf benchmarks](https://mlcommons.org/benchmarks/) , the Blackwell B200 boasts up to a **2.6x increase in raw training performance** , and up to a staggering **4x increase in raw inference performance** compared to the previous generation Hopper architecture.


## Specs & Performance - NVIDIA B200 vs. H200


The most significant difference between the NVIDIA B200 and the NVIDIA H200 is the Blackwell chip's "dual-die" design allowing for a total of 208 billion transistors, more than double the H200's 80 billion.


The NVIDIA B200 also benefits from the following upgrades:


- **192 GB of VRAM** , supporting slightly larger model sizes over the H200's 141 GB.
- **8 TB/s memory bandwidth** , double the H200's 4 TB/s.
- **5th generation Tensor cores** , resulting in better performance for mixed precision workloads vs. the H200's 4th gen Tensor cores.
- **Native FP4 and FP6 support** , courtesy of the new second generation Transformer engine.
-


- The H200 only supports FP8, FP16 and BF16 (also supported by the B200).


- **NVLink 5 with 1.8 TB/s of GPU-to-GPU bandwidth** , doubling the interconnected bandwidth capacity for better multi-GPU scaling.


## B200 GPU Pricing - Cloud Deployments


The following is a look at cloud rental options for B200 GPUs across a number of providers, both those that are listed on Shadeform's marketplace and those that aren't.


Provider


B200 x 1


B200 x 2


B200 x 4


B200 x 8


Available on Shadeform


Hydra Host


n/a


n/a


n/a


[$34.80/hr](https://platform.shadeform.ai/launch?cloud=whitefiber&shadeInstanceType=B200x8&region=blonduos-iceland-1)


yes


Verda Cloud


[$4.90/hr](https://platform.shadeform.ai/launch?cloud=verda&shadeInstanceType=B200&region=helsinki-finland-5)


[$9.80/hr](https://platform.shadeform.ai/launch?cloud=verda&shadeInstanceType=B200x2&region=helsinki-finland-5)


[$19.60/hr](https://platform.shadeform.ai/launch?cloud=verda&shadeInstanceType=B200x4&region=helsinki-finland-5)


[$39.20/hr](https://platform.shadeform.ai/launch?cloud=verda&shadeInstanceType=B200x8&region=helsinki-finland-5)


yes


WhiteFiber


n/a


n/a


n/a


[$44.00/hr](https://platform.shadeform.ai/launch?cloud=whitefiber&shadeInstanceType=B200x8&region=blonduos-iceland-1)


yes


RunPod


$5.99/hr


$11.98/hr


$23.96/hr


$47.92/hr


no


Modal (Serverless)


$6.25/hr


n/a


n/a


n/a


no


Baseten (Serverless)


$9.98/hr


n/a


n/a


n/a


no


## B200 GPU Pricing - On-Premise Deployments


If you want to purchase B200 GPUs for an on-premise deployment, you'll have to buy them in groups of 8 GPUs for[~$373,000](https://viperatech.com/product/nvidia-umbriel-b200-baseboard-1-5tb-hbm3e) , or $46,625 per chip–––NVIDIA doesn't sell the B200 in a single GPU package.


## Which Use Cases is the B200 Best Suited For?


As of this writing, the NVIDIA B200 is at the bleeding edge of compute performance, making it capable of running almost any workload you could think of.


However, it's particularly well-suited to large scale, compute-heavy and latency-sensitive workloads that would strain H200 based systems.


Examples include:


- **Training frontier-scale foundation models** like GPT-5 class LLMs, multi-modal AI, and trillion parameter plus models.
-


- In addition to the raw performance increases, the doubled interconnect bandwidth makes the B200 perform exponentially better at the largest scales of AI training.


- **Serving massive models to large user bases** , i.e. chat assistant platforms, voice AI call centers, and AI image and video creation platforms.
-


- At high scales of concurrent users, the B200's native FP4/FP6 support, larger interconnect bandwidth and high inference throughput make it the best option on the market for providing large amounts of users with a low latency experience.


- **Time-sensitive AI applications** like fraud detection and drug discovery.
-


- The B200's ultra-low precision support and leading high memory bandwidth make it the best available option for time sensitive applications that require the fastest time-to-results.


## Find the Best B200 Prices on Shadeform


With Shadeform, you can compare and deploy GPUs from over 20 different providers in one platform. You'll have access to the best prices across 150+ regions.


Think the B200 is right for your workloads?[Browse available deployment options](https://platform.shadeform.ai/) on the Shadeform marketplace.

---
schema_version: "1.0.0"
document_id: "f24684f1657cd15f4e9a59833aeaefcbf04deead3ffae7f0787ecbadd3a7373f"
company_key: "super-micro-computer-inc-common-stock"
company: "Super Micro Computer Inc."
source_id: "super-micro-computer-inc-common-stock-rss-44766a8a0f56"
canonical_url: "https://learn-more.supermicro.com/data-center-stories/supermicro-nvidia-blackwell-systems-linear-scalability-mlperf-training"
published_at: "2026-06-18T01:02:20+00:00"
first_seen_at: "2026-07-20T04:35:54.594017+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:7e373e677dea17cbc93c01e89f5668b7ddb615f107692d0579b205a9187adf3a"
---

# Supermicro NVIDIA Blackwell Systems Demonstrate Linear Scalability for MLPerf Training v6.0

# Supermicro NVIDIA Blackwell Systems Demonstrate Linear Scalability for MLPerf Training v6.0


Written by:[Supermicro Experts](https://learn-more.supermicro.com/data-center-stories/author/supermicro-experts) |


1 min read


[AI](https://learn-more.supermicro.com/data-center-stories/tag/ai?hsLang=en)


In the latest MLPerf Training V6.0 benchmarking effort, Supermicro demonstrated linear scaling in training performance for 3 different models, scaling from 8 GPU to 72 B300 GPUs. Additional runs on HGX B200 systems were run, which showed that B300 performs better than B200 in Training performance.


The following systems with NVIDIA Blackwell GPUs were used in the benchmarking effort.


**Supermicro System**


**# nodes**


**CPU SKU**


**# CPU**


**GPU SKU**


SRS-GB300-NVL72-M1


(18x ARS-121GL-NB3)


Liquid Cooling


18


Neoverse-V2 (Grace)


36


NVIDIA Blackwell Ultra GPU (GB300)


AS-8126GS-NB3RT


Air Cooling


2


AMD EPYC 9575F


4


NVIDIA Blackwell Ultra GPU (B300-SXM-270GB)


AS-4126GS-NB3RT-LCC


Liquid Cooling


1


AMD EPYC 9575F


2


NVIDIA Blackwell Ultra GPU (B300-SXM-270GB)


SYS-222GS-NB3OT-ALC


Advanced Liquid Cooling


1


Intel(R) Xeon(R) 6776P


2


NVIDIA Blackwell Ultra GPU (B300-SXM-270GB)


SYS-422GS-NB3RT-LCC


Liquid Cooling


1


Intel(R) Xeon(R) 6776P


2


NVIDIA Blackwell Ultra GPU (B300-SXM-270GB)


AS-4126GS-NBR-LCC


Liquid Cooling


1


AMD EPYC 9965


2


NVIDIA Blackwell GPU (B200-SXM-180GB)


SYS-A22GA-NBRT


Air Cooling


1


Intel(R) Xeon(R) 6979P


2


NVIDIA Blackwell GPU (B200-SXM-180GB)


In the LLAMA 3.1 8b benchmark, 2 systems of HGX-B300 showed even better performance than the linear scaling for HGX-B300 to NVL72, due to the optimal use of networking for this small model. In the bigger LLAMA 3.1 70b model LORA, the 2 systems of HGX-B300 follow the linear scaling of the HGX-B300 to NVL72.


Overall, Supermicro demonstrates the linear scaling capability of the NVIDIA Blackwell GPU architecture that uses NVLINK with performance that scales linearly from 8 GPU B300 (HGX-B300) to 72 GPU B300 (NVL72).


### Subscribe to Data Center Stories


By clicking subscribe, you consent to allow Supermicro to store and process the personal information submitted above to provide you the content requested.


You can unsubscribe from these communications at any time. For more information on how to unsubscribe, our privacy practices, and how we are committed to protecting and respecting your privacy, please review our[Privacy Policy](https://www.supermicro.com/en/about/privacy-policy) .

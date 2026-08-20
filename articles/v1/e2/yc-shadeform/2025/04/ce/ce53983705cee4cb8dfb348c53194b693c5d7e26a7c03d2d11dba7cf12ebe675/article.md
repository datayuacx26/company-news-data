---
schema_version: "1.0.0"
document_id: "ce53983705cee4cb8dfb348c53194b693c5d7e26a7c03d2d11dba7cf12ebe675"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/articles/nvidia-h200-gpu-price-guide"
published_at: "2025-04-28T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:149db22a8cc652b437279ab444023497be9df313301b806ee4db6110206c9616"
---

# NVIDIA H200 GPU Price Guide

The NVIDIA[H200 GPU](https://www.nvidia.com/en-us/data-center/h200/) , announced on November 13th, 2023, is the latest processor based on NVIDIA’s[Hopper architecture](https://www.nvidia.com/en-us/data-center/technologies/hopper-architecture/) , offering a major upgrade in memory capacity and inference performance compared to the[H100](https://www.nvidia.com/en-us/data-center/h100/) .


## Specs & Performance - NVIDIA H200 vs. H100


Because the H200 shares the same Hopper architecture as the H100, they also share some specifications; the 80B transistors of the H100 remain unchanged on the H200.


The primary upgrades of the H200 are instead in its memory capacity and memory bandwidth, introduced specifically to address the increasing size of SOTA model releases.


The NVIDIA H200 offers:


- **141GB of VRAM** , nearly double the H100’s 80GB, enabling much larger model deployments.
- Up to **4.8 TB/s memory bandwidth** , a 43% increase over the H100’s 3.35 TB/s.


As a result, you can expect **inference performance increases between 1.6x to 2x** , with larger models exhibiting the greatest improvements.


## Which Use Cases is the H200 Best Suited For?


Thanks to its expanded memory and faster memory bandwidth, the H200 is particularly well suited for high-end inference tasks that require massive models and long context windows, or are otherwise highly memory intensive.


Examples include:


- Serving 100B+ parameter LLMs like Deepseek R1, Mistral Large, and Llama 4 Behemoth.
- Batch serving for high volumes of inference requests.
- Retrieval-Augmentation Generation (RAG) pipelines with large vector databases.
- Complex scientific applications like protein modeling and particle physics simulations.


## H200 GPU Pricing - Direct Purchase


If you want to purchase an H200 for yourself, it will cost you[~$30,000](https://viperatech.com/shop/nvidia-h200-nvl-graphic-card-141-gb-passive-pcie-900-21010-0040-000/) per chip.


However, H200s are usually bought in multiples of 8 and assembled into reference architectures like the[NVIDIA DGX SuperPOD](https://www.nvidia.com/en-us/data-center/dgx-h200/) .


Full node setups can range from ~300,000 to several million dollars, depending on scale.


## H200 GPU Pricing - Renting On-Demand in the Cloud


A more flexible and cost-efficient way to access H200 GPUs is through on-demand cloud rental or GPUaaS providers.


On-demand H200 rentals are hard to come by due to the limited nature of these chips, however, these providers allow you to rent H200s by the hour for remote access.


Provider


H200 x 1


H200 x 2


H200 x 4


H200 x 8


Available on Shadeform


Verda Cloud


$3.37


$6.74


$13.48


$26.95


yes


Boost Run


n/a


n/a


n/a


$26.00


yes


Nebius


$3.65


n/a


n/a


$28.44


yes


RunPod


$3.99


$7.98


$15.96


$31.92


no


## Find the Best H200 Prices on Shadeform


With **Shadeform** , you can compare, deploy, and manage H200 GPUs from multiple cloud providers in a single platform. You’ll have access to the best prices across 150+ global regions.


Think the H200 is right for your AI workloads?[Start deploying](https://platform.shadeform.ai/) in minutes with Shadeform.

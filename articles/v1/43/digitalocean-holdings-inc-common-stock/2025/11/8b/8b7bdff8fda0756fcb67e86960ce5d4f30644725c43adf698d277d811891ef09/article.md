---
schema_version: "1.0.0"
document_id: "8b7bdff8fda0756fcb67e86960ce5d4f30644725c43adf698d277d811891ef09"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/now-available-gpu-doks-observability"
published_at: "2025-11-12T20:56:52.577+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:25:17.571501+00:00"
content_hash: "sha256:d3eb6d367bb5f6fc97db45fb2c53ab7c506e5c267f6a136517b23e6264ee9c86"
---

# GPU Observability: Get Deeper Insights into Your Droplets and DOKS Clusters

We’re introducing a new set of[basic observability](https://docs.digitalocean.com/products/monitoring/details/availability/#gpu-observability-availability) metrics for all GPU Droplets and[DOKS clusters](https://docs.digitalocean.com/products/kubernetes/details/features/#gpu-observability-in-doks) , giving you a powerful, simple way to monitor and optimize your AI workloads.


## Why GPU Observability Matters


When running large-scale training, inference, and complex data processing—cluster performance and stability are paramount. Our new observability features are designed to give you the visibility you need to ensure effective utilization of your resources and quickly debug any performance bottlenecks.


Get real-time, individual metrics from your NVIDIA and AMD GPUs and their network interfaces on critical factors like utilization, temperature, power consumption, and more—all directly within the DigitalOcean Insights UI, and with zero setup required.


## What’s Included: New Metric Categories


We’ve grouped the new metrics into five intuitive categories to provide a comprehensive view of your GPU and DOKS cluster health and performance:


-


**Utilization:** Understand how busy your GPU cores and memory are. This includes key metrics like GPU Occupancy and Memory Utilization, allowing you to optimize your setup for peak performance live.


-


**Temperature:** Monitor thermal conditions to prevent overheating and ensure stable operation under heavy load.


-


**Power:** Track power consumption, which is essential for understanding GPU performance and efficiency.


-


**Throttle:** Identify if your GPU is limiting its performance due to thermal, power, or voltage constraints. This is crucial for debugging sudden performance degradations.


-


**Interconnect:** Gain insights into the network interface performance connecting your GPU resources.


## Zero Setup, No Extra Cost


Observability shouldn’t be a hurdle. That’s why we’ve made this feature as seamless as possible:


-


Default on: Observability will be enabled by default the moment you create a GPU Droplet. There is no configuration or effort required on your part.


-


Free: These essential observability metrics are included with the AI/ML Ready images for GPU Droplets.


We’re committed to continually improving the GPU experience and plan to add more advanced, differentiated features to our observability suite in the future.


## Benefits of GPU Droplets with DigitalOcean


-


Simplified Deployment: Our intuitive[platform](https://cloud.digitalocean.com/gpus/new?i=0a3f9f&size=gpu-h100x8-640gb&region=tor1&fleetUuid=a9cd094a-2263-467d-8215-fccde097ec88) makes it easy to provision and manage your AI infrastructure, allowing you to focus on developing your applications rather than managing complex setups.


-


Cost-Effectiveness:[GPU Droplets](https://www.digitalocean.com/products/gradient/gpu-droplets) start at $0.76/GPU/hour and we offer flexible configurations (including single and eight GPU options), helping you optimize costs for your specific use cases.


-


Seamless Integration: Leverage GPU Droplets with your existing DigitalOcean projects, integrating with our[Kubernetes](https://www.digitalocean.com/products/kubernetes) service.


-


Reliability: Benefit from enterprise-grade SLAs, HIPAA-eligibility and SOC 2 compliance, and the peace of mind that comes with building on DigitalOcean’s trusted cloud infrastructure.


[Start exploring](https://cloud.digitalocean.com/gpus/new?i=0a3f9f&size=gpu-h100x8-640gb&region=tor1&fleetUuid=a9cd094a-2263-467d-8215-fccde097ec88) your new GPU metrics today in the DigitalOcean Insights UI today and take control of your cluster’s performance.

---
schema_version: "1.0.0"
document_id: "4051c8950dbe9134da987f5035ae4cbf9c510db827bd7641ec8ec3aa1b0ae5a4"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/how-we-measured-the-co2-emissions-of-our-ai-models-at-inference-time"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:11c59dca5314b5fa3ed71744aca4cea4f1ec11ff569fde38865e29d3b514dda9"
---

# How we measured the CO2 emissions of our AI models at inference time

At Photoroom, we build AI models specialized in photo editing. In 2024, we decided to train our models on green energy and reduced our CO2 emissions by 1,000+ tons of CO2e per year by doing so. Our next step was to measure CO2 emissions during inference. Here's how we did it.


One thing to keep in mind is that, at inference stage, the following three incentives are aligned on the first order: improving user experience, reducing costs, and minimizing CO2 emissions. While most companies track execution time and GPU costs, few measure CO2 emissions.


## Methodology


We measure CO2 emissions in three steps:


1.


Estimate the energy consumed by computation


2.


Account for the energy efficiency of the data center


3.


Account for the carbon efficiency of the region


In the following, we will use the term *server* to refer to a group of GPUs hosting the same features. For example, one of our servers hosts AI Backgrounds, AI Shadows, AI Fill, AI Erase, AI Expand features.


### Energy consumed by computation


In the current infrastructure, it’s not possible to compute the exact amount of energy consumed per image processed on our servers for the simple reasons that each GPU hosts multiple AI features. Furthermore, we don’t have access to the power usage of the CPUs we use or the network.


We estimate energy consumption based on these assumptions:


-


**Energy consumption is proportional to execution time** (latency).


-


**Server energy consumption is equivalent to full GPU capacity.** While CPUs, RAM, and the network also consume power, we do not typically operate at full GPU capacity and estimate this assumption is accurate for Photoroom’s high GPU usage. It should be verified for other cases.


### Energy efficiency of the data center


The **PUE** **(Power Usage Effectiveness)** measures data center energy efficiency, specifically how much energy is used for non-IT operations (such as cooling and power distribution) compared to IT equipment (servers, storage, etc.).


Google Cloud’s Council Bluffs, Iowa, 2nd facility, has a PUE of[1.08](https://www.gstatic.com/gumdrop/sustainability/google-2024-environmental-report.pdf#page=79) , meaning 0.08 watts go to cooling and other operations for every 1 watt used by IT equipment, reflecting high efficiency. For reference, the worldwide average PUE is[1.56](https://www.statista.com/statistics/1229367/data-center-average-annual-pue-worldwide/) in 2024.


### Carbon efficiency of the region


**Carbon intensity** reflects CO2 emissions based on the electricity consumed by the datacenter.


The Greenhouse Gas (GHG) Protocol recommends dual reporting using both location-based and market-based emissions:


-


**Location-based emissions** : Use the grid’s average carbon intensity. Google Cloud’s location-based carbon intensity is[430 gCO2eq/kWh](https://cloud.google.com/sustainability/region-carbon#data) for us-central-1. For reference, the worldwide average was[481 gCO2eq/kWh](https://www.statista.com/statistics/1229367/data-center-average-annual-pue-worldwide/) in 2023.


-


**Market-based emissions** : Include renewable energy purchases. In North America, 64% of Google Cloud’s electricity purchases comes from carbon-free energy (["total renewable energy allocated” divided by “total electricity”](https://www.gstatic.com/gumdrop/sustainability/google-2024-environmental-report.pdf#page=77) ), setting the market-based carbon intensity to 36% of the location-based carbon intensity.


Also, Google Carbon-Free Energy (CFE)% tracks the percentage of carbon-free energy purchased at a local and hourly level. The CFE% for us-central-1 is[95%](https://cloud.google.com/sustainability/region-carbon#data) , a strong indicator of low environmental impact. However, we don’t use this method due to the lack of publicly shared calculation details.


## Calculation


So what do we need to compute CO2 emissions?


1.


Max power usage of the GPUs, in W


2.


The number of GPUs per servers


3.


The number of daily requests per endpoint and as such per server


4.


The latencies per endpoint and how they are served


5.


The data center PUE


6.


The region carbon intensity, in gCO2eq/kWh


The amount of daily consumed energy per server can be computed as follows:


We sometimes have multiple endpoints per GPU which can run in parallel. As such it’s hard to know exactly how much one endpoint consumes. What we can do is compute a contribution based on the latencies of each endpoint.


Let’s say we have features F1, F2, F3 served on the same GPU with latencies L1, L2, L3 and number of daily requests N1, N2, N3. We can define a pro rata factor:


Which then gives us a new estimated number of requests in the referential of the slowest endpoint :


Given the daily energy consumed by the GPUs on which those endpoints are served and the number of GPUs used we get the energy consumed per feature and per day:


To get the energy consumed per request, we can do:


To get an estimation of the CO2 emissions per request we can then do :


## Results


For confidentiality reasons, we do not disclose the specific GPU models we use, the exact volume of requests processed, or the latency of our models. We can share that we process millions of images daily, ensuring a statistical robustness to our calculation.


Based on our methodology, we can share the following estimates:


-


**Energy consumed by computation**


-


Remove background model consumes **0.027 Wh** per request (≈10 seconds of a LED light)


-


Generate shadow model consumes **0.040 Wh** per request (≈15 seconds of a LED light)


-


Generate background model consumes **0.068 Wh** per request (≈24 seconds of a LED light)


-


**Location-based emissions** (not taking into account provider’s renewable energy purchases)


-


Remove background model emits **12 mg CO2** per request (≈1 human breadth)


-


Generate shadow model emits **19 mg CO2** per request (≈2 human breadth)


-


Generate background model emits **32 mg CO2** per request (≈3 human breadth)


-


**Market-based emissions** (taking into account provider’s renewable energy purchases, in line with the GHG Protocol standard)


-


Remove background model emits **4 mg CO2** per request


-


Generate shadow model emits **7 mg CO2** per request


-


Generate background model emits **11 mg CO2** per request


Using the Google CFE% (95% for our location) would lower results even further.


For reference:


-


Running a 10W LED bulb for 1 second uses 0.0028 Wh.


-


A single human breath emits approximately 10 mg of CO2.


Disclaimer: The CO2 emissions estimates provided are based on our current methodology and available data. While we strive for accuracy, these figures are approximate and may be subject to adjustments as new data or methods become available. They are shared to promote transparency and our commitment to sustainability.


## Conclusion


At Photoroom, we integrate sustainability into our AI practices, sharing our CO2 emissions and methodology for accountability. By publishing these figures, we aim to encourage other AI developers to do the same, fostering a culture of responsible AI within the industry.


As our CTO, Eliot Andres, says:


> "By prioritizing environmental considerations, as a fast-growing startup, we hope to inspire other companies to follow."


We also believe that sharing actionable insights with the community—such as transitioning to green energy-powered clusters or reducing AI model latency—adds even more value. By adopting these practices, AI community members can lower emissions while boosting performance and efficiency, whether driven by business or sustainability goals.


As an AI developer, if you want to reduce your AI model latency and CO2 emissions, check out this content:


-


How we reduced our yearly CO2 emissions by 1000+ tons by picking our **[NVIDIA](https://www.photoroom.com/inside-photoroom/so-you-want-to-rent-an-nvidia-h100-cluster-2024-consumer-guide)[cluster](https://www.photoroom.com/inside-photoroom/so-you-want-to-rent-an-nvidia-h100-cluster-2024-consumer-guide)**


-


How we reduced latency up to 25% with **[Inference Pipeline Optimizations](https://www.nvidia.com/en-us/on-demand/session/gtc24-s62726/)** (NVIDIA GTC 24)


-


How we minimized our carbon footprint by building **[our own Foundation Diffusion mModel](https://www.photoroom.com/inside-photoroom/photoroom-foundation-diffusion-model)**


-


How we made Stable Diffusion up to 100% faster using **[Memory Efficient Attention](https://www.photoroom.com/inside-photoroom/stable-diffusion-100-percent-faster-with-memory-efficient-attention)**


-


How we made Stable Diffusion 25% faster and saved seconds using **[TensorRT](https://www.photoroom.com/inside-photoroom/stable-diffusion-25-percent-faster-and-save-seconds)**


-


How we made Image Segmentation 4 times faster with **[TRTorch](https://www.photoroom.com/inside-photoroom/faster-image-segmentation-trtorch-tensorrt)**


And if you’re interested in joining a small AI team that cares for sustainability, have a look at our **[open positions](https://www.photoroom.com/company)** .

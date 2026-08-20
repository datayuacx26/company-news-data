---
schema_version: "1.0.0"
document_id: "e0f810f8f99804eff2f20eb3530b6afc35c638db86ab868993c7bc98fe4f131b"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/resources/blog/computex-2026-demo-how-linear-optics-enables-longer-link-reach-lower-latency-for-scorpio-smart-fabric-switches/"
published_at: "2026-06-03T20:06:04+00:00"
first_seen_at: "2026-07-20T23:21:25.038635+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:504803bfbea2d4084ced6cd686b6997fda7076130b90311aeb4d79a28188976c"
---

# Computex 2026 Demo: How Linear Optics Enables Longer Link Reach, Lower Latency for Scorpio Smart Fabric Switches

*Astera Labs demonstrated PCIe® Gen 6 connectivity of Scorpio Smart Fabric Switches enabled by a low-power, ultra low-latency LPO OSFP module at Computex 2026.*


Two years ago, Astera Labs exhibited the[first successful demonstration of end-to-end retimed optical PCIe connectivity](https://www.asteralabs.com/extending-our-connectivity-leadership-industrys-first-end-to-end-pcie-over-optics-demo/) . This week, we’ve advanced the conversation again.


Just a month removed from the announcement of our[Scorpio™ X-Series 320 Lane AI fabric switch](https://www.asteralabs.com/products/scorpio-smart-fabric-switch/) , we showcased a low-power, ultra low-latency linear pluggable optical (LPO) connection between Scorpio switches at root complex and at a mimicked endpoint to visitors to our booth at Computex 2026. We’ve taken the retimers out of the equation and enabled up to 50m of high-integrity fiber connectivity.


As the industry surges ahead toward an optical future, this marks an important step that will resolve significant hurdles between current architectures and tomorrow’s near-packaged optics (NPO) and co-packaged optics (CPO) solutions.


## High Density, Low Latency: Redefining Scale-Up Architecture With Linear Optics


At Computex 2026, we demonstrated an end-to-end optical PCIe 6 link enabled by our high-radix, low-latency Scorpio X-Series fabric switches. In this configuration, a Scorpio switch embedded in the root complex is connected to an OSFP-XD LPO module.


This module is connected to an identical module at the endpoint with a fiber cable that can span up to 50 meters before latency and PCIe retry buffer depth begin to meaningfully impact the PCIe protocol. This configuration enables signal transmission with an extremely low bit error rate of 1e-8 or better at a level of power consumption significantly below conventional retimed architectures.


In high radix multi-rack scale-up architectures where space is at a premium, transitioning from retimed optics to linear optics optimizes the performance of our Scorpio fabric switches in another significant way: lane density. Because linear optical connectivity does away with the need for a retimer or DSP to condition the signal, it can be packaged in high density form factors. This enables system architects to more fully utilize Scorpio’s impressive lane count.


While these gains are minimal for an LPO solution with pluggable modules that themselves take up significant front panel space, our demo serves as a crucial proof of concept for next-gen linear optical solutions like NPO and CPO. When the same signal processing components from an LPO are architected into an NPO configuration, you could see 576 optically connected lanes or more in a single rack unit.


### Use Cases: Linear Optics for AI Workloads


Density and signal integrity gains in configurations that pair linear optical connectivity with a high-radix fabric switch have promising implications for several major AI modeling use cases.


- **Frontier LLM training:** Even marginal latency reductions compound significantly across billions of training iterations. PCIe 6 connectivity via linear optics and a Scorpio fabric switch directly reduces collective communication overhead during the continuous all-reduce operations required to synchronize scale-up GPU clusters used in training frontier-scale LLMs. The low power profile of a retimer-less solution also helps manage the enormous energy budgets for these clusters.
- **Mixture-of-Experts (MoE) model training and inference:** MoE architectures are notoriously sensitive to interconnect performance because tokens must be dynamically routed between expert clusters potentially residing in different racks. This all-to-all communication pattern creates unpredictable traffic that demands low latency and high bandwidth across disaggregated racks (which is even more powerful paired with the[Scorpio X-Series’ Hypercast™ capability](https://www.asteralabs.com/why-your-mixture-of-experts-model-is-only-as-good-as-your-switch/) ).
- **Large-scale inference with KV cache offloading:** As context grows exponentially for current inference demands, latency becomes correspondingly harder to manage due to the enormous volume of key-value cache data that must move between disaggregated memory pools and GPU compute in near real-time. As KV cache retrieval sits in the critical path of every token generated in high-context inference, utilizing PCIe Gen 6 bandwidth via linear optics can help keep latency in these operations minimal.


## Coordinating Linear Optical Links With COSMOS Telemetry


High-speed links (operating at 112G or 224G per lane) are incredibly sensitive. A link that is perfectly tuned in a lab at 25°C will quickly degrade or drop packets when deployed in a dense AI cluster. Three major variables are constantly working against link stability:


- **Temperature fluctuations:** AI workloads create massive, sudden thermal spikes. As chips and optical lasers heat up, their electrical and optical characteristics shift.
- **Voltage and power drift:** Power delivery networks fluctuate under heavy, bursty AI training loads.
- **Component aging:** Over time, laser diodes degrade and output less power, while silicon characteristics subtly change over years of continuous operation.


Removing the DSP from the optical module drastically reduces latency and power consumption. This is what makes the optical link “linear,” but it also leaves it without an internal DSP to automatically compensate for signal distortion. The entire burden of maintaining signal integrity for the entire high speed link shifts to the host and endpoint.


This is where[COSMOS, a sophisticated telemetry and control platform](https://www.asteralabs.com/products/cosmos/) , becomes absolutely essential. Because a linear optical link lacks an onboard brain to fix these issues, COSMOS acts as the centralized nervous system. It continuously monitors telemetry, dynamically adjusts settings across the entire link path, and diagnoses and debugs errors in real time:


- **Fabric orchestration:** COSMOS manages the structural architecture of the high-speed fabric. It orchestrates dynamic bifurcation, maps physical routing paths, and actively manages port enablement and power states to align hardware resources with workload demands.
- **Initialization and bring-up:** COSMOS executes complex bring-up algorithms in which it sweeps through combinations of SerDes and optical settings to find the optimal “sweet spot” for a clean, low-BER connection.
- **Host and endpoint SerDes tuning:** COSMOS orchestrates the end-to-end channel equalization. It establishes an optimized baseline during link training using protocol-defined preset tuning. During live runtime, COSMOS utilizes continuous lane margining to non-destructively sample voltage and timing margins, dynamically adjusting host transmitter pre-emphasis (FFE) and endpoint receiver equalization (CTLE/DFE). This continuous closed-loop optimization tracks thermal drift and signal degradation in real time, keeping the First Bit Error Rate (FBER) well within acceptable limits before Forward Error Correction (FEC) is applied.
- **Optical module calibration:** Simultaneously, COSMOS fine-tunes the transmit (TX) laser bias currents and receive (RX) transimpedance amplifier (TIA) gains directly inside the linear optical module.
- **Diagnostics and debugging:** COSMOS utilizes an API to check for signal quality parameters, expected state, and recovery rate for links and identifies cases when one of these parameters is outside the user-defined limits. It can save per-lane data to a Python directory for human-readable reporting and debugging.


By actively compensating for time, temperature, and aging, COSMOS keeps linear optics robust, self-healing, and capable of sustaining the massive, uninterrupted data rates that AI scale-up clusters demand.


## The Astera Labs Advantage: Optics, Radix, Real-Time Telemetry


The latency and power consumption[benefits of linear optics are achievable at scale](https://www.asteralabs.com/scaling-the-ai-rack-how-astera-labs-is-approaching-optics-for-scale-up/) when optics, high-radix switches a real-time telemetry system are co-developed. Looking ahead to PCIe 7 and expanded adoption of the[UALink open scale-up standard](https://www.asteralabs.com/advancing-ai-with-amd-delivering-purpose-built-connectivity-for-scale-up-architecture-with-ualink/) , when these gains will be amplified by higher radix, higher data rate architectures, the seamless coordination of the hardware and software layers is even more crucial.


Holistic rack-scale interconnect solutions like the one we’ve exhibited at Computex keep GPU utilization optimized and AI architectures scalable at the leading edge of modeling workload demands.

---
schema_version: "1.0.0"
document_id: "b4207b59f09b2e1e406a3b3b1f421eaa231c3ac3612727f29bfb45b369bffa05"
company_key: "beamr-imaging-ltd-ordinary-share"
company: "Beamr Imaging Ltd. Ordinary Share"
source_id: "beamr-imaging-ltd-ordinary-share-rss-d7edb5bf531d"
canonical_url: "https://blog.beamr.com/2025/08/13/beamr-is-pushing-the-boundaries-of-av-data-efficiency-accelerated-by-nvidia/"
published_at: "2025-08-13T11:05:20+00:00"
first_seen_at: "2026-07-26T10:14:08.454679+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:697b94bab8e766784c470231b356d03fe547d1a8073a011e28e5f406d0f46490"
---

# Beamr is Pushing the Boundaries of AV Data Efficiency, Accelerated by NVIDIA

### Designed to protect visual fidelity, Beamr’s solution helps address the infrastructure challenges associated with autonomous vehicle development pipelines


#### **Balancing Compression with Data Integrity**


To manage autonomous vehicle (AV) petabyte-scale video data infrastructure, compression technology is essential to maintain the fidelity critical for model performance. The challenge isn’t just about storage capacity – it’s about preserving the precise visual details that AV models depend on.


Even minor compression inefficiencies can degrade critical data and translate into substantial infrastructure costs, slowing development cycles across ingestion, structuring and labeling, to training and validation, and finally deployment of AVs at scale. Therefore, balancing compression efficiency with the fidelity requirements of machine learning (ML) models represents a critical challenge.


#### **Successful Testing with Real-World Footage**


Beamr’s content-adaptive technology has gone through rigorous testing using real-world AV footage, including challenging scenarios with complex camera configurations and diverse environmental conditions. Beamr’s patented compression technology, Content-Adaptive Bitrate (CABR), already used by global giants for video solutions optimized for human vision – is now tailored to address the challenges of[autonomous vehicle video data](https://blog.beamr.com/2025/06/10/how-content-adaptive-compression-tackles-autonomous-vehicle-data-explosion/) .


The Beamr solution uses NVENC, an encoder integrated into the NVIDIA GPU, and NVIDIA SDKs, which are included with NVIDIA AI Enterprise software platform, to accelerate GPU applications As a result, Beamr’s technology delivers efficient, high fidelity compression at the scale of AV development demands — helping developers reduce infrastructure costs, preserve critical data quality, and accelerate AV development cycles.


Beamr collaborated closely with the NVIDIA AV Infrastructure team to address a critical challenge: ensuring compression doesn’t degrade the visual information ML models need to make accurate, and safe, driving decisions. Using NVIDIA’s robust quality metrics and testing protocols, Beamr’s solution was rigorously evaluated, demonstrating that it preserves the essential visual elements AV models rely on.


Beamr’s content-adaptive encoding delivered significant compression improvements while maintaining quality with no perceptual artifacts that could impact model accuracy. **CABR delivered 23% compression improvement over current working point using the same codec, with up to 40-50% reduction when switching to CABR-HEVC or CABR-AV1** – that includes CABR AV-designed compression along with codec modernization to HEVC or AV1.


The validation confirmed that Beamr’s approach preserves the temporal and spatial cues essential for object detection and tracking:


> *This is an improvement for our video data pipeline – with compression savings and no degradation to our AV models. The content-adaptive approach demonstrates the efficiency needed for AV systems development without compromising model integrity* “ **** said Zachary Taylor Senior Director, AV Infrastructure at NVIDIA.


The production deployment phase will test the technology’s full-scale performance, with integration supported by Beamr’s engineering team to ensure a smooth roll out without disrupting existing workflows.


#### **Positive Signal for the AV Industry**


For AV companies managing their own petabyte-scale video challenges, Beamr’s technology sends a signal that ML-optimized compression is ready for enterprise AV operations. The same content-adaptive encoding technology is available to accelerate other autonomous vehicle pipelines while reducing infrastructure costs.


CABR directly addresses AV-scale challenges:


- **Content-Adaptive savings:** Beamr’s compression technology dynamically adjusts to the varying complexity of AV scenes. Similar validations with other AV organizations have shown even greater compression gains, demonstrating the technology’s broad applicability across different AV infrastructures.
- **ML Model Integrity:** Beamr validates visual fidelity using quality measures specifically designed for ML video, ensuring compression preserves the temporal and spatial cues needed for object detection and tracking without degrading downstream model performance.
- **Processing Efficiency:** Beamr’s technology can handle the high throughput demands of AV data at high speed and low cost, enabling rapid and scalable compression. For example, using a few dozen NVIDIA L40 data center GPUs – it is possible to process one petabyte of video data every day.


#### **Next Steps in Video Data Processing**


“Our collaboration with NVIDIA underscores the critical need for smart video compression in the AV industry,” ** said Sharon Carmel, Beamr CEO. “We’re proud to provide a solution that not only significantly reduces operational costs but also guarantees the fidelity essential for reliable AV models and enables efficient operations at scale.”


For the AV industry, Beamr’s content-adaptive technology, now tailored for AV workflows, can help accelerate other organizations’ development while simplifying infrastructure complexities and costs.


———————–


**-> Run Beamr’s ML-safe compression on your own data:[beamr.com/autonomous](http://beamr.com/autonomous)**


———————–


**Learn more in our ML-Safe AV Video Data Testing series** :


Part 2:[ML-Safe AV Video Data Processing Achieves Up to 50% Storage Reduction](https://blog.beamr.com/2025/12/18/ml-safe-av-video-data-processing-achieves-up-to-50-storage-reduction/)


Part 3:[Deep Dive: Managing the Petabyte-Scale AV Video Data Bottlenecks](https://blog.beamr.com/2026/01/05/deep-dive-managing-the-petabyte-scale-av-video-data-bottlenecks/)


Part 4:[Beamr’s ML-Safe Video Compression Validated on NVIDIA Cosmos Curator](https://blog.beamr.com/2026/03/31/beamrs-ml-safe-video-compression-validated-on-nvidia-cosmos-curator/)

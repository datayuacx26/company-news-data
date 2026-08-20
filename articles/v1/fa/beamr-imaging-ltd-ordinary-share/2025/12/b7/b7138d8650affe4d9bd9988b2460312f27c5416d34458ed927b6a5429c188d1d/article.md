---
schema_version: "1.0.0"
document_id: "b7138d8650affe4d9bd9988b2460312f27c5416d34458ed927b6a5429c188d1d"
company_key: "beamr-imaging-ltd-ordinary-share"
company: "Beamr Imaging Ltd. Ordinary Share"
source_id: "beamr-imaging-ltd-ordinary-share-rss-d7edb5bf531d"
canonical_url: "https://blog.beamr.com/2025/12/18/ml-safe-av-video-data-processing-achieves-up-to-50-storage-reduction/"
published_at: "2025-12-18T09:29:44+00:00"
first_seen_at: "2026-07-26T10:14:08.454679+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:db7c70e0e3ec9a480f1af50774da92edc7326ebe5288ece546ead7d629eb6d67"
---

# ML-Safe AV Video Data Processing Achieves Up to 50% Storage Reduction

### **ML-Safe Testing Series, Part 2: Benchmark testing shows how AV developers can achieve greater savings across storage, networking and compute without compromising ML model accuracy**


For engineering teams building next-generation autonomous driving solutions, one major obstacle can slam the brakes on development and innovation: the massive volume of video data. As autonomous driving (AV) development scales, companies need to handle tens to hundreds of petabytes of real-world and synthetic footage. These massive data volumes lead directly to escalating **** storage costs, and slower data transfer times that bottleneck ML pipelines.


Beamr has[validated](https://blog.beamr.com/2025/08/13/beamr-is-pushing-the-boundaries-of-av-data-efficiency-accelerated-by-nvidia/) a new approach, helping to address the infrastructure challenges associated with AV and Advanced Driver Assistance Systems (ADAS) development pipelines. **New benchmark testing further demonstrates how content-adaptive compression can cut AV and ADAS video data footprint by up to 50% with practically no impact on the performance of the critical ML models that power autonomous systems.**


> Beamr’s AV video data processing can offer immediate impact, acting as a powerful optimization layer that reduces costs without disrupting established operations


##### **Solving the Compression Dilemma**


Generic video compression creates a fundamental dilemma for AV and ADAS teams, who are forced to choose between two undesirable outcomes: aggressive compression that risks degrading the very visual information that ML models depend on for accurate perception, or conservative approaches that fail to achieve meaningful storage savings. Because AV perception models are trained on high-quality datasets, they can be highly sensitive to any compression artifacts introduced by generic encoders.


**Beamr’s patented Content-Adaptive Bitrate (CABR)** **technology overcomes this trade-off. It analyzes AV footage frame-by-frame, optimizing bitrate and file size while preserving the critical visual cues that perception models rely on, ensuring high-fidelity data for AV models.**


##### **Testing Beamr’s technology: Massive Savings, Zero Compromise**


To put Beamr’s CABR to the test, we compared 2 compression workflows: CABR AV optimized compression against industry-standard workflows, using[PandaSet](https://pandaset.org/) , a real-world, multi-camera AV dataset.


The validation focused on object detection, a foundational task for AV perception. We deployed a standard object detection model, YOLOv8 (Nano), on both the baseline and CABR-compressed videos. We conducted a comprehensive comparative analysis and measured any difference in model accuracy across the most prevalent classes for both PandaSet and the AV industry in general: persons, cars, motorcycles, and trucks.


The overall performance results showed the significant improvement offered by CABR compression: Beamr achieved **~48% average file size reduction** with a **<2% difference in mean Average Precision (mAP)** , well within the model’s expected variance, thus confirming ML-safe compression.


To further confirm this ML-safe process, we also observed robust performance across multiple industry-standard quality metrics, with Average PSNR values over 40 dB, LPIPS below 0.05 and both mAP50 and mAP50_95 well above 0.9.


### **The Impact on AV and ADAS Workflows**


This testing validates that retaining only about half the data size is possible, while maintaining the data fidelity required for safety-critical autonomous driving systems. This directly translates to dramatically lower storage costs, faster data transfer and accelerated ML training cycles. All that, with a **** low-risk path for AV teams.


Beamr’s CABR can offer immediate impact, acting as a powerful optimization layer that reduces costs without disrupting established operations. CABR integrates seamlessly with existing workflows, requiring no changes to existing ML pipelines, with multiple deployment options:


- **SDK Integration** : Direct integration into existing video processing pipelines.
- **FFmpeg Plugin** : Drop-in replacement for current encoding workflows.
- **Managed Service** : Fully managed compression service on the cloud.


**Beamr offers a seamless way to scale your ML and AV video data workflows, with proven technology that requires no changes to existing ML pipelines.** ****


———————–


**-> Run Beamr’s ML-safe compression on your own data:[beamr.com/autonomous](http://beamr.com/autonomous)**


———————–


**Learn more in our ML-Safe AV Video Data Testing series** :


Part 1:[Beamr is Pushing the Boundaries of AV Data Efficiency, Accelerated by NVIDIA](https://blog.beamr.com/2025/08/13/beamr-is-pushing-the-boundaries-of-av-data-efficiency-accelerated-by-nvidia/)


Part 3:[Deep Dive: Managing the Petabyte-Scale AV Video Data Bottlenecks](https://blog.beamr.com/2026/01/05/deep-dive-managing-the-petabyte-scale-av-video-data-bottlenecks/)


Part 4:[Beamr’s ML-Safe Video Compression Validated on NVIDIA Cosmos Curator](https://blog.beamr.com/2026/03/31/beamrs-ml-safe-video-compression-validated-on-nvidia-cosmos-curator/)

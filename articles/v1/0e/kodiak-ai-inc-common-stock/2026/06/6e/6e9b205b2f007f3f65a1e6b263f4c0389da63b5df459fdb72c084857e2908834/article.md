---
schema_version: "1.0.0"
document_id: "6e9b205b2f007f3f65a1e6b263f4c0389da63b5df459fdb72c084857e2908834"
company_key: "kodiak-ai-inc-common-stock"
company: "Kodiak AI Inc."
source_id: "kodiak-ai-inc-common-stock-rss-dc4af1b0a9ba"
canonical_url: "https://kodiak.ai/news/training-physical-ai-at-scale"
published_at: "2026-06-11T14:00:00+00:00"
first_seen_at: "2026-07-20T23:18:01.334765+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:1961de09c8ca68a4bac470182b091f6d8202358e7daf44ba8c6328ba893d1230"
---

# How We Build AI That Succeeds In Navigating The Complexity Of The Real World

# How We Build AI That Succeeds In Navigating The Complexity Of The Real World


[Shubham Shrivastava](https://kodiak.ai/news?author=shubham-shrivastava)


|


June 11, 2026


Kodiak has established itself as a pioneer in Physical AI. The Kodiak Driver, our AI-powered autonomous driving system, is commercially deployed in 28 driverless trucks with no humans in the cab as of March 31, 2026.


Across every mile, we aim to set new standards for safe and reliable driving in ground autonomy. Fresh advances have brought a paradigm shift in how we build AI for deployment in the real world.


At Kodiak, GigaFusionNet embodies this shift.


GigaFusionNet is the core foundation model powering our autonomous driving system, and an essential component of our unique approach. It is a large-scale neural network architecture designed to understand the physical world and the complex dynamics inherent to driving.


This singular, powerful model ingests and processes multimodal sensor data from cameras, LiDAR, and radar to construct a holistic representation of the driving environment. GigaFusionNet’s rich representation serves as the bedrock for all subsequent critical tasks, ranging from building 3D bounding boxes and 3D scene understanding to end-to-end driving token prediction.


The longevity of Kodiak’s system is then derived from an AI Flywheel, a self-reinforcing loop that drives continuous, autonomous improvement which ensures models constantly improve during real-world operation.


At CVPR last week in Denver, I detailed our work on GigaFusionNet and how training large-scale Physical AI foundation models requires tightly integrated, accelerated computing infrastructure optimized for multimodal AI, distributed training, and high-throughput data movement.


### **Building the Brain**


Creating a neural network capable of safely and confidently navigating the real world is a monumental engineering challenge. It requires a model that internalizes the physics of the world: how objects move, interact, and behave under a near-infinite variety of conditions. To extract this knowledge from the millions of autonomous miles logged by Kodiak's fleet, we employ a sophisticated, multi-stage AI training pipeline.


These are the key steps:


**1. Data curation: Maximizing learning efficiency**


GigaFusionNet uses advanced filtering to maximize the entropy of the training data. We actively seek out and prioritize samples that represent rare, challenging edge-case scenarios. This strategic focus is essential to improve the model's generalization capabilities, prevent overfitting to common situations, and ensure maximum learning efficiency from available compute resources.


**2. Pre-training a large-scale GigaFusionNet**


We pre-train GigaFusionNet on an enormous unlabeled dataset, using a self-supervised or weakly supervised objective function, standard techniques for training neural networks. The objective function, which could be a form of next-token prediction or its equivalent in the multimodal, spatiotemporal domain, forces the AI to learn deep, general concepts about the physical world.


These concepts include spatial relationships, temporal coherence, object permanence, and interaction dynamics, all learned without explicit, costly human labels. This process builds a robust, generic world knowledge base transferable across diverse driving scenarios.


**3. Leveraging and specializing the knowledge base**


Once the pre-trained model has acquired this fundamental understanding of the world, it is specialized for various specific autonomous-driving tasks, such as 3D bounding-box detection and road geometry recognition. Further, it serves as the foundation for Vision-Language-Action (VLA) models that intuitively reason about and physically adapt to real-world environments.


**4. Supervised Fine Tuning (SFT) for model alignment**


Utilizing a high-quality, human-labeled dataset is the critical next step for training the model. It corrects any unintended biases or subtle inaccuracies the model may have learned during the large-scale, pre-training phase, and it refines the model’s output to meet rigorous performance and safety standards required for commercial deployment.


###
**The AI flywheel: Continuous improvement via autolabeling**


The core of Kodiak’s sustained progress is its AI Flywheel, a self-reinforcing engine that utilizes autolabeling to drive ongoing improvement.


Manually labeling such a large volume of data is prohibitively expensive and slow. Autolabeling provides a scalable, efficient solution for generating vast quantities of high-quality supervision data.


When smart data curation is paired with high-fidelity autolabeling, it ensures the system not only has a large volume of data but also maximizes variation and distribution coverage.


A key innovation is the ability to leverage future frames in the autolabeling process. The model learns from object trajectories and scene evolution, not just forward in time, but also backward in time. This enables a Teacher-Student regime in training GigaFusionNet.


In this setup, a powerful, often less-efficient "Teacher" model processes rich, spatiotemporal data to generate high-quality autolabels. These superior autolabels then train a much more efficient "Student" model. This technique helps extract as much actionable information as possible from every mile driven in the real world.


The consequence of this flywheel is that the majority of Kodiak’s models are primarily trained with autolabels today. While this ensures models constantly improve during real-world operation, a potential risk exists: the failure modes of current models might get exacerbated with reinforcement through the autolabeling feedback loop.


This is precisely why the targeted amount of human-labeled data remains invaluable. It provides the critical external supervision necessary to correct and mitigate these potential errors, ensuring the model's safety profile remains rigorous.


###
**Kodiak Brain: Generalization across platforms and ODDs**


The unified training methodology, combining large-scale autolabeled data with supervised fine-tuning yields a highly generalized **** AI. This powerful knowledge base is architected to perform robustly across various Operational Design Domains, such as highway, surface street, and off-road driving, under different weather conditions and across different platforms, including various vehicle types and sensor configurations.


This generalized knowledge is then leveraged to train the final end-to-end driving VLA model. This VLA model can reason about the world and is conditioned on:


1. **Spatiotemporal multimodal features** derived from GigaFusionNet’s foundational backbone.
2. **Ego history** represents the vehicle's past movements and states.
3. **Intent tokens** to encode both high-level and low-level goals.


This conditioning allows the VLA model to reason about how to drive strategically and safely in complex, dynamic, and varied scenarios.


### **Training infrastructure: The Kodiak-Lambda partnership**


Autonomous driving represents one of the most demanding Physical AI workloads, requiring large-scale training infrastructure. This includes high-GPU memory, high inter-node bandwidth, and sustained data throughput. When Kodiak's on-prem hardware hit its ceiling, the team needed to scale fast without moving petabytes of sensor data.


Kodiak turned to Lambda. Our partnership provides NVIDIA HGX H100 accelerated computing infrastructure optimized for large-scale AI training, high-bandwidth GPU communication, and distributed multimodal model deployment.


**Fast interconnects**


Maintaining training efficiency requires high node-to-node throughput, which is facilitated by NVIDIA NVLink and high-speed NVIDIA networking technologies critical for distributed multimodal workloads. This allows for the quick exchange of gradients across the distributed cluster, alongside high data bandwidth and low-latency storage, to feed vast multimodal sensor data to the GPUs at a high, sustained rate, preventing I/O from becoming a bottleneck.


**Data pipelines**


A fast compute cluster starved of data is still a bottleneck. GigaFusionNet ingests multimodal sensor data, cameras, LiDAR, and radar, at volumes that can overwhelm conventional storage architectures. We need to stream hundreds of terabytes of sensor data at the latency required by the training workload, keeping GPUs fed without diverting engineering attention to plumbing.


###
**Efficient AI: Distillation onto the NVIDIA DRIVE Hyperion Platform**


To achieve large-scale, commercial deployment, the immense power of these foundation models must be made computationally efficient for in-vehicle operation. Kodiak has partnered with NVIDIA to scale its driverless vehicle efforts using the NVIDIA DRIVE Hyperion architecture in next-generation platforms.


Harnessing the immense power of large foundation models that encode a deep understanding of the physical world on the state-of-the-art NVIDIA AGX Thor X computing platform requires model distillation.


The ultimate goal is to create a Student model that performs as well as the Teacher model but with significantly lower latency and computational cost, making it viable for real-time operation in the power-constrained vehicle. The Kodiak-Lambda partnership is central to enabling and accelerating this world-model distillation process at massive, commercial scale.


###
**Frontier technology, commercially deployed today**


Combining the understanding of GigaFusionNet with high-performance AI infrastructure, Kodiak has built a highly scalable, asset-light AI flywheel that rapidly iterates and improves.


As we continue to distill these models onto the edge, we are proving that driverless trucks can be safely and reliably deployed in commercial operation and, in turn, establishing the definitive road toward Physical AI achieving widespread scale in global logistics and beyond.


## More like this


June 3, 2026[Quantifying Safety Knowns and Unknowns: How Kodiak Uses PRA and AI to Measure Risk](https://kodiak.ai/news/pra-quantifying-safety)


May 18, 2026[Kodiak AI Issues Statement on BUILD America 250 Act Bill Supporting Autonomous Trucks](https://kodiak.ai/news/build-america-250-act)

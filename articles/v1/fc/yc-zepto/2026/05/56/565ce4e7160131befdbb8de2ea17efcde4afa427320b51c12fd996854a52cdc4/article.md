---
schema_version: "1.0.0"
document_id: "565ce4e7160131befdbb8de2ea17efcde4afa427320b51c12fd996854a52cdc4"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/zepiris-reimagining-scalable-face-authentication-for-attendance-at-zepto-040da77d8231"
published_at: "2026-05-25T10:11:12+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:ecb6e5c5c4f942b58006e3f5b6043dfacf5b923a9bfeb5ce2acd7103e2f9d691"
---

# ZepIris: Reimagining Scalable Face Authentication for Attendance at Zepto

At Zepto, speed isn’t just about delivery — it defines how we build, operate, and innovate across every layer of the stack.


In large-scale, high-velocity supply chain operations like ours — where thousands of packers and riders work in tight coordination to enable 10-minute deliveries — even minor inefficiencies can cascade into operational delays. Traditionally, attendance tracking in such environments has relied on **non-tech solutions like paper registers** or **basic digital systems such as app-based check-ins** . These approaches often suffer from delays, manual errors, and, in many cases, **identity-related frauds** like proxy attendance or buddy punching.


To overcome these challenges, we introduced **face authentication-based attendance** , a more comprehensive and tamper-proof solution. By verifying that the same person who was onboarded is the one marking attendance, the system not only enhances reliability but also protects the integrity of performance-linked incentives — such as those tied to order frequency or daily targets. In essence, this ensures **speed, accuracy, and accountability** across our rapidly scaling dark store network.


This led to the development of ZepIris — Zepto’s face authentication platform, powering fast, secure, and intelligent attendance for every packer and rider in our network.


### Why Build ZepIris


Looking at our supply chain operations, it became clear that building a reliable identity layer was essential before scaling further. With thousands of people moving across Dark Stores and Mother Hubs daily, we needed to ensure attendance accuracy, prevent impersonation, and enable fast, seamless verification. To address these needs, we identified two foundational problems to solve:


- ***1:1 Verification:***
For dark stores, we adopted a **1:1 verification approach** — each user’s live image is matched against their registered reference image using their own device, ensuring quick and secure attendance marking.
- ***1:N Search:*** At Mother Hubs, users are identified by matching their live image against the entire database using a central device. This enables quick onboarding and attendance for large groups without manual ID checks or app dependencies — making it practical even in remote locations where users may not have high-end smartphones.


While both 1:1 and 1:N systems strengthen identity verification across our operations, deploying them at scale brings additional challenges. These include **computer vision issues** such as poor image quality, low lighting, occlusions, and multiple faces in a frame; **fraud prevention** concerns like spoofing or replay attacks; and the need to maintain **content safety and secure handling of biometric data** . Addressing these effectively is key to ensuring accuracy, reliability, and user trust across the network.


We realised that to achieve the right balance of precision, speed, and cost-effectiveness — along with complete control over performance tuning and system evolution — the best approach was to build our own in-house solution, fully aligned with our requirements and growth objectives.


### The Foundation: Face Authentication, Reimagined


Press enter or click to view image in full size


ZepIris is built on a simple yet powerful two-part system:


- **Face Detection** — performed on-device
- **Face Recognition** — performed on the backend


Both components are deeply integrated with Zepto’s operational workflows, optimised for performance on budget smartphones, and designed for real-world constraints — including low-light warehouses, limited user guidance, and patchy network conditions.


### On-Device Face Detection using ML Kit


The authentication flow begins on the device. When a packer or rider starts their shift, they are prompted to take a selfie through the Zepto Operations app. To reduce backend load and ensure only high-quality inputs proceed, we use Google’s ML Kit to perform face detection and validation directly on the device. This on-device module performs real-time checks:


- Ensures a face is present and centered
- Verifies both eyes are open (preventing spoofing)
- Detects extreme close-ups or framing issues to prompt re-capture


These validations significantly reduce the number of backend API calls, thus reducing the system load and help maintain a smoother user experience by reducing back and forth time for invalid inputs.


### Face Recognition Pipeline & Embedding Generation


Once an image is validated at capture, it is sent to the backend for recognition and verification.
The attendance mechanism begins with user onboarding, where each user’s image is enrolled into the system database. During onboarding, every image is transformed into a facial embedding — a compact numerical representation that encodes discriminative facial features for efficient comparison.
For embedding generation, we initially experimented with the DeepFace framework, evaluating multiple backend architectures (such as VGG-Face and FaceNet variants). However, these models exhibited inconsistent embedding stability under diverse real-world conditions — including variable lighting, camera quality, and head pose.
To overcome these limitations, ZepIris implements a highly flexible face-embedding infrastructure built on the open-source **ArcFace (Additive Angular Margin Loss)** framework. Rather than binding our platform to a single static model, ZepIris provides an abstract inference layer natively compatible with multiple ArcFace-backed configurations e.g. **AuraFace** .
The system generates 512-dimensional, L2-normalized embeddings optimized for cosine similarity computations. This architectural approach ensures high intra-class compactness and inter-class separability. This abstraction layer not only stabilizes recognition accuracy across environmental variations but also allows developers to flexibly toggle or configure their preferred underlying embedding engine to match specific commercial compliance or deployment requirements.


ZepIris supports two recognition flows:


### * 1:1 Verification (Attendance for Dark Store Packers and Riders)


For attendance marking, ZepIris performs a direct 1:1 verification


1. Generates an embedding from the new selfie
2. Retrieves the user’s stored embedding from the vector database
3. Compares them using cosine similarity
4. Marks attendance if the similarity score exceeds the configured threshold


### * 1:N Search (Onboarding and Attendance at Mother Hubs)


When onboarding a new user, the system:


1. Generates an embedding for the captured selfie
2. Searches across all stored embeddings to detect potential duplicates
3. **For Onboarding:** The system flags or blocks a user if a matching face already exists in the database.
4. **For Attendance:** The system marks attendance for the closest verified match based on facial similarity.


This prevents duplicate or fraudulent registrations and enables seamless multi-user authentication from shared devices like tablets.


### Why 1:N Matching Needs a Vector Database


While 1:1 verification is straightforward, 1:N matching requires comparing a new embedding against thousands of existing ones — demanding fast, approximate nearest-neighbor (ANN) search across high-dimensional vectors.


We initially implemented **FAISS** for this, which worked well offline but required full reindexing whenever embeddings were updated and was much more challenging to integrate these index updates — creating availability and latency challenges. To enable real-time updates and searches, we needed a persistent vector database that could scale seamlessly.


### Why We Chose Milvus


During initial exploration, we evaluated several managed vector search solutions but found that they struggled to deliver consistent latency and accuracy at scale. These options also limited our ability to fine-tune parameters and optimize for our specific embedding setup.


To overcome these challenges, we adopted Milvus — an open-source vector database purpose-built for approximate nearest neighbor (ANN) search — which offered the right balance of accuracy, predictable performance, and native cosine similarity support aligned with face embeddings.


We began with a standalone Milvus setup and later migrated to a distributed configuration to efficiently manage the growing volume of embeddings and parallel queries. This shift gave us full control over performance tuning, leading to noticeable improvements in both latency and accuracy.


There are different indexing strategies that Milvus offers. Based on our accuracy and latency requirements we tweaked some parameters and went for the HNSW indexing strategy


### Indexing Strategy for Milvus: HNSW


To achieve high recall with low latency, we adopted the **HNSW (Hierarchical Navigable Small World)** indexing strategy — a graph-based approach optimized for fast and scalable approximate nearest neighbor (ANN) search. Unlike IVF_FLAT, which relies on coarse clustering, HNSW builds a multi-layer proximity graph that enables efficient traversal through embedding space.


We configured the index with the following key parameters:


- **M** → controls the number of bi-directional connections for each node, balancing accuracy and memory usage.
- **d** → dimensionality of the face embeddings.
- **ef_construction** → governs graph construction accuracy, ensuring robust neighborhood connectivity.
- **ef_search** → determines the breadth of the search process during queries, directly affecting recall and latency.


By carefully tuning these parameters, we achieved a strong balance between **search precision and performance** , maintaining low query times even as the embedding volume scaled. This configuration enabled real-time 1:N searches without compromising match quality or system responsiveness.


### Beyond Matching: Spoofing, Blur, and Safety Checks


Facial authentication alone isn’t enough to guarantee trust in real-world warehouse environments. Even if a model identifies a face correctly, operational challenges — low-end cameras, uneven lighting, and misuse attempts — can compromise reliability. At Zepto scale, we found that **quality assurance and misuse prevention** are as crucial as recognition accuracy.


To strengthen the system, we built three supporting classifiers focused on **security** , **image quality** , and **data hygiene** .


**1. Spoof Detection — Preventing Impersonation Attacks**


The attendance system’s purpose is to accurately log employee check-ins via face verification. However, traditional solutions are vulnerable if users attempt to **spoof** identity using printed photos or screens.


Our initial exploration with off-the-shelf anti-spoofing models (DeepFace, SilentFace) showed poor generalisation in actual warehouse conditions. Low lighting, camera distortions, and motion blur frequently led to incorrect matches.


We therefore engineered an **in-house MobileNet-based anti-spoofing model** , fine-tuned on operational data.


- Detects prints, screens, and replay attacks
- Maintains high recall even on low-quality camera feeds


**2. Blur Detection — Ensuring High-Fidelity Embeddings**


Face embeddings degrade sharply if the input frame is blurry or low-resolution. That leads to false mismatches and poor user experience.


To solve this, we trained a **ResNet-based blur classifier** that automatically rejects unclear images and offers the user a retry mechanism before proceeding.


- Reduces failed recognition due to poor captures
- Real-time feedback improves the first-attempt success rate


**3. Nudity Detection — Protecting Data Hygiene & Compliance**


Unexpected content uploads are a harsh reality of open camera input systems. To maintain platform safety and compliance, we developed a **MobileNet-powered nudity classifier** that filters inappropriate submissions before they enter the data pipeline.


- Blocks obscene or non-compliant content
- Maintains clean datasets and protects portal visibility


### Takeaway: Recognition Isn’t the Whole Story


A production-ready attendance system is more than face matching. It must:


- Detect spoofing attempts
- Ensure high-quality image capture
- Enforce responsible user behavior


By building **specialised lightweight CNNs** , optimised for warehouse environments, we significantly improved trustworthiness of facial authentication at the edge.


Also, running these models in parallel kept the latencies in check.


### Backend and Visualization Portal


Once the Data Science component of the solution was in place, the next challenge was operationalizing it for broader adoption across Product and Operations teams. This required a centralized system that could manage user status, maintain synchronization with embeddings, and provide visibility into ongoing transactions.


To achieve this, the Backend team built the **Image Matching Interface (IMI)** — a robust and scalable backend service tightly integrated with the main application. IMI manages user states via a structured database schema, ensuring data consistency and validity across the entire workflow. It also powers a dedicated **web portal** that visualises transactions in real time, enabling case-level and user-level analysis.


Through this portal, stakeholders can monitor recognition outcomes, trace issues, and derive actionable insights directly from live system data. By seamlessly integrating the backend logic, database management, and visualization layer, **IMI serves as the bridge between the machine learning engine and operational decision-making** , transforming complex recognition workflows into an accessible, auditable, and insight-driven platform.


### Scaling Challenges and Solutions


As ZepIris scaled across hundreds of stores and thousands of users, new challenges emerged:


- **Embedding Model Bottleneck:** High concurrent traffic during shift changes caused throughput issues. We solved this by fine-tuning machine resources and implementing asynchronous inferences.
- **Adaptive Thresholds:** Different use cases — attendance, onboarding, and audits — required separate similarity thresholds. ZepIris now allows configurable thresholds per workflow type to cater to this.
- **Face Positioning Failures:** Early captures often failed due to extreme close-ups or off-angle selfies. Real-time feedback and stricter on-device framing validation significantly improved capture success rates.


### Integration at Scale


ZepIris integrates end-to-end with Zepto’s mobile app, attendance APIs, and internal dashboards. It processes thousands of requests daily — even under poor lighting, low bandwidth, or low-end devices — maintaining high accuracy and uptime.


Operational teams use ZepIris dashboards to monitor exceptions, review matches and audit anomalies in real time through the dashboard.


### The Results


Since deploying ZepIris:


- **Greater system transparency and auditability** , ensuring complete traceability and easier performance and frauds monitoring.
- **Faster, more reliable attendance experience** for frontline teams, supported by real-time selfie feedback.
- **Accelerated feedback integration** , allowing on-ground insights to be incorporated rapidly into ongoing system improvements.
- **Optimized infrastructure and cost efficiency** through streamlined in-house processing.
- **Improved accuracy** enabled by deeper visibility into data and tighter alignment with real-world conditions.
- **Reduced latency** achieved through optimized embedding generation and vectorized search operations.


Most importantly, ZepIris has earned the trust of our warehouse teams — attendance is now quick, reliable, and secure. No OTPs, no manual input — just a selfie.


### Final Thoughts


Authentication at scale isn’t just a backend problem — it’s a full-stack systems challenge spanning hardware, software, and user behavior. Multiple teams across Engineering, Product, and Operations came together to solve this challenging problem and ensure successful on-ground implementation.


With ZepIris, we’ve built a fast, intelligent, and secure facial authentication platform that meets the realities of Zepto’s operations — built for scale, reliability, and speed. Just like our 10-minute delivery promise, it’s designed to be fast, dependable, and always on time.


---


[ZepIris: Reimagining Scalable Face Authentication for Attendance at Zepto](https://blog.zepto.com/zepiris-reimagining-scalable-face-authentication-for-attendance-at-zepto-040da77d8231) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.

---
schema_version: "1.0.0"
document_id: "7b279c1039c9bfcaa972b76bb08fb724baf9b97b9db3df657582fb157c8708dd"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/real-time-package-damage-detection-at-scale-integrating-yolo-in-warehouse-operations"
published_at: "2026-03-26T20:25:26.412+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:adf0f047f7d0105a85b8104ab7b5f0dad02b763f918fdcf25654c66f25c06a8f"
---

# Real-Time Package Damage Detection at Scale: Integrating YOLO in Warehouse Operations

YOLO model detects package damage in real-time as items move along a conveyor belt


# Introduction: The High Cost of the "Last Mile"


At Wayfair, we prioritize the safe arrival of every customer order. Currently, quality control relies on manual visual inspections, a system that is difficult to scale and prone to human error. Damaged package shipments impact the customer experience through delayed replacements and frustration, which also drives up our costs with increased logistical strain. To address this, we developed Tarragon, a[YOLO](https://github.com/Megvii-BaseDetection/YOLOX) -based Computer Vision (CV) model that automatically detects damaged packages in real-time. By integrating Tarragon into our warehouse sorting flow, we are moving toward a scalable, automated solution that protects the customer experience before the "last mile" begins. In this post, we will dive into how we build and integrate Tarragon into our operations.


# The Sorter Scan Tunnel


To illustrate the functionality of our damage detection system, let's look at a typical Wayfair Fulfillment Center (FC) in the US. The facility utilizes high-speed sorting systems equipped with advanced scan tunnels. As each package passes through the tunnel, multiple specialized linear cameras capture high-resolution grayscale images from up to six different angles, including the top, bottom and various side perspectives. These images provide the granular detail necessary to spot even small punctures or tears.


Packages are scanned and routed to either the outbound shipping lane or the Jackpot manual intervention lane.


To maintain a seamless flow of thousands of packages per day, our detection model must perform its assessment in real-time. From the moment the cameras start streaming data, the system has less than *one second* to analyze all images and transmit a diversion signal to the sorter hardware. This ensures that any package flagged as potentially damaged can be programmatically diverted to a dedicated lane for manual review.


# Hybrid Data Strategy


Our primary goal was to develop an automated system capable of identifying and localizing surface damage in real time. To meet the high processing speeds required by our sorting hardware, we chose the YOLO architecture for its inference efficiency. However, the lack of labeled data for our specific warehouse environment posed a significant challenge. To address this, we developed a hybrid pipeline that utilizes Vision Large Language Models (VLLMs) to intelligently curate samples for human labeling.


## The Zero-Four Damage Scale


We collaborated with warehouse associates to standardize labels into two actionable categories:


- **Scale 0 to 2:** No damage or minor scratches that do not require repair.
- **Scale 3 or 4:** Actionable defects, such as punctures, tears or significant crushing, that require re-taping or re-boxing.


Images of damaged packages showing defects (scale 3 on the left, 4 on the right).


Because package damage is rare, manual searching for damaged training examples to label is a "needle in a haystack" problem. We fine-tuned the Gemini 2.0 Flash model with a small amount of hand-labeled data as a Damaged/NoDamage classifier to pre-select *damaged* images for human labeling. This targeted selection ensured most selected images are truly damaged, and saved months of manual effort by narrowing the focus for our annotators to images with a high probability of defects. The data was split into two subsets: 20% was double-annotated for quality control, and 80% was labeled by a single annotator. Among the double-labeled set, our human annotators disagreed on 16% of the image-level Damage/No Damage labels. This high variability highlights that identifying damage is a difficult task, even for humans.


To handle this variability, the vetted and corrected 20% data (clean set, double-labeled) is used for high-fidelity validation and initial training of the YOLO model, and the remaining 80% data (noisy set, single-labeled) is used for training augmentation.


Here is what we learned:


- **Effective data augmentation:** Initial inclusion of noisy data negatively impacts performance, but training with a larger, more diverse dataset ultimately leads to improvement.
- **The "Sweet Spot":** We found that more data isn't always better. Once we reached a certain volume, the model stopped improving, showing us that we had reached a point of diminishing returns.
- **Significantly better than guessing:** Our final model is highly effective. If we simply guessed based on how often damage usually occurs, we would be right only 3.3% of the time. Our best model performs over 12 times better than that No-skill baseline, as seen in the PR-AUC (Area Under the Precision-Recall Curve) trend below:


Holdout PR-AUC from models trained over different combinations of clean and noisy sets. Higher values indicate better performance.


# Cloud Deployment


To ensure Tarragon integrates seamlessly into our global fulfillment network without slowing down operations, we designed an end-to-end workflow that bridges cloud inference with physical warehouse sorting flow.


Cloud deployment architecture for real-time package damage detection.


The Image Processor preprocesses images from the scan tunnel and transfers them to a cloud-hosted YOLO model for scoring, and the decisions are cached for low-latency lookups. The cached decisions drive package diversion and downstream Pub/Sub consumers.


From a latency point of view, it is desirable to deploy the CV model on an edge device, near the physical sorter. This however requires running an inference node next to each Sorter in the warehouse. As we scale up the operation to our global fulfillment network, the complexity of setting up and maintaining separate inference nodes quickly becomes unmanageable. We decided to host the CV model in a Vertex AI endpoint, a common practice to deploy ML models at Wayfair, and only send pre-processed features to the endpoint. While cloud inference adds about 250 ms of latency compared to running inference locally, it is still within our 1s Service-Level Agreement (SLA) and is a small price to pay for a service that runs 7/24 at scale.


## The Jackpot Feedback Loop


The Jackpot lane where flagged packages are diverted to is critical for manual intervention and data refinement. The system assists associates by allowing them to review the specific images that led to the CV assessment. By logging their final repair decisions into our database, associates contribute a continuous stream of ground-truth feedback. This collected data is then used to refine the CV model, allowing the model to quickly adapt to new package types and changing warehouse conditions. This ensures high performance even with fewer training samples when deploying to a new sorter.


# Lessons Learned


**GenAI as a Force Multiplier**


One of our most impactful strategic choices was using Generative AI to overcome the data scarcity barrier. Using fine-tuned multimodal LLMs to pre-select rare positive samples from unlabeled warehouse imagery meant our offshore human annotators could focus exclusively on high-value defect validation and bounding box labeling, and significantly reduced the time required to build a robust dataset.


**Engineering for the Real World: Beyond the Code**


Warehouse-based AI projects must navigate physical realities. Early in development, we identified a camera calibration issue where images were consistently under or overexposed. Because our model relies on high-fidelity visual data, this was a significant blocker.


Fixing this required months of coordination with our hardware vendor to schedule scanner software updates and onsite calibrations. These mechanical adjustments were ultimately completed during a single 12-hour maintenance window in early January.


# Conclusion


We have successfully validated the operational readiness of the Tarragon model at our Erlanger, KY warehouse, and plan to scale it to the rest of the FC network. This deployment demonstrates how cutting-edge Computer Vision and Generative AI can bridge the gap between digital intelligence and physical operations. By proactively identifying damaged packages in real-time, we are not only preventing millions in annual losses but also ensuring that our customers receive their orders in perfect condition.


# Join the Team


If solving complex, real-world challenges at the intersection of machine learning and logistics excites you, we want to hear from you! Wayfair is actively recruiting for roles in large-scale logistics and deep learning. Check out our open roles on the[Wayfair Careers Page](https://www.wayfair.com/careers/jobs) .


# Acknowledgment


We appreciate the valuable input from Joe Gardunio of Wayfair Industrial Engineering, who provided feedback on Sorter integration and reviewed numerous sample images.

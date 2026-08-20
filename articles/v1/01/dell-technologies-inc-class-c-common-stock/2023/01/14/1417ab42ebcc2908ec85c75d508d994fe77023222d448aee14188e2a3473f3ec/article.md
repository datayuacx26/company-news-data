---
schema_version: "1.0.0"
document_id: "1417ab42ebcc2908ec85c75d508d994fe77023222d448aee14188e2a3473f3ec"
company_key: "dell-technologies-inc-class-c-common-stock"
company: "Dell Technologies Inc."
source_id: "dell-technologies-inc-class-c-common-stock-rss-5d64130a27f7"
canonical_url: "https://www.dell.com/en-us/blog/what-autonomous-vehicles-taught-me-about-the-edge/"
published_at: "2023-01-04T14:00:20+00:00"
first_seen_at: "2026-07-20T04:35:28.920829+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:946b794404baa6300204d711eb766d1fdbf9c045c9a9dc266ead6bba2f85f29f"
---

# What Autonomous Vehicles Taught Me About the Edge

Yesterday, I reconnected with a college buddy. We laughed as we reminisced about our ongoing, creative college quest to find a free lunch. Faced with strict, college-student budgetary constraints, our goal was financially motivated. Ah, but so much has changed since then. Today, many free lunches are available at companies that provide complimentary food for employees to retain talent via positive employee experiences. One of my favorites is at[Ben and Jerry’s,](https://www.benjerry.com/flavors/3-pints-a-day) where employees get three pints of free ice cream for every day of work.


As wonderful as free lunch sounds, let’s be honest, somebody must pay for it. And, as several Dell Technologies edge customers have shown me, the same is true for edge applications deployed using a cloud-first technology strategy; somebody will have to pay for it. Sadly, this is nowhere near “free.” The costs sneakily emerge after implementation, and they can be excessive. Let’s look at why this matters through the lens of a real-world example that brings this issue – and its resolution – into crystal clear focus.


#### **The Significance? It’s Massive and Growing**


The need to get edge applications right is critically important. Estimates predict that by 2025, most of the world’s enterprise-generated data will be outside the core data center and the cloud. IoT streaming use cases will generate much of that data, but the edge applications consuming that data need to be data-first, not cloud-first. The penalties for cloud-first often incur steep costs and high latency, which, ironically, negate the key benefits of edge and near real-time computing.


#### **A Very Cool Edge Application**


One of our customers uses autonomous vehicles to deliver parts between warehouses. Each vehicle is equipped with a PowerEdge server that aggregates sensor telemetry and video streams. The data is ultimately shipped to a public cloud location to perform analytics for route planning purposes. This autonomous vehicle example is an exemplary edge application and IoT streaming use case, with video data as a component of the streaming data. ****


#### **At First Blush, Cloud-first Looks Good**


Initially, this customer adopted a cloud-first strategy to transport the sensor and camera data over a cellular network to the cloud for processing and storage. Developer preference drove this decision, leveraging cloud infrastructure and services for analytics, inferencing and model training. The cloud AI services allowed the organization to analyze data streams, identify the most relevant scenes and correlate sensor data to optimize routes. It also allowed them to store and post-process scenes to retrain models. Soon, however, it became apparent this approach limited response times, further exacerbated by the bandwidth required to transport video over a cellular network. An additional concern was the cost to centralize the storage and processing of this data in the cloud. ****


#### **Then, Implementation and Quick Fixes Reveal Blind Spots**


As a stop-gap measure to manage costs and cellular bandwidth, the customer used the server in the truck to transcode the video streams to a lower resolution and bitrate before transmitting to the cloud. However, this approach initiated a negative domino effect, worsening the issues it aimed to correct. Transcoding the files meant lowering the video fidelity, which in turn resulted in lower inference scores, requiring more data scientists’ time to analyze scenes and retrain models. This increased the compute cost and overhead to produce accurate outcomes. The net results? Higher costs and lower performance. ****


#### **A Better Approach is Data-first**


Dell Technologies worked together with the customer to architect a data-first solution that leveraged the pipeline of computation and storage from the vehicle to the cloud, resulting in lower latency, reduced cost and improved performance. The solution involved using a GPU in the onboard PowerEdge server to inference video streams with full fidelity. Consequently, this approach improved inference performance and allowed the customer to filter many of the non-relevant data frames. Then, the remaining data of interest could be transported off-vehicle, at full fidelity, over a cellular network to a distribution of co-location centers much closer to the vehicle. As a result, the customer used its choice of on-prem cloud stack for batch analytics, model training and deep storage, while preserving the original development implementation choices.


#### **The Right Edge Architecture Drives the Right Outcomes**


Edge applications need to be architected properly to deliver business outcomes. To do so requires optimization across the far edge, near edge, the cloud and data centers. The data-first strategy versus a cloud-first approach in the autonomous vehicle case study pivoted to a hybrid cloud solution, using localized processing to improve business operations and lower costs. For the autonomous vehicle customer, the move to data-first is making an enormous improvement in operations and outcomes. Now, back to that Ben & Jerry’s ice cream…


#### **Want to Learn More?**


Contact a **[Dell Technologies subject-matter expert](https://www.dell.com/en-us/dt/forms/contact-us/edge-computing-solutions.htm)** to discuss planning, implementing and improving your edge application.


Read our new **[AI at the Manufacturing Edge eBook](https://www.dell.com/en-us/dt/solutions/edge-computing/industries.htm#tab0=0&pdf-overlay=//www.delltechnologies.com/asset/en-us/products/ready-solutions/briefs-summaries/ai-edge-manufacturing-ebook.pdf)** to learn more about the edge in manufacturing.

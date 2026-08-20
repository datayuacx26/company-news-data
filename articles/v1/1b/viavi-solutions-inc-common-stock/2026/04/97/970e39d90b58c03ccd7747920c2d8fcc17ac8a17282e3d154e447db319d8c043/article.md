---
schema_version: "1.0.0"
document_id: "970e39d90b58c03ccd7747920c2d8fcc17ac8a17282e3d154e447db319d8c043"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/04/27/the-five-considerations-for-validating-enterprise-readiness-in-ai-workloads/"
published_at: "2026-04-27T15:32:52+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:52:06.558365+00:00"
content_hash: "sha256:6104177ac4a090b858ba92158e83be920f9ea0fa7e64e9e5cb17d1d1dd9a84e9"
---

# The Five Considerations for Validating Enterprise-Readiness in AI Workloads

**


# The Five Considerations for Validating Enterprise-Readiness in AI Workloads


- **Steve Rumsby
- ** April 27, 2026
- 1


Scale is a critical factor for AI. The growth in the size of models used for generative AI over the past decade is just one aspect of the effects of scaling. Now, as AI embeds in everyday life, the capacity needed for successful deployment is challenging networking and computing architectures.


Whether training AI models or deploying production versions for inference, the data-intensive nature of the underlying technology places enormous stress on data center networks. And this stress manifests in ways that differ from those encountered in traditional data-center installations. Those implementing such infrastructure need to look at multiple variables to determine how best to design and build a suitable hardware and software infrastructure to run AI systems.


Once the architecture is in place, extensive analysis and testing are essential to ensure that the network topology, equipment, and security models work together in harmony. There are five key areas data center engineers should consider in this process. The choices in each of them and their interactions will determine how well the network will perform.


### **1. Workload Environment**


AI workloads place demands on data-center networks that are unlike those of conventional IT. In addition, those demands change based on circumstances. A training-oriented environment places a high degree of emphasis on the network’s ability to support intense bursts of activity between all the active nodes. This is driven primarily by weight-update algorithms passing data through a scale-out network as the AI model trains on each new batch of inputs. The traffic patterns are, on the other hand, more predictable than those encountered with inferencing workloads.


Inferencing is driven primarily by user demand. Increasing activity will place more stress on the network and is less predictable than training updates. Because there is less need to cater for the any-to-all updates of training, inferencing provides more opportunities for dynamic re-routing of traffic to avoid temporary bottlenecks.


Like training, inferencing has the same lack of tolerance for packet loss, latency, and the overhead of traditional approaches to retransmission. This is driving the adoption of novel forms of Ethernet, which support more efficient routing, redundancy, and retransmission techniques.


The need for novel protocols will differ depending on the target environment. Hyperscalers handling highly differentiated traffic patterns may favor the facilities offered by Ultra Ethernet. Neocloud or enterprise installations may prefer the deployment speed, security, and management facilities available with more conventional equipment. Many will also often be able to effect greater control over inferencing demand.


### **2. Topology and Network Features**


There are key performance and reliability characteristics an AI-focused network will need to satisfy. Decisions on inference architectures can affect the inter-server traffic of AI data centers. Access to data storage may be a major factor in enterprise deployments. Some systems developed will rely on multiple AI models working together to improve the quality of results. Those will exhibit east-west transfer patterns that are potentially quite different from single-model systems. In these, individual nodes will primarily handle prompts and data sent by external network connections, which will place a greater focus on north-south traffic.


The security architecture will have additional effects on network choices. The use of zero-trust security assumptions may lead to access control and encryption being used on all internal network paths. Others will employ a tiered approach to security. Choices over where capacity and how protection measures are deployed will need to be tested to demonstrate they are appropriate for the end system.


### **3. Equipment Characteristics**


There are many equipment options when developing a network to support AI inferencing. Novel Ethernet derivatives, such as Ultra Ethernet, bring AI-focused features that come with novel operating characteristics. These choices bring with them questions about how aspects such as latency, congestion control, and packet loss will be handled in the live network.


These choices lead to differences in how the network might fail to perform as expected. Usage spikes or changes in access patterns may cause unexpected packet losses because the congestion concentrates in equipment less able to tolerate the changes. Similarly, decisions made to improve security may cause some routers to drop packets because they do not have the required processing capacity. All these factors will need to be tested.


*VIAVI*[ONE LabPro](https://www.viavisolutions.com/en-us/products/one-labpro) *(left),*[TestCenter D2](https://www.viavisolutions.com/en-us/products/testcenter) *, and*[CyberFlood](https://www.viavisolutions.com/en-us/products/cyberflood) *(right) enable validation across all OSI levels (L0-L7), including for 1.6T*


### **4. Scaling Testing**


Scale is just as important in testing as it is in deployment. The AI data center relies on the complex interactions between nodes and networking links throughout the installation. Performance will depend on many factors at different layers within the stack. Low-level physical connections will determine the bit error rate, which will often determine how many packets fail to reach their destinations. Congestion at the link level will determine how many packets are lost because of queues overflowing. Packet loss and congestion have massive ramifications on AI training and GPU utilization.


Then there are complex packet patterns that each different model configuration or application is likely to generate. These flows may cause congestion to build up at key pressure points. They are points the implementer will want to relieve as much as possible with changes to topology and link capacity.


Traditionally, testing these conditions at scale would demand the equivalent of a data center to generate the test traffic and process the results. Thanks to developments in test hardware and automation, that is no longer necessary. It is possible to generate high-volume packet profiles that fully exercise the network at high levels of utilization. In doing so, that approach provides insights that are not possible with individual, link-level tests.


### **5. Corner Cases**


At scale, improbable events can lead to large-scale failures. For this reason, it is important to consider not just aggregate performance but also any rare situations that will have disproportionate effects. Test hardware that makes it possible to inject custom packet profiles to gauge tail latency or create situations to force equipment to reroute transmissions or lose packets can show how well the network will handle adverse events.


A test solution that provides the ability to generate custom traffic and prompt patterns is essential for performing testing at this level of granularity. But by testing latency, loss, and performance under different scenarios, implementers can ensure the network architecture they have chosen is as resilient as possible.


### **Summary**


AI’s demands place intense pressure on network design. Whether they are working in hyperscaler, neocloud or enterprise environments, data center engineers can achieve success by considering these five factors and developing a validation strategy that tests them.


Learn more about[AI Data Center Network Testing](https://www.viavisolutions.com/en-uk/solutions/ai-data-center-networking-test) solutions.


- ** Categories:[Hyperscale](https://blog.viavisolutions.com/category/hyperscale/)
- ** Tags:[High-Speed Ethernet](https://blog.viavisolutions.com/tag/high-speed-ethernet/)


-
-
-
-
-
-

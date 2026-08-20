---
schema_version: "1.0.0"
document_id: "14e19807c4d861761ccc5f3052dbe7fc348ae59631253a07167d026b5bd0bc3b"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/02/17/the-state-of-6g-and-ai-ran/"
published_at: "2026-02-17T20:31:52+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:54:15.153981+00:00"
content_hash: "sha256:9d83c0a1555bbf7b1db67f5f966a7b7ecd52b92f8eb60fe844b6172b5152c1f7"
---

# The State of 6G and AI-RAN

**


# The State of 6G and AI-RAN


- **Ian Wong
- ** February 17, 2026
- 1


2025 was a watershed year for the next generation of connectivity. With the ITU finalizing IMT-2030 requirements and 3GPP officially launching Release 20 Study Items, 6G’s definition is slowly crystallizing. The ecosystem is evolving from research to early prototypes.


During the year ahead, we can particularly expect the focus to shift to the AI-Native Network and Air Interface, with engineers building the testbeds to prove integrated sensing and communications (ISAC) and 6G’s higher frequency bands can operate successfully outside of the lab.


Adding to the complexity is the convergence of networks, AI, security, wireless, photonics and sensing, which used to evolve as independent domains. This convergence is redefining how networks and critical infrastructure must be designed, validated, and protected for them to be trusted and resilient at scale.


In this blog we’ll take a look at the changes that appear in Release 20 and the use of AI-native air interfaces with an eye to testing nondeterministic neural receivers. We’ll also take a look at VIAVI’s 6G Forward Program and how it’s being used. These technologies will be demonstrated at[Mobile World Congress (MWC) Barcelona](https://www.viavisolutions.com/en-us/events/2026/mwc-barcelona-2026) 2026 at VIAVI’s stand 5B18 from March 2-5.


### **3GPP Release 20**


Release 20 includes formal research items for the 6G RAN and arguably one of the biggest changes – at least for those working on the physical layer – involves new FR3 spectrum and an “AI-native” approach to the air interface.


If we look at 5G, we see a significant overhead that is there to maintain compatibility with LTE. Release 20 seeks instead to reduce this by eliminating always-on signaling. This should have the effect of reducing the density of reference signals and therefore the interference floor, and as a result improve energy efficiency.


Other big changes exist at the spectrum level, with the FR3 band (7 GHz to 24 GHz) introduced to allow bandwidths >100 MHz, but without the attenuation issues that would come with the higher-frequency mmWave bands. Implementing this upper-mid band does, however, bring several difficulties, not least is the significant increase in the number of antennas needed to achieve the same level of coverage as existing 3.5 GHz sites. Indeed, much larger antenna arrays, potentially up to 1024 elements are required for this, and a significant focus is therefore given in Release 20 to preventing control-plane bottlenecks when using these massive antenna counts.


One of the more significant changes, however, comes in the way AI is integrated into the physical layer. In 5G, AI was largely kept to management-plane tasks like load balancing or power optimization, but for 6G study items this changes with AI implemented directly into the signal processing chain to create the AI-native air interface.


### **AI-Native Air Interfaces**


The incorporation of AI at a deeper level should bring significant advances in site-specific optimization, with trained neural networks replacing standardized algorithms for tasks such as channel estimation. The benefit of this approach is the ability to cope with specific environments in different ways, with the AI trained to cope with highly diverse scenarios. This enhances throughput and resilience, especially in more complex (urban) environments.


The challenge this raises is how to ensure the AI has been trained to cope with new settings. With performance tied to training history, a model trained on data from Miami might not perform as well when transferred to New York, and vice versa. The engineering priority will therefore be to ensure these models can generalize when out in the field and not just in the labs where they were trained.


Engineers will also need to understand how models drift over time and the biases inherent in the system. Validation will therefore be essential in proving the AI is suited to the environment it will be operating in, with techniques such as digital twins playing a vital role in demonstrating this is the case before anything is actually rolled out.


Figure 1: A ray tracing capture of the Dún Laoghaire harbor in Dublin, Ireland as created for NVIDIA and its Aerial Omniverse Digital Twin


### **Validation for 6G**


VIAVI’s 6G Forward Program provides the tools to validate 6G’s most demanding use cases, specifically integrated sensing and communications (ISAC) and city-scale digital twins.


For example, VIAVI is assessing the performance and co-existence of ISAC technologies, using a GPU-powered real-time testbed to prove that a single waveform can provide high-accuracy localization without degrading the data throughput for the user.


To handle the sensitivity of high frequency 6G signals, the program is working with organizations such as Northeastern University’s Open6G Cooperative Research Center and Hanyang University in Seoul to understand the behavior of an AI-RAN when unleashed into the field.


These are built using ray tracing technologies to create exact 3D replicas of entire urban environments and can model the way signals bounce off buildings and trees or interact with moving traffic and buildings under construction.


Training is then undertaken within the twin using equipment such as the VIAVI TM500 Network Tester and TeraVM AI RAN Scenario Generator (AI RSG) to emulate the user interfaces and traffic patterns under a wide variety of situations, including heavy use and the introduction of DDoS attacks and viruses to ensure the AI-RAN remains stable.


### **Out of the Lab, Into the MWC Expo Hall**


With the AI-RAN shifting from the lab to the field this year, we’re set to be able to see and quantify the improvements in energy efficiency, and radio performance of 6G. And we’re also set to see the errors created from training biases and model drifts. Closed-loop testing, with the system constantly validating decisions versus real-world performance will therefore be vital in 6G’s advance.


VIAVI’s 6G Forward platform will therefore be a core element of our Mobile World Congress offering, with several key demonstration stations. Here’s a preview of what you’ll see:


**Network Digital Twins**


VIAVI is merging advanced digital twin technology with best-in-class AI/ML integration, and real-world measurement expertise to deliver meaningful, validated performance improvements and accelerating the journey to 6G.


At MWC, VIAVI will be highlighting several recent projects including an AI-driven Beam Management with NTT Docomo that delivers greater automation, network performance, and energy efficiency for 5G and 6G networks; an agentic RAN digital twin created in collaboration with NVIDIA and AWS for scenarios rendered with stunning detail using NVIDIA raytracing technology; an AI-Driven Tilt Optimization with Hanyang University; and LLM-based beamforming with Yonsei University and Singapore University of Technology and Design (SUTD).


**6G and Integrated Sensing and Communications**


Our second demonstration highlights how VIAVI can help validate AI-native 5G/6G features under real-world conditions, boost capacity with AI-optimized feedback, and cut RF modelling time and cost.


In these demonstrations we will highlight our 6G integrated sensing and communications platform for precise emulation of the communication and sensing channels, as well as our agentic-AI-based raytracing with drone-enabled mapping.


**Channel Emulation and Massive MIMO**


Our third 6G demonstration station looks towards MIMO and the performance gains it brings, with three elements highlighting the combination of the Vertex channel emulation with the TM500 UE emulation; massive MIMO testing in partnership with Nokia; and a site-specific digital twin using the TM500 to create highly realistic replicas of radio environments.


**AI-RAN & VALOR™ Ecosystem Engagement**


Our final 6G station looks specifically at AI native RAN, where proving the quality of data used to train and update the AI is essential. VIAVI’s VALOR is leading the way here and visitors to the stand will be able to see how this is aiding working groups within the AI-RAN Alliance. VALOR is funded by the[National Telecommunications and Information Administration (NTIA)](https://www.ntia.gov/)[Public Wireless Supply Chain Innovation Fund](https://www.ntia.gov/funding-programs/public-wireless-supply-chain-innovation-fund) .


We will be showcasing our Data4AI in the AI-RAN Alliance data validation demonstration; our AI neuromorphic receiver testing process, highlighting our AI-RAN test methodology; an AI-driven ghost preamble detection with GlobalLogic; and our AI-RAN demonstration with NVIDIA on the DGX Spark.


These demonstrations will be available to view throughout Mobile World Congress from VIAVI’s Stand 5B18. Other demonstrations will include VIAVI’s latest solutions for security and quantum-safe networks, mission-critical communications, AI data centers and AI Operations (AIOps).


- ** Categories:[5G](https://blog.viavisolutions.com/category/5g/) ,[6G](https://blog.viavisolutions.com/category/6g/) ,[AI](https://blog.viavisolutions.com/category/ai/) ,[Industry Trends](https://blog.viavisolutions.com/category/industry-trends/)
- ** Tags:[5G](https://blog.viavisolutions.com/tag/5g/) ,[6G](https://blog.viavisolutions.com/tag/6g/) ,[AI-RAN](https://blog.viavisolutions.com/tag/ai-ran/) ,[Digital Twin](https://blog.viavisolutions.com/tag/digital-twin/) ,[ISAC](https://blog.viavisolutions.com/tag/isac/)


-
-
-
-
-
-

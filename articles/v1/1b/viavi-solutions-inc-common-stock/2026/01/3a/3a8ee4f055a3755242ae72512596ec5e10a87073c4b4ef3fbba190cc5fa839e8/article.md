---
schema_version: "1.0.0"
document_id: "3a8ee4f055a3755242ae72512596ec5e10a87073c4b4ef3fbba190cc5fa839e8"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/01/13/how-will-test-and-measurement-evolve-in-the-new-year/"
published_at: "2026-01-13T20:21:28+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:61344f9681994e005e8300a7b802b61784e1e4914b49d6c9712fd0d62f3564f9"
---

# How Will Test and Measurement Evolve in the New Year?

**


# How Will Test and Measurement Evolve in the New Year?


- **Sameh Yamany
- ** January 13, 2026
- Like


Test and measurement for critical infrastructure is entering a new phase. In 2026, changes that have been emerging in the background are very much set to shift to become front of mind.


These changes relate to security, AI, photonics, and sensing. While all could be said to be related but evolving independently, they are now converging to directly affect one another. This convergence categorically changes how we need to develop, roll out and test the networks, data centers, and critical infrastructure they run on.


In this blog, I outline my big five predictions for the year ahead.


### **1) The migration to ultra-high-speed, fabric-aware deployment and testing in AI data centers.**


To say AI workloads are exploding would be a gross understatement. Training and inference at scale are currently driving an unprecedented demand for 400G, 800G and 1.6T Ethernet and optical technologies. 3.2T is not far away either.


As a result, the data center architectures are being reshaped around massive GPU and xPU fabrics. Unfortunately, from a testing point of view, traditional throughput tests won’t be enough to guarantee that the network can handle the synchronised nature of AI compute. Instead, we now need to look beyond the amount of data each pipe can hold and look at how the network handles the traffic jams that occur when thousands of GPUs try to talk to each other at the exact same millisecond.


Test and measurement strategies therefore need to incorporate fabric-aware validation methods that model the specific, high-pressure traffic patterns that are unique to AI. These include the all-reduce operations, where GPUs synchronize their learning; as well as east-west traffic flows, which come in bursts as the servers offload data to one another; and the congestion cascades that build as each failure directly triggers the next.


Most importantly, testing must now consider tail-latency sensitivity to manage the straggler packets that can cause an entire AI training session to grind to a halt.


All of this will require cost-efficient emulation of GPU workloads, which will be essential for testing the performance of these new fabrics.


### **2) AI-native threats will force and reshape cybersecurity and quantum-safe communications.**


AI is already being used for (and is alarmingly good at) phishing attacks, polymorphic malware, and identity spoofing.


Meanwhile the technology is also being used to enable both autonomous and agentic defense systems. We’re therefore at the point where cybersecurity is entering an AI-versus-AI era.


And then you have the rise of quantum computers and the risk they pose to RSA and elliptic curve cybersecurity techniques. These can[already complete calculations](https://blog.google/technology/research/google-willow-quantum-chip/#:~:text=General%20summary,medicine%2C%20energy%2C%20and%20AI.) a leading-edge supercomputer would fail to do if left alone to run from now until the[universe’s big crunch and after every star in the universe had died](https://www.livescience.com/space/astronomy/the-universe-is-dying-much-faster-than-scientists-thought-new-study-suggests) . With Q-Day fast approaching, the race to implement post-quantum cryptography (PQC) and (for optical networks) quantum key distribution (QKD) is underway, shifting from theory to actual implementation with validation.


Indeed, validation is vital in ensuring such systems are not only mathematically sound, but also can be deployed without affecting latency, throughput and other essential KPIs on the networks they are protecting. Testing will therefore need to be implemented for crypto-agility, performance under load, and key lifecycle management at scale. And this will need to be done for not only PQC and QKD, but also hybrid systems, where these technologies work in conjunction with classical security on networks. These hybrid networks will be the norm, rather than the exception, for the foreseeable future. As such, it will be vital to ensure interoperability between these key management systems.


### **3. AI-RAN will enter the field trials stage**


This shift out of the lab is a major milestone, but it brings with it several unknowns, including training biases, model drift, energy efficiency, and radio performance. To understand these, we will need to implement closed-loop testing, where the system constantly validates the AI’s decisions against real-world performance in real-time.


Simultaneously, photonics and fiber are reasserting themselves as the strategic nervous system of the network. As a result, we are already seeing a move toward coherent optics, as well as hollow-core and multi-core fiber with integrated photonics and fiber sensing. These are all becoming increasingly tied to AI-driven analytics, with modeling undertaken via digital twins to deliver enable proactive maintenance, rather than reactive troubleshooting.


### **4. Data4AI will become a strategic asset**


As photonics and digital twins begin to provide the strategic nervous system, the quality of the data used to train it will be central to their success. This year will, therefore, see Data4AI evolve from a secondary byproduct of network management into a primary strategic asset for telecom operators.


To move beyond the trial phase, high-fidelity, labeled ground-truth data (captured from both live physical networks and high-precision digital twins) become essential to train, validate, and guard-rail AI models. Without this data foundation, AI cannot be trustworthy, explainable, and regulator-ready AI at scale.


### **5. PNT and ISAC to become national security priorities.**


Recent years have seen the vulnerabilities in GNSS exploited as part of international warfare, with jamming and spoofing techniques used to affect[aircraft](https://www.bbc.co.uk/news/articles/c9d07z1439zo) ,[ocean tankers](https://windward.ai/blog/gps-jamming-is-now-a-mainstream-maritime-threat/) , and other critical infrastructure and systems.


As such, we need now to not only validate that a signal has reached its destination, but to certify that a network is resilient enough to be trusted.


Because many countries now have a near-total reliance on these technologies for positioning, navigation, and timing (PNT), their resilience will move to the top of the national security agenda, not just to prevent military and commercial navigation errors, but also for AI data centers, 5G/6G telecom networks, and energy grids, which all require perfectly synchronization.


And then there is the emergence of integrated sensing and communication (ISAC, or JCAS/JSAC) technologies, where sensing and spatial location are integrated into the mobile networks to allow line of sight interruption detection in mmWave radio networks as well as for security, automotive/smart-city systems and logistics.


In short, ISAC provides the situational awareness that allows national infrastructure to autonomously detect and defend itself against physical threats and will therefore join PNT as a national security priority.


### **The common thread**


Of course, these predictions all have a common thread across them: they focus on validating behavior, optimizing AI solutions, and understanding resiliency and trust at scale. And they are all converging on each other making test and measurement across all of these systems absolutely critical.


- ** Categories:[5G](https://blog.viavisolutions.com/category/5g/) ,[Aerospace and Defense](https://blog.viavisolutions.com/category/aerospace-and-defense/) ,[AI](https://blog.viavisolutions.com/category/ai/) ,[Cybersecurity](https://blog.viavisolutions.com/category/cybersecurity/) ,[Data Center](https://blog.viavisolutions.com/category/data-center/) ,[Hyperscale](https://blog.viavisolutions.com/category/hyperscale/) ,[Industry Trends](https://blog.viavisolutions.com/category/industry-trends/)
- ** Tags:[AI](https://blog.viavisolutions.com/tag/ai/) ,[AI-RAN](https://blog.viavisolutions.com/tag/ai-ran/) ,[Data centers](https://blog.viavisolutions.com/tag/data-centers/) ,[hyperscale](https://blog.viavisolutions.com/tag/hyperscale/) ,[Industry trends](https://blog.viavisolutions.com/tag/industry-trends/) ,[PNT](https://blog.viavisolutions.com/tag/pnt/)


-
-
-
-
-
-

---
schema_version: "1.0.0"
document_id: "d4ae574d78d64ac331d80171c1693eadd966c3ead102d9bc13fe98acd922ff82"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/01/19/creating-quantum-safe-networks/"
published_at: "2026-01-19T17:49:35+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:54:46.350230+00:00"
content_hash: "sha256:d0fa3d389877746151059305112a60b10082248413629d42a16f7398e09afcbc"
---

# Creating Quantum-Safe Networks

**


# Creating Quantum-Safe Networks


- **Sameh Yamany
- ** January 19, 2026
- Like


Advances in quantum computing, coupled with the rise of “harvest now, decrypt later” strategies among cybercriminals, are heightening the urgency to safeguard data and networks. This has been underscored by several recent high-profile breaches, including in October 2025, when it was revealed[that “vast amounts” of classified UK government data](https://removepaywalls.com/https:/www.thetimes.com/uk/politics/article/dominic-cummings-china-state-secrets-csxn7s8pt) had been taken in a state attack. In addition, various[healthcare data breaches](https://www.beckershospitalreview.com/healthcare-information-technology/cybersecurity/10-biggest-healthcare-data-breaches-of-2025/) affected more than 20 million people in the United States in 2025. The theoretical risks are beginning to feel significantly more tangible.


Even with this in mind, we are about to enter 2026 with some grounds for optimism: three post-quantum cryptography (PQC) algorithms[have been standardized](https://csrc.nist.gov/news/2024/postquantum-cryptography-fips-approved) , major[browsers](https://cloud.google.com/security/resources/post-quantum-cryptography) and[operating systems](https://support.apple.com/en-gb/122756) have enabled post-quantum key agreements by default, quantum key distribution (QKD)[techniques and rollouts are advancing](https://www.grandviewresearch.com/industry-analysis/quantum-key-distribution-market-report) to secure the physical layer, and[government mandates are in place](https://pqcc.org/international-pqc-requirements/) to enforce these in critical applications.


Despite these steps forward, plus the formation of multiple industry bodies, preparedness at the organizational level is still somewhat lagging behind the targets being set by the threat.


In this blog, we’ll give an overview of where we are, where we should be and how best to get there. Test and measurement are vital in ensuring that measures will work. We’ll particularly examine how to implement this and discuss how VIAVI is working to advance the compliance and efficacy of the rollout process.


If you are attending[MWC 2026](https://www.mwcbarcelona.com/) in Barcelona this year, come by our booth 5B18 and see firsthand our latest platforms and tools that will simplify the process for both system architects and operators.


### **Quantum-safe architectures**


For PQC, NIST’s cryptographic algorithms ([FIPS 203, 204 and 205](https://csrc.nist.gov/news/2024/postquantum-cryptography-fips-approved) ) have been standardized since August 2024. These algorithms use lattice- (FIPS 203, 204) as well as larger, slower hash-based (FIPS 205) cryptographic techniques to establish secret key-based secure connections (FIPS 203) and for digital signatures (FIPS 204, 205).


In addition to PQC, QKD can also be used as a more secure measure, with this key exchange method using optical links to transmit qubit-based keys. While there is still potential to eavesdrop the fibers themselves, this can be instantly identified with systems automatically falling back to PQC.


These will obviously also have to work with legacy networks for the foreseeable future. Therefore, measures need to be in place on hybrid networks to enable the negotiation of a classical encrypted tunnel and allow communications to continue.


### **State of implementation in 2026**


Implementing protection across hybrid networks is particularly important but, as a recent[survey of 2,600](https://www.isaca.org/resources/news-and-trends/newsletters/atisaca/2025/volume-9/quantum-computing-preparedness-lagging) people working in digital trust and cybersecurity highlights, just 5% of those surveyed believe their organization has a defined quantum-security plan. The survey also revealed that just 5% of respondents believe it is a priority for the near future, and only 7% said they had a strong understanding of the NIST standards. This is despite 56% citing harvest now/decrypt later strategies as a concern.


Quantum computers are forecast to have the potential to break RSA-2048 based encryption[by 2030](https://security.googleblog.com/2025/05/tracking-cost-of-quantum-factori.html?ref=hackernoon.com) . Google has already announced that its Willow quantum processor (105 qubit) with advanced error corrections can perform a standard benchmark computation in under five minutes that would take one of today’s[fastest supercomputers](https://www.olcf.ornl.gov/frontier/) 10 septillion (that is, 1025) years — a number that vastly exceeds the age of the Universe. IBM’s next-generation Kookabura is said to deliver 4,158+ qubits (24053+ more than Willow) and this is[scheduled for 2026](https://insidehpc.com/2022/05/ibm-quantum-promises-4000-qubit-processor-in-2025-envisions-quantum-centric-supercomputing/) .


Q-day, or the day when quantum computers can break traditional encryption methods, is approaching quickly. The biggest challenges hindering the rollout of quantum-safe algorithms and techniques are ones of interoperability, scalability and performance. In short, those implementing such systems need to ensure they can work seamlessly, will not break the network or system and won’t degrade the performance of transactions when implemented.


This is where test and measurement come into the equation.


Table 1: A framework for testing QKD, PQC and Hybrid Systems, comparing classical and quantum components


### **Test and Measurement Techniques**


Test and measurement can be used to ensure the effectiveness of both PQC and QKD (and hybrid) security approaches as well as validating key management system (KMS) interoperability. Furthermore, it not only helps ensure compliance but can also be undertaken first on digital-twin networks to prevent any unforeseen issues that would otherwise cause complications if deployed on a live system.


If we look first at PQC methods: from a standards and compliance point of view, it is vital to understand what happens in failure scenarios, with testing used to simulate mismatched PQC key selections or communications with a party that does not support PQC, thereby evaluating interoperability.


From a performance point of view, these algorithms are operationally much heavier than traditional methods. Testing needs to consider the algorithms’ effect on KPIs such as computational efficiency, encryption speed, key size variations and key generation. Such tests should also be emulated at scale, with the network or system under exceptional loads. Here, digital twins can emulate many thousands of users with realistic traffic patterns to understand the effect on latency, throughput, and MoS scores.


Unlike PQC, QKD validation takes place in the quantum channel itself, and stress testing is needed to cover this and extend to the service layer. Emulated twin networks are once again vital in this process, and tests should examine polarization disturbances and reflection events to interference from DWDM signals – ensuring quantum and classical traffic can co-exist within the same DWDM fiber.


Testing also needs to demonstrate resilience against side-channel attacks, to confirm PQC will be used in the event of QKD links being compromised, and (depending on whether the system is using discrete- or continuous-variable QKD) evaluate the precision of its clock synchronization, or validation of its phase reference alignment.


Once the system is live, QKD networks need to also continually monitor the qubit error rate (QBER) to detect possible eavesdropping.


Shifting to KMS (Key Management Systems), these need to be certified as interoperable across QKD, PQC, and hybrid systems, with the ability to interface with multiple standards, and operate across vendors and protocols. Standards adherence is vital, with at-scale testing protocols simulating corruption, loss and delays in key delivery.


Figure 2: Summary of testing a quantum versus a classic channel


### **VIAVI’s role in PQC, QKD and Hybrid Testing**


As we highlighted above, arguably the biggest challenge in ensuring the rollout of these quantum safe technologies are ones of interoperability and scalability, with test and measurement playing a crucial role in enabling this.


At VIAVI, we are addressing the breadth and depth needed through an array of specialized tools to comprehensively test every facet required to ensure the compliance and reliability of quantum-safe systems. VIAVI offers security frameworks and validation tools that enable organizations to move quantum algorithms and architectures from theoretical models and lab environments into secure, real-world deployments.


These interoperable solutions range from[remote fiber test systems](https://www.viavisolutions.com/en-us/products/onmsi-remote-fiber-test-system-rfts) for 24-7 fault detection,[real-time sensing technologies](https://www.viavisolutions.com/en-us/products/fth-dtss) to detect physical intrusion and environmental strain to[field validation platforms](https://www.viavisolutions.com/en-us/products/oneadvisor-800-platform) for 400G/800G fiber testing and[AIOps engines](https://www.viavisolutions.com/en-us/solutions/nitro-aiops) for detecting threats and automating anomaly responses across hybrid networks.


However, the core of quantum validation relies on rigorously stressing both the physical and software layers. For the physical layer, the[VIAVI MAP-300 is a modular optical test platform](https://www.viavisolutions.com/en-us/products/map-300-multiple-application-platform) capable of creating flexible photonic systems to accurately emulate live networks and generate controlled optical stresses.


On the software side, VIAVI’s[TeraVM Security](https://www.viavisolutions.com/en-us/products/teravm-security) and[CyberFlood](https://www.viavisolutions.com/en-us/products/cyberflood) are playing an essential role in enabling organizations to be PQC ready, giving the ability to emulate tens of thousands of users and their traffic patterns/behaviors to put PQC algorithms through massive scale stress testing and validate interoperability and performance in hybrid environments.


Beyond these, VIAVI is also working with key industry players to build the ecosystems that will help accelerate adoption. This includes a[strategic partnership with QNu Labs](https://investor.viavisolutions.com/news-events/news-releases/news-details/2025/VIAVI-and-QNu-Labs-Partner-to-Accelerate-Quantum-Safe-Network-Security/default.aspx) , and a[technology partnership with quantum security specialist Squareroot8](https://itbrief.asia/story/squareroot8-viavi-forge-asia-pacific-quantum-pact) .


### **Performance Benchmarking**


The threat that quantum computing poses encryption makes preparation critical for all industries but not least sectors such as finance, healthcare, military and government. These sectors rely heavily on encryption to protect long-lived data and mission-critical transactions, making them early adopters of quantum-safe solutions. Post-quantum cryptography (PQC) offers protection, but it must be tested for performance, resilience, and compatibility. This requires tools that can assess quantum readiness prior to deployment in areas ranging from performance benchmarking to hybrid key exchange testing and fallback mechanisms.


VIAVI[CyberFlood](https://www.viavisolutions.com/en-us/products/cyberflood) test solution helps organizations prepare for the transition to quantum-safe encryption by proactively identifying potential infrastructure gaps and performance issues. CyberFlood provides a full complement of PQC cipher suites compliant with the latest NIST FIPS 203, and FIPS 204 standards. It supports hybrid KEM testing to effectively validate robust fallback strategies, pinpoint interoperability challenges and enable smoother deployments. Additionally, PQC cipher suites are fully integrated into CyberFlood’s extensive HTTP protocols and application scenarios, ensuring validation under realistic, production-like traffic conditions.
VIAVI Security Test portfolio provides powerful, easy-to-use web-based testing that generates thousands of realistic application traffic scenarios to stress-test network devices, applications, and security solutions for performance, scalability, and security efficacy in modern cloud, on-premise, and hybrid environments.


### **More information**


In March, VIAVI will be in attendance in Barcelona for MWC26. If you’d like to find out more, you can visit our Security & Quantum-Safe zone to see how we are turning quantum theory into field-ready reality.


Here you’ll be able to see demonstrations of PQC performance testing based on digital twins, as well as QKD channel evaluation that will ensure your network is resilient before, on, and after Q-Day. Our experts will be on hand to discuss how we are bridging the gap between quantum innovation and deep network expertise to protect networks and build trust in the AI era.


- ** Categories:[Industry Trends](https://blog.viavisolutions.com/category/industry-trends/)
- ** Tags:[post-quantum cryptography](https://blog.viavisolutions.com/tag/post-quantum-cryptography/) ,[pqc](https://blog.viavisolutions.com/tag/pqc/) ,[QKD](https://blog.viavisolutions.com/tag/qkd/) ,[Quantum safe communications](https://blog.viavisolutions.com/tag/quantum-safe-communications/) ,[quantum security](https://blog.viavisolutions.com/tag/quantum-security/)


-
-
-
-
-
-

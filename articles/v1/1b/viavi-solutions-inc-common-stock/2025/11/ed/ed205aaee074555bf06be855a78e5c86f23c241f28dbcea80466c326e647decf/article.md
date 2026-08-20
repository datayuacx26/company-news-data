---
schema_version: "1.0.0"
document_id: "ed205aaee074555bf06be855a78e5c86f23c241f28dbcea80466c326e647decf"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/11/03/test-and-measurement-for-validating-performance-and-compliance-of-post-quantum-secure-systems/"
published_at: "2025-11-03T17:01:22+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:55:26.781986+00:00"
content_hash: "sha256:3c763cd600bfe8f00bfde4527a25da5e8211e356a1e052b88ce39cdfce32cbe9"
---

# Test and Measurement for Validating Performance and Compliance of Post-Quantum-Secure Systems

**


# Test and Measurement for Validating Performance and Compliance of Post-Quantum-Secure Systems


- **Sameh Yamany
- ** November 3, 2025
- 1


Advances in quantum computers mean elliptic curve cryptographic (ECC) techniques will likely be cracked[in about five years](https://hackernoon.com/quantum-hackers-are-coming-is-your-encryption-ready-for-2030-and-beyond) . These advances not only include processing power (IBM’s Kookaburra processor is set to[exceed 4,000 qubits](https://insidehpc.com/2022/05/ibm-quantum-promises-4000-qubit-processor-in-2025-envisions-quantum-centric-supercomputing/) later this year), but also in error correction. As an example, Google’s Willow is a 105 qubit computer and these error correction advances have enabled it to undertake calculations in less than five minutes that are[effectively impossible for even the leading supercomputers](https://blog.google/technology/research/google-willow-quantum-chip/#:~:text=General%20summary,medicine%2C%20energy%2C%20and%20AI.) .


This forecast advance has not gone unseen by cybercriminals and the[harvest now, decrypt later](https://postquantum.com/post-quantum/harvest-now-decrypt-later-hndl/) strategy is fast being adopted.


In this blog we’ll outline the three core protective PQC algorithms, along with validation techniques and technologies that help ensure their effectiveness.


If you’d like more detail on any aspect of this blog, a more detailed white paper has also been created and this can be downloaded[here](https://www.viavisolutions.com/en-us/literature/future-proofing-communications-rise-quantum-safe-technology-application-notes-en.pdf) .


### **NIST Algorithms**


At the time of writing, three algorithms have been standardized by NIST:[FIPS 203, 204 and 205](https://csrc.nist.gov/news/2024/postquantum-cryptography-fips-approved) , with a fourth announced but deferred for being both difficult to implement consistently and vulnerable to side-channel attacks.


Each serves a slightly different function, with FIPS 203 and 204 using lattice-based techniques to respectively establish a secure connection and prove a message’s origin and authenticity, and FIPS 205 using hash-based cryptography to give an alternative approach to FIPS 204 – albeit a slower one with a larger signature size.


Adoption of these algorithms has been mandated[by the NSA](https://www.nsa.gov/Press-Room/Press-Releases-Statements/Press-Release-View/article/3498776/post-quantum-cryptography-cisa-nist-and-nsa-recommend-how-to-prepare-now/) , and other national security bodies, with national security systems needing to complete the transition to PQC[by 2030](https://www.nsa.gov/Portals/75/documents/resources/everyone/csfc/capability-packages/CSfC%20Post%20Quantum%20Cryptography%20Guidance%20Addendum%201_0%20Draft%20_5.pdf?ver=wCGPoDQXcJKEWTgbH8xsRA%3D%3D) – be it via software-coded algorithms (post quantum cryptography/PQC), hardware-based key exchange methods that use optical links (quantum key distribution/QKD) or a hybrid approach for transitional architectures.


Unfortunately, global preparation to defend against quantum attacks[appears to be lagging](https://www.isaca.org/resources/news-and-trends/newsletters/atisaca/2025/volume-9/quantum-computing-preparedness-lagging) .


Industry alliances, however, are forming with the[Post-Quantum Cryptography Alliance](https://pqca.org/) (PQCA), and the[Post-Quantum Telco Network Task Force](https://www.gsma.com/solutions-and-impact/technologies/security/post-quantum-telco-network-task-force/) (POTN) being notable examples with wide-scale backing.


### **Validating compliance**


Implementing these algorithms is not enough. They have been demonstrated to be mathematically sound, but each implementation needs to be tested to ensure its compliance.


And the three-pronged (QKD, PQC and hybrid) nature of these means there is no one-size-fits-all approach to testing.


Figure 1: Workflows showing the testing of quantum and classic channels


For QKD systems, we need to measure the qubit error rate (QBER) of the quantum channel being validated, with a high QBER being a potential indication of eavesdropping. Physical stress testing should also be undertaken, using an emulated network and a range of controlled optical stresses. Such testing must also extend to the service layer to ensure the key management system can resiliently service applications.


Conversely, the primary concern of PQC systems is the performance and architectural impact of using longer encryption keys – with algorithms tending to be more operationally heavy. Computational efficiency testing is therefore needed to examine encryption and key generation speed, as well as key size variations. Large-scale stress testing is needed here too – including the emulation of users and usage/traffic patterns to monitor KPIs such as latency and throughput under load. Finally, testing needs to include failure scenarios, for example mismatched PQC key selections, and the inability of one party to support PQC.


Finally, for hybrid systems we need to build on these techniques to take into consideration the increased complexity. Functional testing allows the validation of the quantum-classical interface and checks that timing and synchronization are stable. Security/resilience testing is also critical and should include the simulation of side-channel attacks, and the verification that the system will fall back to PQC if a QKD link fails.


As for performance validation, both classical and quantum metrics should be measured simultaneously, with the physical layer also being tested to ensure coexistence of quantum-classical traffic without noise or polarization effects.


These techniques exhibit many similarities to the testing of a classic one, however the nature of the photonic interface differs significantly. Further changes can be seen in terms of the level and type of optical stressors.


### **VIAVI’s Test and Measurement Platforms for QKD, PQC and Hybrid Systems**


In this section, we’ll break down some of the core technologies that enable the testing and validation of post quantum implementations.


Again, we’ll start with QKD systems, and their multi-layer testing process. For the physical optical layer, VIAVI has developed the[VIAVI MAP-300](https://www.viavisolutions.com/en-us/products/map-300-multiple-application-platform) . This is a reconfigurable photonic system and is used to emulate a broad range of power and spectral loading scenarios found in live networks, thereby allowing engineers to validate the resiliency of new QKD systems.


Specifically, the device is able to generate multiple optical stresses, including in-band noise from amplifiers, polarization disturbances, and reflection events, that will take QKD links to their breaking point.


For the service layer, a different platform is required, the[TeraVM](https://www.viavisolutions.com/en-us/products/teravm) . This is used to both emulate clients that fetch post-quantum pre-shared keys (PPKs) and to test the quality of experience under load as keys are continuously rotated. Using such a system therefore ensures both ETSI compliance and the validation of the QKD Key Management System’s resiliency.


Moving next to PQC systems, where the challenge is performance. Here a variant on the TeraVM system ([TeraVM Security](https://www.viavisolutions.com/en-us/products/teravm-security) ) can be used to test the impact of the algorithms. This software-based tool is capable of stress testing VPN headends encrypted with the new PQC algorithms by emulating many tens of thousands of users and their application traffic (collaboration tools, video conferences) and measuring the resulting latency, throughput, and MoS scores under load among other KPIs.


The TeraVM Security also measures key size variations and re-keying rounds and other PQC-specific metrics. And for hybrid test scenarios, the system is able to test PQC-enabled initiators against a non-PQC responder to ensure a classic VPN tunnel is still established.


To enable the workflow to be automated, VIAVI’s cloud-based VAMOS platform for managing test campaigns should also be implemented to improve resource use.


Finally, the physical fiber infrastructure itself must be secured and monitored. Here, the[ONMSi Remote Fiber Test System](https://www.viavisolutions.com/en-us/products/onmsi-remote-fiber-test-system-rfts) can be used to continually scan the fiber network using OTDR to automatically detect and locate faults or degradation that could indicate fiber tapping. Distributed Temperature and Strain Sensing systems, can also be used to monitor for the strain and temperature changes caused by physical tampering, such as bending or cutting the fiber, which would signal a possible attack.


- ** Categories:[Industry Trends](https://blog.viavisolutions.com/category/industry-trends/) ,[Uncategorized](https://blog.viavisolutions.com/category/uncategorized/)
- ** Tags:[post-quantum cryptography](https://blog.viavisolutions.com/tag/post-quantum-cryptography/) ,[pqc](https://blog.viavisolutions.com/tag/pqc/) ,[Quantum Computing](https://blog.viavisolutions.com/tag/quantum-computing/) ,[quantum security](https://blog.viavisolutions.com/tag/quantum-security/)


-
-
-
-
-
-

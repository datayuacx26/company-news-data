---
schema_version: "1.0.0"
document_id: "51285520e9628f43d3dc80941d9842aa32b1dbc2345a8bcc801893f1711a4aca"
company_key: "sealsq-corp-ordinary-shares"
company: "SEALSQ Corp"
source_id: "sealsq-corp-ordinary-shares-rss-e73bd7aa50b6"
canonical_url: "https://www.sealsq.com/sealsq-blog/why-smart-metering-security-must-evolve-beyond-pqc"
published_at: "2026-05-19T16:05:21+00:00"
first_seen_at: "2026-07-25T01:07:14.211835+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:de09db1ca031daf7d633314fe13d96b59de5f1ea2157f29366485491ca473c2c"
---

# Why Smart Metering Security Must Evolve Beyond PQC

The smart metering industry is right to accelerate its transition toward post-quantum cryptography (PQC). With the DLMS User Association finalizing Suite 3, introducing ML-KEM for key encapsulation and ML-DSA for digital signatures — the sector is preparing for the day quantum computers can break today’s classical public-key cryptography.


Regulatory timelines make this transition unavoidable. CNSA 2.0 requirements begin impacting new deployments as early as 2027, while European regulatory frameworks are expected to follow closely behind.


But quantum computing is not the only threat reshaping the security landscape.


## AI Changes the Threat Model


Modern AI systems are dramatically accelerating reverse engineering and vulnerability discovery. Firmware binaries, protocol state machines, cryptographic flows, and implementation weaknesses can now be analysed at a speed and scale far beyond traditional manual approaches.


In smart metering environments, this creates a significant challenge. DLMS/COSEM stacks may still contain:


- access-control logic flaws


- parser vulnerabilities


- timing side-channel exposures


- insecure firmware update mechanisms


Even a device implementing post-quantum cryptography can remain exposed if attackers identify and exploit weaknesses in the surrounding software environment.


The industry must now design for a new reality: quantum-resistant cryptography alone is not sufficient if the underlying security architecture remains software-dependent.


## Why Hardware Boundaries Matter


This is where hardware-rooted security becomes critical.


A dedicated, isolated Secure Element (SE) establishes a physical security boundary between sensitive cryptographic assets and the application environment. Cryptographic keys are generated and stored inside the secure boundary and never exposed in plaintext. Sensitive operations execute in an isolated environment protected against side-channel and fault-injection attacks.


This architectural model significantly reduces the software attack surface available to AI-assisted analysis and automated exploitation techniques.


The DLMS roadmap already reflects this evolution. Suite 3 / PQC is not only about adopting new algorithms, it also introduces the requirement for a PQC-ready Secure Element capable of supporting long-term cryptographic resilience.


This transition is further detailed in the *DLMS-UA Post-Quantum Cryptography Position Paper 2025* , which outlines the industry’s evolving requirements for quantum-resistant cryptography and secure hardware architectures.


[https://www.dlms.com/knowledge-hub/](https://www.dlms.com/knowledge-hub/)


## Requirements for Next-Generation Smart Meters


Devices being designed today will remain deployed well into the 2040s. Security decisions made now will define the resilience of critical infrastructure for decades.


For next-generation smart metering systems, Secure Elements should provide:


- Physical isolation from the application processor through a minimal and auditable interface


- Native support for post-quantum cryptographic operations


- Secure update capabilities to support evolving standards over long device lifecycles


- Resistance against physical and side-channel attacks


- Certification aligned with Common Criteria security requirements


These capabilities are increasingly important as regulations such as the Cyber Resilience Act (CRA) introduce stronger expectations around long-term vulnerability management and secure update mechanisms.


## Building Long-Term Trust in Critical Infrastructure


The transition to post-quantum cryptography is essential. But in long-life connected infrastructures such as smart metering, security cannot rely on algorithms alone.


As AI accelerates software analysis and attack automation, the industry must strengthen the architectural foundations of device security itself.


Hardware-rooted trust, implemented through certified Secure Elements, is becoming a key component in building resilient, future-ready smart metering infrastructures.


[Learn more about PQC Regulation](https://www.sealsq.com/resources/post-quantum-cryptography-regulation-compliance)

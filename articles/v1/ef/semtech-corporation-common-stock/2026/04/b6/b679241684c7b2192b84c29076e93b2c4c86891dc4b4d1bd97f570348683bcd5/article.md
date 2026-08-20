---
schema_version: "1.0.0"
document_id: "b679241684c7b2192b84c29076e93b2c4c86891dc4b4d1bd97f570348683bcd5"
company_key: "semtech-corporation-common-stock"
company: "Semtech Corporation"
source_id: "semtech-corporation-common-stock-rss-bc5ca864e78b"
canonical_url: "https://blog.semtech.com/lorawan-security-enterprise-grade-protection-built-into-every-byte-and-at-every-stage-of-the-byte-journey"
published_at: "2026-04-08T21:02:56+00:00"
first_seen_at: "2026-07-20T03:32:47.192655+00:00"
fetched_at: "2026-07-28T20:52:48.201959+00:00"
content_hash: "sha256:00898c689ef2ed14b9cd58c9a7a97a95c749cbbb5a202c1c3922689e8779d3a6"
---

# LoRaWAN® Security: Enterprise-Grade Protection Built Into Every Byte and At Every Stage of the Byte Journey

When evaluating low-power wide-area network (LPWAN) technologies for internet of things (IoT) deployments, security is no longer a checkbox, it's a fundamental design requirement. As regulations tighten and cyber threats grow more sophisticated, the question isn't just


*"does this technology work?"* but


*"can I trust it with my data, my devices, and my compliance obligations?"*


The answer for LoRaWAN® is a confident yes and here's why.


## A Security Foundation Designed for the Real World


Wireless networks face a unique challenge: unlike wired infrastructure, there is no physical perimeter protecting communication. Any attacker within radio range can attempt eavesdropping, replay attacks, traffic modification, or unauthorized network access.


LoRaWAN was designed from the ground up to defeat these threats through cryptography and security handshakes. Every message transmitted over a LoRaWAN network benefits from:


- **End-to-end encryption** using Advanced Encryption Standard (AES)-CTR mode per National Institute of Standards and Technology (NIST) publication SP 800-38A, ensuring that application data is readable only by the intended application server — not gateways, not network operators, not eavesdroppers.


- **Message integrity protection** via AES-CMAC (NIST) SP 800-38C, using a Message Integrity Code (MIC) that proves a frame hasn't been altered in transit.


- **Replay attack prevention** through a monotonically increasing frame counter embedded in every message and covered by the MIC calculation.


- **Device authentication** via data origin authentication, where the frame originator's identity is baked into the MIC, making spoofing cryptographically infeasible.


The cryptographic backbone is AES-128, aligned with NIST recommendations that confirm this security strength remains acceptable for a very long time


1


. Future spec revisions will also introduce crypto agility, including 256-bit AES support.


## Two Layers of Keys, One Seamless Security Model


LoRaWAN's security key scheme architecture is robust. Each device carries its own, unique root key (AppKey)


**** and identifier for IEEE-assigned Extended Unique Identifier (


EUI)


. When a device joins the network via Over-the-Air Activation (OTAA) or renews session via OTAA, these credentials are used to dynamically derive two session keys:


- **Network Session Key (NwkSKey)** : shared with the Network Server for frame integrity and network-layer encryption


- **Application Session Key (AppSKey)** : shared exclusively with the Application Server for end-to-end payload encryption


This separation ensures that no single point in the network has full visibility into both the network and application layers. OTAA is the recommended activation method because it generates fresh session keys on every join, far more secure than static, lifetime credentials.


Cloning or impersonating a device is prevented by keeping the root key strictly confidential: stored in dedicated memory or hardware that is not readable, not printed on the device, and is never exposed in documentation.


## End-Device Certification Ensures Proper Security Implementation


The LoRaWAN certified mark confirms that a manufacturer has correctly implemented the LoRaWAN standard. This rigorous process verifies that the device can securely perform OTAA, session keys derivation and correct use of session keys for encryption and MIC computations.


## Backend Security: Enterprise Standards Throughout


Security doesn't stop at the air interface. LoRaWAN's backend infrastructure (the interfaces between Network Servers, Join Servers and Application Servers) is secured using Hypertext Transfer Protocol Secure (HTTPS), virtual private network (VPN) technologies or other equivalent secured protocols, providing mutual authentication, integrity protection and confidentiality, consistent with best practices across the broader telecom and internet industry.


For deployments requiring even stronger assurances, LoRaWAN supports


**Secure Elements** (hardware-level key storage equivalent to SIM cards) in end-devices, and


**Hardware Security Modules (HSMs)** in backend servers, i.e., the same class of protection used in banking and critical infrastructure.


## Regulatory Compliance: LoRaWAN Is Ready


The regulatory landscape for IoT security is evolving rapidly and LoRaWAN is well-positioned to meet it head-on.


Since the Radio Equipment Directive Delegated Act (RED DA) cybersecurity requirements and EU Cyber Resilience Act (CRA) came into force, compliance activity across the LoRaWAN ecosystem has been picking up.


The vast majority of gateway manufacturers have announced full EN 18031 compliance and can issue CE Declarations of Conformity for their products reflecting the new cybersecurity requirements.


For end nodes, LoRaWAN's security architecture also aligns naturally with the CRA's Annex I requirements. The protocol's built-in cryptographic protections satisfy the security properties required for connected products, and the protocol support for firmware updates over-the-air (FUOTA) addresses the CRA's requirement for products to remain securable throughout their lifecycle. With the latest release of


[LoRa Basics™ Modem-E](https://www.semtech.com/products/wireless-rf/software-library-protocol-solution/) , FUOTA is now available by default over all chips that support this embedded ready-to-go pre-certified LoRaWAN stack.


LoRaWAN's security model is not a compromise, it is comparable in depth to other leading LPWAN technologies:


#### Feature


#### LoRaWAN


#### NB-IoT / LTE-M


Encryption


AES-128 end-to-end


3GPP (cellular grade)


Mutual Authentication


Yes (via Join Server)


Yes (SIM-based)


Replay Protection


Yes (frame counter + MIC)


Yes


Over-the-Air (OTA) Key Refresh


Yes (OTAA)


Yes


Secure Element Support


Yes


Yes (SIM)


FUOTA


Yes


Yes


Open Standard


Yes


Yes


Furthermore, it is worth mentioning that Semtech makes available the LoRaWAN stack (through LoRa Basics™ Modem and Unified Software Platform) under a formal software maintenance plan that incorporates a vulnerability management process, utilizing a centralized vulnerability tracking database to ensure the timely identification, assessment, and remediation of security risks.


## Want to Go Deeper?


The LoRa Alliance® has published an extensive set of resources for those who want to explore LoRaWAN security further:


-


[LoRaWAN Security, the Foundation for a Secure System](https://resources.lora-alliance.org/security/lorawan-security-the-foundation-for-a-secure-system) : a technical webinar by Alper Yegin (CTO, Actility) and Robert Cragie (Director of Security, LoRa Alliance) covering AES-128 cryptography, join procedures, FUOTA, backend infrastructure security, and regulatory compliance.


-


[LoRaWAN End-to-End Security Walk-Through](https://resources.lora-alliance.org/security/lorawan-world-expo-2022-lorawan-end-to-end-security-walk-through-july-7-2022-2) : LoRa Alliance Technical Committee leaders walk through security at every layer of the network, from device to application server.


-


[Destination LoRaWAN: Security, from the Network to the Device](https://resources.lora-alliance.org/security/destination-lorawan-lorawan-security-from-the-network-to-the-device) : a panel discussion expanding the security conversation to the full device lifecycle.


-


[LoRaWAN Security FAQ](https://resources.lora-alliance.org/security/lorawan-security-faq) : a concise summary of the most frequently asked questions about LoRaWAN security.


-


[Radio Equipment Directive Security FAQ](https://resources.lora-alliance.org/security/lora-alliance-radio-equipment-directive-security-faq) : guidance on RED compliance specifically for LoRaWAN end-device manufacturers.


-


[LoRaWAN Is Secure (but Implementation Matters)](https://resources.lora-alliance.org/security/lorawan-is-secure-but-implementation-matters) : a deeper look at cryptographic choices and how to ensure security at the implementation level.


All LoRaWAN technical specifications are publicly available at the


[LoRa Alliance Resource Hub](https://lora-alliance.org/resource-hub) .


Sign up for monthly LoRa Updates


Notes:


1


[https://csrc.nist.gov/projects/post-quantum-cryptography/faqs](https://csrc.nist.gov/projects/post-quantum-cryptography/faqs) : see question


*“*To protect against the threat of quantum computers, should we double the key length for AES now? *”*


*Semtech®, the Semtech logo, LoRaWAN®, LoRa®, and LoRa Basics™ are registered trademarks or service marks of Semtech Corporation and its affiliates. LoR a Alliance® is a registered mark of the LoRa Alliance. Other product or service names referenced herein may be trademarks of their respective owners*

---
schema_version: "1.0.0"
document_id: "dc93ebc7f449405cd67c14620ac68f455b116506db72cd9b42d3cf1fda5e1bf8"
company_key: "sealsq-corp-ordinary-shares"
company: "SEALSQ Corp"
source_id: "sealsq-corp-ordinary-shares-rss-e73bd7aa50b6"
canonical_url: "https://www.sealsq.com/sealsq-blog/are-humanoids-safe-and-secure-key-takeaways-from-lattices-security-seminar"
published_at: "2026-04-08T08:23:49+00:00"
first_seen_at: "2026-07-25T01:07:14.211835+00:00"
fetched_at: "2026-07-28T22:15:58.980681+00:00"
content_hash: "sha256:1bd27c605ea56fe6af3011efa6274e9aea39b6a172c2c3cc0bd9d8f7e7423a84"
---

# Are Humanoids Safe and Secure? Key Takeaways from Lattice’s Security Seminar

As humanoid robots move closer to real-world deployment, the conversation is shifting from innovation to trust. Beyond performance, one question stands out:


##


## **Can these systems be truly secure?**


That was the focus of a recent seminar hosted by Lattice Semiconductor, bringing together experts from across the ecosystem, including Eric Sivertson, Petr Shamsheyeu, and Steve Clark.


****


##


## **Safety Isn’t Enough Anymore**


Humanoid robots are cyber-physical systems. Safety has always been a priority—but security is now just as critical. A robot can be mechanically safe, yet still vulnerable.


Once connected, it becomes exposed to:


- Remote exploitation
- Data interception
- Behavioral manipulation


As Eric Sivertson put it during the session: **“You can have a safe humanoid but if it’s connected and vulnerable, it can still be exploited.”**


And unlike traditional devices, humanoids interact with the real world. A breach doesn’t just compromise data, it can impact physical environments.


****


##


## **Security Must Start at the Hardware Level**


The panel made one thing clear: security can’t be layered on later. It must be built from the ground up, combining:


- Hardware-based protection
- Trusted device identity
- Secure boot and firmware validation
- Encrypted communications


Technologies like FPGA enable real-time threat detection, while TPMs anchor trust at the hardware level. Together, they create systems that are not only functional but resilient.


In parallel, insights shared by Petr Shamsheyeu highlighted the importance of **hardware-level safety mechanisms** , such as sensor fusion and real-time validation, to reduce risks like delayed response or false positives in safety-critical environments.


## **Quantum Threat Is Driving Immediate Action**


As Steve Clark underlined, Quantum Threat isn’t a future problem, it’s already underway.


Today’s encryption (RSA, ECC) will be broken by future quantum computers. That creates a critical risk: “harvest now, decrypt later.” (HNDL)


Sensitive data captured today could be exposed tomorrow. And regulators are not waiting:


- The National Institute of Standards and Technology (NIST) has already standardized Post-Quantum Cryptography (PQC)


algorithms (FIPS 203–205)
- The National Security Agency (NSA), through CNSA 2.0, is mandating quantum-resistant cryptography for national security systems
- The U.S. National Security Memorandum 10 requires agencies to inventory and prepare migration
- In Europe, the Cyber Resilience Act (CRA) is introducing mandatory cybersecurity requirements for connected products, including long-term security, vulnerability management, and crypto-agility


The timeline is set:


- Start preparing now
- Be quantum-ready by ~2027
- Migrate at scale by ~2030
- Complete the transition by ~2035


For systems designed to last 10–20 years, like humanoid robots, this is not optional, it’s a design constraint.


## **Identity Is the New Security Perimeter**


Beyond encryption, Steve highlighted a more fundamental layer: device identity. In a world of autonomous systems, trust starts with knowing:


- What the device is
- Whether it’s authentic
- Whether it has been tampered with


This is where PKI (Public Key Infrastructure) becomes critical. It enables:


- Strong authentication
- Secure communications (TLS)
- Trusted interactions between systems


Combined with PQC, it ensures that trust holds even against future threats.


## **TPM: Anchoring Trust in the Device**


The Trusted Platform Module (TPM) plays a central role in enforcing this trust. It provides:


- Secure key storage
- Cryptographic operations
- Verification of the entire boot chain


In practice, this means:


- Only trusted firmware runs
- Systems can prove their integrity
- Communications remain authenticated


For humanoids operating in open environments, this hardware root of trust is essential.


## **A Converging Security Architecture**


What emerged from the discussion is a clear architecture for securing humanoids:


- FPGA → real-time control and hardware-level safety
- TPM → root of trust and secure identity
- PQC + PKI → future-proof communication and authentication


This combination delivers something critical: deterministic, verifiable, and resilient systems.


And while humanoids are the focus, the same principles apply across:


- Robotics and drones
- Industrial IoT
- Smart infrastructure


## **The Real Challenge: Long-Term Trust**


Humanoid robots won’t be short-lived devices. They’ll operate for a decade or more. That changes everything.


Security must:


- Evolve over time
- Support secure updates
- Adapt to new threats


As Steve pointed out, a robot that becomes insecure five years after deployment isn’t just obsolete, it could become a liability.


## **Securing Autonomy and Agency**


Looking ahead, another challenge is emerging: delegated autonomy. Humanoids won’t just execute commands, they’ll act on behalf of users (Booking services. Accessing systems. Making decisions, …)


That raises a new question: How do you secure agency?


Defining and enforcing what a robot is allowed to do and preventing abuse, will be a key frontier in cybersecurity.


## **Designing for Trust Starts Now**


The message from the seminar is clear: **Security is not a feature. It’s a foundation.**


As humanoid technologies evolve, trust will determine adoption.


That means:


- Designing security from day one
- Leveraging proven standards and technologies
- Collaborating across the ecosystem


SEALSQ, alongside its partners, is helping organizations build that foundation—combining device identity, PKI, and post-quantum readiness to secure the next generation of connected systems.


[Watch Security Seminar Replay](https://www.youtube.com/watch?v=xxGrticCprQ)

---
schema_version: "1.0.0"
document_id: "b015f46320203187ef1320f7084bba30f17a5b995ed7491384e8af1856e47e82"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/workdays-post-quantum-cryptography-migration-how-we-re-securing-your-data-for-the-future-a9106e593c5c"
published_at: "2026-06-22T20:54:13+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-08-20T00:44:57.479297+00:00"
content_hash: "sha256:24a7cb8da8ccb015e72cea5f94bf50dba3d3c5a606efe2bc7a707049d52b601f"
---

# Workday’s Post-Quantum Cryptography Migration: How We’re Securing Your Data for the Future

### Executive Summary


At Workday, our primary mission is to ensure the enduring confidentiality of your HR and financial data — stopping adversaries from stealing and reading it later — and its absolute integrity, preventing anyone from forging signatures or faking identities. One of the emerging security challenges we’re getting ahead of is the increasing power of quantum computers, which have the potential to eventually break today’s standard encryption.


Traditional cryptographic standards, such as[RSA (Rivest-Shamir-Adleman)](https://csrc.nist.gov/glossary/term/rsa) and[ECC (Elliptic Curve Cryptography)](https://csrc.nist.gov/glossary/term/elliptic_curve_cryptography) , are safe for now. However, they face a severe long-term risk from future quantum attacks. With NIST finalizing its official post-quantum recommendations, we now have the standardized cryptographic blueprints needed to deploy a resilient defense.


To neutralize this ahead of time, Workday is launching a comprehensive[Post-Quantum Cryptography (PQC)](https://csrc.nist.gov/projects/post-quantum-cryptography) Migration Program. We are beginning the rollout in 2026, targeting full platform migration and permanent operational agility for 2028 and beyond. We are actively engineering our global SaaS infrastructure to transition to quantum-resistant algorithms, making sure your data remains protected for decades to come.


**Note on Bulk Encryption:** This migration focuses primarily on the asymmetric layers (RSA/ECC) that protect data in transit and identities. Workday’s bulk data-at-rest encryption already utilizes **AES-256** , which is inherently considered quantum-resistant.


### The Threat: Addressing “Trust Now Forge Later” and “Harvest Now, Decrypt Later”


The focus of our PQC migration is driven by two specific adversarial tactics. Security threats usually exploit live vulnerabilities, but quantum computing introduces a unique challenge: retroactive risk.


Adversaries are actively intercepting enterprise traffic *today* , banking on the fact that[Cryptographically Relevant Quantum Computer (CRQC)](https://csrc.nist.gov/glossary/term/crqc) quantum computers will mature before that data loses its value.


- [Harvest Now, Decrypt Later (HNDL):](https://postquantum.com/quantum-security-reference/what-is-trust-now-forge-later/) A passive attack vector where adversaries intercept and archive encrypted data payloads today, intending to retroactively decrypt them once a quantum computer is available.
- [Trust Now, Forge Later (TNFL):](https://postquantum.com/quantum-security-reference/what-is-trust-now-forge-later/) The digital signature counterpart to HNDL, where attackers collect identity tokens and code-signing keys today to forge authenticated, malicious variants that systems will inherently trust in the future.


#### Quantifying the threat: Mosca’s Inequality


Workday uses a comprehensive risk formula ([Mosca’s inequality](https://cdn.prod.website-files.com/63ef0996726f31b9968ba679/648c8e28cfee25748915738f_a-methodology-for-quantum-risk-assessment-pdf.pdf) ) to evaluate our security horizon and the quantum threat:


**X+Y > Z**


- X (Data Shelf Life): The duration for which your data must remain secured.
- Y (Migration Time):The time required to transition our entire ecosystem to PQC (estimated at 2–4 years).
- Z (Quantum Maturity): The time until a CRQC is physically realized.


To stay ahead of the TNFL and HNDL threats, Workday is transitioning to Post-Quantum Cryptography. While government and private sectors track toward NIST’s 2030 and 2035 mandate to deprecate traditional crypto, we are prioritizing early adoption to neutralize the risk of future retroactive decryption. That is, we’re shrinking the “Y” variable starting today to protect our customers’ most sensitive data from the quantum threats of tomorrow.


### Our Technical Framework: The “Hybrid” Advantage


Workday is aligning with the standards finalized by NIST in August 2024. Our strategy utilizes a **Hybrid Deployment Model** , the industry gold standard for a stable and secure transition.


#### Algorithmic Targets & Maturity


We distinguish between technologies that are ready for immediate scale and those in early-stage development.


### Defining the Hybrid Deployment Model


Because post-quantum algorithms are relatively new, deploying them by themselves introduces operational risk — unforeseen software bugs or mathematical vulnerabilities could theoretically exist, which could cause data to be decryptable today, even without quantum computers. To eliminate this risk, Workday does not completely swap old math for new math. Instead, we use a hybrid model.


**How it works:** A hybrid deployment combines a traditional, battle-tested classical algorithm (like ECC or RSA) with a new, quantum-safe algorithm (like ML-KEM) inside a single cryptographic operation.


This dual-layer approach provides two vital advantages that echo throughout our entire roadmap:


1. **Dual-Layer Security:** To compromise a hybrid connection, an attacker must defeat both algorithms. In the context of HNDL, if a mathematical flaw is ever discovered in the new PQC algorithm, the data remains fully protected by the underlying classical layer.
2. **Seamless Backward Compatibility:** Public internet traffic is incredibly diverse. By utilizing hybrid certificates, our servers can gracefully negotiate traditional, classical connections for older client systems while automatically elevating modern, compatible endpoints to quantum-safe status — all without breaking the user experience or requiring independent infrastructure.


### **Engineering the Post-Quantum Transition: Our Strategic Roadmap (2026–2028)**


Migrating a complex infrastructure to post-quantum cryptography (PQC) is less about a single “switch-flick” and more about managing risk, dependencies, and regulatory mandates. Our 24-month roadmap is explicitly prioritized based on data exposure vectors and strict regulatory deadlines, most notably the Commercial National Security Algorithm Suite ([CNSA 2.0](https://media.defense.gov/2025/May/30/2003728741/-1/-1/0/CSA_CNSA_2.0_ALGORITHMS.PDF) ) timelines, which mandate post-quantum software signing by 2027 and full system migration between 2030 and 2033.


#### Laying the Groundwork: Discovery and Architecture (2026–2027)


Before swapping out algorithms, we have to map our dependencies. The opening phase of our initiative focuses on executing a comprehensive, automated inventory of every cryptographic asset across our repositories, data stores, and cloud infrastructure. We aren’t just looking for TLS certificates; we are identifying hardcoded keys, legacy cipher suites, and underlying libraries. The engineering output of this phase is twofold: establishing a foundational “crypto-agility” framework — allowing us to update algorithms via configuration rather than code rewrites — and finalizing our blueprint for a PQC-ready architecture that complies with emerging NIST standards.


#### Frontline Defense: Hybrid TLS and Defeating TNFL and HNDL (2027)


Our immediate external priority is neutralizing “Trust Now, Forge Later” (TNFL) and “Harvest Now, Decrypt Later” (HNDL) threats. Because public-facing endpoints are the most exposed to this vector, our second phase focuses on deploying hybrid ML-KEM key exchanges across all edge proxies. By blending classical ECDH with quantum-safe ML-KEM, we ensure that session negotiation remains securely retrofitted against future quantum decryption without breaking compatibility for legacy clients that don’t yet support PQC. Crucially, our architecture implements strict anti-rollback controls to prevent downgrade attacks; if a client demonstrates PQC capability, our proxies mandate the quantum-safe layer and explicitly block malicious attempts to force a fallback to classical-only ciphers.


#### Deep Infrastructure: Internal Services and Public Key Infrastructure (2027–2028)


Once the perimeter is secured, the migration moves deep into our internal network topology and identity layers. This phase represents the heaviest engineering lift, requiring systematic upgrades to our internal service mesh for post-quantum mutual TLS (mTLS). Simultaneously, we will be refactoring our authentication pipelines — specifically upgrading SAML and OAuth token signing mechanisms — alongside our centralized Key Management Systems (KMS). Crucially, to hit the 2027 CNSA 2.0 milestone, our code-signing infrastructure will be transitioned to stateful hash-based signatures or ML-DSA to guarantee supply chain integrity.


#### Long-Term Agility and Legacy Deprecation (2028+)


The final phase shifts from active migration to operational sustainability. By 2028, we expect to fully implement automated crypto-agility controls capable of dynamically shifting algorithms based on real-time vulnerability data. With the core technical debt resolved, our focus turns to deprecating legacy, non-quantum-safe cryptographic protocols across all environments. This phase is anchored by ongoing engineering training programs and continuous policy compliance monitoring, ensuring that any new microservice or feature deployed to production is quantum-safe by default.


### What This Means for Our Customers


Workday’s commitment to hybrid standards is designed to maintain backward compatibility. However, PQC migration is an ecosystem-wide shift that requires coordination across infrastructure teams.


- **Infrastructure & Browser Compatibility:** Most modern environments — including Google Chrome (124+), Go (1.23+), and OpenSSL (3.0+) — already natively support hybrid post-quantum key exchanges.
- **Network Middleware & Firewalls:** Because quantum-resistant algorithms require larger key sizes, a hybrid TLS ClientHello message grows to roughly 1,400 bytes. This causes the handshake to exceed standard Maximum Transmission Unit (MTU) limits and fragment across multiple TCP packets. Customers should verify that their internal firewalls, load balancers, and Web Application Firewalls (WAFs) are configured to permit packet-fragmented TLS handshakes and dual-algorithm certificates.
- **SAML & Federation:** PQC for SAML is tied to emerging XML Signature standards and third-party Identity Provider (IdP) support. Workday is actively tracking these cross-industry dependencies to ensure seamless authentication interoperability.
- **External Integrations:** As legacy RSA and ECC key pairs are phased out globally, configuration endpoints and certificate pin lists for custom external integrations will eventually require routine rotation.
- **Regulatory Readiness:** Workday’s proactive timeline ensures our infrastructure inherently satisfies emerging NIST, EU, and CNSA 2.0 compliance mandates well before federal deadlines take effect.


### Closing


The quantum threat is not a distant hypothetical; it is an active one. Adversaries harvesting encrypted traffic today are betting that a cryptographically relevant quantum computer will arrive before your data loses its value. Workday’s PQC Migration Program is our answer to that bet: a structured, standards-aligned transition engineered to ensure that the HR and financial data you entrust to us remains secure not just against today’s threat landscape, but for the decades ahead.


By adopting NIST’s finalized standards (ML-KEM, ML-DSA, and SLH-DSA) and pioneering a hybrid deployment approach, we are moving now — without waiting for regulatory mandates to force the issue. For us, crypto-agility is not a temporary project or a one-time migration; it is a permanent shift in how we build, deploy, and operate our global infrastructure.


---


[Workday’s Post-Quantum Cryptography Migration: How We’re Securing Your Data for the Future](https://medium.com/workday-engineering/workdays-post-quantum-cryptography-migration-how-we-re-securing-your-data-for-the-future-a9106e593c5c) was originally published in[Workday Technology Blog](https://medium.com/workday-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.

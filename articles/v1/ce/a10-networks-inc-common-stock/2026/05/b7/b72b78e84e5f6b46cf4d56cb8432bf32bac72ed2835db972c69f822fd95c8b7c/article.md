---
schema_version: "1.0.0"
document_id: "b72b78e84e5f6b46cf4d56cb8432bf32bac72ed2835db972c69f822fd95c8b7c"
company_key: "a10-networks-inc-common-stock"
company: "A10 Networks Inc."
source_id: "a10-networks-inc-common-stock-rss-284845abe605"
canonical_url: "https://www.a10networks.com/blog/post-quantum-cryptography-comes-to-a10-ssl-tls-data-plane/"
published_at: "2026-05-07T17:12:49+00:00"
first_seen_at: "2026-07-23T23:21:56.777318+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:4405e43f7c5d650b6de131647823017e0e249222164848d5565f265595b5e27e"
---

# Post-quantum Cryptography Comes to A10 SSL/TLS Data Plane

## Hybrid KEM Support in ACOS 7.0.3


The U.S. National Institute of Standards and Technology finalized its first post-quantum cryptographic standards in 2024. While large-scale, cryptographically relevant quantum computers may still be years away, the security decisions we make today must account for tomorrow’s threats. One of the most serious cyber threats is “harvest now, decrypt later,” where encrypted traffic is captured today and stored for future decryption. This risk arises because current public-key cryptography can be broken by future quantum computers. Post-quantum cryptography and hybrid key exchange are required now to ensure long-term data confidentiality.


To address this reality, we are launching a software platform built from the ground up around the NIST-standardized post-quantum algorithm, hybrid post-quantum cryptography (PQC) support in[Advanced Core Operating System (ACOS)](https://www.a10networks.com/solutions/advanced-core-operating-system/) 7.0.3, enabling customers to begin their PQC transition now, without waiting for new hardware or ecosystem maturity.


## Why Post-quantum Cryptography Matters Now


Conventional public‑key cryptography relies on mathematical problems that are difficult for classical computers to solve but are expected to become vulnerable with the rise of quantum computing—putting widely used algorithms such as RSA and elliptic‑curve cryptography at long-term risk.


[Post‑quantum cryptography](https://www.youtube.com/watch?v=7sPsj5wasEs) addresses this threat by introducing quantum resistant algorithms designed to protect data against future quantum adversaries.


Rather than abruptly replacing proven cryptographic methods, A10 implements the industry standard which is a hybrid KEM approach, combining classical and post‑quantum algorithms in parallel. With both key exchanges executed simultaneously, an attacker would need to break both to compromise a session.


For[SSL/TLS](https://www.a10networks.com/solutions/security/ssl-tls-inspection/) , A10 prioritizes key establishment, as the security of the entire session ultimately depends on the strength of the key exchange. Even the strongest encryption and authentication mechanisms cannot protect a session if the key exchange itself is compromised—making hybrid KEM the most practical and secure first step toward quantum resilience.


## PQC Strategy in ACOS 7.0.3


With ACOS 7.0.3, A10 introduces hybrid KEM support in the SSL/TLS data plane using OpenSSL 3.5.


- Post‑quantum Key Exchange: Establishes encryption keys using hybrid mechanisms that pair classical cryptography with postquantum algorithms to remain secure even against future quantum computers.
- Elliptic‑curve Cryptography (Classical Component): Provides proven, efficient cryptographic security today and serves as a trusted component within hybrid post-quantum designs during the transition to quantum‑safe standards.
- Hybrid Key Establishment Algorithms Supported


- X25519 + ML‑KEM‑768
- secp256r1 + ML‑KEM‑768
- secp384r1 + ML‑KEM‑1024


Client-side SSL (7.0.3) Server-side SSL (ACOS 7.0.3)


PQC enabled by default Crypto-agile by design using dual-key share model


Cryptographic preference order:


- X25519 + ML-KEM-768 (hybrid KEM)
- X25519
- secp256r1
- secp384r1
- secp521r1


Configure both hybrid KEM and legacy groups using same classical curve


Auto negotiates the strongest supported group Back-end servers can select either hybrid or legacy key share during the TLS handshake


Administrators can explicitly control behavior using supported-group configuration under SSL template No additional handshake messages, preserving performance and saving one RTT


Operationally simple and transparent to existing clients Enables seamless coexistence for mixed client/server population during PQC transition


Provides immediate PQC protection for inbound TLS Allows gradual, risk-managed PQC rollout without compatibility penalties


This enables PQC adoption without new hardware, allowing customers to:


- Begin PQC testing and validation immediately
- Gain operational experience with PQ algorithms


And align with evolving NIST standards while maintaining production safety.


## Preparing for a Moving Target—Safely


Post-quantum cryptography is not a single event. It’s a journey. Algorithms will evolve. Standards will change. Some candidates may be deprecated.


A10’s philosophy is simple:


- Start with software
- Adopt hybrid models
- Preserve crypto agility


With software based PQC support in ACOS 7.0.3, A10 customers can confidently begin their post‑quantum journey today—without disrupting existing deployments or betting on unproven assumptions.


### Share this post:


[Share on X (Twitter)](https://twitter.com/intent/tweet?text=Post-quantum%20Cryptography%20Comes%20to%20A10%20SSL%2FTLS%20Data%20Plane&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fpost-quantum-cryptography-comes-to-a10-ssl-tls-data-plane%2F)[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fpost-quantum-cryptography-comes-to-a10-ssl-tls-data-plane%2F)[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fpost-quantum-cryptography-comes-to-a10-ssl-tls-data-plane%2F&title=Post-quantum%20Cryptography%20Comes%20to%20A10%20SSL%2FTLS%20Data%20Plane&source=https%3A%2F%2Fwww.a10networks.com)Share on Email


Categories:


[A10 News](https://www.a10networks.com/blog/category/a10-news/)[Cyber Security](https://www.a10networks.com/blog/category/cyber-security/)[Network Security](https://www.a10networks.com/blog/category/network-security/)


---


** Previous Post


[Carpet-Bombing DDoS: The Attack Pattern Your Per-IP Defenses Won’t Catch](https://www.a10networks.com/blog/carpet-bombing-ddos-the-attack-pattern-your-per-ip-defenses-wont-catch/)


Next Post **


[The Most Famous DDoS Attacks in History](https://www.a10networks.com/blog/5-most-famous-ddos-attacks/)


---


[Suman Rajaraman](https://www.a10networks.com/blog/author/srajaraman)


| May 7, 2026

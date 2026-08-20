---
schema_version: "1.0.0"
document_id: "85203b4cd8362eb26eff793a6564427cde18fa282085f785d1da4a39f8a1e3ca"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/confidential-computing-compliance-gap-regulated-industries"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-24T16:50:44.532118+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:c55834c363244f25953c37305015f9f4ed3e10343e92d337fee37bcfd8956ca6"
---

# How Confidential Computing Closes the Compliance Gap in Regulated Industries

Organizations today handle far more than data-at-rest or data-in-motion. With modern workloads including analytics, AI/ML, data collaboration, and multi-party processing, a significant portion of data is being actively computed on, the *data in use* . Traditional security approaches like disk encryption and TLS do not protect this critical phase, leaving a gap that can be exploited by malicious insiders, compromised cloud administrators, or sophisticated cyberattacks.


This security gap has become urgent for two reasons. First, the volume and sensitivity of processed data — financial transactions, health records, customer analytics, AI training data, intellectual property, trade secrets — continue to grow. Second, regulatory and compliance regimes are evolving to demand stronger protections not just for storage and transfer, but for how data is processed, who can access it, and how resilient the processing environments are.


With this backdrop, confidential computing is not a nice-to-have. It is increasingly becoming an essential tool in the enterprise security and compliance toolbox.


## Accelerating Adoption: What the Data Shows


New[IDC reports](https://www.prnewswire.com/news-releases/new-study-finds-confidential-computing-emerging-as-a-strategic-imperative-for-secure-ai-and-data-collaboration-302631213.html) from December 2025 show a dramatic shift in confidential computing adoption. This mirrors recent findings from[Gartner,](https://www.gartner.com/en/newsroom/press-releases/2025-10-20-gartner-identifies-the-top-strategic-technology-trends-for-2026) which pegs confidential computing as a **Top 3 CIO strategic initiative for 2026** .[Apple](https://security.apple.com/blog/expanding-pcc/) has deployed confidential computing as a critical enabler for its next-generation private AI capability.[Microsoft](https://www.techtimes.com/articles/318275/20260612/london-tech-week-2026-closes-microsofts-30b-uk-push-puts-505000-nhs-staff-copilot.htm) is using it to meet increasingly strict data sovereignty requirements.


According to a[2025 white paper](https://www.prnewswire.com/news-releases/new-study-finds-confidential-computing-emerging-as-a-strategic-imperative-for-secure-ai-and-data-collaboration-302631213.html) sponsored by the Confidential Computing Consortium and produced by IDC, **75% of surveyed organizations are already using confidential computing** — 18% in production and 57% in pilot or testing. This mirrors what we see with our financial services customers, who are rapidly increasing use of Trusted Execution Environments (TEEs) for strategic risk mitigation.


Key benefits cited by adopters include:


- **Improved data integrity** — cited by 88% of respondents
- **Confidentiality with technical assurance** — 73%
- **Enhanced regulatory compliance** — 68%


Many organizations also point to confidential computing as a strategic business enabler, unlocking secure multi-party data collaboration, privacy-preserving analytics, secure AI/ML pipelines, and stronger sovereignty over data and workloads.


## Regulatory Frameworks Raising the Bar: DORA


A strong example of emerging regulation is the EU's[Digital Operational Resilience Act (DORA) ,](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en) which entered into application on January 17, 2025. DORA is a unified,[binding regulation](https://www.itgovernance.co.uk/eu-digital-operational-resilience-act) covering banks, insurers, investment firms, payment services, crypto-asset service providers, and their third-party ICT providers.


Its[core obligations](https://www.itgovernance.co.uk/eu-digital-operational-resilience-act) include:


- Establishing an ICT risk management framework
- Classifying, managing, and controlling ICT-related risk (cyber risks, system failures, third-party dependencies)
- Mandatory reporting of major ICT incidents to competent authorities
- Digital operational resilience testing to validate that systems can withstand disruptions
- Protection of data in all states (in-transit, in-storage, and most notably *in-use)*
- Third-party ICT risk management with contractual assurances and governance over dependencies
- Information sharing on cyber threats and vulnerabilities across financial sector actors


DORA is not only about confidentiality and integrity. It demands availability and resilience. Systems must be robust, tested, and capable of continuing secure operations under disruption. Financial institutions and their ICT providers must demonstrate data security, prove operational resilience, and maintain control over their computing supply chain.


## How Confidential Computing Directly Supports Compliance


Data is at its most vulnerable not when stored or transmitted, but when it is actively being processed. Traditional encryption-at-rest and in-transit controls leave data exposed in memory during computation. Confidential computing addresses this security gap directly.


### Closing the data-in-use gap


- **Hardware-enforced isolation:** Confidential computing uses TEEs (secure enclaves in hardware) to ensure data and code run in an isolated environment, separate from the host OS or hypervisor. Even if the host is compromised, data inside the TEE remains protected from hackers or insider threats.
- **Encryption during processing:** Data inside a TEE is only decrypted within the enclave during computation, and never exposed outside in plaintext form.
- **Attestation and verifiability:** TEEs support remote attestation using cryptographic proofs that confirm the environment's security state and that the code running is exactly what is expected. Organizations and regulators can verify that sensitive computations were performed under the correct security conditions.


In effect, confidential computing completes full-stack data protection: at rest, in motion, and in use.


### Supporting resilience, control, and auditability


- By isolating processes at the hardware level, confidential computing reduces attack surfaces — even from privileged insiders or cloud operators — strengthening overall ICT security posture.
- Remote attestation and cryptographic proofs turn compliance from a paper-based exercise into a **verifiable, technical artifact** . Organizations can show instead of tell that data was processed securely. This aligns directly with modern compliance demands for auditability.
- With confidentiality, integrity, and availability preserved during active processing, confidential computing supports regulations like DORA that require consistent service delivery, operational resilience, and continuity even under cyber incidents.


## Anjuna Simplifies and Streamlines Compliance


While confidential computing provides powerful protections, deploying and managing TEEs across complex, multi-cloud, hybrid, or multi-tenant environments is not trivial. This is where Anjuna comes in.


- Anjuna abstracts the complexity of TEE hardware, attestation, and enclave management, providing a developer-friendly, enterprise-ready platform that does not require deep kernel, cryptography, OS, or hardware expertise.
- By integrating with existing infrastructure — on-premises, cloud, or hybrid — Anjuna enables compliant workloads across diverse deployment models, helping meet data sovereignty, residency, and regulatory-governance requirements without major architectural rewrites.
- Anjuna's platform supports strong audit trails. Since enclaves provide cryptographic attestation, organizations can produce verifiable proofs for regulators, auditors, and internal compliance teams, demonstrating that sensitive data was processed securely in an approved environment.
- With confidential computing and Anjuna, organizations can confidently pursue advanced use cases including secure data sharing, multiparty analytics, AI/ML on sensitive datasets, and cross-organization collaboration while staying within compliance guardrails.


## Conclusion: Confidential Computing as Compliance-First Architecture


Confidential computing bridges a critical gap in data security by protecting data in use. For industries subject to rigorous regulation, it maps to the full spectrum of compliance requirements: confidentiality, integrity, and availability during active processing; hardware-based isolation and cryptographic attestation for verifiable compliance proofs; and management of third-party and supply-chain risk by enabling sensitive workloads to run securely even on untrusted infrastructure.


As regulators evolve beyond data-at-rest and in-transit protections toward full lifecycle security, organizations must rethink their foundational assumptions. Confidential computing represents a paradigm shift: **from trust-based security to proof-based, verifiable security.** For heavily regulated sectors such as finance under DORA, healthcare under HIPAA or GDPR, government, and critical infrastructure, confidential computing is not merely a security enhancement. It becomes a compliance enabler and a strategic differentiator.


Realizing confidential computing's full potential requires more than deploying the underlying technology. Enterprises need the expertise and tooling to launch confidential computing-enabled services with strong trust models, high performance, ease of use, and multi-cloud flexibility. For regulated organizations dealing with sensitive data and demanding compliance requirements, the combination of confidential computing and a mature platform like Anjuna's makes a compelling case for confidential computing as compliance-first architecture.


To learn more about how Anjuna is helping enterprises embrace confidential computing without complexity,[start with a free trial](https://www.anjuna.io/anjuna-free-trial) or[talk to our team](https://www.anjuna.io/contact-sales) .

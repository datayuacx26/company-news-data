---
schema_version: "1.0.0"
document_id: "92eb8f620d0effdfd4d7194554de142a97e27662ea2e5bd6ce971ad1399d537e"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/cisas-sase-guidance-changes-the-tic-compliance-calculus"
published_at: "2026-08-06T07:01:51+00:00"
first_seen_at: "2026-08-10T01:01:10.452862+00:00"
fetched_at: "2026-08-10T01:01:12.343007+00:00"
content_hash: "sha256:14f70183231288d5a8a6a47fce399a7cdecec52e8fbdf9b98a34b70b020c14cc"
---

# CISA’s SASE Guidance Changes the TIC Compliance Calculus

Federal agencies have spent nearly two decades routing traffic through Managed Trusted Internet Protocol Services (MTIPS) gateways built for a world that no longer exists. While the architecture kept running, the environment moved on without it.


In June 2026, the Cybersecurity and Infrastructure Security Agency (CISA) published


[The Journey to Zero Trust: Using Secure Access Service Edge in a Modern TIC 3.0 Solution](https://www.cisa.gov/resources-tools/resources/using-sase-modern-tic-30-solution) , the clearest federal signal yet that the centralized gateway model has run its course. The policy foundation goes back to OMB M-19-26 (2019), which removed the mandatory routing requirement that defined Trusted Internet Connections (TIC) 2.0. TIC 3.0 is CISA’s operational implementation of that shift. Secure access service edge (SASE) is now explicitly named as a compliant path forward.


The core change is philosophical: The old policy prescribed where security controls had to live. TIC 3.0 prescribes what they have to accomplish. Agencies can distribute enforcement across a cloud-delivered architecture, provided they maintain visibility and feed telemetry to CISA’s Comprehensive Log Aggregation Warehouse (CLAW) at the logging levels the guidance specifies. The changes mean that the architecture is flexible, but accountability is not.


CISA is also clear that while SASE enables zero trust, it doesn’t replace it. Agencies still need strong identity and access controls, continuous monitoring, and least privilege enforcement.


## TLS inspection: where the real debate is


One section of the guidance deserves more scrutiny than it’s been getting. CISA describes transport layer security (TLS)/SSL break and inspect (B&I) as “no longer a universally recommended solution,” pointing to AI and machine learning (ML) analysis of encrypted traffic as an alternative. The reasoning behind that language matters: Certificate pinning, the wider adoption of TLS 1.3 and QUIC protocols, and emerging post-quantum cryptography (PQC) cipher suites all create real engineering complexity.


The engineering complexity is real. But it should be a question of


how


to inspect, not


whether


to inspect. Zero trust is built on continuous verification, and skipping decryption doesn’t reduce risk, it eliminates visibility. What looks like a policy accommodation is, in practice, a return to perimeter-based implicit trust.


## What gets bypassed


Without decrypting traffic, you bypass data security engines entirely. DNS filtering, FQDN/URL filtering, and JA3 signatures tell you where the traffic is going, not what it contains. Skipping decryption also means losing API and JSON decoding, which identifies the actual application, the specific instance, the logged-in identity, and what action was taken. Without it, you know 200MB went to a Microsoft endpoint. You don’t know it went to a personal OneDrive instance where a user uploaded sensitive government data.


For AI traffic, you know a prompt was sent. You don’t know it contained PII or that an AI agent was executing unauthorized actions across your environment. Human-in-the-loop practices exist in AI deployments precisely because autonomous systems behave in unexpected ways. The July 2026


[Hugging Face breach](https://www.netskope.com/blog/when-the-attacker-is-your-own-model-what-the-hugging-face-incident-means-for-security-leaders) , where an AI agent executed thousands of actions through encrypted sessions to compromise infrastructure, illustrates what happens without one. Adversaries exploit the same blind spot: Attackers routinely use Microsoft 365 tenants for malware hosting and command-and-control precisely because security tools bypass decryption for trust domains. Behavioral analysis tells you something looks wrong but TLS inspection tells you what data moved, where it went, and whether it should have left the environment at all.


## The compliance gap


CISA requires agencies to feed high-fidelity telemetry to CLAW. Without TLS decryption, that telemetry is source, destination, and bytes transferred. With decryption, agencies are able to provide Layer-7 contextual data such as application, activity, identity, and context, which is the level of visibility that TIC 3.0 is designed to achieve.


## Addressing the challenges


The engineering challenges are real. The answer is to solve them, not to accept a blind spot. Netskope’s NewEdge architecture is built for exactly this: a single-pass inspection engine that decrypts traffic once and runs all policy engines in parallel, eliminating the performance bottleneck that makes full decryption difficult for legacy proxy architectures. Configurable bypass policies handle certificate-pinned applications. TLS 1.3 is natively compatible with active inline proxy architecture. QUIC redirects to inspectable HTTPS automatically. Context-driven policies let agencies exempt privacy-sensitive categories like banking or healthcare without sacrificing broader inspection. As agencies move toward National Security Memorandum 10 (NSM-10) post-quantum cryptography requirements, the urgency increases. Adversaries are already archiving encrypted federal data today for future decryption. CISA’s own guidance confirms TLS B&I may still be enabled with endpoint detection and response (EDR) for compensating coverage.


At Netskope, we believe the right architecture runs both behavioral analysis and inline inspection, each doing what it does best.


## Where we fit


[Netskope One](http://netskope.com/products/agentic-broker) was built as a data security platform first. Zero trust requires continuous verification, and verification requires visibility into the payload, not just the connection. That’s why TLS inspection isn’t just a feature added to a network access product, it’s how the platform is designed. Netskope’s NewEdge architecture decrypts traffic once and runs all policy engines in parallel, so agencies get full inline inspection across SaaS, web, private apps, and AI traffic without the performance trade-offs that lead other platforms to recommend bypasses.


For agencies implementing TIC 3.0, that inspection feeds CISA’s CLAW at the logging levels the guidance requires. We integrate with CISA’s Protective DNS service for an additional threat protection layer the guidance specifically calls out, and


[Netskope Cloud Risk Exchange](https://www.netskope.com/resources/data-sheets/netskope-cloud-risk-exchange) (CRE) and


[Cloud Threat Exchange (CTE)](https://www.netskope.com/products/cloud-exchange) enable bidirectional sharing with partner security tools, so agencies can build an integrated detection picture as well as meet visibility requirements.


Not every SASE platform sees the same things. A network-centric platform can tell you something moved. We can tell you what it was, who moved it, and provide enough context to enforce a policy decision in real time, not waiting until after a human reviews a log.


For more information on how Netskope can help federal agencies visit our webpage:


[https://www.netskope.com/solutions/public-sector/federal-government](https://www.netskope.com/solutions/public-sector/federal-government)

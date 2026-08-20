---
schema_version: "1.0.0"
document_id: "8c6fbce8543b2e15b431fcad17fb0e4bc9cb86f72664646e9d9ce15aef6fa134"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/bringing-crypto-agility-and-pqc-visibility-to-the-network-with-nav/"
published_at: "2026-08-12T19:05:09+00:00"
first_seen_at: "2026-08-12T20:54:57.982037+00:00"
fetched_at: "2026-08-12T20:55:00.670917+00:00"
content_hash: "sha256:904ebc84867e7eb97bd7b1fae56d6ca1b87b9994ab3c74e19d53ac5394e9fbd1"
---

# Bringing Crypto Agility And PQC Visibility To The Network With NAV

The[Q4 2025 Wave™](https://www.forrester.com/report/the-forrester-wave-tm-network-analysis-and-visibility-solutions-q4-2025/RES186659?ref_search=4331141_1786372469835) evaluation for[Network Analysis and Visibility (NAV) solutions](https://www.forrester.com/blogs/announcing-the-forrester-wave-network-analysis-and-visibility-q4-2025/?ref_search=4331141_1786372489580) was the first iteration of the research to evaluate vendors on their post-quantum cryptography (PQC) capabilities, a decision that has become more pronounced in 2026. The rationale was compelling then; it is unavoidable now. Most notably, the[U.S. federal government](https://www.whitehouse.gov/presidential-actions/2026/06/ushering-in-the-next-frontier-of-quantum-innovation/) has moved[PQC migration](https://www.forrester.com/blogs/quantum-negligence-on-the-clock-the-us-just-set-the-egg-timer-on-quantum-migration-as-an-enterprise-risk/?ref_search=4331141_1786372263328) from a long-term discussion to a time-bound activity, effectively putting organizations on the clock.


Although PQC might not be an apt name, it refers to a class of public key algorithms that are considered unbreakable by a sufficiently sized quantum computer. NIST has standardized several PQC algorithms that are rapidly becoming the global benchmark (though a few nations are advancing their own standards or are embracing NIST plus additional options).


The urgency extends well beyond regulatory compliance. The rise of “harvest now, decrypt later” attacks fundamentally changes the risk equation. Additionally, recent quantum computing advances from[Google](https://cloud.google.com/security/resources/post-quantum-cryptography) ,[Cloudflare](https://blog.cloudflare.com/post-quantum-roadmap/) , and[Microsoft](https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/) have pushed some large technology vendors to set their own migration deadlines to 2029. For sectors such as healthcare research and development, where intellectual property, clinical data, and scientific discoveries may retain material value for decades, the threat is particularly acute.


In short, quantum risk is no longer theoretical. It is a present-day security and resilience challenge with long-term consequences. Organizations can no longer postpone quantum readiness as the work towards it should have ideally started yesterday.


Organizations are facing three hard realities:


1. **The assumption.** For decades, organizations operated under the belief that encryption was effectively unbreakable within any practical timeframe. Hence, it became a binary checkbox – at least for majority of organizations. Consequentially, no one ever considered mechanisms for migrating away from these algorithms, thus making them very “brittle”.
2. **The challenge.** The path to post-quantum cryptography is far more complex than enterprises realize. Many PQC implementations rely on TLS 1.3, forcing enterprises to first complete long-overdue modernization efforts of upgrading from TLS 1.2. The ability to rotate certificates seamlessly is also a prerequisite. With such prerequisites themselves being big, hard-to-scale steps (and in some cases not feasible), organizations are not even able to consider PQC much less start their PQC journey.
3. **The solution.** PQC-focused vendor solutions, such as QuSecure, AppViewX, and Keyfactor, help address cryptographic discovery, asset inventories, cryptographic metadata, and (most importantly) crypto-agility.


### **NAV Is Critical To PQC Readiness, But The Market Lags**


Today’s NAV market offers limited visibility into post-quantum cryptography. ExtraHop stands out as one of the few vendors providing meaningful PQC visibility. But the depth of visibility is constrained by the vendor’s support for the quantum resistant algorithms, certificates, and its underlying metadata.


NAV solutions can provide visibility into algorithms such as Kyber and Elliptic Curve Diffie-Hellman Ephemeral (ECDHE), along with select certificate metadata, including key type and size, SNI hostname, and certificate fingerprints or thumbprints. While valuable, this level of visibility is still at its surface level as elements such as root of the issued certificate, who issued it, the certificate hierarchy, and other attributes are left out.


NAV vendors, still playing catch-up on post-quantum cryptography creates a significant visibility gap that enterprises cannot afford to ignore. Until vendors close that gap, organizations will need to compensate the lack of visibility with workarounds. Ingesting cryptographic metadata and related context from dedicated PQC solutions already deployed within the environment into your NAV solution helps bridge this gap to some extent. When combined with deep packet inspection (DPI) and broader network threat detection capabilities that PQC specific solutions lack, this additional cryptographic context becomes more valuable and actionable.


The benefits extend beyond threat detection. Such enhanced visibility within a NAV solution can also strengthen initiatives such as microsegmentation, where policy decisions could often lack cryptographic context. By incorporating PQC-related metadata and visibility from NAV into segmentation strategies, organizations can create trust-based architectures that enforce communication only between systems meeting specific PQC requirements.


Most importantly, given that NAV technologies are out of band and do not facilitate any form of native response, this combination with PQC specific solutions facilitates crypto-agility that extends beyond visibility into action via integrations. NAV solutions can leverage these integrations to deploy reverse proxies and establish tunnels that circumvent the need to upgrade legacy protocols and infrastructure. Other integrations such as within internal HSM (Hardware Security Module) help enterprises to build and maintain their CBOM (Cryptographic Bill of Materials). As organizations prepare for a multi-year PQC migration, the winners will be those that can translate cryptographic intelligence into operational security outcomes.


### **Let’s Connect**


Forrester clients who have questions about this topic or anything related to threat intelligence can[book an inquiry or guidance session with me](https://www.forrester.com/inquiry) .


Share


-
-

---
schema_version: "1.0.0"
document_id: "be0fb6c8b0a502d95de24276ad453bd63f8b4636650c3857932827c7cf84c22d"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-rss-2984dc1ab104"
canonical_url: "https://www.tenable.com/sc-report-templates/post-quantum-ciphers-analysis"
published_at: "2026-01-29T18:02:15+00:00"
first_seen_at: "2026-07-20T23:16:56.508607+00:00"
fetched_at: "2026-07-28T22:22:55.791060+00:00"
content_hash: "sha256:3572d67e9219410c0236b775773cde225bed2f527d0ae06da360da4b4b75088a"
---

# Post Quantum Ciphers Analysis

by[Josef Weiss](https://www.tenable.com/profile/josef-weiss)


Public-key cryptography is the invisible trust fabric which secures web browsing, VPN connectivity, cloud authentication, software updates, and identity verification. For decades, the global economy, national security apparatus, and critical infrastructure have relied on asymmetric cryptography—specifically RSA and Elliptic Curve Cryptography (ECC)—to secure this data. Their security rests on the mathematical difficulty of factoring large integers or solving discrete logarithm problems within any realistic timeframe. Quantum computing changes that assumption. Quantum computers utilize qubits, which combined with quantum entanglement, allows for massive parallelism in calculation. Tenable’s Research team has developed a series of plugins to help identify an organization's progress in mitigating this future threat.


While quantum systems, capable of shattering current encryption standards may be years away, there is a significant threat operational today, through a strategic doctrine of "Harvest Now, Decrypt Later" (HNDL). Adversaries can identify and capture and store encrypted data now, awaiting for future decryption when quantum decryption becomes readily available, and the data can be decrypted retroactively. For security leaders this creates a familiar strategic problem. Public-key cryptography is embedded everywhere: certificate authorities, TLS stacks, VPN gateways, secure email, identity providers, firmware signing, code pipelines, and cloud key management. The implications are catastrophic for current standards:


- **RSA-2048 and RSA-4096:** Completely broken.
- **ECDH and ECDSA (Elliptic Curve):** Completely broken.
- **Diffie-Hellman:** Completely broken.


Tenable has a number of plugins that assist organizations, including:


- 277650 - Remote Services Not Using Post-Quantum Ciphers.
- 277652 - Target Cipher Inventory.
- 277653- Remote Services Using Post-Quantum Ciphers.


Understanding the impact of the transition to using Post Quantum Ciphers is where security teams need actionable insight, understanding where vulnerable cryptographic algorithms are deployed across their infrastructure. To support this visibility, Tenable provides the Post Quantum Ciphers Dashboard. This Tenable Security Center report is designed to help organizations identify systems relying on cryptographic algorithms that will be vulnerable in a post-quantum world. Key features are the identification where RSA and ECC are currently deployed across your infrastructure, supporting prioritization of modernization efforts. Information within the report assists organizations identify remote services using/not using post-quantum ciphers, including the identification of ciphers in Web Application Scanning (WAS) environments, and identifies potentially vulnerable ciphers, certificates and assets.


Organizations need a coherent operational strategy to navigate the migration. Based on[NIST SP 1800-38](https://csrc.nist.gov/pubs/sp/1800/38/iprd-(1)) and[CISA guidance](https://www.cisa.gov/topics/risk-management/quantum) , the following phased approach is recommended.


**Phase 1: Automated Discovery -> Phase 2: Prioritization and Risk Assessment -> Phase 3: Remediation and Crypto-Agility -> Phase 4: Continuous Verification**


This approach begins with establishing the baseline. Tenable Plugin 277652 (Target Cipher Inventory) Extends detection capabilities to cover cryptographic ciphers and algorithms discovered during the scan as a machine parsable JSON file attachment. Tenable Plugins 277653 (Remote Services Using Post-Quantum Ciphers) and 277650 (Remote Services Not Using Post-Quantum Ciphers) help filter the signal from the noise. Identify systems with the highest risk and the most critical data, allowing organizations to move quickly into the remediation phase. Regression is prevented naturally with Tenable’s ability to provide Continuous Verification by incorporating this assessment into regular scanning intervals.


Bottom line, these tactics allow for the surfacing of cryptographic dependencies across the environment, security teams gain the operational intelligence needed to begin structured migration planning today. Organizations that begin assessing exposure now, establish migration roadmaps, and integrate post-quantum readiness into security strategy will move through this transition deliberately and safely.


This Report contains the following Chapters:


- **Executive Summary** : This chapter is designed to help organizations identify systems relying on cryptographic algorithms that will be vulnerable in a post-quantum world. Key features are the identification where RSA and ECC are currently deployed across your infrastructure, supporting prioritization of modernization efforts. Information within the chapter assists organizations identify remote services using/not using post-quantum ciphers, including the identification of ciphers in Web Application Scanning (WAS) environments, and identifies potentially vulnerable ciphers, certificates and assets.
- **Target Cipher Inventory Details:** This chapter provides the details of plugin 277652 which collects the cryptographic ciphers and algorithms discovered during the scan and then present the data in an iterative table for deeper analysis.
- **Remote Services Not Using Post-Quantum Ciphers Details:** This chapter provides the details of plugin 277650 which identifies the network services that do not offer post-quantum ciphers. Tenable makes no attempt to determine whether the remote service would be vulnerable to a post-quantum attack. The chapter presents the collected data in an iterative table for deeper analysis.
- **Remote Services Using Post-Quantum Ciphers Details:** This chapter provides the details of plugin 277652 which collects cryptographic ciphers and algorithms discovered during the scan and then presents the data in an iterative table for deeper analysis.
- **Encryption Ciphers Detected by Nessus Details:** This chapter displays any detected Cipher plugins that were seen by Nessus. The widget utilizes the Plugin Name filter with a match on 'Cipher Suites, SSL Ciphers, Weak Kerberos, and Deprecated Ciphers'. The widgets of the chapter provide the details of cipher related findings such as weak cipher suites such as NULL or RC4, blockchaing issues, recommended changes and other common issues that are impacted by the Post-Quantum Ciphers issue.
- **Certificate Information Detected by Nessus Details:** This chapter displays any detected SSL Certificate information collected by Nessus. The widgets of the chapter provide details for certificate related findings such as Certificate Chain issue, Certificate Expiry, Certificate Key issues other common issues that are impacted by the Post-Quantum Ciphers issue.
- **Encryption Ciphers Detected by WAS Details:** This chapter displays the most prominent SSL/TLS plugins which were detected by WAS. The widgets of the chapter provides the information by using the plugin name filter to look at plugin names containing 'SSL/TLS' in the name. The widgets provides the details for SSL/TLS issues such as weak or insure cipher suites, expired certificates and other common issues that are impacted by the Post-Quantum Ciphers issue.
- **SSH Algorithms Detected by Nessus Details:** This chapter displays any detected SSH algorithm information. The widgets of the chapter provides the details for SSH issues such as weak or insure algorithms suites other common issues that are impacted by the Post-Quantum Ciphers issue.

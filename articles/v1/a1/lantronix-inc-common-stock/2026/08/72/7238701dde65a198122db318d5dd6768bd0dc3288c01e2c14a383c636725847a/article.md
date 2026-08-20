---
schema_version: "1.0.0"
document_id: "7238701dde65a198122db318d5dd6768bd0dc3288c01e2c14a383c636725847a"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/lmos-6-9-now-with-better-access-stronger-security-and-faster-everything-for-oob/"
published_at: "2026-08-18T19:11:01+00:00"
first_seen_at: "2026-08-18T21:22:19.570069+00:00"
fetched_at: "2026-08-18T21:22:23.833386+00:00"
content_hash: "sha256:470924b793ab5dabc76f6fb73813107ed3dc7c0ccb8354ae32e8d8e85567ea43"
---

# LMOS 6.9, now with better access, stronger security, and faster everything for OOB

August 18, 2026


- [General](https://www.lantronix.com/blog/category/general/)


### LMOS 6.9, now with better access, stronger security, and faster everything for OOB


LMOS 6.9 released on July 31st bringing significant operational improvements for the LM-Series console servers and Lantronix Control Center. Key enhancements include automated device discovery, configurable authentication controls, and strengthened security features that reduce manual configuration overhead and accelerate deployment timelines. Here are some highlights:


**Auto-Discovery of Managed Devices**
Both Local Manager (LM) and Control Center (CC) now automatically discover devices connected to managed ports. When you connect a device, the system recognizes it immediately and eliminates manual port assignments for each piece of equipment. At scale, this capability reduces deployment time from weeks to an afternoon.


**TACACS+ Enhancements**
Three improvements enhance TACACS+ functionality. Authentication timeout is now configurable, addressing scenarios where remote TACACS+ servers introduce latency. Group membership options are configurable, allowing you to map existing AAA groups to LM roles without restructuring your directory. Additionally, TACACS+ requests now include the client IP, enabling your security team to trace authentication events to their source.


**Control Center Authentication Improvements**
When using CC as the authentication type, requests include both the CC IP and Local Manager IP, providing your SIEM with complete visibility across authentication events.


**Break Glass Authentication**
Break Glass access provides emergency console access when standard authentication is unavailable. Version 6.9 now supports authentication for Break Glass users in Control Center, enabling controlled emergency access with full audit logging.


**User Lockout Policy**
User lockout after failed authentication attempts, previously available in Local Manager, is now consistent across Control Center as well which ensures uniform security policy across your infrastructure.


**Port Display Enhancement**
The port display now shows both hostname and description simultaneously, reducing configuration errors. When managing 80+ ports, this visibility significantly decreases wrong-port connections and associated operational issues.


**Terminal Application SSH Preference**
Terminal Application now prioritizes SSH over RSSH when direct SSH access to Local Manager is available, with RSSH serving as a fallback. This improves both latency and security for environments requiring direct connection paths.


**Operating System Updates**
Both LM and CC now run updated Linux kernels with current security patches, plus 50+ RPM updates. These changes are transparent in the UI but critical for security audits and compliance.


**Upgrade Compatibility**
Upgrades are supported from versions 6.6.x, 6.7.x, and 6.8.x across LM4, LM80/LM83X, Virtual LM, and Control Center (Build 45480).[Contact your Lantronix account team](https://www.lantronix.com/technical-support/) for questions about your specific upgrade path or deployment considerations.

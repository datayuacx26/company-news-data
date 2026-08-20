---
schema_version: "1.0.0"
document_id: "d2e6d0b0c611603284749166421feb4b3f36b2f9a6301edaea78f566fe3a002f"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/large-scale-credential-attacks/"
published_at: "2026-08-18T19:05:33+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:31776fa110e279e8365640ae15284944e998aff762016bc32c5dcf97398fbedc"
---

# Threat Brief: Mitigating Large-Scale Credential Attacks (Updated August 18)

Unit 42 is aware of a large-scale password spraying and credential theft campaign[(“FortiBleed”)](https://socradar.io/blog/fortibleed-fortinet-firewalls-compromised/) against Fortinet devices. We observed attempts targeting MSSQL devices as well, and have seen reports of Sophos devices also being targeted. While this activity is not targeting Palo Alto Networks devices, Unit 42 has observed suspicious login attempts in customer telemetry and we are providing this report out of an abundance of caution to ensure our customers have the latest intelligence and product recommendations to protect, detect and respond to attacks to their network.


The threat actors are using a curated password list to attempt password spraying against services exposed to the internet. Unit 42 assesses that the initial password list for this activity was likely developed through a mix of previous breaches, including the successful exploitation of vulnerabilities. Once they obtain credentials, they add them to their password list for future attempts against additional targets, as well as for logging into accounts they successfully compromised.


The threat actors are leveraging a multi-stage process to gain persistent, high-privilege access:


- **Password spraying for initial access:** Massive internet-wide scanning and password spraying attempts against Fortinet, Sophos and MSSQL services
- **Configuration Extraction:** Depending on the permissions of their initial access, the actor may exploit a privilege escalation vulnerability prior to pulling device configuration files, including stored credentials
- **Offline Cracking:** Offline password cracking of the stolen credentials adds to the password list used in step one to target new devices, as well as to log into compromised devices to establish persistence as an administrator


Unit 42 observed an initial access broker (IAB) on the Russian-language cybercrime forum Exploit\[.\]in claiming responsibility for this campaign, referencing a CVE (no further information), and offering the harvested credentials for sale on June 16, 2026. Unit 42 has not validated their claims at this time.


Figure 1. Darkweb post of IAB selling credentials.Unit 42 recommends auditing remote access logs for suspicious activity with a focus on successful logins shortly after large volume password failure events. We also recommend reviewing and implementing the hardening guidance below for edge devices.


[SOCRadar](https://socradar.io/blog/fortibleed-fortinet-firewalls-compromised/) provided the initial reporting on the targeting of FortiGate devices. We observed attempts targeting MSSQL devices as well, and have seen reports of Sophos devices also being targeted.


Palo Alto Networks customers receive assistance protecting against and mitigating credential attacks in the following ways:


- PAN-OS uses a[Master Key](https://docs.paloaltonetworks.com/ngfw/administration/certificate-management/master-key-encryption) to encrypt cryptographic keys in either ES-256-CBC or AES-256-GCM encryption algorithm
- PAN-OS only stores[SHA-256 encrypted and salted hashes](https://docs.paloaltonetworks.com/ngfw/administration/authentication/authentication-types/multi-factor-authentication)
- Customers can[integrate several MFA platforms](https://docs.paloaltonetworks.com/ngfw/administration/authentication/authentication-types/multi-factor-authentication) to enhance their security posture
- Customers can[customize Password Profiles](https://docs.paloaltonetworks.com/panorama/11-1/panorama-admin/administer-panorama/configure-panorama-password-profiles-and-complexity) and complexity
- Customers can follow our[Administrative Access Best Practices](https://docs.paloaltonetworks.com/best-practices/10-1/administrative-access-best-practices)


Palo Alto Networks also recommends the following hardening guidelines:


- **Require MFA:** Require strong phishing-resistant multi-factor authentication for all remote services. NGFW customers can[integrate several MFA platforms](https://docs.paloaltonetworks.com/ngfw/administration/authentication/authentication-types/multi-factor-authentication) (including Palo Alto Networks Idira MFA) and customize their[Password Profiles](https://docs.paloaltonetworks.com/panorama/11-1/panorama-admin/administer-panorama/configure-panorama-password-profiles-and-complexity) and complexity to enhance their security posture.
- **Adopt Zero Trust Architecture:** Leverage “jump boxes” and[Zero Trust Network Access (ZTNA)](https://www.paloaltonetworks.com/cyberpedia/what-is-zero-trust-network-access-ztna) policies to ensure management interfaces are never exposed directly to the public internet, further narrowing the attack surface for configuration extraction.
- **Change Default Credentials:** Change the credentials for default accounts, ensuring long, complex passwords are used to mitigate the risk of password guessing attempts. Ideally onboard accounts to[Privileged Access Management (PAM)](https://www.paloaltonetworks.com/idira/human/privileged-access-management) system and rotate passwords automatically on-time and on-use.
- **Implement ITDR:** Timely detect malicious access attempts and accelerate response with automated identity-centric actions.
- **Disable Unused Accounts:** Run continuous discovery of privileged accounts. Onboard and disable unused accounts to limit the attack surface.
- **Update and Patch:** Ensure you have the latest software versions and patches installed to mitigate known vulnerabilities, including local privilege escalation vulnerabilities.


The[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) can also be engaged to help with a compromise or to provide a proactive assessment to lower your risk.


## Conclusion


Unit 42 will continue to monitor the situation for updated information. We encourage customers to implement the hunting and hardening recommendations to identify, mitigate, and prevent credential attacks against their networks.


Palo Alto Networks has shared our findings with our fellow Cyber Threat Alliance (CTA) members, including Fortinet. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


Palo Alto Networks customers are better protected by our products, as listed below. We will update this threat brief as more relevant information becomes available.


## Palo Alto Networks Product Protections and Consulting Services


Palo Alto Networks customers can leverage a variety of product protections and consulting services to identify and defend against this threat.


If you think you might have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


### Deep and Darkweb Monitoring


Unit 42's[Deep and Dark Web (DDW)](https://www.paloaltonetworks.com/resources/datasheets/unit-42-deep-and-dark-web-service) monitoring is a service that assists clients in identifying sensitive information and leaked credentials that surface on the dark web, providing critical insights to reduce risk exposure and reduce the time between detection and response.


### Cortex Cloud


Cortex Cloud[Identity Security](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/What-is-Cortex-Cloud-Identity-Security) encompasses Cloud Infrastructure Entitlement Management (CIEM), Identity Security Posture Management (ISPM), Data Access Governance (DAG) as well as Identity Threat Detection and Response (ITDR) and provides clients with the necessary capabilities to improve their identity related security requirements. By providing visibility into cloud based identities, and their permissions, Cortex Cloud can detect misconfigurations, unwanted access to sensitive data and real-time analysis surrounding usage and access patterns. Additionally, Cortex Cloud provides protections against credentials that were compromised, lost or exposed being leveraged against cloud resources, such as those discussed within this article.


### Idira Identity Threat Protection


[Idira Identity Threat Protection](https://www.paloaltonetworks.com/resources/techbriefs/threat-detection-and-response) (ITP) enables security teams to counter identity-based attacks targeting Idira Next Generation Identity (NGI) Platform and the identities it secures. Using near real-time detection, powered by CORA AI, and leveraging Idira’s visibility across multiple contexts (like PAM, authentication, SSO, cloud, endpoints, browsers, and more), Idira ITP can apply automated, tailored non-disruptive in-session response to contain and minimize potential identity-based threats.


### Idira MFA


[Idira MFA](https://www.paloaltonetworks.com/idira/human/identity-and-access-management) helps protect organizations against password spraying, credential theft, and other identity-based attacks by verifying that the person signing in is the legitimate user, not just someone with a valid password. Using phishing-resistant MFA, including passkeys, biometrics, and FIDO2 security keys, along with adaptive, risk-based authentication, Idira evaluates signals such as device trust, location, and login behavior. When risk is detected, it requires additional verification before granting access, helping prevent account compromise while keeping access simple for trusted users.


### Idira Privileged Access Management


[Idira Privileged Access Management](https://www.paloaltonetworks.com/idira/human/privileged-access-management) is a SaaS-delivered Privileged Access Management (PAM) solution that mitigates credential compromise and password spraying by prioritizing automated discovery, onboarding, and rotation. The platform continuously scans hybrid environments and infrastructure to detect unmanaged local, domain, service accounts and cloud roles. Discovered credentials are automatically onboarded into a hardened digital vault for centralized management. Privilege Cloud then enforces programmatic transactional credential rotation using complex, randomized strings. This eliminates the static, predictable passwords exploited during spraying attacks, removing standing privileges and blocking lateral movement across the network infrastructure and devices.


## References


- [Analysis of Reported Credential Compromise of Fortigate Devices](https://www.fortinet.com/blog/psirt-blogs/analysis-of-reported-credential-compromise-of-fortigate-devices) — Fortinet
- [FortiBleed Breach: How 80,000+ Corporate Firewalls Were Quietly Compromised](https://socradar.io/blog/fortibleed-fortinet-firewalls-compromised/) — SOCRadar
- [What is Zero Trust Network Access (ZTNA)?](https://www.paloaltonetworks.com/cyberpedia/what-is-zero-trust-network-access-ztna) — Palo Alto Networks
- [Next-Generation Firewall: Multi-Factor Authentication](https://docs.paloaltonetworks.com/ngfw/administration/authentication/authentication-types/multi-factor-authentication) — Palo Alto Networks, Tech Docs
- [Administrative Access Best Practices](https://docs.paloaltonetworks.com/best-practices/10-1/administrative-access-best-practices) - Palo Alto Networks, Tech Docs
- [Panorama Administrator's Guide: Configure Panorama Password Profiles](https://docs.paloaltonetworks.com/panorama/11-1/panorama-admin/administer-panorama/configure-panorama-password-profiles-and-complexity) — Palo Alto Networks, Tech Docs


*Updated June 26, 2026 at 1:00 p.m. PT to add product protection for Idira Security and Cortex Cloud, and more information to the hardening guidelines section.*

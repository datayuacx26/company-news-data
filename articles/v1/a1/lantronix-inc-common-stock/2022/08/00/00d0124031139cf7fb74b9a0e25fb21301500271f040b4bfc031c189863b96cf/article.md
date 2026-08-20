---
schema_version: "1.0.0"
document_id: "00d0124031139cf7fb74b9a0e25fb21301500271f040b4bfc031c189863b96cf"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/7-iot-security-practices-for-iot-projects/"
published_at: "2022-08-27T23:17:06+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:1d67b9aa8b581d0f62c5a0dba5963585177f1dab145d9eb9f63c9bfabbfade67"
---

# 7 IoT Security Practices for IoT Projects!

August 27, 2022


- [General](https://www.lantronix.com/blog/category/general/)
- [Industry in-the-know Information](https://www.lantronix.com/blog/category/industry-in-the-know-information/)


### 7 IoT Security Practices for IoT Projects!


#### 7 IoT Security Practices you Must Consider for your IoT Projects – How Do Your Projects Stack UP?


IoT Security is always a concern when developing your IoT projects and[Network World’s](https://www.networkworld.com/) Bob Violino has developed 7 security practices you may not have considered, in his article, “[7 Steps to Enhance IoT Security”](https://www.networkworld.com/article/3404198/7-things-you-can-do-to-enhance-iot-security.html) . These practices range from big moves to small adjustments to ensure network, systems, data, and devices are protected. As we read his article, a thought came to mind – How do Lantronix embedded IoT products “stack up”?


But before we look at what makes up these 7 practices for IoT security, let’s look at how the IoT device manufacturers are being challenged by legislation in Californian ([senate bill # 327](https://leginfo.legislature.ca.gov/faces/billTextClient.xhtml?bill_id=201720180SB327) ) and the United Kingdom as an example. In California, a bill went into effect January 1, 2020, that requires manufacturers of internet-connected devices to increase their security capabilities to better protect consumer data transition through their devices. This has had wide-sweeping effects on the sales of Internet of Things (IoT) devices impacting many everyday products.


The California IoT legislation has seriously impact manufacturers of IoT devices not just in California but across the United States. The first step in analyzing whether the law is applicable to your company is to determine whether your company produces “connected devices.” If your company does manufacture such devices that connect directly or indirectly to the Internet, and those devices are sold or offered for sale in California, then this IoT legislation is applicable.


Following in California’s footsteps, the United Kingdom has instituted similar IoT security legislation that has become law. The UK legislation mandates:


- IoT device passwords are unique and not re settable to any universal factory default (as is the case in the California law);
- Manufacturers of Internet-connected devices provide a public point of contact as part of a vulnerability disclosure policy to enable issues to be reported to the manufacturer; and
- Internet-connected devices are capable of being securely updated, and manufacturers must explicitly state the minimum length of time for which a device will receive software updates.


These two examples clearly show that manufacturers need to take additional measures to ensure the security of their products and the components that make up the intricate platforms of IoT devices. So, let’s look at the 7 practices you can implement today to provide better IoT security for your products according to Network World:


1. **IoT security – start by thinking small** – Laura DiDio principal at research and consulting firm ITIC states, “The majority of IoT devices are very small. Therefore, the source code tends to be written in the ‘common tongue’—C or C++ and C# languages, which frequently fall victim to common problems like memory leaks and buffer-overflow vulnerabilities. These issues are the network equivalent of the common cold.”
2. **Deploy context-aware access controls –** Controlling access within an IoT environment is one of the bigger security challenges companies face when connecting assets, products, and devices. That includes controlling network access for the connected objects themselves.
3. **Hold vendors accountable for their IoT equipment –** Companies should apply the controls outlined in common security frameworks to IoT devices. For example, include security functional requirements in your contracts; request recent vulnerability scans or assert the right to scan them yourself; obligate the vendors to provide timely updates to address identified weaknesses; and rescan the devices after any firmware updates to ensure that identified issues have been resolved and that no new issues have been introduced.
4. **Defend against IoT identity spoofing** – Hackers and their techniques have become more proficient over the years, and this can represent a significant threat for IoT security. That makes it imperative that businesses and their security and IT departments verify the identity of the IoT devices that they’re communicating with, and ensure that they are legitimate for critical communications, software updates, and downloads.
5. **Establish one-way connections for IoT devices –** Companies should limit the ability of IoT devices to initiate network connections, and instead only connect to them using network firewalls and access control lists. Enterprises can also force connections to IoT devices to go through jump hosts and/or network proxies.
6. **Consider using a segregated network –** Many types of control devices, such as thermostats and lighting controls, connect via wireless. Most enterprise wireless networks require WPA2-Enterprise/802.1x.
7. **Insert security into the supply chain** – IoT endeavors typically reach across multiple partners in a supply chain, including technology vendors, suppliers, and customers, and security must take that into account.


These are some great tips for improving IoT Security but how does this translate into real-world scenarios and what should developers and designers such as project engineers, systems integrators and OEM manufacturers be looking for in secure IoT modules and gateways. As an example, let take the Lantronix offering for embedded products (the xPico and XPort family) and breakdown the IoT security components that encompass **InfiniShield™ Security** – Lantronix’s built-in security software.


InfiniShield provides:


- **Secure Boot –** this feature ensures the only authorized firmware, approved by the OEM, can be loaded so even if someone gets physical access to the device, your trusted firmware cannot be replaced.
- **Controlled Access –** role-based access control allows for multiple users, each with a role that gives different access rights. This provides access for your team but differentiates the level of admin privileges.
- **Encrypted Storage –** of credentials and configurations **. C** ertificate storage establishes a root of trust to be able to identify servers and the devices itself with client certificates for specific enterprise security standards.
- **Secure Communications –** support for security protocols and ciphers with embedded gateway applications. (TLS1.2, AES 256-bit, SHA-2)
- **Secure Firmware Updates –** reliable updates of authorized firmware done remotely. Plus, ongoing support to keep your device secure and monitor it for critical vulnerabilities.


These are just a few of the security features of the new **Infinishield** ™ built-in software, but this gives you a roadmap of what to look for when designing in module gateways or any embedded product to ensure end to end security. Our final recommendation is to find and work with partners who (as Tip 7 explains) insert security into your supply chain whether they are technology vendors, suppliers, system integrators, or resellers.


To learn more about Lantronix, the Infinishield™ built-in security software and our award-winning line of embedded products go to www.Lantronix.com or https://www.lantronix.com/support/infinishield-security/ or check out our whitepaper for a deeper drive into Security in the IoT Age at https://www.lantronix.com/blog/security-in-the-age-of-iot/ .

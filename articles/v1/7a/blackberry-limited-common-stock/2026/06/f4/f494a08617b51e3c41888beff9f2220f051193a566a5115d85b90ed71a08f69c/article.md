---
schema_version: "1.0.0"
document_id: "f494a08617b51e3c41888beff9f2220f051193a566a5115d85b90ed71a08f69c"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/why-global-telecom-interconnect-systems-are-not-secure"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-23T03:42:51.442589+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:424e1939d304a364e72040f6167a65ab3ab3448f56ef2baa2223d20d6e4cbc09"
---

# Why Global Telecom Interconnect Systems Are Not Secure

# Why Global Telecom Interconnect Systems Are Not Secure


Global telecom interconnect systems are structurally vulnerable to surveillance.


Jun 2, 2026


·


Blog


·


​​​​​Jonathan Jackson


Global telecom interconnect systems — the networks that enable telecom providers worldwide to exchange voice, messaging, and data — are widely trusted by governments and mission-critical organizations to transmit sensitive information. However, recent intelligence confirms that this ecosystem is fundamentally insecure.


Recently, researchers at the University of Toronto published a


[report documenting active surveillance campaigns that exploit the fundamental weaknesses of global mobile networks](https://citizenlab.ca/research/uncovering-global-telecom-exploitation-by-covert-surveillance-actors/)


including a ‘VVIP’ executive targeted with more than 500 location tracking attempts across 11 operator identities in 9 countries in a single four-hour operation. These campaigns are targeting high-value individuals and extracting critical intelligence without detection. Infrastructure from the UK, Israel, Channel Islands, Sweden, Italy, Cambodia, Mozambique and more was used to route surveillance traffic, making attribution nearly impossible.


These findings confirm that the telecommunications infrastructure relied upon by decision-makers is being weaponized against them. High-profile attacks like the[Salt Typhoon](https://www.blackberry.com/en/secure-communications/insights/glossary/salt-typhoon) campaign and[UNC3886 in Singapore in 2025](https://www.imda.gov.sg/resources/press-releases-factsheets-and-speeches/press-releases/2026/largest-cyber-operation-mounted-to-counter-unc3886-threat) underscore the considerable risks.


Critically, these vulnerabilities are not simple software bugs that engineers can patch. They are flaws built into the trust model that underpins international roaming. As long as communications rely on carrier infrastructure, they remain exposed to any actor with access to that layer.


## **The Core Flaws in Global Mobile Networks**


Global mobile networks prioritize efficiency and inter-operator trust over security. Protocols like SS7 and Diameter lack authentication, integrity checks, and encryption. These outdated designs allow malicious actors with access to the signaling layer to query data, track locations, and exploit metadata.


### **Protocols Designed for Trust, Not Security**


Telecom networks were built assuming only trusted operators would have access. Today, thousands of entities can access the infrastructure, making networks inherently vulnerable.


### **Misplaced Trust in Carrier-Level Protection**


Organizations assume telecom providers ensure secure boundaries, but attackers routinely bypass these defenses. They impersonate operators and blend malicious actions with legitimate traffic, evading detection.


## **Exploiting Telecommunications Infrastructure**


Threat actors exploit telecom networks for


[location tracking](https://citizenlab.ca/2026/03/metadata-surveillance)


, metadata extraction, and delivering malicious payloads. Advanced techniques like zero-click exploits use hidden messages to silently compromise devices, bypassing conventional security solutions. Additionally, attackers hide behind spoofed operator identities, routing traffic through multiple countries to evade detection.


## **Why Carrier Security Measures Are Insufficient**


Telecom providers have implemented firewalls and filtering, but these cannot resolve structural weaknesses. Networks rely on self-reported routing data, making it impossible to authenticate message origins. Vulnerabilities extend across all network generations, from 2G to 5G, and


[even satellite networks](https://www.csis.org/analysis/satellite-encryption-gap-why-everyday-citizens-communications-are-risk)


.


**Common Threat Exploits and Responses**


**Threat**


**BlackBerry Counter-Capability**


SS7/Diameter location queries extract timing, cell ID, subscriber state and routing metadata


**Metadata Shielding** — conceals identifiers, routing data, timing and location from the carrier layer, removing intelligence value from intercepted signaling traffic


Attackers obtain access via commercial leasing of legitimate operator infrastructure


**Sovereign Control** — customers own encryption keys, infrastructure and policies; deployable on-premises, air-gapped or in sovereign cloud, removing dependency on carrier trust


Operator identities are spoofed; carrier systems cannot verify true message origin


**Identity Assurance** — cryptographic enrollment, not phone-number based; only verified users on approved devices can communicate, independent of carrier authentication


SIMjacker exploit silently compromises the device SIM with zero user interaction


**Device Resilience** — secure containers, at-rest encryption and policy enforcement protect the communications environment; access revoked instantly if device is compromised


Multi-year persistent campaigns adapt across 5G, 4G, 3G and satellite networks


**Network Continuity** — trusted communications maintained across 5G, Wi-Fi, satellite and sovereign networks regardless of whether public carrier infrastructure is degraded or compromised


## **Protecting Government and Critical Sectors**


The sectors facing the highest exposure are those that rely most heavily on communications for operational continuity:


**Governments and defense** face adversaries with both the motive and the access to exploit the signaling layer for intelligence gathering. Continuity during crises depends on communications that cannot be intercepted or disrupted at the carrier level.


**Law enforcement and emergency responders** coordinate in real time during high-pressure incidents. A compromised communications layer doesn't just create an intelligence risk — it creates an operational one.


**Critical infrastructure operators** — energy providers, transportation networks, utilities — face regulatory requirements around communications security and cannot afford the exposure that carrier-dependent platforms carry.


## The Path Forward


BlackBerry Secure Communications is built around the understanding that carrier infrastructure cannot be trusted as a security boundary. Its solutions are validated by NATO Restricted, FIPS 140-2, Common Criteria, FedRAMP Class D (High), and NIAP, certifications that reflect real-world deployment in the environments where the consequences of failure are highest.


For organizations operating at that level of sensitivity, the question is no longer whether the telecom layer is compromised. It's whether your communications platform was designed with that assumption from the start.


Get updates about the latest in-depth knowledge for secure communications.

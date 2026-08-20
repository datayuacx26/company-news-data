---
schema_version: "1.0.0"
document_id: "25200c4932f5df531e0ffb90e4fd39a2c3b84f7da1e42918207d0355cce6b955"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/botnet-pulse"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:39a0253a581703355ef9b774469c3499a519fbebb7c520cbd00b389ad62b4f63"
---

# Botnet Pulse

## Executive Summary


July’s botnet-driven distributed denial-of-service (DDoS) activity remained elevated, with pressure spikes around the U.S. holiday period and continued automation from commodity botnets.


NETSCOUT observed more than **20,000** total DDoS attack events (more than **600/day** ), with a sharp **July 3** surge to more than **1,100** . Hacktivist activity was persistent, with more than 700 total attacks, driven largely by[NoName057(16)](https://www.netscout.com/blog/asert/noname057-16) **(200+ DDoS attack events)** . On the ground, defenders saw familiar mechanics: **TCP SYN** dominance, frequent **multivector** blends, and sustained hits on **TCP 80/443** and **UDP 53** , with noticeable chatter on **UDP 80/110/443** .


### Key Findings


- **Attack frequency:** July 2025 saw more than 20,000 botnet-driven DDoS attack events, more than 600 DDoS attack events per day, with a significant spike on July 3 exceeding 1,100 attacks, approximately 71 percent above the monthly average.
- **Dominant threat actor:** NoName057(16) claimed more than 200 attacks, accounting for a significant portion of the 700+ total claimed attacks. The group’s operations, including HTTP/2 POST floods, TCP ACK, and TCP SYN floods, showed strong alignment with observed attack patterns, targeting government, transportation, and financial services.
- **Port targeting patterns:** TCP ports 80 and 443 were the most common combination, appearing in more than 900 unique attacks, reflecting sustained pressure on web-facing infrastructure. UDP ports 443, 80, and 53 were frequently targeted, with notable combinations such as 500 and 4500 (VPN services), indicating DNS and VPN service abuse.
- **Top attack vectors:** TCP[SYN floods](https://www.netscout.com/what-is-ddos/syn-flood-attacks) led with approximately 3,000 attacks, followed by multivector combinations such as TCP SYN + DNS flooding, TCP ACK + TCP SYN, and complex blends involving NTP and DNS amplification, designed to exhaust both device state and bandwidth.
- **Geographic source patterns:** Mongolia was the top single-country source with more than 1,000 attacks, primarily from Internet of Things (IoT) and router infections. The most frequent multicountry combination was Mauritius and South Africa, involved in more than 100 attacks, highlighting the global distribution of botnet infrastructure.
- **Vulnerability exploitation:** Attackers leveraged well-known vulnerabilities, including CVE-2015-2051, CVE-2017-17215, and others, to compromise IoT devices, routers, and web servers, with Mirai variants driving significant botnet recruitment via Telnet brute-forcing and default credential exploits.


### July 2025 at a Glance


- **Daily average:** More than **600** DDoS attack events/day
- **Peak:** More than **1,100** DDoS attack events (July **3** )
- **Avg. unique targets/day: ~57**
- **Avg. duration:16 minutes, 53 seconds**
- **Avg. bandwidth: 3.84Gbps**
- **Avg. packet rate: 924kpps**
- **Avg. packet size: 808 bytes**
- **Top single source country: Mongolia**


## Detailed Analysis


### Attack Frequency and Trends


July’s botnet-driven DDoS activity targeting the service provider space aligns with previous trends in which backbone infrastructure and transit networks face persistent pressure. These events are captured via provider-side telemetry, which serves as the basis for this analysis.


July held a ~600/day DDoS attack event cadence, punctuated by **1,105 events on July 3** , a sharp but not anomalous intramonth peak. The timing aligned with the **U.S. holiday window, new botnet rollouts** (e.g., a Go-based “[hpingbot](https://fastnetmon.com/2025/07/07/new-botnet-hpingbot-emerges-with-pastebin-based-payload-delivery-and-hping3-ddos-engine/) ” leveraging hping3 for TCP-/UDP flood crafting), and persistent[hacktivist campaigns](https://cyberpress.org/hacktivist-group-targets-over-20-critical-sectors/) .


### Threat Actor Activity


**Despite**[Operation Eastwood](https://www.europol.europa.eu/media-press/newsroom/news/global-operation-targets-noname05716-pro-russian-cybercrime-network) **, NoName057(16)** remained the most active and visible threat actor in July. Of the 700+ attacks claimed by all groups, NoName057(16) alone accounted for **more than 200** . More importantly, there was a strong overlap between that group’s claims and observable attack traffic. Although some groups tend to exaggerate their activity, claiming credit when websites go offline for unrelated reasons, NoName057(16)’s announcements often align with observable attack activity. Campaigns are typically multivector, rotate through techniques quickly, sometimes without an apparent technical rationale, and may use several vectors simultaneously, including the following:


- HTTP/2 POST floods
- TCP ACK and TCP SYN floods
- Sustained multiminute engagements


NoName057(16)’s targets included **government websites, transportation and logistics** , and **financial services** .


Although the group’s bot activity is globally distributed, a significant portion of observed attack traffic either **originates from** or is **transmitted through** a small number of content delivery networks (CDNs) and cloud hosting providers. These networks are used to host bots directly or serve as relays via proxies and virtual machines. This approach provides attackers with reliable bandwidth and complicates mitigation, because malicious traffic often blends with legitimate flows from trusted infrastructure.


Other groups such as Keymous+, TEAM FEARLESS, Dark Storm Team, and Z-ALLIANCE were also active, although their operational footprint was far smaller. Although these actors occasionally make headlines or claim credit for outages, their observed attack volume and consistency remained limited throughout July.


### Port and Protocol Targeting


Botnets in July targeted familiar ground, but often in more strategic combinations. The most common port pairs were:


- **TCP** : Among all observed botnet-driven DDoS attack events, port **443** was the most frequently targeted individually. The most common port combination was **80 and 443** , appearing in more than 900 unique attacks. This continues the trend of sustained pressure on public-facing web infrastructure.
- **UDP** : 443, 80, and 53 were frequently targeted individually, indicating ongoing abuse of encrypted traffic and DNS services. The first notable combination was **500 and 4500** , typically used for VPN services.


VPN infrastructure saw continued probing in July. Although VPN-related ports didn’t top the charts, their appearance in targeted combos shows attackers are still actively interested in disrupting remote-access services.


### Attack Vectors and Methodologies


Single-vector attacks still dominated in raw volume, with **TCP SYN floods** at the top of the list ( **~3,000 attacks** ). But attackers increasingly leaned into **multivector combinations** , including:


- TCP SYN + DNS query flooding
- TCP ACK + TCP SYN
- NTP amplification + TCP ACK/RST/SYN + TCP SYN/ACK amplification
- DNS amplification + TCP ACK/RST/SYN + TCP SYN/ACK amplification
- DNS amplification + NTP amplification + TCP ACK/RST/SYN + TCP SYN/ACK amplification


Attackers blended state-exhaustion with reflection/amplification, pushing both device state and upstream bandwidth to the limit.


### Geographic Source Patterns


Source analysis showed that although many attacks used globally distributed infrastructure, single-country events were also very common. Mongolia continues to lead with more than 1,000 attacks, primarily traced to localized IoT and router infections. These source IPs were confirmed to be legitimate based on their consistent activity over multiple days, combined with supporting evidence from passive monitoring tools and honeypots, including observed connection attempts. Taken together, this points to real, reachable devices being used in the attacks, rather than fake or short-lived sources.


For July, the most frequent multicountry combination was the Republic of Mauritius and South Africa, involved together in more than 100 attacks.


### Vulnerabilities and Bot Infrastructure


DDoS-capable botnets continue to exploit known vulnerabilities in IoT devices, routers, and web servers to expand their reach. In July 2025, attackers leveraged a combination of well-known exploits and brute-force tactics, compromising thousands of unique IP addresses to build botnets. Key vulnerabilities included:


- **CVE-2015-2051:** Exploited by Mirai variants (FICORA, CAPSAICIN) to target D-Link routers, enabling remote command execution for DDoS botnet recruitment
- **CVE-2017-17215:** Used by Mirai to compromise Huawei HG532 routers, often through Telnet brute-forcing campaigns
- **CVE-2017-16894, CVE-2019-17050, CVE-2021-41714:** Frequently targeted service-provider infrastructure to expand botnet networks


These vulnerabilities, although years old, remain effective due to unpatched devices, weak default credentials, and inadequate security practices. Notably, July 2025 saw increased scanning and exploitation of VStarcam C7824WIP cameras (via unauthenticated remote access) and Actiontec C1000A routers (via Telnet backdoors).


Attackers systematically scanned and compromised devices at scale, with many bots participating in attacks over multiple days. To mitigate these threats, organizations should apply firmware patches, disable Telnet, change default passwords, and monitor for suspicious traffic on ports such as 23 and 2323.


## **Recommendations**


Service providers are still squarely in the crosshairs, and July made that even more obvious. To stay ahead of these botnet-driven DDoS threats, we recommend the following:


- **Real-time visibility** into botnet behavior and attack patterns. Tools such as[NETSCOUT Arbor Sightline](https://www.netscout.com/product/arbor-sightline) can help surface early signs of trouble.
- **Proactive mitigation** with automated systems such as[NETSCOUT Arbor Threat Mitigation System (TMS)](https://www.netscout.com/product/arbor-threat-mitigation-system) or[Arbor Edge Defense (AED)](https://www.netscout.com/product/arbor-edge-defense) . These can stop both volumetric floods and more-complex, multivector attacks.
- **Intelligence-driven defense** . Feeds such as[NETSCOUT’s ATLAS Intelligence Feed (AIF)](https://www.netscout.com/product/atlas-intelligence-feed) provide information about context, what’s trending, who’s being targeted, and how actors are evolving.


It’s not just about stopping traffic; it’s about understanding where that traffic is coming from, why it’s happening, and what it could become. July’s activity shows that DDoS attack events are still growing in sophistication and intent.

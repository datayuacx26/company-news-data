---
schema_version: "1.0.0"
document_id: "024237939d6e5ed5907cc60eab315260c05e4e37464b1cff177d8ccbaffdf35a"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/who-turns-stone-now"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:8874ecddf2bd2a76758c5e1e02986510b416524a274353ea29e05ac21f5a237c"
---

# Who Turns to Stone Now?

## Executive Summary


**When does sharing threat intelligence actually stop attacks?** The MegaMedusa case provides a rare, measurable answer. In March 2025, when open threat intelligence identifying proxy infrastructure went public, something remarkable happened: RipperSec’s impactful[distributed denial-of-service (DDoS) attacks](https://www.netscout.com/what-is-ddos) dropped significantly, even as the pro-Palestinian group continued claiming successful operations. This near-instant decline demonstrates that timely, shared threat intelligence doesn’t just help defenders respond faster, it can neutralize entire attack campaigns.


RipperSec’s MegaMedusa toolkit generates HTTP(s)[application layer attacks](https://www.netscout.com/what-is-ddos/application-layer-attacks) by relaying requests through open proxy infrastructure. By sharing and monitoring these proxies, the security community essentially pulled the plug on the threat actor’s primary weapon. However, blocking 1.1 million candidate proxies would cause massive collateral damage. ASERT’s analysis refined this list to approximately 100,000 confirmed attack sources, significantly reducing the risk of overblocking legitimate users while maintaining robust protection.


This case study proves what defenders have long suspected: When the industry shares intelligence quickly and openly, even sophisticated adversaries lose their edge.


### Key Takeaways


- **MegaMedusa is a DDoS toolkit used by the pro-Palestinian group RipperSec** to conduct HTTP(s) application layer attacks by relaying traffic through open proxy infrastructure.
- **Open threat intelligence neutralized the threat almost immediately.** After proxy identification feeds went public in March 2025, RipperSec’s impactful DDoS attacks declined sharply, even as the group continued claiming victories.
- **Refined intelligence prevents collateral damage.** ASERT reduced 1.1 million candidate proxies to ~100,000 confirmed attack sources, protecting legitimate users from being wrongfully blocked during mitigation.
- **This case study proves that timely, shared threat intelligence shifts the advantage to defenders** and can render sophisticated attack tools ineffective.


## RipperSec’s Approach to DDoS


Many adversaries leverage conventional botnets for their DDoS efforts. Conventional botnets tend to talk to a command-and-control (C2) infrastructure, from which compromised hosts receive attack commands. These commands are parsed, and the infected machines start to generate attack traffic accordingly. The pro-Palestinian adversary RipperSec took a different approach. The adversary still uses compromised machines, but these machines act merely as a proxy through which attack traffic is reflected. The original generation of traffic happens through other infrastructure that is under the control of the adversary but is relayed through third-party infrastructure. The DDoS toolkit is called MegaMedusa, and it leverages predefined lists of proxies for HTTPS application layer attacks.


NETSCOUT pairs open threat intelligence data with first-party insights to provide industry-leading[DDoS protection](https://www.netscout.com/solutions/ddos-protection) to customers. Further, analysts from NETSCOUT’s threat intelligence division ASERT discovered the real value of threat intelligence sharing within the industry. By the time an openly accessible threat intelligence feed on MegaMedusa-victimized proxy infrastructure appeared, NETSCOUT rarely observed impactful application layer attacks referenced to RipperSec anymore.


### How MegaMedusa Works


MegaMedusa is a project composed of various tools to stress systems with HTTP request floods. The actual attack tool is an obfuscated NodeJS script that loads a list of proxies from a local file and then generates HTTP(s) requests relayed through these proxies in a multithreaded environment. The DDoS tool comes with a few helper tools. One is a predefined list of proxies to use for the tool. It consists of more than 30,000 proxies. It is unclear how often this list is updated and how functional the many potential proxies are. Next, there are two scraper tools that download additional proxies from known and public lists of open proxies. The scraper downloads proxy candidates and performs rudimentary verification on whether or not the infrastructure responds with protocol-compliant messages. Last but not least, the DDoS toolkit has an installer script that prepares a host with all dependencies.


### Candidate Infrastructure and Misused Proxies


A MegaMedusa attack relays all HTTP traffic through proxies. For such an attack, it is critical to block incoming traffic from proxies. To provide its world-class protection, NETSCOUT pairs third-party feeds with first-party intelligence. On March 11, a new feed surfaced on the internet. The new feed, with the name MiniMedusa in allusion to the name MegaMedusa, shares lists of candidate IP addresses with a rudimentary labelling on the origin of said list. The feed became widely known in the industry and was prioritized to be harvested regularly by Day 1 for ASERT’s threat intelligence.


Out of 1.1 million candidate IPs that are checked as potential proxy candidates, around 100,000 make it into ASERT’s threat intelligence feeds daily (see Figure 1). In the same time, the number of RipperSec claims is slightly declining, although as of late October, the increase of observed proxies in ASERT’s threat intelligence data does not correspond with the threat actor’s attack claims.


It becomes apparent that the number of attack claims from the threat actor is likely decoupled from the number of proxies being observed in NETSCOUT’s feeds. Furthermore, there is a multitude of snapshots of the MegaMedusa toolkit online. Thus, actors other than RipperSec might also leverage the tool for their own undertakings.


**Figure 1: A daily number of illicitly used open proxies confirmed through continuous monitoring of DDoS attacks sources. The trend loosely resembles the weekly number of DDoS attack claims by RipperSec, the threat actor behind the DDoS tool.**


ASERT’s intelligence team reduces the original feed to around 10 percent of its total by pairing it with source IPs of observed DDoS attacks. A sigmoid function assigns confidence levels to each IP address based on how many attacks it participated in. As Figure 2 shows, a large number of candidate proxies pose attack sources rather rarely, whereas a few hundred appear in numerous DDoS attacks. The confidence is influenced not only by the attack frequency but also by the previously mentioned label of the public MiniMedusa feed. Distinguishing between frequently used harmful proxies and rarely used ones helps customers separate attack traffic from legitimate traffic during a DDoS attack.


**Figure 2: Overview of elements in the MegaMedusa feed to protect service providers and enterprises alike from HTTPS application layer attacks. By default, NETSCOUT’s Arbor Edge Defense/Arbor Enterprise Manager (AED/AEM) drops traffic for all elements with a high confidence of 80 and beyond.**


## Effectiveness of Intelligence Feeds


In Figure 3, the attack claims are mapped onto observed DDoS attacks in NETSCOUT’s attack database. On March 11, the MiniMedusa feed went online. Since then, it has provided candidate proxy infrastructure potentially misused by the MegaMedusa toolkit. The appearance of this public feed yielded a sudden drop of impactful layer 7 DDoS attacks as observed by NETSCOUT.


**Figure 3: NETSCOUT mapped RipperSec’s attack claims onto observed DDoS attack telemetry data. The visualization reveals that the introduction of an open feed of MegaMedusa’s proxy candidates yielded a decline in impactful DDoS attacks.**


The graph not only shows the absence of impactful MegaMedusa DDoS attacks since mid-june but also plots three events: The introduction of the public feed, ASERT’s analysis feed to prove its effectiveness, and the introduction of the MegaMedusa feed to protect NETSCOUT’s customers from proxy-relayed DDoS attacks.


The second half of 2025 evidently demonstrates the effectiveness of threat intelligence feeds, both open and proprietary. The decline of notable DDoS attacks from the adversary group RipperSec is a prime example of the success of threat intelligence sharing within the industry.


Arguably, RipperSec’s observed DDoS attacks declined the moment the open feeds went online. ATLAS identified a need to pair the open feeds with its own threat intelligence data. The decline of 1.1 million daily candidates to merely a few hundred IPs (or a few thousand, depending on confidence levels) reduces collateral damage substantially. After all, many open proxies are present in residential area networks, meaning a wrongfully blocked IP address will restrict a household from accessing certain parts of the internet.


The data shown is as complete a picture of the global threat landscape as NETSCOUT can draw it. Claims of RipperSec still appear on the threat actor’s Telegram channels. Still, there is a lack of noticeable DDoS attacks attributed to RipperSec’s claims, which demonstrates a resiliency milestone the moment a to-be-disclosed benevolent contributor started to share threat intelligence data openly for the sake of helping the industry. RipperSec continues to claim successful attacks, but current threat intelligence gives defenders a clear advantage.


## Conclusion


The case of MegaMedusa demonstrates the power of threat intelligence sharing in cybersecurity defense. When open-sourced threat intelligence identifying RipperSec’s proxy infrastructure became available in March 2025, the impact was immediate and measurable. Notable DDoS attacks attributed to this threat actor declined sharply, even as the group continued making attack claims on its Telegram channels.


This success story highlights two critical lessons for defenders:


**First, timely intelligence matters.** The near-instant drop in impactful attacks after the MiniMedusa feed went public shows that defenders can neutralize threats when they have access to current, actionable data about attack infrastructure.


**Second, refined intelligence prevents collateral damage.** By pairing open-source threat intelligence with NETSCOUT’s proprietary analysis, ASERT reduced 1.1 million candidate proxies to approximately 100,000 confirmed attack sources. This refinement protects legitimate users, particularly households behind residential proxies, from being wrongfully blocked during mitigation efforts.
NETSCOUT’s ATLAS Intelligence Feed (AIF) continues to monitor and validate proxy infrastructure used in MegaMedusa attacks, providing customers with high-confidence threat data that balances comprehensive protection with minimal false positives. The combination of community-shared intelligence and NETSCOUT’s global visibility shifted the advantage decisively back to defenders.


When the industry shares threat intelligence openly and responsibly, even sophisticated adversaries such as RipperSec find their tools turned to stone.


[Learn more about AIF and NETSCOUT’s threat intelligence capabilities](https://www.netscout.com/product/atlas-intelligence-feed) **.**

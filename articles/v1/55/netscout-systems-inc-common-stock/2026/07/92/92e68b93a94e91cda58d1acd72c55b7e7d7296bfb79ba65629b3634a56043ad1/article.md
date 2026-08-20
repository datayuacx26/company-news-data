---
schema_version: "1.0.0"
document_id: "92e68b93a94e91cda58d1acd72c55b7e7d7296bfb79ba65629b3634a56043ad1"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/winter-games-effect-when-gold-meets-ddos"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:fcc7cad840694394479505909acf8342871ed036719205396801a1555e07f970"
---

# The Winter Games Effect: When Gold Meets DDoS

## Executive Summary


Major international events attract not just global audiences but also distributed denial-of-service (DDoS) attacks. The Milano Cortina 2026 Winter Games proved no exception: DDoS attack volume against Italian infrastructure in the country surged 181 percent over 2025 levels, which were themselves elevated by sustained NoName057(16) campaigns. Attackers treated the Winter Games calendar as an operational playbook, escalating two weeks before opening ceremonies and terminating abruptly after the event’s closing.


### Key Findings


- Attack volumes 6–10x historical levels during the Winter Games period (February 6–23, 2026)
- Peak attack count reached more than 2,200 attacks on February 23
- NoName057(16) dominated public DDoS hacktivist claims with 47, although ransomware groups (Qilin, LockBit 5.0) also claimed success in various attacks
- Tactical shift from pre-Winter Games high-bandwidth attacks (412.89Gbps peak) to Winter Games-period high-throughput attacks
- Geographic concentration on Milan (Winter Games cohost), Cortina infrastructure (hotels, ski sites), and symbolic targets (consulates, defense)


### Temporal Analysis


Attack activity in Italy can be divided into three distinct periods:


**Pre-Winter Games (January 20–February 5):** 4,963 attacks, averaging 300 daily. The first escalation occurred January 22, with triple the 2024 and 2025 levels for comparable dates. January 25 recorded maximum traffic intensity of 412.89Gbps, with average bandwidth per attack ranging from 0.74 to 5.45Gbps. These high-magnitude attacks suggest testing network capacity limits and defensive responses.


**Winter Games period (February 6–23):** 12,963 attacks, averaging 720 daily. This period accounted for 56 percent of total attack volume and 6x times higher attack activity compared with 2024 and 2025. Activity escalated from 191 attacks on February 16 to 1,890 on February 23. During the Winter Games, we can see tactical shifts in attack types to packet rate­–intensive patterns to sustain pressure via quantity over bandwidth knockouts.


**Post-Winter Games (February 24–March 3):** 5,315 attacks, peaking at 2,281 on February 24—a record-breaking single-day count representing the highest attack volume observed against Italy in the past three years. Activity declined 88 percent to 272 attacks by February 26.


Figure 1: Year-over-year DDoS attack comparison during Milano Cortina 2026


The temporal pattern with the Winter Games calendar shown in Figure 1 illustrates the dramatic escalation in attack frequency during 2026 compared with prior years. Notably, the five highest single-day attack counts recorded against Italy in the past three years all occurred during the February 17–25 period: February 24 (2,281 attacks), February 23 (1,890 attacks), February 21 (1,865 attacks), February 22 (1,828 attacks), and February 20 (1,684 attacks). Beyond attack volume, the intensity of individual attacks also showed significant variation throughout the observation period.


### Attack Vector Analysis


Attackers combined multiple techniques simultaneously, averaging more than two vectors per attack; UDP flooding was dominant, showing up in 85 percent of attacks, while additional DDoS attack vectors showed up in 87 percent, meaning most attacks mixed direct UDP flooding with amplified traffic from devices susceptible to reflection/amplification.


Figure 2: Attack vector distribution during Milano Cortina 2026


UDP flooding led attacks at 85 percent, followed by DNS amplification (19 percent), and memcached amplification (11.8 percent). The amplification toolkit extended to NTP (7.5 percent), STUN (6.6 percent), SSDP (5.9 percent), and SNMP (5 percent).


### Threat Actor Claim Analysis


Between February 4 and February 24, 2026, threat actors publicly claimed responsibility for attacks targeting Italian infrastructure via social media and Telegram channels. These claims represent self-reported attribution and have not been independently validated against observed DDoS telemetry.


NoName057(16) dominated claims activity with 47 attacks claimed against Italy during this period, representing 40 percent of all the attribution. This represents notable concentration of NoName activity: The group claimed a total of 488 attacks globally during same time frame, meaning Italy accounted for 10 percent of NoName’s global targeting. Historical analysis shows that between December 1 and February 3, NoName057(16) claimed only one attack against Italy and made no claims toward Italy after February 28, 2026, indicating the Winter Games period attracting the threat actor.


Secondary actors generated 60 percent of the remaining claims, but in substantially lower claim volumes during the Winter Games period: Server Killers (8 claims), Z-Pentest Alliance (4 claims), Dark Storm Team (3 claims). The remaining 47 attacker claims were distributed among ransomware groups, individual actors, and various entities.


### DDoSia Analysis


DDoSia is a homegrown DDoS platform developed by[NoName057(16)](https://www.netscout.com/blog/asert/noname057-16) operating since early 2022. DDoSia detection events by the NETSCOUT ASERT team recorded 3,491 attacks against 74 unique Italian domains during pre-Winter Games and Winter Games periods, aimed at disrupting the Winter Games infrastructure, government operations, and critical services. The primary vectors include HTTP/HTTPS/HTTP2 floods; TCP floods on port 80, 443, 2222, 8080; and slowloris-style resource exhaustion attacks.


Target Category


Examples


Top Targeted Domains


Winter Games Infrastructure Cortina hotels, ski entries, transportation


hotelcortina.com (168)


stradeanas.it (108)


regione.emilia-romagna.it (100)


Government Ministries, regional/municipal sites, consulates


mit.gov.it (80)


consulates abroad (70–75 each)


Critical Infrastructure Defense, ports, airports, utilities


leonardo.com (48)


aeronautica.difesa.it (28)


porto.trieste.it (25)


Telecommunications ISPs, telecom providers


Tiscali domains


communication infrastructure


Table 1: DDoSia target categories and observations count during Milano Cortina Winter Games 2026


### Aisuru IoT Botnet Analysis


[Aisuru](https://www.netscout.com/blog/asert/asert-threat-summary-aisuru-and-related-turbomirai-botnet-ddos) operates as a Mirai-derivative Internet of Things (IoT) botnet first disclosed in August 2024, comprising of more than 1 million compromised consumer routers, cameras, and IoT systems. (Note: This analysis period is prior to the recent[law-enforcement takedown action](https://www.justice.gov/usao-ak/pr/authorities-disrupt-worlds-largest-iot-ddos-botnets-responsible-record-breaking-attacks) impacting the Aisuru botnet.) The botnet functions as DDoS-for-hire services, with attacks of up to 31Tbps purported. After observing dominance of direct-path UDP flooding—the most common Aisuru attack type—NETSCOUT’s ASERT team tracked more than 683 Aisuru-tagged instances against Italian cities, with Milan absorbing 94 percent of activity (642 instances).


City Instances Percentage


Milan 642 93.9%


Scarperia 15 2.1%


Torino 7 1.0%


Sona 7 1.0%


Table 2: Aisuru geographic distribution during Milano Cortina Winter Games 2026


### Conclusion


The DDoS campaign targeting Italy during the Milano Cortina 2026 Winter Games demonstrated how major international events create predictable windows for coordinated cyberthreat activity. The attack activity observed against Italy between January and March 2026 demonstrates an elevated DDoS threat landscape as compared with global DDoS trends during the same periods in prior years. The attacks represented a 181 percent frequency increase compared with 2025, when Italy was the target of NoName057 due to global geopolitical tensions reported by the ASERT team in the “Italy in the Crosshairs” campaign.[Global visibility via NETSCOUT ATLAS threat intelligence](https://www.netscout.com/resources/?page=1&areas_of_interest=Network+Performance+Monitoring+%2528NPM%2529) and[adaptive DDoS protection](https://www.netscout.com/ddos-protection) via NETSCOUT Arbor products equip organizations with robust and proactive defense strategies, ensuring the supporting infrastructure of major international events remains undisrupted.

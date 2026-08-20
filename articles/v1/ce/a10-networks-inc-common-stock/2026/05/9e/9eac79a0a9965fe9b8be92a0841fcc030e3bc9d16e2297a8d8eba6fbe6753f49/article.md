---
schema_version: "1.0.0"
document_id: "9eac79a0a9965fe9b8be92a0841fcc030e3bc9d16e2297a8d8eba6fbe6753f49"
company_key: "a10-networks-inc-common-stock"
company: "A10 Networks Inc."
source_id: "a10-networks-inc-common-stock-rss-284845abe605"
canonical_url: "https://www.a10networks.com/blog/carpet-bombing-ddos-the-attack-pattern-your-per-ip-defenses-wont-catch/"
published_at: "2026-05-04T23:30:33+00:00"
first_seen_at: "2026-07-23T23:21:56.777318+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:1f3ad6b5e6e75314e39313abec8ef8aff8cff0ab8968400dad8ff53bc0048dcf"
---

# Carpet-Bombing DDoS: The Attack Pattern Your Per-IP Defenses Won’t Catch

## DDoS Carpet-bombing Attack Unfolds in Real Time. Here’s What It Looked Like.


25 Ips, one subnet, 40 minutes, and your per-IP rate limiter didn’t see a thing


## A Quiet Morning in South Asia


At 07:35 UTC on April 28, 2026, a regional ISP in South Asia started taking fire. Not the kind of fire that trips alarms. No single IP experienced a traffic spike dramatic enough to cross a typical per-IP detection threshold. But something far more deliberate was underway.


Over the next 40 minutes, we watched through[A10 Defend Threat Control](https://www.a10networks.com/products/a10-defend-threat-control/) as attackers systematically walked through more than 25 distinct IP addresses within a single /24 subnet, hitting each one with high-complexity NTP amplification. A new target every 30 to 90 seconds. Each attack lasted anywhere from 21 seconds to nearly 8 minutes. Then the attacker moved on to the next address.


This is carpet-bombing and it is one of the most effective DDoS evasion techniques in use today.


## What Carpet-bombing Looks Like in the Data


Here is a condensed view of the attack timeline, pulled directly from the A10 Defend Threat Control victim data. Every row is a separate attack event. Every IP belongs to the same /24 owned by the same regional broadband provider.


Time (UTC) Victim IP (Masked) Duration Vector


07:35:17 160.250.82.x (.45) 21 sec NTP – High


07:35:41 160.250.82.x (.242) 404 sec NTP – High


07:36:16 160.250.82.x (.115) 57 sec NTP – High


07:37:14 160.250.82.x (.56) 80 sec NTP – High


07:38:36 160.250.82.x (.207) 54 sec NTP – High


07:39:32 160.250.82.x (.43) 63 sec NTP – High


07:40:40 160.250.82.x (.232) 37 sec NTP – High


07:42:27 160.250.82.x (.176) 49 sec NTP – High


07:43:20 160.250.82.x (.141) 354 sec NTP – High


07:44:26 160.250.82.x (.70) 56 sec NTP – High


07:45:25 160.250.82.x (.215) 77 sec NTP – High


07:46:44 160.250.82.x (.254) 29 sec NTP – High


07:47:15 160.250.82.x (.229) 67 sec NTP – High


07:49:16 160.250.82.x (.85) 88 sec NTP – High


07:50:51 160.250.82.x (.117) 37 sec NTP – High


… … (25+ IPs total) … …


08:01:14 160.250.82.x (.131) 479 sec NTP – High


Figure: Condensed timeline of the carpet-bombing campaign. Full dataset available in A10 Defend Threat Control.


Notice the pattern. No single IP accumulates enough traffic to dominate a dashboard. The attacker distributes the pain across the entire subnet, ensuring that the aggregate bandwidth hitting the victim’s upstream link is enormous, even though each individual target looks only moderately busy.


## Why This Matters More Than You Think


Carpet-bombing exploits a fundamental assumption baked into most DDoS detection systems: that attacks concentrate on a single destination. Traditional mitigation triggers on per-IP volumetric thresholds. When traffic to a single IP spikes, you scrub it. Simple.


But what happens when no single IP crosses the threshold? When the attacker instead sends 40 Gbps spread across 25 IPs, each absorbing just 1.6 Gbps. Individually, those flows might sit below your alerting baseline. Collectively, they saturate the upstream link and take the entire subnet offline.


This is not theoretical. In this campaign, the targeted ISP’s entire /24 was under sustained NTP amplification for 40 continuous minutes. The longest single-IP attack lasted nearly 8 minutes (479 seconds), but the campaign lasted far longer because the attacker simply rotated targets.


## The Clues Were There—If You Knew Where to Look


What made this campaign visible in A10 Defend Threat Control was not any single attack event, but the correlation across events. The platform captures victim data across all observed attacks globally, which means patterns that span multiple IPs, subnets, and time windows become visible in a way they never would from a single vantage point.


When we queried the data for this /24, the carpet-bombing pattern jumped out immediately: same ASN, same attack vector, same complexity rating, tightly clustered timestamps, and sequential IP targeting. An analyst looking at any individual event would see a routine 60-second NTP hit. An analyst looking at the subnet-level pattern would see an orchestrated campaign.


That difference—between seeing one event and seeing the campaign—is the difference between reacting and anticipating.


> “An analyst looking at one event sees a routine NTP hit. An analyst looking at the subnet sees an orchestrated campaign. That gap is where carpet-bombing lives.”


## What Should Defenders Do?


- **Think in subnets, not just IPs.** If your detection logic only evaluates per-destination thresholds, carpet-bombing will fly under the radar. Aggregate traffic analysis at the /24 or prefix level is essential.
- **Correlate across time windows.** A 60-second NTP attack is unremarkable. Twenty-five of them against the same subnet in 40 minutes is a campaign. Your tooling needs to surface that pattern automatically.
- **Use external threat intelligence.** The victim ISP in this case may not have had visibility into the broader pattern. They only see their own ingress. A platform like[A10 Defend Threat Control](https://www.a10networks.com/products/a10-defend-threat-control/) provides the global vantage point that turns isolated alerts into actionable intelligence.
- **Assume NTP amplification isn’t going away.** Every attack in this campaign used NTP reflection at high complexity. Open NTP servers continue to provide massive amplification, and attackers know it.


## See it for Yourself


This carpet-bombing campaign is just one pattern we surfaced from over one million DDoS attacks observed by A10 Defend Threat Control in April 2026 alone. The platform lets you filter by attack type, victim country, ASN, industry, duration, and more, turning raw data into the kind of intelligence that keeps you ahead of the next campaign.


Explore the live data on the[A10 Defend Threat Control dashboard](https://threats.a10networks.com/) or[sign up for a 90-day free trial](https://www.a10networks.com/products/a10-defend-threat-control-trial/) to see what attackers are doing right now.


### Share this post:


[Share on X (Twitter)](https://twitter.com/intent/tweet?text=Carpet-Bombing%20DDoS%3A%20The%20Attack%20Pattern%20Your%20Per-IP%20Defenses%20Won%E2%80%99t%20Catch&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fcarpet-bombing-ddos-the-attack-pattern-your-per-ip-defenses-wont-catch%2F)[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fcarpet-bombing-ddos-the-attack-pattern-your-per-ip-defenses-wont-catch%2F)[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fcarpet-bombing-ddos-the-attack-pattern-your-per-ip-defenses-wont-catch%2F&title=Carpet-Bombing%20DDoS%3A%20The%20Attack%20Pattern%20Your%20Per-IP%20Defenses%20Won%E2%80%99t%20Catch&source=https%3A%2F%2Fwww.a10networks.com)Share on Email


Categories:


[Cyber Security](https://www.a10networks.com/blog/category/cyber-security/)[Network Security](https://www.a10networks.com/blog/category/network-security/)


---


** Previous Post


[AI Security is Being Over Thought](https://www.a10networks.com/blog/ai-security-is-being-over-thought/)


Next Post **


[Post-quantum Cryptography Comes to A10 SSL/TLS Data Plane](https://www.a10networks.com/blog/post-quantum-cryptography-comes-to-a10-ssl-tls-data-plane/)


---


[Anidhya Pandita](https://www.a10networks.com/blog/author/apandita)


| May 4, 2026

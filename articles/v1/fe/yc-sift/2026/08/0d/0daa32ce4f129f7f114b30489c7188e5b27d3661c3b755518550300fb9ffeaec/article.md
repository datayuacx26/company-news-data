---
schema_version: "1.0.0"
document_id: "0daa32ce4f129f7f114b30489c7188e5b27d3661c3b755518550300fb9ffeaec"
company_key: "yc-sift"
company: "Sift"
source_id: "yc-sift-rss-c48c6d88eecb"
canonical_url: "https://sift.com/blog/one-attack-two-dashboards/"
published_at: null
first_seen_at: "2026-08-13T20:10:41.208623+00:00"
fetched_at: "2026-08-13T20:10:43.884382+00:00"
content_hash: "sha256:e71a5774afa45b9d6ebdcb1d3d27c7334b3dae50e9c307e31296fdfb5c7dbbae"
---

# One Attack, Two Dashboards

## **Part 2: How the CISO and the Fraud Leader See the Same Compromise**


*The security team sees a contained incident. The fraud team sees losses with no origin story. Both are right. That’s the problem.*


In[Part 1](https://sift.com/blog/cybersecurity-has-a-fraud-problem/) of this series, I argued that account takeover (ATO) is one event with two owners: a security failure and a fraud event running through the gap between two teams that rarely compare notes. This article walks through the same attack twice, once from each side, because the fastest way to understand the gap is to look at what each leader can and can’t see.


## **The view from the SOC**


Start with what the security leader actually sees. The Security Operations Center (SOC) catches the front of the attack: impossible travel, device anomalies, a spike of credential stuffing against the login endpoint. The team triages, contains the wave, and closes the incident. Mean time to detect (MTTD) and mean time to contain (MTTC) both look strong.


Those metrics measure how fast the SOC reacts, and they measure it well. What they can’t measure is what happened inside the accounts that got through. A credential-stuffing spike may look contained once the login defense holds, but the accounts that slip past can sit quietly for days before changing recovery details, testing small transactions, or draining stored value. None of that activity looks like an intrusion, but it’s where the actual damage happens.


Christopher Mascaro, chief cyber and fraud officer at payments processor North, describes the measurement problem: “AI can process a million transactions in seconds, but it takes leadership to ask whether we’re measuring the right things in the first place.”


The control set has the same blind spot as the metrics. Multi-factor authentication (MFA) is necessary and no longer sufficient: Obsidian Security estimates that more than 65% of breached accounts had MFA enabled at the time of compromise, a figure Sift highlights in its[Q2 2026 Digital Trust Index](https://sift.com/index-report-q2-2026/) . Attackers phish one-time passcodes, hijack live sessions, and provision stolen cards into digital wallets that inherit the victim’s trusted status. Once the login succeeds, everything the SOC watches goes quiet, and everything that matters next happens somewhere else.


The blind spot shows up even in the frameworks CISOs use to set priorities. Gartner® 2025 ThreatScape matrix, which plots threats by signal quality and attacker advantage, tracks account takeover as a high-signal “complex threat.” Customer account takeover appears separately, in the latent zone, where Gartner notes threats are “below the radar for most organizations” (Gartner, “Update Your Cybersecurity Priorities in a Shifting Threat Landscape,” September 2025). In my view, the industry’s own prioritization map treats the enterprise version of this attack as a known fight and the customer version as barely visible. The reason is a visibility gap: the signals that expose customer ATO often live in fraud’s systems, outside the security team’s view.


## **The view from the fraud desk**


The fraud leader meets the same attacker weeks later, logged in as a paying customer. What lands in the queue is an altered payment method, an odd refund pattern, a payout account changed the day before a withdrawal. The chargeback that finally confirms the fraud arrives 30 to 90 days after the transaction, which is itself weeks after the login that started it. By then the trail back to the compromise has gone cold, and the case gets worked as an isolated loss rather than the tail end of a security event.


The fraud team also owns a cost the security team rarely sees: the price of being wrong in the other direction. Across the Sift Global Data Network, a false-positive decision costs $135 on average, and up to $496 in Digital Commerce. With average order values ranging from $14 in Food & Delivery to $342 in Internet & Software, blunt controls scale badly; the same rule that blocks a bad actor also blocks the loyal customer whose lifetime value dwarfs the fraud it prevented. Without access to the identity and session signals sitting in security’s stack, the fraud team’s main remaining lever is friction, and friction costs the business revenue.


## **What neither dashboard shows**


Put the two views side by side and the gap becomes visible. Security’s picture ends at containment. Fraud’s picture starts at monetization. The stretch in between, where a compromised account quietly changes recovery details and probes for value, belongs to no one. The people most likely to notice are the customers themselves: in Sift’s Q2 2026 consumer survey, only 37% of ATO victims found out because the company or platform told them. The rest discovered it on their own, noticing suspicious activity (30%), hearing from a friend, family member, or colleague (18%), or finding themselves locked out of their own account (13%).


The pattern is measurable from the practitioner side as well. In research commissioned by Mastercard, 60% of fraud and risk executives said they typically do not learn about cyber breaches until after fraud losses have already begun, and roughly a quarter of card issuers and acquirers reported having no formal process for cyber and fraud collaboration at all. Urooj Burney, who leads Mastercard’s cybersecurity payments ecosystem work, describes the result: “Fraud and cybersecurity teams are chasing the same criminals down parallel tracks.”


Sift’s network data puts a number on that fragmentation: users associated with fraudulent chargebacks show 15.8x higher Mean Global Linkage (Sift’s measure of connection to broader coordinated patterns) than users without chargebacks. In other words, the losses a fraud team writes off as isolated incidents are usually the local edge of an organized operation that neither team, and often neither company, can see end to end.


The people closest to the network data describe the shift the same way.


*“Fraud no longer happens at a single point in the transaction. It unfolds across the entire customer lifecycle—from account creation and login to payment and post-transaction activity. The organizations that perform best are the ones benchmarking across all of those stages and using that visibility to make smarter risk and revenue decisions.”*


— Kevin Lee, Field Chief Technology Officer, Sift


## **The questions have to change**


In practice, this means the questions teams ask need to change. Instead of “Was there a breach?”, teams need to be asking “What happened after access?” Did the attacker move to monetize right away? Did the account become part of a larger fraud pattern? Did the added friction end up punishing the real customer instead? Did four different teams each hold a piece of the answer while no one owned the response?


Answering those questions takes more than goodwill between two leaders. It takes an operating model built for the attack path both of them are already fighting.


*Next in this series: Part 3, Closing the Gap: a five-part operating model for what happens after the login succeeds.*


*Gartner, Update Your Cybersecurity Priorities in a Shifting Threat Landscape, By Jeremy D’Hoinne, John Watts, 18 September 2025. Gartner is a trademark of Gartner, Inc. and/or its affiliates.*

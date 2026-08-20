---
schema_version: "1.0.0"
document_id: "aab5b094291f2d8ff7d2badac78e367526311f7e14c4ca02cfde1da293ac27c6"
company_key: "telefonica-brasil-s-a-american-depositary-shares-each-representing-one-common-share"
company: "Telefonica Brasil S.A."
source_id: "telefonica-brasil-s-a-american-depositary-shares-each-representing-one-common-share-news-import-015ec63f53f8"
canonical_url: "https://www.telefonica.com/en/communication-room/news/telefonica-and-siminn-pioneer-in-demonstrating-a-tv-roaming-use-case-based-on-the-open-catching-standard-defined-by-svta/"
published_at: "2023-04-18T14:57:05+00:00"
first_seen_at: "2026-07-22T15:56:20.683264+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:9cd3d980b8a2a794329255b200fa5bf9084694fcbf43f61613bfcb5e3d53ec1d"
---

# Telefónica and Siminn pioneer in demonstrating a TV roaming use case based on the Open catching standard defined by SVTA

## A growing trend: video traffic over operators’ networks


TV services are for many operators a core part of their commercial offering, along with their connectivity services.


Video traffic already accounts for the majority of traffic carried by operators’ networks and is therefore one of the main drivers of their investments. This trend will continue and become even more pronounced in the coming years.


The desire to offer the best QoE to users of their TV services and, at the same time, minimise their impact has led most operators to deploy their own CDNs (Operator CDNs) on their networks. However, this approach has two weaknesses that greatly limit its potential:


- When subscribers to the operator’s TV service connect to it from outside the operator’s network, the operator’s CDNs are no longer the best alternative to ensure the best quality and the highest efficiency, as they are no longer the closest to the user. The frequency of this situation depends on video content rights (e.g. whether they are limited to certain countries, cities, etc.) and regulations, such as in the EU, where regulations guarantee users access to their video services from any EU country.
- They were not designed to handle any video traffic distributed over the operator’s network, but in general only traffic related to the operator’s own video services, leaving out most traffic (e.g. Netflix, DAZN, Google, Amazon, …). This third party traffic is usually handled through content providers’ CDNs or commercial CDNs (e.g. Akamai, Cloudfront, Edgeio, …), either with caches deployed inside or outside the operator’s network.


## A new concept: TV Roaming


The **TV Roaming** concept is a proposal to solve the first of the above limitations and consists of applying similar concepts to the well-known roaming in mobile networks: When a user accesses our TV service from another operator’s network, it is the CDN of this visited operator that delivers the content on behalf of the originating Operator and under its control. In this way, it is possible to serve video content from the CDN of the Operator closest to the user, which allows us to offer a better QoE and at the same time generate savings for both operators, thanks to the reduction of peering traffic and optimised delivery on the visited operator’s network.


## Open Caching, the standard defined by the Streaming Video Technology Alliance (SVTA)


The technical solution for implementing TV Roaming is based on the Open Caching standard, defined by the Streaming Video Technology Alliance (SVTA), which, thanks to its standard APIs, allows content providers and CDNs to interconnect with each other.


In the TV Roaming proof of concept carried out by Síminn and Telefónica, both operators have interconnected their respective carrier CDNs using the Open Caching standard. In the case of Síminn, its CDN is based on Broadpeak technology, while Telefónica has used its own technology, called TCDN. The tests have focused on the scenario where a Síminn user accesses his TV service from Spain via Telefónica’s network, and Telefónica CDN (TCDN) is delegated for local delivery instead of a remote delivery from Síminn CDN in Iceland. For now, the tests have been conducted in laboratory environments, but the aim is to be able to take the same proof of concept to production in the coming months.


The solution to the second limitation above is to get the Operators’ CDNs to deliver all the video traffic that is consumed through each operator’s network, as they are the only ones that can guarantee the best QoE of the service and at the same time the highest network efficiency, thanks to their greater capillarity and proximity to the end user. In reality, the scenario is the same as in the case of TV Roaming, but instead of the Home Operator Network, we are talking about a Content Provider or a Commercial CDN, therefore, the technical solution is also the same: Open caching.


Thanks to the TV Roaming scenario, Operators have a great opportunity to leverage their CDNs and contribute to the consolidation of Open Caching as an industry standard without having to wait for the big video companies. By betting on this scenario, operators will be able to improve the quality and efficiency of their TV services, and at the same time avoid the main obstacle for the mass adoption of Open Caching in the industry, its commercial availability.


For these reasons and with this vision, Síminn, Broadpeak and Telefónica have decided to join forces to demonstrate the interest, potential and viability of the TV roaming concept.

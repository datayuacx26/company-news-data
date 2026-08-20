---
schema_version: "1.0.0"
document_id: "498f105fc4cef803ed445bed424555965e19420bdee8e3c2d326ce67bb97c2fe"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/dns-root-server-attacks"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:9dc7dad278aeb99fa4bf5f12f9039f40a03afab50c69424934a826dff63b5ee9"
---

# DNS Root Server Attacks

## Executive Summary


The internet is a system of systems. There is no central organizing committee that governs how it is constructed and operated. There are norms and best practices, as well as agreed-upon standards of operation such as what an Internet Protocol (IP) datagram looks like and how it should be interpreted, but even the behaviors of creating and interpreting IP packets can sometimes vary. For these reasons, to identify the core of the internet, and enforce lasting and comprehensive control over it, is not easy. However, there are a handful of internet subsystems people often name as being critical to the proper and safe functioning of the internet. One such subsystem is the Domain Name System (DNS) root servers. Internet disruptions can take many forms, but if the root DNS system were to become unavailable, it would be practically indiscernible from a complete and total internet outage. In practice, the system’s resiliency and caching behavior of resolvers significantly blunts the likelihood of a complete system failure. Nevertheless, the performance and accuracy of this subsystem is of utmost importance.


The root DNS system has come under attack many times throughout history, and in some cases, we have seen some partial disruption. Overall, however, the DNS root server system has remained robust and widely available. Replication and redundancy of root system component parts, along with high levels of operational care, have largely led to the success of the root server system. However, the root system is always under pressure from high-rate packet floods, route hijacking, and physical sabotage. This blog examines some of these pressures from the perspective of distributed denial-of-service (DDoS) attack traffic to which the root server system is subject.


### Key Findings


* The DNS root server system is highly available and resilient to attack.
* DNS anycast is widely cited as the key mechanism to the system’s resiliency.
* The root servers are subject to significant flood and nuisance traffic every day.
* Different root server instances can receive very different amounts of traffic.


### Background


Most internet client communications start with a DNS query. An application maps an abstract but human-readable name into something about that name such as an IP address. This process is colloquially called the DNS resolution process, and the DNS root servers literally and figuratively stand at the apex of this hierarchical system. They are the entry point into a distributed database that makes mapping names to IP addresses possible. Technically, the internet could operate without DNS, but in practice it has become an important part of the communications process. It is safe to say that the DNS is one of the most important—if not the most important—subsystem of them all. The performance and availability of this system therefore is paramount.


DNS servers come under attack all the time, some more than others. An attack involving the DNS is typically one of two types. The first major type’s purpose is to compromise the integrity of DNS data. This might be performed by altering the source of DNS data itself—by compromising a server and changing zone files, for example. Alternatively, an attacker may try to manipulate a resolution in flight. DNS cache poison attacks are a common vector of attack against the resolution process, for instance.


The second type of attack attempts to disrupt the DNS resolution process by taking an authoritative DNS server in the name space hierarchy out of service. This is a classic denial-of-service attack. The nearer the apex of the name space or for highly impactful zones, a disruption can have far-reaching effects. If the root servers were to be disrupted, for example, this would ultimately cause problems for practically everyone and everything that uses the DNS.


Fortunately, the DNS root server system has rarely been the target of successful integrity or disruption attacks. That is not to say the DNS root system has not been attacked; this Wikipedia page lists a few high-profile attacks DNS root servers have been subject to.


The root server system is extremely well provisioned and operated. There are 12 root server operators and hundreds of root servers located all over the world. Primarily through the use of BGP anycast, the modern root server system is extraordinarily resilient to denial-of-service packet flooding attacks. However, attack attempts still seem to appear from time to time. In the remainder of this article, we examine some of the attacks the root system is subject to, and with the help of third-party data show how well the system has withstood these onslaughts.


### Motivations for DDoS Attacks on DNS Root Servers


The root servers have been subject to a variety of threats, with some degree of success. Due to the extensive redundancy and capacity of the current system, however, disrupting the system with packet flooding?–style attacks is not easy. Furthermore, most modern attacks aim to disrupt a specific subset of service on the internet, not the entire internet itself. Although some attackers may seek to cause general mischief or to exert a show of strength, a degraded root server system would just make everything worse for everyone. This is rarely the objective of today’s internet miscreants. In addition, internet defenders everywhere leap into action the larger and more widespread attacks become. An attack against the root system is not just an attack against the 12 root operators and their systems, but against the entire internet, much of which will respond to thwart attempts to disrupt the system.


So, although attacks on the DNS root occur, most of them are rarely noticed by the public or do not have a significant impact. Nonetheless, we do observe elevated rates of traffic toward the root—traffic that might even overwhelm many other organizations and networks. Attacks against the root may be trying to learn incident response time and defenses. They might also be observing the effect attacks have on public monitoring graphs of performance or response latency—if not for the root specifically, perhaps even local and in-transit networks. The root system, being so central to the internet, is exposed to a lot of suspicious and malicious traffic. Much of this otherwise-unwanted traffic may be simply noise, but whatever the reasons, it is often helpful to study what the root sees, because it just may be a harbinger of what any target on the internet might be up against. What can we learn from analyzing attacks on the root? We explore this question in the next section.


## Analysis


NETSCOUT’s ATLAS visibility platform provides a tremendous amount of telemetry for DDoS attack events. Figure 1 presents a chronological overview of DDoS events aimed at the root servers. The strongest volumetric attack present in the ATLAS dataset shows an attack on the A root server with 21Gb/s of traffic on August 17, 2025.


**Figure 1:** Chronological overview of DDoS attack events on DNS root servers as visible in ATLAS threat intelligence datasets. Illustrated are a total of 38 data points. (The dataset observes no attacks on g.root-servers.net.)


ASERT observes a different set of DDoS attack vectors to different root servers. The A root and the M root face numerous DDoS attack vectors. In contrast, D and H–L root servers are only observed to have seen the combinations of total traffic and Internet Control Message Protocol (ICMP) attacks. Often, the ICMP observations are sympathetic to a DDoS attack, meaning that attackers and/or defenders probe systems to gain insights. In theory, each instance (A through M) of the root should be a mirror of the others.


Why might some root server instances be subject to vastly different amounts of traffic? A variety of reasons could explain this discrepancy. For example, some instances may be preferred by resolvers due to historical accident, topological connectivity, or resolver selection strategy. An interesting speculation of why the A root receives more attacks is because it is the first letter of the alphabet—a dull but probable reason. Root operators deploy different numbers of anycast instances, and those instances are distributed unevenly around the world. Because BGP anycast directs queries to the topologically closest anycast instance, some root instances may naturally attract more traffic, including more noise and invalid queries (see Figure 2).


**Figure 2:** This percentage overview presents the DDoS attack events observed and reveals how some root servers receive a wider array of DDoS attack vectors.


## Discussion


The numerous instances of root servers make it particularly cumbersome to construct a full picture of traffic that reaches the root servers. Although anycast impacts the visibility of external institutions into operational aspects of root server instances, it enhances the resiliency of a DNS root server formidably—a much-desired characteristic for such a critical building block of the internet. The distribution of traffic to different instances provides the advantage of spreading queries out but also isolating sources of DDoS attack traffic to local instances.


Studies over the years have measured significant amounts of query traffic to root servers that are illegitimate. \[Wessel, ISOC\]. Despite the massive overrepresentation of noise to useful query traffic, the steady state of DNS root traffic volumes remains relatively modest compared with other types of services, usually measured in the tens of megabits per second. This is due to the nature of DNS query traffic itself: small, short-lived request/response packets. Long-lived, large data flows don’t occur in the DNS. Furthermore, although the use of DNS over Transmission Control Protocol (TCP) is slightly increasing, TCP-based attacks are still relatively rare and infrequent in the DNS.


What lessons can we learn from the resiliency of the DNS root server system? Simplicity, instance placement distribution, operational diversity, the use of anycast, and of course expert technical operators overseeing it all. These attributes may not be easily replicated in other parts of the internet, but perhaps we can leverage some of what works where it can work in other systems?


## Recommendations


Defenders can take lessons from DNS root server operations. In some cases, their techniques are engineering choices, not commercial purchasing decisions. For example, can anycast be used to help make the attack surface wider and less reliant on single points of failure? To detect and mitigate abusive, ever-changing networks of varying size and duration, we recommend the following:


- **Real-time visibility** into volumetric traffic floods and distributed attack patterns. Tools such as NETSCOUT Arbor Sightline can help surface early signs of trouble and trigger flow-specification and remotely triggered black hole (RTBH) defenses to upstream providers.
- **Proactive mitigation** with automated systems such as Arbor Threat Mitigation System (TMS) or Arbor Edge Defense (AED). These can stop both volumetric floods and more-complex, multivector attacks.
- **Intelligence-driven defense** with feeds such as NETSCOUT’s ATLAS Intelligence Feed (AIF). These provide information about context, what’s trending, who’s being targeted, and how actors are evolving.


Staying ahead of threat actors is an ever-changing job and requires a broad view of where these attacks come from, how they operate, and where they could strike next.


## Resources


- DNS Root Server Hijack:[https://root-servers.org/media/news/2025-06-20_route_hijack.pdf](https://root-servers.org/media/news/2025-06-20_route_hijack.pdf)
- D. Wessels, M. Fomenkov.[Wow, That’s a Lot of Packets. Passive Active Measurement (PAM) Conference 2003](https://catalog.caida.org/paper/2003_dnspackets/cite) .
- [More Than 70% of DNS Root Queries are Junk](https://pulse.internetsociety.org/blog/more-than-70-of-dns-root-queries-are-junk) , Internet Society (ISOC) Pulse blog 2025-10-21.
- [The Power of Names](https://www.netscout.com/blog/asert/the-power-of-names) , NETSCOUT ASERT blog, October 24, 2023.

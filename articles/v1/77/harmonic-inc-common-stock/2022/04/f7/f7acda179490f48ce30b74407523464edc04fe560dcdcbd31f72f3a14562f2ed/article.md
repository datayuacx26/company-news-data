---
schema_version: "1.0.0"
document_id: "f7acda179490f48ce30b74407523464edc04fe560dcdcbd31f72f3a14562f2ed"
company_key: "harmonic-inc-common-stock"
company: "Harmonic Inc."
source_id: "harmonic-inc-common-stock-rss-ca1ab15a9a18"
canonical_url: "https://www.harmonicinc.com/insights/blog/low-latency-broadband"
published_at: "2022-04-29T13:39:58+00:00"
first_seen_at: "2026-07-20T23:17:59.993716+00:00"
fetched_at: "2026-07-28T22:26:32.519238+00:00"
content_hash: "sha256:e182fb70a9e1c7351a9677dcaac8c59bb44af0ad306f7543b23c9dc6c98bd27d"
---

# Low Latency Solutions for Cable and Broadband

## Latency: A Real World Concern with Serious Impact


Latency is considered as the time taken for packets of data to move back and forth between senders and receivers in a network. Reducing latency in the network is one of the many challenges facing any provider of time-sensitive internet applications.


The development of modern broadband communications has enabled the development of technical capabilities that would have seemed fantastical just a few years ago. However, those advances have also made the need to reduce latency even more urgent.


When idle, a DOCSIS network has a round trip time of about 5ms in latency, but under a heavy load this can spike up to 100s of milliseconds. The latter being a bit of a challenges in a world of instant communications.


Latency of more than about 60 milliseconds (ms) has noticeable negative effects on applications, particularly those with interactive elements such as online gaming or videoconferencing. That level of latency decreases to the 20 to 30 ms level for applications using the Metaverse. It is the inconsistent variations in latency that cause an irritating effect in gaming called “jitter”. For end users of these services, this jitter and “lag” can have a tiresome, grating impact on their experience.


## Key Causes Of Latency


Multiple factors may contribute to latency. Not all are related to the efficiency of the access network: processing within laptops, phones, gaming consoles and Wi-Fi routers can all contribute to overall latency. Regardless of the root cause, reducing latency within the network can make a big measurable difference not only in service quality but in terms of network efficiency.


Latency within the network may be the result of several different factors, including:


- **Propagation delay** , determined by the physical distance between sender and receiver,
- **Media acquisition delay** , created by scheduling mechanisms in shared resources like coax that are designed to ensure that only one user is using a transmission slot,
- **Serialization/ deserialization processes,**
- **and** **queuing** – by far the most significant source of latency within networks.


##
How Queueing Creates Latency


Queueing happens when multiple applications share a broadband connection (within a single household, for example). Some of these applications, including video streaming and other applications based on high-volume downloads, are “queue-building”: they increase latency because they try to send a lot of data through the network quickly.


Queue-building applications perform most effectively when the broadband connection allows data to be sent in large volumes and stored in a buffer as it arrives. While this process is underway, congestion avoidance algorithms (such as Reno, used with TCP), which are designed to maximize bandwidth availability, will ensure those buffers are used to their maximum extent. Yet, this increases latency.


Non-queue-building applications, like online gaming, work best when the packets of data they send are not forced to queue in a buffer alongside packets for the queue-building applications. When they are forced to queue, the result is increased latency and a noticeable adverse impact on the non-queue-building applications, regardless of the architecture used in the network.


Queueing will be likely be more and more needed in homes and other end user environments in the next several years. The trend shows an increase in this need in recent years as the number of connected devices in use at the same time has increased. In the home, phones, tablets, computers, laptops, gaming consoles and smart TVs may all be using higher bandwidth, potentially latency-creating and latency-sensitive applications.


## Latency is A Growing Issue


Solving this issue is not just about keeping gamers happy. The pandemic has driven a revolution in working practices for many employers: even if fewer people are working from home (WFH) full-time today than during COVID-19 lockdown conditions, the savings associated with working from home are well recognized by the employer and working from home will remain a practice widely used around the world.


The technology used when working from home has caused an increase to the use of fixed broadband data networks. Applications such as video conferencing (Teams and Zoom for example) are driving the need to provide reliable, low latency solutions to residential customers working from home.


Lockdowns also seem to have increased use of online technologies more generally in many markets, in part because consumers have got more used to using online services for multiple purposes, from shopping to interacting with financial providers and public sector bodies.


For example, in the UK, broadband usage reached 62,000 Petabytes (PB) in 2021, up 20% from 50,000 PB of annual usage in 2020 – which had been a big increase on the 22,000 PB used in 2019,[according to figures from the UK’s largest broadband network Openreach](https://www.openreach.com/news/lockdown-sport-and-festivities--the-story-behind-the-uks-record-2021-broadband-use/) .


In future, even greater increases in usage will follow if or when technologies used heavily within the metaverse, such as virtual reality (VR) and augmented reality (AR), are more widely used.


## What is Low Latency DOCSIS (LLD)?


An important element in the solution to this problem is Low Latency DOCSIS.[Low Latency DOCSIS](https://www.cablelabs.com/technologies/low-latency-docsis) is a technical specification and feature set developed by CableLabs working with DOCSIS vendors and cable operators, launched in 2019 and suitable for use alongside DOCSIS 3.1 and 4.0 equipment.


Deployed by upgrading the software running CMs and CMTSs in the field, LLD can ensure consistent low latency, also referred to as Jitter. We’re talking about under 5 milliseconds on access networks for non-queue building applications. Deployments can also incorporate automatic provisioning and performance measurement tools to demonstrate performance improvements.


## How Low Latency DOCSIS Works


The greatest impact on latency is achieved by addressing the problems caused by queueing, and LLD does just that. LLD enables non-queue building applications like gaming to take a different path through the network, bypassing the delays caused by the queue-building applications. It enables this without hindering those applications’ use of the network.


Non-queue building traffic uses small buffers to reduce latency, but queue-building applications continue to use large buffers. Balancing the requirements of the two types of traffic in this way optimizes the overall flow of traffic through the network.


The process of identifying which traffic flows should pass along the LLD path is thus really important.


## An Effective Solution For R-PHY


Today, flexibility to evolve networks with greater ease is essential. The solutions Harmonic provides with the[cOS™](https://www.harmonicinc.com/broadband/cableos-core-software/)[Platform](https://www.harmonicinc.com/broadband/cableos-core-software/) give operators a clear advantage: optionality. The cOS Platform can be used within Distributed Access Architectures (DAA) in DOCSIS networks regardless of which type of DAA is in use: Remote-PHY (R-PHY).


When R-PHY is used, the DOCSIS MAC and PHY components are deployed separately on the access network head-end and on the node. Both DOCSIS MAC and PHY are co-located at the access node. While the key difference between these approaches is the location of components, in practice this should have little impact on latency, as propagation delay in the access network is not a primary source of latency. So whichever DAA approach an operator is using, deploying LLD capable of operating alongside R-PHY will be an important tool to reduce latency and improve jitter consistancy.


## Anticipating The Metaverse


Latency is becoming a more important factor in determining service quality today. It’s likely that latency will become even more important in the future. The metaverse (or multiple metaverses), including applications like VR, AR and other immersive online technologies will become the basis for a new generation of online services. It’s starting now with the arrival of new forms and ways of working with thanks to evolutions in social media, entertainment, workplaces, education, and multiple other commercial facets.


The metaverse concept and some new services are already being promoted by companies including Facebook (Meta), Sony, Microsoft and Nvidia. And they are supported by a range of enthusiastic investors.


There are already indications that use of these services can and will acquire critical mass: global shipments of extended reality devices are projected to grow rapidly during the next four years from 11 million in 2021 to more than 71 million, including about 40 million standalone VR devices, by the end of 2025,[according to CCS Insight](https://www.ccsinsight.com/press/company-news/extended-reality-is-on-the-path-to-growth/) .


The overall size of the global metaverse market reached $209.77 billion in 2021, but is predicted to reach $716.5 billion in 2027, at a CAGR of 22.7%,[according to Brandessence Market Research](https://brandessenceresearch.com/technology-and-media/metaverse-market-size) .


As use of metaverse devices and services grows this will increase the amount of network traffic that requires Low Latency DOCSIS prioritization. Current VR devices may suffer from latency levels of up to 40ms. While more advanced devices should be capable of avoiding this sort of lag, they will need greater amounts of bandwidth overall. The increased need for bandwidth will likely increase the load on networks. The result will be both the prevalence of latency and the need to use Low Latency DOCSIS to reduce it.


## The Role of Edge Cloud Solutions for Latency Reduction


#### *What is edge cloud?*


Edge cloud solutions for cable and broadband enable the re-location of the cloud, including its applications and data, from outside of the service provider network to the edge of the service provider network.


#### *How do edge cloud solutions reduce latency?*


Moving the transfer of data closer to the edge, and thus closer to the consumer, reduces the latency incurred in the internet and the core service provider network.


#### *Additional advantages of edge cloud solutions for broadband*


Edge cloud solutions could offer a multitude of additional advantages for your broadband business, including operational efficiencies, improved TCO and accelerating multigigabit service roll out to gain new subscribers in the competitive broadband space.


Quality of Experience


Edge cloud can contribute to resolving latency issues ** and meet quality of experience expectations by simplifying the “path of a packet”. Typically, data packets go through the internet, arrive at the service provider’s peering points, travel through its core network and then get transferred all the way to the edge, before finally reaching the end-user.


Simplified workflows


Latency can build at each stage of transfer, which impacts network performance and quality of experience. A major benefit of edge cloud is that most, if not all the data and applications, remain at the edge and avoids additional data transfers in the core of the network, the peering points and the “internet.”


Leveraging edge cloud solutions not only helps reduce latency, but can also provides significant cost savings as the core network and peering points do not need to handle excessive amounts of data traffic.


Readiness for the Ultra-Connected Consumer


Using edge cloud solutions will become necessary, as networks continue to face challenges from the increasing demands coming from usage and roll out of new latency and traffic heavy applications. For example, the metaverse, VR and AR.


Edge clouds will become a facilitator to deliver a higher level of broadband service efficiency and quality with an improived subscribers experience.


Improved total cost of ownership


Using edge clouds also helps to reduce operational costs, by reducing spend on legacy hardware. We’re talking here about the high power and cooling requirements. Edge clouds contribute to less network complexity while increasing the lifetime of individual pieces of equipment, and the network architecture in general.


## Building Future-Proof Broadband Services


Operators need to act now, because alongside the relentless evolution of online technologies, you also have the end user expectations of online services growing just the same. Future-proofing the network, making it fit for purpose today and for years to come, is not an option, but a necessity.


Ultimately, as internet services continue to evolve, the requirement for service operators to reduce latency will increase. Edge cloud solutions will have a key role complimented by LLD.


Service operators needing to deliver and commercialize low latency services should consider Edge Cloud solutions, combined with LLD if needed, as a means to achieve a positive impact on the quality of experience. Harmonic is the leader in providing cloud native edge cloud solutions solutions for DOCSIS networks, whether using R-PHY or R-MAC-PHY architectures. Let us know how we can empower your business to deliver the essential services for the ultra-connected future.

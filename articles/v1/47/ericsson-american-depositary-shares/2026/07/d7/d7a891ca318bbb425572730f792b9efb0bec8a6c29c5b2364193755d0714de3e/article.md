---
schema_version: "1.0.0"
document_id: "d7a891ca318bbb425572730f792b9efb0bec8a6c29c5b2364193755d0714de3e"
company_key: "ericsson-american-depositary-shares"
company: "Ericsson"
source_id: "ericsson-american-depositary-shares-atom-93f46de34b40"
canonical_url: "https://www.ericsson.com/en/blog/2026/7/hybrid-mini-link-packet-evolution-in-microwave"
published_at: "2026-07-01T08:40:35+00:00"
first_seen_at: "2026-07-25T01:08:03.792576+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:3bea8a7556d518d912303516751693cd66be7b054ff72eff540145810174bd59"
---

# And so the hybrid MINI-LINK was born…

Remember the days when TDM test-instruments were the go-to tool for verification of radio link performance, be it to verify a new installation, lab testing or development? If you were in a MINI-LINK development lab up until around 2008, you would find them in heaps, and every designer dreaded the blinking red AIS-led, indicating a signal disruption, when testing their latest feature.


The number of E1s/DS1s supported on a single radio link was very much like the speedometer of a sports car: the higher it went, the more it signaled prestige and performance. In 2007, the Modem Unit, MMU2 D was released, which boasted an impressive capacity of 80 E1s. Up until then, the focus for radio link development had been on increasing the number of E1s but MMU2 D marked the end of the “E1-race”.


The reason for this was the introduction of data services in the mobile network and the emergence of Ethernet as the transport technology carrying these services. In the early stages of 3G, there were provisions for, at the time, fairly high data rates to mobile devices. But with the introduction of the smartphone not yet on the horizon, the limited availability of user friendly applications did not drive the use of packet services in the way we see today.


Nevertheless, the communication service providers (CSP) were keen to introduce packet technology but with the requirement that it must be able to coexist with the TDM infrastructure.


### Time for the hybrid radio link


The[MINI-LINK Traffic Node](https://www.ericsson.com/en/blog/2026/6/the-evolution-of-mini-link-from-wire-substitute-to-traffic-node) was originally derived from a TDM digital cross-connect with its own TDM backplane infrastructure. However, when MINI-LINK TN was introduced, it was on the horizon that TDM would not be the transport technology of the future. Therefore, the clever MINI-LINK engineers had already introduced a high-speed bus in the backplane. As it was not clear what new transport technology would be the winner, this bus had a generic design to be able to handle any kind of technology, be it Ethernet, ATM or SDH.


As things developed it became clear that the future would be IP-based with Ethernet as the transport technology. Even though there were applications using Ethernet over SDH or Ethernet over ATM, native Ethernet became the dominate. The MINI-LINK TN provided Ethernet traffic interfaces both on the Node Processing Unit (NPU) or used plug-in cards should additional interfaces were required.


So, as it turned out in the end, the only application for this bus was to carry Ethernet traffic from the NPU to the Modem units (MMU). As mentioned, the requirement was for the two technologies to coexist, and again, the clever Ericsson engineers were one step ahead. The MMU2 D was not only the TDM-flagship but it was also HW prepared to manage the hybrid radio link.


### “HW prepared” - the promise (and the risk)


HW prepared to work with a later SW release is a phrase that sometimes sends chills down the spine of any developer. To guarantee that an SW application will work on your HW, it of course depends on how the SW is developed. So you release it with some degree of confidence that it will work once the full SW application is in place. But there is always an element of risk when claiming that you are HW prepared.


As it turned out, the HW preparation in MMU2 D worked well and the hybrid radio link was introduced on MMU2 D at the same time as the MMU2 H, which not only supported the hybrid radio link but also included features like XPIC and adaptive modulation (but that’s a whole other story).


So how was it done? From the outside it looks quite straightforward to do this. The below descriptive graphic from the product information material at the time regarding hybrid radio links says it all (or does it?).


Total Link Capacity = m + n m Mbps PDH m Mbps Ethernet PDH Native Ethernet E1/DS1 Granularity


Figure 1. Hybrid traffic


### A look under the hood


Under the hood it was slightly more complicated. The MMUs up until now, had much of its HW architecture built around the behavior of TDM where there is always traffic. In packet traffic you had to adapt to cases where there would be little or sometimes no traffic, and it was not predictable.


TDM has the endearing quality that it cannot be overprovisioned, i.e. the HW resources needed for the number of E1’s you configured were guaranteed through the complete system. From the line interface on one side of a hop to the line interface on the other, you knew exactly what was required to transport it. When developing, you basically had two scenarios: either all traffic was working, or no traffic whatsoever came through, which made troubleshooting a lot easier.


The principle behind the hybrid radio link was that on the MMU, the HW had information about the radio link speed and the required TDM capacity. The NPU sent packet data on the high-speed bus and the MMU signaled back either ‘SEND’ if there was room for packets or ‘STOP’ when the capacity not used for TDM traffic was filled. In this way, the available capacity on the radio link could be used efficiently.


With packet traffic, the incoming traffic could burst up to the line speed of the connection and then had to be squeezed in on a radio link while competing with the TDM traffic. For the first time you could experience that parts of the traffic just disappeared and many late evenings were spent in the lab wondering where they went, before you eventually found the problem and they turned up again.


The hybrid radio link in MINI-LINK TN lived on happily for a few years, but with the increasing demand for data traffic and the drive for higher capacity using wider channels and higher modulation it was clear that MINI-LINK TN was reaching its capacity limit. Something new was needed.


### From MINI-LINK TN to MINI-LINK 6600


When MINI-LINK 6600 was introduced in 2016, the customer requirement for TDM had already started to subside. Many CSPs were moving to a packet-only transport architecture and, of course, with the ever-increasing demand for bandwidth, it was obvious that TDM had long since reached its peak and was now slowly (or quickly depending on CSP) decreasing. It was obvious that many CSPs when modernizing their network, took the chance of removing TDM from their transport networks, as it had become increasingly more expensive to maintain.


But it was still deemed necessary that our new product had to support TDM. But where MINI-LINK TN was a “TDM-machine on which you could run Ethernet” the MINI-LINK 6600 is an “Ethernet-machine on which you can run TDM”. In MINI-LINK 6600, the generic high-speed bus from TN has been replaced with a true Ethernet backplane allowing for higher speeds and simpler solutions leveraging on standard technology.


### Lessons learned


When the E-band products were introduced, it was not on the cards to provide them with TDM-interfaces. Even if TDM sees a dwindling future it was the driver of the hybrid radio link and the start of packet-based transport. It was probably fortunate that packet transport initially was growing at a slower pace, since many lessons were learned on how packet transport over a radio link worked in real-world conditions. The hybrid concept allowed the wrinkles of this innovative technology to be ironed out in a gradual fashion, as TDM was the basis for most traffic.


Many of the lessons learned from the initial hybrid radio links are still valid. Things like use of deep buffers and Quality-of-Service parameters are key in handling the variations in capacity that naturally occurs on a radio link due to changing propagation conditions. And the use of new Performance Management parameters to better understand the utilization, has been vital in the improvements witnessed in the speed and performance in today’s products.


### Read more


- [MIMO for microwave to increase the capacity](https://www.ericsson.com/en/mobile-transport/boost-microwave-capacity)
- [Microwave](https://www.ericsson.com/en/portfolio/networks/ericsson-radio-system/mobile-transport/microwave)
- [50 years of MINI-LINK — and we're just getting started!](https://www.ericsson.com/49f587/assets/content/e91554501ee04f46954553c1de4eacf2/mini-link-50-years.pdf)

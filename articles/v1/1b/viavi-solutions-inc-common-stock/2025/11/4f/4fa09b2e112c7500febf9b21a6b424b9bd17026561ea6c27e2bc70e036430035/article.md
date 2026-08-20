---
schema_version: "1.0.0"
document_id: "4fa09b2e112c7500febf9b21a6b424b9bd17026561ea6c27e2bc70e036430035"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/11/04/validating-l4s-implementations-for-5g-advanced/"
published_at: "2025-11-04T17:15:59+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:55:26.781986+00:00"
content_hash: "sha256:4958e9786748ff969a6b5600211b26c6061e158014c2629a495cf4892df69278"
---

# Validating L4S Implementations for 5G Advanced

**


# Validating L4S Implementations for 5G Advanced


- **Owen O'Donnell
- ** November 4, 2025
- 2


Networks strive to enable both high throughput and low latency. However, as the number of packets moving through a given network reaches their limit, queues start to build, with delays/ congestion introduced as a result.


Congestion therefore needs to be managed but legacy mechanisms for detecting congestion rely on packet loss as a measure that can be used to adapt sending rates… but by the time packets are being lost, delay and jitter are already occurring.


[Three years ago](https://datatracker.ietf.org/doc/html/rfc9331) , we saw the formalization of the Low Latency, Low Loss, Scalable Throughput framework to managing network congestion. Better known as L4S, this technique manages a better trade off and enables ultra-low latencies without sacrificing throughput.


It effectively works like a smart traffic light at a traffic intersection, with network elements proactively signaling congestion in the very earliest of stages, before any packet loss occurs. This is managed through explicit congestion notification (ECN) coupled with a dual-queue-coupled active queue management (AQM) architecture. Early adopters include[Comcast](https://www.rcrwireless.com/20250129/uncategorized/comcast-l4s) and[Apple](https://www.theverge.com/23655762/l4s-internet-apple-comcast-latency-speed-bandwidth#:~:text=Apple%20put%20an,for%20testing.) (on iOS 17) and its use is growing, but the approach is far from universally adopted.


In this blog, we’ll therefore explore what L4S means for 5G RAN and Core engineers, why its validation is a complex, two-sided challenge, and how to build a robust test strategy to ensure a successful rollout.


### **Bufferbloat in 5G**


In any network, and especially in mobile environments, crippling lag can come at any point, even in a high priority 5G network slice for cloud gaming. The RAN (radio access network) is inherently variable and as soon as a slice becomes saturated, legacy congestion management techniques allow queues build in the gNB and UPF, causing latency to spike.


Figure 1: Load vs latency, showing the onset of queuing congestion


Latency isn’t an issue for buffered one way video streaming services such as Netflix. But for cloud gaming, anything more than a 100 ms latency spike from bufferbloat will be noticeable – almost unplayable. In augmented reality, the same latency spike can lead to cybersickness. And in remote robotics or autonomous automotive V2X systems, this lag would be a significant safety breach.


To solve this, 3GPP integrated L4S into last year’s Release 18 for 5G Advanced. This specifies how NG-RAN and/or UPF can detect/mark L4S traffic for XR and other latency-sensitive services and (in theory) means interactive apps can use 100 percent of the available bandwidth with zero (or near zero, at least) queuing delays.


### **The 5G L4S architecture**


For L4S to be effective, it requires full cooperation between the 5G network and the end user’s device.


As we mentioned above, a dual-queue AQM is needed for the network side, with[3GPP standards allowing](https://www.3gpp.org/technologies/xr-sa2?utm_source=chatgpt.com) it to be implemented at either the NG-RAN (gNB) or in the 5G core’s user plane function (UPF). Being closest to the radio congestion, the gNB is arguably best.


Regardless of location, the mechanism is the same, with traffic physically separated into two queues for legacy (classic) and L4S-aware (L) traffic. This ‘L’ queue is kept extremely short to prevent delay.


Similarly for both AQM locations, the gNB is used to detect in its radio scheduler queue. The difference comes in how the ECN marking is achieved, with the gNB needing to forward this information on to the UPF for ECN, if the AQM is located there; but with both detection and marking taking place at the same location if positioned at the NG-RAN.


If we now turn to the client side. Here the mobile OS on the UE must run a scalable congestion controller, with equipped devices signaling their capability through an ECT(1) codepoint marking on packets. Through this signal, the gNB or UPF is instructed to deliver the packet to the L queue.


If congestion is detected, the AQM switches the ECT(1) to a CE codepoint. On receipt, the L4S controller on the UE makes a small, but immediate reduction in its sending rate, and the queue is therefore unable to form. Indeed, L4S has been shown to[cut queuing significantly](https://datatracker.ietf.org/doc/rfc9330/) and take latency into the low millisecond range.


Figure 2: L4S queuing structure


### **Dealing with legacy traffic**


The deployment of L4S, however, presents a major risk for a mobile operator, and those implementing the technology need to ensure safeguards are in place to prevent the faster L4S flows from taking over the available bandwidth. This would effectively starve the classic flows used by the majority of users and degrade quality of experience (QoE) for them.


This is solved by the coupled part of L4S’s dual-queue coupled AQM. Here, the AQM implements an in-built fairness mechanism, and if the classic queue begins to become congested, the L4S queue is signaled to begin the marking of L4S packets with CE sooner and more aggressively.


And while it is technically possible to mark traffic as L4S even after congestion signals are issued, the AQM is able to defend against applications that do so. This would result in a buildup of packets in the L queue, and should this happen, strategies have been created for the AQM to identify packets as being non-compliant and demote them to neutralize attempts cheat the system.


### **Validating the L4S Rollout**


Here it should be said that, despite the benefits they bring, the coupled AQM and cheater-detection mechanisms are very much not plug-and-play. Indeed, each of them creates a complex validation challenge that needs to be overcome ahead of their rollout.


First is the need to prove the fairness mechanism works at scale, ensuring backward compatibility with the millions of UEs that do not support L4S-aware congestion control. The risk to the operator of failure is broken legacy services.


Moving beyond this is the need to test interactions between the ECN-based signaling and existing 5G traffic shaping and QoS slice policies.


The third validation challenge relates to security and stability. Any new protocol behavior absolutely must be vetted for robustness against misuse. To test this it is important to actively test against misconfiguration and emulate “cheater” applications to prove the AQM’s demotion mechanism can neutralize them every time.


And then there’s QoE validation. For this, validation is needed with real-world application emulation to show that it achieves the goal and delivers a better QoE across a diverse array of use cases – which is the reason it’s being implemented in the first place.


### **Conclusion: A Smarter, Faster Future**


L4S’s rollout is accelerating and several more major operators have committed to its implementation. Notably both[Vodafone](https://www.vodafone.com/news/technology/vodafone-advances-future-ready-radio-access-network) and[T-Mobile](https://www.t-mobile.com/news/network/unlock-l4s-5g-advanced?msockid=133338f7f015646631652d3df41562c3) have done so, with T-Mobile calling the technology one of 5G Advanced’s “most important capabilities”. But as we’ve outlined, this is not a simple “flip the switch” solution and lab-based validation is essential.


VIAVI is able to offer several test services for L4S, with the[TeraVM](https://www.viavisolutions.com/en-uk/products/teravm) ™ platform able to act as a comprehensive network and application emulator capable of validating implementations ahead of their at-scale rollouts. Through the TeraVM, operators are able to emulate the entire 5G ecosystem at scale, including thousands of L4S-aware UEs (running TCP Prague) mixed with legacy traffic. The system is also capable of testing L4S interactions with 5G QoS slices, emulate “cheater” applications, and provide real-world QoE validation for latency-sensitive applications.


For more information on this topic, please visit our[Security Solutions for Wireless Operators page](https://www.viavisolutions.com/en-uk/products/teravm-security) .


- ** Categories:[Uncategorized](https://blog.viavisolutions.com/category/uncategorized/)
- ** Tags:[5G Advanced](https://blog.viavisolutions.com/tag/5g-advanced/) ,[TeraVM](https://blog.viavisolutions.com/tag/teravm/)


-
-
-
-
-
-

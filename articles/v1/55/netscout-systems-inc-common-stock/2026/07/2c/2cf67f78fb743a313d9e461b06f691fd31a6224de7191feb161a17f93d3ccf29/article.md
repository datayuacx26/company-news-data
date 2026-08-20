---
schema_version: "1.0.0"
document_id: "2cf67f78fb743a313d9e461b06f691fd31a6224de7191feb161a17f93d3ccf29"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/ntp-security"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:6c66ce4c558e94435cfcd301f94ed4991eb683dbc01450223acd6a3f413766dd"
---

# NTP Security

## Executive Summary


There are just a few internet system services that practically every internet host uses on a regular basis. The Network Time Protocol (NTP), perhaps little known to the average user, is just one such system. However, some NTP systems have been abused to facilitate reflection and amplification (RA)[distributed denial-of-service (DDoS)](https://www.netscout.com/what-is-ddos) attacks. And because NTP is so widespread and critical, any vulnerabilities in the system are worrying. NTP-based attacks and scanning for abuseable systems is an ongoing phenomenon. So far in 2025, we have seen more than 45,000 DDoS alerts where NTP was involved as reflectors. Compared with the hundreds of thousands of attacks we see over the same time frame, this is a relatively small proportion. However, we ignore and underestimate this activity at our peril.


This report looks at the current state of NTP scanning in the wild, the potentially abuseable NTP systems still in operation on the public internet, the current landscape of NTP packet-flooding attacks, and patterns defenders should know about to detect and mitigate unwanted NTP activity.


## Key Findings


NTP remains one of the most ubiquitous, widely used subsystems on the internet.
Legitimate NTP traffic has specific patterns, payloads, and packet-size profiles.
Relatively few hosting providers make up the bulk of unsolicited NTP activity.


## Background


NTP is one of the internet’s longest-running protocol systems. It is also one of the most widely deployed and used services. NTP is present and active on the largest internet servers, down to the smallest handheld devices you might find in your pocket. Its job is to help ensure a system has an accurate notion of time by performing periodic synchronization exchanges to one or more time servers. It performs this feat via a series of well-structured messages running on top of the User Datagram Protocol (UDP). NTP is widely used and critical for all sorts of applications, ranging from authentication to logging.


Unfortunately, many UDP-based applications, such as DNS, RIPv1, SNMP, and NTP, have been susceptible to spoofed source reflection attacks. The NTP-specific threat derives from a number of management functions built into the protocol that historically required no authentication. For example, the “monlist” command, if supported, could return a cached list of NTP clients, their source IP address, and their port number to anyone who asks by using a small, single UDP datagram. For popular NTP servers, such a query could result in many full-size UDP datagrams being returned. Monlist or related capabilities have made NTP a favorite DDoS attack vector because there originally were hundreds of thousands of abuseable systems when this capability for DDoS was first recognized more than 10 years ago.


Fortunately, the number of reflection/amplification (R/A) attacks involving NTP has been decreasing as software and operations have tended toward better defaults and best practices.\[1\] Nevertheless, the threat of abuseable NTP servers has not been completely eradicated. Interestingly, unlike with many other protocols, there are very clear and specific traffic characteristics that differentiate unwanted NTP traffic from the rest. We explore these differences here to help operators better understand their own NTP traffic patterns and ultimately help mitigate unwanted NTP-related traffic.


## NTP Activity Analysis


In the following three subsections, we consider internet NTP activity from three distinct vantage points. First, we detail the behavior of NTP scanners in the wild. Using a sample of traffic seen at our sensors, we summarize what third-party scanners are doing and how they are doing it. Second, we look at the responses to internet NTP scanning. Our team conducts low-impact internetwide NTP scans to detect abuseable systems on the internet. We share the results with our customers to enhance their DDoS defense capabilities. Third, we summarize recent NTP-specific attack alerts that we see from our own DDoS activity intelligence. Together, these three perspectives help paint a more complete picture of the NTP traffic threat landscape.


### NTP Scanners


Our investigation starts with NETSCOUT honeypots, configured as low-interaction systems that capture but do not serve content. We recorded UDP messages to UDP destination port 123 and analyzed what we found.


Our honeypots see primarily three broad categories of unsolicited NTP messages called “modes.” The mode is encoded in a 3-bit NTP header field. The most common value we see is mode 7. This corresponds to a private-use or implementation-specific NTP message type. In practice, the reference NTP.org implementation uses this mode for a variety of management functions. It is mode 7 where the infamous “monlist” function is found. Monlist, if supported, instructs an NTP service to return a cached list of other NTP endpoints that have communicated with the system. This list can be quite large, and because a single source address–spoofed packet can solicit the response, this feature has been widely abused in reflection/amplification attacks.


The next most common NTP packet types we see are mode 3, which corresponds to client time-synchronization messages. Mode 3 packets and their responses, with few exceptions for local management traffic, are the only NTP communication you’d normally expect to see on a network. Mode 3 requests and responses are effectively small packets of equal, fixed size. They convey the information necessary for one system to synchronize its clock with a high degree of precision to another by using encoded time stamps and round-trip time estimations. These packets generally pose relatively little risk, even when unsolicited. Usually, we find sources of this traffic originating from legitimate threat intelligence and research NTP surveying systems. However, if you have a system that will respond to unsolicited mode 3 time-synchronization requests, this may be an early warning that someone is cataloging your network or fingerprinting accessible systems. There are many open NTP servers on the internet where this activity is warranted, but you should be sure you want your servers to be among them before allowing them to reply to clock-synchronization messages from anywhere or anyone.


The only other NTP mode we see in any measurable amount is mode 6 messages. NTP mode 6 packets are defined as control messages, having more in common with NTP.org’s mode 7 usage than mode 3 messages. In practice, NTP.org is the only widely deployed implementation that has supported both mode 6 and 7 messages by default. Unlike mode 3 messages, both mode 6 and 7 messages can be abused for R/A attacks. They can also leak information about the system that you may otherwise wish to limit (e.g., operating system version information and software configuration settings).


The sources of scans for each mode vary, but most activity we see originates from legitimate NTP research and surveyor projects. However, there are a number of sources that are more suspicious. Sometimes these originate from little-known hosting providers; other times they originate from systems we wouldn’t expect to see unsolicited traffic from (e.g., a country with which we would rarely expect to exchange traffic). Some may even be spoofed, but we see little evidence of that behavior at our honeypots.


### NTP Responders


Our low-impact vulnerability scanning service looks to identify abuseable NTP systems on the internet. We examined responses to a recent scan on the IPv4 internet. There are multiple techniques to NTP scanning, but here we consider only responses to a special “mode 7” request commonly referred to as “GET_MONLIST.” This is the special management query used by the reference NTP implementation. If available and permitted, the NTP server will respond with a list of client systems it has communicated with, stored in a local cache. A valid response suggests that the NTP responder is susceptible to R/A attacks, although some responses may be from third-party NTP honeypots in the wild.


There are many NTP servers on the internet, but we are particularly interested in those that may contribute to R/A attacks. At one time, the number of abuseable NTP systems was measured in hundreds of thousands. Fortunately, the vast majority of vulnerable systems have been updated or removed from service. We now measure them in the mere thousands. We'd like this number to effectively be zero, but compared with other threat vectors, NTP going from one of the biggest populations of amplifiers to one this small is a net positive. The reduction in vulnerable NTP systems was a result of broad community recognition and cooperation to mitigate the problem.


Our surveying turns up a diverse set of networks, meaning we find no one set of networks is responsible for the majority of vulnerable systems. Some systems may even be other honeypots unknown to us, masking a lower bound of the threat population. We continue to report on those we find vulnerable and include them in our regularly updated threat intelligence feeds. The reason for this will become obvious as we detail it in the next section.


### NTP DDoS Attacks


Despite the significant decline in the number of vulnerable NTP systems used in DDoS attacks, we still find a surprising number of attack alerts where NTP is involved. So far this year, for example, we have seen more than 45,000 attack alerts with an NTP traffic component. About one-fourth (12,000) of those attack alerts were triggered by our NTP attack detection logic. These numbers seem to far exceed what we’d expect given the population of systems we see from scanning. There are reasonable explanations for this seemingly unlikely discrepancy. One possibility is that NTP attacks have been part of the miscreant DDoS toolkit for a long time, and they remain popular with some attack kits, while the number of vulnerable systems continues to be abused rather than fixed. Another is that NTP attacks we see may not always be reflections from amplifiers we look for, but just UDP floods from direct-path sources. In some uncommon but devastatingly harmful cases, we have also seen device manufacturers set NTP servers to hard-coded IP addresses of unwitting third parties. If these devices proliferate or if the third party renumbers, otherwise uninteresting time-synchronization traffic can pose significant problems to those not expecting it.


### NTP Traffic Profiling


One of the little secrets about NTP is how little legitimate traffic fluctuates in normal operations. Nowadays, we rarely see anything but mode 3 packets in legitimate operations, and those packets are practically always the same size in both directions (i.e., 48 bytes of UPD payload). Knowing a certain class of traffic (e.g., UDP port 123) should only ever be a specific length can significantly aid in monitoring and filtering actions. When we know what is normal, what is not, and what we might wish to allow, we can more effectively mitigate traffic that does not fit the expected profile.


Very few protocols and applications behave this way in practice. Beware, however: This is not guaranteed to be true in every environment. There are scenarios where NTP behavior may show variation to these rules, so do not carelessly make simple assumptions or impose traffic rules until you’ve meticulously studied your own environment for outliers. Even if you cannot rely on NTP traffic to behave exactly as we describe 100 percent of the time, in extreme situations when an attack exhibits lots of unusual NTP traffic patterns, the common case can be used in emergency situations temporarily.


## Recommendations


To detect and mitigate abusive, ever-changing networks of varying size and duration, we recommend the following:


- **Real-time visibility** into volumetric traffic floods and distributed attack patterns. Tools such as NETSCOUT[Arbor Sightline](https://www.netscout.com/product/arbor-sightline) can help surface early signs of trouble and trigger flow-specification and remotely triggered black hole (RTBH) defenses to upstream providers.
- **Proactive mitigation** with automated systems such as[Arbor Threat Mitigation System (TMS)](https://www.netscout.com/product/arbor-threat-mitigation-system) or[Arbor Edge Defense (AED)](https://www.netscout.com/product/arbor-edge-defense) . These can stop both volumetric floods and more-complex multivector attacks.
- **Intelligence-driven defense** with feeds such as NETSCOUT’s[ATLAS Intelligence Feed (AIF)](https://www.netscout.com/product/atlas-intelligence-feed) . These provide information about context, what’s trending, who’s being targeted, and how actors are evolving.


Staying ahead of threat actors is an ever-changing job and requires a broad view of where these attacks come from, how they operate, and where they could strike next.


## Conclusion


Due to the very ubiquity of NTP services on the internet, we keep a pulse on what is normal and expected behavior year after year. At one time, DDoS attacks stemming from vulnerable NTP systems were among the most frequent and powerful threat vectors in an adversary’s arsenal. Today, NTP systems are more resilient and less susceptible to abuse than they once were. The threat has not been eliminated, however, and we continue to see abuse stemming from vulnerable NTP systems regularly. The good news is the frequency and impact has waned. Furthermore, mitigation techniques are available, and realizing how normal NTP traffic behaves can help further reduce future threats. NTP is one of the few protocols that is so widespread but predictable in how it behaves. We use these features to our advantage to protect against not only today’s threats, but also tomorrow’s.


## References


- **IETF BCP 223 – Network Time Protocol Best Current Practices:**[https://www.rfc-editor.org/rfc/rfc8633.html](https://www.rfc-editor.org/rfc/rfc8633.html)
